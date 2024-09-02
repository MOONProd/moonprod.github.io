---
layout: post
title:  "React에서 map 사용하기"
date:   2024-08-14 22:15:23 +0900
categories: React
---

안녕하세요 한창 *React* 공부를 하고 있는 문소연입니다.

한 Component에서 List 형식으로 되어있는 data들을 렌더링하기 위해서는 **map**이 정말 자주 쓰입니다.

`python`에서의 **map**과 `java`에서의 **Map**의 쓰임이 다른 것처럼 *React*에서의 **map**도 그 역할이 다릅니다 ! 이는 마치 `forEach`의 역할과 비슷하다고 할 수 있습니다.

간단하게 이들의 차이점에 대해 짚고 *React*에서의 **map** 사용법에 대해 설명해보겠습니다.

---
### python에서의 map
`python`에서의 **map**은 반복 가능한 객체의 각 요소에 설정한 함수를 적용하여 새로운 반복 가능한 객체를 반환합니다.

```python
numbers = [1, 2, 3, 4, 5]
result = map(lambda x: x * 2, numbers)
print(list(result))  
# 출력: [2, 4, 6, 8, 10]
```
이처럼 `numbers`라는 배열의 요소들을 *2 해주는 함수를 적용하여 그 배열을 반환하여줍니다.


### java에서의 Map
`java`에서의 **Map**은 인터페이스로 키-값 쌍을 저장하는 자료구조로, 각 키는 고유값이며, 빠른 데이터 검색을 위해 사용됩니다.

**Map**이라고 따로 대문자로 명시하는 이유도 인터페이스이기 때문입니다.

가장 자주 쓰이는 `HashMap`을 예로 들면 이러합니다.
```java
Map<String, Integer> map = new HashMap<>();
map.put("one", 1);
map.put("two", 2);
System.out.println(map.get("one"));  
// 출력: 1
```
`String`값으로 key를 두고 `Integer`값을 저장합니다.
그리고 `Integer`값에 접근하기 위해 `map.get()`을 사용하여 키 값으로 이를 읽어옵니다.

### React에서의 map
`javascript` 배열 메서드(+Array.prototype.map())로, 배열의 각 요소를 변환하여 *새로운 배열*을 반환하며, 주로 컴포넌트 리스트를 렌더링할 때 사용됩니다.

```javascript
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) => 
  <li key={number.toString()}>{number}</li>
);

return <ul>{listItems}</ul>;
```
`return` 내에 바로 **map**을 정의해도 되나, 예시와 같이 사용하는 것이 추후 수정사항이 있을경우 더 용이할 수 있습니다.
예시처럼 **map** 사용시에 key 값을 속성으로 지정해주어야 안정성을 확보할 수 있습니다.

+**Array.prototype.map()** 는 앞서 말했듯 `javascript`의 배열 메서드입니다. 이 메서드는 배열의 각 요소에 대해 주어진 콜백 함수를 호출하고, 그 결과를 새로운 배열로 반환해주는 역할을 합니다.

---
### React에서의 map 사용
앞서, `forEach`와 비슷하다고 언급하였습니다. 두 개념은 각 요소를 반복하며 콜백 함수를 실행한다는 점이 닮았지만, 가장 큰 차이점이 있습니다.

`map` 함수는 **새로운 배열**을 반환합니다.  
콜백 함수를 실행한 결과를 모아 **새로운 배열**을 반환하는 반면,

`forEach` 함수는 따로 반환값이 없습니다.  
단순히 콜백 함수를 실행하여 각 요소를 출력해줄 뿐, 새 배열을 만들지 않습니다.

이와 같은 차이로 `forEach`는 단순 effect, `console.log`시에 유용하게 쓰인다고 생각하면 됩니다.


React Component를 만들다 보면 `map`을 사용할 일이 정말 많습니다. 간단한 예를 들어보자면  
*블로그 포스트 리스트*, *도서 목록 리스트* 등   
그냥 웬만한 리스트 요소를 가진 콘텐츠를 보여주기 위해서는 `map`을 사용합니다.

출석부를 만들고자 할 때, 학생들의 번호를 key 값으로 이름을 렌더해주는 Component를 형성하고자 합니다. 이 때 `map`의 쓰임을 예시로 살펴보겠습니다.

```javascript
const [students, setStudents] = useState([
    { no: '1', name: 'Do' },
    { no: '2', name: 'Baek' },
    { no: '3', name: 'June' },
]);

return (
    <div>
        <ul>
            {students.map((student) => {
                return (
                    <li key={student.no}>
                        {student.name}
                    </li>
                );
            })}
        </ul>
    </div>
);

```
이처럼 직접 `return`내에 `map` 함수를 사용할 수도 있습니다.
저 같은 경우, 아직 작은 규모의 실습만 진행 중이라 대부분 이 예시와 같이 작성하곤 했습니다.

>>다만, 주의해야할 점이 있습니다 ❗️  
여기서 `map`함수를 사용하기 위해서는 오직 **배열**이어야만 합니다.  
>>만약 여기서 `students`를 객체로 정의했다면, `map`함수를 호출할 수 없습니다 ❗️  
>> **배열**의 형태로 정보들이 담겨져있을 때 이를, **새로운 배열**로 렌더하기 위해 사용하는 것이 `map` 메서드 입니다.


그리고 `key`속성을 부여하지 않을 시에는 개발자 도구에서 경고문이 출력되기도 합니다. 안정성을 위해 배열에서 지정된 id 값이 없다면 되도록이면 index를 이용하여 `key`속성을 지정해주는 것이 좋습니다.

```javascript
// 만약 students 배열에 no 값이 없다면
{students.map((student,index)=>(
    <li key={index}>
        {student.name}
    </li>
))}
```
이렇게 지정해주시면 됩니다. 하지만 배열의 삭제/수정이 이루어질 경우, `index`는 좋은 `key`값이 될 수 없습니다.
배열의 상태가 고정되어있고, 변경되지 않는 경우에만 사용하시길 바랍니다.

### 맺음말
no를 사용한 예시와 no 값이 없을 때 예시의 차이를 발견했다면 아시겠지만 !!   
콜백함수를 사용시에 return값이 한 개의 값일 때는 `{return();}`을 생략할 수 있습니다.
