# React Native 跨平台移动应用开发

- React Developer Tools chrome 插件

- 初始化RN旧版本项目

  - 安装rninit工具 `npm install -g rninit`
  - 初始化指定版本的RN项目 `rninit init projectName --source react-native@0.x.x`

- 列出某个包所有可用版本 `npm view react-native versions -json`

- 数据线连接安卓设备 `adb reverse tcp:8081 tcp:8081`

- 使用`console.log`对RN应用的性能有影响，在应用发包时最好删除或注释`console.log`

- **调试**

  - Ctrl+P ，直接输入需要调试的文件名
  - **快捷键**
    - F8，cmd+/ Resume script execution 继续执行代码
    - F10，cmd+‘ Step over next function call 单步执行，不进入子函数单步
    - F11，cmd+; Step into next function call 单步执行，遇子函数进入并进行单步
    - cmd+F8 Deactivate breakpoint 使当前断点失效
    - Pause on Exception，当开关打开时，蓝色，如果RN代码遇到异常，就会暂停代码的运行并进入单步调试，对调试非常有用
  - 使用ADM调试
    - 命令行输入monitor(如果环境变量配置正确)，打开DDMS，在logcat窗口中查看日志信息

- 注释

  - RN代码可以 使用`//`
  - JSX代码必须另起一行 使用 `{/*  */}`

- 状态机思维

  - 努力让自定义的RN组件成为无状态的组件

  - 只是用setState函数来改变状态机变量

  - 尽一切可能让UI中可变的数据来源是状态机变量与属性

  - setState函数回调

    - 如果还有操作要在界面重新渲染完成后执行，就不能放在this.setState语句后，而是应该放在回调函数中

    - ```javascript
      updateNum(nextTxt){
        this.setState((oldState)=>{
          for(var aName in oldState){
            console.log(aName);
            console.log(oldState[aName])
          }
          return {
            inputNum:nextTxt,
            newField:'I am a new variable'
          };
        },this.changeNumDone);
      }
      changeNumDone(){
        console.log('changeNumDone')
      }
      ```

  - 判断是否渲染 `boolean shouldComponetUpdate(nextProps,nextState)`

  - 强制渲染 `forceUpdate(callback)`

    - 如果因为某种原因可变数据必须从状态机变量与属性外获取，可以调用该函数，则所有级别的UI组件重新 获取 计算 渲染，开发中应尽量避免使用

  - 语法简化

    - ```javascript
      // 将形式参数写成与状态机变量同名，可省略key的写法
      updatePW(inputedNum){
        this.setState(()=>{
          return {inputedNum}
        });
      }

      updatePW(inputedNum){
        this.setState({inputedNum})
      }

      onChangeText={(inputedNum)=>this.setState({inputedNum})}
      ```

- 成员变量

  - RN延续了js的开发风格,可以在任何需要它的地方定义(任何成员函数的任意地方),坏处是不知道是否赋了初值,比较好的做法是在构造函数中定义

  - ```javascript
    constructor(props){
      this.myProperty1 = '';
      this.myProperty2 = '';
    }
    在组件中 this.myProperty1
    ```

- 静态变量

  - ```javascript
    ...
    export default class LearnRN extends Component{
      static staticField11 = 'field1'
      static staticMethod(){
        console.log('test');
      }
    }
    ...
    // 调用 不能使用 this. 需要类名.
    LearnRN.staticField1
    ```

- 组件回调函数绑定

  - 绑定操作是为了让回调函数能够正确的解析出 this, 如果回调函数使用箭头函数的方式回调,则不需要绑定

  - 回调函数的四种写法

    - 使用箭头函数指向回调 `onChangeText={(newText)=>this.updateNum(newText)}`

    - 回调函数使用箭头函数定义

      - ```javascript
        updateNum = (newText)=>{
          this.setState((state)=>{
            reuturn {
              inputedNum:newText,
            }
          });
        }
        ...
        onChangeText={this.updateNum}
        ```

    - 在构造函数中进行绑定

      - ```javascript
        this.updatePW = this.updatePW.bind(this)
        ...
        onChangeText={this.updateNum}
        ```

    - 构造函数中不绑定,在使用时进行绑定

      - ```javascript
        onChangeText={this.updateNum.bind(this)}
        // 如果在 render 函数中这么做,那么每次渲染都会进行绑定,耗性能
        ```

- 平台判断 `PlatFrom.OS === 'android'`

  - 必须要是小写的

- 开发版本判断 `if (__DEV__)`

- 回退键判断

  - ```javascript
        componentDidMount() {
            if (Platform.OS === 'android') {
                console.log("平台是 Android");
                BackHandler.addEventListener('backPressed',this._onBackPressed)
            } 
        }

        componentWillUnmount(){
            if(Platform.OS === 'android'){
                BackHandler.removeEventListener('backPressed',this._onBackPressed)
            }
        }
    ```

- **属性确认** (仅在开发环境中有效,正式发布环境是不会进行检查的)

  - ```javascript
    MyText.propTypes = {
        name:PropTypes.string,
    }
    ```

  - 要求 JavaScript 基本类型 `React.PropTypes.array`

    - array bool func number object string

  - 要求可渲染的节点(指数字 数字数组 字符串 字符串数组) `React.PropTypes.node`

  - 要求某个 React 元素 `React.PropTypes.element`

  - 要求某个类的实例 `React.PropTypes.instanceOf(nameOfClass)`

  - 要求取值为特定的几个值 `React.PropTypes.oneOf(['value1','value2'])`

  - 要求指定类型中的一个 

    - ```javascript
      React.PropTypes.oneOfType([
       React.PropTypes.string,
        React.PropTypes.number,
        React.PropTypes.instanceOf(nameOfClass)
      ])
      ```

  - 要求为指定类型的数组 `React.PropTypes.arrayOf(React.PropTypes.number)`

  - 要求是一个有特定成员变量的对象 `React.PropTypes.objectOf(React.PropTypes.number)`

  - 要求是一个指定构成方式的对象

    - ```
      React.PropTypes.shape({
        color:React.PropTypes.string,
        fontSize:React.PropTypes.number
      })
      ```

  - 可以是任意类型 `React.PropTypes.any`

  - 以上均可以加上**isRequired**要求必须 `React.PropTypes.string.isRequired`

- 默认属性值,一般放在类声明结束后

  - ```javascript
    MyText.defaultProps = {
        name:'jack',
        age:10,
    }
    ```

- Alert

  - ```javascript
    Alert.alert(
                '弹出框标题',
                '弹出框内容',
                [
                    {text:'按钮1',onPress:(()=>{ToastAndroid.show('按钮1',ToastAndroid.SHORT);})},
                    {text:'按钮2',onPress:this._onPressBtn2},
                    {text:'按钮3',onPress:this._onPressBtn3}
                ]
            );
    Android 平台下最多只能有三个选项, ios 平台下无限制
    ```

- 混合开发基础

  - 回调

    - Android 代码定义类 ExampleModule 实现ReactContextBaseJavaModule

    - 函数必须添加注解@ ReactMethod,才能被 RN 侧调用,并且这些函数不能有返回值(因为被调用的原生代码都是异步执行的,所以结果只能通过回调或发送消息通知给 RN)

    - 定义类 ExamplePackage 继承 ReactPackage, 并在 createNativeModules() 中添加ExampleModule

    - 在 ReactNativeHost getPackages()中添加 ExamplePackage

    - ```javascript
      js 代码中调用
      nativeModule.doAsync({key:'这是个耗时操作'}).then(
                  (msg)=>{
                      ...
                  }
              );
      对应原生方法
          @ReactMethod
          fun doAsync(map:ReadableMap?,promise: Promise?){
              ...
          }

      ```

  - 事件

    - ```javascript
      原生代码
      在ExampleModule中拿 ReactApplicationContext的引用发送消息reactApplicationContext.getJSModule<DeviceEventManagerModule.RCTDeviceEventEmitter>(DeviceEventManagerModule.RCTDeviceEventEmitter::class.java).emit(eventName, paramObject)
      js代码添加消息监听
      DeviceEventEmitter.addListener(eventName,this._onNativeEvent.bind(this))
      ```

  - 常量 跨语言常量

    - ```java
      重写 ExampleModule 中 getConstants() 方法提供常量
          override fun getConstants(): Map<String, Any>? {
              val constants = HashMap<String, Any>()
              constants[DURATION_SHORT_KEY] = Toast.LENGTH_SHORT
              constants[DURATION_LONG_KEY] = Toast.LENGTH_LONG
              constants[EVENT_TIME] = EVENT_TIME
              return constants
          }
      ```

  - 生命周期

  - 注意

    - 混合开发中原生代码不应该假设自己会在什么线程中执行,而是要自己维护线程做操作

## 问题

- js中的this，bind函数

- PropTypes 找不到类

  - ```javascript
    React native Undefined is not an object(evaluating'_react3.default.PropType.shape')
    react 16.0以后放在了另外的包中
    npm install --save prop-types
    ```

  - ​