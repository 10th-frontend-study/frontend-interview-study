# npm에 대해 설명해 주세요.

<aside>
💡 Node Package Manager의 약자
- 명령어로 자바스크립트 라이브러리를 설치하고 관리하는 패키지 매니저

</aside>

- 개발자들은 몇 줄의 명령어로 공개된 모듈들을 설치하고 활용할 수 있다.

### 1. npm 필요성

- 프로젝트에서는 아주 많은 패키지를 사용하게 되는데, 이러한 패키지의 버전이 빈번하게 업데이트 되기 때문에, 프로젝트가 의존하고 있는 패키지들이 관리될 필요가 있다.
- `npm`은 `package.json` 파일로 프로젝트의 정보와 패키지들의 의존성을 관리한다.
  - 어떻게?
    - `dependencies`, `devDependencies` 안의 패키지 리스트로 관리
    - `package.json`은 리스트로 관리하지만, `package-lock.json`은 하위 의존성까지 정확하게 관리 ⇒ 자세한건 다음 주제로…

### 2. 사용 방법

1. `npm init`

- `npm`을 쓸 수 있는 초기 환경을 세팅한다.

> 직접 `npm init`을 해본 예시

![Untitled](../../resources/npm에%20대해%20설명해%20주세요./image1.png)

> `npm init` 이후 명령을 진행한 폴더에 `package.json` 파일이 생성된 것을 볼 수있다.

![Untitled](../../resources/npm에%20대해%20설명해%20주세요./image2.png)

- `package name` : 패키지명
- `version` : 패키지 버전 (1.0.0이 디폴트)
- `description` : 패키지 설명
- `entry point` : 시작 파일명 (`index.js`가 디폴트)
- `test command` : `npm test`를 호출할 때마다 실행되는 명령
- `git repository` : git 저장소 url
- `keywords` : 패키지 keyword
- `author` : 원작자 이름
- `license` : 패키지 사용자에 대한 라이선스

<br/>

2. `npm install ‘패키지명’`

- 원하는 패키지를 설치할 수있다.
- `npm install -D ‘패키지명’` : 개발, 테스트 할 때만 이 패키지를 사용한다. (실제 운영 단계에서 사용 x)
- `npm install -g '패키지명’` : 전역으로 해당 패키지를 설치하고싶을때 사용한다. ⇒ -g 옵션을 쓰면 시스템 레벨의 node modules에 저장된다.

> 그 외 명령어 참고

[CLI Commands | npm Docs](https://docs.npmjs.com/cli/v10/commands)

### 정리

- npm은 node package manager로 패키지의 버전 관리나 패키지를 설치해주는 역할을 한다. npm은 프로젝트 내에 패키지가 많아질 수록 여러 패키지 간의 의존성이 생기게 되는데 이것을 해결해주는 역할을 하기 때문에 필요하다.
