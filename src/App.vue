<template>
  <div class="container" id="app">
    <div class="row">
      <div class="col-md-12">
        <!-- <div class="title text-center">Cryptocurrency Pricing</div> -->
      </div>
      <div class="col-md-4 main">
        <div class="card" v-for="(result, index, i) in results" :key="index">
          <div class="card-name">
            {{ index }}
            <img :src="imgSrc[i]" alt="Crypto logotype" class="logo">
          </div>
          <div class="card-usd">
            $ {{ result.USD }}
          </div>
          <div class="card-eur">
            &#8364 {{ result.EUR }}
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios';

export default {
      data:()=> ({
        url: "https://min-api.cryptocompare.com/data/pricemulti?fsyms=BTC,ETH,EOS,XRP,ZEC&tsyms=USD,EUR",
        results: [],
        imgSrc: [
          'http://icons.iconarchive.com/icons/cjdowner/cryptocurrency-flat/256/Bitcoin-BTC-icon.png',
          'https://cdn.iconscout.com/icon/free/png-256/ethereum-1-283135.png'
        ]

      }),

      mounted() {
        axios.get(this.url).then(response => {
          this.results = response.data
        })
      }
    };

</script>

<style lang="css">

* {
    -webkit-box-sizing: border-box;
    box-sizing: border-box
}

:after,
:before {
    -webkit-box-sizing: border-box;
    box-sizing: border-box
}

::-webkit-input-placeholder {
    color: #666;
    opacity: 1
}

:-moz-placeholder {
    color: #666;
    opacity: 1
}

::-moz-placeholder {
    color: #666;
    opacity: 1
}

:-ms-input-placeholder {
    color: #666;
    opacity: 1
}

body input:focus:required:invalid,
body textarea:focus:required:invalid {
    color: #666
}

body input:required:valid,
body textarea:required:valid {
    color: #666
}

body,
html {
    height: 100%
}

body {
    font-size: 16px;
    min-width: 320px;
    position: relative;
    line-height: 1.65;
    font-family: Montserrat, sans-serif;
    overflow-x: hidden;
    padding: 40px
}

.img-responsive {
    display: block;
    max-width: 100%;
    height: auto
}

.text-center {
    text-align: center
}

::-moz-selection {
    background-color: orange;
    color: #fff
}

::selection {
    background-color: orange;
    color: #fff
}

.title {
    font-size: 22px;
    font-weight: 600
}

.main {
    margin: 0 auto;
    background-color: #eee;
    padding: 10px;
    border: 1px solid rgba(51, 51, 51, .24);
    -webkit-border-radius: 10px;
    border-radius: 10px;
    text-align: center
}

.main .card {
    border: 1px solid rgba(51, 51, 51, .34);
    -webkit-border-radius: 6px;
    border-radius: 6px;
    padding: 10px;
    margin-bottom: 10px
}

.main .card .card-name {
    font-size: 18px;
    font-weight: 700
}

.main .card .card-name .logo {
  position: absolute;
  width: 20%;
  top: 0;
  left: 0;
}

</style>
