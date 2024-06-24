<script setup>
import { ref, reactive, provide } from 'vue';

import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Popup from './components/Popup.vue';
import Step1 from './components/steps/Step1.vue';
import Step2 from './components/steps/Step2.vue';
import Step3 from './components/steps/Step3.vue';
import Step4 from './components/steps/Step4.vue';
import Step5 from './components/steps/Step5.vue';
import Step6 from './components/steps/Step6.vue';

const currentStep = ref(1);
const qualification = ref([]);
const workStyle = ref([]);
const requestDate = ref('');
const situation = ref('');
const zipCode = ref('');
const prefecture = ref('');
const city = ref('');
const name = ref('');
const year = ref('');
const years = ref([]);
const tel = ref('');
const email = ref('');
const errorMessages = reactive({});
const touchedFields = reactive({});
const errorClasses = reactive({
  zipCode: false,
  prefecture: true,
  city: true,
  name: true,
  tel: true,
  email: false,
});

const validateField = (field) => {
  let errorMessage = '';
  if (field === 'qualification' && qualification.value.length === 0) {
    errorMessage = '保有資格は必須項目です';
  }
  if (field === 'workStyle' && workStyle.value.length === 0) {
    errorMessage = '希望勤務形態は必須項目です';
  }
  if (field === 'zipCode' && zipCode.value) {
    zipCode.value = zipCode.value.replace(/[^0-9]/g, '');
    if (zipCode.value.length !== 7 || !/^\d{7}$/.test(zipCode.value)) {
      errorMessage = '郵便番号は7文字でなければなりません';
    }
  }
  if (field === 'prefecture' && !prefecture.value) {
    errorMessage = '都道府県は必須項目です';
  }
  if (field === 'city' && !city.value) {
    errorMessage = '市区町村は必須項目です';
  }
  if (field === 'name' && !name.value) {
    errorMessage = '名前は必須項目です';
  }
  if (field === 'tel') {
    const toHalfWidth = (str) => {
      return str.replace(/[０-９]/g, function (char) {
        return String.fromCharCode(char.charCodeAt(0) - 0xfee0);
      });
    };
    tel.value = toHalfWidth(tel.value).replace(/[^0-9]/g, '');
    if (!tel.value) {
      errorMessage = '電話番号は必須項目です';
    } else if (tel.value.length < 10) {
      errorMessage = '電話番号は10文字以上でなければなりません';
    } else if (tel.value.length > 11) {
      errorMessage = '電話番号は11文字以内にしてください';
    }
  }
  if (field === 'email') {
    const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    if (email.value && !emailPattern.test(email.value)) {
      errorMessage = 'メールアドレスは有効なメールアドレスではありません';
    }
  }
  if (touchedFields[field]) {
    errorMessages[field] = errorMessage;
  }
  errorClasses[field] = errorMessage !== '';
  return errorMessage === '';
};

const touchField = (field) => {
  if (!touchedFields[field]) {
    touchedFields[field] = true;
  }
  validateField(field);
};

const validateStep = (step) => {
  let isValid = true;
  if (step === 1) {
    isValid = validateField('qualification') && isValid;
  }
  if (step === 2) {
    isValid = validateField('workStyle') && isValid;
    isValid = validateField('requestDate') && isValid;
  }
  if (step === 3) {
    isValid = validateField('situation') && isValid;
  }
  if (step === 4) {
    isValid = validateField('prefecture') && isValid;
    isValid = validateField('city') && isValid;
  }
  if (step === 5) {
    isValid = validateField('name') && isValid;
    isValid = validateField('year') && isValid;
  }
  if (step === 6) {
    isValid = validateField('tel') && isValid;
  }
  return isValid;
};

const clearErrorMessages = () => {
  Object.keys(errorMessages).forEach((key) => {
    errorMessages[key] = '';
  });
};

const goToStep = (step) => {
  if (step > currentStep.value) {
    if (validateStep(currentStep.value)) {
      clearErrorMessages();
      currentStep.value = step;
      window.scrollTo(0, 0);
    }
  } else {
    currentStep.value = step;
    window.scrollTo(0, 0);
  }
};

const toggleAccordion = (event) => {
  const head = event.currentTarget;
  head.classList.toggle('active');
};

provide('currentStep', currentStep);
provide('errorMessages', errorMessages);
provide('validateStep', validateStep);
provide('goToStep', goToStep);
provide('touchField', touchField);
provide('validateField', validateField);
provide('qualification', qualification);
provide('workStyle', workStyle);
provide('requestDate', requestDate);
provide('situation', situation);
provide('zipCode', zipCode);
provide('prefecture', prefecture);
provide('city', city);
provide('name', name);
provide('year', year);
provide('years', years);
provide('tel', tel);
provide('email', email);
provide('toggleAccordion', toggleAccordion);
provide('errorClasses', errorClasses);
</script>

<template>
  <Header />
  <main>
    <Step1 />
    <Step2 />
    <Step3 />
    <Step4 />
    <Step5 />
    <Step6 />
  </main>
  <Footer />
  <Popup />
</template>

<style scoped></style>
