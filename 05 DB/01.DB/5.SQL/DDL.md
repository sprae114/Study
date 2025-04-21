## CREATE
새로운 데이터베이스 객체(테이블, 뷰, 인덱스 등)를 생성합니다.
```sql
CREATE TABLE 테이블명 (
열1 데이터타입 제약조건,
열2 데이터타입 제약조건,
...
);
```

## ALTER
기존 데이터베이스 객체의 구조를 변경합니다.
- 테이블에 열 추가
```sql
ALTER TABLE 테이블명 ADD 열명 데이터타입;
```

- 테이블의 열 수정
```sql
ALTER TABLE 테이블명 MODIFY 열명 데이터타입;
```

- 테이블의 열 삭제
```sql
ALTER TABLE 테이블명 DROP COLUMN 열명;
```

## DROP
데이터베이스 객체를 삭제합니다.
```sql
DROP TABLE 테이블명;
```

## TRUNCATE
테이블의 모든 데이터를 삭제하되, 테이블 구조는 유지합니다.
```sql
TRUNCATE TABLE 테이블명;
```