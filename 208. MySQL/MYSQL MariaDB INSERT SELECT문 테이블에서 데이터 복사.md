

```SQL

INSERT	INTO
	tb_rule_preprocess_info (
		RULE_SEQ,
		PREPROCESS_TYPE,
		PREPROCESS_CONDITION,
		CONDITION_TYPE,
		REGEX_YN,
		PREPROCESS_CONTENT ,
		`POINT`,
		SORT_NUM
	)
SELECT
	RULE_SEQ,
	PREPROCESS_TYPE,
	PREPROCESS_CONDITION,
	CONDITION_TYPE,
	REGEX_YN,
	PREPROCESS_CONTENT ,
	`POINT`,
	SORT_NUM
FROM
	tb_rule_preprocess_info_back;
```