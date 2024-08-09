[Carson带你学Android：手把手教你写一个完整的自定义View-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1963227)
![](https://cdn.nlark.com/yuque/0/2024/png/26356902/1718893332669-c0afb06e-edc5-480e-b8a1-dd5ed8fde8a9.png#averageHue=%23fafaf9&clientId=u361885c3-0f8f-4&from=paste&id=ubee6c17d&originHeight=916&originWidth=1670&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u0cf5cea9-781f-4fd9-ab2b-7355e666a92&title=)
## 具体介绍和使用场景
![](https://cdn.nlark.com/yuque/0/2024/png/26356902/1718893714365-9a33d80c-3a56-4564-9c94-d53072cfa481.png#averageHue=%23f2f2f2&clientId=u361885c3-0f8f-4&from=paste&id=u1f0eb1eb&originHeight=370&originWidth=860&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u6cf2cf29-e26b-4656-934c-ebb1f6a44aa&title=)
## 继承View
![](https://cdn.nlark.com/yuque/0/2024/png/26356902/1718893759501-a4649777-b673-4fc1-9cce-4eea95bc4630.png#averageHue=%23f1f1f1&clientId=u361885c3-0f8f-4&from=paste&id=u1ff5ec60&originHeight=130&originWidth=840&originalType=url&ratio=1.375&rotation=0&showTitle=false&status=done&style=none&taskId=u04f9ef1c-47f9-401e-895f-3c2ffda3059&title=)

- 如何实现一个基本的自定义View（继承VIew）
- 如何自身支持wrap_content & padding属性
- 如何为自定义View提供自定义属性（如颜色等等）
- 实例说明：画一个实心圆
1. 创建自定义View
```java
// 用于绘制自定义View的具体内容
// 具体绘制是在复写的onDraw（）内实现

public class CircleView extends View {

    // 设置画笔变量
    Paint mPaint1;

    // 自定义View有四个构造函数
    // 如果View是在Java代码里面new的，则调用第一个构造函数
    public CircleView(Context context){
        super(context);

        // 在构造函数里初始化画笔的操作
        init();
    }


// 如果View是在.xml里声明的，则调用第二个构造函数
// 自定义属性是从AttributeSet参数传进来的
    public CircleView(Context context,AttributeSet attrs){
        super(context, attrs);
        init();

    }

// 不会自动调用
// 一般是在第二个构造函数里主动调用
// 如View有style属性时
    public CircleView(Context context,AttributeSet attrs,int defStyleAttr ){
        super(context, attrs,defStyleAttr);
        init();
    }


    //API21之后才使用
    // 不会自动调用
    // 一般是在第二个构造函数里主动调用
    // 如View有style属性时
    public  CircleView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }

    // 画笔初始化
    private void init() {

        // 创建画笔
        mPaint1 = new Paint ();
        // 设置画笔颜色为蓝色
        mPaint1.setColor(Color.BLUE);
        // 设置画笔宽度为10px
        mPaint1.setStrokeWidth(5f);
        //设置画笔模式为填充
        mPaint1.setStyle(Paint.Style.FILL);

    }


    // 复写onDraw()进行绘制  
    @Override
    protected void onDraw(Canvas canvas) {

        super.onDraw(canvas);

       // 获取控件的高度和宽度
        int width = getWidth();
        int height = getHeight();

        // 设置圆的半径 = 宽,高最小值的2分之1
        int r = Math.min(width, height)/2;

        // 画出圆（蓝色）
        // 圆心 = 控件的中央,半径 = 宽,高最小值的2分之1
        canvas.drawCircle(width/2,height/2,r,mPaint1);

    }

}
```

2. **在布局文件中添加自定义View类的组件**
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="scut.carson_ho.diy_view.MainActivity">

<!-- 注意添加自定义View组件的标签名：包名 + 自定义View类名-->
    <!--  控件背景设置为黑色-->
    <scut.carson_ho.diy_view.CircleView
        android:layout_width="match_parent"
        android:layout_height="150dp"
        android:background="#000000"

</RelativeLayout>
```
### 支持wrap_content


## 自定义View基础
### 什么是View
### 视图分类

1. 单一视图：一个View，不包含子View，如TextView
2. 视图组，各种布局等等
### View

- 视图的核心类是：View类
- View类是Android中各种组件的基类，如View是ViewGroup基类
- View的构造函数：共有4个，具体如下：
```java
// 构造函数1
// 调用场景：View是在Java代码里面new的
public CarsonView(Context context) {
    super(context);
}

// 构造函数2
// 调用场景：View是在.xml里声明的
// 自定义属性是从AttributeSet参数传进来的
public  CarsonView(Context context, AttributeSet attrs) {
    super(context, attrs);
}

// 构造函数3
// 应用场景：View有style属性时
// 一般是在第二个构造函数里主动调用；不会自动调用
public  CarsonView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
}

// 构造函数4
// 应用场景：View有style属性时、API21之后才使用
// 一般是在第二个构造函数里主动调用；不会自动调用
public  CarsonView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
    super(context, attrs, defStyleAttr, defStyleRes);
}
```
详细参考:
[深入理解Android View的构造函数 - 泡网](https://paonet.com/a/7777777777777704575)
### 视图结构：
![](https://cdn.nlark.com/yuque/0/2024/webp/26356902/1719127533104-3f3a8a72-e3ca-440d-9c0a-e7652abff139.webp#averageHue=%23fcfcfc&clientId=u7c237f03-db76-4&from=paste&id=u1631373c&originHeight=457&originWidth=686&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=u242e538b-54ac-4f40-808d-56fd4b24f0d&title=)
### 坐标系
![](https://cdn.nlark.com/yuque/0/2024/webp/26356902/1719127545405-28ac1c41-2c1b-46b4-b17e-e409701e0404.webp#averageHue=%23fbfbfb&clientId=u7c237f03-db76-4&from=paste&id=ub6c27444&originHeight=457&originWidth=686&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=u4d37f1cc-260b-4695-be83-936b3a61b5b&title=)
### 位置获取方式

1. 视图位置
```java
获取顶部距离(Top)：getTop()
获取左边距离(Left)：getLeft()
获取右边距离(Right)：getRight()
获取底部距离(Bottom)：getBottom()
```

2. 与MotionEvent中`get()`和`getRow()`
```java
//get() ：触摸点相对于其所在组件坐标系的坐标
 event.getX();       
 event.getY();

//getRaw() ：触摸点相对于屏幕默认坐标系的坐标
 event.getRawX();    
 event.getRawY();
```
### 角度，弧度
### 颜色

- ARGB8888：四通道高精度(32位)
- ARGB4444：四通道低精度(16位)
- RGB565：Android屏幕默认模式(16位)
- Alpha8：仅有透明通道(8位)
## View工作流程
### 
### ViewRoot

- 定义
连接器，对应于ViewRootImpl类
- 作用
   1. 连接WindowManager 和 DecorView
   2. 完成View的三大流程： measure、layout、draw

```java
// 在主线程中，Activity对象被创建后：
// 1. 自动将DecorView添加到Window中 & 创建ViewRootImpll对象
root = new ViewRootImpl(view.getContent(),display);

// 3. 将ViewRootImpll对象与DecorView建立关联
root.setView(view,wparams,panelParentView)
```
### DecorView

- **定义：顶层View**
   - 即 Android 视图树的根节点；同时也是 FrameLayout 的子类
- 作用：显示 & 加载布局
   - View层的事件都先经过DecorView，再传递到View
- 特别说明
   - 内含1个竖直方向的LinearLayout，分为2部分：上 = 标题栏（titlebar）、下 = 内容栏


![](https://cdn.nlark.com/yuque/0/2024/webp/26356902/1719129981814-e183302c-aa05-4d17-939d-ee1710c884b3.webp#averageHue=%23f9f9f9&clientId=u7c237f03-db76-4&from=paste&id=u834372d1&originHeight=368&originWidth=280&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=u7f34d413-837c-4ea0-a496-eff46727b57&title=)

> 在Activity中通过 setContentView（）所设置的布局文件其实是被加到内容栏之中的，成为其唯一子View = id为content的FrameLayout中

```java
// 在代码中可通过content得到对应加载的布局

// 1. 得到content
ViewGroup content = (ViewGroup)findViewById(android.R.id.content);
// 2. 得到设置的View
ViewGroup rootView = (ViewGroup) content.getChildAt(0);
```

### 绘制准备
![](https://cdn.nlark.com/yuque/0/2024/webp/26356902/1719130824337-16292b0f-32d7-4c4f-b826-23bdbabf4e17.webp#averageHue=%23f7f7f7&clientId=u7c237f03-db76-4&from=paste&id=Ysuar&originHeight=320&originWidth=981&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=u59bddbc0-bc52-4f0a-9dd4-26f846d5eda&title=)

1. 创建PhoneWindow、DecorView、ViewRootImpl等等
### 绘制流程

1. ViewRootImpl的`performTraversals()`
```java
/**
  * 源码分析：ViewRootImpl.performTraversals()
  */
  private void performTraversals() {

        // 1. 执行measure流程
        // 内部会调用performMeasure()
        measureHierarchy(host, lp, res,desiredWindowWidth, desiredWindowHeight);

        // 2. 执行layout流程
        performLayout(lp, mWidth, mHeight);

        // 3. 执行draw流程
        performDraw();
    }
```

2. 

### Window Activity DecorView和ViewRoot的关系

![](https://cdn.nlark.com/yuque/0/2024/webp/26356902/1719130045352-f7d7e1cf-4eb4-4703-b05b-3dabe24336cb.webp#averageHue=%23f3f3f3&clientId=u7c237f03-db76-4&from=paste&id=uf273fb74&originHeight=410&originWidth=970&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=ub5faedd6-4c49-44e2-9658-37fa6f2770f&title=)

## ViewRoot DecorView和Window简介

### ViewRoot
![](https://cdn.nlark.com/yuque/0/2024/webp/26356902/1719130129078-784ba046-3c20-4ac7-8afd-8fa4579e5fcc.webp#averageHue=%23ececec&clientId=u7c237f03-db76-4&from=paste&id=uc74b9d6a&originHeight=490&originWidth=1148&originalType=url&ratio=1.100000023841858&rotation=0&showTitle=false&status=done&style=none&taskId=u3521960c-4593-401c-889e-83cdce49028&title=)


### 参考
[Android自定义View：ViewRoot、DecorView & Window的简介](https://www.jianshu.com/p/28d396a0f05f)
## DecorView的创建
### DecorView的创建
DecorView是显示的顶层View，那么View的绘制准备从DecorView创建开始说起。
```java
/**
  * 具体使用：Activity的setContentView()
  */
  @Override
    public void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    }

/**
  * 源码分析：Activity的setContentView()
  */
   public void setContentView(int layoutResID) {
        // getWindow() 作用：获得Activity 的成员变量mWindow ->>分析1
        // Window类实例的setContentView（） ->>分析2
        getWindow().setContentView(layoutResID);
        initWindowDecorActionBar();
   }

/**
  * 分析1：成员变量mWindow
  */
  // 1. 创建一个Window对象（即 PhoneWindow实例）
  // Window类 = 抽象类，其唯一实现类 = PhoneWindow
  mWindow = new PhoneWindow(this, window);
  
  // 2. 设置回调，向Activity分发点击或状态改变等事件
  mWindow.setWindowControllerCallback(this);
  mWindow.setCallback(this);

  // 3. 为Window实例对象设置WindowManager对象
        mWindow.setWindowManager(
                (WindowManager)context.getSystemService(Context.WINDOW_SERVICE),
                mToken, mComponent.flattenToString(),
                (info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED) != 0);

    }

/**
  * 分析2：Window类实例的setContentView（）
  */
  public void setContentView(int layoutResID) {

        // 1. 若mContentParent为空，创建一个DecroView
        // mContentParent即为内容栏（content）对应的DecorView = FrameLayout子类
        if (mContentParent == null) {
            installDecor(); // ->>分析3
        } else {
            // 若不为空，则删除其中的View
            mContentParent.removeAllViews();
        }

        // 2. 为mContentParent添加子View
        // 即Activity中设置的布局文件
        mLayoutInflater.inflate(layoutResID, mContentParent);

        final Callback cb = getCallback();
        if (cb != null && !isDestroyed()) {
            cb.onContentChanged();//回调通知，内容改变
        }
    }

/**
  * 分析3：installDecor()
  * 作用：创建一个DecroView
  */
  private void installDecor() {

    if (mDecor == null) {
        // 1. 生成DecorView ->>分析4
        mDecor = generateDecor(); 
        mDecor.setDescendantFocusability(ViewGroup.FOCUS_AFTER_DESCENDANTS);
        mDecor.setIsRootNamespace(true);
        if (!mInvalidatePanelMenuPosted && mInvalidatePanelMenuFeatures != 0) {
            mDecor.postOnAnimation(mInvalidatePanelMenuRunnable);
        }
    }
    // 2. 为DecorView设置布局格式 & 返回mContentParent ->>分析5
    if (mContentParent == null) {
        mContentParent = generateLayout(mDecor); 
        ...
        } 
    }
}

/**
  * 分析4：generateDecor()
  * 作用：生成DecorView
  */
  protected DecorView generateDecor() {
        return new DecorView(getContext(), -1);
    }
    // 回到分析原处

/**
  * 分析5：generateLayout(mDecor)
  * 作用：为DecorView设置布局格式
  */
  protected ViewGroup generateLayout(DecorView decor) {

        // 1. 从主题文件中获取样式信息
        TypedArray a = getWindowStyle();

        // 2. 根据主题样式，加载窗口布局
        int layoutResource;
        int features = getLocalFeatures();

        // 3. 加载layoutResource
        View in = mLayoutInflater.inflate(layoutResource, null);

        // 4. 往DecorView中添加子View
        // 即文章开头介绍DecorView时提到的布局格式，那只是一个例子，根据主题样式不同，加载不同的布局。
        decor.addView(in, new ViewGroup.LayoutParams(MATCH_PARENT, MATCH_PARENT)); 
        mContentRoot = (ViewGroup) in;

        // 5. 这里获取的是mContentParent = 即为内容栏（content）对应的DecorView = FrameLayout子类
        ViewGroup contentParent = (ViewGroup)findViewById(ID_ANDROID_CONTENT); 
      
        return contentParent;
    }
```
#### 源码总结

1. 创建Window抽象类的子类PhoneWindow类的实例对象;
2. 为PhoneWindow类对象设置WindowManager对象;
3. 为PhoneWindow类对象创建1个DecroView类对象(根据所选的主题样式增加);
4. 为DecroView类对象中的content增加Activity中设置的布局文件。

此时，DecorView(即顶层View)已创建和添加Activity中设置的布局文件中，但目前仍未显示出来，即不可见。

### DecorView的显示

