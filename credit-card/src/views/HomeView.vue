<template>
  <div class="main">
    <h1>Cards Management App</h1>
    <div class="inp-box">
      <label>Card Number</label>
      <input type="number" class="inp" v-model="card_data.card_number" />
    </div>
    <div class="inp-box">
      <label>Card Holder Name</label>
      <input type="text" class="inp" v-model="card_data.holder_name" />
    </div>
    <div class="inp-box">
      <label>CCV</label>
      <input type="number" class="inp" v-model="card_data.cvv" />
    </div>
    <div class="inp-box">
      <label>Expiry Date</label>
      <div>
        <input
          type="number"
          class="exp-inp"
          placeholder="MM"
          v-model="card_data.expiry_date_month"
        />
        <span>/</span>
        <input
          type="number"
          class="exp-inp"
          placeholder="YY"
          v-model="card_data.expiry_date_year"
        />
      </div>
    </div>
    <input class="button" type="button" value="Add" @click="add" />
    <div style="border: 1px dashed red; width: 50%; margin: 20px"></div>
    <ul>
      <li v-for="card in card_data.cards" :key="card.id">
        <label>Holder Name: {{ card.holder_name }}</label>
        <label>Number: {{ card.card_number }}</label>
        <label>CVV: {{ card.cvv }}</label>
        <label
          >Expiry Date {{ card.expiry_date_month }}/{{
            card.expiry_date_year
          }}</label
        >
        <input class="remove_btn" type="button" value="Remove" @click="remove(card.id)" />
      </li>
    </ul>
  </div>
</template>


<style>
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.inp {
  width: 400px;
  height: 40px;
  font-size: 20px;
}
.exp-inp {
  width: 175px;
  height: 40px;
  font-size: 20px;
  text-align: center;
}
.inp-box {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 20px;
}
label {
  font-size: 25px;
  font-weight: 900;
}
span {
  padding-left: 20px;
  padding-right: 20px;
  font-size: 25px;
  font-weight: 700;
}
.button {
  width: 200px;
  height: 40px;
  font-size: 20px;
}
.remove_btn{
  width: 160px;
  height: 40px;
  font-size: 20px;
  align-self: center;
  margin-top: 10px;
  border-radius: 20px;
  background-color: rgb(171, 5, 5);
  color:white;
  border-color: rgb(171, 5, 5);
}
ul {
  display: flex;
  flex-direction: row;
  justify-content: center;
  flex-wrap: wrap;
}
li {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border: 1px solid grey;
  padding: 20px;
  margin: 20px;
  border-radius: 20px;
}
</style>

<script>
import { reactive, watch } from "vue";
export default {
  setup() {
    const card_data = reactive({
      card_number: "",
      holder_name: "",
      cvv: "",
      expiry_date_month: "",
      expiry_date_year: "",
      cards: [],
    });

    watch(card_data, (newValue) => {
      if (String(newValue.card_number).length > 14) {
        alert("Card Number can have only 14 digits");
        card_data.card_number = Number(
          String(card_data.card_number).slice(0, -1)
        );
      }
      if (String(newValue.cvv).length > 3) {
        alert("CVV can have only 3 digits");
        card_data.cvv = Number(String(card_data.cvv).slice(0, -1));
      }
      if (
        newValue.expiry_date_month != "" &&
        (newValue.expiry_date_month > 12 || newValue.expiry_date_month < 1)
      ) {
        alert(newValue.expiry_date_month + " is invalid month");
        card_data.expiry_date_month = "";
      }
      if (
        String(newValue.expiry_date_year) != "" &&
        newValue.expiry_date_year < 1
      ) {
        alert(newValue.expiry_date_year + " is invalid year");
        card_data.expiry_date_year = "";
      }
      if (
        String(newValue.expiry_date_year) != "" &&
        String(newValue.expiry_date_year).length > 2
      ) {
        alert("Year can have only 2 digits");
        card_data.expiry_date_year = "";
      }
    });

    function add() {
      const new_card = {
        id: Math.random(),
        card_number: card_data.card_number,
        holder_name: card_data.holder_name,
        cvv: card_data.cvv,
        expiry_date_month: card_data.expiry_date_month,
        expiry_date_year: card_data.expiry_date_year,
      };
      card_data.cards.push(new_card);
      card_data.card_number = "";
      card_data.holder_name = "";
      card_data.cvv = "";
      card_data.expiry_date_month = "";
      card_data.expiry_date_year = "";
    }
    function remove(id){
      card_data.cards = card_data.cards.filter((card)=>card.id!=id)
    }

    return {
      card_data,
      add,
      remove
    };
  },
};
</script>
