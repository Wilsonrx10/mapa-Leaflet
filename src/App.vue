<template>
  <div class="principal">
    <mapa_coordenadas />
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
  }
}
</script>

<style scoped>
@import 'style.css';
</style>
