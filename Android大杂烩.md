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