<template>
    <section class="list-of-cards">
        <h2 class="upper-case">Your cards</h2>
        <Card v-for="(card,index) in cardList" :key="card.cardNumber" 
            class="pos-card" 
            :style="{transform: `translateY(${index * 2.4}rem)`}" 
            :class="card.vendor"
            :card="card"
            @click.native="$emit('active-card', card)"
        />
    </section>
</template>

<script>
import Card from "./Card.vue"
export default {
    components:{Card},
    props :['cardList'],
    data(){
        return{

        }
    }
}
</script>

<style>
    .list-of-cards{
        min-height: 100%;
        grid-area: cards;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .pos-card{
        position:absolute;
        top: 26rem;
    }
</style>