# Claude Code 커스텀 커맨드

이 폴더에는 프로젝트 자동화를 위한 커스텀 커맨드들이 저장되어 있습니다.

## 📦 설치된 커맨드

### 1. `create-component` - React 컴포넌트 생성기

React 컴포넌트 보일러플레이트를 자동으로 생성합니다.

#### 사용법
```bash
create-component <ComponentName>
```

#### 예시
```bash
create-component Button
create-component Modal
create-component UserCard
```

#### 생성되는 파일 구조
```
components/
└── ComponentName/
    ├── ComponentName.tsx       # 메인 컴포넌트 (TypeScript, Tailwind, PropTypes)
    ├── types.ts                # Props 인터페이스 정의
    └── index.ts                # Export 파일
```

#### 생성되는 코드의 특징

**1. TypeScript 지원**
- Props 인터페이스 자동 생성
- 완전한 타입 안정성 (any 타입 없음)

**2. Tailwind CSS 반응형 스타일**
- 모바일 우선 접근법
- `sm`, `md`, `lg` 반응형 클래스 포함
- 실제 개발에 맞는 스타일 조합

**3. PropTypes 검증**
- 런타임 Props 타입 검증
- 개발 환경에서 실수 방지
- 기본값(`defaultProps`) 자동 설정

**4. 성능 최적화**
- `useMemo`로 스타일 클래스 최적화
- 불필요한 재계산 방지

**5. 접근성(A11y)**
- `aria-disabled` 속성 지원
- 의미있는 HTML 구조

#### 예제: Button 생성 및 사용

```bash
# 1. 버튼 컴포넌트 생성
create-component Button

# 2. 생성된 파일 편집
# components/Button/Button.tsx - 로직 구현
# components/Button/types.ts - Props 타입 수정

# 3. 사용
import { Button } from '@/components/Button';

export default function Page() {
  return (
    <Button variant="primary" size="md">
      클릭하세요
    </Button>
  );
}
```

#### 자동 생성되는 Props 옵션

```typescript
interface ComponentNameProps {
  variant?: 'primary' | 'secondary' | 'outline';  // 스타일 변형
  size?: 'sm' | 'md' | 'lg';                      // 크기
  disabled?: boolean;                              // 비활성화 상태
  className?: string;                              // 추가 클래스
  children?: React.ReactNode;                      // 자식 요소
}
```

#### Tailwind CSS 반응형 클래스 가이드

| 클래스 | 해상도 | 용도 |
|--------|--------|------|
| `w-full` | 모든 화면 | 기본 모바일 (100% 너비) |
| `sm:w-auto` | 640px+ | 태블릿 이상 (자동 너비) |
| `md:px-5` | 768px+ | 태블릿 (패딩 조정) |
| `lg:px-6` | 1024px+ | 데스크톱 (큰 패딩) |
| `xl:` | 1280px+ | 와이드 데스크톱 |

#### 팁

1. **Props 타입 수정**: 생성 후 `types.ts`에서 필요한 Props를 추가/제거하세요.

```typescript
export interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'danger';  // 'danger' 추가
  size?: 'sm' | 'md' | 'lg' | 'xl';             // 'xl' 추가
  onClick?: () => void;                          // 클릭 핸들러
  // ... 기타 Props
}
```

2. **스타일 커스터마이징**: `ComponentName.tsx`의 `variantClasses`, `sizeClasses`를 수정하세요.

```typescript
const variantClasses = {
  primary: 'bg-blue-600 text-white hover:bg-blue-700',
  secondary: 'bg-gray-200 text-gray-900 hover:bg-gray-300',
  danger: 'bg-red-600 text-white hover:bg-red-700',  // 새로운 변형 추가
};
```

3. **로직 구현**: 생성된 JSX를 필요에 맞게 수정하세요.

```typescript
const Button: FC<ButtonProps> = ({ onClick, ...props }) => {
  const handleClick = () => {
    console.log('버튼 클릭됨');
    onClick?.();
  };

  return (
    <button onClick={handleClick} {...props}>
      {/* ... */}
    </button>
  );
};
```

---

## 커스텀 커맨드 추가하기

새로운 커맨드를 추가하려면:

1. `.claude/commands/` 폴더에 bash 스크립트 생성
2. 실행 권한 설정: `chmod +x .claude/commands/command-name`
3. 스크립트에 shebang 추가: `#!/bin/bash`

예제:
```bash
#!/bin/bash
# .claude/commands/create-hook
echo "✅ Custom Hook 생성: $1"
```
