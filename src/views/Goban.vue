<script setup>
import { ref, onMounted } from "vue";

const size = ref(9);
const whiteTurn = ref(true);
const isKo = ref(false);
// const isKo = ref({
//   x: 9,
//   y: 6,
// });
const map = ref({});

onMounted(() => {
  for (var i = 1; i <= size.value; i++) {
    var array = {};
    for (var j = 1; j <= size.value; j++) {
      array[j] = undefined;
    }
    map.value[i] = array;
  }
});

const hasStone = (x, y) => map.value[x][y] !== undefined;
const hasKo = (x, y) => isKo && isKo.x == x && isKo.y == y;
const voisins = (x, y) => {
  var array = [];
  if (map.value[x + 1][y] === map.value[x][y]) array.push({ x: x + 1, y: y });
  if (map.value[x][y] === map.value[x][y]) array.push({ x: x, y: y });
  if (map.value[x][y + 1] === map.value[x][y]) array.push({ x: x, y: y + 1 });
  if (map.value[x + 1][y + 1] === map.value[x][y]) array.push({ x: x + 1, y: y + 1 });

  return array;
};
const libertes = (x, y) => {
  var array = [];
  if (map.value[x + 1][y] === undefined) array.push({ x: x + 1, y: y });
  if (map.value[x][y] === undefined) array.push({ x: x, y: y });
  if (map.value[x][y + 1] === undefined) array.push({ x: x, y: y + 1 });
  if (map.value[x + 1][y + 1] === undefined) array.push({ x: x + 1, y: y + 1 });

  return array;
};

const play = (x, y) => {
  if (hasStone(x, y) || hasKo(x, y)) return;
  console.log("play", x, y);

  map.value[x][y] = whiteTurn.value;
  whiteTurn.value = !whiteTurn.value;
};
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
          haut: x == 1,
          bas: x == size,
          droite: y == size,
          gauche: y == 1,
          ko: hasKo(x, y),
          blanc: map[x][y] !== undefined && map[x][y] === true,
          noir: map[x][y] !== undefined && map[x][y] === false,
        }"
      >
        <div class="pierre" @click="play(x, y)"></div>
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
  background-image: url("https://raw.githubusercontent.com/atarnowsky/go-assets/master/images/board/wood_full_lighter.jpg");
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
  border: 1px solid black;
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

main .case:not(.blanc, .noir) .pierre:hover {
  border: 3px solid rgba(255, 255, 255, 0.5);
  cursor: pointer;
}

main .case.ko ::before {
  content: "⏳";
  font-size: 30px;
  font-weight: 900;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
