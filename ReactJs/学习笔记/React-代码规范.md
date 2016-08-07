# [Coding Style(编码风格)](https://github.com/JasonBoy/javascript/tree/master/react)

## Basic Rules(基本原则)

- 每个文件中只包含一个React组件。
- 尽可能地使用JSX语法。
- 除非不用JSX语法创建一个应用，否则不要使用React.createElement方法。

## Component(组件规范)

### Class 与 React.createClass方法

尽可能地使用ES6中的类的语法，除非有特殊的对于Mixin的需求。

```javascript
  // bad
  const Listing = React.createClass({
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    }
  });

  // good
  class Listing extends React.Component {
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    }
  }
```

函数的定义不要使用箭头定义，应该使用function定义

```javascript
  // bad
  class Listing extends React.Component {
    render() {
      return <div>{this.props.hello}</div>;
    }
  }

  // bad (relying on function name inference is discouraged)
  const Listing = ({ hello }) => (
    <div>{hello}</div>
  );

  // good
  function Listing({ hello }) {
    return <div>{hello}</div>;
  }
```

### 组件命名

- 扩展名：使用.jsx作为React组件的扩展名。
- 文件名：使用帕斯卡命名法命名文件，譬如ReservationCard.jsx。
- 引用命名：使用帕斯卡命名法命名组件和camelCase命名实例。

```javascript
  // bad
  const reservationCard = require('./ReservationCard');

  // good
  const ReservationCard = require('./ReservationCard');

  // bad
  const ReservationItem = <ReservationCard />;

  // good
  const reservationItem = <ReservationCard />;
```

组件的命名与导入，组件命名已大写字母开头，已文件夹为准，导入时不需要指定到index

```javascript
  // bad
  import Footer from './Footer/Footer';

  // bad
  import Footer from './Footer/index';

  // good
  import Footer from './Footer';
```

使用函数式返回组件时，需要给每个组件组要给他定义一个名称，而不是默认返回传入的组件，这样不利于分辨组件的用途。

```javascript
  // bad
  export default function withFoo(WrappedComponent) {
    return function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }
  }

  // good
  export default function withFoo(WrappedComponent) {
    function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }

    const wrappedComponentName = WrappedComponent.displayName
      || WrappedComponent.name
      || 'Component';

    WithFoo.displayName = `withFoo(${wrappedComponentName})`;
    return WithFoo;
  }
```

### Declaration(声明)

- 不要使用displayName来命名组件，而使用引用。

```javascript
  // bad
  export default React.createClass({
    displayName: 'ReservationCard',
    // stuff goes here
  });

  // good
  export default class ReservationCard extends React.Component {
  }
```

### Props

- 对于Props的命名使用camelCase。

```javascript
  // bad
  <Foo
  UserName="hello"
  phone_number={12345678}
  />

  // good
  <Foo
    userName="hello"
    phoneNumber={12345678}
  />
```

props值为true时省略值

```javascript
  // bad
  <Foo
    hidden={true}
  />

  // good
  <Foo
    hidden
  />
```

图片定义需要加上相应的提示

```javascript
// bad
<img src="hello.jpg" />

// good
<img src="hello.jpg" alt="Me waving hello" />

// good
<img src="hello.jpg" alt="" />

// good
<img src="hello.jpg" role="presentation" />
```

不要用数组的索引做为key，应该更多的使用数据的ID作为key

```javascript
  // bad
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // good
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
```

- 将Props或者State的声明写在类外。

```javascript
  import React, { Component, PropTypes } from 'react';

  const propTypes = {
    id: PropTypes.number.isRequired,
    url: PropTypes.string.isRequired,
    text: PropTypes.string,
  };

    const defaultProps = {
    text: 'Hello World',
  };

  export default class Link extends Component {
    static methodsAreOk() {
      return true;
    }

    render() {
      return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
    }
  }

  Link.propTypes = propTypes;
  Link.defaultProps = defaultProps;

  export default Link;
```

## JSX(JSX规范)

### Alignment(对齐)

- 跟随如下的JSX的语法

```javascript
  // bad
  <Foo superLongParam="bar"
      anotherSuperLongParam="baz" />

  // good
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  />

  // if props fit in one line then keep it on the same line
  <Foo bar="bar" />

  // children get indented normally
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  >
    <Spazz />
  </Foo>
```

### Quotes

对于JSX的属性用双引号表示，对于其他属性，用单引号表示。

```javascript
  // bad
  <Foo bar='bar' />

  // good
  <Foo bar="bar" />

  // bad
  <Foo style={{ left: "20px" }} />

  // good
  <Foo style={{ left: '20px' }} />
```

### Spacing(空格)

- 在自闭合的标签中仅使用单空格。

```javascript
  // bad
  <Foo/>

  // very bad
  <Foo                 />

  // bad
  <Foo
  />

  // good
  <Foo />

  // bad
  <Foo className="stuff"></Foo>

  // good
  <Foo className="stuff" />
```

### 多段

- 当JSX包含多行代码时，将它们包含在小括号中。

```javascript
  /// bad
  render() {
  return <MyComponent className="long body" foo="bar">
            <MyChild />
          </MyComponent>;
  }

  // good
  render() {
    return (
      <MyComponent className="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  }

  // good, when single line
  render() {
    const body = <div>hello</div>;
    return <MyComponent>{body}</MyComponent>;
  }
```

## bind

不要在事件中bind当前this，应该在构造器中bind当前this

```javascript
  // bad
  class extends React.Component {
    onClickDiv() {
      // do stuff
    }

    render() {
      return <div onClick={this.onClickDiv.bind(this)} />
    }
  }

  // good
  class extends React.Component {
    constructor(props) {
      super(props);

      this.onClickDiv = this.onClickDiv.bind(this);
    }

    onClickDiv() {
      // do stuff
    }

    render() {
      return <div onClick={this.onClickDiv} />
    }
  }
```

## Methods

### Naming(方法命名)

- 对于一个React组件的内部方法，不要使用下划线作为前缀。

```javascript
  // bad
  React.createClass({
    _onClickSubmit() {
        // do stuff
    }

    // other stuff
  });

  // good
  class extends React.Component {
    onClickSubmit() {
        // do stuff
    }

    // other stuff
  });
```

### Ordering(顺序)

1. React.Component子类
2. constructor
3. optional static methods
4. getChildContext
5. componentWillMount
6. componentDidMount
7. componentWillReceiveProps
8. shouldComponentUpdate
9. componentWillUpdate

10. componentDidUpdate

11. componentWillUnmount

12. clickHandlers or eventHandlers like onClickSubmit() or onChangeDescription()
13. getter methods for render like getSelectReason() or getFooterContent()
14. Optional render methods like renderNavigation() or renderProfilePicture()
15. render

16. React.createClass

17. displayName

18. propTypes
19. contextTypes
20. childContextTypes
21. mixins
22. statics
23. defaultProps
24. getDefaultProps
25. getInitialState
26. getChildContext
27. componentWillMount
28. componentDidMount
29. componentWillReceiveProps
30. shouldComponentUpdate
31. componentWillUpdate
32. componentDidUpdate
33. componentWillUnmount
34. clickHandlers or eventHandlers like onClickSubmit() or onChangeDescription()
35. getter methods for render like getSelectReason() or getFooterContent()
36. Optional render methods like renderNavigation() or renderProfilePicture()
37. render
