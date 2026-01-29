# Python-pptx 학술 프레젠테이션 가이드

HTML → PPTX 변환은 표, 이미지, 복잡한 레이아웃에서 문제가 발생할 수 있습니다.
**학술 발표**나 **복잡한 슬라이드**는 python-pptx를 직접 사용하는 것을 권장합니다.

---

## ⚠️ 가장 중요한 원칙: 논문 Figure 활용

> **"도형으로 새로 그리지 말고, 논문의 원본 Figure를 직접 활용하라"**

### 왜 중요한가?

```
❌ 나쁜 접근: 텍스트 → 도형/플로우차트로 변환
   - add_shape()로 박스와 화살표를 그려서 "시각화"
   - 결과: "도형화된 텍스트"일 뿐, 진정한 시각화가 아님

✅ 좋은 접근: 텍스트 → 논문 원본 Figure + 주석
   - 저자가 이미 잘 만들어놓은 Figure 활용
   - 실제 결과 이미지로 "눈으로 보여주기"
   - 결과: 청중이 직관적으로 이해
```

### 실제 비교 예시

| 항목 | 나쁜 예 (도형 기반) | 좋은 예 (Figure 기반) |
|------|---------------------|----------------------|
| 아키텍처 설명 | `add_shape()`로 박스+화살표 | 논문 Figure 1 이미지 + 빨간 주석 |
| 결과 비교 | "FID: 44.2 → 34.2 (개선)" 텍스트 | 실제 3D 모델 렌더링 비교 이미지 |
| 방법 비교 | O/X 텍스트 테이블 | 실제 출력물 비교 이미지 + 😢/😀 |

### Figure 활용 우선순위

```
1순위: 논문 원본 Figure (arxiv.org/html/[ID]/x[N].png)
2순위: 프로젝트 페이지 이미지 ([method].github.io)
3순위: 논문 PDF에서 추출
4순위: 직접 도형으로 그리기 (최후의 수단)
```

---

## 핵심 체크리스트

### 발표 생성 전 확인사항
1. **저자 정보**: arXiv/논문에서 정확한 저자명 확인
2. **이벤트 유형**: PhD Seminar, Weekly Paper Review, Conference 등 확인
3. **청중 파악**: 전문가 수준, 배경 지식 파악
4. **필요 이미지**: 논문의 Figure, 프로젝트 페이지 이미지 URL 수집

### 이미지 소스
논문 이미지는 다음에서 다운로드:
- `https://arxiv.org/html/[PAPER_ID]` - HTML 버전의 고화질 Figure
- 프로젝트 페이지 (보통 `[method].github.io`)
- 논문 PDF에서 추출

### 슬라이드별 필수 이미지 (학술 발표)

| 슬라이드 | 필수 이미지 | 소스 |
|----------|-------------|------|
| 타이틀 | Teaser/Main Figure | 프로젝트 페이지 |
| Why This Paper | Scaling Law 그래프, 기존 방법 한계 | 논문 Figure |
| Background | 기존 방법 아키텍처 | 관련 논문 Figure |
| Method | 제안 방법 아키텍처 | 논문 Figure 1-2 |
| Results | **실제 결과 비교 이미지** | 논문 Figure (가장 중요!) |
| Takeaway | Method Figure 재활용 + 주석 | 논문 Figure |

---

## 권장 레이아웃 패턴

### 패턴 1: 2분할 레이아웃 (가장 효과적)
```
┌─────────────────┬─────────────────┐
│                 │                 │
│   텍스트 설명    │   논문 Figure   │
│   (bullet points)│   (이미지)      │
│                 │                 │
└─────────────────┴─────────────────┘
```

### 패턴 2: 비교 레이아웃 (결과 슬라이드)
```
┌─────────────────┬─────────────────┐
│   Baseline 😢   │   Ours 😀       │
│   [결과 이미지]  │   [결과 이미지]  │
├─────────────────┴─────────────────┤
│         핵심 인사이트 박스          │
└───────────────────────────────────┘
```

### 패턴 3: Figure + 주석 레이아웃
```
┌───────────────────────────────────┐
│         논문 원본 Figure           │
│    ↑                    ↑         │
│  [주석1]              [주석2]      │
└───────────────────────────────────┘
```

---

## 기본 설정

### 슬라이드 크기
```python
from pptx import Presentation
from pptx.util import Inches, Pt

prs = Presentation()
prs.slide_width = Inches(13.333)  # 960pt (16:9)
prs.slide_height = Inches(7.5)    # 540pt
```

### 색상 정의
```python
from pptx.dml.color import RGBColor

# 학술 발표 기본 팔레트
DARK_BG = RGBColor(0x1e, 0x29, 0x3b)      # #1e293b - 다크 배경
WHITE = RGBColor(0xff, 0xff, 0xff)
BLUE = RGBColor(0x25, 0x63, 0xeb)          # #2563eb - Ours/강조
RED = RGBColor(0xdc, 0x26, 0x26)           # #dc2626 - Baseline/문제
GREEN = RGBColor(0x16, 0xa3, 0x4a)         # #16a34a - 해결책
GRAY = RGBColor(0x64, 0x74, 0x8b)          # #64748b - 보조 텍스트
LIGHT_GRAY = RGBColor(0x94, 0xa3, 0xb8)    # #94a3b8

# 배경색
LIGHT_BG = RGBColor(0xf8, 0xfa, 0xfc)      # 밝은 회색
LIGHT_BLUE_BG = RGBColor(0xef, 0xf6, 0xff) # 연한 파랑
LIGHT_GREEN_BG = RGBColor(0xf0, 0xfd, 0xf4) # 연한 초록
LIGHT_RED_BG = RGBColor(0xfe, 0xf2, 0xf2)  # 연한 빨강
```

---

## 핵심 함수

### 텍스트 박스 추가
```python
from pptx.enum.text import PP_ALIGN

def add_text_box(slide, left, top, width, height, text,
                 font_size=18, bold=False, color=DARK_BG,
                 align=PP_ALIGN.LEFT, font_name="Arial"):
    txBox = slide.shapes.add_textbox(left, top, width, height)
    tf = txBox.text_frame
    tf.word_wrap = True
    p = tf.paragraphs[0]
    p.text = text
    p.font.size = Pt(font_size)
    p.font.bold = bold
    p.font.color.rgb = color
    p.font.name = font_name
    p.alignment = align
    return txBox
```

### 테이블 추가
```python
def add_table(slide, left, top, width, height, rows, cols, data,
              header_color=DARK_BG, cell_color=DARK_BG):
    table = slide.shapes.add_table(rows, cols, left, top, width, height).table

    col_width = width // cols
    for i in range(cols):
        table.columns[i].width = col_width

    for row_idx, row_data in enumerate(data):
        for col_idx, cell_text in enumerate(row_data):
            cell = table.cell(row_idx, col_idx)
            cell.text = str(cell_text)

            para = cell.text_frame.paragraphs[0]
            para.font.size = Pt(14)
            para.font.name = "Arial"
            para.alignment = PP_ALIGN.CENTER

            if row_idx == 0:  # Header
                para.font.bold = True
                para.font.color.rgb = WHITE
                cell.fill.solid()
                cell.fill.fore_color.rgb = header_color
            else:
                para.font.color.rgb = cell_color
                cell.fill.solid()
                cell.fill.fore_color.rgb = WHITE

    return table
```

### 이미지 추가
```python
import os

def add_image(slide, img_path, left, top, width=None, height=None):
    """이미지 추가 (width나 height 중 하나만 지정하면 비율 유지)"""
    if os.path.exists(img_path):
        if width and height:
            slide.shapes.add_picture(img_path, left, top, width=width, height=height)
        elif width:
            slide.shapes.add_picture(img_path, left, top, width=width)
        elif height:
            slide.shapes.add_picture(img_path, left, top, height=height)
        else:
            slide.shapes.add_picture(img_path, left, top)
        return True
    return False
```

---

## 슬라이드 템플릿

### 타이틀 슬라이드
```python
def create_title_slide(prs, title, authors, presenter, date, event, arxiv_id, img_path=None):
    slide = prs.slides.add_slide(prs.slide_layouts[6])  # Blank

    # 다크 배경
    from pptx.enum.shapes import MSO_SHAPE
    bg = slide.shapes.add_shape(MSO_SHAPE.RECTANGLE, 0, 0, prs.slide_width, prs.slide_height)
    bg.fill.solid()
    bg.fill.fore_color.rgb = DARK_BG
    bg.line.fill.background()

    # arXiv 참조
    add_text_box(slide, Inches(0.8), Inches(0.6), Inches(3), Inches(0.4),
                 f"arXiv:{arxiv_id}", font_size=14, color=LIGHT_GRAY)

    # 제목 (이미지가 있으면 좌측에, 없으면 전체 너비)
    title_width = Inches(7) if img_path else Inches(11)
    add_text_box(slide, Inches(0.8), Inches(1.5), title_width, Inches(1.5),
                 title, font_size=36, bold=True, color=WHITE)

    # 저자
    add_text_box(slide, Inches(0.8), Inches(3.8), Inches(7), Inches(0.6),
                 authors, font_size=12, color=LIGHT_GRAY)

    # 이미지 (오른쪽)
    if img_path and os.path.exists(img_path):
        slide.shapes.add_picture(img_path, Inches(8), Inches(1.2), width=Inches(4.8))

    # 발표 정보
    add_text_box(slide, Inches(0.8), Inches(5.5), Inches(6), Inches(0.3),
                 f"발표: {presenter} | 날짜: {date}", font_size=16, color=WHITE)
    add_text_box(slide, Inches(0.8), Inches(5.9), Inches(6), Inches(0.3),
                 event, font_size=14, color=GRAY)
```

### 결과 비교 슬라이드 (이미지 포함)
```python
def create_results_slide(prs, title, left_img, right_img, summary_text, page_num, total):
    slide = prs.slides.add_slide(prs.slide_layouts[6])

    # 헤더
    add_text_box(slide, Inches(0.8), Inches(0.5), Inches(10), Inches(0.6),
                 title, font_size=32, bold=True, color=DARK_BG)
    add_text_box(slide, Inches(12), Inches(0.5), Inches(1), Inches(0.4),
                 f"{page_num} / {total}", font_size=14, color=LIGHT_GRAY, align=PP_ALIGN.RIGHT)

    # 이미지 (높이 제한으로 겹침 방지)
    if os.path.exists(left_img):
        slide.shapes.add_picture(left_img, Inches(0.8), Inches(1.5), height=Inches(3.5))
    if os.path.exists(right_img):
        slide.shapes.add_picture(right_img, Inches(6.8), Inches(1.5), height=Inches(3.5))

    # 요약 박스 (하단)
    from pptx.enum.shapes import MSO_SHAPE
    box = slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE,
                                  Inches(0.8), Inches(5.3), Inches(11.5), Inches(1.2))
    box.fill.solid()
    box.fill.fore_color.rgb = LIGHT_GREEN_BG
    box.line.fill.background()

    add_text_box(slide, Inches(1.0), Inches(5.5), Inches(11), Inches(0.8),
                 summary_text, font_size=14, color=DARK_BG)
```

---

## 검증 워크플로우

### PowerPoint COM을 이용한 슬라이드 내보내기
```python
import win32com.client
import time

def export_slides_to_png(pptx_path, output_dir):
    """PPTX를 PNG로 내보내기 (시각적 검증용)"""
    import os
    os.makedirs(output_dir, exist_ok=True)

    ppt = win32com.client.Dispatch('PowerPoint.Application')
    ppt.Visible = True

    presentation = ppt.Presentations.Open(os.path.abspath(pptx_path))
    time.sleep(2)

    for i in range(1, presentation.Slides.Count + 1):
        slide = presentation.Slides(i)  # 1-based index
        output_path = os.path.join(output_dir, f'slide_{i:02d}.png')
        slide.Export(output_path, 'PNG', 960, 540)
        print(f'Exported: slide_{i:02d}.png')

    presentation.Close()
    ppt.Quit()
```

### 검증 체크리스트
- [ ] 텍스트 겹침 없음
- [ ] 이미지가 박스/텍스트와 겹치지 않음
- [ ] 테이블이 올바르게 렌더링됨
- [ ] 저자명, 날짜, 이벤트 정보 정확함
- [ ] 페이지 번호 일관성

---

## 흔한 실수와 해결책

### 1. 텍스트 겹침
**문제**: 제목과 저자명이 겹침
**해결**: 요소 간 Y 좌표 간격 최소 0.5인치 확보

### 2. 이미지 겹침
**문제**: 이미지가 하단 요약 박스와 겹침
**해결**: width 대신 height로 이미지 크기 제한
```python
# 나쁜 예: width만 지정하면 세로로 길어질 수 있음
slide.shapes.add_picture(img, left, top, width=Inches(5))

# 좋은 예: height 제한
slide.shapes.add_picture(img, left, top, height=Inches(3.5))
```

### 3. 잘못된 인덱싱
**문제**: PowerPoint COM에서 슬라이드 접근 시
**해결**: PowerPoint는 1-based index 사용
```python
# 잘못된 예 (Python list처럼 사용)
slide = presentation.Slides[i]

# 올바른 예
slide = presentation.Slides(i)  # 1부터 시작
```

### 4. 색상 코드
**문제**: RGBColor에 #이 포함됨
**해결**: # 없이 16진수 값만 사용
```python
# 잘못된 예
RGBColor('#1e293b')

# 올바른 예
RGBColor(0x1e, 0x29, 0x3b)
```

### 5. 인코딩 오류 (Windows)
**문제**: 유니코드 문자 (✓, ✗) 출력 시 cp949 오류
**해결**: ASCII 대체 또는 print 제거
```python
# 나쁜 예
print("Download complete ✓")

# 좋은 예
print("Download complete")
# 또는
print("Download complete [OK]")
```

### 6. 도형으로 "시각화" 시도 (가장 흔한 실수)
**문제**: 텍스트를 도형과 화살표로 변환하면 "시각적"이라고 착각
**현실**: 도형화된 텍스트일 뿐, 진정한 시각화가 아님

```python
# ❌ 나쁜 예: 도형으로 플로우차트 그리기
slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE, ...)  # "Input"
slide.shapes.add_shape(MSO_SHAPE.RIGHT_ARROW, ...)
slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE, ...)  # "Process"
slide.shapes.add_shape(MSO_SHAPE.RIGHT_ARROW, ...)
slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE, ...)  # "Output"
# 결과: 텍스트 박스 3개 + 화살표 2개 = 여전히 텍스트

# ✅ 좋은 예: 논문 Figure 직접 활용
slide.shapes.add_picture('images/paper_figure_1.png', ...)
# 결과: 저자가 공들여 만든 시각 자료 그대로 활용
```

### 7. 결과를 숫자로만 설명
**문제**: "FID: 44.2 → 34.2 (15% 개선)" - 숫자만으로는 와닿지 않음
**해결**: 실제 결과 이미지 비교 + 이모지 피드백

```python
# ❌ 나쁜 예
add_text_box(slide, ..., "VecSet: FID 44.2\nVoxSet: FID 34.2 (15% 개선)")

# ✅ 좋은 예
# 왼쪽: VecSet 결과 이미지 + 😢
slide.shapes.add_picture('images/vecset_result.png', Inches(0.5), Inches(1.5))
add_text_box(slide, Inches(2.5), Inches(1.0), ..., "VecSet 😢")

# 오른쪽: VoxSet 결과 이미지 + 😀
slide.shapes.add_picture('images/voxset_result.png', Inches(7), Inches(1.5))
add_text_box(slide, Inches(9), Inches(1.0), ..., "VoxSet (Ours) 😀")
```

---

## 이미지 다운로드

### 논문 이미지 수집 스크립트
```python
import urllib.request
import ssl
import os

ssl._create_default_https_context = ssl._create_unverified_context

def download_paper_images(paper_id, project_url=None):
    """논문 이미지 다운로드"""
    os.makedirs('images', exist_ok=True)

    # arXiv HTML 버전 이미지
    arxiv_base = f'https://arxiv.org/html/{paper_id}'

    images = {
        'figure_1.png': f'{arxiv_base}/x1.png',
        'figure_2.png': f'{arxiv_base}/x2.png',
        # 필요한 Figure 번호 추가
    }

    # 프로젝트 페이지 이미지 (선택)
    if project_url:
        images['main.png'] = f'{project_url}/statics/images/teaser.png'

    for filename, url in images.items():
        filepath = f'images/{filename}'
        try:
            print(f'Downloading: {filename}')
            urllib.request.urlretrieve(url, filepath)
            print(f'  -> OK: {os.path.getsize(filepath)} bytes')
        except Exception as e:
            print(f'  -> FAILED: {str(e)[:50]}')
```

---

## 참고: 학술 발표 슬라이드 구성

### 표준 구성 (15-20장)
1. 타이틀 (논문명, 저자, 발표자)
2. Why This Paper? (동기)
3. Agenda
4-6. Background (기존 방법, 문제점)
7-8. Method (제안 방법)
9. Architecture
10-11. Results (정량적/정성적)
12-14. Takeaways (3개)
15. Limitations
16. Summary / Q&A

### 청중별 조정
- **전문가**: 기술적 세부사항 강조
- **비전문가**: 직관적 설명, 시각 자료 많이
- **혼합 청중**: 점진적 깊이 (쉬운 것 → 어려운 것)

---

## 실전 워크플로우: Figure 중심 슬라이드 제작

### Step 1: 논문 Figure 수집 (가장 먼저!)

```python
import urllib.request
import os

os.makedirs('images', exist_ok=True)

# arXiv HTML 버전에서 Figure 다운로드
paper_id = "2512.03052"  # 예: LATTICE 논문
figures = {
    'teaser.png': f'https://arxiv.org/html/{paper_id}/x1.png',
    'architecture.png': f'https://arxiv.org/html/{paper_id}/x2.png',
    'results.png': f'https://arxiv.org/html/{paper_id}/x3.png',
    'comparison.png': f'https://arxiv.org/html/{paper_id}/x4.png',
}

for name, url in figures.items():
    try:
        urllib.request.urlretrieve(url, f'images/{name}')
        print(f'Downloaded: {name}')
    except:
        print(f'Failed: {name} - 수동 다운로드 필요')
```

### Step 2: 2분할 레이아웃 템플릿 활용

```python
def create_figure_slide(prs, title, bullet_points, figure_path, page_num, total):
    """Figure 중심 2분할 슬라이드"""
    slide = prs.slides.add_slide(prs.slide_layouts[6])

    # 제목
    add_text_box(slide, Inches(0.5), Inches(0.3), Inches(12), Inches(0.6),
                 title, font_size=28, bold=True)

    # 왼쪽: 텍스트 (간결하게!)
    bullets = "\n".join([f"• {p}" for p in bullet_points])
    add_text_box(slide, Inches(0.5), Inches(1.2), Inches(5.5), Inches(5),
                 bullets, font_size=16, color=DARK_BG)

    # 오른쪽: 논문 Figure (핵심!)
    if os.path.exists(figure_path):
        slide.shapes.add_picture(figure_path, Inches(6.5), Inches(1.0),
                                  width=Inches(6.3))

    # 페이지 번호
    add_text_box(slide, Inches(12.3), Inches(0.3), Inches(0.8), Inches(0.4),
                 f"{page_num}/{total}", font_size=12, color=GRAY)

    return slide
```

### Step 3: 결과 비교 슬라이드 (이미지 중심)

```python
def create_comparison_slide(prs, title, left_img, right_img,
                            left_label, right_label, insight):
    """결과 비교 슬라이드 - 이미지가 주인공!"""
    slide = prs.slides.add_slide(prs.slide_layouts[6])

    # 제목
    add_text_box(slide, Inches(0.5), Inches(0.3), Inches(12), Inches(0.5),
                 title, font_size=28, bold=True)

    # 왼쪽 이미지 + 라벨 (😢)
    if os.path.exists(left_img):
        slide.shapes.add_picture(left_img, Inches(0.5), Inches(1.2),
                                  width=Inches(5.8))
    add_text_box(slide, Inches(0.5), Inches(0.85), Inches(5.8), Inches(0.35),
                 f"{left_label} 😢", font_size=16, bold=True, color=RED)

    # 오른쪽 이미지 + 라벨 (😀)
    if os.path.exists(right_img):
        slide.shapes.add_picture(right_img, Inches(6.8), Inches(1.2),
                                  width=Inches(5.8))
    add_text_box(slide, Inches(6.8), Inches(0.85), Inches(5.8), Inches(0.35),
                 f"{right_label} 😀", font_size=16, bold=True, color=GREEN)

    # 하단 인사이트 박스
    box = slide.shapes.add_shape(MSO_SHAPE.ROUNDED_RECTANGLE,
                                  Inches(0.5), Inches(5.8), Inches(12), Inches(1))
    box.fill.solid()
    box.fill.fore_color.rgb = LIGHT_GREEN_BG
    box.line.fill.background()

    add_text_box(slide, Inches(0.7), Inches(6.0), Inches(11.6), Inches(0.6),
                 insight, font_size=15, bold=True, color=DARK_BG)

    return slide
```

### Step 4: 사용 예시

```python
# Method 슬라이드 - Figure 중심
create_figure_slide(
    prs,
    title="LATTICE: 3단계 파이프라인",
    bullet_points=[
        "Stage 1: Coarse Generation",
        "Stage 2: Voxelize & Sampling",
        "Stage 3: Structure-Aware DiT"
    ],
    figure_path="images/architecture.png",  # 논문 Figure!
    page_num=9, total=16
)

# 결과 슬라이드 - 이미지 비교 중심
create_comparison_slide(
    prs,
    title="결과: Training-time Scaling",
    left_img="images/vecset_scaling.png",   # 실제 결과 이미지!
    right_img="images/voxset_scaling.png",  # 실제 결과 이미지!
    left_label="VecSet (0.6B → 3B: 변화 없음)",
    right_label="VoxSet (0.6B → 4.5B: 품질 향상)",
    insight="VoxSet이 3배 이상 효율적인 Scaling Law를 보임"
)
```

---

## 최종 점검: 좋은 학술 발표 슬라이드 기준

### 슬라이드별 이미지 비율 체크

| 슬라이드 유형 | 권장 이미지 비율 | 텍스트 |
|--------------|-----------------|--------|
| 타이틀 | 40-50% | 제목 + 발표자 정보 |
| Background | 50-60% | 핵심 bullet만 |
| Method | **60-70%** | 최소한의 설명 |
| Results | **70-80%** | 숫자보다 이미지 |
| Takeaway | 30-40% | 핵심 메시지 강조 |

### 자가 진단

```
□ 3장 연속 텍스트만 있는 슬라이드가 있는가? → 논문 Figure 추가
□ "숫자로만" 결과를 설명하는 슬라이드가 있는가? → 결과 이미지 추가
□ add_shape()로 그린 플로우차트가 있는가? → 논문 Figure로 교체
□ 청중이 "눈으로" 차이를 볼 수 있는가? → 비교 이미지 추가
```
