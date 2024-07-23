# 함수 파라미터

LC5 내에서 함수를 작성하고자 할 때, 원하는 비즈니스 로직을 작성하기 위해서는 해당 페이지의 상태값, 외부 모듈, 트리거된 이벤트 등에 대한 접근이 필수적일 것입니다. LC5에서는 자유도 높은 로직 작성을 위해 생성 중인 페이지와 연결되는 풍부한 파라미터를 제공합니다.

현재 지원 중인 파라미터 리스트는 다음과 같습니다.

![Image](assets/function/parameter.png)

다음은 각 파라미터에 대한 설명입니다.

## state

앞서 [Meta, State 문서](meta_state.md)에서 설명했던 State 데이터가 바인딩됩니다. 이를 통해 함수 내 로직에서 현재 폼 및 테이블 등 컴포넌트들이 갖고 있는 값에 접근할 수 있습니다.

## history

[**Window.history**](https://developer.mozilla.org/en-US/docs/Web/API/History)를 복사하여 커스텀한 객체입니다. 이를 통해 `history.push`와 같은 페이지 이동 로직을 구현할 수 있습니다.

**해당 history가 Window.history의 모든 프로퍼티를 담고 있지 않다는 것을 유의해야 합니다.** 파라미터 안의 history는 `push`, `replace`, `state` 프로퍼티만 포함하고 있습니다. 이는 사용자의 브라우저 기록에 대한 직접적인 조작을 제한하고, 보안과 안정성을 유지하기 위함입니다.

## api

API-Hub에 등록한 모듈을 가져오는 파라미터입니다. `get`, `post`, `patch`, `put` 네 가지 메서드를 지원하며, `api.get([지정한 URL])`와 같은 형식으로 사용합니다. 해당 파라미터를 통해 API Hub에서 작성한 서버 로직을 LC5와 연결할 수 있습니다.

## currentUser

현재 서비스에 접속 중인 유저 정보를 가져옵니다.

## mode

Builder 페이지에서 지정할 수 있는 mode 데이터를 가져옵니다. mode란 같은 UI를 지녔지만 input의 readonly 속성만 다른 조회 페이지와 수정 페이지처럼, 일부 프로퍼티를 제외하면 UI가 완전히 똑같은 페이지를 별도의 Builder를 생성하는 수고로움 없이 하나의 Builder로 관리하기 위한 데이터입니다. mode 값으로는 `CREATE`, `READ`, `UPDATE`, `DELETE`가 올 수 있으며 설정하지 않으면 `undefined` 값을 갖습니다. Builder의 Paths 테이블에서 경로별로 설정할 수 있습니다.

## params

URL 파라미터를 `{[이름] : [값]}` 형태로 가져옵니다. 각 파라미터의 이름은 Builder의 Paths 테이블에서 설정할 수 있습니다. 만약 설정한 URL 파라미터가 없을 경우 빈 객체를 반환합니다.

## s3

AWS S3 SDK와 연결된 s3 모듈을 리턴합니다. 이를 통해 s3 관련 작업을 할 수 있습니다.

## file

pdf, xlsx 작업과 연결된 file 모듈을 리턴합니다. pdf 및 엑셀 파일 전환 작업 등을 할 때 사용합니다.

## fn

Builder에서 설정한 Global Functions를 가져옵니다. Global Functions란 최초 함수, 컴포넌트 함수 등 여러 함수에서 재사용할 수 있는 함수 로직을 저장할 수 있는 공간으로 Builder에서 설정할 수 있습니다. 주로 반복되는 함수 코드를 줄이고자 할 때 용이하며, 호출 시 지정한 이름을 붙여 `fn.funcName()` 형식으로 활용하면 됩니다. 지정한 Global Functions가 없을 경우 빈 객체를 반환합니다.

## oEvent

컴포넌트 이벤트 함수일 경우 이벤트 객체가 담깁니다. 이를 통해 이벤트로 들어온 value나 target 등에 접근할 수 있습니다. 최초 함수여서 이벤트가 들어오지 않을 경우 undefined를 반환합니다.

## require

@bsgp/lib-api, @bsgp/lib-core 등 내부적으로 정의된 라이브러리를 호출할 수 있는 함수입니다. `require("@bsgp/lib-core")` 형식으로 사용합니다.

## componentKey

컴포넌트 이벤트 함수일 경우 함수가 호출된 컴포넌트의 key를 반환합니다.
