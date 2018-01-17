# JSX语法

JSX 并不是一门新的语言，仅仅是一个语法糖，允许开发者在 JavaScript 中写 HTML 语法

```
var root =(
  <ul className="myList">
    <li>Content of node1</li>
    <li>Content of node2</li>
  </ul>
);
```

- 标签

  - JSX 标签其实就是 HTML 标签，例如`<h1>JSX Syntax</h1>`

  - 还有就是 ReactJS 创建的组建类标签，例如可以使用 `<Hello/>` 的方式引入：

    - ```
      class Hello extends React.Component {
          render() {
              return (
                  <div> hello </div>
              );
          }
      }

      React.render(
        <Hello />
        document.getElementById('container')
      )
      ```

  - ReactJS 中约定自定义的组件标签首字母一定要大写，区别于 HTML 标签

- 执行 JavaScript 表达式

  - JSX 可以用大括号来加入 JavaScript 表达式，例如

    - ```
      var msg = "Hello ReactJS";
      // 在 JSX 中调用这个变量
      <h1>{msg}</h1>
      ```

- 属性

  - JSX 中可以通过标签上的属性来改变当前元素的样式，例如`var msg = <h1 with="10px">Hello ReactJS</h1>;`

  - **延展属性**，传入对象的属性会被复制到组件内，它可以被多次使用，也可以和其他属性一起使用

    - ```
      var props = {};
      props.foo = "1";
      props.bar = "2";
      <h1 {...props} foo = "2"> Hello,ReactJS</h1>
      ```

    - `...` 操作符，这是一个延展操作符，`...props` 的意思就是遍历 props 这个对象，将所有属性都肤质给 h1这个元素，注意元素后面的值会覆盖前面的值，foo 的值就是2

- 样式

  - web 开发中应该将样式写在独立的 CSS 文件中，而 JSX 中对于某个特定组件，样式相对简单而独立，一般可以直接定义在 JSX 中，JSX 中的样式，通过 style 属性来定义

    - ```
      <h1 style={{color: '#ff0000', fontSize: '15px'}}>Hello ReactJS.</h1>
      ```

    - 第一个大括号是 JSX 语法，第二个大括号是 JavaScript 对象，把需要定义的样式都以对象的方式写在大括号里。也可以把一个样式赋值给一个变量，然后传进去

    - ```
      var headStyle = {
        color: '#ff0000',
        fontSize: '14px'
      };
      var node = <h1 style={headStyle}>Hello ReactJS.</h1>;
      ```

    - **注意**：需要将属性名转为驼峰命名格式，若是不转换的话，需要用引号括起来

      - ```
        //驼峰的命名格式
        fontSize: '14px'
        //加引号的方式
        'font-size':'14px'
        ```

      - ​

- **实践**

  - 在字符串中引用变量，不能使用单引号 '  需要使用特殊符号 `(ESC 下方的键)

    - ```
      `transactionType=${this.session.transactionType}`
      ```

  - 组件定义**回调函数**

    - ```
      type
          Props = {
          mOnItemClick: (url: string) => void
      }
      class MyList extends PureComponent {
          props: Props
          constructor(props) {
              super(props);
          }

          render() {
              return (
                  <TouchableNativeFeedback
                      onPress={() => {
                          this.props.mOnItemClick(item.url)
                      }}
                  >
                  </TouchableNativeFeedback>
              )
          }
      }
      ```

    - ​