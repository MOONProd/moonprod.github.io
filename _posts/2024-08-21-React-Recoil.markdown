---
layout: post
title:  "React에서 Recoil 시작하기"
date:   2024-08-20 20:32:09 +0900
categories: start
---

안녕하세요 한창 *React* 공부를 하고 있는 문소연입니다.

현재 React의 기본적인 Hook `useState` `useEffect` 에 대한 사용은 어느정도 익힌 상태입니다.

이에 이어서 `Context`에 대해 학습하다 __*Recoil*__ 의 개념을 같이 접하게 되었습니다.

`Context`를 배우면서도 `props`가 더 유용하게, 더 많이 쓰일 듯 하였습니다. 그래도 프로젝트의 복잡성을 극복하여 구현하기 위해 `Context`가 유연하게 쓰일 수 있는 상황도 생각해봤습니다.

하지만, __*Recoil*__ 개념을 습득한 순간 '보다 편리하다' 라는 생각을 지우지 못했습니다.

이후로 이를 익히기 위해 컴포넌트를 생성하며 작은 실습을 진행하였습니다. __*Recoil*__ 을 처음 익히며 쉽게 할 수있는 실수들과 기본 개념들에 대해 정리해보겠습니다.

---

### Recoil이란?
React를 위한 상태관리 라이브러리로 간단한 설치를 통해 react app에서 사용할 수 있습니다.

저는 npm을 이용하여 설치하였습니다.
```
npm install recoil
```

### Recoil 시작
import로 시작해보겠습니다. Recoil을 사용하기 위해서는 다음과 같은 정의가 필요합니다.
```javascript
import { RecoilRoot, atom, useRecoilState, useRecoilValue } from 'recoil';
```
생각보다 많은게 필요한가요? 하지만 복잡하게 생각할게 하나도 없습니다. 

먼저 **RecoilRoot**는 *최상위 부모 컴포넌트에서 감싸주는 태그*로 사용하면 됩니다.
해당 컴포넌트에서 recoil을 사용하겠다는 의미와 같죠.

코드 예시는 제가 실습으로 진행한 간단한 계산기를 기반으로 작성하겠습니다. (ClassCalc()는 App.js의 역할)

```javascript
function ClassCalc(props) {
    return (
        <RecoilRoot>
            <CalcHeader/>
            <CalcBody/>
            <CalcResult/>
        </RecoilRoot>
    );
}
```
이제 recoil을 사용할 수 있습니다!

### Recoil 정의
이제 **atom**을 통해 프로젝트에서 전역변수로 사용하고자 하는 값을 정의해줍니다.

예를 들어, 간단한 계산기를 구현하기 위해서 필요한 atom 값은,  
계산에 사용되는 데이터들입니다.
1. *숫자1*
2. *숫자2* 
3. *연산자*
4. *결과*
5. *boolean 토글*    
---
atom을 정의하는 방법은 다음과 같습니다.
```javascript
const info = atom({
    key:"info",
    default:{
        firstNumber:"",
        secondNumber:"",
        operator:"+",
        result:"",
        toggle:false,
    },
});
```
info는 변수명으로 recoil을 사용할 시 쓰이는 변수라고 생각하시면 됩니다.

atom에 정의된 값은 전역변수로 쓰이기 때문에 직관적인 이해를 돕고자 코드의 import 선언 다음으로 위치시켜주었습니다.

하지만 대게 *atom.js*나 *store.js*와 같은 이름으로 atom 값들을 따로 정의해주는 파일을 형성하여 접근한다고 합니다. 

### Recoil 사용
변수가 필요한 컴포넌트에서 정의된 atom값을 사용할 차례입니다.
이 때 **useRecoilState** 와 **useRecoilValue**를 사용합니다.

```javascript
const [refer,setRefer] = useRecoilState(info);
```

**useRecoilState**는 `useState`와 같은 형식으로 사용합니다. 해당 컴포넌트내에서 atom내의 값 set이 필요하다면 **useRecoilState**로 정의하여 `refer.data`와 같은 형식으로 접근하여 관리합니다.  

```javascript
    function handleOnClick() {
        setRefer({...refer,toggle:true});
        setInput(refer.firstNumber+refer.operator+refer.secondNumber);
    }
```
'계산' 버튼을 클릭했을 때 실행되는 함수 `handleOnClick`입니다.
`setRefer`을 통해 atom 값에 접근하여 스프레드 함수를 통해 이전 atom 값을 불러와준 후 `toggle`상태를 *true*로 변경해줍니다.

atom 값 자체가 객체이기 때문에 `setRefer({})`과 같은 형식으로 접근합니다.

>❗️이때, 주의할 점이 있습니다❗️\
개인적으로 atom 활용이 익숙치 않아 스프레드 함수로 불러올 이전값에 대해 `...info`로 명시하여 제대로 작동하지 않았습니다.\
어떻게 보면 당연하지만 쉽게 혼동될 수 있으므로 주의해주세요‼️

  `setInput`은 계산에 [`eval`함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/eval)를 사용하기 위해 정의한 State입니다.
  `eval`함수는 사용하지 않는 것이 좋습니다만, 정말 간단한 사칙연산 구현을 위해 사용하였습니다.

  `setInput`을 보면 알 수 있지만, 앞서 말했듯이 atom의 값에 접근하기 위해서는 `refer.data`의 형식을 사용합니다.

  <br>

  ```javascript
  function CalcResult() {
    const val = useRecoilValue(info);
    console.log(val);
    

    return(
        <div>
            {val.toggle && `${val.firstNumber} ${val.operator} ${val.secondNumber} = ${val.result}`}
        </div>
    );
}
```
마지막 결과를 출력해주는 컴포넌트입니다.
여기서는 State관리가 필요없이 atom의 값을 이용만 하기에 **useRecoilValue**을 사용하여 보았습니다.

코드와 같이 변수명과 `useRecoilValue(atom변수명)`으로 정의해주면 변수명을 통해 바로 접근이 가능합니다.

### 마무리
정말 간단한 `Recoil` 사용법이라 **selector**부분은 사용되지 않았습니다. `selector` 또한 recoil의 기본에 속하기 때문에 알아두고 사용하면 좋을 듯 합니다.
