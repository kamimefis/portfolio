<script lang="ts">
// import data from "@data/data.json";
import { ref, defineComponent, onMounted } from "vue";
import ProjectButton from "./ProjectButton.vue";
import { BACKEND_URL } from "@config/api";

interface Project {
  id: string;
  name: string;
  description: string;
  url: string[];
  stack: string[];
  image: string;
};

export default defineComponent({
  components: { ProjectButton },
  setup() {
    // const projects = ref<Project[]>(data.projects || []);
    //console.log(projects.value);
    const projects = ref<Project[]>([]);


    // console.log("Llamando a:", BACKEND_URL);
    const fetchProjects = async () => {
      try {
        // const res = await fetch("http://localhost:3001/api/data");
        const res = await fetch(BACKEND_URL);
        const data = await res.json();
        projects.value = data.projects;
        console.log("Datos cargados:", projects.value);
      } catch (error) {
        console.error("Error al obtener los datos:", error);
      }
    };
    onMounted(fetchProjects);

    const currentIndex = ref(0);

    const nextProject = () => {
      currentIndex.value = (currentIndex.value + 1) % projects.value.length;
    };

    const prevProject = () => {
      currentIndex.value =
        (currentIndex.value - 1 + projects.value.length) % projects.value.length;
    };

    return { projects, currentIndex, nextProject, prevProject, leftIcon: "m15 19-7-7 7-7", rightIcon: "m9 5 7 7-7 7", };
  },
})
</script>

<template>
  <section class="mx-auto max-w-6xl px-4 w-full mb-8">
    <h1 class="tracking-wide text-2xl text-center mb-3">Proyectos</h1>
    <div v-if="projects.length" class="mx-auto max-w-md overflow-hidden rounded-xl bg-white shadow-md md:max-w-2xl">
      <div class="md:shrink-0">
        <img :src="'/images/' + projects[currentIndex].image" alt="Project Image"
          class="h-48 w-full object-cover md:h-84 md:w-full" />
      </div>
      <div class="p-8 bg-gray-300 ">
        <h1 class=" text-xl font-semibold tracking-wide text-purple-950 uppercase">{{ projects[currentIndex].name }}
        </h1>
        <div class="flex flex-wrap gap-2 mt-2">
          <span v-for="(tech, index) in projects[currentIndex].stack" :key="index"
            class="border-2 border-purple-500 text-purple-700 px-3 py-1 rounded-full text-xs">
            {{ tech }}
          </span>
        </div>
        <p class="my-4 text-gray-500">
          {{ projects[currentIndex].description }}
        </p>
        <div class="flex flex-wrap gap-2 mt-4">
          <a v-for="(link, index) in projects[currentIndex].url" :key="index" :href="link" target="_blank"
            class="bg-purple-950 hover:bg-purple-600 hover:text-white active:bg-purple-700 px-4 py-2 font-thin text-sm rounded-md ">
            {{ projects[currentIndex].url.length > 1 ? `REPOSITORIO ${index + 1}` : "REPOSITORIO" }}
          </a>
        </div>
      </div>
    </div>
    <div class="mx-auto max-w-md overflow-hidden rounded-xl md:max-w-2xl gap-4 flex justify-center p-2">
      <ProjectButton :onClick="prevProject" :iconPath="leftIcon" ariaLabel="Anterior" />
      <ProjectButton :onClick="nextProject" :iconPath="rightIcon" ariaLabel="Siguiente" />
    </div>
  </section>
</template>