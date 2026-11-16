# How Text-to-Image Works — 쌓기와 다듬기

**DALL·E는 쌓고, Stable Diffusion은 다듬는다.** 첫 DALL·E(Ramesh 외, ICML 2021)와 Stable Diffusion(Rombach 외, CVPR 2022)의 구조를 **자기회귀(쌓기) vs 확산(다듬기)** 한 가지 차이에 집중해 브라우저에서 직접 비교하는 인터랙티브 교육 자료입니다. 그림을 공간 순서로 한 칸씩 확정하느냐, 전체를 노이즈 수준 순서로 여러 번 고쳐 쓰느냐에서 이산 토큰 vs 연속 잠재, 실수 전파, 텍스트 넣는 자리, 가이던스, 생성 속도의 차이가 모두 따라 나옵니다.

🔗 **[데모 바로가기](https://dev-jonghoonpark.github.io/how-text-to-image-works/)**

## 다루는 내용

1. **같은 목표, 두 가지 분해** · 연쇄 법칙(공간 순서) vs 노이즈 사슬(선명도 순서)
2. **픽셀 대신 압축된 칸 위에서** · dVAE 정수 토큰 32×32 vs VAE 연속 잠재 64×64×4, 왜 그 형태여야 하는가
3. **쌓기** · 16×16 토큰을 래스터 순서로 쌓는 장난감, 실수 하나를 넣으면 뒤로 번지고 고쳐지지 않는 모습
4. **노이즈는 세밀한 것부터 지운다** · 실제 SD 스케줄로 섞은 x_t와 반경 파워 스펙트럼 — 확산이 거친 것부터 그리는 이유
5. **다듬기** · DDIM 샘플러로 노이즈에서 그림까지, 추정 x̂₀가 흐림→선명으로 가는 과정, 초반/후반에 넣은 얼룩의 운명, 같은 그림을 쌓기와 다듬기로 나란히 그리는 진행률 슬라이더와 필름 스트립
6. **텍스트 넣기** · 인과 마스크로 앞에 붙이기 vs 교차 어텐션
7. **텍스트를 더 세게** · CLIP 재순위 vs 분류기 없는 가이던스, 2차원 혼합 분포에서 w를 바꾸며 랑주뱅 샘플링
8. **순차 호출 수** · 1024번 vs 50번, 해상도와 단계 수에 따른 변화
9. **정리 표** · 우도, 인페인팅, img2img까지
10. **그 이후** · DALL·E 2·3은 확산, SD3은 트랜스포머로 확산, Parti·VAR·MAR·GPT-4o의 자기회귀 — 구조와 생성 방식은 별개의 축
11. **퀴즈** 6문항

데모의 신경망은 동작 방식을 보여 주기 위한 가짜 모델입니다(데모 6의 점수 함수만 해석적으로 정확).

## 참고

- Ramesh 외 (2021), *Zero-Shot Text-to-Image Generation* — [arXiv:2102.12092](https://arxiv.org/abs/2102.12092)
- Rombach 외 (2022), *High-Resolution Image Synthesis with Latent Diffusion Models* — [arXiv:2112.10752](https://arxiv.org/abs/2112.10752)
- Ho, Jain & Abbeel (2020), *Denoising Diffusion Probabilistic Models* — [arXiv:2006.11239](https://arxiv.org/abs/2006.11239)
- Song, Meng & Ermon (2021), *Denoising Diffusion Implicit Models* — [arXiv:2010.02502](https://arxiv.org/abs/2010.02502)
- Ho & Salimans (2022), *Classifier-Free Diffusion Guidance* — [arXiv:2207.12598](https://arxiv.org/abs/2207.12598)
- Dieleman (2024), *Diffusion is spectral autoregression* — [블로그](https://sander.ai/2024/09/02/spectral-autoregression.html)
