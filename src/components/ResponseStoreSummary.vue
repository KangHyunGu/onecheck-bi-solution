<template>
  <div class="response-template-container">
    <!-- 스트리밍된 HTML이 표시될 영역 -->
    <div v-html="streamedContent" class="streamed-content"></div>
  </div>
</template>

<script>
export default {
  name: 'ResponseTemplate',
  data() {
    return {
      /**
       * (1) 멀티라인으로 작성된 HTML 문자열
       *  - 테이블을 없애고, 텍스트·줄바꿈만으로 점포 정보를 표시
       */
      fullContent: `
        <div class="q-pa-md">

          <!-- ✅ 전체 점포 리스트 -->
          <div class="q-mb-md">
            <div class="bg-grey-3 text-black q-pa-sm text-h6">🏢 전체 점포 현황</div>

            <!-- 1) 긴시초점 -->
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">긴시초점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 37,939,133엔<br>
                  &emsp;<strong>전월 대비</strong>: -8,820,099엔 (-18.89% 감소)
                </p>
                <p>
                  &emsp;<strong>방문자 수</strong>: 16.1만명 (-4.19% 감소), 영향: <span class="text-negative">-33bps</span><br>
                  &emsp;<strong>구매 전환율</strong>: 60.23% (-600bps), 영향: <span class="text-negative">-87bps</span><br>
                  &emsp;<strong>평균 단가</strong>: 390엔 (-6.88% 하락), 영향: <span class="text-negative">-56bps</span>
                </p>
              </div>
              <div class="card-section bg-grey-2">
                <p>&emsp;<strong>향후 조치사항:</strong> 구매 전환율 향상을 위한 대책 필요.</p>
              </div>
            </div>

            <!-- 2) 츠키시마점 -->
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">츠키시마점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 85,840,701엔<br>
                  &emsp;<strong>전월 대비</strong>: -5,105,125엔 (-5.61% 감소)
                </p>
                <p>
                  &emsp;<strong>방문자 수</strong>: 32.8만명 (-5.92% 감소), 영향: <span class="text-negative">-108bps</span><br>
                  &emsp;<strong>구매 전환율</strong>: 66.59% (+282bps), 영향: <span class="text-positive">+76bps</span><br>
                  &emsp;<strong>평균 단가</strong>: 393엔 (-3.92% 하락), 영향: <span class="text-negative">-70bps</span>
                </p>
              </div>
              <div class="card-section bg-grey-2">
                <p>&emsp;<strong>향후 조치사항:</strong> 방문자 수 감소 대응 필요.</p>
              </div>
            </div>

            <!-- 3) 롯폰기점 -->
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">롯폰기점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 41,940,315엔<br>
                  &emsp;<strong>전월 대비</strong>: -6,902,114엔 (-14.13% 감소)
                </p>
                <p>
                  &emsp;<strong>방문자 수</strong>: 16.2만명 (-4.67% 감소), 영향: <span class="text-negative">-41bps</span><br>
                  &emsp;<strong>구매 전환율</strong>: 61.80% (-494.78bps), 영향: <span class="text-negative">-73bps</span><br>
                  &emsp;<strong>평균 단가</strong>: 418엔 (-2.71% 하락), 영향: <span class="text-negative">-23bps</span>
                </p>
              </div>
              <div class="card-section bg-grey-2">
                <p>&emsp;<strong>향후 조치사항:</strong> 구매전환율 향상 필요.</p>
              </div>
            </div>
          </div>

          <!-- ✅ Top 3 점포 -->
          <div class="q-mb-md">
            <div class="bg-green-3 text-black q-pa-sm text-h6">🚀 Top 3 점포 (매출 감소율 적음)</div>
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">츠키시마점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 85,840,701엔<br>
                  &emsp;<strong>전월 대비</strong>: -5,105,125엔 (-5.61% 감소)
                </p>
              </div>
            </div>
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">도요스점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 84,860,736엔<br>
                  &emsp;<strong>전월 대비</strong>: -3,493,874엔 (-3.95% 감소)
                </p>
              </div>
            </div>
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">시오도메점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 41,722,166엔<br>
                  &emsp;<strong>전월 대비</strong>: -1,412,676엔 (-3.28% 감소)
                </p>
              </div>
            </div>
          </div>

          <!-- ✅ Worse 3 점포 -->
          <div class="q-mb-md">
            <div class="bg-red-3 text-black q-pa-sm text-h6">⚠️ Worse 3 점포 (매출 감소율 큼)</div>
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">긴시초점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 37,939,133엔<br>
                  &emsp;<strong>전월 대비</strong>: -8,820,099엔 (-18.89% 감소)
                </p>
              </div>
            </div>
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">롯폰기점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 41,940,315엔<br>
                  &emsp;<strong>전월 대비</strong>: -6,902,114엔 (-14.13% 감소)
                </p>
              </div>
            </div>
            <div class="store-card q-mb-md">
              <div class="card-section">
                <div class="text-h6">동운점</div>
                <p>
                  &emsp;<strong>4월 매출</strong>: 80,536,046엔<br>
                  &emsp;<strong>전월 대비</strong>: -10,249,272엔 (-11.29% 감소)
                </p>
              </div>
            </div>
          </div>

        </div>
      `,
      streamedContent: '', // 스트리밍되어 화면에 표시될 HTML
      currentIndex: 0,     // 현재까지 노출한 문자열의 인덱스
      timer: null,         // setInterval 타이머 참조
    };
  },
  mounted() {
    // (2) 멀티라인 문자열에서 불필요한 줄바꿈/공백을 정리
    this.fullContent = this.fullContent.replace(/\s*\n\s*/g, ' ').trim();
    console.log(this.fullContent);
    // (3) 스트리밍 시작
    this.startStreaming();
  },
  beforeDestroy() {
    if (this.timer) {
      clearInterval(this.timer);
    }
  },
  methods: {
    /**
     * 20ms 간격으로 한 글자씩 streamedContent에 추가하여
     * LLM 스트리밍 효과처럼 보여줍니다.
     */
    startStreaming() {
      this.timer = setInterval(() => {
        if (this.currentIndex < this.fullContent.length) {
          this.streamedContent += this.fullContent[this.currentIndex];
          this.currentIndex++;
          this.$emit('content-updated'); // 🔥 자동 스크롤 이벤트 발생
        } else {
          clearInterval(this.timer);
        }
      }, 20);
    }
  }
};
</script>

<style scoped>
/* (4) 스트리밍 영역의 white-space 설정 */
.streamed-content {
  white-space: normal; /* HTML 태그는 해석, 태그 사이 공백은 자동 조정 */
}

/* 필요 시 .response-template-container에 추가 스타일 적용 */
.response-template-container {
  /* 예: padding: 16px; */
}

/* Quasar-like 스타일(예시) */
.q-pa-md {
  padding: 16px;
}
.q-mb-md {
  margin-bottom: 16px;
}
.bg-grey-3 {
  background-color: #e0e0e0;
}
.bg-grey-2 {
  background-color: #eeeeee;
}
.bg-green-3 {
  background-color: #a5d6a7;
}
.bg-red-3 {
  background-color: #ef9a9a;
}
.text-black {
  color: black;
}
.text-h6 {
  font-size: 1.25rem;
  font-weight: bold;
}
.q-pa-sm {
  padding: 8px;
}

/* store-card, card-section 등 임의 클래스 */
.store-card {
  border: 1px solid #ccc;
  margin-bottom: 16px;
}
.card-section {
  padding: 8px;
}

/* 증감률에 따른 텍스트 색상 예시 */
.text-negative {
  color: red;
}
.text-positive {
  color: green;
}
</style>
