<script setup>
import { inject, watch } from 'vue';

const currentStep = inject('currentStep');
const errorMessages = inject('errorMessages');
const validateStep = inject('validateStep');
const goToStep = inject('goToStep');
const touchField = inject('touchField');
const validateField = inject('validateField');

const tel = inject('tel');
const email = inject('email');
const errorClasses = inject('errorClasses');
const toggleAccordion = inject('toggleAccordion');

const handleTelChange = () => {
  touchField('tel');
  validateField('tel');
};

const handleEmailChange = () => {
  touchField('email');
  validateField('email');
};

watch(tel, () => {
  touchField('tel');
});
watch(email, () => {
  touchField('email');
});
</script>

<template>
  <div
    id="step6"
    v-show="currentStep === 6"
  >
    <div class="step-img">
      <img
        src="@/assets/images/step-5.png"
        alt="step5"
      />
    </div>

    <dl :class="['form-item', { error: errorClasses.tel }]">
      <dt>ご連絡先を入力してください。</dt>
      <dt class="required">電話番号</dt>
      <dd>
        <input
          type="tel"
          class="tel"
          name="tel"
          autocomplete="tel"
          placeholder="例）09011112222"
          v-model="tel"
          @change="handleTelChange"
        />
      </dd>
      <p class="tel-comment">お電話にて非公開求人をご紹介いたします。</p>
      <p
        class="error"
        v-if="errorMessages.tel"
        v-cloak
      >
        {{ errorMessages.tel }}
      </p>
    </dl>

    <dl :class="['form-item', { error: errorClasses.email }]">
      <dt
        class="acoBox accordion-head"
        @click="toggleAccordion"
      >
        <span class="optional">メール</span>
      </dt>
      <dd class="accordion-content">
        <input
          type="text"
          name="email"
          autocomplete="email"
          placeholder="例）nanuk@nanuk.jp"
          v-model="email"
          @change="handleEmailChange"
        />
      </dd>
      <p
        class="error"
        v-if="errorMessages.email"
        v-cloak
      >
        {{ errorMessages.email }}
      </p>
    </dl>

    <div class="button-area">
      <button
        type="button"
        class="backbtn"
        id="back6"
        @click="goToStep(currentStep - 1)"
      >
        <p>戻る</p>
      </button>
      <button
        type="submit"
        class="nextbtn loading"
        v-bind:class="{ 'button-shine': validateStep(6) }"
        id="next6"
        :disabled="!validateStep(6)"
      >
        <p>
          <span class="small-text">利用規約と個人情報の取扱いに</span
          ><br />同意して登録
        </p>
      </button>
    </div>

    <div class="gs-register">
      <div class="gs-check">
        <input
          type="hidden"
          name="gs_check"
          value="0"
        />
        <input
          class="gs_check"
          type="checkbox"
          name="gs_check"
          id="gs_check"
          checked="checked"
          value="Nanuk Worker Proにも同時登録する"
        />
        <label for="gs_check">Nanuk Worker Proにも同時登録する</label>
      </div>
      <ul class="gs-poricies">
        <li>
          <a
            href=""
            target="_blank"
            >Nanuk Worker利用規約</a
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<style lang="scss">
@import '@/assets/css/steps.scss';
</style>
