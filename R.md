---
title:热修复原理
tags: Kotlin
grammar_cjkRuby: true
---

# 一、代码修复
## 1、类加载方案
原理：
- 65535限制
- LinearAlloc限制

由于以上两个限制，产生了Dex分包方案。
Dex分包方案主要代码：
```java
public Class<?> findClass(String name, List<Throwable> suppressed) {
        for (Element element : dexElements) {//1
            Class<?> clazz = element.findClass(name, definingContext, suppressed);//2
            if (clazz != null) {
                return clazz;
            }
        }
        if (dexElementsSuppressedExceptions != null) {
            suppressed.addAll(Arrays.asList(dexElementsSuppressedExceptions));
        }
        return null;
    }
```

Element内部封装了DexFile，DexFile用于加载dex文件，因此每个dex文件对应一个Element。
多个Element组成了有序的Element数组dexElements。当要查找类时，会在注释1处遍历Element数组dexElements（相当于遍历dex文件数组），注释2处调用Element的findClass方法，其方法内部会调用DexFile的loadClassBinaryName方法查找类。如果在Element中（dex文件）找到了该类就返回，如果没有找到就接着在下一个Element中进行查找。

根据上面的查找流程，我们将有bug的类Key.class进行修改，再将Key.class打包成包含dex的补丁包Patch.jar，放在Element数组dexElements的第一个元素，这样会首先找到Patch.dex中的Key.class去替换之前存在bug的Key.class，排在数组后面的dex文件中的存在bug的Key.class根据ClassLoader的双亲委托模式就不会被加载，这就是类加载方案

#### 市面实现方式
QQ空间的超级补丁和Nuwa是按照上面说得将补丁包放在Element数组的第一个元素得到优先加载。
微信Tinker将新旧apk做了diff，得到patch.dex，然后将patch.dex与手机中apk的classes.dex做合并，生成新的classes.dex，然后在运行时通过反射将classes.dex放在Element数组的第一个元素。
饿了么的Amigo则是将补丁包中每个dex 对应的Element取出来，之后组成新的Element数组，在运行时通过反射用新的Element数组替换掉现有的Element 数组。

采用类加载方案的主要是以腾讯系为主，包括微信的Tinker、QQ空间的超级补丁、手机QQ的QFix、饿了么的Amigo和Nuwa等等。

## 2.底层替换方案
与类加载方案不同的是，底层替换方案不会再次加载新类，而是直接在Native层修改原有类，由于是在原有类进行修改限制会比较多，不能够增减原有类的方法和字段，如果我们增加了方法数，那么方法索引数也会增加，这样访问方法时会无法通过索引找到正确的方法，同样的字段也是类似的情况。
底层替换方案和反射的原理有些关联，就拿方法替换来说，方法反射我们可以调用java.lang.Class.getDeclaredMethod

传入的javaMethod（Key的show方法）在ART虚拟机中对应的一个ArtMethod指针，ArtMethod结构体中包含了Java方法的所有信息，包括执行入口、访问权限、所属类和代码执行地址等等，ArtMethod结构如下所示。
``` c++
class ArtMethod FINAL {
...
 protected:
  GcRoot<mirror::Class> declaring_class_;
  std::atomic<std::uint32_t> access_flags_;
  uint32_t dex_code_item_offset_;
  uint32_t dex_method_index_;
  uint16_t method_index_;
  uint16_t hotness_count_;
 struct PtrSizedFields {
    ArtMethod** dex_cache_resolved_methods_;//1
    void* data_;
    void* entry_point_from_quick_compiled_code_;//2
  } ptr_sized_fields_;
}
```

ArtMethod结构中比较重要的字段是注释1处的dex_cache_resolved_methods_和注释2处的entry_point_from_quick_compiled_code_，它们是方法的执行入口，当我们调用某一个方法时（比如Key的show方法），就会取得show方法的执行入口，通过执行入口就可以跳过去执行show方法。
替换ArtMethod结构体中的字段或者替换整个ArtMethod结构体，这就是底层替换方案

AndFix采用的是替换ArtMethod结构体中的字段，这样会有兼容问题，因为厂商可能会修改ArtMethod结构体，导致方法替换失败。Sophix采用的是替换整个ArtMethod结构体，这样不会存在兼容问题。
底层替换方案直接替换了方法，可以立即生效不需要重启。采用底层替换方案主要是阿里系为主，包括AndFix、Dexposed、阿里百川、Sophix

## 3 Instant Run方案
除了资源修复，代码修复同样也可以借鉴Instant Run的原理， 可以说Instant Run的出现推动了热修复框架的发展。
Instant Run在第一次构建apk时，使用ASM在每一个方法中注入了类似如下的代码：
```java
IncrementalChange localIncrementalChange = $change;//1
		if (localIncrementalChange != null) {//2
			localIncrementalChange.access$dispatch(
					"onCreate.(Landroid/os/Bundle;)V", new Object[] { this,
							paramBundle });
			return;
		}
```

其中注释1处是一个成员变量localIncrementalChange ，它的值为$change，$change实现了IncrementalChange这个抽象接口。当我们点击InstantRun时，如果方法没有变化则$change为null，就调用return，不做任何处理。如果方法有变化，就生成替换类，这里我们假设MainActivity的onCreate方法做了修改，就会生成替换类MainActivity$override，这个类实现了IncrementalChange接口，同时也会生成一个AppPatchesLoaderImpl类，这个类的getPatchedClasses方法会返回被修改的类的列表（里面包含了MainActivity），根据列表会将MainActivity的$change设置为MainActivity$override，因此满足了注释2的条件，会执行MainActivity$override的access$dispatch方法，accessdispatch方法中会根据参数&quot;onCreate.(Landroid/os/Bundle;)V&quot;执行‘MainActivity dispatch方法中会根据参数&quot;onCreate.(Landroid/os/Bundle;)V&quot;执行`MainActivitydispatch方法中会根据参数"onCreate.(Landroid/os/Bundle;)V"执行‘MainActivityoverride`的onCreate方法，从而实现了onCreate方法的修改。
借鉴Instant Run的原理的热修复框架有Robust和Aceso。