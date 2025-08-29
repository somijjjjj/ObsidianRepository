
### collection 기준 4뎁스 연결 조회
```
MATCH (n:MISO_RAG_COLLECTION {id: 'col_m7csp04q'})-[r*1..4]-(m) RETURN n, r, m
```


### collection 그래프 조회 
```
MATCH (n:__Entity__) WHERE ANY(cn IN ['col_ooolc0ad'] WHERE cn IN n.collection_name) OPTIONAL MATCH (n)-[r]-(m) RETURN n, r, m
```

### 

```sql
UNWIND ["01K3QRQZ2D7JWJ9SNP5M14Q56G","01K3QRQZ2DJ9GDJTFA6DBR04H8","01K3QRQZ2DK16DEKGA3NR1FRZ2","01K3QRQZ2DQCMRMDS16VARHFJ7","01K3QRQZ2DTVZNSS4KKF0GYVYG"] AS chunk_id 
    MATCH (chunk:MISO_RAG_CHUNK {id: chunk_id})
    OPTIONAL MATCH (chunk)<--(node1)
    OPTIONAL MATCH (node1)-[r2]-(node2)

    WHERE
        r2 IS NOT NULL
        AND node2 <> chunk
        AND NOT (
            node2:MISO_RAG_CHUNK
            AND type(r2) = 'MISO_RAG_CONTEXT_OF'
            AND startNode(r2) = node1
            AND node2 <> chunk
        )

    RETURN DISTINCT startNode(r2) AS n, r2 AS r, endNode(r2) AS m

```

- 입력한 여러 `chunk_id` 각각에 대해,    
- 청크(`chunk`)와 직접 연결된 모든 `node1`을 찾고,    
- 그 `node1`과 연결된 `node2`를 따라가 **2홉 이웃 관계 `r2`들을 수집**,    
- 단,    
    - 2홉이 원래 청크로 되돌아오는 경우 제외,        
    - `node1 -[:MISO_RAG_CONTEXT_OF]-> (청크)` 패턴의 2홉은 제외,        
- 그리고 그 관계의 시작/끝 노드와 관계 자체를 중복 없이 반환.


### 테이블 반환
```sql
UNWIND ["01K3QRQZ2D7JWJ9SNP5M14Q56G","01K3QRQZ2DJ9GDJTFA6DBR04H8","01K3QRQZ2DK16DEKGA3NR1FRZ2","01K3QRQZ2DQCMRMDS16VARHFJ7","01K3QRQZ2DTVZNSS4KKF0GYVYG"] AS chunk_id 
    MATCH (chunk:MISO_RAG_CHUNK {id: chunk_id})
    OPTIONAL MATCH (chunk)<--(node1)
    OPTIONAL MATCH (node1)-[r2]-(node2)

    WHERE
        r2 IS NOT NULL
        AND node2 <> chunk
        AND NOT (
            node2:MISO_RAG_CHUNK
            AND type(r2) = 'MISO_RAG_CONTEXT_OF'
            AND startNode(r2) = node1
            AND node2 <> chunk
        )

    RETURN DISTINCT startNode(r2) AS n, r2 AS r, endNode(r2) AS m
```

- 