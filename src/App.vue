<template>
  <div id="app">
    <div class="lista-mercado">
      <img class="logo" :src="grocery" alt="Lista mercado"/>
      <h1 class="lista-mercado-header">Lista del mercado</h1>
      <div class="lista-mercado-container">
        <Articulo v-for="articulo in lista" :articulo=articulo @refrescar=obtenerArticulos :key="articulo.id" />
      </div>
      <input type="text" v-model="articulo" v-on:keyup.enter="crearNuevoArticulo"/>
      <div class="lista-mercado-add" @click="crearNuevoArticulo()">+</div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Articulo from "./components/Articulo.vue";
import Logo from "./assets/grocery.png";

export default {
  name: "app",
  components: {
    Articulo
  },
  data() {
    return {
      lista: [],
      articulo: "",
      grocery: Logo,
      uri: "https://inalambria-tech-test.herokuapp.com/api/v1/articulos"
    };
  },
  created: function() {
    this.obtenerArticulos();
  },
  methods: {
    /**
     * Obtiene los articulos de la lista
     */
    obtenerArticulos() {
      axios
        .get(this.uri)
        .then(response => {
          this.lista = response.data;
        })
        .catch(error => {
          alert(error);
        });
    },
    /**
     * Crea un nuevo articulo en la lista
     */
    crearNuevoArticulo() {
      if (!this.articulo) {
        alert("Inserta un articulo");
        return;
      }
      axios
        .post(this.uri, { nombre: this.articulo })
        .then(() => {
          this.obtenerArticulos();
        })
        .catch(error => {
          alert(error);
        });
      this.articulo = "";
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, Helvetica Neue, Helvetica,
    Arial, sans-serif;
  background: linear-gradient(#159957, #155799);
  height: auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.logo {
  width: 60px;
  position: relative;
  top: 60px;
  margin-bottom: 15px
}

.lista-mercado {
  text-align: center;
  border: 1px solid white;
  width: 80vw;
  height: auto;
  box-shadow: 2px 3px 15px rgba(0, 0, 0, 0.5);
  background: #f6f6f6;
  padding-bottom: 60px;
  margin: 40px auto;
}

.lista-mercado-header {
  color: black;
  font-family: -apple-system, BlinkMacSystemFont, Helvetica Neue, Helvetica,
    Arial, sans-serif;
  font-weight: 400;
  text-transform: uppercase;
  margin: 70px auto 30px;
}

.lista-mercado-add {
  color: white;
  font-size: 2em;
  width: 0.5em;
  height: 0.5em;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  cursor: pointer;
  background: #73ff73;
  border-radius: 10px;
  box-shadow: 1px 1px 1px #47a947;
  margin: 20px auto 0;
}

.lista-mercado-add:hover {
  box-shadow: none;
  margin-top: 21px;
  margin-left: auto;
}

.lista-mercado-container {
  width: 80%;
  margin: 0 auto;
}

input {
  width: 60%;
  padding: 10px;
  font-size: 1em;
  margin: 10px auto;
  box-shadow: 1px 3px 20px 0px rgba(0, 0, 0, 0.3);
}
</style>
