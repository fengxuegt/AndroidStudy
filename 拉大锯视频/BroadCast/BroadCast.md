# 广播

## 广播特性

- 广播接收者的生命周期是非常短暂的，在接收到广播的时候创建，onReceive()方法结束之后销毁
- 广播接收者中不要做一些耗时的工作，否则会弹出Application No Response错误对话框
- 最好也不要在广播接收者中创建子线程做耗时的工作，因为广播接收者被销毁后进程就成为了空进程，很容易被系统杀掉
- 耗时的较长的工作最好放在服务中完成



## 广播类型

- 标准广播：是一种完全异步的广播，在广播发出之后，所有的广播接收器几乎会在同一时间接收到广播，因为他们之间没有任何先后顺序可言；广播效率会比较高，但是无法拦截；
- 有序广播：是一种同步执行的广播，发出广播之后，同一时刻只能有一个接收器接收广播，接收广播之后，后续的接收器才能接收；并且先后顺序通过优先级排序，如果高优先级的接收器接收之后将广播断掉了，那么就无法再接收了；



## 实例——监测电池电量变化

- 首先声明一个广播接收器，并重写它的onReceive方法

  - ```java
            @Override
            public void onReceive(Context context, Intent intent) {
                // 使用intent.getAction来区分检测的频道
                if (Intent.ACTION_BATTERY_CHANGED.equals(intent.getAction())) {
                    // 获取当前电池电量
                    int level = intent.getIntExtra(BatteryManager.EXTRA_LEVEL, -1);
                    // 获取电池最大电量
                    int scale = intent.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
                    // 计算电池电量百分比
                    float batteryPct = level / (float) scale;
    
                    Log.d("BatteryReceiver", "Current battery level: " + level + ", scale: " + scale + ", percentage: " + batteryPct * 100 + "%");
    
                    // 这里可以根据需要处理电池电量信息，比如更新UI显示电量等
    
                }
            }
    ```

    通过Intent能够接收广播内容

- 声明一个过滤器，设置频道

  - ```java
    // 要监听的频道：电量变化
    IntentFilter intentFilter = new IntentFilter();
    // 设置频道
    intentFilter.addAction(Intent.ACTION_BATTERY_CHANGED);
    // 注册广播
    this.registerReceiver(new BatteryReceiver(), intentFilter);
    ```

- 注册广播

以上是一个监测电池电量广播的代码，当电池电量变化时，接收器会接收到广播

- 取消广播注册

  在onDestroy方法中unregister

## 广播注册类型

### 动态注册

如上例子，是根Activity的声明周期联系在一起的

### 静态注册

![image-20240619103257566](imgs\image-20240619103257566.png)

比较耗费资源，因为会一直监听；动态注册的话可以在onDestroy中解除注册；

## 实例——监听应用的安装与卸载

主要是收集信息；收集用户的使用习惯；

## 广播接收权限

### 发送端设置权限

发送广播时有个权限的参数；并且要在发送广播的app中声明权限；

![image-20240619115530201](imgs\image-20240619115530201.png)



![image-20240619115602522](imgs\image-20240619115602522.png)

然后在发送的时候发送，同时在接收app中也要声明有这个权限；

![image-20240619115718525](imgs\image-20240619115718525.png)

### 接收端设置权限

![image-20240619120244293](imgs\image-20240619120244293.png)

首先声明权限，然后在receiver中设置权限；

这种情况下发送方也必须有权限才能发送给接收方；

![image-20240619120440391](imgs\image-20240619120440391.png)







