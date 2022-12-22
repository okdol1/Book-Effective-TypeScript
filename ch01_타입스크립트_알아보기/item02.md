## 요약

- 타입스크립트 컴파일러는 언어의 핵심 요소에 영향을 미치는 몇 가지 설정을 포함하고 있습니다.
- 타입스크립트 설정은 커맨드 라인을 이용하기보다는 tsconfig.json을 사용하는 것이 좋습니다.
- 자바스크립트 프로젝트를 타입스크립트로 전환하는게 아니라면 noImplicatAny를 설정하는 것이 좋습니다.
- “undefined는 객체가 아닙니다.” 같은 런타임 오류를 방지하기 위해 strictNullChecks를 설정하는 것이 좋습니다.
- 타입스크립트에서 엄격한 체크를 하고 싶다면 strict 설정을 고려해야 합니다.

### 타입스크립트 설정

- 타입스크립트 컴파일러는 매우 많은 설정을 가지고 있다.
- 설정들은 두가지 방법으로 설정할 수 있다.
    - 커맨드 라인
        
        ```jsx
        $ tsc --noImplicitAny program.ts
        ```
        
    - tsconfig.json 설정 파일
        
        ```jsx
        {
        	"compilerOptions": {
        		"noImplicitAny": true
        	}
        }
        ```
        
        - 가급적 설정 파일을 사용하는 것이 좋습니다. 그래야만 타입스크립트를 어떻게 사용할 계획인지 동료들이나 다른 도구들이 알 수 있습니다. 설정 파일은 tsc —init만 실행하면 간단히 생성됩니다.

### noImplicitAny

- noImplicitAny는 변수들이 미리 정의된 타입을 가져야 하는지 여부를 제어합니다.
- 다음 코드는 noImplicateAny가 해제되어 있을 때에는 유효합니다.
    
    ```jsx
    function add(a, b) {
    	return a + b;
    }
    ```
    
    - 편집기에서 add 부분에 마우스를 올려 보면, 타입스크립트가 추론한 함수의 타입을 알 수 있습니다
    
    ```jsx
    function add(a: any, b: any): any
    ```
    
    - any 타입을 매개변수에 사용하면 타입 체커는 속절없이 무력해집니다.
- any는 유용하지만 매우 주의해서 사용해야 합니다.
- any를 코드에 넣지 않았지만, any 타입으로 간주되는 것을 ‘암시적 any’라고 부릅니다.
- 그런데 noImplicatAny가 설정되었다면 위 코드는 오류가 됩니다.
- 이 오류들을 명시적으로 :any 라고 선언해 주거나 더 분명한 타입을 사용하면 해결할 수 있습니다.
    
    ```jsx
    function add(a: any, b: any) {
    	return a + b;
    }
    ```
    
- 타입스크립트는 타입 정보를 가질 때 가장 효과적이기 때문에, 되도록이면 noImplicatAny를 설정해야 합니다.
- noImplicatAny의 장점
    - 타입스크립트가 문제를 발견하기 수월해진다.
    - 코드의 가독성이 좋아진다.
    - 개발자의 생산성이 향상된다.
- noImplicatAny 설정 해제는, 자바스크립트로 되어 있는 기존 프로젝트를 타입스크립트로 전환하는 상황에만 필요합니다.

### strictNullChecks

- strictNullChecks는 null과 undefined가 모든 타입에서 허용되는지 확인하는 설정입니다.
- 다음은 strictNullChecks가 해제되었을 때 유효한 코드입니다.
    
    ```jsx
    const x: number = null // 정상, null은 유효한 값입니다.
    ```
    
- 다음은 strictNullChecks를 설정하면 오류가 됩니다.
    
    ```jsx
    const x: number = null 
    // ~ 'null' 형식은 'number' 형식에 할당할 수 없습니다.
    ```
    
    - null 대신 undefined를 써도 같은 오류가 납니다.
- 만약 null을 허용하려고 한다면, 의도를 명시적으로 드러냄으로써 오류를 고칠 수 있습니다.
    
    ```jsx
    const x: number | null = null;
    ```
    
- 만약 null을 허용하지 않으려면, 이 값이 어디서부터 왔는지 찾아야 하고, null을 체크하는 코드나 단언문을 추가해야 합니다.
    
    ```jsx
    const el = document.getByElementById('status');
    el.textContent = 'Ready';
    // ~~ 개체가 'null'인 것 같습니다.
    
    if (el) {
    	el.textContent = 'Ready'; // 정상, null은 제외됩니다.
    }
    el!textContent = 'Ready'; // 정상, el이 null이 아님을 단언합니다.
    ```
    
    - strictNullChecks는 null과 undefined 관련된 오류를 잡아 내는 데 많은 도움이 되지만, 코드 작성을 어렵게 합니다. 새 프로젝트를 시작한다면 가급적 strictNullChecks를 설정하는 것이 좋지만, 타입스크립트가 처음이거나 자바스크립트 코드를 마이그레이션 하는 중이라면 설정하지 않아도 괜찮습니다.