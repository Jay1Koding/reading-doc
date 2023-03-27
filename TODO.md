# Doing :)

- [ ] sweet-alert으로 popup ➡️

  - [ ] 컴포넌트화 ➡️
  - [ ] 네이밍 수정
  - [ ] font, button, color 수정 ➡️

- [ ] product list, detail, my product

---

# TODO

- Sign Form Valid, Auth 다시 확인할 것

- [ ] 뒷단 project 받아서 확인 `<port>`까지

  - [ ] Quick Motion Studio 확인할 것

- [ ] drobo-fs(NAS) - synology
- [ ] OneNote?
- [ ] id#profileimg 위치 찾아서 width height 고정적으로 줘야할 듯
- [ ] User Profile / Edit
  - [ ] 파일을 업로드 했을 때 그 prop을 받아 img src에 들어가게

---

# Checked

- [x] `input[type='password']` fontSize가 작게 나온 이유는 line-height 이빠이 주고 size를 작게 줘서임
- [x] Sign `onSubmit()`는 인자로 그냥 Form 데이터 그 자체를 받음 :)
- [x] Social login (OAuth) 할 지 ?
  - 나중에
- [x] email input button(중복 체크) 구현 :)

- Sign Form Valid, Auth
  - [x] submit할 때 `au-code = ROLE_USER`로 보낼 것
  - [x] submit할 때 isValid()로 체크할 것
  - [x] ROLE_ADMIN
- [x] 다우오피스, jira/confluence
- [x] 구글 드라이브/ ORelly 계정
- [x] email regex 체크해서 다시 수정 ->
  - ✅ us_id는 DB에서 indexing 할 거임
  - [x] input의 입력이 끝난 후 비동기로 valid를 처리한 뒤에 button을 클릭할 수 있게 해줘야함
- [x] input onChange 이벤트 발생시 버튼을 클릭한 이후 지속적으로 렌더링이 되고 state가 공유가 안됨
  - [x] 각종 validation을 뚫어버려서 github 사이트를 참고한 결과 default로 버튼을 disable하고 input 이벤트 발생 뒤 약 1~2초 뒤에
        해당 input을 valid하고 button을 enable하게 해줌
  - [x] 수정 : regex에 충족할 경우 email valid check과 submit이 가능하도록 수정 / 단 submit은 form valid를 통과해야 함
- [x] handle axios post status
  - 단순하게 try catch로 handling을 해버림...
