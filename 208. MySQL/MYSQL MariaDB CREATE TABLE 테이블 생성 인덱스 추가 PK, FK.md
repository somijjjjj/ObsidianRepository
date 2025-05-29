


```SQL
# 사용자사전
CREATE TABLE `tb_rule_prefix_info` (
  `PREFIX_DETAIL_SEQ` int(10) NOT NULL AUTO_INCREMENT COMMENT '사용자사전 상세 규칙 시퀀스',
  `RULE_SEQ` int(10) NOT NULL COMMENT '규칙 시퀀스',
  `PREFIX_TARGET` char(1) NOT NULL DEFAULT 'A' COMMENT '적용대상(A: 전체, T:해당)',
  `PREFIX_TYPE` char(1) NOT NULL COMMENT '전처리 유형(R: 교체, D: 제거)',
  `PREFIX_CONDITION` varchar(200) NOT NULL COMMENT '필요 조건 내용',
  `CONDITION_TYPE` char(1) NOT NULL COMMENT '필요 조건 유형(1: Equals, 2: Not Equals, 3: Contains, 4: Not Contains)',
  `REGEX_YN` char(1) NOT NULL DEFAULT 'N' COMMENT '정규식 여부(Y/N)',
  `PREFIX_CONTENT` varchar(100) DEFAULT NULL COMMENT '바꿀 내용',
  `POS` varchar(10) DEFAULT NULL COMMENT '바꿀 내용의 품사',
  `SORT_NUM` int(3) DEFAULT 0 COMMENT '정렬 순서',
  PRIMARY KEY (`PREFIX_DETAIL_SEQ`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='규칙 사용자사전 규칙 정보 테이블';
```