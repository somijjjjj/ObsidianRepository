
```
collection 기준 4뎁스 연결 조회
MATCH (n:MISO_RAG_COLLECTION {id: 'col_n8tmimek'})-[r*1..4]-(m) RETURN n, r, m
```

```
MATCH (n:__Entity__) WHERE ANY(cn IN ['col_ooolc0ad'] WHERE cn IN n.collection_name) OPTIONAL MATCH (n)-[r]-(m) RETURN n, r, m
```