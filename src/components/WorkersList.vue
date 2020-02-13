<template>
  <div>
    <h1>Lista pracowników</h1>
    <div class="search-wrapper">
      <input type="text" v-model="filters.search" placeholder="Podaj imię / nazwisko" />
      <select name="Dzial" v-model="filters.department">
        <option value>Wszystkie działy</option>
        <option
          v-for="department in allowedDepartments"
          :key="'department-'+department"
          :value="department"
        >{{department}}</option>
      </select>
      <input type="number" v-model="filters.salaryFrom" placeholder="Wynagrodzenie (od)" />
      <input type="number" v-model="filters.salaryTo" placeholder="Wynagrodzenie (do)" />
    </div>
    <div class="table-wrapper">
      <table>
        <thead>
          <th>IMIĘ</th>
          <th>NAZWISKO</th>
          <th>DZIAŁ</th>
          <th>WYNAGRODZENIE(kwota)</th>
          <th>WYNAGRODZENIE(waluta)</th>
        </thead>
        <tbody>
          <tr
            v-for="(worker,index) in workers"
            v-bind:key="worker.name"
            v-show="filteredWorkersIds.indexOf(index) > -1"
          >
            <td>{{worker.imie}}</td>
            <td>{{worker.nazwisko}}</td>
            <td>{{worker.dzial}}</td>
            <td>{{worker.wynagrodzenieKwota}}</td>
            <td>{{worker.wynagrodzenieWaluta}}</td>
          </tr>
        </tbody>
      </table>
      <div class="sum">
        <p>Wynagrodzenie (suma): {{this.totalSum}}</p>
      </div>
    </div>

    <div class="add-wrapper">
      <input type="text" placeholder="Imię" v-model="formData.imie" />
      <input type="text" placeholder="Nazwisko" v-model="formData.nazwisko" />
      <input type="text" placeholder="Dział" v-model="formData.dzial" />
      <input type="number" placeholder="Wynagrodzenie" v-model="formData.wynagrodzenieKwota" />
      <select name="Waluta" v-model="formData.wynagrodzenieWaluta">
        <option disabled selected>Wybierz walutę</option>
        <option value="PLN">PLN</option>
        <option value="EUR">EUR</option>
        <option value="USD">USD</option>
      </select>
      <input type="submit" value="Dodaj pracownika" class="btn" v-on:click="addWorker " />
    </div>
  </div>
</template>

<script>
import workers from "../../data.json";

export default {
  name: "workersList",
  created() {
    this.workers = workers;
    this.setDepartmentsFilter();
  },
  data() {
    return {
      allowedDepartments: [],

      filters: {
        search: "",
        department: "",
        salaryFrom: "",
        salaryTo: ""
      },
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
      if (
        this.formData.imie &&
        this.formData.nazwisko &&
        this.formData.dzial &&
        this.formData.wynagrodzenieKwota &&
        this.formData.wynagrodzenieWaluta
      ) {
        this.workers.push(newWorker);
        this.setDepartmentsFilter();
        this.clearWorkersForm();
      } else {
        alert("wypełnij wszystkie pola formularza aby dodać pracownika");
      }
    },

    clearWorkersForm() {
      this.formData.imie = "";
      this.formData.nazwisko = "";
      this.formData.dzial = "";
      this.formData.wynagrodzenieKwota = "";
      this.formData.wynagrodzenieWaluta = "";
    },

    setDepartmentsFilter() {
      this.allowedDepartments = [
        ...new Set(workers.map(worker => worker.dzial))
      ];
    }
  },
  computed: {
    filteredWorkersIds: function() {
      var filteredIds = [];

      for (const [index, worker] of this.workers.entries()) {
        if (
          this.filters.department != "" &&
          this.filters.department != worker.dzial
        ) {
          continue;
        }

        if (
          this.filters.salaryFrom != "" &&
          this.filters.salaryFrom > parseInt(worker.wynagrodzenieKwota)
        ) {
          continue;
        }

        if (
          this.filters.salaryTo != "" &&
          this.filters.salaryTo < parseInt(worker.wynagrodzenieKwota)
        ) {
          continue;
        }

        if (
          this.filters.search != "" &&
          !worker.imie.toLowerCase().match(this.filters.search.toLowerCase()) &&
          !worker.nazwisko
            .toLowerCase()
            .match(this.filters.search.toLowerCase())
        ) {
          continue;
        }

        filteredIds.push(index);
      }
      return filteredIds;
    },

    totalSum: function() {
      let sum = 0;
      this.workers.forEach((worker, index) => {
        sum =
          this.filteredWorkersIds.indexOf(index) > -1
            ? sum + parseFloat(worker.wynagrodzenieKwota)
            : sum;
      });

      return sum;
    }
  }
};
</script>


<style scoped>
* {
  font-family: "Public Sans", sans-serif;
  box-sizing: border-box;
}

.add-wrapper,
.search-wrapper {
  text-align: left;
  margin-left: 5%;
  margin-right: 5%;
}
.add-wrapper {
  margin-bottom: 50px;
}
.table-wrapper {
  position: relative;
  display: inline-block;
  align-content: left;
  margin-left: 5%;
  margin-right: 5%;
}
input,
select {
  box-sizing: border-box;
  background-color: #4bb2cc;
  color: aliceblue;
  text-shadow: 1px 1px 4px #000000;
  font-size: 15px;
  margin-right: 30px;
  margin-top: 15px;
  width: 200px;
  height: 30px;
  padding: 2px;
  border: 2px solid #4b9db3;
  text-decoration: none;
  box-shadow: 0 0 5px rgb(0, 0, 0);
}

.add-wrapper input {
  display: block;
}
.btn {
  background-color: #4bb2cc;

  text-shadow: 0px 0px 1px rgba(0, 0, 0, 0.932);
  color: #ffffff;
  text-transform: uppercase;
  cursor: pointer;
  transition: 0.3s;
}
.btn:hover {
  color: #000000;
  box-shadow: 0 0 4px rgb(255, 255, 255);
}
::placeholder {
  color: aliceblue;
  opacity: 1;
}

table {
  border-collapse: collapse;
  margin: 50px auto 20px;
  text-align: center;
}

tr {
  text-align: center;
  overflow: hidden;
  border: 1px solid black;
  width: 100%;
  margin: 0;

  color: rgb(14, 13, 12);
  font-size: 15px;
  word-wrap: break-word;
}
h1 {
  color: rgb(255, 242, 172);
  text-shadow: 1px 1px #000000;
}
th {
  background-color: #b37842;
  text-align: center;
  text-transform: uppercase;
  font-weight: normal;
  border: 1px solid black;
  min-width: 180px;
  padding: 3px;
}
td {
  border: 1px solid black;
  text-align: center;
  min-width: 180px;
  padding: 3px;
}
tr:nth-of-type(odd) {
  background-color: rgb(224, 214, 155);
}
tr:nth-of-type(even) {
  background-color: rgb(248, 227, 156);
}
tr:hover {
  background-color: #429cb3;
}

h1 {
  text-align: left;
  margin-left: 5%;
  margin-bottom: 30px;
}
p {
  display: inline-block;
  color: aliceblue;
  font-size: 20px;
  text-shadow: 1px 1px #000000;
}
div.sum {
  padding-right: 10px;
  position: absolute;
  bottom: -40px;
  right: 0;
}
/* SEKCJA FLEX */

@media screen and (max-width: 800px) {
  .table-wrapper {
    margin-bottom: 30px;
  }
  th,
  tr {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    text-align: center;
    margin-bottom: 2px;
  }
  td,
  th {
    width: 100%;
  }
  th:nth-of-type(odd) {
    background-color: #b37942c4;
  }
  td:nth-of-type(odd) {
    background-color: rgb(224, 214, 155);
  }
  td:nth-of-type(even) {
    background-color: rgb(248, 227, 156);
  }
  td:hover {
    background-color: #429cb3;
  }
}
</style>
