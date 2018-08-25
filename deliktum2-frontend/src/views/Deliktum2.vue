<template>
   <div>
      <div>
    <div>
      <h2>Search and add a pin</h2>
      <label>
        <gmap-autocomplete 
          @place_changed="setPlace">
        </gmap-autocomplete>
        <button @click="addMarker">Add</button>
      </label>
      <br/>

    </div>
    <br>
    <input type="text" v-model="lati"/>
    <input type="text" v-model="longi"/>
    <gmap-map 
      :center="center"
      :zoom="12"
      style="width:100%;  height: 400px;"
    >
    
      <gmap-marker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        @click="center=m.position"
      ></gmap-marker>

    </gmap-map>
     
     <button
      type="button"
      class="btn"
      @click="showModal"
    >
      Open Modal!
    </button>
<div >
     <Reporte v-show="isModalVisible"
      @close="closeModal"/>
      </div>
</div>

      
  </div>
  

</template>

<script>
import Reporte from '../components/Reporte.vue';

export default {
  name: "GoogleMap",
  components: {
      Reporte
    },
  data() {
    return {
      // default to Montreal to keep it simple
      // change this to whatever makes sense
      center: { lat: 45.508, lng: -73.587 },
      markers: [],
      places: [],
      currentPlace: null,
      lati:"",
      longi:"",
      feeds: ["Evento 1","Evento 2","Evento 3"],
      infoWindow: {
          position: {lat: 0, lng: 0},
          open: false,
          template: ''
        },
        isModalVisible: false
    
    };
  },

  mounted() {
    this.geolocate();
  },

  methods: {
    // receives a place object via the autocomplete component
    setPlace(place) {
      this.currentPlace = place;
    },
    addMarker() {
      if (this.currentPlace) {
        const marker = {
          lat: this.currentPlace.geometry.location.lat(),
          lng: this.currentPlace.geometry.location.lng()
        };
        this.markers.push({ position: marker });
        this.places.push(this.currentPlace);
        this.center = marker;
        this.currentPlace = null;
      }
    },
    geolocate: function() {
      navigator.geolocation.getCurrentPosition(position => {
        this.center = {
          lat: position.coords.latitude,
          lng: position.coords.longitude
        };
        this.lati = this.center.lat;
        this.longi = this.center.lng;
      });

      
    },
   showModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      }
  }
};
</script>
