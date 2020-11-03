# Fix: Cache data may be lost when replacing the getAllPosts field of a Query object.

Supposing we have a ```getAllPosts``` query that returns all posts. When we  perform a ```delete``` operation using the recently released ```Apollo v3``` and uses ```refetchQueries``` to update our UI, we are greeted with this ```warning```:
```sh
Cache data may be lost when replacing the getAllPosts field of a Query object.

To address this problem (which is not a bug in Apollo Client), define a custom merge function for the Query.getAllOccupants field, so InMemoryCache can safely merge these objects:

  existing: [{"__ref":"postData:5f650581d6cbc41abf422cd3"},{"__ref":"postData:5f6505a9d6cbc41abf422cd4"}]
  incoming: [{"__ref":"postData:5f650581d6cbc41abf422cd3"}]

For more information about these options, please refer to the documentation:

  * Ensuring entity objects have IDs: https://go.apollo.dev/c/generating-unique-identifiers
  * Defining custom merge functions: https://go.apollo.dev/c/merging-non-normalized-objects
  ``` 

  To fix this issue, simply locate the file where you did your ```Apollo``` set up and update your ```InMemoryCache``` with __typePolicies__ like so: 
  ```js
  // This method has always worked for me.
  const cache = new InMemoryCache({
    typePolicies: {
      Query: {
        fields: {
          getAllPosts: {
            merge(existing, incoming) {
              return incoming;
            }
        },
      },
    },
  });
```
Another option is to return ```merge: true```

  ```js
  const cache = new InMemoryCache({
    typePolicies: {
      Query: {
        fields: {
          getAllPosts: {
            merge: true,
            }
        },
      },
    },
  });
```

Another option is to return the ```ids``` of all the queries. Whichever way, one of these should work for you.

You can add other queries that gets that same error after a delete opweration. In this case let's add ```getAllAuthors``` query that throws the same error when an ```author``` is deleted.

  ```js 
  const cache = new InMemoryCache({
    typePolicies: {
      Query: {
        fields: {
          getAllPosts: {
            merge(existing, incoming) {
              return incoming;
          }
        },
         getAllAuthors: {
            merge(existing, incoming) {
              return incoming;
          }
        },
      },
    },
  });
```