
<template>
<template v-if="isLoading">
  <div class="align-self-center mt-5">
    <img :src="`/img/3Dteca.png`" alt="logo cargando">
    <p>Cargando...</p>
  </div>
</template>
<template v-else>
  <div class="jumbotron jumbotron-fluid">
      <div class="container banner p-5">
        <h1 class="display-4 subtitulo-white">DISEÑ<span>O</span>S</h1>
      </div>
    </div>
        <div class="container my-5">
            <div class="row row-cols-1 row-cols-md-4 g-4">
                <div class="col" v-for="(item, index) in diseños" :key="index">
                    <div class="card">
                        <div class="card-body">
                            <h2 class="card-title">{{ item.nombre }}</h2>
                            <p class="card-text">{{ item.descripcion }}</p>
                            <a :href="item.stl" download="archivo.stl" class="btn btn-success">Descargar archivo</a>
                        </div>
                        <img :src="item.foto" class="card-img-top">
                    </div>
                </div>
            </div>
        </div>
</template>
</template>
<script>
import 'regenerator-runtime/runtime'
import { collection, getDocs } from 'firebase/firestore';
import { db } from '../../services/firebase';
export default {
    name: 'cargar_diseños',
    data() {
        return {
            isLoading: true,
          diseños: [],
          diseño: {
            nombre: '',
            descripcion: '',
            foto: ''
          }
        }
    },
    methods:{
    async obtenerDatos () { 
      const querySnapshot = await getDocs(collection(db, "diseños"));
        querySnapshot.forEach((doc) => {
        let diseño = doc.data()
        diseño.id = doc.id
        this.diseños.push(diseño)
      });
      this.isLoading = false;
    }
    },
    mounted() {
        this.obtenerDatos();
    },
}
</script>
