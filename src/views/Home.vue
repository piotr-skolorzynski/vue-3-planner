<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />

    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '@/components/FilterNav.vue';

export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: 'all',
    };
  },
  mounted() {
    this.fetchProjects();
  },
  methods: {
    fetchProjects() {
      fetch('http://localhost:3000/projects')
        .then((response) => response.json())
        .then((data) => (this.projects = data))
        .catch((error) => console.log(error));
    },
    handleDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    handleComplete(id) {
      const proj = this.projects.find((project) => project.id === id);
      proj.complete = !proj.complete;
    },
  },
  computed: {
    filteredProjects() {
      switch (this.current) {
        case 'completed':
          return this.projects.filter((project) => project.complete);
        case 'ongoing':
          return this.projects.filter((project) => !project.complete);
        case 'all':
        default:
          return this.projects;
      }
    },
  },
};
</script>
