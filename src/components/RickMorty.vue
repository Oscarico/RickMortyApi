<template>
  <div class="container">
    <h1 style="text-align:center;" class="mb-4">Rick and Morty Api</h1>

    <div class="row mb-4">
      <div class="col-md-12 col-lg-6 offset-lg-3">
        <form class="d-flex">
          <input type="search" aria-label="Search" v-model="busqueda" class="form-control" placeholder="Buscar por nombre">
        </form>
      </div>
    </div>

    <div class="row mt-3 justify-content-center">
      <div class="card mb-3 me-3" style="width: 18rem;" v-for="character in charactersFiltrados" :key="character.id">
        <img :src="character.image" class="card-img-top" alt="...">
        <div class="card-body">
          <h5 class="card-title">{{character.id}}-Nombre: {{character.name}}</h5>
          <p class="card-text">
            Especie: {{ traducirEspecie(character.species) }}
            <br>Genero: {{ traducirGenero(character.gender) }}
            <br>Estatus: {{ traducirEstatus(character.status) }}
            <br>Origen: {{ traducirOrigen(character.origin.name) }}
            <br>Localizacion: {{ traducirLugar(character.location.name) }}
          </p>
        </div>
      </div>
    </div>

    <div class="row mt-3">
      <div class="col">
        <nav aria-label="Page navigation example">
          <ul class="pagination justify-content-center">
            <li class="page-item">
              <a class="page-link" href="#" tabindex="-1" aria-disabled="true" @click="paginarAnterior" :class="{ 'disabled': pagina === 1 }">Anterior</a>
            </li>
            <li class="page-item active" aria-current="page">
              <a class="page-link" href="#">{{ pagina }}</a>
            </li>
            <li class="page-item">
              <a class="page-link" @click="paginarSiguiente" :class="{ 'disabled': pagina === 42 }">Siguiente</a>
            </li>
          </ul>
        </nav>
      </div>
    </div>
  </div>
</template>

<script>

import { ref, watch } from "vue";
import { useRouter } from "vue-router";

export default {
  setup() {
    const characters = ref([]);
    const charactersFiltrados = ref([]);
    const busqueda = ref('');
    let pagina = ref(1);
    let totalCaracteres = ref(0);

    const consultarCharacters = () => {
      fetch(`https://rickandmortyapi.com/api/character?page=${pagina.value}`)
        .then((respuesta) => respuesta.json())
        .then((datosRespuesta) => {
          console.log(datosRespuesta);
          characters.value = datosRespuesta.results;
          totalCaracteres.value = datosRespuesta.info.count;
          filtrarCharacters();
        })
        .catch(console.log);
    };

    const paginarAnterior = () => {
      if (pagina.value > 1) {
        pagina.value--;
        consultarCharacters();
      }
    };

    const paginarSiguiente = () => {
      if (pagina.value < 42) {
        pagina.value++;
        consultarCharacters();
      }
    };

    const filtrarCharacters = () => {
      charactersFiltrados.value = characters.value.filter((character) =>
        character.name.toLowerCase().includes(busqueda.value.toLowerCase())
      );
    };

    const traducirEspecie = (especie) => {
      const especieTraducida = {
        Human: "Humano",
        Alien: "Extraterrestre",
      };
      return especieTraducida[especie] || especie;
    };

    const traducirGenero = (genero) => {
      const generoTraducido = {
        Male: "Masculino",
        Female: "Femenino",
        Genderless: "Sin género",
      };
      return generoTraducido[genero] || genero;
    };

    const traducirEstatus = (estatus) => {
      const estatusTraducido = {
        Alive: "Vivo",
        Dead: "Muerto",
        unknown: "Desconocido",
      };
      return estatusTraducido[estatus] || estatus;
    };

    const traducirLugar = (lugar) => {
        const lugarTraducido = {
        "Earth (C-137)": "Tierra (C-137)",
        "Earth (Replacement Dimension)": "Tierra (Dimension Replica)",
        "Worldender's lair": "Guarida del Worldender",
        "Citadel of Ricks": "Ciudadela de los Ricks",
        unknown: "Desconocido",
        "Signus 5 Expanse": "Signus 5 Expansión",
        "Post-Apocalyptic Earth": "Tierra PostApocaliptica",
        "Purge Planet": "Planeta Purga",
        "Anatomy Park": "Parque Anatomico",
        "Interdimensional Cable": "Cable Interdimensional",
        "Immortality Field Resort": "Resort Campo de Inmortalidad",
        "Cronenberg Earth": "Tierra de Cronenberg",
        "Giant's Town": "Ciudad de los Gigantes",
        "Bird World": "Mundo de las Aves",
        "St. Gloopy Noops Hospital": "Hospital St. Gloopy Noops",
        "Earth (5-126)": "Tierra (5-126)",
        "Mr. Goldenfold's dream": "El sueño del Sr. Goldenfold",
        "Testicle Monster Dimension": "Dimensión del monstruo testicular",
        "Earth (C-500A)": "Tierra (C-500A)",
        "Rick's Battery Microverse": "Microverso de la batería de Rick",
        "The Menagerie": "La Menagerie",
        "Earth (K-83)": "Tierra (K-83)",
        "Hideout Planet": "Planeta Escondite",
        "Unity's Planet": "Planeta de la Unidad",
        "Earth (Unknown dimension)": "Tierra (Dimension Desconocida)",
        "Earth (J19ζ7)": "Tierra (J19ζ7)",
        "Roy: A Life Well Lived": "Roy: Una vida bien vivida",
        "Eric Stoltz Mask Earth": "Eric Stoltz Mascara de la Tierra",
        "Earth (Evil Rick's Target Dimension)":
          "Tierra (Dimensión objetivo de Rick el Malvado)",
        "Planet Squanch": "Planeta Squanch",
        "Resort Planet": "Planeta Resort",
        "Interdimensional Customs": "Costumbres Interdimensionales",
        "Galactic Federation Prison": "Prisión de la Federación Galáctica",
        "Hamster in Butt World": "Hámster en el mundo de las nalgas",
        "Earth (Giant Telepathic Spiders Dimension)":
          "Tierra (Dimensión de las arañas telepáticas gigantes)",
        "Zigerion's Base": "Base de Zigerion",
        "Fantasy World": "Mundo de fantasía",
        "Zeep Xanflorp's Miniverse": "Miniverso de Zeep Xanflorp",
        "Larva Alien's Planet": "Planeta de Larva Alien",
        "Earth (K-22)": "Tierra (k-22)",
        "Mr. Meeseeks Box": "Caja de Sr. Meeseeks",
        "Vindicator's Base": "Base del Vindicador",
        "Pawn Shop Planet": "Planeta Casa de Empeño",
        "Mega Gargantuan Kingdom": "Mega Reino Gargantuesco",
        "Gear World": "Mundo del engranaje",
        "Earth (D-99)": "Tierra (D-99)",
        "Earth (D716)": "Tierra (D716)",
        "Earth (D716-B)": "Tierra (D716-B)",
        "Earth (D716-C)": "Tierra (D716-C)",
        "Earth (J-22)": "Tierra(J-22)",
        Froopyland: "Froopylandia",
        "Trunk World": "Mundo baúl",
        "Blips and Chitz": "Blips y Chitz",
        "Earth (C-35)": "Tierra (C-35)",
        "Snuffles' Dream": "El sueño de Snuffles",
        "Earth (Pizza Dimension)": "Tierra (Dimensión Pizza)",
        "Earth (Phone Dimension)": "Tierra (Dimensión telefónica)",
        "Greasy Grandma World": "El mundo de la abuela grasienta",
        "Earth (Chair Dimension)": "Tierra (Dimensión Silla)",
        "Alien Day Spa": "Spa de día Alien",
        "Earth (Fascist Dimension)": "Tierra (Dimensión fascista)",
        "Snake Planet": "Planeta Serpiente",
        "Earth (Fascist Shrimp Dimension)":
          "Tierra (Dimensión Camarón Fascista)",
        "Earth (Fascist Teddy Bear Dimension)":
          "Tierra (Dimensión del osito fascista)",
        "Earth (Wasp Dimension)": "Tierra (Dimensión Avispa)",
        "Monogatron Mothership": "Nave nodriza monogatrón",
        "Gorgon Quadrant": "Cuadrante Gorgona",
        "Mount Space Everest": "Monte Everest Espacial",
        "Heistotron Base": "Base Heistotron",
        "Mount Olympus": "Monte Olimpo",
        "Earth (Tusk Dimension)": "Tierra (Dimensión Colmillo)",
        "New Improved Galactic Federation Quarters":
          "Nuevos cuarteles mejorados de la Federación Galáctica",
        "Story Train": "Tren de la Historia",
        "Non-Diegetic Alternative Reality": "Realidad alternativa no diegética",
        "Tickets Please Guy Nightmare": "Entradas por favor chivo pesadilla",
        "Morty’s Story": "La histora de Morty",
        "Ricks’s Story": "La historia de Rick",
        "Glorzo Asteroid": "Asteroide Glorzo",
        "Alien Acid Plant": "Planta ácida alienígena",
        "Merged Universe": "Universo fusionado",
        "Near-Duplicate Reality": "Realidad casi duplicada",
        "NX-5 Planet Remover": "Removedor de planetas NX-5",
        "Defiance's Ship": "Barco de Defiance",
        "Defiance's Base": "Base de Defiance",
        "The Ocean": "El Oceano",
        "Narnia Dimension": "Dimension Narnia",
        "Elemental Rings": "Anillos Elementales",
        Space: "Espacio",
        Hell: "Infierno",
        "Space Tahoe": "Espacio Tahoe",
        France: "Francia",
        "Birdperson's Consciousness": "Conciencia de pajarero",
        "Rick's Consciousness": "La conciencia de Rick",
        "Avian Planet": "Planeta Aviario",
        "Normal Size Bug Dimension": "Tamaño normal Dimensión del insecto",
        "Rick and Two Crows Planet": "Planeta Rick y dos cuervos",
        "Rick's Memories": "Memorias de Rick",
        };
      return lugarTraducido[lugar] || lugar;
    };

    const traducirOrigen = (origen) => {
      const origenTraducido = {
        "Earth (C-137)": "Tierra (C-137)",
        "Earth (Replacement Dimension)": "Tierra (Dimension Replica)",
        "Worldender's lair": "Guarida del Worldender",
        "Citadel of Ricks": "Ciudadela de los Ricks",
        unknown: "Desconocido",
        "Signus 5 Expanse": "Signus 5 Expansión",
        "Post-Apocalyptic Earth": "Tierra PostApocaliptica",
        "Purge Planet": "Planeta Purga",
        "Anatomy Park": "Parque Anatomico",
        "Interdimensional Cable": "Cable Interdimensional",
        "Immortality Field Resort": "Resort Campo de Inmortalidad",
        "Cronenberg Earth": "Tierra de Cronenberg",
        "Giant's Town": "Ciudad de los Gigantes",
        "Bird World": "Mundo de las Aves",
        "St. Gloopy Noops Hospital": "Hospital St. Gloopy Noops",
        "Earth (5-126)": "Tierra (5-126)",
        "Mr. Goldenfold's dream": "El sueño del Sr. Goldenfold",
        "Testicle Monster Dimension": "Dimensión del monstruo testicular",
        "Earth (C-500A)": "Tierra (C-500A)",
        "Rick's Battery Microverse": "Microverso de la batería de Rick",
        "The Menagerie": "La Menagerie",
        "Earth (K-83)": "Tierra (K-83)",
        "Hideout Planet": "Planeta Escondite",
        "Unity's Planet": "Planeta de la Unidad",
        "Earth (Unknown dimension)": "Tierra (Dimension Desconocida)",
        "Earth (J19ζ7)": "Tierra (J19ζ7)",
        "Roy: A Life Well Lived": "Roy: Una vida bien vivida",
        "Eric Stoltz Mask Earth": "Eric Stoltz Mascara de la Tierra",
        "Earth (Evil Rick's Target Dimension)":
          "Tierra (Dimensión objetivo de Rick el Malvado)",
        "Planet Squanch": "Planeta Squanch",
        "Resort Planet": "Planeta Resort",
        "Interdimensional Customs": "Costumbres Interdimensionales",
        "Galactic Federation Prison": "Prisión de la Federación Galáctica",
        "Hamster in Butt World": "Hámster en el mundo de las nalgas",
        "Earth (Giant Telepathic Spiders Dimension)":
          "Tierra (Dimensión de las arañas telepáticas gigantes)",
        "Zigerion's Base": "Base de Zigerion",
        "Fantasy World": "Mundo de fantasía",
        "Zeep Xanflorp's Miniverse": "Miniverso de Zeep Xanflorp",
        "Larva Alien's Planet": "Planeta de Larva Alien",
        "Earth (K-22)": "Tierra (k-22)",
        "Mr. Meeseeks Box": "Caja de Sr. Meeseeks",
        "Vindicator's Base": "Base del Vindicador",
        "Pawn Shop Planet": "Planeta Casa de Empeño",
        "Mega Gargantuan Kingdom": "Mega Reino Gargantuesco",
        "Gear World": "Mundo del engranaje",
        "Earth (D-99)": "Tierra (D-99)",
        "Earth (D716)": "Tierra (D716)",
        "Earth (D716-B)": "Tierra (D716-B)",
        "Earth (D716-C)": "Tierra (D716-C)",
        "Earth (J-22)": "Tierra(J-22)",
        Froopyland: "Froopylandia",
        "Trunk World": "Mundo baúl",
        "Blips and Chitz": "Blips y Chitz",
        "Earth (C-35)": "Tierra (C-35)",
        "Snuffles' Dream": "El sueño de Snuffles",
        "Earth (Pizza Dimension)": "Tierra (Dimensión Pizza)",
        "Earth (Phone Dimension)": "Tierra (Dimensión telefónica)",
        "Greasy Grandma World": "El mundo de la abuela grasienta",
        "Earth (Chair Dimension)": "Tierra (Dimensión Silla)",
        "Alien Day Spa": "Spa de día Alien",
        "Earth (Fascist Dimension)": "Tierra (Dimensión fascista)",
        "Snake Planet": "Planeta Serpiente",
        "Earth (Fascist Shrimp Dimension)":
          "Tierra (Dimensión Camarón Fascista)",
        "Earth (Fascist Teddy Bear Dimension)":
          "Tierra (Dimensión del osito fascista)",
        "Earth (Wasp Dimension)": "Tierra (Dimensión Avispa)",
        "Monogatron Mothership": "Nave nodriza monogatrón",
        "Gorgon Quadrant": "Cuadrante Gorgona",
        "Mount Space Everest": "Monte Everest Espacial",
        "Heistotron Base": "Base Heistotron",
        "Mount Olympus": "Monte Olimpo",
        "Earth (Tusk Dimension)": "Tierra (Dimensión Colmillo)",
        "New Improved Galactic Federation Quarters":
          "Nuevos cuarteles mejorados de la Federación Galáctica",
        "Story Train": "Tren de la Historia",
        "Non-Diegetic Alternative Reality": "Realidad alternativa no diegética",
        "Tickets Please Guy Nightmare": "Entradas por favor chivo pesadilla",
        "Morty’s Story": "La histora de Morty",
        "Ricks’s Story": "La historia de Rick",
        "Glorzo Asteroid": "Asteroide Glorzo",
        "Alien Acid Plant": "Planta ácida alienígena",
        "Merged Universe": "Universo fusionado",
        "Near-Duplicate Reality": "Realidad casi duplicada",
        "NX-5 Planet Remover": "Removedor de planetas NX-5",
        "Defiance's Ship": "Barco de Defiance",
        "Defiance's Base": "Base de Defiance",
        "The Ocean": "El Oceano",
        "Narnia Dimension": "Dimension Narnia",
        "Elemental Rings": "Anillos Elementales",
        Space: "Espacio",
        Hell: "Infierno",
        "Space Tahoe": "Espacio Tahoe",
        France: "Francia",
        "Birdperson's Consciousness": "Conciencia de pajarero",
        "Rick's Consciousness": "La conciencia de Rick",
        "Avian Planet": "Planeta Aviario",
        "Normal Size Bug Dimension": "Tamaño normal Dimensión del insecto",
        "Rick and Two Crows Planet": "Planeta Rick y dos cuervos",
        "Rick's Memories": "Memorias de Rick",
      };
      return origenTraducido[origen] || origen;
    };

    // Obtener el objeto router
    const router = useRouter();

    // Obtener el parámetro de página de la URL
    const paginaParam = parseInt(router.currentRoute.value.query.page) || 1;

    // Asignar el valor del parámetro de página a la variable 'pagina'
    if (!isNaN(paginaParam) && paginaParam >= 1 && paginaParam <= 42) {
      pagina.value = paginaParam;
    } else {
      // Redirigir a la página predeterminada si el parámetro es inválido
      router.push({ query: { page: pagina.value.toString() } });
    }

    // Actualizar la URL cuando cambie la página
    const actualizarPaginaURL = () => {
      router.push({ query: { page: pagina.value.toString() } });
    };

    // Escuchar cambios en la variable 'pagina' y actualizar la URL
    watch(pagina, actualizarPaginaURL, { immediate: true });

    // Escuchar cambios en la variable 'busqueda' y filtrar los personajes
    watch(busqueda, filtrarCharacters);

    // Llamar a la función de consulta inicial
    consultarCharacters();

    return {
      characters,
      charactersFiltrados,
      busqueda,
      pagina,
      totalCaracteres,
      paginarAnterior,
      paginarSiguiente,
      traducirEspecie,
      traducirGenero,
      traducirEstatus,
      traducirOrigen,
      traducirLugar,
    };
  },
};

</script>