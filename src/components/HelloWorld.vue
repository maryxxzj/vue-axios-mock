<template>
  <div>
    <h1 style="text-align: center">{{ msg }}</h1>
    <ul v-for="(item,index) in dateList" :key="index">
      <li style="margin-bottom: 10px;">
        <span style="padding-right: 20px;">{{item.author_name}}</span>
        <span style="padding-right: 20px;">{{item.title}}</span>
        <span style="padding-right: 20px;">{{item.date}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        data() {
            return {
                msg: 'Welcome mock',
                dateList: []
            }
        },
        mounted: function () {
            this.getTest();
        },
        methods: {
            getTest() {
                var _this = this;
                _this.$http.get('/api/test').then(function (res) {
                    console.log(res.data);
                    _this.dateList = res.data.data;
                });
            }
        }
    }
</script>

<style scoped>
  h1 {
    font-weight: normal;
  }
</style>
