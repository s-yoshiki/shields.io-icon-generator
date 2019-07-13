<template>
  <div class="main">
    <div>
      Template :
      <select v-model="selectedIcon" v-on:change="bindItemStatus('');generateIconUrl()">
        <option disabled value>Please select one</option>
        <option v-for="option in options" v-bind:key="option.name">{{ option.name }}</option>
      </select>
    </div>
    <br>
    <div>
      TextLeft:
      <input type="text" v-model="inputItemTextLeft" v-on:keyup="generateIconUrl()">
      TextRight:
      <input
        type="text"
        v-model="inputItemTextRight"
        v-on:keyup="generateIconUrl()"
      >
      Logo:
      <input type="text" v-model="inputItemLogo" v-on:keyup="generateIconUrl()">
      Color:
      <input type="text" v-model="inputItemColor" v-on:keyup="generateIconUrl()">
      Style:
      <select v-model="selectedStyle" v-on:keyup="generateIconUrl()">
        <option disabled value>Please select one</option>
        <option v-for="option in iconStyle" v-bind:key="option.name">{{ option.value }}</option>
      </select>
    </div>
    <br>
    <div class="center">
      <img :src="dispLogoUrl" height="80">
    </div>
    <br>
    <div class="center">
      <button v-on:click="addItem()">add icon</button>
    </div>
    <br>
    <hr>
    <div class="center img-height-50" v-html="addesUrlsHTML"></div>
    <br>
    <pre><code>{{addesUrlsHTML}}</code></pre>
  </div>
</template>

<script>
import icons from "../lib/icons.js";
import iconStyle from "../lib/iconStyle.js";
export default {
  name: "Index",
  data() {
    return {
      selectedIcon: "vue.js",
      options: icons,
      selectedStyle: "plastic",
      iconStyle: iconStyle,
      inputItemTextRight: "",
      inputItemTextLeft: "",
      inputItemLogo: "",
      inputItemColor: "",
      dispLogoUrl: "",
      addedIconsUrl: [],
      addesUrlsHTML: "",
      addesUrlsText: ""
    };
  },
  methods: {
    getColorCode(key) {
      for (let i = 0; i < icons.length; i++) {
        const name = icons[i].name;
        if (name === key) {
          return icons[i].color;
        }
      }
      return "";
    },
    bindItemStatus() {
      this.inputItemTextLeft = "";
      let text = this.selectedIcon.split("-").join(" ")
      text = text.split('')
      text[0] = text[0].toUpperCase()
      text = text.join('')
      this.inputItemTextRight = text;
      this.inputItemLogo = this.selectedIcon;
      this.inputItemColor = this.getColorCode(this.selectedIcon);
    },
    generateIconUrl() {
      this.dispLogoUrl = this.bindCustomIconUrl(
        this.inputItemTextLeft,
        this.inputItemTextRight,
        this.inputItemLogo,
        this.inputItemColor,
        this.selectedStyle
      );
    },
    bindCustomIconUrl(
      textLeft,
      textRight,
      logo,
      color = "ccc",
      style = "flat"
    ) {
      textLeft = encodeURI(textLeft);
      textRight = encodeURI(textRight);
      logo = encodeURI(logo);
      color = encodeURI(color);
      style = encodeURI(style);
      return `https://img.shields.io/badge/${textLeft}-${textRight}-${color}.svg?logo=${logo}&style=${style}`;
    },
    addItem() {
      this.addedIconsUrl.push(this.dispLogoUrl);
      this.addesUrlsHTML = this.addedIconsUrl
        .map(e => {
          return `<img src="${e}">`;
        })
        .join("\n");
    }
  },
  mounted() {
    this.bindItemStatus();
    this.generateIconUrl();
  }
};
</script>

<style>
.center {
  text-align: center;
}

.img-height-50 > img {
  height:40px;
}

pre {
  padding: 5px;
  margin: 5px;
  border-radius: 10px;
  background-color: #efefef;
}
</style>