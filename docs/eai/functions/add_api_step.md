# REST API Step 추가

REST API를 호출하는 Step입니다.

> * API를 호출하기 위해서 공통적으로 사용되는 도메인과 헤더정보는 Config에 등록해야 합니다.    
> * Interface 별로 API의 path와 method가 다르기 때문에 Interface와 해당 정보를 매칭해주는 작업이 필요합니다. (4번 작업이 필수적으로 진행되야 합니다.) 

1. "Steps" tab에서 "Add" 버튼을 클릭합니다.
2. "Type" 컬럼에서 "API"를 선택합니다.
3. "Config" 컬럼에서 도메인과 헤더정보가 담긴 설정을 선택합니다.
4. "PreExecution"에서 Interface와 path, method 정보를 매칭시키는 작업을 진행합니다. 
```js
(draft, context) => {
  const { ifObj } = context
  let path
  switch (ifObj.id) {
    case "IF_API_TEST" : {
      path = "/posts"
      method = "POST"
    }
    break
    case "IF_API_GET": {
      path = "/posts/1"
      method = "GET"
    }
    break
    default:
  }
  
  ifObj.apiPath = path
  ifObj.apiMethod = method
  
  draft.json.ifObj = ifObj
}
```
5. "PostExecution"에는 현재 Step이 실행 완료된 후 동작하는 함수의 소스코드를 입력합니다.
