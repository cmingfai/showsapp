<script setup>
  import {ref,watch} from "vue"
  import axios from "axios"
  import Card from "./Card.vue"

  const characters=ref(null)
  const page =ref(1)

  const occupation=ref(["Teacher","Fireman","Police","Nurse","Engineer","Manager"])
  
  const onlyUnique=(value, index, array) => {
     return array.indexOf(value) === index;
  }

  const nextPage=()=>{
    page.value=page.value+1
  }

  const prevPage=()=> {
    if (page.value>1) {
      page.value=page.value-1
    }
  }

  function randomOccupation() {
    return [
    occupation.value[Math.floor(Math.random() * occupation.value.length)],
    occupation.value[Math.floor(Math.random() * occupation.value.length)],
    occupation.value[Math.floor(Math.random() * occupation.value.length)]
    ].filter(onlyUnique).sort()
  }


  const response=await axios.get("https://randomuser.me/api/?results=8&page=1&seed=abc")
  characters.value=response.data.results
  console.log("=== BreakingBadCards ===")
  console.log(characters.value)

  characters.value.forEach(function (item, index) {
    item.occupation=randomOccupation()
  });

  console.log("=== BreakingBadCards ===")
  console.log(characters.value)
  watch(page,async() => {
    const res=await axios.get(`https://randomuser.me/api/?results=8&page=${page.value}&seed=abc`)
    characters.value=res.data.results
    characters.value.forEach(function (item, index) {
      item.occupation=randomOccupation()
    });
  })

 
</script>

<template>
  <div class="container">
    <div class="cards">
      <Card 
      v-for="character in characters" :key="character.email"
      :image="character.picture.large"
      :name="`${character.name.first} ${character.name.last}`"
     >
    
       <div class="jobs">
       <p v-for="(job,index) in character.occupation" :key="job">
        {{ job }}<span v-if="index<character.occupation.length-1">,&nbsp;</span>
    </p>

       </div>
      </Card>
    </div>
    <div class="button-container">
      <button @click="prevPage">&lt;</button>
      <button @click="nextPage">&gt;</button>
    </div>
  </div>
</template>

<style scoped>
.container {
    background-color: rgb(27, 26, 26);
    padding: 30px
}
.cards {
    max-width: 1000px;
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    /* height: 700px */
   
}
.cards h3 {
    font-weight: bold;
}
.cards p {
    font-size: 10px;
}
.jobs {
    display: flex;
    flex-wrap: wrap;
}
.button-container {
    display: flex;
    justify-content: center;
    padding-top: 30px
}
.button-container button {
    border: none;
    width: 50px;
    height: 50px;
    border-radius: 100%;
    margin: 0 5px;
    cursor: pointer;
}
.spinner {
    display: flex;
    align-items: center;
    justify-content: center;
}

p {
    font-size: 10px;
}

.jobs {
    display: flex;
    flex-wrap: wrap;
}
</style>