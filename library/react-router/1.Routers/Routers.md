# Routers

### 1. [Data APIS](https://reactrouter.com/en/main/routers/picking-a-router#data-apis)

- Data API를 지원하는 새로운 라우터가 도입
  - [createBrowserRouter](#createbrowserrouter)
    - [routes](#1-routes)
    - [basename](#2-basename)
    - [future](#3-future-optional)
    - [window](#4-window)
  - [createHashRouter](#createhashrouter)
  - [createMemoryRouter](#creatememoryrouter)
  - [createStaticRouter](#createstaticrouter)
- 아래의 라우터는 더이상 지원하지 않음
  - `<BrowserRouter>`
  - `<MemoryRouter>`
  - `<HashRouter>`
  - `<NativeRouter>`
  - `<StaticRouter>`

### 2. Web Projects

- 모든 프로젝트에서 createBrowserRouter을 사용하는 것을 권고
- 전체 URL은 SEO에 더 좋고 서버 렌더링에 더 좋으며 나머지 웹 플랫폼과 호환이 더 잘됨
- 정적 파일 서버에서 앱을 호스팅하는 경우 404가 발생하지 않도록 모든 요청을 index.html로 보내도록 구성해야함
- 전체 URL을 사용할 수 없는 경우 createHashRouter가 차선책

### 3. Testing

- 앱에서 사용하는 라우터 대신 DOM history API를 필요로 하는 createMemoryRouter 또는 <MemoryRouter>를 사용하면 가장 쉬움

```javascript
module.exports = {
  setupFiles: ['whatwg-fetch'],
  // ...rest of the config
};
```

### 4. RN

- React Native 프로젝트에서 `<NativeRouter>`를 사용될 예정
- 곧 지원 예정

---

## createBrowserRouter

- 모든 React 라우터 웹 프로젝트에 권장되는 라우터
- DOM history API를 사용해 URL을 업데이트하고 히스토리 스택을 관리
- loader, action, fetcher 등과 같은 v6.4 데이터 API를 활성화

> 데이터 API 설계에서 불러오기와 렌더링이 분리되어 있기 때문에 정적으로 정의된 경로 집합을 사용하여 React 트리 외부에 라우터를 생성해야 함.
> [When To Fetch: Remixing React Router - Ryan Florence](https://www.youtube.com/watch?v=95B8mnhzoCM)

```javascript
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

import Root, { rootLoader } from './routes/root';
import Team, { teamLoader } from './routes/team';

const router = createBrowserRouter([
  {
    path: '/',
    element: <Root />,
    loader: rootLoader,
    children: [
      {
        path: 'team',
        element: <Team />,
        loader: teamLoader,
      },
    ],
  },
]);

ReactDOM.createRoot(document.getElementById('root')).render(
  <RouterProvider router={router} />
);
```

> 1. 커뮤니티에선 이전 버전과 비슷한 JSX 중첩 방식을 더 선호하는 편 ✨
> 2. **loader와 action은 컴포넌트 내부에 작성하는 것이 관례**이나
>    컴포넌트 내부에서 작성하면 새로고침 시 해당 컴포넌트만 렌더링되며 로직이 다시 실행되지 않을 수 있습니다. 예를 들어, 컴포넌트의 상태 초기화나 데이터 로딩 로직이 componentDidMount 메서드에 작성되어 있다면, **새로고침 시 해당 메서드가 실행되지 않아 초기 상태나 데이터가 로드되지 않을 수** 있어 외부에 작성해 유지보수와 구조화를 꾀할 것
> 3. [RouterProvider](#routerprovider)

- type declaration

```javascript
function createBrowserRouter(
  routes: RouteObject[],
  opts?: {
    basename?: string;
    future?: FutureConfig;
    hydrationData?: HydrationState;
    window?: Window;
  }
): RemixRouter;
```

### 1. routes

- children 속성에 중첩된 경로가 있는 Route 객체의 배열

### 2. basename

- 도메인의 루트에 배포할 수 없고 하위 디렉토리에 배포할 수 없는 경우 앱의 기본 이름
- **단, vite 같은 환경에선 config를 따로 설정해 개발해야함**

```javascript
createBrowserRouter(routes, {
  basename: `${basename}`,
});
```

- root에 연결할 때 trailing slash(후행 슬래쉬)가 존중됨

```javascript
createBrowserRouter(routes, {
  basename: '/app',
});
<Link to='/' />; // results in <a href="/app" />

// ✨
createBrowserRouter(routes, {
  basename: '/app/',
});
<Link to='/' />; // results in <a href="/app/" />
```

### 3. future (optional)

- 다음 버전으로 마이그레이션 할 수 있도록 함

### 4. window

- 브라우저 개발 도구 플러그인이나 글로벌 창이 아닌 다른 창을 사용하는 테스트와 같은 환경에 유용

---

## createHashRouter

- 모든 트래픽을 리액트 라우터 애플리케이션으로 리디렉션하도록 웹 서버를 구성할 수 없는 겨우 유용
- 일반 URL을 사용하는 대신 URL의 해쉬(#)를 사용해 URL을 관리
- 기능적으로는 createBrowserRouter와 동일

---

## createMemoryRouter

- 브라우저의 히스토리를 사용하는 대신 메모리에서 자체 히스토리 스택을 관리
- 주로 storybook, test 및 컴포넌트 개발 도구에 유용하지만 브라우저가 아닌 환경에서도 실행하는데 사용할 수 있음
- [createMemoryRouter](https://reactrouter.com/en/main/routers/create-memory-router)

---

## createStaticRouter

- 서버(ex: Node 또는 자바스크립트 런타임)에서 렌더링에 데이터를 라우터를 활용하려는 경우 사용
- [SSR](https://reactrouter.com/en/main/guides/ssr)
- [createStaticHandler](https://reactrouter.com/en/main/routers/create-static-handler), [`<StaticRouterProvider>`](https://reactrouter.com/en/main/routers/static-router-provider)

```javascript
export async function renderHtml(req) {
  let { query, dataRoutes } = createStaticHandler(routes);
  let fetchRequest = createFetchRequest(req);
  let context = await query(fetchRequest);

  // 리디렉션 응답을 받으면 short circuit하고 Express 서버가
  // 직접 처리하도록 함
  if (context instanceof Response) {
    throw context;
  }

  let router = createStaticRouter(dataRoutes, context);
  return ReactDOMServer.renderToString(
    <React.StrictMode>
      <StaticRouterProvider router={router} context={context} />
    </React.StrictMode>
  );
}
```

- type declaration

```javascript
declare function createStaticRouter(
  routes: RouteObject[],
  context: StaticHandlerContext
): Router;
```

---

## `<RouterProvider>`

> 일반적으로 Provider란 컴포넌트 트리 상에서 데이터를 공유하기 위해 사용되는 컴포넌트이며 Context API를 기반으로 동작함
> Provider는 Umbrella Component라고도 함
> 변수 router를 선언해 createBrowserRouter 안에 route를 배열로 묶어 할당하고 Provider 로 넣어주는 것을 권고

```javascript
// 생략
ReactDOM.createRoot(document.getElementById('root')).render(
  // 여기임
  <RouterProvider router={router} />
);
```

## fallbackElement

- 앱을 서버에 렌더링하지 않는 경우 createBrowserRouter는 마운트할 때 일치하는 모든 경로의 로더를 시작함
- 이 시간동안 사용자에게 앱이 동작 중이라는 표시를 제공하기 위해 제공
- 정적 호스팅 TTFB(Time To First Byte)을 활용해볼 것
  - [참고1](https://equus3144.medium.com/ttfb%EB%A1%9C-%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%84%B1%EB%8A%A5-%EC%B8%A1%EC%A0%95%ED%95%98%EA%B8%B0-21baef090c7d)
  - [참고2](https://darrengwon.tistory.com/938)

```javascript
<RouterProvider router={router} fallbackElement={<SpinnerOfDoom />} />
```
