<template>
  <div class="mx-auto overflow-hidden rounded-lg">
    <v-card>
      <v-navigation-drawer permanent>
        <v-list>
          <v-list-item link>
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
          <v-list-item-group v-model="model" @change="changeIndex">
            <v-list-item>
              <v-list-item-icon>
                <v-icon>mdi-plus-outline</v-icon>
              </v-list-item-icon>
              <v-list-item-title>Crear Proyecto</v-list-item-title>
            </v-list-item>

            <v-list-group :value="false" no-action sub-group>
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title>Mis Proyectos</v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item
                link
                v-for="project in myProject"
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

            <v-list-group :value="false" no-action sub-group>
              <template v-slot:activator>
                <v-list-item-content>
                  <v-list-item-title>Todos los Proyectos</v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item
                link
                v-for="project in myProject"
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
    <v-main></v-main>
  </div>
</template>

<script>
export default {
  name: "Navigation",

  props: {
    changeSelectedIndexNavigation: {
      type: Function,
      required: true,
    },
  },

  data() {
    return {
      model: -1,
      myProject: [
        { id: 1, name: "Lorem Lorem" },
        { id: 2, name: "Praesentium accusantium" },
      ],
    };
  },

  methods: {
    changeIndex() {
      this.changeSelectedIndexNavigation(this.model);
      return this.model;
    },
    loadProject(project) {
      console.log(JSON.stringify(project));
    },
  },
};
</script>
