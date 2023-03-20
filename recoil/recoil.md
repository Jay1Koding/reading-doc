# 동기

- 단순하게 외부 글로벌 상태관리 라이브러리보단 내장 기능이 좋으나 리액트의 한계가 있음
  - 컴포넌트 상태는 공통된 상위요소까지 끌어올려야 공유 가능하며 이 과정에서 거대한 트리가 다시 렌더링되는 효과를 야기
  - context는 단일값만 저장할 수 있으며 자체 소비자(consumer)를 가지는 여러 값들의 집합을 가질 수 없다
  - 이 두가지 특성이 최상단(state가 존재하는 곳)부터 트리의 말단(state가 사용되는 곳)까지의 코드 분할이 어려움

Recoil은 직교(orthogonal)하지만 본질적인 방향 그래프를 정의하고 React 트리에 붙인다. 상태 변화는 이 그래프의 뿌리(atoms)로부터 순수함수(selectors)를 거쳐 컴포넌트로 흐르며, 다음과 같은 접근 방식을 따른다.

- 우리는 공유상태(shared state)도 React의 내부상태(local state)처럼 간단한 get/set 인터페이스로 사용할 수 있도록 boilerplate-free API를 제공한다. (필요한 경우 reducers 등으로 캡슐화할 수도 있다.)
- 우리는 동시성 모드(Concurrent Mode)를 비롯한 다른 새로운 React의 기능들과의 호환 가능성도 갖는다.
- 상태 정의는 점진적이고(incremental) 분산되어 있기 때문에, 코드 분할이 가능하다.
- 상태를 사용하는 컴포넌트를 수정하지 않고도 상태를 파생된 데이터로 대체할 수 있다.
- 파생된 데이터를 사용하는 컴포넌트를 수정하지 않고도 파생된 데이터는 동기식과 비동기식 간에 이동할 수 있다.
- 우리는 탐색을 일급 개념으로 취급할 수 있고 심지어 링크에서 상태 전환을 인코딩할 수도 있다.
- 전체 애플리케이션 상태를 하위 호환되는 방식으로 유지하기가 쉬우므로, 유지된 상태는 애플리케이션 변경에도 살아남을 수 있다.

---

# 주요 개념

## 개요

- Recoil을 사용하면 atoms (공유 상태)에서 selectors (순수 함수)를 거쳐 React 컴포넌트로 내려가는 data-flow graph를 만들 수 있다. Atoms는 컴포넌트가 구독할 수 있는 상태의 단위다. Selectors는 atoms 상태값을 동기 또는 비동기 방식을 통해 변환한다.

## Atoms

- Atoms는 상태의 단위이며, 업데이트와 구독이 가능
- atom이 업데이트되면 각각의 구독된 컴포넌트는 새로운 값을 반영하여 다시 렌더링
- atoms는 런타임에서 생성될 수도 있다
- Atoms는 React의 로컬 컴포넌트의 상태 대신 사용할 수 있다
- 동일한 atom이 여러 컴포넌트에서 사용되는 경우 모든 컴포넌트는 상태를 공유한다
- atom()으로 생성

```javascript
const fontSizeState = atom({
  key: 'fontSizeState',
  default: 14,
});
```

- 디버깅, 지속성 및 모든 atoms의 map을 볼 수 있는 특정 고급 API에 사용되는 고유한 키가 필요
- 두개의 atom이 같은 키를 갖는 것은 오류이기 때문에 키값은 전역적으로 고유하도록 해야한다.
- React 컴포넌트의 상태처럼 기본값도 가진다
- 컴포넌트에서 atom을 읽고 쓰려면 useRecoilState라는 훅을 사용한다.
  - React의 useState와 비슷하지만 상태가 컴포넌트 간에 공유될 수 있다는 차이가 있다.

## Selector

- Selector는 atoms나 다른 selectors를 입력으로 받아들이는 순수 함수(pure function)다.
- 상위의 atoms 또는 selectors가 업데이트되면 하위의 selector 함수도 다시 실행된다.
- 컴포넌트들은 selectors를 atoms처럼 구독할 수 있으며 selectors가 변경되면 컴포넌트들도 다시 렌더링된다.
- Selectors는 상태를 기반으로 하는 파생 데이터를 계산하는 데 사용된다.
- 최소한의 상태 집합만 atoms에 저장하고 다른 모든 파생되는 데이터는 selectors에 명시한 함수를 통해 효율적으로 계산함으로써 쓸모없는 상태의 보존을 방지한다.
  - Selectors는 어떤 컴포넌트가 자신을 필요로하는지, 또 자신은 어떤 상태에 의존하는지를 추적하기 때문에 이러한 함수적인 접근방식을 매우 효율적으로 만든다.
- 컴포넌트의 관점에서 보면 selectors와 atoms는 동일한 인터페이스를 가지므로 서로 대체할 수 있다.
- Selectors는 selector()를 사용해 정의한다.

- get 속성은 계산될 함수
- 전달되는 get 인자를 통해 atoms와 다른 selectors에 접근할 수 있다.
- 다른 atoms나 selectors에 접근하면 자동으로 종속 관계가 생성되므로, 참조했던 다른 atoms나 selectors가 업데이트되면 이 함수도 다시 실행된다.

---

# 설치

`npm install recoil`

## Bundler

- Recoil은 Webpack 또는 Rollup과 같은 모듈 번들러와도 문제없이 호환된다.

## ES5 지원

- Recoil 빌드는 ES5로 트랜스파일 되지 않으므로, Recoil을 ES5와 사용하는 것은 지원하지 않는다.
- ES6 기능을 natively하게 제공하지 않는 브라우저를 지원해야 하는 경우 Babel로 코드를 컴파일하고 preset @babel/preset-env을 이용하여 이를 수행할 수는 있지만 문제가 발생할 수도 있다.
  - 특히, React와 같이, Recoil은 ES6의 Map과 Set 타입에 의존하는데, 이러한 ES6의 요소들을 polyfills를 통해 에뮬레이션하는 것은 성능상의 문제를 야기할 수 있다.

## ESLint

- 'useRecoilCallback'을 additionalHooks 목록에 추가하는 것이 좋다.
  - 이를 추가하면, ESLint는 useRecoilCallback()을 사용하기 위해 전달된 종속성이 잘못 지정되었을 때 경고를 표시하고 해결 방안을 제시한다. additionalHooks의 형식은 정규식(regex) 문자열이다.

```json
// 수정된 .eslint 설정
{
  "plugins": ["react-hooks"],
  "rules": {
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": [
      "warn",
      {
        "additionalHooks": "useRecoilCallback"
      }
    ]
  }
}
```

---

# Recoil 시작하기

## RecoilRoot

- recoil 상태를 사용하는 컴포넌트는 부모 트리 어딘가에 나타나는 RecoilRoot 가 필요하다. 루트 컴포넌트가 RecoilRoot를 넣기에 가장 좋은 장소다.

```javascript
import React from 'react';
import {
  RecoilRoot,
  atom,
  selector,
  useRecoilState,
  useRecoilValue,
} from 'recoil';

function App() {
  return (
    <RecoilRoot>
      <CharacterCounter />
    </RecoilRoot>
  );
}
```

[여기서부터](https://recoiljs.org/ko/docs/basic-tutorial/intro)
