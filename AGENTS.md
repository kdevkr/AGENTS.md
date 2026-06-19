# AGENTS.md

에이전트 지침 및 정보가 기록되는 공간입니다.

## 공통 규칙 (General Rules)

### 1. 보안: 민감 정보(API Key, 비밀번호) 노출 금지
*   코드 본문이나 설정 파일 내에 API Key, 데이터베이스 비밀번호, 개인 키(Private Key) 등의 민감한 정보를 직접 기입(하드코딩)하지 않습니다.
*   환경 변수(`.env` 등)를 사용하도록 처리하며, 해당 정보가 포함된 파일이 Git에 커밋되지 않도록 `.gitignore` 설정을 철저히 준수합니다.

### 2. 정리: 임시 파일 및 테스트 파일 관리
*   작업을 검증하기 위해 생성하는 임시 테스트 스크립트나 결과 파일들은 프로젝트 루트의 임시 디렉토리(예: `tmp/`, `.tmp/`) 내부에서만 생성하여 작업해야 하며, 해당 임시 디렉토리는 반드시 `.gitignore`에 추가하여 원격 저장소에 커밋되지 않도록 강력하게 차단(가드)해야 합니다.
*   임시 작업 및 검증이 끝난 후에는 생성한 임시 디렉토리와 파일들을 깔끔하게 정리(삭제)합니다.

### 3. 커밋 규칙: Conventional Commits v1.0.0 준수
Git 커밋 메시지는 반드시 [Conventional Commits v1.0.0](https://www.conventionalcommits.org/ko/v1.0.0/) 규격을 준수하여 작성합니다.

*   **구조:**
    ```text
    <type>[optional scope]: <description>

    [optional body]

    [optional footer(s)]
    ```
*   **주요 타입(Type) 정의:**
    *   `feat`: 새로운 기능 추가
    *   `fix`: 버그 수정
    *   `docs`: 문서 관련 변경 (README, AGENTS.md 등)
    *   `style`: 코드 의미에 영향을 주지 않는 포맷팅, 세미콜론 누락 수정 등 (코드 스타일 변경)
    *   `refactor`: 버그를 수정하거나 기능을 추가하지 않는 코드 리팩토링
    *   `perf`: 성능 향상을 위한 코드 변경
    *   `test`: 테스트 코드 추가 및 수정
    *   `build`: 빌드 시스템 또는 외부 종속성 관련 변경 (npm, maven 등)
    *   `ci`: CI 설정 파일 및 스크립트 변경 (GitHub Actions 등)
    *   `chore`: 소스 코드나 테스트 파일을 수정하지 않는 기타 변경 작업
    *   `revert`: 이전 커밋을 되돌림

### 4. Git 작업 승인 규정
*   **커밋(Commit) 및 푸시(Push) 사전 승인:** 코드 변경 사항에 대해 로컬 커밋을 실행하거나 원격 저장소로 푸시하기 전에, 반드시 변경 파일 목록과 변경 내용(diff)을 사용자에게 제시하고 명시적인 승인을 받아야 합니다.
*   **로컬 커밋 병합 (Amend):** 원격 저장소에 푸시(Push)되지 않은 로컬 커밋들은 히스토리가 파편화되지 않도록 `git commit --amend` 등을 사용하여 하나의 커밋으로 병합해 깔끔한 상태를 유지합니다.
*   **Git Pull 시 Rebase 수행:** 원격 저장소의 최신 변경 사항을 가져올 때 불필요한 머지 커밋(Merge commit)이 생성되어 히스토리가 지저분해지지 않도록, 기본적으로 `git pull --rebase`를 사용하여 선형 히스토리를 유지합니다.

---

## 자바 (Java) 코딩 규칙

### 1. FQN(Fully Qualified Name) 인라인 사용 금지
코드 본문 내에서 클래스의 전체 패키지 경로(FQN)를 직접 명시하는 인라인 작성을 금지하며, 반드시 파일 상단에 `import` 문을 정의하여 사용합니다.

*   **이유:** 코드 가독성을 높이고 코드 라인 길이를 간결하게 유지하기 위함입니다.
*   **올바르지 않은 예 (Bad):**
    ```java
    java.util.List<String> list = new java.util.ArrayList<>();
    ```
*   **올바른 예 (Good):**
    ```java
    import java.util.List;
    import java.util.ArrayList;

    List<String> list = new ArrayList<>();
    ```

*   **예외 사항 (Exceptions):**
    동일한 파일 내에서 서로 다른 패키지에 속한 동일한 이름의 클래스를 동시에 사용해야 하여 네이밍 충돌(Name Collision)이 발생하는 경우는 예외적으로 FQN 인라인 사용을 허용합니다. (예: `java.util.Date`와 `java.sql.Date`를 한 클래스 내에서 함께 사용하는 경우)
