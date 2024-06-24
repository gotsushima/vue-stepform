<script setup>
import { inject, watch } from 'vue';

const currentStep = inject('currentStep');
const errorMessages = inject('errorMessages');
const validateStep = inject('validateStep');
const goToStep = inject('goToStep');
const touchField = inject('touchField');
const validateField = inject('validateField');

const name = inject('name');
const year = inject('year');
const years = inject('years');
const errorClasses = inject('errorClasses');

//生まれ年リスト
const currentYear = new Date().getFullYear();
const nextYear = currentYear + 1;
const startYear = nextYear > 2025 ? 1950 + (nextYear - 2025) : 1949;
const totalYears = 58;
years.value = Array.from({ length: totalYears }, (_, i) => {
  const year = startYear + i;
  if (year === 1989) {
    return `${year}/昭和64・平成1`;
  } else if (year >= 1990) {
    const heiseiYear = year - 1988;
    const heiseiLabel = heiseiYear < 10 ? `0${heiseiYear}` : heiseiYear;
    return `${year}/平成${heiseiLabel}`;
  } else {
    return `${year}/昭和${year - 1925}`;
  }
});
year.value = years.value.find((y) => y.startsWith('1980'));

const transitionToStep6 = () => {
  goToStep(6);
};

const handleNameChange = () => {
  touchField('name');
  validateField('name');
};

const handleYearChange = () => {
  touchField('year');
  validateField('year');
  transitionToStep6();
};

watch(name, () => {
  touchField('name');
});
watch(year, () => {
  touchField('year');
});
</script>

<template>
  <div
    id="step5"
    v-show="currentStep === 5"
  >
    <div class="step-img">
      <img
        src="@/assets/images/step-4.png"
        alt="step4"
      />
    </div>

    <dl :class="['form-item', { error: errorClasses.name }]">
      <dt>お名前・生まれ年を入力してください。</dt>
      <dt class="required">お名前</dt>
      <dd>
        <input
          type="text"
          name="name"
          autocomplete="name"
          placeholder="例) 白熊 花子"
          v-model="name"
          @change="handleNameChange"
        />
      </dd>
      <p
        class="error"
        v-if="errorMessages.name"
        v-cloak
      >
        {{ errorMessages.name }}
      </p>
    </dl>

    <dl class="form-item">
      <dt class="required">生まれ年</dt>
      <dd>
        <span class="select-arrow">
          <select
            name="year"
            class="year"
            v-model="year"
            @change="handleYearChange"
          >
            <option
              v-for="yearOption in years"
              :key="yearOption"
              :value="yearOption"
            >
              {{ yearOption }}
            </option>
          </select>
        </span>
      </dd>
    </dl>

    <div class="button-area">
      <button
        type="button"
        class="backbtn"
        id="back5"
        @click="goToStep(currentStep - 1)"
      >
        <p>戻る</p>
      </button>
      <button
        type="button"
        class="nextbtn"
        v-bind:class="{ 'button-shine': validateStep(5) }"
        id="next5"
        @click="goToStep(currentStep + 1)"
        :disabled="!validateStep(5)"
      >
        <p>次が最後の項目</p>
      </button>
    </div>
  </div>
</template>

<style lang="scss">
@import '@/assets/css/steps.scss';
</style>
