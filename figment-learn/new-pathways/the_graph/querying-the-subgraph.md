## 🧐 Querying the subgraph

After deploying the subgraph, we need to wait a little in order to make it sync with the Ethereum mainnet.

We can follow the progression of the sync looking at the logged output by the running docker instance of our local graph node.

## 👨‍💻 Using the GraphQL endpoint

Having a fresh deployed subgraph make no sense if we don't query him. A graph node come up with a **GraphQL** endpoint, available at `http://localhost:8000/subgraphs/name/punks`.

We're going to display the ten most expensive trades done on the CryptoPunk contract.

```graphql
query {
  punks(first: 10, orderBy: value, orderDirection: desc) {
    id
    index
    owner {
      id
    }
    value
    date
  }
}
```

## 🥳 Enjoy the result of your work

Now, it's time to enjoy the result of your work! Click on the **Query the subgraph** button on the right, and say hello to the Punks.

## 🏁 Conclusion

Nice, you made it! You were able to complete the **THE GRAPH** pathway. If you want to learn more check ours [tutorials](https://www.learn.figment.io).