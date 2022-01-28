<template>
    <article @click="$emit('handle-delete')" class="card" :class="updateVendor">
        <img class="blipp-icon" src="../../assets/wifi.svg" alt="wifi">
        <img class="sim-card-icon" src="../../assets/chip.svg" alt="chip">
        <img class="card-logo" v-if="updateVendor" :src="require(`../../assets/${updateVendor}.svg`)" alt="updateVendor">
        <p class="bank-number">{{updateCardNumb}}</p>
        <div class="card-holder">
            <h3 class="upper-case static-text">CardHolder name</h3>
            <p class="dynamic-text">{{updateCardName}}</p>
        </div>
        <div class="valid-date">
            <p class="upper-case static-text">Valid Thru</p>
            <p class="dynamic-text">{{fullDate}}</p>
        </div>
    </article>
</template>

<script>
export default {
    props : ['card'],
    data(){
        return{
            cardData : {
                cardNumber : "",
                cardHolderName : "",
                year : "YY",
                month : "MM",
                vendor : ""  
            }
        }
    },
    computed :{
        fullDate(){
            return this.card.month + "/" + this.card.year;
        },
        updateCardNumb(){
            return this.card.cardNumber;
        },
        updateCardName(){
            return this.card.cardHolderName;
        },
        updateYear(){
            return this.card.year;
        },
        updateMonth(){
            return this.card.month;
        },
        updateVendor(){
            return this.card.vendor;
        }
    },
    updated(){
        this.cardData.cardNumber = this.card.cardNumber;
        this.cardData.cardHolderName = this.card.cardHolderName;
        this.cardData.year = this.card.year;
        this.cardData.month = this.card.month;
        this.cardData.vendor = this.card.vendor;
    }
}
</script>

<style>
    .card{
        box-sizing: border-box;
        padding: 1rem;
        max-height: 13.5rem;
        border-radius: 0.5rem;
        background-color: #D0D0D0;
        max-width: 360px;
        display: grid;
        grid-template-columns: repeat(4,1fr);
        grid-template-rows: repeat(4,50px);
        grid-template-areas:
        "blipp . . logo"
        "sim-card . . ."
        "number number number number"
        "name name name date";
    }
    .card p,h2,h3,img{
        margin: 0;
        padding: 0;
    }
    .valid-date{
        grid-area: date;
        display: flex;
        flex-direction: column;
    }
    .valid-date p{
        align-self: flex-end;
    }
    .bank-number{
        grid-area: number;
        font-size: 1.5rem;
        letter-spacing: 0.1rem;
        align-self:center;
    }   
    .card-holder{
        grid-area: name;
    }
    .card-holder h3{
        font-weight: normal;
    }
    .blipp-icon{
        grid-area: blipp;
    }
    .sim-card-icon{
        grid-area: sim-card;
    }
    .card-logo{
        grid-area: logo;
    }
    .card-logo,.blipp-icon,.sim-card-icon{
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .blipp-icon,.sim-card-icon{
        justify-content: flex-start;
        align-items: center;
    }
    .static-text{
        font-size: 0.8rem;
    }
    .dynamic-text{
        font-size: 1rem;
        text-transform: up;
        font-weight:600;
    }
    article .bitcoin{
        background-color:#FFAE34;
        color: #000000;
    }
    article .ninja{
        background-color: #222222;
        color: #FFFFFF;
    }
    article .blockchain{
        background-color: #8B58F9;
        color:  #FFFFFF;
    }
    article .evil{
        background-color: #F33355;
        color:  #FFFFFF;
    }
</style>