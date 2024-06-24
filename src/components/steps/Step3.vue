<script setup>
import { inject, watch } from 'vue';

import situationImg01 from '@/assets/images/btn-icon-situation01.png';
import situationImg02 from '@/assets/images/btn-icon-situation02.png';
import situationImg03 from '@/assets/images/btn-icon-situation03.png';
import situationImg04 from '@/assets/images/btn-icon-situation04.png';
import situationImg05 from '@/assets/images/btn-icon-situation05.png';
import situationImg06 from '@/assets/images/btn-icon-situation06.png';

const currentStep = inject('currentStep');
const validateStep = inject('validateStep');
const goToStep = inject('goToStep');
const touchField = inject('touchField');
const validateField = inject('validateField');

const situation = inject('situation');

const situations = [
  {
    value: '離職中／退職確定済',
    label: '離職中／退職確定済',
    img: situationImg01,
  },
  {
    value: 'できるだけ早く辞めたい',
    label: 'できるだけ早く<br>辞めたい',
    img: situationImg02,
  },
  {
    value: '良い転職先なら辞めたい',
    label: '良い転職先なら<br>辞めたい',
    img: situationImg03,
  },
  {
    value: '良い転職先なら検討する',
    label: '良い転職先なら<br>検討する',
    img: situationImg04,
  },
  {
    value: '半年以上は辞められない',
    label: '半年以上は<br>辞められない',
    img: situationImg05,
  },
  {
    value: 'あまり辞める気はない',
    label: 'あまり辞める気はない',
    img: situationImg06,
  },
  { value: 'その他', label: 'その他', img: situationImg06 },
];

const transitionToStep4 = () => {
  goToStep(4);
};

const handleSituationChange = () => {
  touchField('situation');
  validateField('situation');
  transitionToStep4();
};

watch(situation, () => {
  touchField('situation');
});
</script>

<template>
  <div
    id="step3"
    
    v-show="currentStep === 3"
  >
    <div class="step-img">
      <img
        src="@/assets/images/step-2.png"
        alt="step2"
      />
    </div>

    <dl class="form-item">
      <dt class="required">お仕事状況</dt>
      <dd>
        <ul class="form-check-btn">
          <li
            class="list-icon situation-list"
            v-for="situationOption in situations"
            :key="situationOption.value"
          >
            <label>
              <input
                type="radio"
                name="situation"
                :value="situationOption.value"
                v-model="situation"
                @change="handleSituationChange"
              />
              <span>
                <span class="button-icon situation-button-icon">
                  <img :src="situationOption.img" />
                </span>
                <span v-html="situationOption.label"></span>
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
        id="back3"
        @click="goToStep(currentStep - 1)"
      >
        <p>戻る</p>
      </button>
      <button
        type="button"
        class="nextbtn"
        v-bind:class="{ 'button-shine': validateStep(3) }"
        id="next3"
        @click="goToStep(currentStep + 1)"
        :disabled="!validateStep(3)"
      >
        <p>次へ進む</p>
      </button>
    </div>
  </div>
</template>

<style lang="scss">
@import '@/assets/css/steps.scss';
</style>
