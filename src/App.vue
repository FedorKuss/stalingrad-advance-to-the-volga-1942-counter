<script setup>
import { ref, computed } from 'vue'

const countRussian = ref(0);
const countGerman = ref(0);

const difAttacks = computed(() => countGerman.value - countRussian.value);

const difference = computed(() => {
  if (countRussian.value > countGerman.value) {
    return "Repulse";
  } else if (countRussian.value === 0 && countGerman.value === 0) {
    return "zero"
  } else if (countRussian.value === countGerman.value) {
    return "Stalemate";
  } else if (difAttacks.value > countRussian.value) {
    return "Overrun";
  } else if (countRussian.value < countGerman.value) {
    return "Success";
  }
});

console.log(difference);

const { incrementCountGerman, decrementCountGerman, incrementCountRussian, decrementCountRussian, reset } = (() => {
  const increment = (count) => {
    count.value++;
  };

  const decrement = (count) => {
    if (count.value > 0) {
      count.value--;
    }
  };

  const reset = () => {
    countRussian.value = 0;
    countGerman.value = 0;
  };

  return {
    incrementCountGerman: () => increment(countGerman),
    decrementCountGerman: () => decrement(countGerman),
    incrementCountRussian: () => increment(countRussian),
    decrementCountRussian: () => decrement(countRussian),
    reset
  };
})();

</script>


<template>
  <section id="russian">
    <h3>Russian</h3>
    <h2>Defense Total {{ countRussian }}</h2>
    <br/>
    <button class="counter" type="button" @click="incrementCountRussian">+</button>
    <button class="counter" type="button" @click="decrementCountRussian">-</button>
  </section>
  <section id="german">
    <h3>German</h3>
    <h2>Atack Total {{ countGerman }}</h2>
    <br/>
    <button class="counter" type="button" @click="incrementCountGerman">+</button>
    <button class="counter" type="button" @click="decrementCountGerman">-</button>
  </section>
 
  <section id="result">
    <button type="reset" class="reset" @click="reset">Reset</button>
    <span>Result {{ difAttacks }}</span>
    <div v-if="difference !== 'zero'">
      <h2>{{ difference }}</h2>
      <p v-if="difference === 'Repulse'">If the AT &lt; DT, the German attack has been repulsed and tThere is no effect on the Soviet unit. The Lead Atacking Unit is eliminated and is placed in the Out of Action Box. All of the remaining atacking units are flipped to their Spent side. Retreat is required in cases of a failed Mandatory Attack (9.1). Reduce German Morale -1.</p>
      <p v-if="difference === 'Stalemate'">If the AT = DT, the German attack has suffered a Stalemate. There is no effect on the Soviet unit. All attacking units are flipped to their Spent side.</p>
      <p v-if="difference === 'Overrun'">If the attack is a Success and the difference between the AT and DT is greater than the Defense Strength of the Soviet unit, an Overrun has been achieved. The Soviet Unit is eliminated and removed from the map. All attacking units are flipped to their Spent side. Place a German Control marker into the Area. Cancels the effects of the “Heroes,” “Ambush,” and “Fanatic” Soviet Defense Strategies. Advance the “German Controlled” markers on the Record Track as needed.</p>
      <p v-if="difference === 'Success'"> If the AT > DT, the German attack has achieved a Success. The Soviet unit is eliminated and removed from the map. All attacking units are flipped to their Spent side. Place a German Control marker in the Area. Advance the “German Controlled” markers on the Record Track as needed.</p>
    </div>
  </section>
</template>

<style scoped>
  h2 {
    font-size: 2rem;
    margin-bottom: 0;
  }
  h3 {
    font-size: 1.5rem;
    margin-bottom: 0;
  }
  p {
    font-size: 1.3rem;
  }
  section {
    display: flex;
    flex-direction: column;
  }
  #russian {
    background-color: brown;
  }

  #german {
    background-color: black;
  }

  button {
    margin: 5px;
  }

  button.counter {
    font-size: 1.5rem;
  }

  .reset {
    background-color: brown;
    font-size: 1.5rem;
  }
</style>
