<template>
  <q-layout view="lHh Lpr lFf" style="height: 100vh;" color="orange-9">
    <!-- 상단 헤더 -->
    <q-header elevated class="bg-orange-9">
      <q-toolbar>
        <!-- <q-toolbar-title>週刊レ포ート</q-toolbar-title> -->
        <q-space />
        <!-- <div class="text-caption q-mr-md">2025년 6월 22일 (일요일)</div> -->
        <q-btn flat round icon="settings" @click="onClickSettings" />
      </q-toolbar>
    </q-header>

    <!-- 페이지 컨테이너 -->
    <q-page-container>
      <q-page class="row no-wrap fit q-pa-md" style="height: 100%;">
        <!-- 왼쪽: ResponseTemplate 컴포넌트 및 Q&A 카드 목록 (스크롤 영역) -->
        <q-scroll-area class="col-10 q-pr-md">
          <div style="padding-right: 16px;">

            <!-- Q&A 카드 목록 -->
            <div
              v-for="(qa, index) in qaList"
              :key="index"
              class="q-mt-md"
              style="white-space: pre-wrap;"
            >
              <q-card outlined>
                <q-card-section>
                  <div class="text-subtitle1">Q. {{ qa.question }}</div>
                </q-card-section>
                <q-separator />
                <q-card-section>
                  <response-template v-if="qa.question_knd == 1 || qa.question.includes('전체 매출')"></response-template>
                  <response-store-summary v-if="qa.question_knd == 2 || qa.question.includes('점포별 매출')"></response-store-summary>
                  <div v-else class="text-body1" style="white-space: pre-wrap;" v-html="qa.answer"></div>
                </q-card-section>
              </q-card>
            </div>
          </div>
        </q-scroll-area>

        <!-- 오른쪽: 질문 입력 섹션 (col-2) -->
        <div class="col-2">
          <q-card class="column full-height" outlined>
            <q-card-section>
              <p class="text-body2">
                궁금하신 점을 아래에 입력해 주세요.<br/>
                질문을 등록하면, 왼쪽에 새로운 카드가 추가됩니다.
              </p>
            </q-card-section>
            <q-separator />
            <q-card-actions class="q-pa-md full-width">
              <q-input
                class="col"
                v-model="question"
                type="textarea"
                outlined
                autogrow
                rows="1"
                max-rows="5"
                placeholder="Shift+Enter로 줄바꿈, Enter로 전송"
                @keyup="onKeyUp"
              >
                <template v-slot:append>
                  <q-btn flat round icon="send" @click="sendQuestion" />
                </template>
              </q-input>
            </q-card-actions>

            <!-- 질문 유형 샘플 버튼 목록 -->
            <q-card-section>
              <p class="text-caption q-mb-sm">질문 유형 샘플</p>
              <div class="column items-start q-col-gutter-sm">
                <q-btn
                  v-for="(sample, i) in sampleQuestions"
                  :key="i"
                  class="q-mb-sm fit"
                  color="orange-9"
                  outline
                  size="mb"
                  :label="sample"
                  @click="onSampleQuestion(sample, i)"
                />
              </div>
            </q-card-section>
          </q-card>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script>
import ResponseTemplate from 'components/ResponseTemplate.vue';
import ResponseStoreSummary from 'src/components/ResponseStoreSummary.vue';

export default {
  name: 'MainLayout',
  components: {
    ResponseTemplate,
    ResponseStoreSummary
  },
  data() {
    return {
      question: '',
      qaList: [],  // 각 Q&A 항목: { question: string, answer: string }
      question_knd: 0,
      sampleQuestions: [
        '전체 매출 요약',
        '점포별 매출 현황',
      ]
    }
  },
  methods: {
    onClickSettings() {
      alert('설정 창을 열 예정입니다.');
    },
    onKeyUp(event) {
      if (event.key === 'Enter' && !event.shiftKey) {
        this.sendQuestion();
        event.preventDefault(); // textarea 기본 개행 막기
      }
    },
    sendQuestion(question_knd = 0) {
      // 질문이 비어있으면 중단
      if (!this.question.trim()) return;

      // (1) 로딩 표시 시작
      this.$q.loading.show({message: '응답 준비 중입니다.'});

      // (2) 2~5초 사이 랜덤 대기 시간 계산 (ms 단위)
      const delay = Math.floor(Math.random() * (5000 - 2000 + 1)) + 2000;

      // (3) setTimeout으로 랜덤 대기 후 답변 생성
      setTimeout(() => {
        const generatedAnswer = this.generateAnswer(this.question);

        // (4) Q&A 리스트에 추가
        this.qaList.push({
          question: this.question,
          answer: generatedAnswer,
          question_knd,
        });

        // (5) 입력창 비우기
        this.question = '';

        // (6) 로딩 표시 종료
        this.$q.loading.hide();
      }, delay);
    },
    generateAnswer(q) {
      if(q.includes("전체 매출") || q.includes("점포별 매출")){
        return '';
      }
      return `요청하신 내용에 대해 답변이 어렵습니다.`;
    },
    onSampleQuestion(sample, index){
      this.question = this.sampleQuestions[index];
      this.sendQuestion(index + 1);
    }
  }
}
</script>

<style scoped>
/* 필요한 추가 스타일 작성 */
</style>
