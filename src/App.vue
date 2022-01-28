<template>
  <div class="wrapper">
    <HomeView v-if="currentView === 'home'" @toogle-view="toogleView" :cards="cards" @update-list="updateList"/>
    <AddCardView v-else @toogle-view="toogleView" @new-card-data="handleArrayOfCards" :cards="cards"/>

  </div>
</template>

<script>
import HomeView from "./Views/Home.vue"
import AddCardView from "./Views/AddCard.vue"
export default {
  components : {HomeView, AddCardView},
  data(){
    return{
      currentView : "home",
      cards : [],
    }
  },
  created(){
      if(localStorage.getItem("cards") != null){
        this.cards = JSON.parse(localStorage.getItem("cards"));
      }
  },
  updated(){  
      localStorage.setItem("cards", JSON.stringify(this.cards));
      
  },
  methods : {
    toogleView(newView){
      this.currentView = newView;
    },
    handleArrayOfCards(newCard){
      this.cards.push(newCard);
    },
    updateList(newCards){
      this.cards = newCards
    }
  }
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=PT+Mono&display=swap');
  @import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;700;900&display=swap');

  body{
      padding: 0;
      margin: 0;
      min-height: 100vh;
  }
  .wrapper{
    min-height: 100vh;
  }
  p,button{
      font-family: 'PT Mono', monospace;
  }
  h1,h2,h3{
    font-family: 'Source Sans Pro', sans-serif;
  }
</style>
