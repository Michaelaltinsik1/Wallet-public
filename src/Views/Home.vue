<template>
    <article class="home">
        <header>
            <h1 class="upper-case">E-wallet</h1>
        </header>
        <main>
            <section class="active-card-container">
                <h2 class="upper-case">active card</h2>
                <Card @handle-delete="changeStatusDelete" v-if="activeCard" :card="activeCard"/>
                <confirmationDialog :deletedStatus="deleteCard" @handle-deletion="handleDeletion"/>
            </section>
            <CardList 
                :cardList="listOfCards"
                @active-card="updateActiveCard"
            />
        </main>
        <nav>
            <button @click="toogleView" class="nav-button upper-case">Add a new card</button>
        </nav>
    </article> 
</template>

<script>
import Card from '../Components/Card.vue';
import CardList from "../Components/CardList.vue"
import confirmationDialog from "../Components/Dialog.vue"
export default {
    components:{CardList, Card,confirmationDialog},
    props:['cards'],
    data(){
        return{
            listOfCards : [],
            activeCard : "",
            deleteCard : false
        }
    },
    methods:{
        updateActiveCard(newActiveCard){
            this.activeCard = newActiveCard;
        },
        toogleView(){
            this.$emit('toogle-view', 'addcard');
            this.$emit("card-list", this.listOfCards);
        },
        changeStatusDelete(){
            this.deleteCard = true;
        },
        handleDeletion(data){
            const newArr = [];
            if(data != "cancel"){
                for(let card of this.listOfCards){
                if(card.cardNumber != this.activeCard.cardNumber){
                    newArr.push(card);
                    }
                }
                this.listOfCards = newArr;
                this.activeCard = "";
                this.$emit("update-list", this.listOfCards);
            }
            this.deleteCard = false;
        }
    },
    created(){
        this.listOfCards = this.cards;
    }
}
</script>

<style scoped>
    body,html{
        padding: 0;
        margin: 0;
        min-height: 100vh;
    }
    header{
        margin-bottom: 0.4rem;
    }
    main{
        display: flex;
        flex-direction: column;
        align-items: center;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 15rem 1fr;
        grid-template-areas: 
        "active-card"
        "cards";  
    }
    .active-card-container{
        min-height: 100%;
        grid-area: active-card;
        margin-bottom: 1rem;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    header{
        display: flex;
        align-items: center;
        justify-content: center;  
    }
    h1{
        font-size: 2rem;
        font-weight:900;
    }
    .list-of-cards h2, .active-card-container h2{
        display: flex; 
        flex-direction: column;
        text-align: center;
        font-size: 0.8rem;
        margin-bottom: 0.6rem;
        color: rgb(56, 56, 55);
    }
    
    .home{
        min-height: 100vh;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 120px 1fr 100px;
        margin: 1.2rem;
    }
    .upper-case{
        text-transform: uppercase;
    }
    nav{
        margin: 1rem 0rem;
        display: flex;
        align-items: flex-end;
        justify-content: center;
    }
    nav button{
        min-width: 100%;
        border: 1px solid rgb(32, 32, 32);
        background-color: white;
        min-height: 4rem;
        border-radius: 0.5rem;
        font-size: 1.2rem;
        font-weight: 700;
    }
    nav button:hover{
        background-color: rgb(32, 32, 32);
        color: rgb(218, 213, 213);
        
    }
</style>