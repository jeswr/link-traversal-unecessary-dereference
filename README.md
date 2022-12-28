
Setup

```bash
npm i
```

Start running the server

```bash
npm run serve
```

In a separate terminal run each of the following commands

```
npm run demo:q1
npm run demo:q1:traverse
npm run demo:q2
npm run demo:q2:traverse
```

The `q1` commands runs the query

```rq
SELECT ?a ?b ?c WHERE {
  <http://localhost:3010/data.n3#me> a ?o .
  
  GRAPH ?o {
    ?a ?b ?c
  }
}
```

Without link traversal

```
[
{"a":"http://localhost:3010/data.n3#a","b":"http://localhost:3010/data.n3#b","c":"http://localhost:3010/data.n3#c"}
]
```

is produced.

With link traversal the engine seems to be derefencing links to infinitum

---

The `q2` commands run the query

```rq
SELECT ?o WHERE {
  <http://localhost:3010/data.n3#me> a ?o .
}
```

With and without link traversal enabled we get the following result and only <http://localhost:3010/data.n3> is dereferenced.

```
[
{"o":"_:bc_0_n3-0"}
]
```

---

I argue that `q1` should only need to dereference <http://localhost:3010/data.n3> even with link traversal enabled as all quads live inside a graph with a blank node identifier; therefore *all* triples in that graph must live in <http://localhost:3010/data.n3> and so we should not be derefencing any more documents than `q2`.
