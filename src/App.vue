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
      <p><strong>languages: </strong> {{ languages }}</p>
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
// import Nomics from "nomics";
// const nomics = new Nomics({
  // apiKey: "0cacf06725e60be47b57ca8646b9ab39",
// });
// nomics.currenciesTicker({
  /*
    Specify the interval for interval data in return
    One or more strings can be provided. If not provided, **all** are used.
    The intervals specified will affect what is returned in the response (see below)
  */
  // interval: ['1d'], // '1d', '7d', '30d', '365d', 'ytd'
  /*
    Limit the returned currencies to the ones in the following array. If not
    specified, **all** will be returned
  */
  // ids: ["BTC", "ETH", "DOT", "BNB"],
  /*
    Specify the currency to quote all returned prices in
  */
  // quoteCurrency?: "EUR", // [DEPRECATED] use "convert" below instead
  // convert?: "EUR", // defaults to "USD"
// });

export default {
  name: "App",
  data() {
    return {
      currencies: {},
      navigator: window.navigator,
      height: window.innerHeight,
      width: window.innerWidth,
      devicePixelRatio: window.devicePixelRatio,
      href: window.location.href,
      languages: window.clientInformation?.languages?.join(),
      network: "",
      browserName: "",
      isWebView: window.navigator.userAgent.includes("wv"),
    };
  },
  created() {
    this.setBackgroundColor();
    // console.log("8==navigator==>", this.navigator);
    const ids = 'BTC,ETH,DOT,BNB,XPR,LTC,DOT,DOGE,LINK,EOS,DASH'
    const key = '0cacf06725e60be47b57ca8646b9ab39'
    axios
      .get(`https://api.nomics.com/v1/currencies/ticker?ids=${ids}&interval=1d&key=${key}`)
      .then((res) => {
        console.log('8=res===>',res.data);
        // this.network = res.data;
      })
      .catch((e) => {
        console.log("8==Error=>", e);
      });

    // this.detectBrowser();
    // async function client() {
    //   // all currencies provided by the ticker with default options
    //   try {
    //     const currencies = await nomics.currenciesTicker();
    //     console.log("8=currencies===>", currencies);
    //   } catch (e) {
    //     console.log("8====>", e);
    //   }
    // }
    // client();
  },
  mounted() {
    // nomics
    //   .currenciesTicker()
    //   .then((res) => {
    //     console.log("8====>");
    //     console.log("8===res=>", res);
    //   })
    //   .catch((e) => {
    //     console.log("8==Error=>", e);
    //   });
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
