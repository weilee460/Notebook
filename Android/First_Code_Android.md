# 《第一行代码 Android读书笔记》

## Activity
一个Activity代表一个UI界面，即对应着App的一个交互界面。每一个Activity都需要在AndroidManifest.xml文件中注册。Activity之间使用Intent交互，例如使用Intent启动另一个Activity，同时还可以传递数据。

Intent使用分为两种：显式Intent和隐式Intent。简单的来说，显式Intent在代码中制定该Intent要启动的Activity，隐式Intent不在代码中指定要启动的Activity，但指定了Intent action，此外响应隐式Intent还需要在注册时，使用intent-filter指明要响应的action和category。

### Activity重要的方法
1. onCreate(): Activity第一次创建时调用，主要做一些初始化操作。
2. onStart(): 在Activity由不可见变为可见时调用。
3. onResume(): 在Activity准备和用户交互式调用，此时Activity处于栈顶，处于运行状态。
4. onPause(): 在系统准备启动或恢复另一个Activity时调用。此时，通常将消耗CPU的资源释放掉，并保存关键数据，执行速度必须要快。
5. onStop(): 在Activity完全不可见时调用。
6. onDestroy(): 在Activity被销毁前调用，调用后将变为销毁状态。此处对于需要保存状态的应用，保存好必须的数据。
7. onRestart(): Activity重新启动时调用。可以在此处做状态的恢复。

### Activity生存期
1. 完整生存期：即Activity在onCreate()到onDestroy()运行过程。
2. 可见生存期：即Activity在onStart()和onStop()的运行过程。此过程中，应用对于用户是可见的，但不一定是可交互的。
3. 前台生存期：即Activity在onResume()和onPause()之间的运行过程。此时应用对于用户是可见的，且是可交互的。

### Activity启动模式
在AndroidManifest.xml文件中，给<activity>标签指定android:launchMode属性，设置Activity启动模式。

1. standard:

2. singleTop:

3. SingleTask:

4. SingleInstance: 


