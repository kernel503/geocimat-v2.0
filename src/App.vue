<template>
  <v-app>
    <v-main height="auto">
      <v-row no-gutters>
        <v-col cols="auto">
          <Navigation :setNewComponent="setNewComponent" />
        </v-col>
        <v-col class="ma-2">
          <template
            v-if="contentComponent !== null && contentComponentProps === null"
          >
            <component :is="contentComponent"> </component>
          </template>
          <template v-if="contentComponentProps !== null">
            <component
              :is="contentComponent"
              v-bind="{ ...contentComponentProps }"
            ></component>
          </template>
        </v-col>
      </v-row>
    </v-main>
  </v-app>
</template>

<script>
import Navigation from "./components/Navigation";

export default {
  name: "App",

  components: {
    Navigation,
  },

  data() {
    return {
      contentComponent: null,
      contentComponentProps: null,
    };
  },

  methods: {
    setNewComponent(component, props) {
      if (props) {
        this.contentComponent = component;
        this.contentComponentProps = { id: props };
      } else {
        this.contentComponent = component;
        this.contentComponentProps = null;
      }
    },
  },
};
</script>
