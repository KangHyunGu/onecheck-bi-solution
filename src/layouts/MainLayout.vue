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
        <q-scroll-area ref="scrollArea" class="col-10 q-pr-md" @scroll="onUserScroll">
          <div style="padding-right: 16px;" class="q-card">
            <!-- Q&A 카드 목록 -->
            <div
              v-for="(qa, index) in qaList"
              :key="index"
              ref="qaCards"
              class="q-mt-md"
              style="white-space: pre-wrap;"
            >
              <q-card outlined>
                <q-card-section>
                  <div class="text-subtitle1">Q. {{ qa.question }}</div>
                </q-card-section>
                <q-separator />
                <q-card-section class="q-card-section" v-if="isLoading && !qa.isAnswer">
                  <q-spinner-comment color="orange-9" size="3em" /> 잠시만 기다려주세요, 분석 작업 진행 중입니다…
                </q-card-section>

                <q-card-section class="q-card-section" v-else>
                  <!-- 각 스트리밍 컴포넌트는 업데이트 시 해당 카드에 포커스 -->
                  <response-template
                    v-if="qa.question_knd == 1 || qa.question.includes('전체 매출')"
                    @content-updated="scrollToContext(index)"
                    v-model:isRunning="isRunning"
                  ></response-template>
                  <response-store-summary
                    v-else-if="qa.question_knd == 2 || qa.question.includes('점포별 매출')"
                    @content-updated="scrollToContext(index)"
                    v-model:isRunning="isRunning"
                  ></response-store-summary>
                  <div
                    v-else
                    class="text-body1"
                    style="white-space: pre-wrap;"
                    v-html="qa.answer"
                  ></div>
                </q-card-section>

              </q-card>
            </div>
          </div>
        </q-scroll-area>

        <!-- 오른쪽: 질문 입력 섹션 (col-2) -->
        <div class="col-2">
          <q-card class="column full-height" outlined>
            <q-card-section>
              <p class="text-h4 text-center">
                매장 운영·고객 행동 분석 등 <br />필요하다면 물어보세요!
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
                placeholder="편하게 질문해 보세요"
                @keyup="onKeyUp"
                :disable="isRunning"
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
                  :disable="isRunning"
                  size="mb"
                  :label="sample"
                  @click="onSampleQuestion(sample, i)"
                />
              </div>
            </q-card-section>
            <q-card-section>

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
        '점포별 매출 현황'
      ],
      isUserScrollingUp: false, // 사용자가 위로 스크롤하고 있는지 여부
      isLoading: false,
      isRunning: false,
    }
  },
  watch: {
    // 새로운 Q&A 항목이 추가될 때, 사용자가 스크롤 중이 아니면 마지막 카드에 포커스
    qaList(newVal, oldVal) {
      if (!this.isUserScrollingUp && newVal.length > oldVal.length) {
        this.$nextTick(() => {
          const cards = this.$refs.qaCards;
          if (cards && cards.length) {
            const lastIndex = cards.length - 1;
            cards[lastIndex].scrollIntoView({ behavior: 'smooth', block: 'center' });
          }
        });
      }
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
      if (!this.question.trim()) return;

      // 로딩 표시 시작
      //this.$q.loading.show({ message: '응답 준비 중입니다.' });

      // 2~5초 사이 랜덤 대기 시간 계산 (ms 단위)
      const delay = Math.floor(Math.random() * (5000 - 2000 + 1)) + 2000;

      const qaMake = {
        question: this.question,
        answer: '',
        knd: question_knd,
        isAnswer: false
      }

      this.qaList.push(qaMake)
      this.isLoading = true;
      this.isRunning = true;
      // 랜덤 대기 후 답변 생성
      setTimeout(() => {
        const generatedAnswer = this.generateAnswer(this.question);

        // Q&A 리스트에 추가
        // this.qaList.push({
        //   question: this.question,
        //   answer: generatedAnswer,
        //   question_knd
        // });
        qaMake.answer = generatedAnswer;
        qaMake.knd = question_knd;
        qaMake.isAnswer = true;

        // 입력창 비우기
        this.question = '';
        this.isLoading = false;
        // 로딩 표시 종료
        //this.$q.loading.hide();
      }, delay);
    },
    generateAnswer(q) {
      let answer = '';
      if (q.includes("전체 매출") || q.includes("점포별 매출")) {
        answer = '';
      } else {
        this.isRunning = false;
        answer = '요청하신 내용에 대해 답변이 어렵습니다.';
      }
      return answer;
    },
    onSampleQuestion(sample, index) {
      this.question = this.sampleQuestions[index];
      this.sendQuestion(index + 1);
    },

    onUserScroll() {
      if (!this.$refs.scrollArea) return;

      const { verticalPosition, verticalSize, scrollHeight } = this.$refs.scrollArea.getScroll();

      // 사용자가 스크롤 영역의 80% 미만에 머무르면 사용자가 직접 스크롤 중인 것으로 판단
      this.isUserScrollingUp = verticalPosition + verticalSize < scrollHeight * 0.8;
    },

    // 각 카드에 대해 포커스 이동 (자동 스크롤)
    scrollToContext(index) {
      if (this.isUserScrollingUp) return;
      this.$nextTick(() => {
        const cards = this.$refs.qaCards;
        const card = cards[index];
        if (!card) return;
        // 초기 스크롤
        card.scrollIntoView({ behavior: 'smooth', block: 'end' });

        const cardSection = this.$refs.qaCards[index].querySelector('.q-card-section');

        if(!cardSection) return;
        const contentEl = cardSection.querySelector('.streamed-content');

        // 콘텐츠가 넘치면 100px씩 확장
        if ((contentEl.scrollHeight + 100) > cardSection.clientHeight) {
          const current = parseInt(getComputedStyle(cardSection).minHeight) || cardSection.clientHeight;
          cardSection.style.minHeight = (current + 100) + 'px';
        }

      });
    },

  //   observeCardResize(card, index) {
  //     const observer = new ResizeObserver(entries => {
  //       for (let entry of entries) {
  //         // 콘텐츠가 커질 때만 실행
  //         if (entry.target.offsetHeight > (entry.target.dataset.prevHeight || 0)) {
  //           console.log('this.$refs.scrollArea : ', this.$refs.scrollArea);
  //           //this.$refs.scrollArea.getScrollElement().scrollBy({ top: 10, behavior: 'smooth' });
  //           entry.target.dataset.prevHeight = entry.target.offsetHeight;
  //         }
  //       }
  //     });
  //     observer.observe(card);
  // },
  }
}
</script>

<style scoped>
/* 필요한 추가 스타일 작성 */

.q-card-section {
  overflow: hidden;
  transition: min-height 0.25s ease;
  min-height: 0;
}
</style>
