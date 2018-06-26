# React Native Android 设置自定义OkHttpClient

### 背景

项目需要在React Native（之后简称RN）的网络请求中写入cookie数据给服务端校验，但是RN在0.42版本后不再支持用户自定义设置OkHttpClient，所以就需要换一种方式给RN的网络请求写入cookie。

我当前环境：

- RN 0.51.0
- Android 16及以上

RN当中fetch请求是在`NetWorkingModule` 这个类中的OkHttpClient发起的，所以我们需要替换成我们自己的client,大致有以下三种方案：

- **下载RN源码修改然后编译打包**(不推荐直接修改源码，如果RN版本更新那么维护起来就比较痛苦了)

  - Android端的源码其实就是node modules/react-native下`ReactAndroid`和`ReactCommon`两个项目，关于怎么导入到安卓工程，有很多文章：[RN源码编译1](https://www.jianshu.com/p/bd4bcdceba9b) [RN源码编译2](https://www.jianshu.com/p/fbd29a9799ee)

  - 找到 `NetWorkingModule` 这个类的构造方法

    - ```
      /**
         * @param context the ReactContext of the application
         */
        public NetworkingModule(final ReactApplicationContext context) {
          原来 this(context, null, OkHttpClientProvider.createClient(), null);
          改为 this(context, null, OkHttpClientProvider.getOkHttpClient(), null);
        }
      ```

      ​

    - 然后设置自定义的client `OkHttpClientProvider.replaceOkHttpClient(client)`

- **反射设置自定义的client**

  - 虽然`NetWorkingModule` 中的client是final的，但是并没有在声明的时候就进行初始化，所以通过反射是可以进行设值的，由于时间原因，并没有去实践这种做法。

- **实现自定义的 MainReactPackage**

  - 由于 `NetWorkingModule` 是在 `MainReactPackage` 中初始化的，而 `MainReactPackage` 又是我们自己设置给 `ReactInstanceManager` ，所以就可以尝试继承 `MainReactPackage` ，重写生成 `NetWorkingModule` 的逻辑。需要注意的是 `NetWorkingModule` 的访问是包范围的，所以初始化代码也要放在对应的包下面，即`com.facebook.react.modules.network`

  - ```
    public class MyMainReactPackage extends MainReactPackage {
        @Override
        public List<ModuleSpec> getNativeModules(ReactApplicationContext context) {
            List<ModuleSpec> nativeModules = super.getNativeModules(context);
            return adjustModules(context, nativeModules);
        }

        private List<ModuleSpec> adjustModules(ReactApplicationContext context, List<ModuleSpec> moduleSpecs) {
            ArrayList<ModuleSpec> modules = new ArrayList<>(moduleSpecs);

            for (int i = 0; i < modules.size(); i++) {
                ModuleSpec spec = modules.get(i);
                if (spec.getType().equals(NetworkingModule.class)) {
                    modules.set(i, getCustomNetworkingModule(context));
                    break;
                }
            }

            return modules;
        }

        private ModuleSpec getCustomNetworkingModule(final ReactApplicationContext context) {
            return ModuleSpec.nativeModuleSpec(
                    NetworkingModule.class,
                    new Provider<NativeModule>() {
                        @Override
                        public NativeModule get() {
                            return NetworkingModuleUtils.createNetworkingModuleWithCustomClient(context);
                        }
                    });
        }
    }
    ```

    ​

  - ```
    public class NetworkingModuleUtils {
        public static NetworkingModule createNetworkingModuleWithCustomClient(ReactApplicationContext context) {
            OkHttpClient client = OkHttpClientProvider.createClient();
            // ... full access to customize client
            return new NetworkingModule(context, null, OkHttpClientManager.getInstance().mOkHttpClient);
        }
    }
    ```

  ​

方法1和方法3亲测可用，方法2只是一个思路，估计还会有坑，比如怎么获取 `NetWorkingModule` 的引用