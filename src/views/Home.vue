<template >
<v-row>
  <v-col cols="12" style="text-align:center;margin-top:250px;" md="12" v-if="productData.length == 0">
    <span style="font-weight:bold;font-size:24px;color:red">No Product Added</span>
  </v-col>
  <v-col v-for="(data,i) in productData" :key="i" sm="4" cols="12">
      <v-card  class="mx-auto my-12" max-width="350">
    <v-img
      height="300"
      width="350"
      :src="data.imgSrc"
    ></v-img>
    <v-card-text>
      <v-row>
        <v-col sm="6" cols="12" style="font-size:18px;"> {{data.name.length > 20 ? data.name.substring(0,20)+'...':data.name}}</v-col>
        <v-col sm="6" cols="12" style="text-align: end" class="pt-5">
          <span style="font-weight: bold; font-size: 21px">{{data.price}}:-</span>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
  </v-col>
</v-row>
</template>
<script>
export default {
  name: "Home",
  mounted:async function(){
    let data = JSON.parse(localStorage.getItem('ProductData'))
    this.productData = data == null ?[]:data
  },
  data: () => ({
    productData:[]
  }),

  components: {},
};
</script>
<style>
</style>