# 데이터베이스의 종류 💻
## 1️⃣ 관계형 DB
- 행과 열을 가지는 표 형식 데이터를 저장하는 형태의 데이터 베이스
### ✨ MySQL
- 대부분의 운영체제와 호환되면 가장 많이 사용
- `C`, `C++` 로 만들어짐
- `MyISAM` 인덱스 압축 기술
- B-트리 기반의 인덱스
- 스레드 기반의 메모리 할당 시스템
- 빠른 조인
- 최대 64개의 인덱스 제공
### ✨ PostgreSQL
- SQL 뿐만 아니라 JSON으로도 데이터에 접근 가능

## 2️⃣ NoSQL DB
- Not only SQL
- SQL을 사용하지 않는 데이터 베이스
### ✨ MongoDB
- JSON을 통해 데이터에 접근
- Binary JSON 형태로 데이터가 저장됨
- 빅데이터를 저장할 때 성능이 좋음
- 도큐먼트(MySQL의 Row = MongoDB의 Document)를 생성할 때 마다 유니크한 값인 ObjectID가 생성됨
### ✨ redis
- 인메모리 데이터베이스
- `키-값` 데이터 모델 기반의 데이터 베이스
- 기본 데이터 타입 `string`
- 최대 512MB 저장
