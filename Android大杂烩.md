---
title: Android大杂烩 
tags: android零碎知识
grammar_cjkRuby: true
---

## @SuppressLint("NewApi")
屏蔽一切新api中才能使用的方法报的android lint错误

## 底部弹出Dialog

```java
public class FamilyDialog extends Dialog implements View.OnClickListener {
    private Context context;
    private TextView popCancle;
    private TextView popOk;
    private RadioGroup popGroup;
    private View pView;

    public FamilyDialog(Context context, View pView){
        this(context,pView,0);

    }

    public FamilyDialog(Context context,View pView,int style){
        super(context, style);
        this.context = context;
        this.pView = pView;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        View view = LayoutInflater.from(context).inflate(R.layout.family_dialog, null,false);

        popCancle = (TextView)view.findViewById(R.id.tv_pop_cancle);
        popOk = (TextView)view.findViewById(R.id.tv_pop_ok);
        popGroup = (RadioGroup)view.findViewById(R.id.rg_pop_group);
        popCancle.setOnClickListener(this);
        popOk.setOnClickListener(this);

        setContentView(view);

        getWindow().setGravity(Gravity.BOTTOM);

        WindowManager.LayoutParams lp = getWindow().getAttributes();
        lp.x = 0;
        lp.y = 0;
        lp.width = ViewGroup.LayoutParams.MATCH_PARENT;
        lp.height = ViewGroup.LayoutParams.WRAP_CONTENT;
        onWindowAttributesChanged(lp);


    }

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case R.id.tv_pop_cancle:
                dismiss();
                break;
            case R.id.tv_pop_ok:
                for (int i = 0; i < popGroup.getChildCount(); i++) {
                    RadioButton radio = (RadioButton)popGroup.getChildAt(i);
                    if (radio.isChecked()){
                        ((TextView)pView.findViewById(R.id.tv_bottom_menu)).setText(radio.getText().toString());
                    }
                }
                dismiss();
                break;
        }
    }
}
```
> `pView`是你想与之交互的界面，可以把Dialog选择结果返回给界面。

自定义的样式，可以去除边框等东东

```xml
<style name="MyDialogTheme" parent="@android:style/Theme.Holo.Light">
        <item name="android:windowFrame">@null</item><!-- 边框 -->
        <item name="android:windowNoTitle">true</item>
        <item name="android:backgroundDimEnabled">true</item><!-- 外部变灰 -->
        <item name="android:windowContentOverlay">@null</item><!-- 内部阴影 -->
    </style>
```

## 设置键盘回车键变为搜索
EditText的布局文件中设置

```xml
android:imeOptions="actionSearch"
android:inputType="text"
```

监听事件

```java
etSearch.setOnEditorActionListener(this);
//......
public boolean onEditorAction(TextView v, int actionId, KeyEvent event) {
        if (actionId == EditorInfo.IME_ACTION_SEND || (event!=null&&event.getKeyCode()== KeyEvent.KEYCODE_ENTER)){
            keyBodrdManager.hideSoftInputFromWindow(etSearch.getWindowToken(),0);//隐藏软键盘

            UIUtil.showToast(v.getText().toString());

            return true;
        }
        return false;
}
```

## 获取局域网内的设备信息以及注册设备
 > 使用的框架，jmDns.jar。
 
 代码示例：
 开始
 
```java
String type = "_http._tcp.local.";
WifiManager wifi = (WifiManager) getSystemService(android.content.Context.WIFI_SERVICE);  
WifiManager.MulticastLock lock =wifi.createMulticastLock(getClass().getSimpleName());  
lock.setReferenceCounted(false);  
lock.acquire();
   
JmDNS mJmdns = JmDNS.create();  
   mJmdns.addServiceListener(type,listener = new ServiceListener() {  
        public voidserviceResolved(ServiceEvent ev) {  
            Log.d("test","Service resolved:"  
                     +ev.getInfo().getQualifiedName()  
                     + " port:" +ev.getInfo().getPort());  
        }  
        public void serviceRemoved(ServiceEv entev) {  
            Log.d("test","Service removed:" + ev.getName());  
        }  
        public void serviceAdded(ServiceEvent event) {  
            mJmdns.requestServiceInfo(event.getType(),event.getName(), 1);  
       }  
    });  
```

关闭

```java
mJmdns.removeServiceListener(type, listener);  
mJmdns.close();  
lock.release();  
```

## 父类调用子类方法
简单示例
> 父类的具体实现代码

```java
abstract public class Fother {
    public Fother() {
        System.out.println("the constructor of Fother");
    }

    abstract String getTime();

    private static Son mSon = new Son("fother invocation son");

    public static void invocationSon(){
        mSon.getTime();
        ((Fother)mSon).print();
    }

    public void print(){
        System.out.println("fother print");
    }

}
```
> 子类的具体实现代码

```java
public class Son extends Fother{
    public Son(String str) {
        System.out.println(str);
    }

    @Override
    String getTime() {
        return new Date().toString();
    }
	
}
```

需要注意，在父类中使用子类，需要设为静态，否则会出现stackoverflow错误。

但是为什么要这样写呢？
**猜测：**
这样做有一个功能，父类的静态方法想调用自己的非静态方法时，不必将非静态方法设置为静态的之后再调用，而可以通过子类去调用这些非静态方法。

## 抽象类的其他用法
抽象类
```java
abstract public class AbstractDemo {
    public static AbstractDemo abstractDemo;

    public abstract void print(String str);
    public abstract void setAge(int age);
}
```

初始化

```java
public class Client {
    static {
        AbstractDemo.abstractDemo = new AbstractDemo() {
            @Override
            public void print(String str) {
                System.out.println(str);
            }

            @Override
            public void setAge(int age) {
                System.out.println(age);
            }
        };
    }
}
```

调用
```java
Client client = new Client();
AbstractDemo.abstractDemo.print("哦哈哈");
```

为何这么写？暂不清楚