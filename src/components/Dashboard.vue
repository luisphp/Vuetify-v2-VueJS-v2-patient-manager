<template>
  <v-card
    class="mx-auto"
  >
    <!-- <v-system-bar
      color="indigo darken-2"
      dark
    >
      <v-spacer></v-spacer>

      <v-icon>mdi-window-minimize</v-icon>

      <v-icon>mdi-window-maximize</v-icon>

      <v-icon>mdi-close</v-icon>
    </v-system-bar> -->

    <!-- <v-toolbar
      color="indigo"
      dark
    >
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>Discover</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>
    </v-toolbar> -->

    <v-container >
        <v-row class="mb-1">
            <v-col
            cols="12"
            >
                <v-card class="text-center">
                    <h1 class="align-center pl-4 pt-4 pb-4">
                        New patient view   <span>
                          <v-btn
                            elevation="2"
                            color="primary"
                            @click="cleanStorage()"
                          >Clean Storage</v-btn>
                        </span>
                    </h1> 
                </v-card>
            </v-col>
            <v-col cols="6">
                <v-card class="text-center">
                    <h3 class="align-center pl-4 pt-4 pb-4">
                        Data of patient
                    </h3>
                        <v-form>
                            <v-row>
                                <v-col
                                cols="10"
                                class="align-center text-center"
                                >
                                <v-text-field
                                    class=" ml-5 align-center"
                                    :counter="10"
                                    label="Full Name"
                                    required
                                    v-model="pacienteName"
                                ></v-text-field>
                                </v-col>

                                <v-col cols="10" >
                                    <v-autocomplete
                                    :items="medicos"
                                    item-text="name"
                                    label="Assigned doctor"
                                    class="ml-4"
                                    v-model="medico"
                                    auto-select-first
                                    dense
                                    rounded
                                    solo
                                    ></v-autocomplete>
                                </v-col>
                                <v-col
                                cols="10"
                                >
                                    <v-textarea
                                    filled
                                    v-model="sintomas"
                                    class=" ml-5 align-center"
                                    name="input-7-4"
                                    label="Symptoms"
                                    placeholder="write it here..."
                                    ></v-textarea>
                                </v-col>
                                    <v-col
                                        cols="10"
                                        class="ml-2"
                                        >
                                        <v-dialog
                                            ref="dialog"
                                            
                                            :return-value.sync="date"
                                            persistent
                                            width="290px"
                                        >
                                            <template v-slot:activator="{ on, attrs }">
                                            <v-text-field
                                                v-model="date"
                                                label="Date of admission"
                                                prepend-icon="mdi-calendar"
                                                readonly
                                                v-bind="attrs"
                                                v-on="on"
                                            ></v-text-field>
                                            </template>
                                            <v-date-picker
                                            v-model="date"
                                            scrollable
                                            >
                                            <v-spacer></v-spacer>
                                            <v-btn
                                                text
                                                color="primary"
                                                @click="modal = false"
                                            >
                                                Cancel
                                            </v-btn>
                                            <v-btn
                                                text
                                                color="primary"
                                                @click="$refs.dialog.save(date)"
                                            >
                                                OK
                                            </v-btn>
                                            </v-date-picker>
                                        </v-dialog>
                                    </v-col>
                                                                    
                            </v-row>
                            
                        </v-form> 
                </v-card>
                <v-btn
                  elevation="2"
                  color="primary"
                  block
                  large
                  @click="guardarPaciente()"
                >Guardar Paciente</v-btn>
            </v-col>
            <v-col cols="6">
                <v-card class="text-center" >
                    <h3 class="align-center pl-4 pt-4 pb-4">
                        List of Patients
                    </h3>
                    <v-col cols="10 mx-auto">
                    <v-card
                      class="mb-3"
                      elevation="2"
                      outlined
                      shaped
                      v-for="item in storageItems"
                    >
                    <v-card-title>{{ item.name }} <span class="d-flex justify-end"> <v-icon @click="editItem(item)"><img src="https://cdn-icons-png.flaticon.com/512/1159/1159633.png" alt="" style="width: 20px;"></v-icon></span></v-card-title>
                    <!-- <p>{{item}}</p> -->
                    <v-card-text>      
                      <div><p> <strong>Assigned Physician:</strong> {{item.medico}} </p></div>
                      <div><p> <strong>Date of admission:</strong> {{item.date}}</p></div>
                      <div><p> <strong>Symptoms:</strong> {{item.sintomas}} </p></div>
                      <div><p> <strong>Actual state:</strong> {{item.status}} </p></div>
                    </v-card-text>
                    </v-card>                    
                    </v-col>
                </v-card>
              
            </v-col>
        </v-row>
    </v-container>
  </v-card>
</template>

<script>
import { Console } from 'console';

  export default {
    data: () => ({
      pacienteName: '',
      sintomas: '',
      date: '',
      status: 'active',
      medico: '',
      storageItems : [],
      medicos: [
          { name: 'Michael Scot', abbr: 'MS', id: 1 },
          { name: 'Pam Bisley', abbr: 'PB', id: 2 },
          { name: 'Diwgt Sruth', abbr: 'DW', id: 3 },
          { name: 'Jim Harper', abbr: 'JH', id: 4 },
          { name: 'Stanley Hudson', abbr: 'SH', id: 5 },
        ],
    }),
    methods: {
      guardarPaciente(){

        var id = Math.floor(Math.random() * 100)

        var datosPaciente = {id, name: this.pacienteName, 
        date: this.date, sintomas : this.sintomas,
        medico: this.medico, status: this.status  }

        console.log('Nuevo Pacinete: ', datosPaciente)

        this.storageItems.push(datosPaciente)

        localStorage.setItem('listaPacientes' , JSON.stringify(this.storageItems))

      },
      cleanStorage() {
        localStorage.clear()
        this.storageItems = []
        console.warn('Storage cleaned!')
      }
    },
    mounted(){

      if(localStorage.getItem('listaPacientes')){
        this.storageItems = JSON.parse(localStorage.getItem('listaPacientes'))
      }
    }
  }
</script>

<style>

</style>