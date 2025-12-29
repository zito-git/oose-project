# 직원 관리 및 조직 협업 시스템

Spring Boot 기반의 직원 관리, 부서 업무, 문서 관리, 학습 동아리, 일정 관리 기능을 제공하는 웹 애플리케이션

---

## 기술 스택

| 구분                | 기술              |
| ------------------- | ----------------- |
| **Backend**         | Spring Boot 3.5.0 |
| **Java Version**    | Java 21           |
| **Database**        | MariaDB           |
| **ORM**             | Spring Data JPA   |
| **Template Engine** | Thymeleaf         |
| **Build Tool**      | Gradle            |

---

## 프로젝트 구조

```
src/main/java/com/example/demo/
├── DemoApplication.java                 # 메인 애플리케이션
└── domain/
    ├── WebConfig.java                   # 정적 리소스 설정
    ├── home/
    │   └── HomeController.java          # 홈 페이지
    ├── employee/
    │   ├── EmployeeController.java
    │   ├── EmployeeEntity.java
    │   ├── EmployeeRepository.java
    │   ├── EmployeeService.java
    │   └── dto/
    │       ├── ReqEmployeeForm.java
    │       └── ResEmployeeDto.java
    ├── department/
    │   ├── DepartmentController.java
    │   ├── DepartmentEntity.java
    │   └── DepartmentRepository.java
    ├── departmentwork/
    │   ├── DepartmentWorkController.java
    │   ├── DepartmentWorkEntity.java
    │   ├── DepartmentWorkRepository.java
    │   ├── DepartmentWorkService.java
    │   └── dto/
    │       ├── ReqDepartmentWorkForm.java
    │       └── ResDepartmentWorkDto.java
    ├── document/
    │   ├── DocumentController.java
    │   ├── DocumentEntity.java
    │   ├── DocumentRepository.java
    │   ├── DocumentService.java
    │   └── dto/
    │       ├── ReqDocumentAddFormDto.java
    │       └── ResDocumentDto.java
    ├── learningclub/
    │   ├── LearningClubController.java
    │   ├── LearningClubEntity.java
    │   ├── LearningClubRepository.java
    │   ├── LearningClubService.java
    │   └── dto/
    │       ├── ReqLearningClubForm.java
    │       └── ResLearningClubDto.java
    └── schedule/
        ├── ScheduleController.java
        ├── ScheduleEntity.java
        ├── ScheduleRepository.java
        ├── ScheduleService.java
        ├── ScheduleViewableEntity.java
        ├── ScheduleViewableRepository.java
        └── dto/
            ├── ReqScheduleForm.java
            └── ResScheduleDto.java
```

---

## API 엔드포인트

### 홈

| Method | URL | 설명                              |
| ------ | --- | --------------------------------- |
| GET    | `/` | 메인 페이지 (전체 직원 목록 조회) |

### 직원 관리 (Employee)

| Method | URL              | 설명           | 파라미터                         |
| ------ | ---------------- | -------------- | -------------------------------- |
| GET    | `/employee/list` | 직원 목록 조회 | `userid` (선택) - 사용자 ID 검색 |
| GET    | `/employee/add`  | 직원 등록 폼   | -                                |
| POST   | `/employee/add`  | 직원 등록      | `ReqEmployeeForm`                |

### 부서 관리 (Department)

| Method | URL               | 설명         |
| ------ | ----------------- | ------------ |
| GET    | `/department/add` | 부서 추가 폼 |

### 부서 업무 관리 (Department Work)

| Method | URL                    | 설명              | 파라미터                                                      |
| ------ | ---------------------- | ----------------- | ------------------------------------------------------------- |
| GET    | `/departmentWork/list` | 부서 업무 조회    | `workName`, `workManager`, `startDate`, `endDate` (모두 선택) |
| GET    | `/departmentWork/add`  | 부서 업무 등록 폼 | -                                                             |
| POST   | `/departmentWork/add`  | 부서 업무 등록    | `ReqDepartmentWorkForm` (유효성 검사 적용)                    |

### 문서 관리 (Document)

| Method | URL                | 설명                    | 파라미터                                      |
| ------ | ------------------ | ----------------------- | --------------------------------------------- |
| GET    | `/document/list`   | 문서 목록 조회          | `type` (category/title/department), `keyword` |
| GET    | `/document/add`    | 문서 등록 폼            | -                                             |
| POST   | `/document/create` | 문서 생성 (파일 업로드) | `ReqDocumentAddFormDto`                       |
| GET    | `/document/test`   | 테스트 API (JSON)       | -                                             |

### 학습 동아리 (Learning Club)

| Method | URL                   | 설명                        | 파라미터                       |
| ------ | --------------------- | --------------------------- | ------------------------------ |
| GET    | `/learning_club/list` | 동아리 목록 조회            | `query` (선택) - 동아리명 검색 |
| GET    | `/learning_club/add`  | 동아리 등록 폼              | -                              |
| POST   | `/learning_club/add`  | 동아리 생성 (이미지 업로드) | `ReqLearningClubForm`          |

### 일정 관리 (Schedule)

| Method | URL              | 설명                         | 파라미터                                      |
| ------ | ---------------- | ---------------------------- | --------------------------------------------- |
| GET    | `/schedule/list` | 일정 목록 조회               | `keyword`, `startDate`, `endDate` (모두 선택) |
| GET    | `/schedule/add`  | 일정 등록 폼                 | -                                             |
| POST   | `/schedule/add`  | 일정 생성 (공유 대상자 지정) | `ReqScheduleForm`                             |

---

## 데이터베이스 스키마

### Employee (직원)

| 컬럼명      | 타입             | 설명               |
| ----------- | ---------------- | ------------------ |
| employee_id | BIGINT (PK)      | 직원 ID (자동증가) |
| userid      | VARCHAR (UNIQUE) | 사용자 ID          |
| password    | VARCHAR          | 암호화된 비밀번호  |
| phone       | VARCHAR          | 전화번호           |
| name        | VARCHAR          | 이름               |
| address     | VARCHAR          | 주소               |
| role        | VARCHAR          | 역할               |
| department  | VARCHAR          | 부서               |
| sms_receive | BOOLEAN          | SMS 수신 여부      |

### Department (부서)

| 컬럼명 | 타입        | 설명    |
| ------ | ----------- | ------- |
| id     | BIGINT (PK) | 부서 ID |
| name   | VARCHAR     | 부서명  |

### DepartmentWork (부서 업무)

| 컬럼명           | 타입        | 설명            |
| ---------------- | ----------- | --------------- |
| work_id          | BIGINT (PK) | 업무 ID         |
| work_name        | VARCHAR     | 업무명          |
| start_date       | DATE        | 시작일          |
| end_date         | DATE        | 종료일          |
| is_public        | BOOLEAN     | 공개 여부       |
| open_start_date  | DATE        | 공개 시작일     |
| open_end_date    | DATE        | 공개 종료일     |
| work_manager     | VARCHAR     | 담당자          |
| linked_unit_work | VARCHAR     | 연계 업무       |
| alarm_setting    | VARCHAR     | 알림 설정 (Y/N) |
| attachment_path  | VARCHAR     | 첨부파일 경로   |
| category         | VARCHAR     | 업무 카테고리   |
| department_id    | BIGINT (FK) | 부서 ID         |

### Document (문서)

| 컬럼명          | 타입        | 설명      |
| --------------- | ----------- | --------- |
| id              | BIGINT (PK) | 문서 ID   |
| title           | VARCHAR     | 제목      |
| contents        | TEXT        | 내용      |
| category        | VARCHAR     | 카테고리  |
| write_at        | DATETIME    | 작성 시간 |
| file_url        | VARCHAR     | 파일 URL  |
| department_name | VARCHAR     | 부서명    |
| employee_id     | BIGINT (FK) | 작성자 ID |

### LearningClub (학습 동아리)

| 컬럼명           | 타입        | 설명        |
| ---------------- | ----------- | ----------- |
| learning_club_id | BIGINT (PK) | 동아리 ID   |
| club_name        | VARCHAR     | 동아리명    |
| intro            | VARCHAR     | 소개        |
| img_url          | VARCHAR     | 이미지 URL  |
| employee_id      | BIGINT (FK) | 동아리장 ID |

### Schedule (일정)

| 컬럼명                | 타입        | 설명         |
| --------------------- | ----------- | ------------ |
| schedule_id           | BIGINT (PK) | 일정 ID      |
| schedule_name         | VARCHAR     | 일정명       |
| start_date            | DATE        | 시작일       |
| end_date              | DATE        | 종료일       |
| date_length           | INT         | 기간 (일 수) |
| share_yn              | BOOLEAN     | 공유 여부    |
| registration_datetime | DATETIME    | 등록 시간    |
| writer_id             | BIGINT (FK) | 작성자 ID    |

### ScheduleViewable (일정 공유 대상)

| 컬럼명               | 타입        | 설명              |
| -------------------- | ----------- | ----------------- |
| id                   | BIGINT (PK) | ID                |
| schedule_id          | BIGINT (FK) | 일정 ID           |
| viewable_employee_id | BIGINT (FK) | 조회 가능 직원 ID |

---

## 환경 변수 설정

애플리케이션 실행 전 다음 환경 변수를 설정해야 합니다:

| 변수명   | 설명                  | 예시                                 |
| -------- | --------------------- | ------------------------------------ |
| `DB_URL` | 데이터베이스 URL      | `jdbc:mariadb://localhost:3306/demo` |
| `DB_ID`  | 데이터베이스 사용자명 | `root`                               |
| `DB_PW`  | 데이터베이스 비밀번호 | `password`                           |

---

## 설치 및 실행

### 사전 요구사항

- Java 21+
- MariaDB
- Gradle

### 1. 프로젝트 클론

```bash
git clone https://github.com/your-username/springboot-test.git
cd springboot-test
```

### 2. 환경 변수 설정

```bash
# Linux/Mac
export DB_URL=jdbc:mariadb://localhost:3306/demo
export DB_ID=root
export DB_PW=password

# Windows (PowerShell)
$env:DB_URL="jdbc:mariadb://localhost:3306/demo"
$env:DB_ID="root"
$env:DB_PW="password"
```

### 3. 빌드 및 실행

```bash
# 빌드
./gradlew build

# 실행
./gradlew bootRun
```

### 4. 접속

브라우저에서 `http://localhost:8081` 접속

---

## 주요 기능

### 1. 직원 관리

- 직원 등록 및 조회
- BCrypt 비밀번호 암호화
- 중복 ID 방지
- 부서 및 역할 관리

### 2. 부서 업무 관리

- 부서별 업무 등록
- 복합 검색 (업무명, 담당자, 날짜 범위)
- 업무 공개 범위 설정
- 입력값 유효성 검사

### 3. 문서 관리

- 문서 등록 및 조회
- 파일 업로드 지원 (최대 10MB)
- 다중 검색 (카테고리, 제목, 부서)
- UUID 기반 파일명 생성

### 4. 학습 동아리

- 동아리 생성 및 조회
- 이미지 업로드 지원
- 중복 동아리명 방지
- 동아리장 관리

### 5. 일정 관리

- 일정 등록 및 조회
- 복합 검색 (일정명, 날짜 범위)
- 일정 기간 자동 계산
- 특정 직원에게 공유 기능

---

## 파일 업로드 경로

| 용도               | URL 경로     | 서버 경로              |
| ------------------ | ------------ | ---------------------- |
| 이미지 (동아리 등) | `/assets/**` | `/app/uploads/assets/` |
| 문서 첨부파일      | `/files/**`  | `/app/uploads/files/`  |
