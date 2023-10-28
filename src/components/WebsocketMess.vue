<template>
  <div class="mess">
    <div class="mess-left">
      <img :src="popcatSrc" @mousedown="catClickHandler" @mouseup="resetImg" />
      <img hidden src="../assets/img/popcat--open.png">
    </div>
    <div class="mess-right">
      <h1>This website does nothing but counts how many times the cat being clicked (across the internet).</h1>
      <template v-if="model === 0">
        <h4>loading from server...</h4>
      </template>
      <template v-else>
        <h2>{{model}}</h2>
      </template>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.mess {
  height: 100vh;
  overflow: hidden;
  display: grid;
  grid-template-columns: 1fr 1fr;

  @media only screen and (max-width: 768px){
    grid-template-rows: 1fr 1fr;
    grid-template-columns: 1fr;
  }

  &-left {
    height: 100vh;

    img {
      cursor: pointer;
      width: 100%;
      height: 100%;
      object-fit: contain;
    }

    @media only screen and (max-width: 768px){
      height: 50vh;
    }
  }

  &-right {
    height: 100vh;

    display: flex;
    justify-content: center;
    flex-direction: column;

    h2 {
      font-size: 300px;
    }

    @media only screen and (max-width: 768px){
      height: 50vh;
      padding: 20px;

      h1 {
        font-size: 30px;
      }

      h2 {
        font-size: 60px;
        text-align: center;
      }
    }
  }
}
</style>

<script>
export default {
  name: "WebsocketMess",
  data(){
    return {
      model: 0,
      ws: {},
      clickStatus: false
    }
  },
  methods: {
    setBridge(){
      // const websocket = new WebSocket('wss://ws.blackcityhenry.workers.dev');
      this.ws = new WebSocket('wss://ws.blackcityhenry.workers.dev');

      this.ws.addEventListener('message', event => {
        this.model = event.data;
      });
    },
    catClickHandler(){
      this.wsSend();
      this.clickStatus = true;
    },
    resetImg(){
      this.clickStatus = false;
    },
    wsSend(){
      this.ws.send('clicked');
    },
  },
  computed: {
    popcatSrc(){
      let filename = 'popcat';
      this.clickStatus ? filename = filename + '--open' : '';
      return new URL(`../assets/img/${filename}.png`, import.meta.url).href;
    }
  },
  mounted(){
    this.setBridge();
  }
}
</script>
