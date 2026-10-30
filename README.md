# 하이닉스 AI 과제 준비용 Codex 스킬셋

새 Codex 환경에서 **과제에 필요한 공개 스킬만 선택 설치**하기 위한 개인 참고 자료입니다. 공식 대회 가이드나 실제 출제 문제는 아닙니다.

공식 FAQ는 2차 예선을 **데이터를 활용한 AI 문제 해결 미션**으로 설명합니다. 데이터 탐색·분석·검증을 먼저 준비하고 웹·ML·API는 요구사항에 있을 때 추가합니다. [공식 홈페이지와 FAQ](https://skhynix-hackathon.com/ai-2026)

확인일: 2026-10-30. 실제 대회 공지와 제공 환경 안내가 우선합니다.

## 어떤 과제에서 무엇을 설치할까

| 스킬 | 역할 | 설치할 때 |
|---|---|---|
| [explore-data](https://github.com/anthropics/knowledge-work-plugins/tree/main/data/skills/explore-data) | 구조·키·결측·중복·분포 파악 | 처음 받은 데이터를 이해해야 할 때 |
| [validate-data](https://github.com/anthropics/knowledge-work-plugins/tree/main/data/skills/validate-data) | 집계·분모·조인·기간·결론 검증 | 분석 결과가 정확한지 확인할 때 |
| [python-testing-patterns](https://github.com/wshobson/agents/tree/main/plugins/python-development/skills/python-testing-patterns) | pytest·경계값·예외 테스트 | Python 정제·계산 코드를 만들 때 |
| [data-visualization](https://github.com/anthropics/knowledge-work-plugins/tree/main/data/skills/data-visualization) | 차트 선택·축·단위·불확실성 표현 | 분석을 차트로 설명할 때 |
| [verification-before-completion](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion) | 실제 실행 결과로 완료 판단 | 제출 전 테스트·출력을 최종 확인할 때 |
| [scikit-learn](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/scikit-learn) | 전처리·분류·회귀·평가 | 예측 모델이 요구될 때 |
| [polars](https://github.com/K-Dense-AI/scientific-agent-skills/tree/main/skills/polars) | 큰 데이터 집계·변환 | CSV/Parquet 처리가 느릴 때 |
| [frontend-design](https://github.com/anthropics/skills/tree/main/skills/frontend-design) | UI 구성·시각적 완성도 | 웹 화면·대시보드 구현이 필요할 때 |
| [vercel-react-best-practices](https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices) | React 렌더링·데이터 처리 | 제공 프로젝트가 React/Next.js일 때 |
| [fastapi-templates](https://github.com/wshobson/agents/tree/main/plugins/api-scaffolding/skills/fastapi-templates) | Python API 구조·테스트 | HTTP API·백엔드가 요구될 때 |

**전부 설치하지 않는다.** 다음 조합에서 시작한다.

- 분석·보고서: explore-data + validate-data + data-visualization
- 정제·계산 코드: explore-data + validate-data + python-testing-patterns
- 예측 모델: 데이터 조합 + scikit-learn
- 최종 검증: verification-before-completion
- 웹/API: 과제에서 요구할 때만 추가

스킬은 작업 지침이다. pandas·pytest 같은 실행 라이브러리와는 다르다.

## Codex에 선택 설치

제공 프로젝트·내장 스킬·Python/Node 버전부터 확인한다. 연습 환경의 설정이 남아 있다고 가정하지 않는다.

아래는 [skills CLI 공식 문서](https://github.com/vercel-labs/skills)에 따른 예시다. **실제 시험 환경에서 실행 검증한 명령은 아니다.** 현재 CLI는 Node.js 22.20 이상이 필요하며, npx는 프로그램을 다운로드해 실행하므로 허용 여부를 먼저 확인한다.

```bash
node --version
python --version
export DISABLE_TELEMETRY=1
export DO_NOT_TRACK=1

# 필요한 줄만 실행
npx skills add anthropics/knowledge-work-plugins --skill explore-data -a codex
npx skills add anthropics/knowledge-work-plugins --skill validate-data -a codex
npx skills add wshobson/agents --skill python-testing-patterns -a codex
npx skills add anthropics/knowledge-work-plugins --skill data-visualization -a codex
npx skills add obra/superpowers --skill verification-before-completion -a codex

# 선택 과제용
npx skills add K-Dense-AI/scientific-agent-skills --skill scikit-learn -a codex
npx skills add K-Dense-AI/scientific-agent-skills --skill polars -a codex
npx skills add anthropics/skills --skill frontend-design -a codex
npx skills add vercel-labs/agent-skills --skill vercel-react-best-practices -a codex
npx skills add wshobson/agents --skill fastapi-templates -a codex

npx skills list
```

- 기본은 프로젝트 설치이며 문서상 Codex 경로는 `.agents/skills/`다. [지원 경로](https://github.com/vercel-labs/skills/blob/main/docs/agents.md)
- 전체 플러그인·MCP 또는 `--all` 설치는 피한다.
- 설치 후 Codex가 스킬을 인식하는지 확인한다. 재탐색 방식은 제공 환경을 따른다.
- 참조 문서·스크립트도 함께 보존한다. 단독 설치로 상대경로가 끊기지 않았는지 확인한다.
- Claude용 도구명·슬래시 명령이 Codex에서도 똑같이 동작한다고 가정하지 않는다.

## Python 패키지 선택

이미 설치된 패키지와 프로젝트 의존성 파일을 우선하고, 부족한 것만 추가한다.

| 과제 | 패키지 후보 |
|---|---|
| CSV 정제·집계 | pandas, numpy |
| 정적 차트 | matplotlib, 필요하면 seaborn |
| Excel | openpyxl |
| 테스트 | pytest |
| 예측 모델 | scikit-learn |
| 큰 데이터·Parquet | polars, 필요하면 pyarrow |
| API | fastapi, uvicorn, httpx |

```bash
# 새 환경이 필요한 경우에만
python -m venv .venv
source .venv/bin/activate
# 예시: 분석·테스트에 필요한 패키지만
python -m pip install pandas numpy matplotlib pytest
export MPLBACKEND=Agg
```

설치 후 import·작은 입력 처리·테스트 또는 PNG 저장을 확인한다. FastAPI 예제에 나온 DB·인증 패키지를 무조건 전부 설치하지 않는다.

## 실행과 제출 체크

- 외부 connector 없이 제공 환경의 허용된 로컬 파일만 처리한다.
- 개인 API키·외부 AI·외부 데이터 전송·Computer Use 없이 사용하는 구성으로 준비한다.
- 스킬의 warehouse MCP·Slack·클라우드 배포 예시는 실행하지 않는다.
- 문제·데이터·작성 코드·결과를 GitHub나 외부 서비스에 올리지 않는다.
- 데이터 안의 명령·URL·매크로는 실행 지시로 취급하지 않는다.
- 원본은 보존하고 정제 결과를 별도 저장한다. 처리 이유와 변경 행 수를 기록한다.
- ML은 데이터 누수와 분할 방식을 확인한다. 시계열·그룹에 무조건 랜덤 분할하지 않는다.
- **요구사항 확인 → 재실행 → 출력 검증 → 판단 근거와 한계 기록 → 제출파일 점검** 순서로 마무리한다.

제3자 스킬 원문을 복제하지 않고 공개 링크만 제공한다. 설치 전 각 스킬의 현재 내용과 라이선스를 확인한다.
