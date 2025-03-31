<template>
  <div class="response-template-container">
    <!-- 스트리밍된 HTML이 표시될 영역 -->
    <div ref="contentContainer" class="streamed-content" v-html="streamedContent"></div>
  </div>
</template>

<script>
export default {
  name: 'ResponseTemplate',
  data() {
    return {
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
      streamedContent: '',
      tokens: [],
      currentIndex: 0,
      timer: null,
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
    // HTML 태그는 그대로, 일반 텍스트는 글자 단위로 분리
    prepareTokens() {
      const tokens = [];
      let inTag = false;
      let currentToken = '';
      for (let i = 0; i < this.fullContent.length; i++) {
        const char = this.fullContent[i];
        if (char === '<') {
          if (currentToken !== '') {
            // 일반 텍스트가 있다면 글자 단위로 분리하여 추가
            tokens.push(...currentToken.split(''));
            currentToken = '';
          }
          inTag = true;
          currentToken += char;
        } else if (char === '>') {
          currentToken += char;
          tokens.push(currentToken);
          currentToken = '';
          inTag = false;
        } else {
          if (inTag) {
            currentToken += char;
          } else {
            tokens.push(char);
          }
        }
      }
      if (currentToken !== '') {
        tokens.push(currentToken);
      }
      this.tokens = tokens;
    },
    startStreaming() {
      const streamNextToken = () => {
        if (this.currentIndex < this.tokens.length) {
          const token = this.tokens[this.currentIndex++];
          if (/^<[^>]+>$/.test(token)) {
            // HTML 태그는 그대로 추가
            this.streamedContent += token;
          } else {
            // 일반 텍스트(글자)는 span 태그로 감싸고, 인덱스 기반 딜레이를 적용
            const delay = (this.currentIndex * 30) + 'ms';
            this.streamedContent += `<span class="letter" style="animation-delay: ${delay}">${token}</span>`;
          }
          this.$emit('content-updated');
          this.timer = setTimeout(streamNextToken, 20);
        } else {
          this.$emit('update:isRunning', false);
        }
      };
      streamNextToken();
    },
  }
};
</script>

<style scoped>
.streamed-content {
  white-space: normal;
}

/* 각 글자를 inline-block으로 표시하여 애니메이션 적용 */
.letter {
  display: inline-block;
  opacity: 0;
  transform: translateY(10px);
  /* fade-in-out 효과: 글자가 나타났다가 잠시 사라졌다가 최종적으로 나타남 */
  animation: fadeInOut 0.6s forwards;
}

/* fade-in-out 키프레임 애니메이션 */
@keyframes fadeInOut {
  0% {
    opacity: 0;
    transform: translateY(10px);
  }
  50% {
    opacity: 1;
    transform: translateY(0);
  }
  70% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
