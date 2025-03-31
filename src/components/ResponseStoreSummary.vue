<template>
  <div class="response-template-container">
    <div v-html="streamedContent" class="streamed-content"></div>
  </div>
</template>

<script>
export default {
  name: 'ResponseTemplate',
  data() {
    return {
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

          <!-- ✅ Bottom 3 점포 -->
          <div class="q-mb-md">
            <div class="bg-red-3 text-black q-pa-sm text-h6">⚠️ Bottom 3 점포 (매출 감소율 큼)</div>
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
      streamedContent: '',
      tokens: [],
      timer: null,
      currentIndex: 0,
    };
  },
  mounted() {
    this.prepareTokens();
    this.startStreaming();
  },
  beforeDestroy() {
    if (this.timer) clearTimeout(this.timer);
  },
  methods: {
    prepareTokens() {
      const regex = /(<[^>]+>|\s+|[^<\s]+)/g;
      this.tokens = this.fullContent.match(regex);
    },
    startStreaming() {
      const streamNextToken = () => {
        if (this.currentIndex < this.tokens.length) {
          this.streamedContent += this.tokens[this.currentIndex++];
          //console.log(this.streamedContent);
          this.$emit('content-updated');
          const delay = 15 + Math.random() * 30; // 더 자연스러운 속도
          this.timer = setTimeout(streamNextToken, delay);
        } else {
          this.$emit('update:isRunning', false);
        }
      };
      streamNextToken();
    }
  }
};
</script>

<style scoped>
.streamed-content {
  white-space: normal;
}
.response-template-container {
  padding: 16px;
}
</style>
