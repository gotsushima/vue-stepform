<script setup>
import { inject, watch } from 'vue';

import qualificationImg01 from '@/assets/images/btn-icon-qualification01.png';
import qualificationImg02 from '@/assets/images/btn-icon-qualification02.png';
import qualificationImg03 from '@/assets/images/btn-icon-qualification03.png';
import qualificationImg04 from '@/assets/images/btn-icon-qualification04.png';
import qualificationImg07 from '@/assets/images/btn-icon-qualification07.png';
import qualificationImg08 from '@/assets/images/btn-icon-qualification08.png';
import qualificationImg05 from '@/assets/images/btn-icon-qualification05.png';
import qualificationImg06 from '@/assets/images/btn-icon-qualification06.png';

const currentStep = inject('currentStep');
const errorMessages = inject('errorMessages');
const validateStep = inject('validateStep');
const goToStep = inject('goToStep');
const touchField = inject('touchField');
const validateField = inject('validateField');

const qualification = inject('qualification');

const qualifications = [
  { value: '介護福祉士', label: '介護福祉士', img: qualificationImg01 },
  {
    value: '初任者研修(ヘルパー2級)',
    label: '初任者研修<span class="qualification-small-txt">(ヘルパー2級)',
    img: qualificationImg02,
  },
  {
    value: '実務者研修(ヘルパー1級)',
    label: '実務者研修<span class="qualification-small-txt">(ヘルパー1級)',
    img: qualificationImg03,
  },
  {
    value: '介護支援専門員(ケアマネージャー)',
    label:
      '介護支援専門員<span class="qualification-small-txt">(ケアマネージャー)',
    img: qualificationImg04,
  },
  {
    value: '資格なし（介護職 経験有）',
    label: '資格なし<span class="qualification-small-txt">（介護職 経験有）',
    img: qualificationImg07,
  },
  {
    value: '資格なし（介護職 未経験）',
    label: '資格なし<span class="qualification-small-txt">（介護職 未経験）',
    img: qualificationImg08,
  },
  { value: '社会福祉士', label: '社会福祉士', img: qualificationImg05 },
  { value: 'その他', label: 'その他', img: qualificationImg06 },
];

const transitionToStep2 = () => {
  if (qualification.value.length > 0) {
    setTimeout(() => {
      goToStep(2);
    }, 100);
  }
};
const handleQualificationChange = () => {
  touchField('qualification');
  validateField('qualification');
  transitionToStep2();
};

watch(qualification, () => {
  touchField('qualification');
});
</script>

<template>
  <div
    id="step1"
    v-show="currentStep === 1"
  >
    <div class="step-img">
      <img
        src="@/assets/images/step-1.png"
        alt="step1"
      />
    </div>

    <dl class="form-item">
      <dt class="required">どの資格をお持ちですか？</dt>
      <dd>
        <ul class="checkbox form-check-btn">
          <li
            class="list-icon qualification-list"
            v-for="qualificationOption in qualifications"
            :key="qualificationOption.value"
          >
            <label>
              <input
                type="checkbox"
                name="qualification"
                :value="qualificationOption.value"
                v-model="qualification"
                @change="handleQualificationChange"
              />
              <span>
                <span class="button-icon qualification-button-icon">
                  <img :src="qualificationOption.img" />
                </span>
                <span v-html="qualificationOption.label"></span>
              </span>
            </label>
          </li>
        </ul>
      </dd>
      <p
        class="error"
        v-if="errorMessages.qualification"
        v-cloak
      >
        {{ errorMessages.qualification }}
      </p>
    </dl>

    <div class="button-area">
      <button
        type="button"
        class="nextbtn"
        v-bind:class="{ 'button-shine': validateStep(1) }"
        id="next1"
        @click="goToStep(currentStep + 1)"
        :disabled="!validateStep(1)"
      >
        <p class="free">無料</p>
        <p>次へ進む</p>
      </button>
    </div>
  </div>
</template>

<style lang="scss">
@import '@/assets/css/steps.scss';
</style>
