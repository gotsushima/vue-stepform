<template>
  <header>
    <div class="header-inner">
      <a class="logo">
        <img
          src="@/assets/images/logo.png"
          alt=""
        />
      </a>
    </div>

    <h1>
      <img
        src="@/assets/images/fv.png"
        alt=""
      />
    </h1>
  </header>
</template>

<style scoped lang="scss">
header {
  .header-inner {
    width: 100%;
    background: #fff;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1% 0;
    .logo {
      width: 15%;
    }
  }
}
</style>
