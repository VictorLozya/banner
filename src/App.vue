<template>
  <div id="app">
    <div class="banner-insertation">
      <label v-if="!loadedFile" class="inputer-label" for="file-inputer"
        >Додай файл сюди</label
      >

      <label v-else class="inputer-label" for="file-inputer"
        >Натисни, щоб обрати інший файл</label
      >
      <input
        accept="image/*"
        type="file"
        id="file-inputer"
        @change="imageLoad"
      />
    </div>
    <div class="image-output">
      <img v-if="imgSize > 0" class="client-image" :src="imgUrl" />
    </div>
    <button class="check-button" v-if="imgSize > 0" @click="checkImage">
      Почати перевірку
    </button>
    <div>
      <p v-if="!pixelsValidation" class="result error">
        Розмір банеру в пікселях не співпадає. У банера розміри {{ imgWidth }} x
        {{ imgHeight }}, коли мають бути 1080 х 90.
      </p>
      <p v-if="!sizeValidation" class="result error">
        Розмір банеру більший за 2MB. Банер важить
        {{ (imgSize / 1000000).toFixed(2) }} MB
      </p>
      <p v-if="!typeValidation" class="result error">
        Файл має невірний тип. Тип файлу: ".{{ imgType }}", коли допустимі типи:
        ".{{ validTypes.join(", .") }}"
      </p>
      <p v-if="resultOfValidation" class="result success">
        Все добре, банер пройшов перевірку. &#128516;
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      imgUrl: "",
      loadedFile: false,
      imgSize: Number,
      imgType: String,
      validTypes: ["gif", "jpeg", "png"],
      imgWidth: 0,
      imgHeight: 0,
      pixelsValidation: Boolean,
      sizeValidation: Boolean,
      typeValidation: Boolean,
      resultOfValidation: false,
    };
  },
  methods: {
    imageLoad(event) {
      this.imgUrl = URL.createObjectURL(event.target.files[0]);
      let fileInfo = event.target.files[0];
      this.imgSize = fileInfo.size;
      this.imgType = fileInfo.type;
      this.imgType = this.imgType.split("/")[1];
      this.loadedFile = true;
      this.resultOfValidation = false;
      console.log(fileInfo, this.imgSize);
    },
    checkImage() {
      this.checkingWAndH();
      this.checkSize();
      this.checkType();
      this.pixelsValidation && this.sizeValidation && this.typeValidation
        ? (this.resultOfValidation = true)
        : (this.resultOfValidation = false);
    },
    checkingWAndH() {
      this.imgHeight = document.querySelector(".client-image").clientHeight;
      this.imgWidth = document.querySelector(".client-image").clientWidth;
      this.imgHeight === 85 && this.imgWidth === 1090
        ? (this.pixelsValidation = true)
        : (this.pixelsValidation = false);
    },
    checkSize() {
      this.imgSize > 0 && this.imgSize < 2097152
        ? (this.sizeValidation = true)
        : (this.sizeValidation = false);
    },
    checkType() {
      this.validTypes.includes(this.imgType)
        ? (this.typeValidation = true)
        : (this.typeValidation = false);
    },
  },
};
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#file-inputer {
  display: none;
}
.check-button {
  border: 2px solid rgb(22, 179, 207);
  padding: 10px;
  font-size: 20px;
  border-radius: 10px;
  background-color: transparent;
  transition: all 0.3s ease;
  cursor: pointer;
}
.check-button:hover {
  background-color: rgb(22, 179, 207);
  color: white;
}
.result {
  font-size: 25px;
  padding: 20px 0;
}
.success {
  background-color: rgba(47, 255, 47, 0.644);
}
.error {
  background-color: rgba(255, 0, 0, 0.671);
}
.image-output {
  margin-bottom: 50px;
}
.inputer-label {
  display: inline-block;
  padding: 10px;
  border: 2px solid #2c3e50;
  border-radius: 10px;
  cursor: pointer;
  margin-bottom: 50px;
  transition: all 0.3s ease;
}
.inputer-label:hover {
  background-color: rgb(22, 179, 207);
  color: aliceblue;
  border: 2px solid rgb(22, 179, 207);
}
</style>
