# 🛠️ 개발 방법 가이드

> **실무 중심의 종합 개발 가이드**  
> 프로젝트 시작부터 배포까지의 실용적인 개발 방법론

## 🎯 가이드 개요

### 📖 이 가이드의 목적
- **실무 중심**: 이론보다는 실제 적용 가능한 방법 제시
- **단계별 안내**: 프로젝트 생명주기 전반에 걸친 체계적 접근
- **품질 보장**: 일관된 품질 기준과 검증 방법 제공
- **효율성 극대화**: 개발 생산성과 코드 품질의 균형

---

## 🚀 프로젝트 시작 가이드

### 📋 1단계: 프로젝트 초기 설정

#### 환경 구성 체크리스트
```bash
# 개발 환경 확인
□ Node.js (LTS 버전)
□ Git 설정 완료
□ IDE/에디터 설정
□ 패키지 매니저 (npm/yarn)
□ 데이터베이스 설치

# 프로젝트 생성
npm create react-app my-project --template typescript
# 또는
npx create-next-app@latest my-project --typescript
```

#### 필수 도구 설치
```bash
# 코드 품질 도구
npm install -D eslint prettier husky lint-staged
npm install -D @typescript-eslint/eslint-plugin
npm install -D @typescript-eslint/parser

# 테스트 도구
npm install -D jest @testing-library/react
npm install -D @testing-library/jest-dom

# 빌드 도구
npm install -D webpack webpack-cli
```

### 📁 2단계: 프로젝트 구조 설계

#### Clean Architecture 기반 폴더 구조
```
src/
├── components/          # 재사용 가능한 UI 컴포넌트
│   ├── common/         # 공통 컴포넌트
│   ├── forms/          # 폼 관련 컴포넌트
│   └── layout/         # 레이아웃 컴포넌트
├── pages/              # 페이지 컴포넌트
├── hooks/              # 커스텀 훅
├── services/           # API 서비스
├── utils/              # 유틸리티 함수
├── types/              # TypeScript 타입 정의
├── constants/          # 상수 정의
├── assets/             # 정적 자원
└── __tests__/          # 테스트 파일
```

#### 설정 파일 구성
```typescript
// tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "es6"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "baseUrl": "src",
    "paths": {
      "@components/*": ["components/*"],
      "@services/*": ["services/*"],
      "@utils/*": ["utils/*"],
      "@types/*": ["types/*"]
    }
  },
  "include": ["src"]
}
```

---

## 💻 개발 프로세스 가이드

### 🔄 Git 워크플로우

#### 브랜치 전략 (Git Flow)
```bash
# 메인 브랜치
main          # 배포 가능한 안정 버전
develop       # 개발 중인 버전

# 지원 브랜치
feature/      # 새로운 기능 개발
bugfix/       # 버그 수정
hotfix/       # 긴급 수정
release/      # 배포 준비
```

#### 실제 사용 예시
```bash
# 새 기능 개발 시작
git checkout develop
git pull origin develop
git checkout -b feature/user-authentication

# 개발 진행...
git add .
git commit -m "feat: Add user login functionality"

# 기능 완료 후
git checkout develop
git pull origin develop
git merge feature/user-authentication
git push origin develop
git branch -d feature/user-authentication
```

#### 커밋 메시지 규칙 (Conventional Commits)
```
feat: 새로운 기능 추가
fix: 버그 수정
docs: 문서 수정
style: 코드 포맷팅 (기능 변경 없음)
refactor: 코드 리팩토링
test: 테스트 추가/수정
chore: 빌드/패키지 관리

예시:
feat(auth): Add JWT token validation
fix(api): Handle null response in user service
docs(readme): Update installation instructions
```

### 🧪 TDD 실무 적용

#### 실제 TDD 사이클 예시
```typescript
// 1. RED: 실패하는 테스트 작성
describe('UserService', () => {
  it('should validate email format', () => {
    const userService = new UserService();
    const result = userService.validateEmail('invalid-email');
    expect(result).toBe(false);
  });
});

// 2. GREEN: 최소한의 구현
class UserService {
  validateEmail(email: string): boolean {
    return email.includes('@');
  }
}

// 3. REFACTOR: 개선된 구현
class UserService {
  private static readonly EMAIL_REGEX = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  
  validateEmail(email: string): boolean {
    if (!email || typeof email !== 'string') {
      return false;
    }
    return UserService.EMAIL_REGEX.test(email.trim());
  }
}
```

#### 테스트 작성 가이드라인
```typescript
// ✅ 좋은 테스트
describe('Calculator', () => {
  describe('add', () => {
    it('should return sum of two positive numbers', () => {
      // Given
      const calculator = new Calculator();
      
      // When
      const result = calculator.add(2, 3);
      
      // Then
      expect(result).toBe(5);
    });
    
    it('should handle negative numbers', () => {
      const calculator = new Calculator();
      expect(calculator.add(-2, 3)).toBe(1);
    });
    
    it('should handle zero values', () => {
      const calculator = new Calculator();
      expect(calculator.add(0, 5)).toBe(5);
    });
  });
});

// ❌ 나쁜 테스트
it('should work', () => {
  const result = someFunction();
  expect(result).toBeTruthy();
});
```

---

## 🏗️ 코드 작성 가이드

### 📝 Clean Code 실습

#### 의미있는 이름 사용
```typescript
// ✅ Good
class UserAccountManager {
  private readonly users: User[] = [];
  
  async createUserAccount(userData: CreateUserRequest): Promise<User> {
    const validatedData = this.validateUserData(userData);
    const hashedPassword = await this.hashPassword(validatedData.password);
    
    return this.userRepository.create({
      ...validatedData,
      password: hashedPassword
    });
  }
}

// ❌ Bad
class Manager {
  private data: any[] = [];
  
  async create(input: any): Promise<any> {
    const d = this.validate(input);
    const p = await this.hash(d.pwd);
    return this.repo.save({ ...d, pwd: p });
  }
}
```

#### 함수 설계 원칙
```typescript
// ✅ Good - 단일 책임 원칙
class EmailService {
  async sendWelcomeEmail(user: User): Promise<void> {
    const emailContent = this.generateWelcomeContent(user);
    await this.sendEmail(user.email, emailContent);
  }
  
  private generateWelcomeContent(user: User): EmailContent {
    return {
      subject: `Welcome, ${user.name}!`,
      body: this.templateEngine.render('welcome', { user })
    };
  }
  
  private async sendEmail(to: string, content: EmailContent): Promise<void> {
    await this.emailProvider.send({
      to,
      subject: content.subject,
      html: content.body
    });
  }
}

// ❌ Bad - 여러 책임
async function processUser(userData: any): Promise<any> {
  // 검증
  if (!userData.email) throw new Error('Email required');
  
  // 비밀번호 해싱
  const hashedPwd = await bcrypt.hash(userData.password, 10);
  
  // DB 저장
  const user = await db.save({ ...userData, password: hashedPwd });
  
  // 이메일 발송
  await emailService.send(user.email, 'Welcome!');
  
  // 로깅
  logger.info(`User created: ${user.id}`);
  
  return user;
}
```

### 🔒 에러 처리 패턴

#### 체계적인 에러 처리
```typescript
// 커스텀 에러 클래스
export class ValidationError extends Error {
  constructor(message: string, public field?: string) {
    super(message);
    this.name = 'ValidationError';
  }
}

export class NotFoundError extends Error {
  constructor(resource: string, id: string) {
    super(`${resource} with id ${id} not found`);
    this.name = 'NotFoundError';
  }
}

// 서비스 레이어에서 에러 처리
class UserService {
  async getUser(id: string): Promise<User> {
    try {
      if (!id || id.trim().length === 0) {
        throw new ValidationError('User ID is required', 'id');
      }
      
      const user = await this.userRepository.findById(id);
      if (!user) {
        throw new NotFoundError('User', id);
      }
      
      return user;
    } catch (error) {
      this.logger.error('Failed to get user', {
        userId: id,
        error: error.message,
        stack: error.stack
      });
      throw error;
    }
  }
}

// 컨트롤러에서 에러 처리
app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await userService.getUser(req.params.id);
    res.json(user);
  } catch (error) {
    next(error); // 글로벌 에러 핸들러로 전달
  }
});

// 글로벌 에러 핸들러
app.use((error: Error, req: Request, res: Response, next: NextFunction) => {
  if (error instanceof ValidationError) {
    return res.status(400).json({
      error: 'Validation Error',
      message: error.message,
      field: error.field
    });
  }
  
  if (error instanceof NotFoundError) {
    return res.status(404).json({
      error: 'Not Found',
      message: error.message
    });
  }
  
  // 예상하지 못한 에러
  logger.error('Unexpected error', { error });
  res.status(500).json({
    error: 'Internal Server Error',
    message: 'Something went wrong'
  });
});
```

---

## 🧪 테스트 전략

### 📊 테스트 피라미드

#### 1. 유닛 테스트 (70%)
```typescript
// 비즈니스 로직 테스트
describe('PriceCalculator', () => {
  let calculator: PriceCalculator;
  
  beforeEach(() => {
    calculator = new PriceCalculator();
  });
  
  describe('calculateDiscountedPrice', () => {
    it('should apply 10% discount correctly', () => {
      const originalPrice = 100;
      const discountRate = 0.1;
      
      const result = calculator.calculateDiscountedPrice(originalPrice, discountRate);
      
      expect(result).toBe(90);
    });
    
    it('should throw error for invalid discount rate', () => {
      expect(() => {
        calculator.calculateDiscountedPrice(100, 1.5);
      }).toThrow('Discount rate must be between 0 and 1');
    });
  });
});
```

#### 2. 통합 테스트 (20%)
```typescript
// API 엔드포인트 테스트
describe('User API Integration Tests', () => {
  let app: Express;
  let testDb: Database;
  
  beforeAll(async () => {
    testDb = await setupTestDatabase();
    app = createApp(testDb);
  });
  
  afterAll(async () => {
    await testDb.close();
  });
  
  describe('POST /users', () => {
    it('should create new user with valid data', async () => {
      const userData = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123'
      };
      
      const response = await request(app)
        .post('/users')
        .send(userData)
        .expect(201);
      
      expect(response.body).toHaveProperty('id');
      expect(response.body.email).toBe(userData.email);
      expect(response.body).not.toHaveProperty('password');
    });
  });
});
```

#### 3. E2E 테스트 (10%)
```typescript
// Playwright를 사용한 E2E 테스트
import { test, expect } from '@playwright/test';

test.describe('User Registration Flow', () => {
  test('should complete user registration successfully', async ({ page }) => {
    // 회원가입 페이지로 이동
    await page.goto('/register');
    
    // 폼 작성
    await page.fill('[data-testid=name-input]', 'John Doe');
    await page.fill('[data-testid=email-input]', 'john@example.com');
    await page.fill('[data-testid=password-input]', 'password123');
    
    // 제출
    await page.click('[data-testid=submit-button]');
    
    // 성공 메시지 확인
    await expect(page.locator('[data-testid=success-message]')).toBeVisible();
    
    // 대시보드로 리다이렉트 확인
    await expect(page).toHaveURL('/dashboard');
  });
});
```

### 🎭 모킹 전략

#### 외부 의존성 모킹
```typescript
// API 호출 모킹
jest.mock('../services/apiClient');
const mockApiClient = apiClient as jest.Mocked<typeof apiClient>;

describe('UserService with mocked API', () => {
  it('should handle API success response', async () => {
    // Arrange
    const mockUser = { id: '1', name: 'John', email: 'john@example.com' };
    mockApiClient.get.mockResolvedValue({ data: mockUser });
    
    const userService = new UserService(mockApiClient);
    
    // Act
    const result = await userService.getUser('1');
    
    // Assert
    expect(result).toEqual(mockUser);
    expect(mockApiClient.get).toHaveBeenCalledWith('/users/1');
  });
  
  it('should handle API error response', async () => {
    // Arrange
    mockApiClient.get.mockRejectedValue(new Error('Network error'));
    const userService = new UserService(mockApiClient);
    
    // Act & Assert
    await expect(userService.getUser('1')).rejects.toThrow('Network error');
  });
});
```

---

## 🔍 코드 리뷰 가이드

### 📋 리뷰어 체크리스트

#### 기능 검토
```
□ 요구사항이 정확히 구현되었는가?
□ 비즈니스 로직이 올바른가?
□ 엣지 케이스가 고려되었는가?
□ 에러 처리가 적절한가?
```

#### 코드 품질 검토
```
□ 코드가 읽기 쉽고 이해하기 쉬운가?
□ 함수/클래스가 단일 책임을 가지는가?
□ 중복 코드가 없는가?
□ 네이밍이 명확하고 일관성 있는가?
```

#### 설계 검토
```
□ Clean Architecture 원칙을 따르는가?
□ SOLID 원칙이 적용되었는가?
□ 의존성 방향이 올바른가?
□ 적절한 추상화 레벨인가?
```

### 💬 효과적인 리뷰 코멘트

#### 좋은 리뷰 예시
```markdown
🔧 **개선 제안**
이 함수가 너무 많은 책임을 가지고 있는 것 같습니다. 
검증 로직과 비즈니스 로직을 분리하는 것이 어떨까요?

📖 **코드 설명 요청**
이 알고리즘의 시간 복잡도가 궁금합니다. 
주석으로 설명을 추가해주실 수 있나요?

✅ **긍정적 피드백**
에러 처리가 매우 깔끔하게 되어 있네요! 
사용자 친화적인 메시지도 좋습니다.
```

---

## 🚀 배포 및 운영

### 📦 빌드 최적화

#### Webpack 설정
```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  mode: 'production',
  entry: './src/index.tsx',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash].js',
    clean: true
  },
  optimization: {
    splitChunks: {
      chunks: 'all',
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all'
        }
      }
    }
  },
  performance: {
    maxAssetSize: 500000,
    maxEntrypointSize: 500000
  }
};
```

#### Docker 컨테이너화
```dockerfile
# Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### 📊 모니터링 설정

#### 핵심 메트릭 정의
```typescript
// 성능 모니터링
class PerformanceMonitor {
  static trackApiCall(endpoint: string, duration: number, status: number) {
    metrics.histogram('api_request_duration', duration, {
      endpoint,
      status: status.toString()
    });
    
    if (status >= 400) {
      metrics.increment('api_errors', 1, { endpoint, status: status.toString() });
    }
  }
  
  static trackUserAction(action: string, userId?: string) {
    metrics.increment('user_actions', 1, { action });
    
    if (userId) {
      analytics.track('User Action', {
        userId,
        action,
        timestamp: new Date().toISOString()
      });
    }
  }
}
```

---

## 🛠️ 개발 도구 활용

### 🔧 필수 VSCode 확장

#### 권장 확장 프로그램 (.vscode/extensions.json)
```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-typescript-next",
    "bradlc.vscode-tailwindcss",
    "ms-vscode.vscode-jest",
    "ms-vscode.vscode-eslint",
    "formulahendry.auto-rename-tag",
    "christian-kohler.path-intellisense"
  ]
}
```

#### 워크스페이스 설정 (.vscode/settings.json)
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.preferences.importModuleSpecifier": "relative",
  "files.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.coverage": true
  }
}
```

### 🔄 자동화 스크립트

#### 개발 편의 스크립트 (package.json)
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint . --ext .ts,.tsx",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "type-check": "tsc --noEmit",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:e2e": "playwright test",
    "prepare": "husky install"
  }
}
```

---

## 📚 학습 리소스

### 📖 권장 도서
- Clean Code - Robert C. Martin
- Clean Architecture - Robert C. Martin
- Refactoring - Martin Fowler
- You Don't Know JS - Kyle Simpson

### 🔗 유용한 링크
- [TypeScript 공식 문서](https://www.typescriptlang.org/)
- [React 공식 문서](https://reactjs.org/)
- [Jest 공식 문서](https://jestjs.io/)
- [ESLint 규칙](https://eslint.org/docs/rules/)

### 🎓 지속적 학습
- 주간 기술 아티클 읽기
- 오픈소스 프로젝트 기여
- 개발 컨퍼런스 참여
- 팀 내 기술 공유

---

**버전**: v1.0  
**최종 업데이트**: [YYYY-MM-DD]  
**작성자**: [개발팀] 