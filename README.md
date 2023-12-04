## Steps

### Loading dataset

#### Load Nodes

```
LOAD CSV WITH HEADERS FROM 'file:///people.csv' AS row
CREATE (:Person {id: toInteger(row.id), name: row.name});
```

#### Load Relationships

```
LOAD CSV WITH HEADERS FROM 'file:///friends.csv' AS row
MATCH (source:Person {id: toInteger(row.source)})
MATCH (target:Person {id: toInteger(row.target)})
CREATE (source)-[:FRIEND]->(target);
```

### Get results
MATCH (p1:Person)-[:IS_FRIEND]->(p2:Person)
WHERE p1 <> p2
RETURN p1.name, COUNT(*) AS friendCount
ORDER BY friendCount DESC;


### use pagerank
CALL gds.graph.project(
'popularityGraph',
'Person',
'FRIEND'

CALL gds.pageRank.stream('popularityGraph')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).id AS id, score
ORDER BY score DESC, id ASC)
