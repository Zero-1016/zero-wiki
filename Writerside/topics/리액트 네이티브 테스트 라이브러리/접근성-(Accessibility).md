# 접근성 (Accessibility)

## `isHiddenFromAccessibility`

```typescript
function isHiddenFromAccessibility(element: ReactTestInstance | null): boolean {}
```

React Testing Library와의 호환성을 위해 `isInaccessible()` 별칭으로도 제공됩니다.

이 함수는 주어진 요소가 보조 기술(예: 화면 판독기)에서 숨겨져 있는지 여부를 확인합니다.

> 참고 사항
>
> 이 함수는 DOM Testing Library의 isInaccessible 함수와 마찬가지로, 접근성 요소와 프레젠테이션 요소(일반 View 요소) 모두를 호스트 플랫폼 측면에서 숨겨져 있지 않은 한 접근 가능한
> 것으로 간주합니다.
> 이 함수는 ARIA의 접근성 트리 개념의 일부만 다룹니다. ARIA는 숨겨진 요소와 프레젠테이션 요소를 모두 접근성 트리에서 제외합니다.

## 요소가 접근 불가능(inaccessible)한 경우

요소 자체 또는 그 조상 중 하나가 다음 조건을 충족할 때 요소는 접근 불가능한 것으로 간주됩니다.

- `display: none` 스타일이 적용된 경우
- `aria-hidden` 속성이 `true`로 설정된 경우
- `accessibilityElementsHidden` 속성이 `true`로 설정된 경우
- `importantForAccessibility` 속성이 `no-hide-descendants`로 설정된 경우
- 형제 호스트 요소에 `aria-modal` 또는 `accessibilityViewIsModal` 속성이 `true`로 설정된 경우

`accessible={false}`, `accessibilityRole="none"`, 또는 `importantForAccessibility="no"` 속성이 설정된 경우에도 해당 요소는 접근 불가능하게 되지 않습니다.