<template>
  <form @submit.prevent="handleUpdate">
    <label for="title">Title:</label>
    <input id="title" type="text" required v-model="title" />

    <label for="details">Details:</label>
    <textarea id="details" required v-model="details"></textarea>

    <button>Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      uri: `http://localhost:3000/projects/${this.id}`,
    };
  },
  mounted() {
    fetch(this.uri)
      .then((response) => response.json())
      .then((data) => {
        this.title = data.title;
        this.details = data.details;
      })
      .catch((error) => console.log(error));
  },
  methods: {
    handleUpdate() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: this.title,
          details: this.details,
        }),
      })
        .then(() => this.$router.push('/'))
        .catch((error) => console.log(error));
    },
  },
};
</script>

<style></style>
