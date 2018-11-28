<template>
  <div id="app">
      <div class="main">
        <div class="card" v-for="(result, index, i) in results" :key="index">
          <div class="card-name">
            {{ index }}
            <img :src="imgSrc[i]" alt="Crypto logotype" class="logo">
          </div>
          <div class="card-usd">
            <span class="icon">$</span> {{ result.USD }}
            <i class="fa" :class="usdClass(index)"></i>
          </div>
          <div class="card-eur">
            <span class="icon">&#8364;</span> {{ result.EUR }}
            <i class="eur fa" :class="euroClass(index)"></i>
          </div>
        </div>
        <button @click="stopUpdate" v-if="!show" class="btn btn-danger">Остановить</button>
        <button @click="startUpdate" v-if="show" class="btn btn-success">Продолжить</button>
      </div>
  </div>

</template>

<script>
import axios from 'axios';

export default {
      data:()=> ({
        show: false,
        url: "https://min-api.cryptocompare.com/data/pricemulti?fsyms=BTC,ETH,EOS,XRP,ZEC&tsyms=USD,EUR",
        results: {"BTC":{"USD":0,"EUR":0},"ETH":{"USD":0,"EUR":0},"EOS":{"USD":0,"EUR":0},"XRP":{"USD":0,"EUR":0},"ZEC":{"USD":0,"EUR":0}},
        cashResults: {"BTC":{"USD":0,"EUR":0},"ETH":{"USD":0,"EUR":0},"EOS":{"USD":0,"EUR":0},"XRP":{"USD":0,"EUR":0},"ZEC":{"USD":0,"EUR":0}},
        intervalTimer: null,
        imgSrc: [
          'http://icons.iconarchive.com/icons/cjdowner/cryptocurrency-flat/256/Bitcoin-BTC-icon.png',
          'https://cdn.iconscout.com/icon/free/png-256/ethereum-1-283135.png',
          'https://zona-1.ru/800/600/http/pooha.net/images/cryptocurrency130318-9.png',
          'https://cdn.iconscout.com/icon/free/png-256/ripple-13-646080.png',
          'https://www.macupdate.com/images/icons256/58202.png'
        ]

      }),
      methods: {
          updateData(){
            axios.get(this.url).then(response => {
            this.results = response.data
            })
          },
          stopUpdate() {
            this.show = !this.show;
            clearInterval(this.intervalTimer);
          },
          startUpdate() {
              this.intervalTimer = setInterval(() => {
              axios.get(this.url).then(response => {
              this.cashResults = this.results;
              this.results = response.data;
              })
            }, 3000);
            this.show = false;
          }          
      },
      computed:{
          usdObj() {
              return key => {
                  let result = this.results[key];
                  let cashRes = this.cashResults[key];
                  let difUsd = result.USD - cashRes.USD;
                  return difUsd;
              }
          },
          euroObj() {
              return key => {
                  let result = this.results[key];
                  let cashRes = this.cashResults[key];
                  let difEur = result.EUR - cashRes.EUR;
                  return difEur;
              }
          },
          usdClass() {
              return key => {
                let usdRes = this.usdObj(key);
                if (usdRes > 0 ) {
                    return 'fa-sort-up';
                } else if (usdRes < 0) {
                    return 'fa-sort-down'
                } else {
                    return 'fa-sort';
                }  
              }
          },
          euroClass() {
              return key => {
               let euroRes = this.euroObj(key);
                if (euroRes > 0 ) {
                    return 'fa-sort-up';
                } else if (euroRes < 0) {
                    return 'fa-sort-down'
                } else {
                    return 'fa-sort';
                }
          }
        }
      },
      mounted() {
        this.startUpdate();
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
    padding: 40px;
}

.fa {
    position: absolute;
    right: 35%;
    font-size: 18px;
}

.fa-sort-up {
    color: green;
    top: 46px;
}

.fa-sort-down {
    color: red;
}

.fa-sort {
    top: 42px;
    color: gray;
    transform: rotate(90deg);
}

.eur.fa.fa-sort-up {
    top: 73px;
}

.eur.fa.fa-sort-down {
    top: 65px;
}

.eur.fa.fa-sort {
    top: 68px;
}

.icon {
    position: absolute;
    left: 36%;
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
    border-radius: 15px;
    text-align: center;
    min-width: 320px;
    max-width: 320px;
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
  top: 21px;
  left: 25px;
}

</style>
