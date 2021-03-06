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
            outlined
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-select
            v-model="form.clasification"
            :rules="rules.requiredSelectField"
            :items="items"
            item-text="value"
            item-value="id"
            label="Clasificación del proyecto"
            outlined
            required
          ></v-select>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="form.longitude"
            :rules="rules.numberLongitudeField"
            label="Longitud"
            append-icon="mdi-map-marker"
            required
            outlined
            @keyup="onChangeLngLat"
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="form.latitude"
            :rules="rules.numberLatitudeField"
            label="Latitud"
            append-icon="mdi-map-marker"
            outlined
            @keyup="onChangeLngLat"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-col cols="12" sm="12" class="px-8" :style="{ height: '650px' }">
        <MglMap
          :accessToken="accessToken"
          :mapStyle="mapStyle"
          :center="coordinateMap"
          :zoom="zoomMap"
          :speed="1"
          @click="setMarker"
          @load="onMapLoaded"
        >
          <template v-if="showMarker">
            <MglMarker :coordinates="coordinatePopUp" />
          </template>
        </MglMap>
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
          outlined
        ></v-textarea>
      </v-col>

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
import { MglMap, MglMarker } from "vue-mapbox";

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
    MglMarker,
  },

  created() {
    this.mapbox = Mapbox;
  },

  data() {
    return {
      accessToken:
        "pk.eyJ1Ijoia2VybmVsNTAzIiwiYSI6ImNrZHA5cmhiYTIwamgyeXBkOTgyZmU1cmkifQ.bK_Wbz4134Uf33qBDGklKg",
      mapStyle: "mapbox://styles/mapbox/satellite-streets-v11",
      coordinateMap: [-88.85, 13.82],
      zoomMap: 8,

      showMarker: false,
      coordinatePopUp: [0, 0],

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

      validForm: true,

      form: { ...defaultForm },

      rules: {
        requiredInputField: [
          (value) => !!value || "Este campo es requerido.",
          (value) =>
            value.length <= 255 ||
            "El nombre del proyecto no puede tener más de 255 caracteres.",
        ],

        requiredSelectField: [(value) => !!value || "Este campo es requerido."],

        textAreaField: [
          (value) =>
            value.length <= 255 ||
            "El nombre del proyecto no puede tener más de 255 caracteres.",
        ],

        numberLongitudeField: [
          (value) =>
            parseFloat(value) == value || "Este campo debe ser un número.",
          (value) =>
            (parseFloat(value) <= 180 && parseFloat(value) >= -180) ||
            "Rango valido -180 a 180.",
        ],

        numberLatitudeField: [
          (value) =>
            parseFloat(value) == value || "Este campo debe ser un número.",
          (value) =>
            (parseFloat(value) <= 90 && parseFloat(value) >= -90) ||
            "Rango valido -90 a 90.",
        ],
      },
    };
  },

  methods: {
    cleanForm() {
      this.$refs.form.resetValidation();
      this.form = { ...defaultForm };
      this.showMarker = false;
    },

    submitForm() {
      const validForm = this.$refs.form.validate();
      if (!validForm) {
        this.snackbar = {
          visible: true,
          text: "Debe completar el formulario.",
        };
        return;
      }
      console.log(this.form);
    },

    onMapLoaded() {
      this.loadingMap = false;
    },

    setMarker(e) {
      const [lng, lat] = Object.values(e.mapboxEvent.lngLat.wrap());
      this.coordinatePopUp = [lng, lat];
      this.form.longitude = lng.toFixed(6);
      this.form.latitude = lat.toFixed(6);
      this.showMarker = true;
    },

    onChangeLngLat() {
      if (+this.form.latitude && +this.form.longitude && this.validForm) {
        this.coordinatePopUp = [+this.form.longitude, +this.form.latitude];
        this.showMarker = true;
        return;
      }
      this.showMarker = false;
    },
  },
};
</script>