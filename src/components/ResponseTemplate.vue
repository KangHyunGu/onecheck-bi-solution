<template>
  <div>
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
        <li>
          <strong>방문자 수 감소</strong>: 전월 대비 4.45% 감소, CtC -424bps
        </li>
        <li>
          <strong>판매 단가 하락</strong>: 2.7% 하락, CtC -259bps
        </li>
        <li>
          <strong>CVR(전환율) 하락</strong>: 92bps 감소, CtC -152bps
        </li>
      </ul>
    </div>

    <!-- 점포별 매출 영향 -->
    <div>
      <h5>점포별 매출 영향</h5>
      <p>매출 감소에 가장 크게 기여한 점포:</p>
      <ul>
        <li>06_동운점 (CtC -205bps)</li>
        <li>01_킨시초점 (CtC -176bps)</li>
        <li>08_롯폰기점 (CtC -138bps)</li>
      </ul>
      <q-separator class="q-my-md" />
      <h5>06_동운점 상세</h5>
      <ul>
        <li>
          매출 단가: 401엔 (전월 대비 +2.06%) → 매출에 <strong>+32bps</strong> 긍정 영향
        </li>
        <li>
          방문자 수: 전월 대비 <strong>-8.41%</strong> → 매출에 <strong>-148bps</strong> 부정 영향
        </li>
        <li>
          CVR: <strong>-332bps</strong> 하락 → 매출에 <strong>-89bps</strong> 부정 영향
        </li>
        <li>
          <em>
            ⇒ 단가 상승에도 불구, 방문자 수 및 CVR 하락으로 전체 매출 감소
          </em>
        </li>
      </ul>
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
        4월 매출은 전월 대비 <strong>-8.4%</strong> 감소한 <strong>458백만 엔</strong>입니다.<br />
        <strong>방문자 수 감소 (4.45%↓)</strong>와 <strong>CVR (92bps↓)</strong> 하락이 주요 원인으로 작용했으며,<br />
        <strong>06_동운점 · 01_킨시초점 · 08_롯폰기점</strong>이 매출 감소에 큰 영향을 미쳤습니다.<br />
        카테고리별로는 <strong>14. 유아용품</strong>이 가장 큰 감소 요인 (CtC -204bps)으로 나타났습니다.
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResponseTemplate"
};
</script>

<style scoped>
/* 추가적인 스타일 */
</style>
