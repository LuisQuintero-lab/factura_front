
<!-- Template principal para tener un toolbar en todas las paginas -->
<template>
  <v-app>

    <!-- Definicion de toolbar sencillo -->
    <v-toolbar class="bg-surface-variant" border density="compact">

      <!-- Estilo de el toolbar y titulo -->
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon> <!-- Se agrega la funcionalidad para desplegar el drawer -->
      <v-toolbar-title>Factura</v-toolbar-title>

      <!-- Boton de corazon para accion adicional por el momento no hace nada -->
      <v-btn icon>
        <v-icon>mdi-heart</v-icon>
      </v-btn>
    </v-toolbar>

    <!-- Creacion de drawer lateral para la navegacion entre las paginas -->
    <v-navigation-drawer
        v-model="drawer"
        temporary
        class="bg-surface-variant"
      >
      <v-layout></v-layout>

      <!-- Creacion de los elementos a los que se pueden acceder -->
      <v-list density="compact" nav>
        <v-list-item prepend-icon="mdi-view-dashboard" title="Historial" value="historial" @click="redirectToRoute('record')"></v-list-item>
        <v-list-item prepend-icon="mdi-forum" title="Nuevo CFDI" value="nuevo" @click="redirectToRoute('create')"></v-list-item>
      </v-list>
      </v-navigation-drawer>

    <v-content>
      <router-view/>
    </v-content>

  </v-app>
</template>

<!-- Scripts para toolbar y drawer -->
<script setup>
  import { ref } from 'vue';
  import { useRouter } from 'vue-router';
  // Inicializacion de el estado de el drawer para que aparezca oculto
  const drawer = ref(false)
  // Router para las redirecciones
  const router = useRouter();
  // Recibe el nombre de la ruta definido en el boton del drawer para realizar la redireccion
  const redirectToRoute = (routeName) => router.push(`./${routeName}`);

</script>
