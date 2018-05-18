---
title: Android---PackageItemInfo
tags: PackageItemInfo
grammar_cjkRuby: true
---

如何获得清单文件中`<meta-data>`的内容？
通过PackageItemInfo的子类去获取，例如获取application标签中的`<meta-data>`内容：

```java
ApplicationInfo info = null;
        try {
            info = getPackageManager().getApplicationInfo(getPackageName(), PackageManager.GET_META_DATA);
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
        }
        Bundle metaData = info.metaData;
        String ohaha = (String) metaData.getSerializable("ohaha");
        Log.d("MainActivity", "ohaha:" + ohaha);
```

- `ApplicationInfo`：从某个应用的AndroidManifest的`〈application〉`节点收集到的信息
- `ComponentInfo`：包括了ActivityInfo,ProviderInfo,ServiceInfo三种信息
- `ActivityInfo`：从AndroidManifest的 `<activity>`和`<receiver>`节点收集来的信息
- `ProviderInfo`：Holds information about a specific content provider. This is returned byPackageManager.resolveContentProvider().
- `ServiceInfo`：从AndroidManifest的` <service>`节点收集来的信息
- `InstrumentationInfo`:从AndroidManifest的`〈instrumentation〉`节点收集到的信息,这个类用作应用的单元测试
- `PermissionGroupInfo`：从AndroidManifest的 `<permission-group>`节点收集来的信息
- `PermissionInfo`：从AndroidManifest的 `<permission>`节点收集来的信息
- `ResolveInfo`：通过解析intent中IntentFilter里面的信息产生的对象，可能为四大组件中任何一种，比如PackageManager一些解析intent产生ResolveInfo对象
- `ConfigurationInfo`：从AndroidManifest的` <uses-configuration> `and `<uses-feature>`节点收集来的信息
- `FeatureInfo`：从AndroidManifest的  `<uses-feature>`节点收集来的信息