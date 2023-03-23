# Accessibility

## Touch and pointer target sizes

- 터치는 시각적 경계를 넘어 확장
- 아이콘은 24x24dp로 표시될 수 있으나 아이콘을 둘러싼 패딩은 전체 48x48dp 터치 대상으로 구현 // 최소로
  - ios는 44x44dp 타겟 권장
- 아이콘 24, 별 아이콘 40, 양쪽 터치 40

## Pointer Target

- 터치 타겟과 유사
- 마우스나 stylus 같은 동작 추적 포인터로 구현
- 최소 44x44dp

## Target Spacing

- 대부분 8dp 이상 공간으로 분리된 대상은 균형 잡힌 정보 밀도와 유용성 촉진

## pointer order

- 시각적 레이아웃 순서(the order of the visual layout) : 위에서 아래로 / 가장 중요한 항목에서 그렇지 않은 항목으로
- 고려할 것

  - 요소가 포커스 받는 순서
  - 요소가 그룹화되는 방식
  - 포커스가 있는 요소가 사라질 때 포커스가 이동하는 위치
  - 포커스 포인트는 시각적 지표와 접근성 텍스트의 조합을 통해 표현

- 그룹화(grouping)

  - 그룹화 내용과 콘텐츠가 공간적으로 구성되어 있음을 알리는 제목 아래에 항목을 그룹화

- 전환(transitions)
  - 화면과 작업 사이의 지속적인 초점 이동은 사용자 경험을 개선하여 작업이 중단된 다음 다시 시작되는 경우 이전에 초점을 맞춘 요소로 돌아갈 수 있음

## Writing

[이어서](https://m3.material.io/foundations/accessible-design/accessibility-basics)
