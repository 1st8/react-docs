---
title: Multiple clients
---

With `react-apollo` it's possible to use multiple instances of ApolloClient in your application.

<h2 id="providing-clients">Providing Clients</h2>

You're already familiar how to provide a default ApolloClient to your component tree via `ApolloProvider`.

Adding other clients might be necessary, depending on your use-case, and can be accomplished easily with `ApolloProvider`, too.   
Simply set the `as` prop to a value different than `"default"` when providing the client.

```jsx
import ApolloClient from 'apollo-client';
import { ApolloProvider } from 'react-apollo';

const client = new ApolloClient();
const extraClient = new ApolloClient();

ReactDOM.render(
  <ApolloProvider client={client}>
    <ApolloProvider client={extraClient} as="extraClient">
      <MyRootComponent />
    </ApolloProvider>
  </ApolloProvider>,
  rootEl
)
```

Your components can now access both the default and extra client.   
If you omit the `as` prop, the client will be provided as the `"default"` client.

Please note, that ApolloProvider also adds the clients `store` to the context, shadowing any previous `store` value.   
This can be a problem when you are using redux, but can be mitigated by integrating both clients with your existing store under [different reducer keys](/react/redux.html#creating-a-store) or using the redux `Provider` after your `ApolloProvider`.

<h2 id="using-apollo">Using Apollo</h2>

The only change to select a client, other than the default one, is to provide a `use` option in your regular `graphql` or `withApollo` usage.   
It specifies the name of the client to use, in this example `'extraClient'`.

Again, if you omit the `use` option, the default client will be used.

```jsx
export default compose(
  graphql(query),
  graphql(specialQuery, { name: 'extraData', use: 'extraClient' }),
)(Component);
```

The results of `query` will be available to `Component` under the regular `data` prop.   
Because we set a name for the second `graphql` instruction, the results of `specialQuery` will be available under the `extraData` prop.
