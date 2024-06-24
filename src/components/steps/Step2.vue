<script setup>
import { ref, inject, watch } from 'vue';

import workstyleImg01 from '@/assets/images/btn-icon-workstyle01.png';
import workstyleImg02 from '@/assets/images/btn-icon-workstyle02.png';
import workstyleImg03 from '@/assets/images/btn-icon-workstyle03.png';
import workstyleImg04 from '@/assets/images/btn-icon-workstyle04.png';
import workstyleImg05 from '@/assets/images/btn-icon-workstyle05.png';
import workstyleImg06 from '@/assets/images/btn-icon-workstyle06.png';
import requestDateImg01 from '@/assets/images/btn-icon-request-date01.png';
import requestDateImg02 from '@/assets/images/btn-icon-request-date02.png';
import requestDateImg03 from '@/assets/images/btn-icon-request-date03.png';
import requestDateImg04 from '@/assets/images/btn-icon-request-date04.png';
import requestDateImg05 from '@/assets/images/btn-icon-request-date05.png';
import requestDateImg06 from '@/assets/images/btn-icon-request-date06.png';

const currentStep = inject('currentStep');
const errorMessages = inject('errorMessages');
const validateStep = inject('validateStep');
const goToStep = inject('goToStep');
const touchField = inject('touchField');
const validateField = inject('validateField');

const workStyle = inject('workStyle');
const requestDate = inject('requestDate');

const previousWorkStyleLength = ref(0);
previousWorkStyleLength.value = 0;

const workStyles = [
  { value: '常勤（夜勤あり）', label: '常勤（夜勤あり）', img: workstyleImg01 },
  { value: '常勤（夜勤なし）', label: '常勤（夜勤なし）', img: workstyleImg02 },
  {
    value: 'パート・アルバイト（夜勤あり）',
    label: 'パート・アルバイト（夜勤あり）',
    img: workstyleImg03,
  },
  {
    value: 'パート・アルバイト（夜勤なし）',
    label: 'パート・アルバイト（夜勤なし）',
    img: workstyleImg04,
  },
  { value: '派遣', label: '派遣', img: workstyleImg05 },
  { value: 'こだわらない', label: 'こだわらない', img: workstyleImg06 },
];

const requestDates = [
  { value: '1ヶ月以内', label: '1ヶ月以内', img: requestDateImg01 },
  { value: '3ヶ月以内', label: '3ヶ月以内', img: requestDateImg02 },
  { value: '6ヶ月以内', label: '6ヶ月以内', img: requestDateImg03 },
  { value: '1年以内', label: '1年以内', img: requestDateImg04 },
  { value: '1年以上先', label: '1年以上先', img: requestDateImg05 },
  { value: '未定', label: '未定', img: requestDateImg06 },
];

const scrollToRequestDate = () => {
  if (workStyle.value.length === 1 && !previousWorkStyleLength.value) {
    const element = document.getElementById('back2');
    if (element) {
      element.scrollIntoView({ behavior: 'smooth', block: 'end' });
    }
  }
  previousWorkStyleLength.value = workStyle.value.length;
};

const transitionToStep3 = () => {
  goToStep(3);
};

const handleWorkStyleChange = () => {
  touchField('workStyle');
  validateField('workStyle');
  scrollToRequestDate();
};

const handleRequestDateChange = () => {
  touchField('requestDate');
  validateField('requestDate');
  transitionToStep3();
};

watch(workStyle, () => {
  touchField('workStyle');
});
watch(requestDate, () => {
  touchField('requestDate');
});
</script>

<template>
  <div
    id="step2"
    
    v-show="currentStep === 2"
  >
    <div class="step-img">
      <img
        src="@/assets/images/step-2.png"
        alt="step2"
      />
    </div>

    <dl class="form-item">
      <dt>ご希望の働き方・転職希望時期はございますか？</dt>
      <dt class="required">勤務形態</dt>
      <dd>
        <ul class="form-check-btn">
          <li
            class="list-icon workstyle-list"
            v-for="workStyleOption in workStyles"
            :key="workStyleOption.value"
          >
            <label>
              <input
                type="checkbox"
                name="workStyle"
                :value="workStyleOption.value"
                v-model="workStyle"
                @change="handleWorkStyleChange"
              />
              <span>
                <span class="button-icon workstyle-button-icon">
                  <img :src="workStyleOption.img" />
                </span>
                <span v-html="workStyleOption.label"></span>
              </span>
            </label>
          </li>
        </ul>
      </dd>
      <p
        class="error"
        v-if="errorMessages.workStyle"
        v-cloak
      >
        {{ errorMessages.workStyle }}
      </p>
    </dl>

    <dl class="form-item">
      <dt class="required">入職希望時期</dt>
      <dd>
        <ul class="form-check-btn">
          <li
            class="list-icon request-date-list"
            v-for="requestDateOption in requestDates"
            :key="requestDateOption.value"
          >
            <label>
              <input
                type="radio"
                name="requestDate"
                :value="requestDateOption.value"
                v-model="requestDate"
                @change="handleRequestDateChange"
              />
              <span>
                <span class="button-icon request-date-button-icon">
                  <img :src="requestDateOption.img" />
                </span>
                <span v-html="requestDateOption.label"></span>
              </span>
            </label>
          </li>
        </ul>
      </dd>
    </dl>

    <div class="button-area">
      <button
        type="button"
        class="backbtn"
        id="back2"
        @click="goToStep(currentStep - 1)"
      >
        <p>戻る</p>
      </button>
      <button
        type="button"
        class="nextbtn"
        v-bind:class="{ 'button-shine': validateStep(2) }"
        id="next2"
        @click="goToStep(currentStep + 1)"
        :disabled="!validateStep(2)"
      >
        <p>次へ進む</p>
      </button>
    </div>
  </div>
</template>

<style lang="scss">
@import '@/assets/css/steps.scss';
</style>
