<template>
  <div class="column is-12-mobile is-4-desktop is-3-tablet">
    <div class="card">
      <div class="card-header">
        <img v-bind:src="character.image" v-bind:alt="character.name">
      </div>
      <div class="card-content">
        <h3 class="title is-size-4">{{character.name}}</h3>
        <a class="button is-success is-rounded is-small" @click="showMore(character.id)">Ver más</a>
      </div>
    </div>
    <!-- fin de la card-->
  </div>
</template>

<!--
props son variables para pasar datos entre un componente padre y un hijo
-->
<!--pasando la variavle character al compopnente mediante props-->
<script>
export default {
  props: ["character"],
  methods: {
    showMore(id) {
      //hay que hacer un emit: es un metodo para comunicar un dato entre un componente hijo a un padre
      //characte(rs es un componete hijo de App
      this.$emit("showModal",id);
      
    }
  }
}
</script>
