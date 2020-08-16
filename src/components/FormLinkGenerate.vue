<template>
  <div class="col-12 py-5">
    <div class="form-group py-2">
      <label for="number">NÚMERO RECEPTOR:</label>
      <input class="form-control" type="text" id="number" v-model="number">
    </div>
    <div class="form-group">
      <label for="message">MENSAGEM:</label>
      <input class="form-control" type="text" id="message" v-model="message">
    </div>
    <button @click="Generate()" class="btn btn-lg btn-block btn-success">GERAR LINK</button>
  </div>
</template>

<script>
export default {
  name: "FormLink",
  data: () => {
    return {
      number: "",
      message: "",
      url: "",
    }
  },
  methods: {
    Generate: function() {
      const n = this.number.replace(/[^0-9]/g, '');
      const m = encodeURI(this.message);
      this.url = `https://wa.me/send?phone=${n}&text=${m}`;

      this.$router.push({
        name: 'About',
        params: {
          link: this.url,
        },
      });
    }
  }
};
</script>