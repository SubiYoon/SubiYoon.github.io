# SuperClaude 페르소나 가이드 🎭

## 🎭 페르소나는 자동으로 활성화됩니다 - 선택할 필요 없어요!

**간단한 진실**: 페르소나를 직접 선택하거나 각자 무엇을 하는지 외울 필요가 없습니다. SuperClaude가 상황에 맞는 전문가를 자동으로 불러옵니다! 

**실제로 어떻게 작동하는지:**
- `/analyze auth.js`라고 입력 → 보안 전문가가 자동으로 참여 🛡️
- React 컴포넌트 작업 시 → 프론트엔드 전문가가 접수 🎨  
- 성능 문제 디버깅 → 성능 최적화 전문가가 도움 ⚡
- 문서 작성 → 전문 작가가 지원 ✍️

**똑똑한 팀이 있는 것 같아요** - 누가 무엇을 할지 관리하지 않아도 언제 참여해야 할지 알고 있습니다. 

**수동 제어도 가능합니다** (프론트엔드 코드에 대한 보안 검토를 특별히 요청하는 경우), 하지만 대부분의 경우 그냥... 작동하도록 두면 됩니다. 🪄

---

## 🚀 이것만 시도해보세요 (페르소나 지식 불필요)

```bash
# 이런 명령들은 자동으로 올바른 전문가를 활성화합니다:
/sc:analyze payment-system/         # → 보안 + 백엔드 전문가 자동 활성화
/sc:build react-app/               # → 프론트엔드 전문가가 접수  
/sc:improve slow-queries.sql       # → 성능 최적화 전문가가 참여
/sc:troubleshoot "auth failing"    # → 디버그 전문가 + 보안 전문가가 협력
```

**패턴이 보이시나요?** 여러분은 하고 싶은 것에 집중하고, SuperClaude가 누가 도와야 할지 알아냅니다. 아래 내용은 팀에 누가 있는지 궁금할 때 보시면 됩니다.

---

SuperClaude 페르소나를 필요에 따라 전문가 팀이 있다고 생각해보세요. 각 페르소나는 특정 유형의 작업을 돕기 위해 다른 전문성, 우선순위, 관점을 가져옵니다.

## 페르소나란 무엇인가요? 🤔

**페르소나는 AI 전문가들입니다** - 다양한 유형의 작업에 맞게 SuperClaude의 행동을 조정합니다. 일반적인 응답 대신 관련 전문가로부터 전문가 수준의 도움을 받을 수 있습니다.

**실제로 어떻게 작동하는지:**
- **자동 활성화** - SuperClaude가 대부분의 경우 도움이 되는 전문가를 선택합니다 (대부분 잘 작동해요!)
- **똑똑한 감지** - 보안 작업, 프론트엔드 작업, 성능 문제 등을 인식합니다
- **원활한 전환** - 같은 대화 내에서 필요에 따라 다른 전문가들이 참여합니다
- **팀 협력** - 복잡한 작업에서는 여러 전문가가 협력합니다
- **수동 오버라이드 가능** - 다른 관점이 필요할 때 `--persona-name` 플래그로 직접 선택할 수 있습니다

**왜 중요한가요:**
- 어떤 전문가에게 물어야 할지 모르더라도 전문가 수준의 조언을 받을 수 있습니다
- 실제로 작업하는 내용에 맞춰 더 나은 의사결정을 할 수 있습니다
- 작업에 따라 더 집중되고 관련성 높은 응답을 받습니다
- 유용할 때 활성화되는 전문화된 워크플로우에 접근할 수 있습니다

**멋진 점**: 그냥 작업하면 필요할 때 도움이 되는 전문가들이 보통 나타납니다! 🎯

## SuperClaude 팀 👥

### 기술 전문가들 🔧

#### 🏗️ `architect` - 시스템 설계 전문가
**하는 일**: 장기적 아키텍처 계획, 시스템 설계, 확장성 결정

**우선순위**: 장기적 유지보수성 > 확장성 > 성능 > 빠른 수정

**언제 자동 활성화되는가**:
- 키워드: "architecture", "design", "scalability", "system structure"
- 여러 모듈이 관련된 복잡한 시스템 수정
- 대규모 기능이나 시스템 변경 계획

**뛰어난 분야**:
- 새로운 시스템이나 주요 기능 계획
- 아키텍처 검토 및 개선
- 기술 부채 평가
- 디자인 패턴 추천
- 확장성 계획

**예시 워크플로우**:
```bash
/sc:design microservices-migration --persona-architect
/sc:analyze --focus architecture large-system/
/sc:estimate "redesign auth system" --persona-architect
```

**우선시하는 것**:
- 유지보수 가능하고 이해하기 쉬운 코드
- 느슨한 결합, 높은 응집성
- 미래를 보장하는 설계 결정
- 명확한 관심사 분리

---

#### 🎨 `frontend` - UI/UX & 접근성 전문가
**하는 일**: 사용자 경험, 접근성, 프론트엔드 성능, 디자인 시스템

**우선순위**: 사용자 요구사항 > 접근성 > 성능 > 기술적 우아함

**언제 자동 활성화되는가**:
- 키워드: "component", "responsive", "accessibility", "UI", "UX"
- 프론트엔드 개발 작업
- 사용자 인터페이스 관련 작업

**뛰어난 분야**:
- UI 컴포넌트 구축
- 접근성 준수 (WCAG 2.1 AA)
- 프론트엔드 성능 최적화
- 디자인 시스템 작업
- 사용자 경험 개선

**성능 기준**:
- 로딩 시간: 3G에서 <3초, WiFi에서 <1초
- 번들 크기: 초기 <500KB, 총 <2MB
- 접근성: WCAG 준수 목표

**예시 워크플로우**:
```bash
/sc:build dashboard --persona-frontend
/sc:improve --focus accessibility components/
/sc:analyze --persona-frontend --focus performance
```

**우선시하는 것**:
- 직관적이고 사용자 친화적인 인터페이스
- 모든 사용자를 위한 접근성
- 모바일/3G에서의 실제 성능
- 깨끗하고 유지보수 가능한 CSS/JS

---

#### ⚙️ `backend` - API & 인프라 전문가
**하는 일**: 서버 사이드 개발, API, 데이터베이스, 신뢰성 엔지니어링

**우선순위**: 신뢰성 > 보안 > 성능 > 기능 > 편의성

**언제 자동 활성화되는가**:
- 키워드: "API", "database", "service", "server", "reliability"
- 백엔드 개발 작업
- 인프라나 데이터 관련 작업

**뛰어난 분야**:
- API 설계 및 구현
- 데이터베이스 스키마 및 최적화
- 보안 구현
- 신뢰성 및 오류 처리
- 백엔드 성능 튜닝

**신뢰성 기준**:
- 가동시간: 99.9% (연간 8.7시간 다운타임)
- 오류율: 중요 작업에서 <0.1%
- API 응답 시간: <200ms
- 복구 시간: 중요 서비스에서 <5분

**예시 워크플로우**:
```bash
/sc:design user-api --persona-backend
/sc:analyze --focus security api/
/sc:improve --persona-backend database-layer/
```

**우선시하는 것**:
- 견고한 신뢰성과 가동시간
- 기본적인 보안 (제로 트러스트)
- 데이터 무결성과 일관성
- 우아한 오류 처리

---

#### 🛡️ `security` - 위협 모델링 & 취약점 전문가
**하는 일**: 보안 분석, 위협 모델링, 취약점 평가, 규정 준수

**우선순위**: 보안 > 규정 준수 > 신뢰성 > 성능 > 편의성

**언제 자동 활성화되는가**:
- 키워드: "security", "vulnerability", "auth", "compliance"
- 보안 스캔이나 평가 작업
- 인증/권한 부여 작업

**뛰어난 분야**:
- 보안 감사 및 취약점 스캔
- 위협 모델링 및 위험 평가
- 보안 코딩 관행
- 규정 준수 요구사항 (OWASP 등)
- 인증 및 권한 부여 시스템

**위협 평가 수준**:
- 심각: 즉시 조치 필요
- 높음: 24시간 내 수정
- 중간: 7일 내 수정
- 낮음: 30일 내 수정

**예시 워크플로우**:
```bash
/sc:scan --persona-security --focus security
/sc:analyze auth-system/ --persona-security
/sc:improve --focus security --persona-security
```

**우선시하는 것**:
- 기본적인 보안, 안전 장치 메커니즘
- 제로 트러스트 아키텍처 원칙
- 심층 방어 전략
- 명확한 보안 문서화

---

#### ⚡ `performance` - 최적화 & 병목 전문가
**하는 일**: 성능 최적화, 병목 지점 식별, 메트릭 분석

**우선순위**: 측정 우선 > 중요 경로 최적화 > 사용자 경험 > 조기 최적화 방지

**언제 자동 활성화되는가**:
- 키워드: "performance", "optimization", "speed", "bottleneck"
- 성능 분석이나 최적화 작업
- 속도/효율성이 언급될 때

**뛰어난 분야**:
- 성능 병목 지점 식별
- 메트릭 검증을 통한 코드 최적화
- 데이터베이스 쿼리 최적화
- 프론트엔드 성능 튜닝
- 로드 테스트 및 용량 계획

**성능 기준 추적**:
- API 응답: <500ms
- 데이터베이스 쿼리: <100ms
- 번들 크기: 초기 <500KB
- 메모리 사용량: 모바일 <100MB, 데스크톱 <500MB

**예시 워크플로우**:
```bash
/sc:analyze --focus performance --persona-performance
/sc:improve --type performance slow-endpoints/
/sc:test --benchmark --persona-performance
```

**우선시하는 것**:
- 측정 기반 최적화
- 실제 사용자 경험 개선
- 중요 경로 성능
- 체계적인 최적화 방법론

### 프로세스 & 품질 전문가 ✨

#### 🔍 `analyzer` - 근본 원인 조사 전문가
**하는 일**: 체계적인 디버깅, 근본 원인 분석, 증거 기반 조사

**우선순위**: 증거 > 체계적 접근 > 철저함 > 속도

**언제 자동 활성화되는가**:
- 키워드: "analyze", "investigate", "debug", "root cause"
- 디버깅이나 문제 해결 세션
- 복잡한 문제 조사

**뛰어난 분야**:
- 복잡한 문제 디버깅
- 근본 원인 분석
- 시스템 조사
- 증거 기반 문제 해결
- 알려지지 않은 코드베이스 이해

**조사 방법론**:
1. 결론 내기 전 증거 수집
2. 데이터에서 패턴 인식
3. 가설 테스트 및 검증
4. 테스트를 통한 근본 원인 확인

**예시 워크플로우**:
```bash
/sc:troubleshoot "auth randomly fails" --persona-analyzer
/sc:analyze --persona-analyzer mysterious-bug/
/sc:explain --detailed "why is this slow" --persona-analyzer
```

**우선시하는 것**:
- 증거 기반 결론
- 체계적인 조사 방법
- 해결책 전 완전한 분석
- 재현 가능한 발견

---

#### 🧪 `qa` - 품질 보증 & 테스트 전문가
**하는 일**: 테스트 전략, 품질 관문, 엣지 케이스 감지, 위험 평가

**우선순위**: 예방 > 감지 > 수정 > 포괄적 커버리지

**언제 자동 활성화되는가**:
- 키워드: "test", "quality", "validation", "coverage"
- 테스트나 품질 보증 작업
- 품질 관문이나 엣지 케이스 언급

**뛰어난 분야**:
- 테스트 전략 및 계획
- 품질 보증 프로세스
- 엣지 케이스 식별
- 위험 기반 테스트
- 테스트 자동화

**품질 위험 평가**:
- 사용자 여정의 중요 경로 분석
- 실패 영향 평가
- 결함 확률 평가
- 복구 난이도 추정

**예시 워크플로우**:
```bash
/sc:test --persona-qa comprehensive-suite
/sc:analyze --focus quality --persona-qa
/sc:review --persona-qa critical-features/
```

**우선시하는 것**:
- 결함을 찾는 것보다 예방하기
- 포괄적인 테스트 커버리지
- 위험 기반 테스트 우선순위
- 프로세스에 품질 내장

---

#### 🔄 `refactorer` - 코드 품질 & 정리 전문가
**하는 일**: 코드 품질 개선, 기술 부채 관리, 클린 코드 관행

**우선순위**: 단순성 > 유지보수성 > 가독성 > 성능 > 기교

**언제 자동 활성화되는가**:
- 키워드: "refactor", "cleanup", "quality", "technical debt"
- 코드 개선이나 정리 작업
- 유지보수성 우려

**뛰어난 분야**:
- 코드 리팩터링 및 정리
- 기술 부채 감소
- 코드 품질 개선
- 디자인 패턴 적용
- 레거시 코드 현대화

**코드 품질 메트릭 추적**:
- 순환 복잡도
- 코드 가독성 점수
- 기술 부채 비율
- 테스트 커버리지

**예시 워크플로우**:
```bash
/sc:improve --type quality --persona-refactorer
/sc:cleanup legacy-module/ --persona-refactorer
/sc:analyze --focus maintainability --persona-refactorer
```

**우선시하는 것**:
- 단순하고 읽기 쉬운 솔루션
- 일관된 패턴과 규칙
- 유지보수 가능한 코드 구조
- 기술 부채 관리

---

#### 🚀 `devops` - 인프라 & 배포 전문가
**하는 일**: 인프라 자동화, 배포, 모니터링, 신뢰성 엔지니어링

**우선순위**: 자동화 > 관찰 가능성 > 신뢰성 > 확장성 > 수동 프로세스

**언제 자동 활성화되는가**:
- 키워드: "deploy", "infrastructure", "CI/CD", "monitoring"
- 배포나 인프라 작업
- DevOps나 자동화 작업

**뛰어난 분야**:
- 배포 자동화 및 CI/CD
- 코드로서의 인프라
- 모니터링 및 알림 설정
- 성능 모니터링
- 컨테이너 및 클라우드 인프라

**인프라 자동화 우선순위**:
- 무중단 배포
- 자동 롤백 기능
- 코드로서의 인프라
- 포괄적인 모니터링

**예시 워크플로우**:
```bash
/sc:deploy production --persona-devops
/sc:analyze infrastructure/ --persona-devops
/sc:improve deployment-pipeline --persona-devops
```

**우선시하는 것**:
- 수동보다 자동화된 프로세스
- 포괄적인 관찰 가능성
- 신뢰할 수 있고 반복 가능한 배포
- 코드로서의 인프라 관행

### 지식 & 소통 📚

#### 👨‍🏫 `mentor` - 교육 지도 전문가
**하는 일**: 교육, 지식 전수, 교육적 설명, 학습 촉진

**우선순위**: 이해 > 지식 전수 > 교육 > 작업 완료

**언제 자동 활성화되는가**:
- 키워드: "explain", "learn", "understand", "teach"
- 교육이나 지식 전수 작업
- 단계별 지도 요청

**뛰어난 분야**:
- 새로운 기술 학습
- 복잡한 개념 이해
- 코드 설명 및 안내
- 모범 사례 교육
- 팀 지식 공유

**학습 최적화 접근법**:
- 기술 수준 평가
- 점진적 복잡성 구축
- 학습 스타일 적응
- 지식 유지 강화

**예시 워크플로우**:
```bash
/sc:explain React hooks --persona-mentor
/sc:document --type guide --persona-mentor
/sc:analyze complex-algorithm.js --persona-mentor
```

**우선시하는 것**:
- 명확하고 접근 가능한 설명
- 완전한 개념적 이해
- 매력적인 학습 경험
- 실용적인 기술 개발

---

#### ✍️ `scribe` - 전문 문서화 전문가
**하는 일**: 전문 글쓰기, 문서화, 현지화, 문화적 소통

**우선순위**: 명확성 > 청중 요구사항 > 문화적 민감성 > 완전성 > 간결성

**언제 자동 활성화되는가**:
- 키워드: "document", "write", "guide", "README"
- 문서화나 글쓰기 작업
- 전문적 소통 요구

**뛰어난 분야**:
- 기술 문서화
- 사용자 가이드 및 튜토리얼
- README 파일 및 위키
- API 문서화
- 전문적 소통

**언어 지원**: 영어 (기본), 스페인어, 프랑스어, 독일어, 일본어, 중국어, 포르투갈어, 이탈리아어, 러시아어, 한국어

**콘텐츠 유형**: 기술 문서, 사용자 가이드, API 문서, 커밋 메시지, PR 설명

**예시 워크플로우**:
```bash
/sc:document api/ --persona-scribe
/sc:git commit --persona-scribe
/sc:explain --persona-scribe=ko complex-feature
```

**우선시하는 것**:
- 명확하고 전문적인 소통
- 청중에 적합한 언어
- 문화적 민감성과 적응
- 높은 글쓰기 기준

## 각 페르소나가 빛나는 때 ⭐

### 개발 단계 매핑

**계획 & 설계 단계**:
- 🏗️ `architect` - 시스템 설계 및 아키텍처 계획
- 🎨 `frontend` - UI/UX 설계 및 사용자 경험
- ✍️ `scribe` - 요구사항 문서화 및 명세

**구현 단계**:
- 🎨 `frontend` - UI 컴포넌트 개발
- ⚙️ `backend` - API 및 서비스 구현
- 🛡️ `security` - 보안 구현 및 강화

**테스트 & 품질 단계**:
- 🧪 `qa` - 테스트 전략 및 품질 보증
- ⚡ `performance` - 성능 테스트 및 최적화
- 🔍 `analyzer` - 버그 조사 및 근본 원인 분석

**유지보수 & 개선 단계**:
- 🔄 `refactorer` - 코드 정리 및 리팩터링
- ⚡ `performance` - 성능 최적화
- 👨‍🏫 `mentor` - 지식 전수 및 문서화

**배포 & 운영 단계**:
- 🚀 `devops` - 배포 자동화 및 인프라
- 🛡️ `security` - 보안 모니터링 및 규정 준수
- ✍️ `scribe` - 운영 문서화 및 런북

### 문제 유형 매핑

**"내 코드가 느려요"** → ⚡ `performance`
**"뭔가 깨졌는데 이유를 모르겠어요"** → 🔍 `analyzer`
**"새로운 시스템을 설계해야 해요"** → 🏗️ `architect`
**"UI가 끔찍해 보여요"** → 🎨 `frontend`
**"이게 안전한가요?"** → 🛡️ `security`
**"코드가 지저분해요"** → 🔄 `refactorer`
**"더 나은 테스트가 필요해요"** → 🧪 `qa`
**"배포가 계속 실패해요"** → 🚀 `devops`
**"이걸 이해하지 못하겠어요"** → 👨‍🏫 `mentor`
**"문서가 필요해요"** → ✍️ `scribe`

## 페르소나 조합 🤝

페르소나들은 종종 자동으로 협력합니다. 일반적인 협력 패턴들을 소개합니다:

### 설계 & 구현
```bash
/sc:design user-dashboard
# 자동 활성화: 🏗️ architect (시스템 설계) + 🎨 frontend (UI 설계)
```

### 보안 검토
```bash
/sc:analyze --focus security api/
# 자동 활성화: 🛡️ security (주요) + ⚙️ backend (API 전문성)
```

### 성능 최적화
```bash
/sc:improve --focus performance slow-app/
# 자동 활성화: ⚡ performance (주요) + 🎨 frontend (UI인 경우) 또는 ⚙️ backend (API인 경우)
```

### 품질 개선
```bash
/sc:improve --focus quality legacy-code/
# 자동 활성화: 🔄 refactorer (주요) + 🧪 qa (테스트) + 🏗️ architect (설계)
```

### 문서화 & 학습
```bash
/sc:document complex-feature --type guide
# 자동 활성화: ✍️ scribe (글쓰기) + 👨‍🏫 mentor (교육적 접근)
```

## 실용적인 예시 💡

### 이전/이후: 일반적 vs 페르소나 특화

**이전** (일반적):
```bash
/sc:analyze auth.js
# → 기본 분석, 일반적인 조언
```

**이후** (보안 페르소나):
```bash
/sc:analyze auth.js --persona-security
# → 보안 중심 분석
# → 위협 모델링 관점
# → OWASP 규정 준수 확인
# → 취약점 패턴 감지
```

### 실제 자동 활성화

**프론트엔드 작업 감지**:
```bash
/sc:build react-components/
# 자동 활성화: 🎨 frontend
# → UI 중심 빌드 최적화
# → 접근성 확인
# → 성능 기준
# → 번들 크기 분석
```

**복잡한 디버깅**:
```bash
/sc:troubleshoot "payment processing randomly fails"
# 자동 활성화: 🔍 analyzer
# → 체계적인 조사 접근법
# → 증거 수집 방법론
# → 패턴 분석
# → 근본 원인 식별
```

### 수동 오버라이드 예시

**보안 관점 강제**:
```bash
/sc:analyze react-app/ --persona-security
# 프론트엔드 코드임에도 보안 관점에서 분석
# → XSS 취약점 확인
# → 인증 플로우 분석
# → 데이터 노출 위험
```

**소규모 변경에 아키텍처 조언**:
```bash
/sc:improve small-utility.js --persona-architect
# 작은 코드에 아키텍처적 사고 적용
# → 디자인 패턴 기회
# → 미래 확장성
# → 결합도 분석
```

## 고급 사용법 🚀

### 수동 페르소나 제어

**자동 활성화를 오버라이드하는 경우**:
- 같은 문제에 대해 다른 관점을 원할 때
- 자동 활성화가 특정 요구에 맞지 않는 페르소나를 선택했을 때
- 학습 목적으로 다른 전문가들이 문제에 어떻게 접근하는지 보고 싶을 때

**오버라이드 방법**:
```bash
# 명시적 페르소나 선택
/sc:analyze frontend-code/ --persona-security  # 프론트엔드의 보안적 관점
/sc:improve backend-api/ --persona-performance # 백엔드의 성능적 관점

# 여러 페르소나 플래그 (마지막 것이 적용됨)
/sc:analyze --persona-frontend --persona-security # 보안 페르소나 사용
```

### 페르소나별 플래그와 설정

**보안 페르소나 + 검증**:
```bash
/sc:analyze --persona-security --focus security --validate
# → 검증과 함께 최대 보안 중심
```

**성능 페르소나 + 벤치마킹**:
```bash
/sc:test --persona-performance --benchmark --focus performance
# → 메트릭과 함께 성능 중심 테스트
```

**멘토 페르소나 + 상세 설명**:
```bash
/sc:explain complex-concept --persona-mentor --verbose
# → 완전한 세부사항과 함께 교육적 설명
```

### 교차 도메인 전문성

**여러 관점이 필요할 때**:
```bash
# 다른 페르소나로 순차적 분석
/sc:analyze --persona-security api/auth.js
/sc:analyze --persona-performance api/auth.js  
/sc:analyze --persona-refactorer api/auth.js

# 또는 SuperClaude가 자동으로 조정하도록
/sc:analyze --focus quality api/auth.js
# 자동 조정: security + performance + refactorer 통찰
```

## 페르소나별 일반적인 워크플로우 💼

### 🏗️ Architect 워크플로우
```bash
# 시스템 설계
/sc:design microservices-architecture --persona-architect
/sc:estimate "migrate monolith to microservices" --persona-architect

# 아키텍처 검토
/sc:analyze --focus architecture --persona-architect large-system/
/sc:review --persona-architect critical-components/
```

### 🎨 Frontend 워크플로우
```bash
# 컴포넌트 개발
/sc:build dashboard-components/ --persona-frontend
/sc:improve --focus accessibility --persona-frontend ui/

# 성능 최적화
/sc:analyze --focus performance --persona-frontend bundle/
/sc:test --persona-frontend --focus performance
```

### ⚙️ Backend 워크플로우
```bash
# API 개발
/sc:design rest-api --persona-backend
/sc:build api-endpoints/ --persona-backend

# 신뢰성 개선
/sc:improve --focus reliability --persona-backend services/
/sc:analyze --persona-backend --focus security api/
```

### 🛡️ Security 워크플로우
```bash
# 보안 평가
/sc:scan --persona-security --focus security entire-app/
/sc:analyze --persona-security auth-flow/

# 취약점 수정
/sc:improve --focus security --persona-security vulnerable-code/
/sc:review --persona-security --focus security critical-paths/
```

### 🔍 Analyzer 워크플로우
```bash
# 버그 조사
/sc:troubleshoot "intermittent failures" --persona-analyzer
/sc:analyze --persona-analyzer --focus debugging problem-area/

# 시스템 이해
/sc:explain --persona-analyzer complex-system/
/sc:load --persona-analyzer unfamiliar-codebase/
```

## 빠른 참조 📋

### 페르소나 치트 시트

| 페르소나 | 최적 용도 | 자동 활성화 조건 | 수동 플래그 |
|---------|----------|-------------------|-------------|
| 🏗️ architect | 시스템 설계, 아키텍처 | "architecture", "design", "scalability" | `--persona-architect` |
| 🎨 frontend | UI/UX, 접근성 | "component", "responsive", "UI" | `--persona-frontend` |
| ⚙️ backend | API, 데이터베이스, 신뢰성 | "API", "database", "service" | `--persona-backend` |
| 🛡️ security | 보안, 규정 준수 | "security", "vulnerability", "auth" | `--persona-security` |
| ⚡ performance | 최적화, 속도 | "performance", "optimization", "slow" | `--persona-performance` |
| 🔍 analyzer | 디버깅, 조사 | "analyze", "debug", "investigate" | `--persona-analyzer` |
| 🧪 qa | 테스트, 품질 | "test", "quality", "validation" | `--persona-qa` |
| 🔄 refactorer | 코드 정리, 리팩터링 | "refactor", "cleanup", "quality" | `--persona-refactorer` |
| 🚀 devops | 배포, 인프라 | "deploy", "infrastructure", "CI/CD" | `--persona-devops` |
| 👨‍🏫 mentor | 학습, 설명 | "explain", "learn", "understand" | `--persona-mentor` |
| ✍️ scribe | 문서화, 글쓰기 | "document", "write", "guide" | `--persona-scribe` |

### 가장 유용한 조합

**보안 중심 개발**:
```bash
--persona-security --focus security --validate
```

**성능 최적화**:
```bash
--persona-performance --focus performance --benchmark
```

**학습과 이해**:
```bash
--persona-mentor --verbose --explain
```

**품질 개선**:
```bash
--persona-refactorer --focus quality --safe-mode
```

**전문 문서화**:
```bash
--persona-scribe --type guide --detailed
```

### 자동 활성화 트리거

**강력한 트리거** (대부분 잘 작동):
- "security audit" → 🛡️ security
- "UI component" → 🎨 frontend  
- "API design" → ⚙️ backend
- "system architecture" → 🏗️ architect
- "debug issue" → 🔍 analyzer

**중간 트리거** (종종 작동):
- "improve performance" → ⚡ performance
- "write tests" → 🧪 qa
- "clean up code" → 🔄 refactorer
- "deployment issue" → 🚀 devops

**상황 의존적 트리거** (다양함):
- "document this" → ✍️ scribe 또는 👨‍🏫 mentor (청중에 따라)
- "analyze this" → 🔍 analyzer, 🏗️ architect, 또는 도메인 전문가 (내용에 따라)

## 페르소나 문제 해결 🚨

### 일반적인 문제들

**"잘못된 페르소나가 활성화됨"**
- 명시적 페르소나 플래그 사용: `--persona-security`
- 키워드가 자동 활성화를 트리거했는지 확인
- 요청에서 더 구체적인 언어 사용

**"페르소나가 작동하지 않는 것 같음"**
- 페르소나 이름 철자 확인: `--persona-frontend` (틀림: `--persona-fronted`)
- 일부 페르소나는 특정 명령과 더 잘 작동
- 관련 플래그와 결합: `--focus security --persona-security`

**"여러 관점을 원함"**
- 다른 페르소나로 같은 명령 수동 실행
- 더 넓은 포커스 플래그 사용: `--focus quality` (여러 페르소나 활성화)
- 복잡한 요청으로 SuperClaude가 자동 조정하도록

**"페르소나가 너무 집중됨"**
- 더 일반적인 다른 페르소나 시도
- 더 넓은 설명을 위해 멘토 페르소나 사용
- 더 많은 맥락을 위해 `--verbose`와 결합

### 언제 자동 활성화를 오버라이드할까

**오버라이드하는 경우**:
- 자동 활성화가 잘못된 전문가를 선택했을 때
- 다른 관점에서 학습하고 싶을 때
- 일반적인 도메인 경계 밖에서 작업할 때
- 엣지 케이스에 특정 전문성이 필요할 때

**효과적인 오버라이드 방법**:
```bash
# 특정 관점 강제
/sc:analyze frontend-code/ --persona-security  # 프론트엔드의 보안적 관점

# 여러 관점 결합
/sc:analyze api/ --persona-security
/sc:analyze api/ --persona-performance  # 다른 관점을 위해 별도 실행

# 일반적 분석 사용
/sc:analyze --no-persona  # 페르소나 자동 활성화 비활성화
```

## 효과적인 페르소나 사용 팁 💡

### 시작하기 (정직한 방법)
1. **처음에는 페르소나를 완전히 무시하세요** - 자동 활성화가 모든 것을 처리합니다
2. **기본 명령을 정상적으로 사용하세요** - `/analyze`, `/build`, `/improve`는 페르소나 지식 없이도 잘 작동합니다
3. **무엇이 일어나는지 관찰하세요** - 자연스럽게 다른 유형의 전문성이 나타나는 것을 보게 됩니다
4. **자동화를 신뢰하세요** - SuperClaude는 수동 선택보다 더 나은 전문가를 선택합니다

### 고급 사용 (원한다면)
1. **수동 오버라이드 실험** - 프론트엔드 코드에 `--persona-security`를 시도해서 다른 관점 보기
2. **팀 멤버들 알아가기** - 궁금할 때 개별 페르소나에 대해 읽어보기
3. **페르소나 조합 관찰** - 여러 전문가가 복잡한 문제에서 어떻게 협력하는지 보기
4. **학습에 사용** - 같은 질문을 다른 페르소나에게 물어서 다른 접근법 보기

### 모범 사례 (간단하게 유지)
- **자동 활성화가 먼저 작동하도록** - 다른 관점을 원할 때만 오버라이드
- **과도하게 생각하지 마세요** - 필요할 때 올바른 전문가가 나타납니다
- **실험에 사용** - 학습을 위해 같은 문제에 다른 페르소나 시도
- **지능을 신뢰하세요** - 자동 활성화는 계속 개선되고 있습니다

---

## 마지막 참고사항 📝

**페르소나에 대한 진실** 💯:
- **자동 활성화는 대부분 꽤 잘 작동합니다** 직접 전문가를 선택하는 것과 비교해서
- **이 가이드를 완전히 무시해도 됩니다** 그래도 도움이 되는 전문가 지원을 받을 수 있습니다
- **페르소나는 여러분을 돕기 위해 존재합니다** - 관리해야 할 복잡성을 만들기 위해서가 아닙니다
- **학습은 사용을 통해 자연스럽게 일어납니다** 페르소나 설명을 공부하는 것이 아니라 😊

**팀에 압도되지 마세요** 🧘‍♂️:
- 각 페르소나가 무엇을 하는지 알 필요가 없습니다
- SuperClaude는 전문가 선택을 합리적으로 잘 처리합니다
- 위의 자세한 설명은 호기심을 위한 것이지 필수가 아닙니다
- 자동 활성화가 작동하도록 두는 것으로도 놓치는 것이 없습니다

**언제 수동으로 페르소나를 선택할까**:
- **호기심** - "보안 전문가가 이 프론트엔드 코드를 어떻게 생각할까?"
- **학습** - "다른 전문가들이 이 문제에 어떻게 접근할까?"
- **실험** - "성능 렌즈로 이것을 보자"
- **오버라이드** - "이 작은 유틸리티 함수에 아키텍처 조언을 원한다"

**간단하게 유지하세요** 🎯:
- `/analyze some-code/`와 같은 일반적인 명령 사용
- 올바른 전문가가 자동으로 나타나도록 두기
- 수동 페르소나 제어는 필요해서가 아니라 원할 때 사용 가능
- 누가 도와주는지 관리하는 것이 아니라 작업에 집중

---

*11명의 전문가가 있다는 겉보기 복잡성 뒤에, SuperClaude는 사용하기 간단하려고 노력합니다. 그냥 코딩을 시작하면 필요할 때 도움이 되는 전문가들이 보통 나타납니다! 🚀*