<template>
    <div class="articulo-cont">
        <label class="check-container">
          <input type="checkbox" v-model="articulo.comprado" :checked="articulo.comprado" :value="articulo.comprado" 
                  v-on:change="actulizarComprado(articulo)">
          <span class="checkmark"></span>
        </label>
        <p class="articulo-cont-text" :class="articulo.comprado?'articulo-comprado-sub':''">{{articulo.nombre}}</p>
        <div class="articulo-cont-delete" @click="borrarArticulo(articulo)">x</div>
    </div>

</template>


<script>
import axios from "axios";

export default {
  name: "Articulo",
  props: ["articulo"],
  data() {
    return {
      uri: "http://localhost:3000/api/v1/articulos/"
    };
  },
  methods: {
    /**
     * Borra un articulo de la lista
     */
    borrarArticulo(articulo) {
      let completeUri = this.uri + articulo._id;
      axios
        .delete(completeUri)
        .then(response => {
          this.$emit("refrescar", response);
        })
        .catch(error => {
          alert("Ha ocurrido un error borrando el articulo");
        });
    },
    /**
     * Actualiza un articulo de la lista
     */
    actulizarComprado(articulo) {
      let completeUri = this.uri + articulo._id;
      let nuevoEstado = {
        comprado: articulo.comprado
      };

      axios
        .put(completeUri, nuevoEstado)
        .then(response => {
          this.$emit("refrescar", response);
        })
        .catch(error => {
          alert("Ha ocurrido un error actualizando el articulo");
        });
    }
  }
};
</script>

<style>
.articulo-cont {
  display: flex;
  justify-content: center;
  align-items: center;
}

.articulo-cont-text {
  width: 90%;
  background-color: white;
  border: 1px solid lightgrey;
  box-shadow: 1px 1px 1px lightgrey;
  padding: 12px;
  margin-right: 10px;
}

.articulo-cont-delete {
  width: 20px;
  padding: 5px;
  height: 20px;
  cursor: pointer;
  background: #ff7373;
  box-shadow: 1px 1px 1px #c70202;
  color: white;
  font-size: 18px;
  margin-right: 5px;
}

.articulo-cont-delete:hover {
  box-shadow: none;
  margin-top: 1px;
  margin-left: 1px;
}

.check-container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 25px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.check-container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
  box-shadow: 1px 1px 1px #2196f3;
}

.check-container:hover input ~ .checkmark {
  background-color: #ccc;
}

.check-container input:checked ~ .checkmark {
  background-color: #2196f3;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.check-container input:checked ~ .checkmark:after {
  display: block;
}

.check-container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
.articulo-comprado-sub {
  text-decoration: line-through !important;
}
</style>