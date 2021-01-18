<template>
  <v-container fluid>
    <p class="font-weight-black">Crear Proyecto.</p>
    <v-form
      ref="form"
      v-model="validForm"
      @submit.prevent="submitForm"
      lazy-validation
    >
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="form.projectName"
            :rules="rules.requiredInputField"
            label="Nombre del proyecto"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-select
            v-model="form.clasification"
            :rules="rules.requiredInputField"
            :items="items"
            item-text="value"
            item-value="id"
            label="Clasificación del proyecto"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="form.longitude"
            :rules="rules.requiredInputField"
            label="Longitud"
            required
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="form.latitude"
            :rules="rules.requiredInputField"
            label="Latitud"
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="12" class="px-8" :style="{ height: '650px' }">
          <MglMap
            :accessToken="accessToken"
            :mapStyle="mapStyle"
            :center="[-88.85, 13.82]"
            :zoom="8"
          />
        </v-col>
        <v-col cols="12" sm="12">
          <v-textarea
            :rules="rules.textAreaField"
            v-model="form.description"
            label="Descripción del proyecto"
            rows="2"
            value=""
            counter
            maxlength="25"
          ></v-textarea>
        </v-col>
      </v-row>

      <v-btn color="grey lighten-2" @click="cleanForm" class="mr-4">
        Limpiar Formulario
      </v-btn>
      <v-btn color="indigo" class="mr-4" @click="submitForm" dark>
        Registrar Proyecto
      </v-btn>
    </v-form>
    <v-snackbar
      v-model="snackbar.visible"
      :timeout="3000"
      color="red darken-1"
      left
    >
      {{ snackbar.text }}
      <template v-slot:action="{ attrs }">
        <v-btn text v-bind="attrs" @click="snackbar.visible = false">
          Cerrar
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>
<script>
import Mapbox from "mapbox-gl";
import { MglMap } from "vue-mapbox";

const defaultForm = Object.freeze({
  projectName: "",
  clasification: "",
  longitude: "",
  latitude: "",
  description: "",
});

export default {
  name: "ProjectForm",

  components: {
    MglMap,
  },

  created() {
    this.mapbox = Mapbox;
  },

  data() {
    return {
      accessToken:
        "pk.eyJ1Ijoia2VybmVsNTAzIiwiYSI6ImNrZHA5cmhiYTIwamgyeXBkOTgyZmU1cmkifQ.bK_Wbz4134Uf33qBDGklKg",
      mapStyle: "mapbox://styles/mapbox/satellite-streets-v11",

      items: [
        { value: "Programming", id: 1 },
        { value: "Design", id: 2 },
        { value: "Vue", id: 3 },
        { value: "Vuetify", id: 4 },
      ],

      snackbar: {
        visible: false,
        text: "",
      },

      textSnackBar: "ho",
      validForm: true,
      form: { ...defaultForm },
      rules: {
        requiredInputField: [
          (value) => !!value || "Este campo es requerido",
          (value) =>
            value.length <= 255 ||
            "El nombre del proyecto no puede tener más de 255 caracteres.",
        ],
        textAreaField: [
          (value) =>
            value.length <= 255 ||
            "El nombre del proyecto no puede tener más de 255 caracteres.",
        ],
      },
    };
  },

  methods: {
    cleanForm() {
      this.$refs.form.reset();
      this.$refs.form.resetValidation();
    },
    submitForm() {
      const validForm = this.$refs.form.validate();
      if (!validForm) {
        this.snackbar = {
          visible: true,
          text: "Debe completar el formulario.",
        };
      }
    },
  },
};
</script>