# 041 — 결과

## 머지된 것

| PR | 머지 커밋 | 내용 |
| --- | --- | --- |
| [#4274](https://github.com/lidge-jun/opencodex/pull/4274) | `1da8dae96` | Zen 프리셋 안정화 (`noJsonSchemaModels`, Go 예산/사다리 가드) |
| [#4282](https://github.com/lidge-jun/opencodex/pull/4282) | `c1ce2560e` | DeepSeek V4.1 전환 전체 |
| [#4258](https://github.com/lidge-jun/opencodex/pull/4258) | — | `#4282`로 캐리 후 종료. 원저자 커밋과 `Co-authored-by` 유지 |

## 이 유닛이 배운 것

**삭제가 제거가 아닌 경우가 있다.** `liveModels` 프로바이더는 정적 행을 지워도 모델이 `/models`로 다시 올라온다. 지워지는 건 컨텍스트 창·사다리·text-only 힌트뿐이라, 결과는 제거가 아니라 능력을 잃은 모델이 계속 보이는 퇴행이다. 실제 제거는 `ROUTED_MODEL_COMPATIBILITY_EXCLUSIONS`가 한다. 독립 감사가 아니었으면 그대로 퇴행을 실어 보냈을 것이다.

**공유 상수는 파생 지점을 먼저 세야 한다.** 첫 설계는 `DEEPSEEK_THINKING_MODELS`에 V4.1을 얹으려 했는데, 그 상수는 `deepseek` 프리셋의 모델 맵 다섯 개와 `models:` 배열까지 먹이고 있었다. 두 번째 설계는 상수를 레거시 전용으로 고정하려 했고, 그러면 신규 id가 사다리와 replay를 전부 잃는다. 감사가 `fail`을 낸 뒤에야 세 갈래 파생이 나왔다.

**CI가 마지막 감사자였다.** 로컬 포커스 테스트 254건이 통과한 뒤에도 CI가 8개 파일을 더 잡았다. 전체 스위트를 로컬에서 돌리지 않기로 한 결정의 대가이자, 그 결정이 작동한 방식이기도 하다.

## 남긴 것

- `scripts/model-metadata.source.json`에 `deepseek-flash`와 `deepseek-v4.1-flash` 행이 없어 두 id의 비용 추정이 빈다. 다음 메타데이터 생성에서 채워진다.
- `opencode-free`는 게이트웨이 상수를 `noJsonSchemaModels`에서만 쓰므로 신규 id가 사다리와 replay를 못 받는다. 기존 V4 id도 같은 비대칭이다.
- PR #4258 리뷰에서 지적한 Qwen 사다리 근거 문제는 후속으로 남았다.
