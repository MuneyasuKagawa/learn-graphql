# Learning Graphql

## Init

1. typeDefs でエンティティを定義する(Query が一般的なエントリーポイント)

```typescript
const typeDefs = gql`
  type Todo {
    id: ID!
    title: String!
    completed: Boolean!
  }

  type Query {
    getTodos: [Todo!]!
  }
`;
```

2. resolvers でエンティティの振る舞いを定義する

```typescript
const resolvers = {
  Query: {
    getTodos: () => todos,
  },
};
```

3. サーバー立ち上げ時に typeDefs と resolvers を渡す

```typescript
const server = new ApolloServer({
  typeDefs,
  resolvers,
});
```

## Query と Mutation

用途によってどちらを使うかが決まる
| Operation | HTTP Method |
| --- | --- |
| Query | GET |
| Mutation | POST / PUT / DELETE |

## Query

1. resolver に関数を追加する

```typescript
const resolvers = {
  Query: {
    getTodos: () => todos,
  },
};
```

2. typeDifs に定義を追加する

```typescript
const typeDefs = gql`
  type Todo {
    id: ID!
    title: String!
    completed: Boolean!
  }

  type Query {
    getTodos: [Todo!]!
  }
`;
```

3. クエリを叩く

```graphql
query GetTodos {
  getTodos {
    id
    title
    completed
  }
}
```

## Mutation

1. resolver に関数を追加する

```typescript
const resolvers = {
  Query: {
    getTodos: () => todos,
  },
  Mutation: {
    addTodo: (_: unknown, { title }: { title: string }) => {
      const newTodo = {
        id: String(todos.length + 1),
        title,
        completed: false,
      };
      todos.push(newTodo);
      return newTodo;
    },
  },
};
```

ここで引数は以下のようになっている(ので第一引数は\_)  
`resolver(parent, args, context, info)`

2. typeDefs に定義を追加する

```typescript
type Mutation {
  addTodo(title: String!): Todo!
}
```

3. クエリを叩く

```graphql
mutation AddTodo($title: String!) {
  addTodo(title: "hoge") {
    title
  }
}
```
