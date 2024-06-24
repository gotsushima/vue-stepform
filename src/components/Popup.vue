<script setup>
import { onMounted } from 'vue';

onMounted(() => {
  const closeButtons = document.querySelectorAll('.close-button');
  closeButtons.forEach((button) => {
    button.addEventListener('click', () => {
      const popup = document.querySelector('.lp_step_question_popup');
      if (popup) {
        popup.style.display = 'none';
      }
    });
  });
});
</script>

<template>
  <div class="lp_step_question_popup">
    <div class="modal">
      <h5 class="popup_title">お気持ちはどちらに近いですか？</h5>
      <button class="close-button first-button">近いうちに転職したい</button>
      <button class="close-button second-button">求人情報を閲覧したい</button>
    </div>
  </div>
</template>

<style scoped lang="scss">
.lp_step_question_popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.6);
  .modal {
    position: relative;
    top: 30%;
    width: 80%;
    padding: 20px;
    margin: 0 auto;
    padding: 3rem 1.5rem;
    border-radius: 4px;
    background-color: #fff;
    text-align: center;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
    .popup_title {
      font-size: 1.2rem;
      font-weight: bold;
      margin-bottom: 1.5rem;
      color: #f60;
    }
    .close-button {
      display: block;
      font-size: 1rem;
      width: 230px;
      margin: 14px auto;
      padding: 1rem;
      color: #fff;
      text-align: center;
      border-radius: 10px;
      box-shadow: 3px 3px 2px rgba(0, 0, 0, 0.3);
      &.first-button {
        background-color: #f60;
      }
      &.second-button {
        background-color: #3dc308;
      }
    }
  }
}
@media screen and (min-width: 460px) {
  .lp_step_question_popup {
    .modal {
      width: 360px;
    }
  }
}
</style>
