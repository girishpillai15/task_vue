<template>
  <v-container>
    <v-row justify="center">
      <v-col sm="5" cols="12">
        Product name <br /><br />
        <v-text-field v-model="name" outlined label="Enter product name"></v-text-field>
      </v-col>
      <v-col sm="5" cols="12">
        Price <br />
        <br />
        <v-text-field v-model="price" type="number" outlined label="Enter Price"></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col sm="5" cols="12">
        Product Description 
        <br /><br />
        <v-textarea v-model="descp" outlined label="Enter description"></v-textarea>
      </v-col>
      <v-col sm="5" cols="12">
        Product image  <br /><br />
        <v-file-input
          color="#1976d2"
          placeholder="Choose file..."
          prepend-icon="mdi-paperclip"
          outlined
          id="file"
          v-model="fileInput"
          @change="OnFileUpload($event)"
          :show-size="1000"
        >
          <template v-slot:selection="{ index, text }">
            <v-chip v-if="index < 2" color="#1976d2" dark label small>
              {{ text }}
            </v-chip>

            <span
              v-else-if="index === 2"
              class="overline grey--text text--darken-3 mx-2"
            >
              +{{ files.length - 2 }} File(s)
            </span>
          </template>
        </v-file-input>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col sm="10" cols="12">
        <v-btn tile style="color:white;background:aqua" @click="insertData()"
          >Add Product</v-btn
        >
      </v-col>
    </v-row>
    <v-row justify="center" class="mb-6" >
      <v-col sm="10">
        <v-data-table
          hide-default-footer
          :headers="headers"
          :items="productData"
          class="elevation-0"
        >
          <template v-slot:body="props">
            <tbody>
              <tr v-for="item in props.items" :key="item">
                <td
                  class="d-block d-sm-table-cell"
                  :key="field"
                  v-for="field in Object.keys(item)"
                >
                  <v-row v-if="field == 'name'">
                    <v-col sm="4" cols="12" class="pt-7">
                      <v-img
                        width="100"
                        height="100"
                        :src="item['imgSrc']"
                      ></v-img>
                    </v-col>
                    <v-col sm="8" cols="12" style="padding-top: 60px">
                      {{ item['name'] }}
                    </v-col>
                  </v-row>
                  <v-row v-else-if="field == 'id'">
                    <v-col sm="12" cols="12">
                      <span style="color:aqua;cursor: pointer;" @click="removeData(item)">Remove</span>
                    </v-col>
                  </v-row>
                  <v-row v-else-if="field == 'description'">
                    <v-col sm="12" cols="12">
                      {{ item['description'].substring(0, 10) + "..." }}
                    </v-col>
                  </v-row>
                  <v-row v-else-if="field == 'price'">
                    <v-col sm="12" cols="12">
                      {{ item['price'] }}
                    </v-col>
                  </v-row>
                </td>
              </tr>
            </tbody>
          </template>
        </v-data-table>
      </v-col>
    </v-row>
    <v-snackbar v-model="snackbar" top>
      {{ Msg }}
    <template v-slot:action="{ attrs }">
        <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>
<script>
export default {
  methods: {
    removeData(e){
      this.productData.splice(e,1)
      this.Msg = "Product Removed Successfully";
        this.snackbar = true;
        localStorage.setItem('ProductData',JSON.stringify(this.productData))
    },
    OnFileUpload() {
      this.csvFileName = this.fileInput.name;
      var vm = this;
      if (window.FileReader) {
        var reader = new FileReader();
        reader.readAsDataURL(this.fileInput);
        reader.onload = function () {
          vm.imgSrc = reader.result;
          vm.Msg = "Image imported successfully";
          vm.snackbar = true;
        };
        reader.onerror = function (evt) {
          if (evt.target.error.name == "NotReadableError") {
            this.Msg = "Cannot read file !";
            this.snackbar = true;
          }
        };
      } else {
        this.Msg = "FileReader is not supported in this browser.";
        this.snackbar = true;
      }
    },
    insertData() {
      if(this.price == "" || this.descp == "" || this.name == "" || this.imgSrc == null)
      {
        this.Msg = "Please fill all the data";
        this.snackbar = true;
      }else{
        this.allData['name'] = this.name
        this.allData['description'] = this.descp
        this.allData['price'] = this.price
        this.allData['imgSrc'] = this.imgSrc
        this.allData['id'] = this.productData.length 
        this.Msg = "Product Added Successfully";
        this.snackbar = true;
        this.productData = this.productData.length == 0 ? [this.allData] : [...this.productData,this.allData]
        localStorage.setItem('ProductData',JSON.stringify(this.productData))
        this.name = ''
        this.imgSrc = null
        this.descp = ''
        this.price = ''
        this.fileInput = []
      }
    },
  },
  mounted:async function(){
    let data = JSON.parse(localStorage.getItem('ProductData'))
    this.productData = data == null ?[]:data
  },
  data: () => ({
    productData:[],
    allData:{},
    price:"",
    descp:'',
    name:'',
    fileInput: [],
    imgSrc: null,
    snackbar: false,
    Msg: "",
    headers: [
      {
        text: "Name",
        align: "start",
        value: "name",
      },
      { text: "Description", value: "description" },
      { text: "Price", value: "price" },
      { text: "", value: "id" },
    ],
    data: [
      {
        name: "Frozen Yogurt",
        description: "159",
        price: 6.0,
        action: "remove",
      },
    ],
  }),
};
</script>
<style>
</style>