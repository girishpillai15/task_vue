<template>
  <v-app class="grey lighten-4">
    <v-container fluid class="ma-0 pa-0">
      <v-row no-gutters>
        <v-col md="2" cols="12" class="d-flex justify-start pt-3 pl-6">
          <img :src="uxIcon" style="height: 45px" />
        </v-col>
        <v-col md="4" cols="12" class="pt-3 d-flex justify-start">
          <v-tabs>
            <v-tab to="/">Home</v-tab>
            <v-tab to="/manage-products">Manage Product</v-tab>
          </v-tabs>
        </v-col>
        <v-col sm="6" cols="12">
          <v-row justify="end" class="pr-8 pt-2">
            <v-col sm="3">
              <v-select label="Shopping Cart" class="custom"></v-select>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>
    <router-view />
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      uxIcon: require("./assets/ux_icon.png"),
      tab: "tab-1",
      items: ["Home", "Manage Products"],
    };
  },
  mounted() {},
};
</script>
<style>
.custom.v-text-field > .v-input__control > .v-input__slot:before {
  border-style: none;
}
.custom.v-text-field > .v-input__control > .v-input__slot:after {
  border-style: none;
}
</style>
