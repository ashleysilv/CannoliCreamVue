
<script setup>
import { ref, computed } from 'vue'

// Catálogo inicial de productos Cannoli Cream
const productos = ref([
  {
    id: 1,
    nombre: 'Gelatina de fresa',
    descripcion: 'Deliciosa gelatina con sabor a fresa.',
    precio: 5000,
    imagen: '🍓'
  },
  {
    id: 2,
    nombre: 'Gelatina de uva',
    descripcion: 'Gelatina suave con sabor a uva.',
    precio: 5000,
    imagen: '🍇'
  },
  {
    id: 3,
    nombre: 'Gelatina de frutos rojos',
    descripcion: 'Una combinación de sabores frutales.',
    precio: 6500,
    imagen: '🍒'
  },
  {
    id: 4,
    nombre: 'Gelatina de durazno',
    descripcion: 'Gelatina dulce y refrescante.',
    precio: 5500,
    imagen: '🍑'
  }
])

// Carrito de compras
const carrito = ref([])

// Agrega un producto o aumenta su cantidad
function agregarAlCarrito(producto) {
  const existente = carrito.value.find(item => item.id === producto.id)

  if (existente) {
    existente.cantidad++
  } else {
    carrito.value.push({ ...producto, cantidad: 1 })
  }
}

// Calcula el total de unidades
const cantidadProductos = computed(() =>
  carrito.value.reduce((total, item) => total + item.cantidad, 0)
)

// Calcula el valor total del carrito
const totalCompra = computed(() =>
  carrito.value.reduce(
    (total, item) => total + item.precio * item.cantidad,
    0
  )
)

// Formato de moneda colombiana
function formatoPrecio(valor) {
  return new Intl.NumberFormat('es-CO', {
    style: 'currency',
    currency: 'COP',
    maximumFractionDigits: 0
  }).format(valor)
}
</script>

<template>
  <div class="tienda">
    <header class="encabezado">
      <h1>🍮 Cannoli Cream</h1>

      <nav>
        <a href="#inicio">Inicio</a>
        <a href="#productos">Productos</a>
        <a href="#carrito">Mi carrito ({{ cantidadProductos }})</a>
      </nav>
    </header>

    <main>
      <section id="inicio" class="banner">
        <h2>Un dulce momento para compartir</h2>
        <p>Descubre nuestras deliciosas gelatinas.</p>
        <a href="#productos" class="boton-banner">Ver productos</a>
      </section>

      <section id="productos" class="catalogo">
        <h2>Nuestros productos</h2>
        <p class="subtitulo">Elige tu sabor favorito</p>

        <div class="productos">
          <article
            v-for="producto in productos"
            :key="producto.id"
            class="tarjeta"
          >
            <div class="imagen-producto">{{ producto.imagen }}</div>

            <h3>{{ producto.nombre }}</h3>
            <p>{{ producto.descripcion }}</p>
            <strong>{{ formatoPrecio(producto.precio) }}</strong>

            <button @click="agregarAlCarrito(producto)">
              Agregar al carrito
            </button>
          </article>
        </div>
      </section>

      <section id="carrito" class="carrito">
        <h2>🛒 Mi carrito</h2>

        <p v-if="carrito.length === 0">
          Tu carrito está vacío.
        </p>

        <div v-else>
          <article
            v-for="item in carrito"
            :key="item.id"
            class="item-carrito"
          >
            <span>{{ item.nombre }} x {{ item.cantidad }}</span>
            <strong>
              {{ formatoPrecio(item.precio * item.cantidad) }}
            </strong>
          </article>

          <h3>Total: {{ formatoPrecio(totalCompra) }}</h3>
        </div>
      </section>
    </main>

    <footer>
      <p>© 2026 Cannoli Cream - Todos los derechos reservados.</p>
    </footer>
  </div>
</template>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: Arial, sans-serif;
  background: #fff8f3;
  color: #49352e;
}

.encabezado {
  background: #713f32;
  color: white;
  padding: 22px 7%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 15px;
}

.encabezado h1 {
  font-size: 27px;
}

nav {
  display: flex;
  gap: 25px;
  flex-wrap: wrap;
}

nav a {
  color: white;
  text-decoration: none;
  font-weight: bold;
}

.banner {
  background: #f4d9ca;
  text-align: center;
  padding: 75px 20px;
}

.banner h2 {
  font-size: 34px;
  margin-bottom: 15px;
}

.banner p {
  margin-bottom: 25px;
}

.boton-banner,
.tarjeta button {
  display: inline-block;
  background: #a85f48;
  color: white;
  padding: 12px 20px;
  border: none;
  border-radius: 7px;
  text-decoration: none;
  cursor: pointer;
}

.catalogo,
.carrito {
  padding: 55px 7%;
  text-align: center;
}

.catalogo h2,
.carrito h2 {
  font-size: 28px;
}

.subtitulo {
  margin: 12px 0 30px;
}

.productos {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 25px;
}

.tarjeta {
  background: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 3px 12px #00000012;
}

.imagen-producto {
  font-size: 65px;
  margin-bottom: 15px;
}

.tarjeta h3 {
  margin-bottom: 10px;
}

.tarjeta p {
  margin-bottom: 15px;
}

.tarjeta strong {
  display: block;
  margin-bottom: 18px;
  color: #a85f48;
}

.carrito {
  background: #f4e5dc;
}

.item-carrito {
  display: flex;
  justify-content: space-between;
  gap: 15px;
  padding: 12px;
  border-bottom: 1px solid #d8b9a8;
}

.carrito h3 {
  margin-top: 20px;
}

footer {
  background: #713f32;
  color: white;
  padding: 22px;
  text-align: center;
}

@media (max-width: 600px) {
  .encabezado {
    justify-content: center;
    text-align: center;
  }

  nav {
    justify-content: center;
  }

  .banner h2 {
    font-size: 26px;
  }
}
</style>
