<script setup lang="ts">
import {Gender, Length, names, Popularity} from "@/data";

interface OptionState {
  gender: Gender;
  popularity: Popularity;
  length: Length;
};
const obj: OptionState = {
  gender: Gender.GIRL,
  length: Length.SHORT,
  popularity: Popularity.TRENDY,
}
const options = reactive<OptionState>({
  gender: Gender.GIRL,
  popularity: Popularity.TRENDY,
  length: Length.SHORT,
});

const selectedNames = ref<string[]>([]);

const computeSelectedNames = () => {
  const filteredNames = names
      .filter((name) => name.gender === options.gender)
      .filter((name) => name.popularity === options.popularity)
      .filter((name) => {
        if (options.length === Length.ALL) return true
        else return name.length === options.length
      })

  selectedNames.value = filteredNames.map((name) => name.name);
};

const optionsArray = [
  {
    title: "1) Choose a gender",
    category: "gender",
    buttons: [Gender.GIRL, Gender.UNISEX, Gender.BOY]
  },
  {
    title: "2) Choose the name's popularity",
    category: "popularity",
    buttons: [Popularity.TRENDY, Popularity.UNIQUE]
  },
  {
    title: "3) Choose the name's length",
    category: "length",
    buttons: [Length.LONG, Length.ALL, Length.SHORT]
  }
]
</script>
<template>
  <div class="container">
    <h1>Baby name generator</h1>
    <p>Choose your options and click the find name button bellow.</p>
    <div class="options-container">
      <Option/>
      <div class="option-container">
        <div v-if="optionsArray">
          <Option
              v-for="option in optionsArray"
              :key="option.title"
              :option="option"
              :options="options"
          />
        </div>
      </div>
      <button class="primary" @click="computeSelectedNames">Find Names</button>
      <div class="cards-container">
        <div v-for="name in selectedNames" :key="name" class="card">
          <h4>{{ name }}</h4>
          <p>x</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

h1 {
  font-size: 3rem;
}

.options-container {
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 4rem auto 0;
  position: relative;
}

.options-container {
  margin-bottom: 2rem;
}

.option {
  background-color: white;
  outline: 0.15rem solid rgb(249, 87, 89);
  border: none;
  padding: 0.75rem;
  width: 12rem;
  font-size: 1rem;
  color: rgb(27, 60, 138);
}

.option-left {
  border-radius: 1rem 0 0 1rem;
}

.option-right {
  border-radius: 0 1rem 1rem 0;
}

.option-active {
  background-color: rgb(249, 87, 89);
  color: white;
}

.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border-radius: 6.5rem;
  border: none;
  padding: 0.75rem 4rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}

.cards-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}

.card {
  background-color: rgb(27, 60, 138);
  width: 28%;
  color: white;
  border-radius: 1rem;
  padding: 1rem;
  margin-right: 0.5rem;
  margin-bottom: 1rem;
  position: relative;
}

.card p {
  position: absolute;
  top: -30%;
  left: 92.5%;
  cursor: pointer;
  color: rgb(255, 255, 0.178);
}
</style>
