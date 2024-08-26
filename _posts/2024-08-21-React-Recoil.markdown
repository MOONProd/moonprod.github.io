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

코드 예시는 제가 실습으로 진행한 간단한 계산기를 기반으로 작성하겠습니다.

```javascript
function App(props) {
    return(
        <RecoilRoot>
            <ChildComponent/>
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

이렇게 네가지로 정리할 수 있습니다.