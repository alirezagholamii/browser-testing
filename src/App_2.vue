<template>
  <div id="app">
    <div class="info">
      <h2>Browser testing</h2>
      <p><strong>userAgent: </strong> {{ navigator.userAgent }}</p>
      <p><strong>browser: </strong> {{ browserName }}</p>
      <p><strong>isWebView: </strong> {{ isWebView }}</p>
      <p><strong>devicePixelRatio: </strong> {{ devicePixelRatio }}</p>
      <p><strong>innerHeight : </strong> {{ height }} px</p>
      <p><strong>innerWidth: </strong> {{ width }} px</p>
      <p><strong>location.href: </strong> {{ href }}</p>
      <p>
        <strong>languages: </strong> {{ languages }}
      </p>
      <p><strong>network: </strong> {{ network }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
const colors = [
  "#eaffd0",
  "#95e1d3",
  "#fbaccc",
  "#f4f9f9",
  "#a7ffeb",
  "#fae0df",
  "#f6f5f1",
  "#fff475",
  "#e6c9a8",
  "#f28b82",
];

export default {
  name: "App",
  data() {
    return {
      navigator: window.navigator,
      height: window.innerHeight,
      width: window.innerWidth,
      devicePixelRatio: window.devicePixelRatio,
      href: window.location.href,
      languages: window.clientInformation?.languages?.join(),
      network: "",
      browserName: "",
      isWebView : window.navigator.userAgent.includes ('wv')
    };
  },
  created() {
    this.setBackgroundColor();
    console.log("8==navigator==>", this.navigator);
    axios
      .get("https://www.cloudflare.com/cdn-cgi/trace")
      .then((res) => {
        this.network = res.data;
      })
      .catch((e) => {
        console.log("8==Error=>", e);
      });

    this.detectBrowser();
  },
  methods: {
    detectBrowser() {
      if (
        (navigator.userAgent.indexOf("Opera") ||
          navigator.userAgent.indexOf("OPR")) != -1
      ) {
        this.browserName = "Opera";
      } else if (navigator.userAgent.indexOf("Chrome") != -1) {
        this.browserName = "Chrome";
      } else if (navigator.userAgent.indexOf("Safari") != -1) {
        this.browserName = "Safari";
      } else if (navigator.userAgent.indexOf("Firefox") != -1) {
        this.browserName = "Firefox";
      } else if (
        navigator.userAgent.indexOf("MSIE") != -1 ||
        !!document.documentMode == true
      ) {
        //IF IE > 10
        this.browserName = "IE";
      } else {
        this.browserName = "unknown";
      }
    },
    setBackgroundColor() {
      const body = document.querySelector("body");
      const i = Math.floor(Math.random() * colors.length);
      body.style.backgroundColor = colors[i];
    },
  },

  components: {},
};
</script>

<style>
body {
  margin: 0;
  padding: 10px;
  height: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
  display: flex;
  justify-content: center;
}
.info {
  display: flex;
  flex-direction: column;
  max-width: 600px;
  align-items: flex-start;
  text-align: left;
}
p {
  font-size: 18px;
}
</style>
