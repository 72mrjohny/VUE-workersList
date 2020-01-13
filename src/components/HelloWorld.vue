<template>
  <div>
    <h1>WorkersList</h1>
    <table>
      <thead>
        <th>IMIĘ</th>
        <th>NAZWISKO</th>
        <th>DZIAŁ</th>
        <th>WYNAGRODZENIE(kwota)</th>
        <th>WYNAGRODZENIE(waluta)</th>
      </thead>
      <tbody>
        <tr v-for="worker in workers" v-bind:key="worker.name">
          <td>{{worker.imie}}</td>
          <td>{{worker.nazwisko}}</td>
          <td>{{worker.dzial}}</td>
          <td>{{worker.wynagrodzenieKwota}}</td>
          <td>{{worker.wynagrodzenieWaluta}}</td>
        </tr>
      </tbody>
    </table>
    <hr />
    <input type="text" placeholder="Imię" v-model="formData.imie" />
    <input type="text" placeholder="Nazwisko" v-model="formData.nazwisko" />
    <input type="text" placeholder="Dział" v-model="formData.dzial" />
    <input type="number" placeholder="Wynagrodzenie" v-model="formData.wynagrodzenieKwota" />
    <select name="Waluta" v-model="formData.wynagrodzenieWaluta">
      <option value="PLN">PLN</option>
      <option value="EUR">EUR</option>
    </select>
    <input type="submit" value="Dodaj pracownika" v-on:click="addWorker" />
    <hr />
    imie:{{formData.imie}}
    <br />
    nazwisko:{{formData.nazwisko}}
    <br />
    dzial:{{formData.dzial}}
    <br />
    Wynagrodzenie(kwota):{{formData.wynagrodzenieKwota}}
    <br />
    Wynagrodzenie(waluta):{{formData.wynagrodzenieWaluta}}
    <br />
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      formData: {
        imie: null,
        nazwisko: null,
        dzial: null,
        wynagrodzenieKwota: null,
        wynagrodzenieWaluta: null
      },
      workers: [
        {
          imie: "Jan",
          nazwisko: "Kowalski",
          dzial: "IT",
          wynagrodzenieKwota: "3000",
          wynagrodzenieWaluta: "PLN"
        },
        {
          imie: "Anna",
          nazwisko: "Bąk",
          dzial: "Administracja",
          wynagrodzenieKwota: "2400.50",
          wynagrodzenieWaluta: "PLN"
        },
        {
          imie: "Paweł",
          nazwisko: "Zabłocki",
          dzial: "IT",
          wynagrodzenieKwota: "3300",
          wynagrodzenieWaluta: "PLN"
        },
        {
          imie: "Tomasz",
          nazwisko: "Osiecki",
          dzial: "Administracja",
          wynagrodzenieKwota: "2100",
          wynagrodzenieWaluta: "PLN"
        },
        {
          imie: "Iwona",
          nazwisko: "Leihs-Gutowska",
          dzial: "Handlowiec",
          wynagrodzenieKwota: "3100",
          wynagrodzenieWaluta: "PLN"
        }
      ]
    };
  },
  methods: {
    addWorker() {
      const newWorker = {
        imie: this.formData.imie,
        nazwisko: this.formData.nazwisko,
        dzial: this.formData.dzial,
        wynagrodzenieKwota: this.formData.wynagrodzenieKwota,
        wynagrodzenieWaluta: this.formData.wynagrodzenieWaluta
      };
      this.workers.push(newWorker);
      this.formData.imie = "";
      this.formData.nazwisko = "";
      this.formData.dzial = "";
      this.formData.wynagrodzenieKwota = "";
      this.formData.wynagrodzenieWaluta = "";
    }
  }
};
</script>


<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
