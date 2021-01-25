<template>
  <div class="mx-auto overflow-hidden rounded-lg">
    {{ listenerModel }}
    <v-card>
      <v-navigation-drawer permanent>
        <v-list>
          <v-list-item link @click="setNewComponent(null, null)">
            <v-list-item-content>
              <v-list-item-title class="title">
                Sandra Adams
              </v-list-item-title>
              <v-list-item-subtitle>sandra_a88@gmail.com</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list>
        <v-divider></v-divider>
        <v-list nav dense>
          <v-list-item-group v-model="model">
            <v-list-item @click="loadProjectForm">
              <v-list-item-icon>
                <v-icon>mdi-plus-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title> Crear Proyecto </v-list-item-title>
            </v-list-item>

            <v-list-group :value="false" no-action sub-group>
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title>Listado Proyectos</v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item
                link
                v-for="project in listOfProjects"
                :key="project.id"
                @click="loadProject(project)"
              >
                <v-tooltip bottom>
                  <template v-slot:activator="{ on, attrs }">
                    <v-list-item-title v-bind="attrs" v-on="on">
                      {{ project.name }}
                    </v-list-item-title>
                  </template>
                  <span>
                    {{ project.name }}
                  </span>
                </v-tooltip>
              </v-list-item>

              <v-list-item link>
                <v-list-item-title>No tooltip!</v-list-item-title>
              </v-list-item>
            </v-list-group>

            <v-list-item link>
              <v-list-item-icon>
                <v-icon>mdi-cog-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Administración</v-list-item-title>
            </v-list-item>
          </v-list-item-group>
        </v-list>
      </v-navigation-drawer>
    </v-card>
  </div>
</template>

<script>
import ProjectRepository from "./ProjectRepository ";
import ProjectForm from "./ProjectForm";

export default {
  name: "Navigation",

  props: {
    setNewComponent: {
      type: Function,
      required: true,
    },
  },

  data() {
    return {
      model: -1,

      listOfProjects: [
        { id: 1, name: "Lorem Lorem" },
        { id: 2, name: "Praesentium accusantium" },
      ],
    };
  },

  computed: {
    listenerModel() {
      if (this.model === 1 || this.model === 2) {
        this.setNewComponent(null, null);
      }
      return null;
    },
  },

  methods: {
    loadProjectForm() {
      this.setNewComponent(ProjectForm, null);
    },

    changeIndex() {
      this.changeSelectedIndexNavigation(this.model);
      return this.model;
    },

    loadProject(project) {
      console.log(JSON.stringify(project));
      this.setNewComponent(ProjectRepository, project.id);
    },
  },
};
</script>
