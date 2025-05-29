

```SQL

-- 트랜잭션 시작
START TRANSACTION;

-- 사용자 변수 초기화
SET @current_master_seq = NULL;
SET @rank = 0;

-- 사용자사전 넘버링 업데이트 쿼리
UPDATE tb_prefix_detail_info AS t
JOIN (
    SELECT
        PREFIX_DETAIL_SEQ,
        PREFIX_MASTER_SEQ,
        @rank := IF(@current_master_seq = PREFIX_MASTER_SEQ, @rank + 1, 1) AS new_sort_num,
        @current_master_seq := PREFIX_MASTER_SEQ
    FROM tb_prefix_detail_info
    ORDER BY PREFIX_MASTER_SEQ, PREFIX_DETAIL_SEQ 
) AS numbered
ON t.PREFIX_DETAIL_SEQ = numbered.PREFIX_DETAIL_SEQ
SET t.SORT_NUM = numbered.new_sort_num;

-- 에러 발생 시 롤백
# ROLLBACK;

-- 트랜잭션 커밋 (성공적으로 완료된 경우)
COMMIT;
```
