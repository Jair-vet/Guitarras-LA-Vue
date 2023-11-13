<script setup>
  import { ref, reactive, onMounted } from 'vue'
  import { db } from './data/guitarras'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'
  import Guitarra from './components/Guitarra.vue'

  // const state = reactive({
  //   guitarras: []
  // })

  const guitarras = ref([])
  const carrito = ref([])
  const guitarra = ref({})

  // Asignar cuando el componente se monta por primera vez
  onMounted(() => {
    guitarras.value = db   // usando Ref
    guitarra.value = db[3]
    /* state.guitarras = db */   // usando Reactive
  })

  const agregarCarrito = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id == guitarra.id)

    if(existeCarrito >= 0){
      carrito.value[existeCarrito].cantidad++
    }else {
      guitarra.cantidad = 1
      carrito.value.push(guitarra)  
    }
  }

  const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id == id)
    if(carrito.value[index].cantidad <= 1) return
    carrito.value[index].cantidad--
  }

  const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id == id)
    if(carrito.value[index].cantidad >= 10) return
    carrito.value[index].cantidad++

  }

</script>

<template>
  <!DOCTYPE html>
  <html lang="es">
  <head>
      <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>GuitarLA</title>
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Outfit:wght@400;700;900&display=swap" rel="stylesheet"> 
      <link rel="stylesheet" href="./src/style.css">
  </head>
  <body>

    <Header 
      :carrito="carrito"
      :guitarra="guitarra"
      @decrementar-cantidad="decrementarCantidad"
      @incrementar-cantidad="incrementarCantidad"
      @agregar-carrito="agregarCarrito"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            
          <Guitarra 
            v-for="guitarra in guitarras"
            :guitarra="guitarra"
            :key="guitarra.key"
            @agregar-carrito="agregarCarrito"
          />
        </div>
    </main>

    <Footer />
  </body>
  </html>
</template>

