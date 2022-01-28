<template>
    <form class="card-form" @submit.prevent="submitForm">
        <div class="card-number">
            <label class="upper-case" for="card-number">Card Number</label>
            <input required :class="{error : errors.CardNumberError}" maxlength="16" @input="validateCardNumber" v-model="cardData.cardNumber" type="text" id="card-number" name="number" placeholder="XXXX XXXX XXXX XXXX">
            <p v-if="errors.CardNumberError" class="text-error">Invalid cardnumber</p>
        </div>
        <div class="card-name">
            <label class="upper-case" for="cardholder-name">Cardholder name</label>
            <input required :class="{error : errors.cardHolderNameError}" maxlength="22" @input="validateCardHolder" v-model="cardData.cardHolderName" type="text" id="cardholder-name" name="cardholder" placeholder="Firstname Lastname">
            <p v-if="errors.cardHolderNameError" class="text-error">Invalid cardnumber</p>
        </div>

        <div class="month">
            <label class="upper-case" for="month">Month</label>
            <select required @change="$emit('monthData', cardData.month)" v-model="cardData.month" name="month" id="month">
                <option v-for="n in 12" :key="n" :value="n">{{n}}</option>
            </select>
        </div>
        <div class="year">
            <label class="upper-case" for="year">Year</label>
            <select required @change="$emit('yearData', cardData.year)" v-model="cardData.year" name="year" id="year">
                <option v-for="n in 12" :key="n" :value="n + 21">{{n + 21}}</option>
            </select>
        </div>
        <div class="vendor">
            <label class="upper-case" for="vendor">Vendor</label>
            <select required @change="$emit('vendorData', cardData.vendor)" v-model="cardData.vendor" name="vendor" id="vendor">
                <option class="upper-case" value="bitcoin">Bitcoin inc</option>
                <option class="upper-case" value="ninja">Ninja bank</option>
                <option class="upper-case" value="blockchain">Block chain inc</option>
                <option class="upper-case" value="evil">Evil corp</option>
            </select>
        </div>          
    </form>
</template>

<script>
export default {
    props : ['cards'],
    data(){
        return{
            cardData : {
                cardNumber : "",
                cardHolderName : "",
                year : "YY",
                month : "MM",
                vendor : ""  
            },
            errors:{
                CardNumberError : "",
                cardHolderNameError : "",
            },
        }
    },
    methods:{
        validateCardNumber(event){
            let data = event.target.value.trim();
            if (/^[0-9]\d*$|^$/.test(data)) {
                this.errors.CardNumberError = "";
                this.$emit('cardNumberData', this.cardData.cardNumber);
                this.$emit('errors', this.errors);
            } 
            else{
                if(this.errors.CardNumberError == ""){
                    this.errors.CardNumberError = "Invalid cardnumber!";
                    this.$emit('errors', this.errors);
                }   
            }
        },
        validateCardHolder(event){
            let data = event.target.value.trim();
            if (/^[a-öA-Ö\s]*$|^$/.test(data)) {
                this.errors.cardHolderNameError = "";
                this.$emit('cardHolderData', this.cardData.cardHolderName.toUpperCase());
                this.$emit('errors', this.errors);
            } 
            else{
                if(this.errors.cardHolderNameError == ""){
                    this.errors.cardHolderNameError = "Invalid cardholder!";
                    this.$emit('errors', this.errors);
                }   
            }
        }   
    }
}
</script>

<style>
    .card-form{
        grid-area: card-form;
        display: grid;
        gap: 1.2rem;
        grid-template-columns: repeat(2, 1fr);
        grid-template-rows: repeat(4,1fr);
        grid-template-areas: 
        "card-number card-number"
        "card-name card-name"
        "month year"
        "vendor vendor";
    }
    .card-number{
        grid-area: card-number;
    }
    .card-name{
        grid-area: card-name;
    }
    .month{
        grid-area: month;
    }
    .year{
        grid-area: year;
    }
    .vendor{
        grid-area: vendor;
    }
    .card-number,.card-name, .month, .year,.vendor{
        display: flex;
        flex-direction: column;
    }
    .card-container{
        grid-area: card;
        margin-bottom: 2rem;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .card-container h2{
        font-size: 0.8rem;
        margin-bottom: 0.4rem;
        color: rgb(56, 56, 55);
    },
    label{
        margin: 0.3rem 0rem;
        font-size: 0.7rem;
    }
    input,select,option{
        border-radius: 0.4rem;
        font-size: 1.2rem;
        padding: 0.8rem;
        border: 1px solid black
    }
    .upper-case{
        text-transform: uppercase;
    }
    .error{
        border: 3px solid red;
        outline: none;
        color: red;
    }
    .text-error{
        color: red;
        margin: 0.2rem;
    }
</style>