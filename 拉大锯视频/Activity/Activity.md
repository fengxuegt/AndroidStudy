# Activity

## 代码写法：

可以将控件单独声明；

控件声明之后新建单独方法将其与xml中的id绑定；

新建单独方法绑定监听器；

```java
private Button button;

private void initListener() {
	button.setOnClickListener(new View.OnClickListener() {
		public void onClick() {
			log.debug("onClick occuered.")
		}
	});
}

private void initView() {
	button = (Button) this.findViewById(R.id.bt);
}
```

这样的代码结构比较清晰；



## 显示意图和隐式意图

一个是直接指定Activity 的全路径类名；

一个是不直接指定，使用xml配置Action和Category，然后在代码中指定来进行跳转；使用setAction和addCategory方法来指定；

使用两种不同意图的原因是？

- 显示意图，一般是用来做应用内组件（Activity、Service等）的跳转，因为Activity的全路径类名是非常明确的，都是自己写的代码；
- 隐式意图一般用于应用之间的跳转，例如打开第三方应用；



### 显示意图跳转第三方应用

![image-20240618105858349](imgs\image-20240618105858349.png)

这个例子是跳转到浏览器，浏览器的包名是通过adb logcat抓出来的；不太稳定，一般跳转第三方应用还是推荐使用隐式意图；

```shell
adb logcat | grep cmp
```

显示意图的写法对于应用内组件的话也是适用的；

![image-20240618110637216](imgs\image-20240618110637216.png)

### 隐式意图跳转第三方应用

同样需要查看APP源码，查看浏览器源码中的AndroidManifest.xml文件；

![image-20240618111640263](imgs\image-20240618111640263.png)

## 组件之间传递数据

### 组件之间传递基本类型数据

![image-20240618112202437](imgs\image-20240618112202437.png)

### 组件之间传递对象

需要实现序列化方法



### 模拟拨打电话例子

#### 需要增加拨打电话的权限

![image-20240618113340772](imgs\image-20240618113340772.png)

#### 在意图中使用setData方法，设置Uri

也需要查看Phone app源码

![image-20240618113507125](imgs\image-20240618113507125.png)



### 模拟发送短信例子

新建Activity来代表要跳转的页面；主要是用来体会schema的作用；

![image-20240618143623773](imgs\image-20240618143623773.png)

![image-20240618143707058](imgs\image-20240618143707058.png)

![image-20240618143752339](imgs\image-20240618143752339.png)

## Activity数据回传



第一，要注意过程，不是用startActivity，而是用startActivityForResult

第二，要复写onActivityResult方法，这里会有返回值

第三，第一个Activity不可finished，否则就接收不到结果了。



#### RequestCode

代表是发送的哪个请求

#### ResultCode

表示返回值的状态

### 拍照返回图片

![image-20240618190054789](imgs\image-20240618190054789.png)



## Activity的生命周期

### onCreate  vs   onDestroy

然后在onCreate方法中恢复数据，在onDestroy方法中保存数据；例如编辑短信的时候不小心退出了，再次回来的时候要能够把短信内容重新恢复；



### onStart  vs  onStop



























