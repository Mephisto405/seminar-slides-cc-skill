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

> **"텍스트를 도형으로 바꾸는 것은 시각화가 아니다"**

### 올바른 접근

| 상황 | 권장 방법 |
|------|----------|
| 아키텍처 설명 | 논문 원본 Figure 사용 |
| 결과 비교 | 실제 결과 이미지 비교 |
| 플로우 설명 | 논문 Figure + 주석 |

### Figure 우선순위

```
1순위: 논문 원본 Figure (arxiv.org/html/[ID]/x[N].png)
2순위: 프로젝트 페이지 이미지
3순위: 논문 PDF에서 추출
4순위: 직접 그리기 (최후의 수단)
```

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
    └── LATTICE_presentation.pptx  # 참고 예시
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

`examples/LATTICE_presentation.pptx` 참고.

- 논문 원본 Figure 적극 활용
- 2분할 레이아웃 (텍스트 | 이미지)
- 결과 비교는 실제 이미지로

## 상세 문서

- [pptx-skill/SKILL.md](.claude/skills/pptx-skill/SKILL.md) - 메인 가이드
- [python-pptx-academic.md](.claude/skills/pptx-skill/python-pptx-academic.md) - 학술 발표 전용 가이드
