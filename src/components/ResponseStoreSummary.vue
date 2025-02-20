<template>
  <div class="q-pa-md">
    <!-- 📌 전체 점포 리스트 -->
    <div class="q-mb-md">
      <div class="bg-grey-3 text-black q-pa-sm text-h6">🏢 전체 점포 현황</div>
      <q-card flat bordered class="q-mb-md" v-for="(store, index) in allStores" :key="'all-' + index">
        <q-card-section>
          <div class="text-h6">{{ store.name }}</div>
          <p>
            <strong>4월 매출</strong>: {{ store.sales }}엔<br />
            <strong>전월 대비</strong>: {{ store.change }}엔 ({{ store.changePercent }}% 감소)
          </p>
        </q-card-section>
        <q-markup-table dense bordered>
          <tbody>
            <tr>
              <td class="text-left"><strong>방문자 수</strong></td>
              <td>{{ store.visitors }}명 ({{ store.visitorChange }}% 감소)</td>
              <td class="text-negative">{{ store.visitorImpact }}bps</td>
            </tr>
            <tr>
              <td class="text-left"><strong>구매 전환율</strong></td>
              <td>{{ store.cvr }}% ({{ store.cvrChange }}bps)</td>
              <td :class="store.cvrImpact > 0 ? 'text-positive' : 'text-negative'">{{ store.cvrImpact }}bps</td>
            </tr>
            <tr>
              <td class="text-left"><strong>평균 단가</strong></td>
              <td>{{ store.avgPrice }}엔 ({{ store.priceChange }}% 하락)</td>
              <td class="text-negative">{{ store.priceImpact }}bps</td>
            </tr>
          </tbody>
        </q-markup-table>
        <q-card-section class="bg-grey-2">
          <p><strong>향후 조치사항:</strong> {{ store.action }}</p>
        </q-card-section>
      </q-card>
    </div>

    <!-- 📌 Top 3 점포 -->
    <div class="q-mb-md">
      <div class="bg-green-3 text-black q-pa-sm text-h6">🚀 Top 3 점포 (매출 감소율 적음)</div>
      <q-card flat bordered class="q-mb-md" v-for="(store, index) in topStores" :key="'top-' + index">
        <q-card-section>
          <div class="text-h6">{{ store.name }}</div>
          <p>
            <strong>4월 매출</strong>: {{ store.sales }}엔<br />
            <strong>전월 대비</strong>: {{ store.change }}엔 ({{ store.changePercent }}% 감소)
          </p>
        </q-card-section>
      </q-card>
    </div>

    <!-- 📌 Worse 3 점포 -->
    <div class="q-mb-md">
      <div class="bg-red-3 text-black q-pa-sm text-h6">⚠️ Worse 3 점포 (매출 감소율 큼)</div>
      <q-card flat bordered class="q-mb-md" v-for="(store, index) in worseStores" :key="'worse-' + index">
        <q-card-section>
          <div class="text-h6">{{ store.name }}</div>
          <p>
            <strong>4월 매출</strong>: {{ store.sales }}엔<br />
            <strong>전월 대비</strong>: {{ store.change }}엔 ({{ store.changePercent }}% 감소)
          </p>
        </q-card-section>
      </q-card>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // ✅ 전체 점포 데이터
      allStores: [
        { name: "긴시초점", sales: "37,939,133", change: "-8,820,099", changePercent: "-18.89", visitors: "16.1만", visitorChange: "-4.19", visitorImpact: "-33", cvr: "60.23", cvrChange: "-600", cvrImpact: "-87", avgPrice: "390", priceChange: "-6.88", priceImpact: "-56", action: "구매 전환율 향상을 위한 대책 필요." },
        { name: "츠키시마점", sales: "85,840,701", change: "-5,105,125", changePercent: "-5.61", visitors: "32.8만", visitorChange: "-5.92", visitorImpact: "-108", cvr: "66.59", cvrChange: "+282", cvrImpact: "+76", avgPrice: "393", priceChange: "-3.92", priceImpact: "-70", action: "방문자 수 감소 대응 필요." },
        { name: "롯폰기점", sales: "41,940,315", change: "-6,902,114", changePercent: "-14.13", visitors: "16.2만", visitorChange: "-4.67", visitorImpact: "-41", cvr: "61.80", cvrChange: "-494.78", cvrImpact: "-73", avgPrice: "418", priceChange: "-2.71", priceImpact: "-23", action: "구매전환율 향상 필요." }
      ],

      // ✅ Top 3 점포
      topStores: [
        { name: "츠키시마점", sales: "85,840,701", change: "-5,105,125", changePercent: "-5.61" },
        { name: "도요스점", sales: "84,860,736", change: "-3,493,874", changePercent: "-3.95" },
        { name: "시오도메점", sales: "41,722,166", change: "-1,412,676", changePercent: "-3.28" }
      ],

      // ✅ Worse 3 점포
      worseStores: [
        { name: "긴시초점", sales: "37,939,133", change: "-8,820,099", changePercent: "-18.89" },
        { name: "롯폰기점", sales: "41,940,315", change: "-6,902,114", changePercent: "-14.13" },
        { name: "동운점", sales: "80,536,046", change: "-10,249,272", changePercent: "-11.29" }
      ]
    };
  }
};
</script>

<style scoped>
.text-negative {
  color: red;
}
.text-positive {
  color: green;
}
</style>
