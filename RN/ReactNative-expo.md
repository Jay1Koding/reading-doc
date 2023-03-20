### 브라우저에서 React 어플리케이션을 만들 수 있게 해주는 온라인 코드 에디터

[snack](https://snack.expo.dev/)

### React Native rules

1. 웹사이트가 아니다! Not HTML // View로 만듬
2. React Native에 있는 모든 text는 text component에 들어가야함
3. View에는 style이 있다
4. StyleSheet.create는 object를 생성함
5. status-bar는 3rd-party package다

[RN dev](https://reactnative.dev/docs/components-and-apis)

### React Native Packages?

- 기본적으로 개발자들에게 최대한 많은 APIs와 Components를 제공하려 했음
- 유지 관리와 업데이트의 어려움을 깨달음
- 서비스의 규모를 줄이고 Community packages를 써야함

### Component VS APIs

- Component는 화면에 렌더링할 항목 //
  - ex ) View
- API는 자바스크립트 코드 // 운영 체제와 소통하는 것
  - Vibration
- Expo는 RN의 일부 Component와 APIs를 복제하고 개선함
  - ex ) 기능은 거의 같지만 함수명이 다름
  - 대부분의 필요 기능들은 구현해놓음

[Expo SDK](https://docs.expo.dev/versions/latest/)

### RN Layout with Flexbox!

- View는 Flex Container
- flex의 비율은 마치 grid의 fr 같은 개념
- 부모 View에 지정 해주지 않음 비율을 맞출 수 없음

```javascript
export default function App() {
  return (
    <View style={{ flex: 1 }}>
      <View style={{ flex: 1, backgroundColor: 'tomato' }}></View>
      <View style={{ flex: 5, backgroundColor: 'teal' }}></View>
      <View style={{ flex: 1, backgroundColor: 'orange' }}></View>
    </View>
  );
}
```

### Style

- ScrollView를 쓸 때 style을 만들고 싶으면 contentContainerStyle를 사용해야함
- ScrollView에서는 flex를 줄 필요가 없음 // 스크린보다 커야하기 때문

[ScrollView](https://reactnative.dev/docs/scrollview)
[dimension](https://reactnative.dev/docs/dimensions)

### Location

[Location](https://docs.expo.dev/versions/v45.0.0/sdk/location/)

---

### Touchable

- TouchableOpacity
  View가 터치에 적절하게 반응하도록 하는 래퍼.
  아래로 누르면 래핑된 View의 opacity가 감소하여 흐리게 표시됩니다.

  [TouchableOpacity](https://docs.expo.dev/versions/v44.0.0/react-native/touchableopacity/)

- TouchableHighlight
  View가 터치에 적절하게 반응하도록 하는 래퍼.
  아래로 누르면 래핑된 View의 background를 표시합니다.

  [TouchableHighlight](https://docs.expo.dev/versions/v44.0.0/react-native/touchablehighlight/)

- TouchableWithoutFeedback
  합당한 이유가 없는 한 사용하지 마십시오.
  Press에 반응하는 모든 요소는 만졌을 때 시각적 피드백이 있어야 합니다.

  [TouchableWithoutFeedback](https://docs.expo.dev/versions/v44.0.0/react-native/touchablewithoutfeedback/)

- Pressable
  Pressable은 정의된 자식에 대한 다양한 Press 상호 작용 단계를 감지할 수 있는 핵심 구성 요소 래퍼입니다.

  [Pressable](https://docs.expo.dev/versions/v44.0.0/react-native/pressable/)

---

### TextInput

[TextInput](https://docs.expo.dev/versions/latest/react-native/textinput/#keyboardtype)

### To Dos

- React에선 state를 직접 수정하면 안되기 때문에 object assign을 사용
  - object를 다른 object와 합치고 새로운 object를 반환해줌
- [얕은 복사/ 깊은 복사](https://velog.io/@recordboy/JavaScript-%EC%96%95%EC%9D%80-%EB%B3%B5%EC%82%ACShallow-Copy%EC%99%80-%EA%B9%8A%EC%9D%80-%EB%B3%B5%EC%82%ACDeep-Copy)

```javascript
const toDos = {};
toDos[Date.now()] = {
  work: false,
};

Object.assign({}, toDos, { [Date.now()]: { work: true } });
```

### Paint To Dos

- Object.keys() // key를 보여줌

```javascript
// ES6
const newToDos = { ...toDos, [Date.now()]: { text, work: working } };
```

### Persist

[AsyncStorage](https://docs.expo.dev/versions/latest/sdk/async-storage/)
