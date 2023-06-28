<script>
import { RouterLink, RouterView, useRouter } from 'vue-router';

export default {
  setup() {
    const router = useRouter();

    // Propiedad computada para verificar si la ruta actual coincide con la ruta activa
    const isActive = (route) => {
      return router.currentRoute.value.path === route;
    };

    return {
      isActive,
    };
  },
};
</script>

<template>
  <header>
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
      <div class="container-fluid">
        <a class="navbar-brand" href="/"> 
          <img alt="Vue logo" class="d-inline-block align-text-top" src="@/assets/logo.png" width="70" height="30" /> RickMorty
        </a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item me-2" aria-current="page" :class="{ active: isActive('/') }">
              <RouterLink class="nav-link" to="/">Home</RouterLink>
            </li>
            <li class="nav-item me-2" :class="{ active: isActive('/rickmorty') }">
              <RouterLink class="nav-link" to="/rickmorty">Rick and Morty</RouterLink>
            </li>
            <li class="nav-item" :class="{ active: isActive('/about') }">
              <RouterLink class="nav-link" to="/about">About</RouterLink>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>

  <RouterView />
</template>

<style scoped>

</style>
