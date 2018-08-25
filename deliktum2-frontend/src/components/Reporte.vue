<template>
  <div class="modal-backdrop" style="position:right">
    <div class="modal">
      <header class="modal-header">
        <slot name="header">
          Reporte

          <button
            type="button"
            
            @click="close"
          >
            x
          </button>
        </slot>
      </header>
      <section class="modal-body">
        <slot name="body">

  <v-card flat>
    <v-snackbar
      v-model="snackbar"
      absolute
      top
      right
      color="success"
    >
      <span>Registro exitoso!</span>
      <v-icon dark>check_circle</v-icon>
    </v-snackbar>
    <v-form ref="form" @submit.prevent="submit">
      <v-container grid-list-xl fluid>
          <v-flex >
        <v-combobox
          v-model="select"
          :items="items"
          label="Seleccione el tipo de incidente"
        ></v-combobox>
        
      </v-flex>
        <v-layout wrap>
        
          <v-flex  xs12 >
            <v-textarea
              v-model="form.bio"
              color="teal"
            >
              <div slot="label">
                Describe el Delito
              </div>
            </v-textarea>
          </v-flex>
          
          <v-flex xs12 sm6>
            <v-text-field
              v-model="form.first"
              :rules="rules.name"
              color="purple darken-2"
              label="Placas del Auto"
              required
            ></v-text-field>
          </v-flex>
          <v-flex xs12 sm6>
            <v-text-field
              v-model="form.last"
              :rules="rules.name"
              color="blue darken-2"
              label="Fecha del Delito"
              required
            ></v-text-field>
          </v-flex>
          
        </v-layout>
      </v-container>
     <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          flat
          color="primary"
        >Sube una imagen</v-btn>
        <v-spacer></v-spacer>
      
      
         <v-spacer></v-spacer>
        <v-btn
          v-on:click="addReporte"
          flat
          color="primary"
          type="submit"
        >Reporta Crimen</v-btn>
         <v-spacer></v-spacer>
      </v-card-actions>
    </v-form>
    
    
  </v-card>
        </slot>
       </section>
       <footer class="modal-footer">
          <slot name="footer">
            <button
              type="button"
              class="btn-green"
              @click="close"
            >
              Cerrar
          </button>
        </slot>
      </footer>
    </div>
  </div>
</template>

<script>

import axios from 'axios'

  export default {
    name: 'modal',

    data () {
      const defaultForm = Object.freeze({
        first: '',
        last: '',
        bio: '',
        favoriteAnimal: '',
        age: null,
        terms: false
      })

   
     return {
        form: Object.assign({}, defaultForm),
        rules: {
          age: [
            val => val < 10 || `I don't believe you!`
          ],
          animal: [val => (val || '').length > 0 || 'This field is required'],
          name: [val => (val || '').length > 0 || 'This field is required']
        },
        animals: ['Dog', 'Cat', 'Rabbit', 'Turtle', 'Snake'],
        conditions: false,
        content: `Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nec odio. Praesent libero. Sed cursus ante dapibus diam. Sed nisi. Nulla quis sem at nibh elementum imperdiet. Duis sagittis ipsum. Praesent mauris. Fusce nec tellus sed augue semper porta. Mauris massa. Vestibulum lacinia arcu eget nulla. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Curabitur sodales ligula in libero. Sed dignissim lacinia nunc.`,
        snackbar: false,
        terms: false,
        defaultForm,
        select: 'Asalto',
        items: [
          'Robo',
          'Pelea',
          'Borrachera',
          'Venta d Drogas'
        ]
      }
    },
    methods: {
      close() {
        this.$emit('close');
      },
       resetForm () {
        this.form = Object.assign({}, this.defaultForm)
        this.$refs.form.reset()
      },
      submit () {
        this.snackbar = true
        this.resetForm()
      },
       addReporte(){
            const data ={
            
            tipo: this.select,
            descripcion: this.form.bio,
            placas:this.form.first,
            fechaDelito:this.form.last,
            imagen: null,
            localizacion: null,
            usuario:''
            }

            axios.put("http://localhost:3000/api/Reportes", data).then((response) => {
          console.log(response);
          

        })


        }
    },
    computed: {
      formIsValid () {
        return (
          this.form.first &&
          this.form.last &&
          this.form.favoriteAnimal &&
          this.form.terms
        )
      }
    }
  };
</script>


<style>
  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal {
    background: #FFFFFF;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
  }

  .modal-header,
  .modal-footer {
    padding: 15px;
    display: flex;
  }

  .modal-header {
    border-bottom: 1px solid #eeeeee;
    color: #4AAE9B;
    justify-content: space-between;
  }

  .modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: flex-end;
  }

  .modal-body {
    position: relative;
    padding: 20px 10px;
  }

  .btn-close {
    border: none;
    font-size: 20px;
    padding: 20px;
    cursor: pointer;
    font-weight: bold;
    color: #4AAE9B;
    background: transparent;
  }

  .btn-green {
    color: white;
    background: #4AAE9B;
    border: 1px solid #4AAE9B;
    border-radius: 2px;
  }
</style>