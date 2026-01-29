# Seminar Slides

학술 세미나 발표 자료 제작을 위한 Claude Code Skills 모음.

## 사용법

### 1. 레포지토리 클론

```bash
git clone https://github.com/Mephisto405/seminar-slides.git
cd seminar-slides
```

### 2. Claude Code에서 사용

프로젝트 폴더에서 Claude Code 실행 후:

```
/pptx-skill [논문 arXiv ID] [발표 주제]
```

또는 직접 요청:

```
"arXiv:2512.03052 LATTICE 논문으로 Weekly Paper Review 발표 자료 만들어줘"
```

## 핵심 원칙

### 1. 시각화의 본질

> **"텍스트를 도형으로 바꾸는 것은 시각화가 아니다"**

| 상황 | 권장 방법 |
|------|----------|
| 아키텍처 설명 | 논문 원본 Figure 사용 |
| 결과 비교 | 실제 결과 이미지 비교 |
| 플로우 설명 | 논문 Figure + 주석 |

**Figure 우선순위:**
```
1순위: 논문 원본 Figure (arxiv.org/html/[ID]/x[N].png)
2순위: 프로젝트 페이지 이미지
3순위: 논문 PDF에서 추출
4순위: 직접 그리기 (최후의 수단)
```

### 2. 발표자 관점

> **"슬라이드만 보고도 무슨 말을 해야 할지 알 수 있어야 한다"**

- 슬라이드에 적힌 글, 표, 그림, 화살표만으로 발표 흐름이 보여야 함
- 별도 스크립트 없이도 자연스럽게 설명할 수 있는 구조
- 키워드와 시각 자료가 발표자의 "큐카드" 역할

```
❌ 나쁜 예: 슬라이드에 "Method" 라고만 적혀있음
   → 발표자가 스크립트를 외워야 함

✅ 좋은 예: "Method: 3단계 파이프라인" + Figure + 각 단계 키워드
   → 슬라이드가 발표 가이드 역할
```

### 3. 청중 관점

> **"슬라이드는 논문이 아니다"**

- **과도한 텍스트 금지**: 슬라이드에 글이 많으면 논문 읽는 것과 다를 바 없음
- **작은 글씨 금지**: 뒷자리에서도 읽을 수 있어야 함 (최소 16pt 권장)
- **한 슬라이드 한 메시지**: 여러 개념을 한 슬라이드에 우겨넣지 않기

```
❌ 나쁜 예:
   - 슬라이드 가득 채운 bullet points
   - 10pt 글씨로 빼곡한 설명
   - "자세한 내용은 논문 참고" 느낌

✅ 좋은 예:
   - 핵심 키워드 3-5개
   - 나머지는 Figure로 대체
   - 청중이 "아, 이게 핵심이구나" 즉시 파악
```

### 요약: 좋은 슬라이드 체크리스트

| 체크 항목 | 기준 |
|----------|------|
| 발표자 | 스크립트 없이 슬라이드만 보고 설명 가능? |
| 청중 | 5초 안에 핵심 메시지 파악 가능? |
| 글씨 크기 | 뒷자리에서도 읽을 수 있음? (최소 16pt) |
| 텍스트 양 | bullet point 5개 이하? |
| 시각 자료 | 논문 Figure 또는 결과 이미지 포함? |

## 디렉토리 구조

```
seminar-slides/
├── .claude/
│   └── skills/
│       ├── pptx-skill/          # PowerPoint 생성 스킬
│       │   ├── SKILL.md
│       │   ├── python-pptx-academic.md
│       │   ├── scripts/
│       │   └── ooxml/
│       └── design-skill/        # HTML 슬라이드 디자인 스킬
│           └── SKILL.md
└── examples/
    ├── LATTICE_presentation.pptx  # 논문 발표 예시
    └── lecture_8.pdf              # 강의 슬라이드 예시
```

## 의존성

### Python
```bash
pip install python-pptx pillow pywin32
```

### Node.js (HTML → PPTX 변환 시)
```bash
npm install pptxgenjs playwright sharp
```

## 예시

### examples/LATTICE_presentation.pptx
LATTICE 논문 (arXiv:2512.03052) Weekly Paper Review 발표 자료

- 논문 원본 Figure 적극 활용
- 2분할 레이아웃 (텍스트 | 이미지)
- 결과 비교는 실제 이미지로

### examples/lecture_8.pdf
Stanford CS231n Lecture 8 슬라이드

- 깔끔한 레이아웃의 좋은 예시
- Figure 중심 설명
- 적절한 텍스트 양

## 상세 문서

- [pptx-skill/SKILL.md](.claude/skills/pptx-skill/SKILL.md) - 메인 가이드
- [python-pptx-academic.md](.claude/skills/pptx-skill/python-pptx-academic.md) - 학술 발표 전용 가이드
