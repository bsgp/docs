# State의 구조

앞서 [Meta, State 페이지](/lc5/concepts/meta_state)에서 Meta 데이터의 경우 Builder 및 Renderer 페이지에서 추상화되어 생성 및 사용되기 때문에 개발자는 그 구조에 대해 알 필요가 없다고 이야기했습니다. 하지만 **State의 경우 개발자가 필요에 의해 직접 업데이트해야 할 때가 있으므로 그 구조를 알 필요가 있습니다.[^1]** 지금부터는 State의 구조에 대해 설명하도록 하겠습니다.

State는 하나의 객체이며, 레이아웃-세부 컴포넌트 구조로 중첩된 객체 형식의 자료구조입니다. State의 구조를 대략적으로 나타내면 다음과 같습니다.

```
{
    [레이아웃 타입]: {
        [레이아웃 key]: {
            [세부 컴포넌트 key]: [세부 컴포넌트 value]
        } // 이 값은 레이아웃 타입에 따라 배열이 될 수도 있습니다.
    }
}
```

예시로 폼 및 테이블 정보를 가진 State 데이터의 일부를 보여드리겠습니다.

```
{
    "forms": {
        "forms1": {
            "name": "세희"
        }
    },
    "tables": {
        "tables1": [
            {
                "name": "영희",
                "age": 14
            },
            {
                "name": "철수",
                "age": 12
            },
            {
                "name": "영수",
                "age": 10
            },
            {
                "name": "맹구",
                "age": 14
            }
        ]
    }
}
```

대충 감이 오시나요? State는 이렇듯 중첩 객체들의 묶음입니다. 그럼 이제부터 각 Key와 Value가 어떻게 정해지는지 알아보겠습니다.

## 최상단 Key : 레이아웃 타입

[앞선 문서](/lc5/concepts/prebuilt_components#_2)에서 말했듯 레이아웃 컴포넌트에는 헤더, 폼, 테이블, 다이얼로그, 코드 에디터, 노드 에디터가 있습니다. 레이아웃 타입 값이란 이 값들을 의미하는 내부적으로 정의된 문자열을 말합니다.

각 타입별로 정의된 문자열은 다음과 같습니다.

| 레이아웃 타입 | State 내 타입 문자열 |
| :-----------: | :------------------: |
|      폼       |        forms         |
|    테이블     |        tables        |
|  다이얼로그   |       dialogs        |
|  코드 에디터  |     codeeditors      |
|  노드 에디터  |     nodeeditors      |

헤더의 경우 State로 정의할 만한 데이터가 없으므로 문자열이 존재하지 않습니다. 화면을 구성하는 Meta와 값을 나타내는 State가 다르다는 것을 항상 기억하세요.

이렇듯 State의 맨 상단 key는 타입 문자열로 이루어져 있습니다. 따라서 아무런 변경도 가하지 않은 State의 초기값은 다음과 같습니다.

```
{
    "forms": {},
    "tables": {},
    "dialogs": {},
    "codeeditors": {},
    "nodeeditors": {},
}
```

## 중간 Key : 레이아웃, 컴포넌트 Key

Builder 페이지의 모든 레이아웃 컴포넌트들과 세부 컴포넌트들은 **key**라는 속성을 필수로 갖습니다. key는 컴포넌트 등록 시 자동으로 생성되지만 개발자가 임의의 값을 지정할 수도 있습니다.

**key 값은 현재 컴포넌트가 위치한 계층 안에서 고유한 값이어야 합니다.** 다만 서로 다른 계층에 있는 컴포넌트의 경우 동일한 key를 가져도 됩니다.[^2]

위에서 등장했던 폼 및 테이블 예시 State에서, 폼 레이아웃의 key는 forms1이고 테이블 레이아웃의 key는 tables1입니다. 그리고 각 객체에 값으로 중첩되어 있는 하위 key들("name", "age")은 각 레이아웃에 속해 있는 세부 컴포넌트의 key들입니다.

## Value : 레이아웃 컴포넌트의 종류에 따른 값

State.["레이아웃 타입"].["레이아웃 key"] 아래에 오는 value는 레이아웃 컴포넌트의 종류에 따라 다른 객체 타입을 가집니다. 예시에서도 볼 수 있듯, table의 경우 value는 객체 배열입니다. 반면 form의 경우 `{"컴포넌트 key" : "컴포넌트 value"}` 형식을 가집니다.

또한 레이아웃 컴포넌트의 종류에 따라 State가 나타내는 의미가 달라집니다. 이를 정리하면 다음과 같습니다.

| 레이아웃 타입 |         State 구조 예시         |                             State 값 타입                              |                             설명                              |
| :-----------: | :-----------------------------: | :--------------------------------------------------------------------: | :-----------------------------------------------------------: |
|      폼       |       state.forms.formKey       |                 `{[componentKey] : [componentValue]}`                  |          폼에 속한 각 컴포넌트의 value를 나타냅니다.          |
|    테이블     |      state.tables.tableKey      |             `{[컬럼 key] : [컬럼 value]}` 형태의 객체 배열             |        테이블에 디스플레이되는 리스트 값을 의미합니다.        |
|  다이얼로그   |     state.dialogs.dialogKey     |                         `{isOpen : boolean }`                          | 다이얼로그 오픈 여부를 나타내는 isOpen 어트리뷰트를 가집니다. |
|  코드 에디터  | state.codeeditors.codeEditorKey |                                `string`                                |               코드 에디터의 value를 의미합니다.               |
|  노드 에디터  | state.nodeeditors.nodeEditorKey | `{ id : string; [nodeKey] : [nodeInfo]; begin: string; end: string; }` |      노드 flow의 노드 정보와 연결 정보 등이 포함됩니다.       |

---

지금까지 UI를 렌더링하기 위해 필요한 데이터 구조인 Meta와 State의 개념과 State의 대략적인 구조에 대해 알아보았습니다.

다음 장에서는 State 변경 로직이 주로 위치하게 될 Function을 다루도록 하겠습니다.

[^1]: 사용자의 상호작용에 의해 전달된 value에 인위적인 변경을 가하거나, 데이터 패칭 후 테이블 리스트를 업데이트하고자 할 때가 이 경우에 해당됩니다.
[^2]: 예를 들어 "form1", "form2" 값을 key로 가진 두 개의 폼 레이아웃이 있다고 가정해 보겠습니다. 각 폼 레이아웃에 속해 있는 하위 컴포넌트는 동일한 key 값(예: "input1")을 가질 수 있습니다. 이는 두 컴포넌트가 서로 다른 레이아웃에 속해 있기 때문입니다. 하지만 동일한 폼 레이아웃("form1") 내에서는 여러 컴포넌트가 중복된 key 값을 가질 수 없으며, 이는 에러를 발생시킬 수 있습니다.