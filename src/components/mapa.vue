<template>
<div style="height: 100%; width: 100%">
  <div style="height: 100%; width: 100%">
    <l-map @click="addMarker" :zoom="zoom" :center="center" :options="mapOptions"
      style="height: 100%" @update:center="centerUpdate" @update:zoom="zoomUpdate">
      <l-tile-layer :url="url"/>

      
      <l-marker :lat-lng="lista" v-for="lista,index in lista" :key="index">
        <l-tooltip :options="{ permanent: true, interactive: true }">
          <div @click="innerClick">
           <p>Titulo: {{lista.titulo}}</p>
           <p>Descricao: {{lista.descricao}}</p>
           <p>Data: {{lista.data}}</p>
          </div>
        </l-tooltip>
      </l-marker>

    </l-map>
  </div>

  <div class="modal" v-if="EstadoModal">
    <div class="formulario">
        <h1>Salvar este Ponto</h1>
        <input v-model="titulo" type="text" placeholder="titulo" />
        <input v-model="descricao" type="text" placeholder="descrição" />
        <button @click.prevent.stop="AdicionarPonto">Guardar Ponto</button>
    </div>
  </div>

</div>
</template>

<script>
import { latLng } from "leaflet";
import { LMap, LTileLayer, LMarker, LTooltip } from "vue2-leaflet";

export default {
  name: "mapa_coordenadas",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LTooltip
  },
  data() {
    return {
      zoom: 13,
      center: latLng(47.41322, -1.219482),
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      withTooltip: latLng(47.41422, -1.250482),
      currentZoom: 11.5,
      currentCenter: latLng(47.41322, -1.219482),
      showParagraph: false,
      mapOptions: {
        zoomSnap: 0.5
      },
      showMap: true,
      markers: null,
      lista:[],
      EstadoModal:false,
      titulo:null,
      descricao:null,
    };
  },
  mounted() {
  this.getLocalstorage();
  },
  methods: {
    zoomUpdate(zoom) {
      this.currentZoom = zoom;
    },
    centerUpdate(center) {
      this.currentCenter = center;
    },
    showLongText() {
      this.showParagraph = !this.showParagraph;
    },
    innerClick() {
      alert(this.zoom);
    },
    addMarker(event) {
      this.markers = []
      this.EstadoModal = true
      this.markers.push(event.latlng);
    },
    AdicionarPonto() {
      // Adicionando o titulo e descrição da coordenada no mapa
      this.markers[0]['titulo'] = this.titulo;
      this.markers[0]['descricao'] = this.descricao;
      this.markers[0]['data'] = this.DataAtual();
      // Guardar a coordenada no navegador do cliente 
      if (this.titulo == null && this.descricao == null) {
        console.log('Por favor é necessário preencher todos os campos')
      } else {
        if (localStorage.getItem('coordenadas')) {
          let storage = JSON.parse(localStorage.getItem('coordenadas'))
          storage.push(this.markers[0]);
          localStorage.setItem('coordenadas', JSON.stringify(storage));
          this.getLocalstorage();
        } else {
          localStorage.setItem('coordenadas', JSON.stringify(this.markers));
          this.getLocalstorage();
        }
      }
      
      this.EstadoModal = false;
      // Esvaziando as variaveis do mapa 
      this.titulo = null;
      this.descricao = null;
    },

    DataAtual() {
      let data = new Date();
      let dia = data.getDate();
      let mes = data.getMonth();
      let ano = data.getFullYear();
      
      let data_atual = dia +'-'+mes+'-'+ano;

      return data_atual;
    },

    getLocalstorage() {
    if (localStorage.getItem('coordenadas')) {
      this.lista = JSON.parse(localStorage.getItem('coordenadas'));
    } else {
       return false;
    }
    }

  }
};
</script>

<style>

</style>
