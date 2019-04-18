<template>
  <div id="app">
    <div class="hero is-white is-gradiant is-bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">Rick & Morty</span>
          <span class="subtitle">Personajes</span>
        </h1>
        <div class="field has-addons is-right">
          <div class="control">
            <input
              v-model="search"
              class="input is-rounded"
              v-on:keyup.enter="searchData"
              placeholder="Busque por nombre"
            >
          </div>
          <div class="control">
            <a v-on:click="searchData" class="button is-success is-rounded">Buscar</a>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
        <character
          @showModal="showModal"
          v-for="character of characters"
          :key="character.id"
          :character="character"
        />
      </div>
      <nav class="pagination" role="navigation" aria-label="pagination">
        <a class="pagination-previous" v-on:click="changePage(page-1)">Anterior</a>
        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current">{{ page }}</a>
          </li>
        </ul>
        <a class="pagination-next" v-on:click="changePage(page+1)">Siguiente</a>
      </nav>
    </div>

    <div class="modal" :class="{'is-active': modal}" v-if="modal">
      <div class="modal-background" @click="modal = false"></div>
      <div class="modal-card">
        <header class="modal-card-head has-background-success">
          <p class="modal-card-title has-text-white">Personaje: {{ currentCharacter.name}}</p>
          <button class="delete" aria-label="close" @click="modal = false"></button>
        </header>
        <section class="modal-card-body">
          <strong>Género:</strong>
          <p>{{currentCharacter.gender}}</p>
          <strong>Estatus:</strong>
          <p>{{currentCharacter.status}}</p>
          <strong>Especie:</strong>
          <p>{{currentCharacter.species}}</p>
          <strong>Tipo:</strong>
          <p>{{currentCharacter.type}}</p>
        </section>
        <footer class="modal-card-foot has-background-success">
          <button class="button is-danger" @click="modal = false">Cancel</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
//importar axios
import axios from "axios";
//importar el componente character
import Character from "./components/Character";

export default {
  name: "App",
  components: {
    Character
  },
  data: function() {
    return {
      characters: [],
      page: 1,
      pages: 1,
      modal: false,
      search: "",
      currentCharacter: {}
    };
  },
  created() {
    this.fetch(); //me despliega la info apenas se carga la página
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search
      };
      let result = axios
        .get("https://rickandmortyapi.com/api/character/", { params })
        .then(res => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
          //console.log(res.data);
        })
        .catch(err => {
          console.log(err);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
    },
    searchData() {
      //reiniciar la pagina
      this.page = 1;
      //consultar de nuevo los datos.. llamo al metodo fetch
      this.fetch();
    },
    showModal(id) {
      //con la id consultaremos en la api
      //haremos una llamada http tipo get llamada fetchOne
      this.fetchOne(id);
    },
    async fetchOne(id) {
      //llamada http

      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}/`
      );
      this.currentCharacter = result.data;
      this.modal = true;
      console.log(this.currentCharacter);
    }
  }
};
</script>

