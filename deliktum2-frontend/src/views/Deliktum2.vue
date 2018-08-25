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
<!--
       <gmap-marker
        v-for="(item, index) in markers"
    :key="index"
    :position="item.position"
    :clickable="true"
    :icon="markerIcon"
    @click="openInfoWindowTemplate(item[index])">

      ></gmap-marker>

        <gmap-info-window
        :options="{maxWidth: 300}"
        :position="infoWindow.position"
        :opened="infoWindow.open"
        @closeclick="infoWindow.open=false">

       
</gmap-info-window>
-->

    </gmap-map>
     <div v-html="infoWindow.template">

       

        </div>
     <button v-on:click="openInfoWindowTemplate()">Agregar evento</button>
      </div>

      <script type="text/x-template" id="modal-template">
  <transition name="modal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              default header
            </slot>
          </div>

          <div class="modal-body">
            <slot name="body">
               <div >
            <div>
            <label>Describe el Delito</label>
            <br/>
            <input type="textarea" ><br/>
             <label>SUBE UNA IMAGEN</label><br/>
            <button>SELECCIONAR ARCHIVO</button>
            </div>
            <div>
            <label>PLACAS DEL AUTO</label>
            <br/>
            <input type="text" ><br/>
             <label>FECHA DEL DELITO</label><br/>
            <input type="date" placeholder="mm/dd/yyyy">

            </div>
        </div>
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              default footer
              <button class="modal-default-button" @click="$emit('close')">
                OK
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</script>

<div id="appVue">
  <button id="show-modal" @click="showModal = true">Show Modal</button>
  <!-- use the modal component, pass in the prop -->
  <modal v-if="showModal" @close="showModal = false">
    <!--
      you can use custom content here to overwrite
      default content
    -->
    <h3 slot="header">custom header</h3>
  </modal>
</div>
  </div>
  

</template>

<script>
import Vue from 'vue'
Vue.component('modal', {
  template: '#modal-template'
})
export default {
  name: "GoogleMap",
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
        showmodal: false
    
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
    openInfoWindowTemplate (/*item*/) {
        //this.setInfoWindowTemplate(item)
        //this.infoWindow.position = this.getCoordinates(item)
        this.infoWindow.open = true
   }
  }
};
</script>
