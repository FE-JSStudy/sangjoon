# [JavaScript30] CSS + JS CLOCK

바닐라 자바스크립트를 처음으로 새로 공부하는 의미에서, 작은 시계 기능을 구현하는 코드를 작성했습니다.

## 📍 적용하고 공부한 개념

### Array.prototype.filter

arr.filter(callback(element[, index[, array]]),[,thisArg]) <br>
: filter 메서드는 주어진 함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열을 반환합니다.

```javascript
const people = [
  { first: "Albert", last: "Einstein", year: 1879, passed: 1955 },
  { first: "Isaac", last: "Newton", year: 1643, passed: 1727 },
  { year: 1564, passed: 1642 },
  { first: "Marie", last: "Curie", year: 1867, passed: 1934 },
  { first: "Johannes", last: "Kepler", year: 1571, passed: 1630 },
];

//  함수의 테스트를 통과하는 모든 요소를 모아 새로운 배열을 반환합니다.
const filter1 = people.filter(
  (person) => person.year >= 1500 && person.year < 2000
);

// 어떤 요소도 테스트를 통과하지 못했으면 빈 배열을 반환합니다.
const filter2 = people.filter((person) => person.year >= 3000);
console.table(filter1);
console.table(filter2);
```

|                         결과                          |
| :---------------------------------------------------: |
| <img src="./images/1.png" alt="filter" width="600" /> |

### Array.prototype.map

arr.map(callback(currentValue[, index[, array]])[, thisArg])<br>
: 메서드는 배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

map을 호출한 배열의 중간이 비어있는 경우, 결과 배열 또한 동일한 인덱스를 빈 값으로 유지합니다.

```javascript
const people = [
  { first: "Albert", last: "Einstein", year: 1879, passed: 1955 },
  { first: "Isaac", last: "Newton", year: 1643, passed: 1727 },
  { year: 1564, passed: 1642 },
  { first: "Marie", last: "Curie", year: 1867, passed: 1934 },
  { first: "Johannes", last: "Kepler", year: 1571, passed: 1630 },
];

const fullNames = people.map((person) => `${person.first} ${person.last}`);
console.table(fullNames);
```

|                        결과                        |
| :------------------------------------------------: |
| <img src="./images/2.png" alt="map" width="600" /> |

## 📍 발생 문제

초침의 css에 transition를 설정하니, 초침이 59 -> 0으로 바뀌면서 뒤로 돌아가는 문제가 있었습니다. 이에 매초 초침의 값을 재할당하는 것이 아니라, 처음에만 할당하고 계속 값을 누적하는 방식으로 해결했습니다.

```css
.hand {
  width: 50%;
  height: 6px;
  background: white;
  position: absolute;
  top: 50%;
  border-radius: 10px;
  transform-origin: 100%;
  transition: all 0.05s;
  transition-timing-function: csubic-bezier(0.28, 0.79, 0.36, 1.18);
}
```

수정 전

```javascript
function getDate() {
  // 초침
  const now = new Date();
  let seconds = now.getSeconds();
  const secondDeg = (seconds / 60) * 360 + 90;
  const secondHand = document.querySelector(".second-hand");
  secondHand.style.transform = `rotate(${secondDeg}deg)`;
}

setInterval(getDate, 1000);
```

수정 후

```javascript
const now = new Date();
let seconds = now.getSeconds();
// 초를 매초마다 재할당
// 변수를 계속 수정하므로, const로 변수할당 시 error가 발생합니다.

function getDate() {
  // 초침
  seconds++;
  const secondDeg = (seconds / 60) * 360 + 90;
  const secondHand = document.querySelector(".second-hand");
  secondHand.style.transform = `rotate(${secondDeg}deg)`;
}

setInterval(getDate, 1000);
```

## 📍 결과물

|                             BEFORE                             |                             AFTER                              |
| :------------------------------------------------------------: | :------------------------------------------------------------: |
| <img src="./images/1.gif" alt="change_password" width="600" /> | <img src="./images/2.gif" alt="change_password" width="600" /> |

## 📍 느낌점

React 프레임워크를 활용하여 프로젝트를 할때, JavaScipt를 짧게나마 공부했으나 이후 부족함을 많이 느끼게 되어 스터디를 시작하게 되었습니다. 이렇게 작은 다양한 기능들을 구현하면서, 자바스크립트에 대한 이론/개념적인 부분에 대해 더 입체적으로 이해하고, 이후 프론트엔드 개발자로 성장하기 위한 기반을 만들어 나가겠습니다.

> 출처: https://javascript30.com/

> 참고: https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Grammar_and_Types
