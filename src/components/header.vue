<template>
  <header>
    <div class="header-content clearfix">
      <h1 class="fll">TodoList</h1>
      <input class="fll input" type="text" placeholder="请输入待办事项" v-model="title" @keyup.enter="handleAdd">
      <button class="flr" @click="handleAdd">添加</button>
    </div>
  </header>
</template>

<script>
  import axios from 'axios'

    export default {
      props:{
          getData:{
            type: Function
          }
      },
      data(){
        return{
          title:""
        }
      },
      methods:{
        handleAdd(){
          if(!this.title.trim()){
            alert("请输入todo后点击添加")
          }else{
            let title = this.title;
            axios.post(`/api/todo`,{title}).then(res => {
              // console.log(res);
              if(res.data.code == 200){
                alert("添加成功！！");
                this.title = '';
                this.getData();
              }else{
                alert("添加失败！！！！");
              }
            })
          }
        }
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
  header{
    background: #000;
  }
  header .header-content{
    width: 600px;
    margin: 0 auto;
  }
  header h1{
    font-size: 30px;
    color: #fff;
  }
  header input{
    border-radius: 4px;
    width: 276px;
    height: 27px;
    line-height: 27px;
    padding: 0 8px;
    margin-top: 5px;
    margin-left: 120px;
  }
  header button{
    display: block;
    width: 50px;
    height: 31px;
    line-height: 30px;
    border: 1px solid #000;
    border-radius: 3px;
    background: #fff;
    color: salmon;
    font-size: 16px;
    font-weight: 500;
    margin-top: 5px;
  }
</style>
