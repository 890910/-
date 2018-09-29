<template>
  <div id="app">

    <div class="Dialogbox" v-show="Isshow">
      <button @click="close" class="delete_btn">X</button>
      <div class="box" >
        <el-tree :data="data" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
        <ul class="content">
          <li v-for="(key,index) in arr" :key="key">{{key}}
            <button @click="deletedata(key)" class="delete_btn">X</button>
          </li>
        </ul>
      </div>
      <div class="btn_box"><button class="btn"  @click="confirm">确定</button><button class="btn" @click="cancel">取消</button></div>
    </div>

    <div class="inputbox">
      <button class="empty" @click="empty">清空</button>
      <input type="textarea" class="inputconten" v-model="val" @focus="ISSHOW"/>
    </div>
  </div>
</template>
<script>

  export default {
    name: 'App',
    created(){
      this.arr = JSON.parse(localStorage.getItem("data")) || [];
    },
    data() {
      return {
        Isshow:false,
        arr: [],
        val:"",

        data: [{
          label: '一级 1',
          children: [{
            label: '二级 1-1',
            children: [{
              label: '三级 1-1-1'
            }]
          }]
        }, {
          label: '一级 2',
          children: [{
            label: '二级 2-1',
            children: [{
              label: '三级 2-1-1'
            }]
          }, {
            label: '二级 2-2',
            children: [{
              label: '三级 2-2-1'
            }]
          }]
        }, {
          label: '一级 3',
          children: [{
            label: '二级 3-1',
            children: [{
              label: '三级 3-1-1'
            }]
          }, {
            label: '二级 3-2',
            children: [{
              label: '三级 3-2-1'
            }]
          }]
        }],
        defaultProps: {
          children: 'children',
          label: 'label'
        }
      };
    },
//    computed: {
//      // 依赖a的长度，长度发生变化，触发msg发生；
//      // computed不支持异步的方法；
//      val(){
//        let str = "";
//        this.arr.forEach((item, index) => {
//          str += item + "；"
//        })
//        return str
//      }
//    },
    methods: {
      close(){
             this.Isshow=false;
      },
      handleNodeClick(data) {
        if (this.arr.every(item => data.label !== item)) {
          this.arr.push(data.label)
          localStorage.setItem("data", JSON.stringify(this.arr))
        }

      },
      deletedata(key){
        this.arr = this.arr.filter(item => item != key)
        localStorage.setItem("data", JSON.stringify(this.arr))
      },
      empty(){
        localStorage.clear("data");
        this.arr = [];
        this.val=""
      },
      ISSHOW(){
          this.Isshow=true;
      },
      confirm(){
        let str = "";
        this.arr.forEach((item, index) => {
          str += item + "；  "
        })
        this.val=str
      },
      cancel(){
        this.Isshow=false;
      }
    }
  }
</script>

<style>
  /*如果是公共样式，最好写在App.vue 中*/
  * {
    margin: 0;
    padding: 0;
  }
  .Dialogbox{
    border: 1px solid blue;
    position: relative;
    background:#F0F0F0;
    padding-bottom:50px;
  }
  ul li {
    list-style: none
  }

  a {
    text-decoration: none;
  }

  .box {
    display: flex;
    clear: both;
  }

  .el-tree {
    width: 300px;
    border: 1px solid #333;
  }

  .content {

    flex: 1;
    margin-left: 10px;
    border: 1px solid #333;
  }
.content>li{
  width:200px;
  height:35px;
  line-height:35px;
}
  .delete_btn {
    float: right;
    padding: 5px;
  }

  .delete_btn:hover {
    background: red;
  }

  .inputbox {
    margin-top: 20px;
    width: 80%;
    margin-left: 10%;
    position: relative;
    height: 100px;
  }

  /*input*/
  .inputconten {
    display: block;
    height:21px;
    width: 100%;
    line-height: 30px;
    vertical-align: top;
    padding-left: 45px;
  }

  .empty {
    display: none;
    width: 45px;
    height: 24px;
    background: red;
    position: absolute;
    left: 0;
    top: 0;
  }

  .inputbox:hover > .empty {
    display: block;
  }
  .btn_box{
    margin-top: 10px;
    text-align:center;
  }
.btn{
   margin-left:50px;
   height:50px;
   width: 100px;
   border:1px solid #333;
   border-radius: 10px;
   background:#fff;
   outline: none;
 }
  .btn:active{
    border: none;
    height:50px;
    width: 100px;
    border:1px solid green;
    background:green;
    color:#fff;
    /*border-radius: 10px;*/
  }
</style>
