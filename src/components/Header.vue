<script setup>
import { computed } from 'vue';
import { RouterLink, useRoute } from 'vue-router';
import {useBebidasStore} from '../stores/bebidas'
import { useNotificacionStore } from '../stores/notificaciones';

const route = useRoute()
const store = useBebidasStore()
const notificaciones = useNotificacionStore()
const paginaInicio = computed(() => route.name === 'inicio')

const handleSubmit = () => {
    if (Object.values(store.busqueda).includes('')) {
        
        notificaciones.$patch({
            texto : 'Todos los campos son obligatorios',
            mostrar : true,
            error : true
        })
        return
    }

    store.obtenerRecetas()

}


</script>

<template>
    <header class="bg-blue-950" :class="{header: paginaInicio}">
        <div class=" mx-auto container px-20 py-2">

            <div class="flex justify-between items-center">
                <div>
                    <RouterLink :to="{name: 'inicio'}">
                        <img class="w-20" src="/img/logo.svg" alt="Logotipo">
                    </RouterLink>
                </div>

                <nav class="flex gap-3 text-white">
                    <RouterLink 
                        :to="{name: 'inicio'}" 
                        class="uppercase font-bold"
                        active-class="text-orange-500"
                    >
                        inicio
                    </RouterLink>
                    <RouterLink 
                        :to="{name: 'favoritos'}" 
                        class="uppercase font-bold"
                        active-class="text-orange-500"
                    >
                        Favoritos
                    </RouterLink>
                </nav>
            </div>

            <form 
                v-if="paginaInicio" 
                class="md:w-1/2 2xl:w-1/3 
                bg-emerald-800 my-10 p-3 rounded-lg shadow space-y-6"
                @submit.prevent="handleSubmit"
            >
                <div class="space-y-4">
                    <label 
                        for="ingrediente"
                        class="block text-white uppercase font-extrabold text-lg"
                    >Nombre o Ingredientes</label>
                    <input 
                        id="ingrediente"
                        type="text"
                        class="p-3 w-full rounded-lg focus:outline-none"
                        placeholder="Nombre o Ingrediente: ej. Vodka, Tequila, etc."
                        v-model="store.busqueda.nombre"
                    >
                </div>

                <div class="space-y-1">
                    <label 
                        for="categoria"
                        class="block text-white uppercase font-extrabold text-lg"
                    >Categoría</label>
                    <select 
                        id="categoria"
                        class="p-3 w-full rounded-lg focus:outline-none"
                        v-model="store.busqueda.categoria"
                    >
                    <option value="">-- Seleccione --</option>
                    <option 
                        v-for="categoria in store.categorias"
                        :key="categoria.strCategory"
                        :value="categoria.strCategory"
                    >{{ categoria.strCategory }}</option>
                    </select>
                </div>
                <input 
                    type="submit" 
                    value="Buscar Recetas"
                    class="bg-sky-600 hover:bg-sky-900 cursor-pointer text-white font-extrabold w-full p-2 rounded-lg uppercase"
                >
            </form>
        </div>
    </header>
</template>


<style>

.header {
    background-image: linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url('/img/bg.jpg');
    background-size: cover;
    background-position: center;
}


</style>