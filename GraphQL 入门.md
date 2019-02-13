# GraphQL 入门

`GraphQL`是一个用于`API`的查询语言, 开发者可以通过`GraphQL`, 查询或操控自己所需的数据.

1. 可以自定义所需的字段名, 获取自己所需要的数据.
2. 多次请求可合并到一个请求, 只需多定义自己所需的字段名.
3. 完善的描述类型系统.

## 搭建一个博客`API`服务器, 实现增删改查功能
本项目采用`Apollo`进行开发.

[源码](https://github.com/xiaoyueguang/graphql-demo)

### 博客的数据类型
主要为两个表, 一个为`User`用户表, 一个为`Post`文章表

#### 用户表

|字段|类型|描述|
|:--|:--|:--|
|id|number|唯一ID|
|nickname|string|昵称|
|age|number|年龄|

#### 文章表

|字段|类型|描述|
|:--|:--|:--|
|id|number|唯一ID|
|title|string|标题|
|content|string|内容|
|userId|number|用户ID|


### 1. 编写`schema`
想搭建一个`GraphQL`的服务器, 首先要先对`API`进行描述, 对接口里的数据尽量的描述完整, 因此, 要先编写`schema`文件, 尽可能的对数据进行描述.

#### 定义一个类型
```
{
  type User {
    id: Int!
    nickname: String
    age: Int
  }
  type Query {
    users: [User]
    user (id: Int): User
    user1: User
  }
}
```
* `User`为一个`GraphQL`对象类型
* `id`, `nickname`, `age`则为类型的字段
* `Int`, `String` 为一个标量类型
* `Int!`表示这个字段是非空, 总是会返回该值
* `[User]`为一个列表, 列表里都为`User`对象类型
* `user (id: Int): User`表示`Query`类型可通过`id`来查找一个用户, 切返回一个`User`
* `user1: User`返回一个用户

#### 标量类型
`GraphQL`自带一组默认的标量类型
* `Int` 有符号32位整数
* `Float` 有符号双精度浮点数
* `String` 字符串
* `Boolean` 真假值
* `ID` 唯一标识符, 字符串
* 标量类型可通过`scalar`进行自定义[如何自定义? 例子](https://github.com/taion/graphql-type-json/blob/master/src/index.js)

#### 枚举
`enum`枚举类型. 限制一个可选值集合.fadsfdsf
```
enum Color {
  RED
  GREEN
  BLUE
}
```
#### 接口与联合类型
接口是一个抽象类型, 包含字段. 如果要实现该接口, 则要求对象类型必须包含接口里的字段.
```
interface Person {
  id: Int
  name: String
}
type Man {
  id: Int
  name: String
}
type Woman {
  id: Int
  name: String
}
```

联合类型也是一个抽象类型, 类似接口, 但是不需要制定任何共同字段.  
比如搜索结果中有用户或文章, 则需要使用联合类型
```
union Result = Post | User
type Query {
  search: [Result]
}
```

#### 输入类型
变更的时候, 如需要传入对象的时候, 则需要输入类型去声明
```
input UserInput {
  name: String
  password: String
}
```

#### 特殊类型
`Query`和`Mutation`为特殊类型, 主要用于查询或变更.
```
{
  query {
    users: [User]
  }
}

{
  mutation Login($user: UserInput) {
    token
  }
}

```
#### 注释
1. `# 注释` 这种注释只存在在开发中, 编译出来的文档中将不会有这行注释
2. `"注释"` 这种注释将显示在文档中作为补充

根据以上的内容, 博客的`schema`文件则编写如下, 仅实现查询功能.
```typescript
// server/typeDefs.ts
import { gql } from 'apollo-server'
export default gql`
  # 注释
  "用户"
  type User {
    id: Int!
    "昵称"
    nickname: String
    "年龄"
    age: Int!
  }
  "文章"
  type Post {
    id: Int!
    "标题"
    title: String
    "内容"
    content: String
    "用户"
    user: User
  }
  type Query {
    "获取文章列表"
    posts: [Post]
    "按ID获取文章"
    post (id: Int!): Post
    "获取用户列表"
    users: [User]
    "按ID或昵称获取用户信息"
    user (nickname: String, id: Int): User
  }
`
```

### 2.resolvers
写完了`schema`后, 则需要写`resolvers`, 按照上面的`schema`来编写对应的`resolvers`, 告诉我们怎么获取, 处理并返回数据.  
编写`resolvers`, 需要将`resolvers`里的方法与`schema`中一一对应, `apollo`才能正确工作.  

```typescript
interface User {
  id: number;
  nickname: string;
}
interface Post {
  id: number;
  title: string;
  userId: number;
}
// 假设有以下数据
const users: User[] = []
const posts: Post[] = []

const schema = gql`
  type Query {
    posts: [Post]
    post (id: Int!): Post
  }
  type Post {
    title: String
    user: User
  }
  type User {
    id: Int
    nickname: user
  }
`

const resolvers = {
  // 这里对应 type Query
  Query: {
    /**
    * 这里对应 type Query 中的 posts: [Posts]
    * resolvers 能接受方法返回为Promise<T>
    * @param parent 父字段传来的对象. Query或Mutation上的则为服务端设置的值
    * @param args 查询的参数
    * @param context 上下文. 解析器共享的对象, 包括状态请求, headers 等等.
    * @param info 当前查询的信息
    */
    async posts (parent, args, context, info) {
      return posts
    }
  },
  // 这里对应的 type Post
  Post: {
    // 这里对应的 type Post 中的 user: User.
    // 只要通过 Post 中获取 User 数据的, 都会经过这个方法.
    user (post: Post) {
      return Users.find(user => user.id === Post.userId)
    }
  }
}
```

### 3.启动服务器
`schema`与`resolvers`都已经编写完了, 则可以启动服务器进行查询了.  
```typescript
import { ApolloServer,  } from 'apollo-server'
// schema
import typeDefs from './typeDefs'
// resolvers
import resolvers from './resolvers'

const server = new ApolloServer({
  typeDefs,
  resolvers
});

server.listen(7000).then(({ url }) => {
  // 监听7000端口
  console.log(url)
})
```
### 4.使用`GraphQL Playground`查看文档或检查数据
服务器启动完成后, 可以先尝试在自带的`GraphQL Playground`中进行调试. 这是一个基于浏览器的图形查询工具, 可轻松实现`Query`或`Mutation`操作, 同时也能很方便的查看文档以及数据结构.

#### 接口请求结果
![接口请求结果](https://ws1.sinaimg.cn/large/006tNc79ly1fznedc06gfj30s30myq7s.jpg)

#### 查看文档信息
![查看文档信息](https://ws3.sinaimg.cn/large/006tNc79ly1fznee12qnzj311r0my43z.jpg)

### Mutation
以上基本都是查询数据操作, 即(`Query`). 如果需要修改数据, 则需要`Mutation`. 同样, 修改数据也需要写`schema`和对应的`resolvers`. 操作基本一致. 为了输入的时候能更好看, 则可以采用`input`去规范输入的值

```typescript
import { gql } from 'apollo-server'
export default gql`
  input PostInput {
    id: Int
    "标题"
    title: String
    "内容"
    content: String
    "用户ID"
    userId: Int
  }
  type Mutation {
    addPost (post: PostInput):
  }
`
```

## 浏览器中如何使用
服务端开发完了, 客户端可采用`Apollo Client`来获取数据, 同时浏览器端也需要编写`schema`文件, 来确定客户端需要什么数据, 这里可将请求的数据写到一个`*.gql`或`*.graphql`文件中, 通过`webpack`的`loader`(`graphql-tag/loader.js`)来加载文件.

```javascript
// api.js
import ApolloClient from 'apollo-boost'

const client = new ApolloClient({
  uri: '//127.0.0.1:7000'
})

// webpack.config.js
{
  test: /\.(graphql|gql)$/,
  exclude: /node_modules/,
  loader: 'graphql-tag/loader.js',
}
```
### 查询
```javascript
import Post from './gql/post.gql'
client
  .query({ query: Post, variables: { id } })
  .then(data => {
    // 处理 data
  })
```
```graphql
query post ($id: Int!) {
  # 获取需要的数据.
  post (id: $id) {
    id
    title
    content
    user {
      id
      nickname
    }
  }
}
```
### 修改
```javascript
import PostMutation from './gql/postMutation.gql'
client
  .mutate({ mutation: PostMutation, variables: {
    // input. 参考服务端配置的input, 传入一个对象即可.
    post: {
      title, content, userId
    }
  }})
  .then(data => {
    // 处理结果
  })
```

```graphql
mutation addPost ($post: PostInput) {
  addPost (post: $post) {
    code
    message
    data {
      title
      content
    }
  }
}
```

## 总结
利用`Apollo`开发完一个小项目后, 发现`GraphQL`对前端来讲, 开发时可以说是非常方便了, 无需找后端要接口或定制相应的接口.  

而后端相对来讲会比较麻烦, 编写`resolvers`, 导致以前使用的`ORM`都没法使用, 数据库一次查询会变为多次查询, 性能会低上不少, 改动也会比较大. 好在`apollo`请求接口获取数据的方式(只需要再加上一个中间件, 在接收客户端请求时, 去访问原先的接口服务器, 拼装出所需的数据返回回去), 简化了不少操作, 或者
`DataLoader`或其他也能解决该问题, 总的来说也是利大于弊的.
