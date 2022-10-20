<template>
  <v-container>
    <v-card>
      <v-card-title class="centrarTitulo">
        CÉDULA DE IDENTIFICACIÓN FISCAL
      </v-card-title>
      <v-row>
        <v-col>
          <div class="d-flex centrar pl-5 accent-4">
            <input type="file" @change="onFileChange" accept="application/pdf,application"/>
            <div v-if="progress" class="progess-bar" :style="{'width': progress}">
              {{progress}}
            </div>
          </div>
        </v-col>
      </v-row>  
      <v-row>
        <div class="d-flex pl-5 centrar accent-4">
          <v-btn @click="onUploadFile" class="upload-button"
            :disabled="!this.selectedFile">ENVIAR</v-btn>
        </div>
      </v-row>      
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
  import axios from "axios";
  
  export default {
    data() {
      return {
        selectedFile: "",
        progress: 0,
        archivo_recibido: 'archivo_recibido',
      };
    },
    methods: {
      onFileChange(e) {
        const selectedFile = e.target.files[0]; // accessing file
        this.selectedFile = selectedFile;
      },
      onUploadFile() {
        const formData = new FormData();
        //formData.append("file", this.selectedFile);  // appending file
        formData.append("archivo_recibido", this.selectedFile) 
  
       // sending file to the backend
        axios
          .post("https://cfdi4-api.centralexpress.info/upload", formData, { 
            headers: {
              "accept": "application/json",
              "Content-Type": "multipart/form-data",
            },
            data:{
              archivo_recibido: this.selectedFile
            },
            onUploadProgress: progressEvent => {
              this.progress = Math.round(
                (progressEvent.loaded / progressEvent.total) * 100
              ) + "%";
            }
          })
          .then((res) => {
            res.data
            console.log(this.selectedFile)
            console.log(res);
            this.progress = null;
          })
          .catch((err) => {
            console.log(this.selectedFile)
            console.log(err.response.data);
            this.progress = null;
          });
      }
    }
  };
  </script>
  
  <style scoped>
.file-upload {
  box-shadow: 2px 2px 9px 2px #ccc;
  border-radius: 1rem;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  font-size: 1rem;
  width: 60%;
  margin: 0 auto;
}

input {
  font-size: 0.9rem;
}

input,
div,
button {
  margin-top: 2rem;
}

.upload-button {
  width: 7rem;
  padding: 0.5rem;
  background-color: #278be9;
  color: #fff;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  border-radius: 1rem;
}

.upload-button:disabled {
  background-color: #b3bcc4;
  cursor: no-drop;
}

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