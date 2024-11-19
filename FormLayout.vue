<template>
    <div class="flex flex-col md:flex-row gap-8">
        <div class="md:w-1/2">
            <div class="card flex flex-col gap-4">
                <h1 class="font-semibold text-xl">Consultar Producto</h1>
                <div class="flex flex-col gap-2">
                    <label for="idProducto">ID del Producto</label>
                    <input 
                        v-model="idProducto" 
                        id="idProducto" 
                        type="number" 
                        class="border p-2 rounded"
                        placeholder="Ingresa el ID del producto" 
                    />
                </div>
                <button 
                    type="button" 
                    @click="consultarProducto"
                    class="bg-green-500 text-white py-2 px-4 rounded mt-2 hover:bg-green-600"
                >
                    Consultar Datos
                </button>
            </div>

            <!-- Mostrar los datos del producto -->
            <div v-if="producto" class="mt-4 p-4 border rounded bg-gray-100">
                <h2 class="font-semibold text-lg">Detalles del Producto</h2>
                <p><strong>Nombre:</strong> {{ producto.name }}</p>
                <p><strong>Color:</strong> {{ producto.data?.color || 'N/A' }}</p>
                <p><strong>Capacidad:</strong> {{ producto.data?.capacity || producto.data?.['capacity GB'] || 'N/A' }}</p>
            </div>

            <!-- Mostrar mensajes de error -->
            <div v-if="error" class="text-red-500 mt-4">
                {{ error }}
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

// Variables reactivas
const idProducto = ref('');
const producto = ref(null);
const error = ref('');

// Función para consultar producto
const consultarProducto = async () => {
    try {
        error.value = ''; // Limpiar error previo
        producto.value = null; // Limpiar datos previos

        if (!idProducto.value) {
            error.value = 'Por favor, ingresa un ID válido.';
            return;
        }

        const response = await fetch(`https://api.restful-api.dev/objects/${idProducto.value}`);
        const data = await response.json();

        if (data) {
            producto.value = data; // Guardar datos del producto
        } else {
            error.value = 'Producto no encontrado. Intenta con otro ID.';
        }
    } catch (e) {
        error.value = 'No se pudo conectar con la API. Intenta más tarde.';
    }
};
</script>

<style>
body {
    font-family: Arial, sans-serif;
}
</style>
