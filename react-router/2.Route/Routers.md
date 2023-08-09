# Route

- Route
- action
- errorElement
- lazy
- loader
- shouldRevalidate

## Route

- type declaration

```javascript
interface RouteObject {
  path?: string;
  index?: boolean;
  children?: React.ReactNode;
  caseSensitive?: boolean;
  id?: string;
  loader?: LoaderFunction;
  action?: ActionFunction;
  element?: React.ReactNode | null;
  Component?: React.ComponentType | null;
  errorElement?: React.ReactNode | null;
  ErrorBoundary?: React.ComponentType | null;
  handle?: RouteObject['handle'];
  shouldRevalidate?: ShouldRevalidateFunction;
  lazy?: LazyRouteFunction<RouteObject>;
}
```

- URL segment와 component, data loading 및 data mutation을 연결함
- router 생성 함수에 전달되는 객체
- JSX 및 createRoutesFromElements로 경로를 선언할 수 있으며 element에 대한 property는 route object의 property와 동일
- RouteProvier를 사용할 때 element를 지정하고 싶지 않다면 component로 지정 가능(Component: {App}) // 대문자임
  - 하지만 `<Routes>` 안에 Component를 사용하면 **생성된 element를 여러 렌더링에서 재사용하는 React의 기능이 최적화되지 않으므로** RouteProvider 안에서만 이 작업을 해야함

```javascript
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route
      element={<Team />}
      path='teams/:teamId'
      loader={async ({ params }) => {
        return fetch(`/fake/api/teams/${params.teamId}.json`);
      }}
      action={async ({ request }) => {
        return updateFakeTeam(await request.formData());
      }}
      errorElement={<ErrorBoundary />}
    />
  )
);
```

---

## path

- URL, href, form action과 일치하는 지 확인하기 위해 URL와 일치시킬 path pattern

## Dynamic Segments

- path segment가 ":" 로 시작하면 dynamic이 됨
- path가 URL과 일치하면 URL에서 파싱되어 다른 router API에 params로 제공
- 하나의 경로에 여러 개의 동적 segment를 포함시킬 수 있음
  - 단 **부분적(partial)**일 수는 없음

```javascript
<Route path='/c/:categoryId/p/:productId' />;
// 둘다 사용 가능
params.categoryId;
params.productId;

// -----

// ❌ "/teams-:teamId"
// ✅ "/teams/:teamId"
// ❌ "/:category--:productId"
// ✅ "/:productSlug"
// 여전히 지원은 하나 구분 분석을 수행해야함
function Product() {
  const { productSlug } = useParams();
  const [category, product] = productSlug.split('--');
  // ...
}
```

## Optional Segments

- segment 끝에 "?"를 추가하여 router segment를 선택적으로 만들 수 있으며 static segment로도 가질 수 있음

```javascript
<Route
  // 이런 식으로 매칭 가능
  // - /categories
  // - /en/categories
  // - /fr/categories
  path='/:lang?/categories'
  // 로더에서 일치하는 params 사용
  loader={({ params }) => {
    console.log(params['lang']); // "en"
  }}
  // and the action
  action={({ params }) => {}}
  element={<Categories />}
/>;

// `useParams`로 element를 전달
function Categories() {
  let params = useParams();
  console.log(params.lang);
}

// ----

<Route path='/project/task?/:taskId' />;
```

## Splats

- catchall, start segment라고 함
- 경로 뒤에 "/"로 끝나면 다른 "/" 문자를 포함하여 "/"뒤에 오는 모든 문자와 일치
- 새 이름을 지정해 사용 가능, 일반적인 이름은 splat이라 함

```javascript
<Route
  // - /files
  // - /files/one
  // - /files/one/two
  // - /files/one/two/three
  path='/files/*'
  loader={({ params }) => {
    console.log(params['*']); // "one/two"
  }}
  action={({ params }) => {}}
  element={<Team />}
/>;

// 모든 element는 `useParams`를 통해서
function Team() {
  let params = useParams();
  console.log(params['*']); // "one/two"
}

let { org, '*': splat } = params;
```

## Layout Routes

- 경로를 생략하면 layout route가 됨
- UI 중첩에 사용되지만 URL에 segment를 추가 ❌
- Outlet을 이용해서 하위 경로의 element prop과 함께 `<h1>`이 렌더링 됨

- Outlet이란?
  - 하위 요소를 렌더링하려면 상위 경로 요소에서 사용해야함
  - 자식이 렌더링 될 때 중첩된 UI 표시 가능
  - 부모 경로만 일치하면 자식 index router를 렌더링하고 index route가 없으면 아무것도 렌더링 하지 않음
  - ex) 화면은 그대로이고 본문 내용만 바뀔 경우 본문 내용 content는 Outlet을 이용해서 렌더링

```javascript
<Route
  element={
    <main>
      <header>Layout</header>
      <Outlet />
    </main>
  }
/>;

// -----
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <Outlet />
    </div>
  );
}
```

## index

- 경로가 없는 하위 경로로 부모의 URL에서 부모의 Outlet에 렌더링됨
- index route인지 여부 결정
- default child route와 같이 부모의 URL에서 부모의 Outlet으로 렌더링 됨

```javascript
<Route path='/teams' element={<Teams />}>
  // ✨
  <Route index element={<TeamsIndex />} />
  <Route path=':teamId' element={<Team />} />
</Route>
```

## children

- component를 props로 전달해서 children 키워드 사용
  - 가능한 이유는 **컴포넌트도 결국엔 함수이기 때문**
- **가능은 하나 권장되지는 않고 중첩 라우팅을 하는 것을 권고**

```javascript
// main.jsx
<App children={<Test />} />

// Test.jsx
export default function Test() {
  return <div>Test</div>;
}

// App.jsx
export default function App({ children }) {
  return (
      <div>
        <h1>This is App</h1>
        {children}
      </div>
  );
}
```

---

## loader ✨✨✨✨

- 렌더링 되거 전에 호출되며 **useLoaderData()**를 통해 element의 data를 제공

```javascript
<Route
  path='/teams/:teamId'
  loader={({ params }) => {
    return fetchTeam(params.teamId);
  }}
/>;

function Team() {
  let team = useLoaderData();
  // ...
}
```

## action ✨✨✨✨

- `<Form>`, fetcher, submit을 router로 전송될 때 호출
  - request를 보낼 때 데이터를 처리하는 부분이라고 생각할 것

> 기본적으로 loader와 action은 Data APIs를 지원하는 라우트를 사용하지 않으면 기본적으로 동작 ❌
> Data APIs를 사용하는 **Component에 작성하는 것이 관습이나 새로고침 시 component만 렌더링되고 데이터는 못 가져**올 수 있어서
> 외부 파일에 따로 작성해야해 router를 구현하는 부분에 inline으로 구현하거나 **외부 파일로 따로 빼서 구조화하는 것이 일반적**
> ex src/utils or helpers/routerUtils.js 혹은 action.js과 loader.js처럼 나눠서 하기도 함
> 단 어디까지나 취향 차이, 정답 ❌

## element / Component(권장 ❌)

- 쓸 경우 형식 주의

```javascript
<Route path="/for-sale" element={<Properties />} />

<Route path="/for-sale" Component={Properties} />
```

## errorElement / ErrorBoundary

- 렌더링 중 loader나 action에서 예외가 발생할 경우 해당 route의 컴포넌트 대신 렌더링함
- React element를 직접 생성하려면 errorElement, 그렇지 않으면 react router가 ErrorBoundary를 이용해 직접 생성함
- Data APIs를 지원하는 라우트를 사용하지 않으면 기본적으로 동작 ❌

```javascript
<Route
  path="/for-sale"
  element={<Properties />}
  loader={PropLoader}
  action={PropAction}
  errorElement={<ErrorBoundary />}
/>

<Route
  path="/for-sale"
  Component={Properties}
  loader={PropLoader}
  action={PropAction}
  ErrorBoundary={ErrorBoundary}
/>
```

## handle

- ex) [useMatches](https://reactrouter.com/en/main/hooks/use-matches)

## lazy

- 애플리케이션 번들을 작게 유지하고 경로의 코드 분할을 지원하기 위해 각 route는 route 정의에서 route와 일치하지 않는 부분(loader, action, Component/element, ErrorBoundary/errorElement 등)을 해결하는 async 함수를 제공
- Data APIs를 지원하는 라우트를 사용하지 않으면 기본적으로 동작 ❌
- 각 lazy 함수는 일반적으로 dynamic import 결과를 return
- 에러가 생긴다면 `<Suspense>`로 감싸서 fallback prop을 전달해줄 것
- dynamic import를 사용해서 lazy loading하는 react의 lazy와 매우 비슷함

```javascript
// react route v5.x.x
const LazyAbout = React.lazy(() => import('./About'));
```

```javascript
let routes = createRoutesFromElements(
  <Route path='/' element={<Layout />}>
    <Route path='a' lazy={() => import('./a')} />
    <Route path='b' lazy={() => import('./b')} />
  </Route>
);
```

```javascript
// lazy router module에서 route에 대해 정의할 속성을 export함
export async function loader({ request }) {
  let data = await fetchData(request);
  return json(data);
}

export function Component() {
  let data = useLoaderData();

  return (
    <>
      <h1>Component</h1>
      <p>{data}</p>
    </>
  );
}
```
