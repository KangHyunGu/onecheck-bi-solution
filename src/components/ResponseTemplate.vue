<template>
  <div class="response-template-container">
    <!-- 스트리밍된 HTML이 표시될 영역 -->
    <div ref="contentContainer" v-html="streamedContent" class="streamed-content"></div>
  </div>
</template>

<script>
export default {
  name: 'ResponseTemplate',
  data() {
    return {
      // 멀티라인으로 작성한 HTML 문자열
      fullContent: `
      <!-- 4월 전체 매출 현황 -->
        <div>
          <h5>4월 전체 매출 현황</h5>
          <p>
            <ul>
              <li><strong>4월 매출</strong>: 458백만 엔<br /></li>
              <li><strong>전월(3월) 대비</strong>: 500백만 엔 → 41.8백만 엔 감소 (약 -8.4%)</li>
            </ul>
          </p>
        </div>

        <!-- 매출 감소의 주요 원인 -->
        <div>
          <h5>매출 감소의 주요 원인</h5>
          <ul>
            <li><strong>방문자 수 감소</strong>: 4.45% 감소 (CtC -424bps)</li>
            <li><strong>판매 단가 하락</strong>: 2.7% 하락 (CtC -259bps)</li>
            <li><strong>CVR(전환율) 하락</strong>: 92bps 감소 (CtC -152bps)</li>
          </ul>
        </div>

        <!-- 점포별 매출 영향 -->
        <div>
          <h5>매출 감소에 가장 크게 기여한 점포:</h5>
          <ul>
            <li>06_동운점 (CtC -205bps)</li>
            <li>01_킨시초점 (CtC -176bps)</li>
            <li>08_롯폰기점 (CtC -138bps)</li>
          </ul>
          <hr class="q-my-md" />
        </div>

        <!-- 카테고리별 매출 영향 -->
        <div>
          <h5>카테고리별 매출 영향</h5>
          <ol>
            <li>
              <strong>14. 유아용품</strong> (CtC -204bps)
              <ul>
                <li>방문자 수: 6.9% 감소</li>
                <li>CVR: 377bps 감소</li>
              </ul>
            </li>
            <li>
              <strong>13. 건강식품 및 보조제</strong> (CtC -89bps)
              <ul>
                <li>CVR: 637bps 감소</li>
              </ul>
            </li>
            <li>
              <strong>15. 애완동물용품</strong> (CtC -69bps)
              <ul>
                <li>방문자 수: 6.6% 감소</li>
                <li>CVR: 324bps 감소</li>
              </ul>
            </li>
          </ol>
        </div>

        <!-- 요약 -->
        <div>
          <h5>요약</h5>
          <p>
            4월 전체 매출은 458백만 엔으로, 전월(3월) 500백만 엔 대비 41.8백만 엔(-8.4%) 감소했습니다.<br />
            매출 감소의 주요 원인은 방문자 수가 4.45% 감소한 점(CtC -424bps)이며, 이에 더해 판매 단가 2.7% 하락(CtC -259bps)과 CVR 92bps 감소(CtC -152bps)가 추가적인 영향을 주었습니다.<br />
            점포별로는 06_동운점(CtC -205bps)이 가장 큰 영향을 미쳤으며, 그 뒤를 01_킨시초점(CtC -176bps)과 08_롯폰기점(CtC -138bps)이 이었습니다.<br />
            특히, 동운점은 매출 단가가 401엔으로 상승(+2.06%)하여 일부 긍정적인 영향을 주었으나, 방문자 수 8.41% 감소와 CVR -332bps 하락으로 전체적으로 -148bps와 -89bps의 부정적인 영향을 받았습니다.<br />
            또한, 카테고리별로는 14. 유아용품(CtC -204bps)이 가장 큰 영향을 미쳤고, 13. 건강식품 및 보조제(CtC -89bps)와 15. 애완동물용품(CtC -69bps)도 각각 CVR 637bps 감소와 방문자 수 6.6% 감소, CVR 324bps 감소의 영향을 주고 있습니다.
          </p>
        </div>
      `,
      streamedContent: '', // 스트리밍되어 화면에 표시될 HTML
      currentIndex: 0,     // 현재까지 노출한 문자열의 인덱스
      timer: null,         // setInterval 타이머 참조
    };
  },
  mounted() {
    // 불필요한 줄바꿈 및 들여쓰기 공백 제거
    console.log('before : ', this.fullContent);
    this.fullContent = this.fullContent.replace(/\s*\n\s*/g, ' ').trim();
    console.log('after : ',  this.fullContent);
    // 스트리밍 시작
    this.startStreaming();
  },
  beforeDestroy() {
    if (this.timer) {
      clearInterval(this.timer);
    }
  },
  methods: {
    // 10ms 간격으로 한 글자씩 streamedContent에 추가
    startStreaming() {
      this.timer = setInterval(() => {
        if (this.currentIndex < this.fullContent.length) {
          this.streamedContent += this.fullContent[this.currentIndex];
          this.currentIndex++;
          this.$emit('content-updated'); // 🔥 자동 스크롤 이벤트 발생
        } else {
          this.$emit('update:isRunning', false);
          clearInterval(this.timer);
        }
      }, 20);
    }
  }
};
</script>

<style scoped>
/* 스트리밍 영역의 스타일 - 필요 시 높이 제한 후 스크롤 생성 */
.streamed-content {
  white-space: normal;
}

/* ul, li 기본 여백 제거 */
ul, li {
  margin: 0;
  padding: 0;
}

/* 필요 시 .response-template-container에 추가 스타일 적용 */
.response-template-container {
  /* 예시: padding: 16px; */
}
</style>
