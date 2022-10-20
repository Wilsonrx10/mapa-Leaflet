<template>
<div>
  <div class="principal">
     <mapa_coordenadas/>
  </div>
</div>
</template>

<script>
import mapa_coordenadas from './components/mapa.vue'
export default {
  name: 'App',
  data() {
    return {
      dadosmapa:null
    }
  },
  components: {
    mapa_coordenadas
  },
  methods:{
  }
}
</script>

<style scoped>
@import 'style.css';
</style>
