# 使用Espresso进行UI测试

[TOC]



## 配置

![](http://7xqdxj.com1.z0.glb.clouddn.com/2016%E5%B9%B46%E6%9C%8830%E6%97%A5_0.jpg)

如果,Studio已经生成了测试代码对应的目录

- androidTest -->Instrument Test 工具测试
- test --> Local Test 本地测试

### 添加依赖
- app/build.gradle dependencies

  ```
  androidTestCompile 'com.android.support.test.espresso:espresso-core:2.2.2'
  androidTestCompile 'com.android.support.test:runner:0.5'
  ```

- app/build.gradle android.defaultconfig

  ```
  testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
  ```


### 安装support library

- 打开Sdk Manager 下载Extras/Android Support Respository

  ![](http://7xqdxj.com1.z0.glb.clouddn.com/2016%E5%B9%B46%E6%9C%8830%E6%97%A5_1.jpg)

## 常见错误

- 打包apk重复文件

  ```
  Error:Execution failed for task ':app:transformResourcesWithMergeJavaResForDebugAndroidTest'.
  > com.android.build.api.transform.TransformException: com.android.builder.packaging.DuplicateFileException: Duplicate files copied in APK META-INF/maven/com.google.guava/guava/pom.properties
  	File1: C:\Users\Vic\.gradle\caches\modules-2\files-2.1\com.google.guava\guava\16.0.1\5fa98cd1a63c99a44dd8d3b77e4762b066a5d0c5\guava-16.0.1.jar
  	File2: C:\worksapce\other\Demo\app\build\intermediates\exploded-aar\com.android.support.test.espresso\espresso-core\2.2.1\jars\classes.jar
  解决:
  用exclude去掉依赖中相关的module
  ```

- 依赖版本冲突

  ```
  Error:Conflict with dependency 'com.android.support:support-annotations'. Resolved versions for app (23.3.0) and test app (23.0.1) differ. See http://g.co/androidstudio/app-test-app-conflict for details.
  使用gradlew -q dependencies app:dependencies --configuration compile检查依赖树,有两个依赖使用到的annotations版本不一致引起
  解决:强制使用指定的版本
  在app/build.gradle添加
  configurations.all {
      resolutionStrategy {
          force 'com.android.support:support-annotations:23.3.0'
      }
  }
  ```

## Show me the code

### 创建测试类

- 直接在AndroidTest目录下新建测试类

- 或者选中类名,右键go to-->test新建测试类

  ```
  @RunWith(AndroidJUnit4.class)
  @LargeTest
  public class RestartActivityInstrumentationTest {

      private static final String STRING_TO_BE_TYPED = "Peter";

      @Rule
      public ActivityTestRule<RestartActivity> mActivityRule = new ActivityTestRule<>(
              RestartActivity.class);

      @Test
      public void sayHello() throws InterruptedException {
          Thread.sleep(3000);
          onView(withId(R.id.et)).perform(replaceText(STRING_TO_BE_TYPED));
          closeSoftKeyboard();
          onView(withText("Say hello!")).perform(click());
          String expectedText = "Hello, " + STRING_TO_BE_TYPED + "!";
          onView(withId(R.id.tv)).check(matches(withText(expectedText)));
      }

  }
  ```

### 运行用例

- 直接右键运行测试类

- 或者添加新的运行选项

  ![](http://7xqdxj.com1.z0.glb.clouddn.com/2016%E5%B9%B46%E6%9C%8830%E6%97%A5_2.jpg)

### [代码](https://git.coding.net/NewDemo/Demo.git)

## 延伸

- [mock android Dependency进行单元测试](https://developer.android.com/training/testing/unit-testing/local-unit-tests.html#setup)
- [Dagger2注入配合Espresso测试](http://blog.csdn.net/caroline_wendy/article/details/50530836)
- [Espresso api](https://segmentfault.com/a/1190000004338384)

## 参考

[Android Studio Test your App](https://developer.android.com/studio/test/index.html#run_a_test)

[Espresso Document](https://google.github.io/android-testing-support-library/docs/espresso/index.html)

