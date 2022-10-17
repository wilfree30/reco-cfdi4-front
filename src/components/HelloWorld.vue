<template>
  <v-container>
    <v-card>
      <v-card-title class="centrarTitulo">
        CÉDULA DE IDENTIFICACIÓN FISCAL
      </v-card-title>
      <v-row>
        <v-col>
          <div class="d-flex centrar pl-5 accent-4">
            <input type="file" id="files" ref="files" multiple v-on:change="handleFilesUpload()"/>
          </div>
        </v-col>
      </v-row>    
       <!-- <div>
           <div v-bind="(file, key) in files" class="file-listing" :key="item">{{ file.name }} <span class="remove-file" v-on:click="removeFile( key )">Remove</span></div>  
        </div> -->
        <br>
    <!--     <div class="large-12 medium-12 small-12 cell">
          <v-btn v-on:click="addFiles()">Agregar Archivos</v-btn>
        </div> -->
        <div class="pl-5 centrar">
          <v-btn v-on:click="submitFiles()" dark color="#42b983">Enviar</v-btn>
        </div>
        <br>
        <v-row >
          <v-col>
            <div class="d-flex centrar l-16 l-auto pa-5 accent-4 red--text">
              *Te invitamos a ser parte de la actualización del Servicio de facturación CFDI versión 4.0.
              <br>
              **Es sencillo, solo tienes que seleccionar el archivo que contiene tu Cédula de Identificación Fiscal (CIF) en formato .pdf y presionar el botón de "ENVIAR"
            </div>
          </v-col>
        </v-row>
    </v-card>
  </v-container>
</template>

<script>
  import axios from 'axios'
  export default {
    /*
      Defines the data used by the component
    */
    data(){
      return {
        files: []
      }
    },

    /*
      Defines the method used by the component
    */
    methods: {
      /*
        Adds a file
      */
      addFiles(){
        this.$refs.files.click();
      },

      /*
        Submits files to the server
      */
      submitFiles(){
        /*
          Initialize the form data
        */
        let formData = new FormData();

        /*
          Iteate over any file sent over appending the files
          to the form data.
        */
        for( var i = 0; i < this.files.length; i++ ){
          let file = this.files[i];

          formData.append('files[' + i + ']', file);
        }

        /*
          Make the request to the POST /select-files URL
        */
        axios.post('/recibidos',
          formData,
          {
            headers: {
                'Content-Type': 'multipart/form-data'
            }
          }
        ).then(function(){
          console.log('¡¡EXITO!!');
        })
        .catch(function(){
          console.log('¡¡FALLIDO!!');
        });
      },

      /*
        Handles the uploading of files
      */
      handleFilesUpload(){
        let uploadedFiles = this.$refs.files.files;

        /*
          Adds the uploaded file to the files array
        */
        for( var i = 0; i < uploadedFiles.length; i++ ){
          this.files.push( uploadedFiles[i] );
        }
      },

      /*
        Removes a select file the user has uploaded
      */
      removeFile( key ){
        this.files.splice( key, 1 );
      }
    }
  }
</script>

<style>
  .centrar {
    justify-content: center;
    text-align: center;
    /* font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif; */
    font-size: 20px;
  }
  .centrarTitulo {
    justify-content: center;
    text-align: center;
    font-size: 30px;
  }
</style>