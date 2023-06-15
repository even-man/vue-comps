<template>
  <div class="temp-class">
    <v-combobox @paste="delimitChipsFromPaste($event)" @change="delimitChips" clearable append-icon="" multiple
      label="Doc IDs" v-model="docIdList" :delimiters="[',', ' ']">

      <template v-slot:selection="data">
        <v-chip dark :color="validateText(data.item) ? 'green' : 'red'" @click:close="removeItem(data.item)" close>
          <!-- <p>IsProd</p><v-checkbox :id="'isProd' + data.item"></v-checkbox> -->
          {{ data.item }}


        </v-chip>
      </template>

    </v-combobox>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',

  data: () => ({
    docIdList: [],
  }),
  methods: {
    removeItem(item) {
      this.docIdList.splice(this.docIdList.indexOf(item), 1);
    },
    delimitChipsFromPaste(e) {
      console.log(e.clipboardData.getData('text'));
      let text = e.clipboardData.getData('text').toString();
      this.docIdList = text.split(/\s+|,|\n/);
      if (this.docIdList.length === 1 && this.docIdList[0] === '') {
        this.docIdList = [];
      }
      e.preventDefault();
    },
    delimitChips() {
      console.log('delimitChips()');
      this.docIdList = this.docIdList.toString().split(/(?:,|\s)/);
      if (this.docIdList.length == 1 && this.docIdList[0] === '') {
        this.docIdList = [];
      }
    },
    validateText(text) {
      let isAlphanum = function () {
        for (let i = 0; i < text.length; i++) {
          let char = text[i];
          if (!(char >= '0' && char <= '9') &&
            !(char >= 'a' && char <= 'z') &&
            !(char >= 'A' && char <= 'Z')) {
            return false;
          }
        }
        return true;
      };
      return (text.length == 8 || text.length == 10) && isAlphanum();
    }
  }
}
</script>

<style>
.temp-class {
  margin: auto;
  width: 500px;
}
</style>
