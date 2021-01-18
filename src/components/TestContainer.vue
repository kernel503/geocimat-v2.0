<template>
  <div>
    <v-toolbar>
      <v-tabs dark background-color="primary" grow>
        <v-tab> Mis Archivos </v-tab>
        <v-tab>
          <v-badge color="green" content="6">Galería</v-badge>
        </v-tab>
      </v-tabs>
    </v-toolbar>
    <v-card class="pa-2 mt-2">
      <p>Original message: "{{ message }}"</p>
      <p>Computed reversed message: "{{ reversedMessage }}"</p>
      <p>Method reversed message: {{ reversedMessage2() }}</p>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "TestContainer",

  data() {
    return { message: "Hello" };
  },

  computed: {
    reversedMessage() {
      return this.message.split("").reverse().join("");
    },
  },

  methods: {
    reversedMessage2() {
      return this.message.split("").reverse().join("");
    },
  },
};
</script>
