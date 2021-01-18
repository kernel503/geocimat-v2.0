<template>
  <v-app>
    <v-main height="auto">
      <v-row no-gutters>
        <v-col cols="auto">
          <Navigation
            :changeSelectedIndexNavigation="changeSelectedIndexNavigation"
          />
        </v-col>
        <v-col class="ma-2">
          <component :is="currentTabComponent"></component>
        </v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import Navigation from "./components/Navigation";
import TestContainer from "./components/TestContainer";
import ProjectForm from "./components/ProjectForm";

export default {
  name: "App",

  components: {
    Navigation,
    TestContainer,
    ProjectForm,
  },

  data() {
    return {
      message: "Hello",
      selectedIndexNavigation: -1,
    };
  },

  computed: {
    currentTabComponent() {
      if (this.selectedIndexNavigation === 0) {
        return ProjectForm;
      }
      if (this.selectedIndexNavigation >= 1) {
        return TestContainer;
      }
      return null;
    },
    reversedMessage() {
      return this.message.split("").reverse().join("");
    },
  },

  methods: {
    changeSelectedIndexNavigation(index) {
      this.selectedIndexNavigation = index;
    },

    reversedMessage2() {
      return this.message.split("").reverse().join("");
    },
  },
};
</script>
