<script setup>
import { ref } from "vue";

const size = ref(9);
const whiteTurn = ref(true);
// const isKo = ref(false);
const isKo = ref({
  x: 9,
  y: 6
});
</script>

<template>
  <main>
    <template v-for="x in size" :key="x">
      <div
        v-for="y in size"
        :key="x + '-' + y"
        :id="x + '-' + y"
        :class="{
          case: true,
          top: x == 1,
          bottom: x == size,
          right: y == size,
          left: y == 1,
          ko: isKo && isKo.x == x && isKo.y == y,
          noir: true
        }"
      >
        <div class="pierre"></div>
      </div>
    </template>
  </main>
</template>

<style lang="postcss" scoped>
main {
  padding: 2.5%;
  display: grid;
  grid-template-rows: repeat(9, 1fr);
  grid-template-columns: repeat(9, 1fr);
  background-color: orange;
}

@media (orientation: landscape) {
  main {
    height: 100vh;
    width: 100vh;
    margin: 0 auto;
  }
}

@media (orientation: portrait) {
  main {
    width: 100vw;
    height: 100vw;
    margin: auto 0;
  }
}

main .case {
  display: flex;
  justify-content: center;
  align-items: center;
}

main .case .pierre {
  width: 66%;
  height: 66%;
  border-radius: 100%;
}

main .case.noir .pierre {
  background-color: #222;
}

main .case.blanc .pierre {
  background-color: #eee;
}

main .case.ko ::before {
  content: '⏳';
  font-size: 30px;
  font-weight: 900;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
