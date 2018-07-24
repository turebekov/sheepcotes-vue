<template>
  <div class="hello">
    <h2 class="day">День {{day}}</h2>
    <div style="width:90%">
      <div class="recipe__item" v-for="(cote) in cotes">
        <div class="container">
          <h3>Загон {{cote.farm_id}}</h3>
          <p>Количества : {{cote.count}}</p>
          <div class="paddock" >
            <p v-for="(sheep,index) in cote.count"> овечка {{index+1}} </p>
          </div>

        </div>
      </div>

    </div>
    <button v-on:click="refresh">Обновить</button>
  </div>

</template>

<script>
  import axios from 'axios'

  export default {
    data() {
      return {
        cotes: [],
        day: ''
      }
    },
    created() {
     this.run();
      setInterval(function () {
        this.run();
      }.bind(this),10000)

    },
    methods: {
      refresh: function () {
        axios.get('http://localhost:8888/api/refresh')
          .then((res) => {
            console.log(res.data);
            this.day = res.data.day;
            this.cotes = res.data.farms;
          })
          .catch((err) => {
            console.log(err)
          })
      },
      run:function () {
        axios.get('http://localhost:8888/api/run')
          .then((res) => {
            console.log(res.data);
            this.day = res.data.day;
            this.cotes = res.data.farms;
          })
          .catch((err) => {
            console.log(err)
          })
      }
    }
  }
</script>
<style>
  .container {
    width: 40%;
    float: left;
    margin: 20px;
  }

  .paddock {
    width: 80%;
    border: 1px solid #71badead;
    height: 175px;
    overflow-y: scroll;
    text-align: right;
    padding-right: 10px;
  }
</style>
