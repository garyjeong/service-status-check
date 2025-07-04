# 🚀 AI 서비스 상태 모니터링 대시보드 개발 일지

> 8일간의 개발 여정을 SNS에 공유하기 위한 일지 모음  
> 각 포스트는 300~600자 내외로 작성되어 Threads, Facebook 등에 바로 게시 가능

---

## 📅 **1일차: 프로젝트 기획 및 아키텍처 설계**

### 🎯 문제 정의와 현실적 고민

개발자로 일하면서 매일 마주하는 현실적인 문제가 있었다. ChatGPT로 코드 리뷰를 받다가 갑자기 "서비스 이용 불가" 메시지가 뜨면, Claude로 넘어간다. Claude도 느리면 Cursor의 AI 기능을 쓰고, 그것도 안 되면 Google AI Studio로 간다. 

하지만 문제는 여기서 끝나지 않는다. 개발 중에 GitHub이 다운되면 코드 푸시가 안 되고, Netlify에 문제가 생기면 배포가 막힌다. AWS나 Firebase에 장애가 발생하면 서비스 전체가 마비된다. 

매번 각 서비스의 상태 페이지를 일일이 확인하는 것은 비효율적이었다. Twitter에서 "ChatGPT 다운됐나요?" 같은 트윗을 찾아보거나, 각 서비스의 공식 상태 페이지를 북마크해두고 하나씩 확인하는 것은 시간 낭비였다.

### 🤔 솔루션 도출 과정과 깊은 고민

"개발자들이 자주 사용하는 모든 외부 서비스의 상태를 한 곳에서 볼 수 있다면 얼마나 좋을까?" 이 단순한 생각에서 시작했다. 

하지만 단순히 상태만 보여주는 것이 아니라, 실제 개발 워크플로우를 고려한 설계가 필요했다. 예를 들어:
- AI 서비스들은 개발자가 가장 자주 체크하므로 상단에 배치
- 클라우드 서비스는 배포 시에 중요하므로 별도 섹션으로 분리
- 개발 도구들은 카테고리별로 그룹화
- 즐겨찾기 기능으로 개인화된 모니터링 가능

### 🏗️ 기술 스택 선정의 철학적 접근

React 18 + TypeScript + Vite를 선택한 이유는 단순히 최신 기술이어서가 아니었다. 

1. **확장성**: 새로운 서비스 추가가 쉬워야 함
2. **성능**: 실시간 상태 체크로 인한 네트워크 부하 최소화
3. **유지보수성**: 각 서비스별 독립적인 모듈 구조
4. **사용자 경험**: 빠른 로딩과 직관적인 인터페이스

TanStack Query로 캐싱과 백그라운드 업데이트를 처리하고, Tailwind CSS로 일관된 디자인 시스템을 구축하기로 했다. 

### 📋 설계 단계에서의 핵심 결정사항

가장 중요한 결정은 "어떤 서비스들을 모니터링할 것인가?"였다. 

개발자 커뮤니티와 내 경험을 바탕으로 다음과 같이 분류했다:
- **AI 서비스**: ChatGPT, Claude, Cursor, Google AI Studio, Perplexity, v0, Replit, Grok
- **클라우드 플랫폼**: AWS, Google Cloud, Firebase, Supabase, Netlify
- **개발 도구**: GitHub, Docker, Slack

각 서비스마다 여러 하위 서비스가 있다는 점도 고려했다. 예를 들어 OpenAI는 ChatGPT Web, OpenAI API, DALL-E, Whisper API 등으로 세분화했다.

---

## 📅 **2일차: 개발 환경 구축 및 프로젝트 초기화**

### ⚙️ 개발 환경 세팅과 도구 선택
pnpm으로 패키지 관리, ESLint + Prettier로 코드 품질 관리, Vitest로 테스트 환경을 구축했다. 개발 서버 최적화를 위해 Vite 설정을 커스터마이징하고, TypeScript 엄격 모드로 타입 안정성을 확보했다.

### 📦 핵심 의존성 설치 및 구조 설계
TanStack Query, React Router, Tailwind CSS 등 필수 라이브러리를 설치하고, src 폴더 구조를 components, hooks, services, types, utils로 체계적으로 분리했다. 각 폴더의 역할과 의존성 방향을 명확히 정의했다.

---

## 📅 **3일차: 타입 시스템 및 API 레이어 구현**

### 🔧 타입 정의와 데이터 모델링
ServiceStatus, ServiceCategory 등 핵심 타입을 정의하고, 각 서비스의 상태 정보를 담을 수 있는 확장 가능한 인터페이스를 설계했다. 타입 안정성을 위해 Union 타입과 Generic을 적극 활용했다.

### 🌐 API 서비스 레이어 구축
외부 서비스 상태 체크를 위한 API 함수들을 구현하고, 에러 핸들링과 재시도 로직을 포함한 robust한 HTTP 클라이언트를 구축했다. 각 서비스별 엔드포인트와 상태 판단 로직을 모듈화했다.

---

## 📅 **4일차: 핵심 컴포넌트 개발 및 상태 관리**

### 🎨 Dashboard 컴포넌트 구현
메인 대시보드 컴포넌트를 구현하고, 서비스 카드 레이아웃과 그리드 시스템을 구축했다. 각 서비스의 상태를 시각적으로 표현할 수 있는 상태 인디케이터와 아이콘 시스템을 설계했다.

### 🔄 커스텀 훅과 상태 관리
useStatus 훅을 구현해 각 서비스의 상태를 실시간으로 모니터링하고, TanStack Query를 활용한 캐싱과 백그라운드 업데이트 로직을 구현했다. 사용자 경험을 위한 로딩 상태와 에러 상태 처리도 포함했다.

---

## 📅 **5일차: UI/UX 완성 및 반응형 디자인**

### 📱 반응형 레이아웃 구현
데스크톱, 태블릿, 모바일 각각에 최적화된 레이아웃을 구현했다. Tailwind CSS의 반응형 유틸리티를 활용해 브레이크포인트별 그리드 시스템을 구축하고, 터치 친화적인 모바일 인터페이스를 설계했다.

### 🎨 시각적 완성도와 브랜딩
각 서비스의 공식 로고와 브랜드 컬러를 적용하고, 다크/라이트 테마를 지원하는 컬러 시스템을 구축했다. 상태별 색상 코딩(녹색: 정상, 빨간색: 장애, 노란색: 부분 장애)으로 직관적인 정보 전달을 구현했다.

---

## 📅 **6일차: 고급 기능 및 사용자 경험 개선**

### ⭐ 즐겨찾기 및 개인화 기능
localStorage를 활용한 즐겨찾기 시스템을 구현하고, 사용자가 자주 사용하는 서비스를 상단에 고정할 수 있는 기능을 추가했다. 개인화된 대시보드 경험을 위한 설정 저장 로직도 구현했다.

### 🔍 검색 및 필터링 시스템
실시간 검색 기능과 카테고리별 필터링을 구현했다. 디바운싱을 적용한 검색 최적화와 키보드 단축키 지원으로 파워 유저를 위한 UX를 제공했다.

---

## 📅 **7일차: 테스트 코드 작성 및 광고 시스템 통합**

### 🧪 테스트 코드 작성과 품질 보증
Vitest를 활용해 유틸리티 함수와 커스텀 훅에 대한 단위 테스트를 작성했다. 상태 변환 로직과 API 함수들의 엣지 케이스를 커버하는 테스트 케이스를 구현해 코드 안정성을 확보했다.

### 💰 카카오 AdFit 광고 시스템 구현
수익화를 위해 카카오 AdFit 배너 광고를 통합했다. 데스크톱용 4개, 모바일용 별도 광고 단위를 설정하고, 랜덤 로테이션 시스템으로 광고 효율을 최적화했다. 광고 로드 실패 시 대체 처리 로직도 구현했다.

---

## 📅 **8일차: 배포 준비 및 Netlify 배포 완료**

### 🚀 프로덕션 빌드 최적화
Vite 빌드 설정을 최적화하고, 번들 사이즈 분석을 통해 불필요한 의존성을 제거했다. 이미지 최적화, 코드 스플리팅, Tree shaking을 적용해 로딩 성능을 개선했다.

### 🌐 Netlify 배포 및 도메인 설정
Netlify에 배포하고 커스텀 도메인을 연결했다. SEO를 위한 메타 태그, sitemap.xml, robots.txt를 설정하고, PWA 지원을 위한 매니페스트 파일도 추가했다. 최종적으로 Lighthouse 점수 90+ 달성으로 성능 검증을 완료했다.

---

## 📊 **프로젝트 성과 및 회고**

### 🎯 달성한 목표
- ✅ 20개 이상의 주요 개발 서비스 실시간 모니터링
- ✅ 반응형 디자인으로 모든 디바이스 지원
- ✅ 개인화 기능으로 사용자 맞춤 경험 제공
- ✅ 광고 시스템 통합으로 수익화 기반 마련
- ✅ 높은 성능 점수와 SEO 최적화

### 🚀 향후 계획
- 알림 시스템 추가 (서비스 장애 시 푸시 알림)
- 히스토리 기능 (과거 장애 이력 추적)
- 커뮤니티 기능 (사용자 제보 시스템)
- 모바일 앱 버전 개발

---

**프로젝트 링크**: [AI 서비스 상태 모니터링 대시보드](https://your-domain.com)  
**기술 스택**: React 18, TypeScript, Vite, TanStack Query, Tailwind CSS  
**배포**: Netlify  
**개발 기간**: 8일 