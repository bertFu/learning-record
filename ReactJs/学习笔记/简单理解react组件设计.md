# 简单理解React组件设计

[原文:深入理解 React](http://reactjs.cn/react/docs/thinking-in-react.html)

## 需求

### 功能：

- 通过用户输入，可以过滤列表内容
- 通过单选框可以过滤没有库存的情况
- 列表头显示 `Name`、 `Price` 两列
- 区分分组信息
- 每组显示组内列表
- 没有库存的商品特殊显示

### 设计原型：

![需求](../images/thinking-in-react-mock.png)

### 服务端返回Json：

```json
[
  {category: "Sporting Goods", price: "$49.99", stocked: true, name: "Football"},
  {category: "Sporting Goods", price: "$9.99", stocked: true, name: "Baseball"},
  {category: "Sporting Goods", price: "$29.99", stocked: false, name: "Basketball"},
  {category: "Electronics", price: "$99.99", stocked: true, name: "iPod Touch"},
  {category: "Electronics", price: "$399.99", stocked: false, name: "iPhone 5"},
  {category: "Electronics", price: "$199.99", stocked: true, name: "Nexus 7"}
];
```

## 分析

### 使用 Jquery 思路分析

> 对 Jquery 掌握不深，简单做分析: Jquery 是一门对Dom操作的艺术，我的理解是，Dom就是 Jquery 的数据库。

- 通过用户输入，可以过滤列表内容

  - 需要一个 `input[type=text]` 的表单项
  - 在列表的DOM中设置 `data-name` 商品名称
  - 用户输入的内容触发 `onChange` 事件，遍历Dom的 `data-name` 隐藏/显示匹配的内容

- 通过单选框显示隐藏没有库存内容

  1. Dom设置 `data-stock`:

    - 需要一个 `input[type=redio]` 的表单项
    - 在列表的Dom中设计 `data-stock` 的值
    - 用户点击Redio触发 `onClick/onChage` 事件，遍历Dom `data-stock` 隐藏显示/匹配内容

  2. Class设置 `no-stock`:

    - `no-stock` 设置特殊样式
    - 用户点击Redio时，设置 `no-stock` 的 `display: none/block` 来控制没有库存情况

- 列表头显示 `Name`、 `Price` 两列

  - Html输出

- 区分分组信息，每组显示组内列表

  - 处理服务端返回Json，`category` 内容显示成 `head`
  - 相同 `category` 以列表形式显示
  - 设置 `data-name` 等于 `name`
  - 设置 `no-stock` 的 `Name` 样色为 `red`

### 使用 React 思路分析

![分解](../images/thinking-in-react-components.png)

> 初学 React ，简单分析：React 是一门操作组件的艺术，React 操作内存中的数据，通过对新旧数据的判断生成Dom，React 不在局限于Dom本身，将数据处理权交给js。

```
├── FilterableProductTable            # （橘色）： 包含整个例子的容器
├── SearchBar                         # （蓝色）： 接受所有 用户输入（ user input ）
├── ProductTable                      # （绿色）： 根据 用户输入（ user input ） 过滤和展示 数据集合（ data collection ）
│   ├── ProductCategoryRow            # （青色）： 为每个 分类（ category ） 展示一列表头
│   ├── ProductRow                    # （红色）： 为每个 产品（ product ） 展示一列
```

#### 搭建静态模板

1. 输入组件
2. 表格组件
3. `category` 展示组件
4. 列表组件

> 通过对 Json 数据的处理，向组件发送数据

#### 设计 State

> 首先我们要了解 `State` 的设计原则

> 1. 是否是从父级通过 `props` 传入的？如果是，可能不是 `state` 。

>   - 可以通过外部传入的数据，不应该使用 `state` 管理

> 2. 是否会随着时间改变？如果不是，可能不是 `state` 。

>   - 不会改变的数据，不应该使用 `state` 管理

> 3. 能根据组件中其它 `state` 数据或者 `props` 计算出来吗？如果是，就不是 `state` 。

>   - 能够通过 `props` 计算出来的不应该使用 `state` 管理

> 对于应用中的每一个 state 数据：

> - 找出每一个基于那个 state 渲染界面的组件。
> - 找出共同的祖先组件（某个单个的组件，在组件树中位于需要这个 state 的所有组件的上面）。
> - 要么是共同的祖先组件，要么是另外一个在组件树中位于更高层级的组件应该拥有这个 state 。
> - 如果找不出拥有这个 state 数据模型的合适的组件，创建一个新的组件来维护这个 state ，然后添加到组件树中，层级位于所有共同拥有者组件的上面。

- `FilterableProductTable`：作为总控层

  - `state.filterText`：记录搜索内容
  - `state.stocked`：记录是否选择过滤没有库存
  - `products`：记录展示数据

- `SearchBar`：向 `FilterableProductTable` 提供更新 `state.filterText` 与 `state.stocked` 的方法

- `ProductTable`：判断 `state.filterText` 与 `state.stocked` 的值，处理出需要显示的列表内容

  - `ProductCategoryRow`：显示组名
  - `ProductRow`：遍历显示出组下的列表

每层组件都处理自己分内的事情，父级组件向子集组件提供需要由子集组件处理的数据。<br>
顶层组件作为总控制，控制所有数据的变化，并向子集组件发送数据
