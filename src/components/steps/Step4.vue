<script setup>
import { inject, watch, ref, reactive } from 'vue';

import address from '@/assets/js/address.js';

const currentStep = inject('currentStep');
const errorMessages = inject('errorMessages');
const validateStep = inject('validateStep');
const goToStep = inject('goToStep');
const touchField = inject('touchField');

const zipCode = inject('zipCode');
const prefecture = inject('prefecture');
const city = inject('city');
const errorClasses = inject('errorClasses');
const toggleAccordion = inject('toggleAccordion');

const addressData = reactive(address);
const cities = ref([]);

const transitionToStep5 = () => {
  goToStep(5);
};

const validateZipCode = (field) => {
  let errorMessage = '';
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
  errorClasses[field] = errorMessage !== '';
  return errorMessage === '';
};

const onChangeZipCode = () => {
  const zipcode = zipCode.value.replace(/-/g, '');
  if (!zipcode) {
    errorMessages.zipCode = '';
    return;
  }
  if (validateZipCode('zipCode')) {
    fetch(`https://api.zipaddress.net/?zipcode=${zipcode}`, {
      mode: 'cors',
    })
      .then((response) => response.json())
      .then((data) => {
        if (data.code === 200) {
          prefecture.value = data.data.pref;
          cities.value = addressData[prefecture.value] || [];
          city.value = data.data.city;
          errorMessages.zipCode = '';
        } else {
          errorMessages.zipCode =
            '指定された郵便番号に対応する住所が見つかりません。';
          errorClasses.zipCode = true;
        }
      })
      .catch((error) => {
        errorMessages.zipCode = '住所情報の取得中にエラーが発生しました。';
        errorClasses.zipCode = true;
      });
  }
};

const onChangePrefecture = () => {
  if (prefecture.value) {
    cities.value = addressData[prefecture.value] || [];
  } else {
    cities.value = [];
  }
  city.value = '';
};

const handlePrefectureChange = () => {
  touchField('prefecture');
  validateZipCode('prefecture');
  onChangePrefecture();
};

const handleCityChange = () => {
  touchField('city');
  validateZipCode('city');
  transitionToStep5();
};

watch(zipCode, () => {
  touchField('zipCode');
  onChangeZipCode();
});
watch(prefecture, () => {
  touchField('prefecture');
});
watch(city, () => {
  touchField('city');
});
</script>

<template>
  <div
    id="step4"
    
    v-show="currentStep === 4"
  >
    <div class="step-img">
      <img
        src="@/assets/images/step-3.png"
        alt="step3"
      />
    </div>

    <dl :class="['form-item', { error: errorClasses.zipCode }]">
      <dt>現在のおすまいは？</dt>
      <dt
        class="acoBox accordion-head"
        @click="toggleAccordion"
      >
        <span class="optional">郵便番号</span>
      </dt>
      <dd class="accordion-content">
        <input
          type="tel"
          name="zipCode"
          autocomplete="postal-code"
          placeholder="例）5410047"
          v-model="zipCode"
          @change="onChangeZipCode"
        />
      </dd>
      <p
        class="error"
        v-if="errorMessages.zipCode"
        v-cloak
      >
        {{ errorMessages.zipCode }}
      </p>
    </dl>

    <dl
      :class="[
        'form-item',
        { error: errorClasses.prefecture || errorClasses.city },
      ]"
    >
      <dt class="required">住所</dt>
      <dd>
        <span class="pref-box">
          <span class="select-arrow">
            <select
              name="prefecture"
              autocomplete="address-level1"
              class="prefecture"
              v-model="prefecture"
              @change="handlePrefectureChange"
            >
              <option value="">都道府県選択</option>
              <option
                v-for="(name, index) in Object.keys(addressData)"
                :key="index"
                :value="name"
              >
                {{ name }}
              </option>
            </select>
          </span>
          <p
            class="error"
            v-if="errorMessages.prefecture"
            v-cloak
          >
            {{ errorMessages.prefecture }}
          </p>
        </span>
        <span class="pref-box">
          <span class="select-arrow">
            <select
              name="city"
              autocomplete="address-level2"
              class="city"
              v-model="city"
              @change="handleCityChange"
            >
              <option value="">選択してください</option>
              <option
                v-for="city in cities"
                :key="city"
                :value="city"
              >
                {{ city }}
              </option>
            </select>
          </span>
          <p
            class="error"
            v-if="errorMessages.city"
            v-cloak
          >
            {{ errorMessages.city }}
          </p>
        </span>
      </dd>
    </dl>

    <div class="button-area">
      <button
        type="button"
        class="backbtn"
        id="back4"
        @click="goToStep(currentStep - 1)"
      >
        <p>戻る</p>
      </button>
      <button
        type="button"
        class="nextbtn"
        v-bind:class="{ 'button-shine': validateStep(4) }"
        id="next4"
        @click="goToStep(currentStep + 1)"
        :disabled="!validateStep(4)"
      >
        <p>次へ進む</p>
      </button>
    </div>
  </div>
</template>

<style lang="scss">
@import '@/assets/css/steps.scss';
</style>
