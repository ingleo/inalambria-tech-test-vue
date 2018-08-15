<template>
    <div class="ArticuloCont">
        <p class="ArticuloCont-Text">{{articulo.nombre}}</p>
        <div class="ArticuloCont-Delete" @click="borrarArticulo(articulo)"> X </div>
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
    borrarArticulo(articulo) {
      let completeUri = this.uri + articulo._id;
      axios
        .delete(completeUri)
        .then(response => {
          this.$emit("borrar", articulo);
        })
        .catch(error => {
          alert('Ha ocurrido un error borrando el articulo');
        });
      
    }
  }
};
</script>

<style>
.ArticuloCont {
  display: flex;
  justify-content: center;
  align-items: center;
}

.ArticuloCont-Text {
  width: 90%;
  background-color: white;
  border: 1px solid lightgrey;
  box-shadow: 1px 1px 1px lightgrey;
  padding: 12px;
  margin-right: 10px;
}

.ArticuloCont-Delete {
  width: 20px;
  padding: 5px;
  height: 20px;
  cursor: pointer;
  background: #ff7373;
  border-radius: 10px;
  box-shadow: 1px 1px 1px #c70202;
  color: white;
  font-size: 18px;
  margin-right: 5px;
}

.ArticuloCont-Delete:hover {
  box-shadow: none;
  margin-top: 1px;
  margin-left: 1px;
}
</style>