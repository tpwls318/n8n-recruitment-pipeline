# 🤖 AI 채용 공고 자동 큐레이션 & 브리핑 파이프라인

원티드(Wanted)·리멤버(Remember)의 프론트엔드 및 AI 엔지니어 채용 공고를 무인 수집하고, LLM 배치(Batch) 분석으로 적합도를 평가한 뒤 **Slack·Telegram으로 실시간 브리핑**하는 24/7 자동화 파이프라인입니다.

---

## 🏗️ 시스템 아키텍처 (Workflow)

```mermaid
flowchart LR
    A[⏰ Schedule Trigger<br/>주기적 자동 실행] --> B1[원티드 API/크롤링<br/>채용 공고 수집]
    A --> B2[리멤버 API/크롤링<br/>채용 공고 수집]
    B1 --> C[🔀 Merge & Deduplicate<br/>이종 데이터 스키마 정규화 및 중복 제거]
    B2 --> C
    C --> D[📦 Batch Aggregator<br/>다건 공고 단일 페이로드 병합]
    D --> E[🧠 Gemini API<br/>직무 적합도 스코어링 & 핵심 요약]
    E --> F{점수 필터링<br/>기준치 이상 선별}
    F -->|Pass| G1[💬 Slack Webhook<br/>채널 브리핑 카드 발송]
    F -->|Pass| G2[✈️ Telegram Bot<br/>개인 알림 메시지 발송]
    F -->|Skip| H[로그 기록 및 종료]