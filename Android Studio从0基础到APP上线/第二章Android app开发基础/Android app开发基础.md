## 2.1  App的开发特点

### 2.1.2  APP的开发语言

- Java
- Kotlin

- C/C++
- XML

wrap_content 表示刚刚能够包内容包裹住

### 2.1.3  APP能够连接的数据库

SQLite：内嵌数据库，APP可以直接读写，不需要进行连接等操作



## 2.2  APP的工程结构

### 2.2.1 APP的工程目录结构

分成两个层次，一个是Project，一个是module；

module依赖于Project，一般说的编译运行app，指的是module；因为module对应的才是实际的app

在**Android**视图下，项目下有两个分类，一个是app，一个是Gradle Scripts；

![image-20240606143824885](imgs\image-20240606143824885.png)

app目录下的三个子目录：

- manifests目录，下面只有一个AndroidManifest.xml，它是APP的运行配置文件
- Java子目录中有三个目录，一个用于存放源代码，另外两个都是存放测试用的Java代码
- res目录，存放当前模块的资源
  - drawable
  - layout
  - mipmap
  - values

Gradle Scripts下面主要是工程的编译配置文件，主要有：

- build.gradle，项目级和module级别，用于app工程的编译规则

- proguard-rules.pro，该文件用于描述Java代码的混淆规则

- gradle.properties，用于配置编译工程的命令行参数，一般无需改动

- settings.gradle，该文件配置了需要编译那些模块，初始内容为include：':app'，代表只编译app模块

  - ![image-20240606145009081](imgs\image-20240606145009081.png)

  - 也有项目的仓库信息，可以把谷歌改成阿里云

    ![image-20240606145503134](imgs\image-20240606145503134.png)

- local.properties，项目的本地配置文件，在工程编译时生成，用于描述开发者计算机的配置，包括SDK的本地路径，NDK的本地路径等

  - ![image-20240606145043210](imgs\image-20240606145043210.png)

​	

## 2.2.2 编译配置文件build.gradle

项目中有两个相关文件，一个是工程的，一个是module的；

项目的仓库信息从Project级别的build.gradle移动到了settings.gradle中

Gradle的版本配置文件在gradle\wrapper\gradle-wrapper.properties中，也可以依次选择File-Project Structure-Project在弹出的页面配置Gradle version

因为Android Studio必须对应正确版本的Gradle，每个版本都不同；

![image-20240606145910105](imgs\image-20240606145910105.png)



### 2.2.3 运行配置文件AndroidManifest.xml

指定了app的运行配置信息，是一个xml文件；

根节点是Manifest，package属性指定了APP的包名，Manifest下面有个application节点，有几种属性：

- ![image-20240606150827021](imgs\image-20240606150827021.png)

Activity节点，只有在此文件中正确配置了Activity节点，才能在运行时访问对应的活动页面。初始的Mainactivity是app的默认主页，之所以是主页，是因为配置了intent-filter标签；

![image-20240606151044820](imgs\image-20240606151044820.png)

category 配置launcher决定了是否在手机屏幕显示app图标，如果有两个Activity配置了这个属性，那么桌面就会显示两个图标；

**注意：**

从Android12开始，任意组件节点一旦配置了intent-filter，那么就必须声明exported属性，以便设定是否支持其他应用调用当前组件

## 2.3 App的设计规范

源码设计规范：界面设计和代码逻辑分开；分别用xml和Java编写；



### 2.3.1 界面设计和代码逻辑

- xml修改了文字，可以直接预览，如果在代码中配置的话，是需要运行才能看到效果的
- 一个界面布局可以被复用多次
- 一份代码逻辑可以对应多个页面



### 2.3.2 利用xml标记描绘界面

三种节点：

- 根节点
- 布局节点
- 控件节点

根节点是特殊的布局节点；

根节点必须指定命名空间，并且每个界面只能有一个根节点；设置了命名空间之后，Android Studio会自动检测是否符合语法；



### 2.3.3 使用Java代码编写逻辑

![image-20240606153626854](imgs\image-20240606153626854.png)

```java
        TextView textView = (TextView) findViewById(R.id.tv_hello);
        textView.setText("你好我的哥");
```



## 2.4 APP的活动页面

### 2.4.1 创建新的app页面

- 创建xml文件
- 创建Java代码
- 注册页面配置

#### 创建xml文件

![image-20240606154031949](imgs\image-20240606154031949.png)

![image-20240606154058598](imgs\image-20240606154058598.png)



#### 创建Java代码

```Java
public class Main2Activity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);
    }
}
```

#### 注册页面配置

在AndroidManifest配置文件中添加一个activity节点；

```xml
<activity android:name=".Main2Activity"></activity>
```

### 2.4.2 快速生成页面源码

![image-20240606154618958](imgs\image-20240606154618958.png)

### 2.4.3 跳转到另一个页面

从源页面跳转到目标页面；

具体写法为：

```java
startActivity(new Intent(MainActivity.this,Main2Activity.class));
```

因为跳转页面通常发生在当前页面，所以可以使用this指代当前页面；

demo：app启动后再首页停留三秒中，然后跳转到新页面；延迟处理功能使用Handler工具的postDelayed方法；

```java
    @Override
    protected void onResume() {
        super.onResume();
        goNextPage();
    }

    private void goNextPage() {
        TextView textView = (TextView) findViewById(R.id.tv_hello);
        textView.setText("3秒后进入下个页面");
        new Handler().postDelayed(mGoNext, 3000);
    }

    private Runnable mGoNext = new Runnable() {
        @Override
        public void run() {
            startActivity(new Intent(MainActivity.this, Main2Activity.class));
        }
    };
```

为什么是onResume方法？

Intent是什么？

Handler工具是怎么使用的？

这些都是课后问题，后续会讲；
