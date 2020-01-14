<template>
  <div>
    <h1>Lista pracowników</h1>
    <input type="text" v-model="filters.search" placeholder="Podaj imię / nazwisko pracownika" />
    <select name="Dzial" v-model="filters.department">
      <option value>Wszystkie</option>
      <option
        v-for="department in allowedDepartments"
        :key="'department-'+department"
        :value="department"
      >{{department}}</option>
    </select>
    <input type="number" v-model="filters.salaryFrom" placeholder="Płaca (od)" />
    <input type="number" v-model="filters.salaryTo" placeholder="Płaca (do)" />
    <br />
    <hr />
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
    <hr />
    <p>Wynagrodzenie(suma):{{this.totalSum}}</p>
    <br />
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
    <p>imie:{{formData.imie}}</p>

    <p>nazwisko:{{formData.nazwisko}}</p>

    <p>dzial:{{formData.dzial}}</p>

    <p>Wynagrodzenie(kwota):{{formData.wynagrodzenieKwota}}</p>

    <p>Wynagrodzenie(waluta):{{formData.wynagrodzenieWaluta}}</p>
    <br />
  </div>
</template>

<script>
import workers from "../../data.json";

export default {
  name: "HelloWorld",
  created() {
    this.workers = workers;
    this.setDepartmentsFilter();
  },
  data() {
    return {
      //filteredWorkersIds: [1, 3],
      allowedDepartments: [],
      search: "",
      searchDepartment: "",
      valueA: "",
      valueB: "",
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
      if (this.formData.imie && this.formData.nazwisko) {
        this.workers.push(newWorker);
        this.setDepartmentsFilter();
        this.clearWorkersForm();
      } else {
        alert("uzupełnij imię lub nazwisko");
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
    },

    filterArray() {
      let a = this.valueA;
      let b = this.valueB;
      let workers = this.workers;

      workers.filter(worker => {
        if (worker.wynagrodzenieKwota > a && worker.wynagrodzenieKwota < b) {
          console.log(worker);
          return (this.workers = this.workers.slice());
        }
        if (worker.wynagrodzenieKwota > a && worker.wynagrodzenieKwota == "") {
          console.log("war. 2");
        }
        if (worker.wynagrodzenieKwota == "" && worker.wynagrodzenieKwota < b) {
          console.log("war. 3");
        }
      });
    }
  },
  computed: {
    filteredWorkers: function() {
      return this.workers.filter(worker => {
        return (
          worker.imie.toLowerCase().match(this.search.toLowerCase()) ||
          worker.nazwisko.toLowerCase().match(this.search.toLowerCase())
        );
      });
    },

    filteredWorkersIds: function() {
      var filteredIds = [];
      console.log(filteredIds);

      for (const [index, element] of this.workers.entries()) {
        if (
          this.filters.department != "" &&
          this.filters.department != element.dzial
        ) {
          continue;
        }

        if (
          this.filters.salaryFrom != "" &&
          this.filters.salaryFrom > element.wynagrodzenieKwota
        ) {
          continue;
        }

        if (
          this.filters.salaryTo != "" &&
          this.filters.salaryTo < element.wynagrodzenieKwota
        ) {
          continue;
        }

        if (
          this.filters.search != "" &&
          !element.imie
            .toLowerCase()
            .match(this.filters.search.toLowerCase()) &&
          !element.nazwisko
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
input,
select {
  background-color: antiquewhite;
  color: rgb(19, 18, 18);
}

th,
td {
  border: 1px solid rgb(18, 18, 19);
}
h1,
th {
  color: rgb(34, 33, 33);
  font-size: 20px;
}
td {
  color: rgb(31, 30, 29);
  font-size: 15px;
}
p {
  display: inline-block;
  margin-right: 100px;
  color: rgb(48, 47, 46);
  font-size: 20px;
}
</style>
