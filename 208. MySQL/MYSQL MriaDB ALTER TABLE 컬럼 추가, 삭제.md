
## 컬럼 추가
```SQL
ALTER TABLE 테이블명 ADD COLUMN 컬럼명 int(10) DEFAULT 0 COMMENT '' AFTER 컬럼명;
```

### 컬럼 삭제

```sql
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;

ALTER TABLE 테이블명
DROP COLUMN 컬럼명1,
DROP COLUMN 컬럼명2,
DROP COLUMN 컬럼명3;

```