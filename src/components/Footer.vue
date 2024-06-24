<template>
  <footer>
    <div class="pmark">
      <img
        src="@/assets/images/privacy.gif"
        alt="プライバシーマーク"
      />
      <p class="txt">
        <span>徹底した管理体制で大切な個人情報をお守りします。</span
        ><br />Nanuk Workerはプライバシーマークを取得しております。
      </p>
    </div>

    <ul class="policyBox">
      <li>
        <a
          href=""
          target="_blank"
        >
          利用規約
        </a>
      </li>
      <li>
        <a
          href=""
          target="_blank"
        >
          個人情報の取扱い
        </a>
      </li>
      <li>
        <a
          href=""
          target="_blank"
        >
          個人情報保護方針
        </a>
      </li>
      <li>
        <a
          href=""
          target="_blank"
        >
          会社概要
        </a>
      </li>
    </ul>

    <div class="copyright">
      <p>&copy; Nanuk Worker ,Inc.</p>
    </div>
  </footer>
</template>

<style scoped lang="scss">
footer {
  padding: 3.5% 0;
  background: #faf4e7;
  margin-top: 55%;
  .pmark {
    display: flex;
    justify-content: center;
    align-items: center;
    img {
      width: 11.5%;
    }
    .txt {
      padding-left: 2%;
      line-height: 1.4;
      font-size: 2.6vw;
      span {
        font-weight: bold;
      }
    }
  }

  .policyBox {
    display: flex;
    flex-flow: row wrap;
    justify-content: center;
    padding: 3% 8% 3.5%;
    li {
      font-size: 3.2vw;
      width: 35%;

      &:nth-child(odd) {
        border-right: 1px solid #b3b3b3;
        margin-right: 5%;
      }
      &:nth-child(n + 3) {
        margin-top: 3%;
      }
    }
  }

  .copyright {
    font-size: 2.6vw;
    text-align: center;
  }
}

@media screen and (min-width: 460px) {
  footer {
    .pmark {
      img {
        width: 50px;
      }
      .txt {
        font-size: 12px;
      }
    }

    .policyBox {
      li {
        font-size: 14px;
      }
    }

    .copyright {
      font-size: 12px;
    }
  }
}
</style>
