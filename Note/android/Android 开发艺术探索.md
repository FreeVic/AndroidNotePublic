# Android 开发艺术探索

### Activity A 开启 Activity B,生命周期函数的调用顺序?

```
A onPause
B onCreate
B onStart
B onResume
A onStop
A 的 onPause 执行完后 B才执行 onResume, 所以 onPause 当中不能做耗时操作,否则会影响到新界面的开启
```

###  Activity 异常情况下生命周期顺序?

