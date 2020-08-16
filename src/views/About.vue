<template>
  <div class="container py-5">
    <div class="row">
      <div class="col-12">
        <div class="form-group">
          <label for="link">Link gerado: </label>
          <textarea 
            class="form-control" 
            id="link" 
            rows="3"
            v-model="$route.params.link"
          ></textarea>
        </div>
        <div class="col-12 d-flex justify-content-around py-3">
          <button class="btn btn-success" @click="ControlDatas()">Gerar Link Minificado</button>
          <button class="btn btn-success" @click="ControlDatas()">Gerar QrCode</button>
        </div>
        <div class="form-group">
          <textarea 
            v-if="shortenUrl"
            class="form-control" 
            id="link-minify" 
            v-model="shortenUrl"
          ></textarea>
        </div>
      </div>
      <div class="col-12 d-flex justify-content-around">
        <router-link to="/" class="btn btn-lg btn-info">VOLTAR</router-link>
        <a target="_blank" :href="$route.params.link" class="btn btn-lg btn-warning">TESTAR</a>
        <button id="copiar" class="btn btn-lg btn-success" data-clipboard-target="#link" >COPIAR</button>
      </div>
    </div>
  </div>
</template>

<script>
import ClipboardJS from "clipboard";
import axios from "axios";
new ClipboardJS("#copiar");

const BITLY_URL = 'https://api-ssl.bitly.com/v3/shorten?';
const BITLY_LOGIN = "o_4nm0b7lfsb";
const BITLY_KEY = "R_3acba2a6f39844c2adb685084688f6bd";

export default {
  name: "About",
  data() {
    return {
      longUrl: this.$route.params.link,
      shortenUrl: "",
      errorInput: false
    }
  },
  methods: {
    ControlDatas: function() {
      this.CropUrl();
    },
    CropUrl: function() {
      var vm = this
      axios.get(BITLY_URL, {
          params: {
            format: "json",
            apiKey: BITLY_KEY,
            login: BITLY_LOGIN,
            longUrl: this.longUrl
          }
        })
        .then(function (response) {
          if (response.status == 200) {
            vm.longUrl = response.data.data.long_url;
            vm.shortenUrl = response.data.data.url;
          } else {
            console.log("Opps dude, status code != 200 :( ")
          }
        })
        .catch(function (error) {
          console.log("Error! " + error);
          this.isLoading = false;
        });
    }
  }
};
</script>
