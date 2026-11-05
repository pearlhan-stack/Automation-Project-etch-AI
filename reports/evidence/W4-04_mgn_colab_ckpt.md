# W4-04: MGN 본 학습 체크포인트(Colab) — 아티팩트 기록

- 목적: Colab에서 학습한 MeshGraphNet 체크포인트의 위치·무결성·구조를 기록한다. 체크포인트 파일 자체는 `.gitignore`(`*.pt`)에 따라 Git에 커밋하지 않고 GitHub Release에 첨부한다.
- 학습 환경: Google Colab [확인]. GPU 종류·패키지 버전·학습 시간은 기록되지 않음 [미확인].

## 아티팩트

| 항목 | 값 |
| --- | --- |
| 파일명 | `ckpt_mgn.pt` |
| 크기 | 1,053,775 bytes (약 1.0 MB) [확인] |
| SHA-256 | `1537d9495f7849596611be7de2c23eeec177eda2b32fc69acf75bfc18af5e008` [확인] |
| 위치 | GitHub Release [`mgn-v0.1`](https://github.com/pearlhan-stack/Automation-Project-etch-AI/releases/tag/mgn-v0.1) (pre-release) [확인] |
| 다운로드 | https://github.com/pearlhan-stack/Automation-Project-etch-AI/releases/download/mgn-v0.1/ckpt_mgn.pt |

Release API가 보고한 첨부 파일의 크기(1,053,775)·digest(`sha256:1537d949...f5e008`)가 로컬 파일과 일치함 [확인].

검증: `sha256sum ckpt_mgn.pt` 출력이 위 값과 같아야 한다. 사용할 때는 `runs/<run_id>/checkpoint.pt`로 둔다.

## 체크포인트 내용 [확인]

torch 없이 `zipfile` + 제한된 `pickle.Unpickler`(클래스 로드 차단)로 키와 텐서 shape만 읽었다. 가중치 값 자체는 검사하지 않았다.

- 최상위 키: `state_dict`, `mu_n`, `sd_n`, `mu_e`, `sd_e`(노드·엣지 정규화 통계), `epoch`
- `epoch`: 5
- `state_dict`: 텐서 118개, 파라미터 251,586개
- 구조(shape로 역산):
  - `node_encoder` 입력 14 → hidden 64, `edge_encoder` 입력 3 → hidden 64
  - `node_decoder` 출력 2
  - `processor_layers` 12개(edge/node 블록 교대) → `processor_size=6`
- design C3 모델 A 규약(`input_dim_nodes=14, input_dim_edges=3, output_dim=2`)과 일치. 크기는 W4-04 축소 모델(`processor_size=6, hidden=64`)과 일치.

## 미기록 항목 [미확인]

- 학습 설정(YAML), 시드, 코드 SHA, `dataset_hash` — W4-01 규약(`runs/<run_id>/`)의 메타데이터가 체크포인트에 없음
- 손실 곡선, 에폭당 시간, val 지표
- `epoch=5`가 최종 에폭인지, 중간 저장인지

위 항목이 채워지기 전까지 W4-04 체크박스는 바꾸지 않는다.
