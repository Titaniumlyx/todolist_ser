<template>
  <div>
    <Header :getData="getData"></Header>
    <List :arr="arr" @changeArr="updateArr" :getData="getData"></List>
  </div>
</template>

<script>
  import axios from 'axios';
  import Header from '@/components/header';
  import List from '@/components/List';

    export default {
      name: "index",
      components: {
        Header,List
      },
      data(){
        return{
          arr: [{
            title: "学习",
            isDone: false
          }]
        }
      },
      methods: {
        updateArr(val){
          this.arr = val;
        },
        getData(){
          axios.get("/api/todo").then(res => {
            console.log(res);
            this.arr = res.data.data.map(item => {
              item.isShow = false;
              return item;
            });
          })
        }
      },
      mounted(){
        this.getData();
      }
    }
</script>

<style scoped>
  *{
    margin: 0;
    padding: 0;
  }
  .clearfix::after{
    content: "";
    display: block;
    clear: both;
    overflow: hidden;
    visibility: hidden;
  }
  .fll{float: left}
  .flr{float: right}
  li{list-style: none}
</style>
