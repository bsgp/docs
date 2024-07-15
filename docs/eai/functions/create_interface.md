# Create Interface

## Interface 생성하기

"Interface"는 "Flow"와 1:N의 관계에 있습니다. Flow와의 관계가 종속적이기 때문에 Flow 페이지 내에서만 Interface를 생성할 수 있습니다.

1. "Flow List" 페이지에서 Interface를 생성하고자 하는 Flow를 선택합니다.
   ![Image](assets/eai_flow_list.png)
2. "Flow" 페이지 하단 테이블 탭에서 Interface❶를 선택합니다.
   ![Image](assets/eai_if_create.png)
3. "Add"❷ 버튼을 클릭합니다.
4. "Interface" 정보를 입력합니다.
   
    |UI  |<p style="text-align: center;">Description</p>|
    |:---:|------------|
    |![Image](assets/eai_if_info.png)  |  <ul style="font-size: .8rem;"><li>"ID"는 사용자가 Infterface를 식별하기 위한 유니크한 값입니다.</li><li>"Type"은 "Flow-Step"에서 어떤 기능을 사용할지 명시적으로 보여주기 위한 값입니다.</li><li>"Name"은 Type이 RFC인 경우 "Function.name"의 역할을 DB인 경우에는 "TableName"의 역할을 합니다.</li><li>"TriggeredBy"는 Interface를 호출하는 SystemID 값입니다.</li><li>"Path"는 Endpoint의 Path 값입니다. 배포된 Endpoint 목록중 선택할 수 있습니다.</li><li>"Method"는 Endpoint의 Method 값입니다. Path 선택시 자동으로 입력됩니다.</li></ul>|  
    
5. "확인" 버튼을 클릭합니다.