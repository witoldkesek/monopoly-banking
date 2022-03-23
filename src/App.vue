<template>
  <div id="app">
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />
    <table style="width: 100%; text-align: center; border: 2px solid">
      <tr>
        <td>Player</td>
        <td>Balance</td>
      </tr>
      <tr v-for="(card, index) in noBankCards(cards)" :key="card.name">
        <td
          :style="`border-color:${card.color}; border: 2px solid ${card.color}`"
        >
          {{ card.name }}
        </td>
        <td>{{ `${card.balance / 1000000}M` }}</td>
        <td>
          <button v-on:click="deletePlayer(cards, index)" class="button delete">
            <i class="fa fa-close"></i>
          </button>
        </td>
      </tr>
    </table>
    Name:
    <input
      name="playerName"
      v-model="playerName"
      size="3"
      style="margin-bottom: 10px"
    />
    <button v-on:click="addPlayer(cards, playerName)">Add player</button>
    <li
      v-for="(card, index) in noBankCards(cards)"
      :key="card.name"
      class="balance"
    >
      {{ `Player: ${card.name} balance: ${card.balance / 1000000}M` }}
      <button v-on:click="deletePlayer(cards, index)" class="button delete">
        Delete player
      </button>
    </li>
    Send
    <input name="amount" v-model="amount" size="2" />
    from
    <select name="balanceFirst" v-model="cardOne">
      <option v-for="(card, index) in cards" :key="index" :value="index">
        {{ card.name }}
      </option>
    </select>
    to
    <select name="balanceSecond" v-model="cardTwo">
      <option v-for="(card, index) in cards" :key="index" :value="index">
        {{ card.name }}
      </option>
    </select>
    <button
      v-on:click="transferM(cards, cardOne, cardTwo, amount)"
      class="button M"
    >
      M
    </button>
    <button
      v-on:click="transferK(cards, cardOne, cardTwo, amount)"
      class="button K"
    >
      K
    </button>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    const cards = [
      { name: "Bank", balance: 0 },
      { name: "Wiciu", balance: 15000000, color: "blue" },
      { name: "Paweł", balance: 15000000, color: "green" },
      { name: "Krzysiu", balance: 15000000, color: "yellow" },
      { name: "Paulinka", balance: 15000000, color: "purple" },
      { name: "Natka", balance: 15000000, color: "orange" },
    ];
    const history = [];
    return {
      cards,
      history,
    };
  },
  methods: {
    transferK: function (array, idOne, idTwo, amount, event) {
      amount *= 1000;
      array[idTwo].balance += amount;
      array[idOne].balance -= amount;
    },
    transferM: function (array, idOne, idTwo, amount, event) {
      amount *= 1000000;
      array[idOne].balance -= amount;
      array[idTwo].balance += amount;
    },
    deletePlayer(array, id, event) {
      console.log(id);
      array.splice(id + 1, 1);
      console.log(array);
    },
    addPlayer(array, name, event) {
      array.push({ name: name, balance: 15000000 });
    },
    noBankCards(array) {
      return array.filter((a) => a.name !== "Bank");
    },
  },
  setup() {},
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  font-size: 20px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}
.balance {
  margin-bottom: 10px;
}
.button {
  border-radius: 5px;
  border-width: 3px;
  background: white;
}
.M {
  border-color: yellow;
}
.K {
  border-color: blue;
}
.delete {
  border-color: red;
  color: red;
}
</style>
