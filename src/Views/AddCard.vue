<template>
    <article class="add-card">
        <header>
            <h1 class="upper-case">Add a new bank card</h1>
        </header>
        <main>
            <article class="card-container">
            <h2 class="upper-case">New card</h2>
            <Card
                :card="cardData"
            />
            </article>
            <CardForm
                @cardNumberData="updateCardNumberData" 
                @cardHolderData="updateCardNameData"
                @yearData="updateYearData"
                @monthData="updateMonthData"
                @vendorData="updateVendorData"
                @errors="handleError"
            />
        </main>
        <nav>
            <!-- <button @click="toogleView" class="nav-button upper-case">toogle-view card</button> -->
            <button @click="validateForm" class="nav-button upper-case">Add card</button>
            <p class="text-error" v-if="emptyFields">No empty fields allowed!</p>
            <p  class="text-error" v-if="duplicateCardNumbers">No duplicate card numbers allowed</p>
        </nav> 
    </article>   
</template>

<script>
import Card from "../Components/Card.vue"
import CardForm from "../Components/CardForm.vue"
export default {
    components:{CardForm,Card},
    props:['cards'],
    data(){
        return {
            cardData : {
                cardNumber : "",
                cardHolderName : "",
                year : "YY",
                month : "MM",
                vendor : ""  
            },
            errors : {},
            emptyFields : false,
            duplicateCardNumbers : false,
            listOfCard : []
        }
    },
    created(){
        this.listOfCard = this.cards;
    },
    methods:{
        toogleView(){
            if(this.duplicateCardNumbers == false){
                this.$emit("toogle-view", "home");
                this.$emit("new-card-data", this.cardData);
            }
        },
        updateCardNumberData(data){
            data = this.addSpaces(data);
            this.cardData.cardNumber = data;
        },
        updateCardNameData(data){
            this.cardData.cardHolderName = data;
        },
        updateYearData(data){
            this.cardData.year = data;
        },
        updateMonthData(data){
            this.cardData.month = data;
        },
        updateVendorData(data){
            this.cardData.vendor = data;
        },
        handleError(data){
            this.errors = data;
        },
        validateForm(){
            let isEmptyFields = this.checkIfEmptyFields();
            let errors = this.checkIfErrors();
            this.checkIfDuplicateCardNumber();
            if(isEmptyFields){
                this.emptyFields = true;
                return;
            }
            else{
                this.emptyFields = false;
                if(!errors){
                    this.toogleView(); 
                }
            }
        
        },
        checkIfEmptyFields(){      
            let values = Object.values(this.cardData);
            for(let value of values){
                if(value == ""){
                   return true;
                }
            }
            return false;
        },
        checkIfErrors(){
            let errors = Object.values(this.errors);
            for(let error of errors){
                if(error != ""){
                   return true;
                }
            }
            return false;
        },
        checkIfDuplicateCardNumber(){
            for(let i = 0; i < this.listOfCard.length; i++){
                if(this.listOfCard[i].cardNumber == this.cardData.cardNumber){
                    this.duplicateCardNumbers = true;
                    return;
                }        
            }
            this.duplicateCardNumbers = false;
        },
        addSpaces(data){
            let tempStorageCardNumb = "";
            for(let i = 0; i < data.length;i++){
                if(i > 0 && i % 4 == 0){
                    tempStorageCardNumb += (" " + data.charAt(i)) ;
                    
                }
                else{
                    tempStorageCardNumb += data.charAt(i);
                }
            }
            return tempStorageCardNumb;
        }
    },
    computed :{
        fullDate(){
            return this.cardData.month + "/" + this.cardData.year;
        }
    }
}
</script>

<style scoped>
    body{
        margin: 0;
        padding: 0;
    }
    .add-card{
        margin: 1.2rem;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-rows: 0.8fr 4fr 0.8fr;
        grid-template-areas: 
        "header"
        "main"
        "nav";
    }
    header{
        grid-area: header;
        max-width: 12rem;
        align-self: center;
        justify-self: center;
        text-align: center;
        
        
    }
    header h1{
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 2rem;
        font-weight:900;
    }
    main{
        grid-area: main;
        display: grid;
        grid-template-columns: 1fr;
        grid-template-areas: 1.5fr 2fr;
        grid-template-areas: 
        "card"
        "card-form";
    }
    
    nav{
        grid-area: nav;
    }
    .upper-case{
        text-transform: uppercase;
    }
    nav{
        margin-top: 2rem;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
    }
    nav p{
        align-self: center;
    }
    nav button{
        min-width: 100%;
        border: 1px solid rgb(32, 32, 32);
        background-color: transparent;
        min-height: 4rem;
        border-radius: 0.5rem;
        font-size: 1.2rem;
        font-weight: 700;
    }
    nav button:hover{
        background-color: rgb(32, 32, 32);
        color: rgb(218, 213, 213);
        
    }
    .text-error{
        color: red;
        margin: 0.2rem;
    }
    
</style>