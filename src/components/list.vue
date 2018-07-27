<template>
  <ul class="lists">
    <li v-for="(item,index) in currentArr" :class="{done: item.isDone}">
      <span style="color: red;" @dblclick="handleDouble(index)" v-show="!item.isShow">{{item.title}}</span>
      <input type="text"
             v-model="item.title"
             v-show="item.isShow"
             @blur="handleBlur(index)"
             @keyup.enter="handleKeyup(index)"
             autofocus>
      <input type="checkbox" v-model="item.isDone" @change="changeStatus(item)">
      <button class="flr" @click="removeSelf(item._id)">删除</button>
    </li>
  </ul>
</template>

<script>
  import axios from 'axios';

    export default {
      name: "list",
      props:{
        arr:{
          type: Array
        },
        getData:{
          type: Function
        }
      },
      data(){
        return{
          currentArr: this.arr
        }
      },
      watch:{
        arr(val){
          this.currentArr = val;
        },
        currentArr(val){
          this.$emit("changeArr", val)
        }
      },
      methods:{
        changeStatus(item){
          let isDone = item.isDone?1:0;
          let title = item.title;
          let id = item._id;
          axios.patch(`/api/todo/${id}`,{isDone,title}).then(res => {
            if(res.data.code == 200){
              alert("修改成功");
              this.getData();
            }
          })
        },
        removeSelf(id){
          axios.delete(`/api/todo/${id}`).then(res => {
            if(res.data.code == 200){
              alert("删除成功！")
              this.getData();
            }else{
              alert("删除失败！！")
            }
          })
        },
        handleDouble(index){
          this.currentArr[index].isShow = true;
        },
        handleBlur(index){
          this.currentArr[index].isShow = false;
          this.changeStatus(this.currentArr[index]);
        },
        handleKeyup($event){
          $event.target.blur();
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
  .lists{
    width: 600px;
    margin: 20px auto 0;
    border-bottom: 1px solid #f1f1f1;
  }
  .lists li{
    height: 40px;
    line-height: 40px;
    border: 1px solid #000;
    border-radius: 3px;
    color: #333;
    font-size: 16px;
  }
  .lists li.done{
    text-decoration: line-through;
  }
  .lists button{
    width: 50px;
    height: 30px;
    line-height: 30px;
    color: #333;
    border: 1px solid #000;
    border-radius: 2px;
  }
</style>
