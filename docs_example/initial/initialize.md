# 프로젝트 문서 초기화 프롬프트

## 📋 프로젝트 문서 기초 구성
- 아래와 같이 프로젝트 문서를 구성하시오 (새로 만드시오)

## 📝 기본 구조

```
docs/
├── 기능_문서.md                 # 🎯 기능 명세 및 요구사항
├── 프로젝트_소개.md             # 🚀 프로젝트 개요 및 목표
├── reports/                     # 📊 결과 보고서
│   ├── ci-cd-report.md         # CI/CD 파이프라인 결과
│   ├── coverage-report.md      # 코드 커버리지 리포트
│   └── performance-report.md   # 기능 효율성 분석
├── guides/                      # 📋 개발 가이드
│   ├── development-guide.md    # 개발 방법
│   ├── build-guide.md          # 빌드 방법
│   └── deployment-guide.md     # 배포 방법
├── adr/                        # 🏛️ Architecture Decision Records
│   ├── README.md              # ADR 가이드
│   └── adr-template.md        # ADR 템플릿
├── development/                # 💻 개발 프로세스
│   ├── development-process.md  # 개발 방법론
│   ├── coding-standards.md    # 개발 규칙
│   └── bug-fix-process.md     # 버그 개선 프로세스
├── review/                     # 🔍 코드 검토
│   ├── quality-checklist.md   # 품질 관리 체크리스트
│   └── review-process.md      # 검토 프로세스
└── prompt-management/          # 🤖 AI 프롬프트 관리
    ├── prompt-changelog.md     # 프롬프트 변경사항
    └── prompt-templates.md     # 프롬프트 템플릿
```
- md파일은 내용 비운채로 만들 것.

## 완료 후
- 프로젝트 기본 폴더와 파일 생성한 뒤 다음을 안내하시오

**🔧 개발 시:**
```
@docs_example/development/development-process.md 
새로운 사용자 인증 기능을 개발하려고 해. 
이 문서의 개발 프로세스를 따라서 개발해줘 만들어줘.
```

**🐛 버그 개선 시:**
```
@docs_example/development/bug-fix-process.md 
로그인 시 세션이 간헐적으로 끊어지는 버그가 있어. 
이 버그픽스 프로세스를 따라서 단계별로 버그개선 해줘.
```

**✅ 검토 시:**
```
@docs_example/review/quality-checklist.md 
내가 작성한 API 코드를 검토해야 해. 
이 품질 체크리스트를 기반으로 코드 리뷰를 및 검토 도와줘.
```