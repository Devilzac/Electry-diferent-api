<template>
  <div class="cont overflow-auto">
    <b-table
      id="cat-table"
      borderless
      striped
      :items="items"
      :fields="fields"
      :per-page="perPage"
      :current-page="currentPage"
      small
      :busy.sync="isBusy"
    >
      <template v-slot:table-busy>
        <div class="text-center text-danger my-2">
          <b-spinner class="align-middle"></b-spinner>
          <strong>Loading...</strong>
        </div>
      </template>

      <template v-slot:cell(population)="data" >
       <span class="population">{{data.item.population}}</span>
      </template>
      <template v-slot:cell(name)="data" >
        <b-button v-b-modal="'Traducciones-' + data.value" >{{data.item.name}}</b-button>

        <b-modal :id="'Traducciones-' + data.value"  title="Traducciones" >
          <p class="my-4"><a :href="'https://www.google.com/search?q='+ data.item.translations.de">{{data.item.translations.de}}</a></p>
          <p class="my-4"><a :href="'https://www.google.com/search?q='+ data.item.translations.es">{{data.item.translations.es}}</a></p>
          <p class="my-4"><a :href="'https://www.google.com/search?q='+ data.item.translations.fr">{{data.item.translations.fr}}</a></p>
          <p class="my-4"><a :href="'https://www.google.com/search?q='+ data.item.translations.ja">{{data.item.translations.ja}}</a></p>
          <p class="my-4"><a :href="'https://www.google.com/search?q='+ data.item.translations.it">{{data.item.translations.it}}</a></p>
        </b-modal>
      </template>
    </b-table>

    <b-pagination
      class="cat-pagination"
      v-model="currentPage"
      :total-rows="rows"
      :per-page="perPage"
      align="fill"
      aria-controls="cat-table"
    ></b-pagination>
  </div>
</template>

<script>
import Axios from "axios";

export default {
  data() {
    return {
      isBusy: false,
      perPage: 15,
      currentPage: 1,
      fields: ["name", "population"],
      mod: ["translations"],
      items: []
    };
  },
  mounted() {
    this.isBusy = true;
    this.getInfo()
      .then(response => {
        console.log("response: ", response);
        this.isBusy = false;
      })
      .catch(error => {
        console.error(error);
      });
  },
  methods: {
    async getInfo() {
      const response = await Axios.get(
        "https://restcountries-v1.p.rapidapi.com/all",
        {
          method: "GET",
          headers: {
            "x-rapidapi-host": "restcountries-v1.p.rapidapi.com",
            "x-rapidapi-key":
              "50464f74ecmsh3921e87c6e4c45cp1f6109jsn03ea370a7b01"
          }
        }
      );
      this.items = await response.data;
      return this.items;
    }
  },
  computed: {
    rows() {
      //console.log('length: ', this.items.length);
      return this.items.length;
    },
    setPages() {
      const numberOfPages = Math.ceil(this.todo.length / this.perPage);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.cont::-webkit-scrollbar {
    width: 5px;
}
.cont::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
    border-radius: 10px;
}
.cont::-webkit-scrollbar-thumb {
  background-color: rgba(0,0,0,0.3);
  outline: 1px solid rgba(0,0,0,0.5);
    border-radius: 10px;
}

#cat-table {
  font-weight: bold;
  table-layout: fixed
}
.cat-pagination {
  position: fixed;
  bottom: 0;
  width: 100%;
  box-shadow: black 1px 0px 5px;
  margin:3px 0;
}
.overflow-auto {
    max-height: calc(100vh - 45px);
}

  button {
    width: 200px;
  }
@media only screen and (max-width: 600px) {
  body tbody {
    button, .population{
      font-size: 12px;
    }    
  }
  button {
    width: 100px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
</style>
