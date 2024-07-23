# 경로 및 버전 관리

LC5에서는 UI 생성 뿐 아니라 서비스 접근 및 빌드에 관련된 기능도 지원합니다. 경로 관리와 버전 관리는 지원 중인 기능 중 하나입니다.

## 경로 관리

![Image](assets/path_version/path.png)

Builder 페이지의 Paths 테이블에서 해당 UI가 연결될 URL path를 설정할 수 있습니다.

경로를 지정하고 Meta를 저장하면, LC5가 설치된 도메인 밑의 해당 경로에서 생성한 서비스에 접근할 수 있습니다.

Path는 nested Path와 URL 파라미터를 지원합니다. URL 파라미터를 설정하고 싶을 경우 `/path/:paramName` 형식으로 Path를 지정합니다. 아래는 URL 파라미터를 설정한 예시입니다.

![Image](assets/path_version/urlparam.png)
`:paramName` 위치에 파라미터 값을 넣어서 해당 path에 접근할 경우, [Function의 params 파라미터](/lc5/reference/function/#params)에서 이 값을 가져올 수 있습니다.

이외에도 Paths 테이블에서는 Mode와 각 Path에 대한 설명인 Title을 입력할 수 있습니다. Mode에 대한 설명은 [Function 페이지의 mode 파트](/lc5/reference/function/#mode)를 참고해 주세요.

## 버전 관리

생성한 페이지는 버저닝을 지원하여 버전 히스토리를 통해 이전 작업 내역의 페이지를 저장할 수 있습니다. 또한 특정 버전을 직접 선택하여 배포하는 것이 가능합니다.
