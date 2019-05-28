<template>
  <div id="app">
    <header>
      <section>
        <form @submit.prevent="doAdd()" id="form">
          <label for="title">ToDoList</label>
          <input id="title" type="text" placeholder="添加ToDo"  required="required" autocomplete="off" v-model='todo' />
        </form>
      </section>
    </header>

    <section>
      <h2 onclick="save()">正在进行 <span id="todocount">{{startCount}}</span></h2>
      <ol class="finish">
        <li v-for="(item,key) in list" v-if="!item.checked">
          <input type="checkbox" v-model="item.checked" @change="saveList()">
          <p>{{item.title}}</p>
          <a @click="removeData(key)"></a>
        </li>
      </ol>

      <h2>已完成 <span id="todocount1">{{endCount}}</span></h2>
      <ul id="donelist">
        <li v-for="(item,key) in list" v-if="item.checked">
          <input type="checkbox" v-model="item.checked" @change="saveList()"> 
          <p>{{item.title}}</p>
          <a @click="removeData(key)"></a>
        </li>
      </ul>
    </section>
    <footer>
			Copyright © cucurbitbaby@163.com <a href="javascript:clear();">clear</a>
		</footer>
  </div>
</template>

<script>

import storage from './model/storage.js';
export default {
  name: 'app',
  data () {
    return {
      required:false,
      todo: '',
      list:[
        {
          title:'学习node.js',
          checked:false
        },
          {
          title: '学习vue.js',
          checked: false
        },
          {
          title: '录制react.js',
          checked: false
        },
          {
          title: '学习html',
          checked: true
        },
          {
          title: '学习css',
          checked: true
        },
          {
          title: '学习javascript',
          checked: true
        }
      ],
      startCount:0,
      endCount:0
    }
  },
  methods:{
    doAdd:function(e){
      // //1.获取文本框输入的值 2.把获取到的文本框值 push到lish里面
      this.list.push({
        title:this.todo,
        checked:false
      });
      this.todo = '';
      storage.set('list',this.list);
    },
    removeList(){
      // console.log('我被触发了')
      this.list = [];
      storage.set('list',this.list);
    },
    getlist:function(){
      console.log(this.list);
    },
    listCount(arr,item,pro){
      var counts = 0;
      arr.map(function(a){
          if(a[item]== pro){
              counts ++;
          }
      })
      return {
          nums:counts,
      };
    },
    removeData(listKey){
      this.list.splice(listKey,1)
      storage.set('list',this.list);
    },
    saveList(){
      storage.set('list',this.list);
    }
  },
  mounted:function(){
    var list=storage.get('list');

    if(list){  /*注意判断*/
      this.list=list;
    }

    this.startCount = this.listCount(this.list,"checked",false).nums;
    this.endCount = this.listCount(this.list,"checked",true).nums;
    window.clear = this.removeList;
  },
  watch:{
    list: {
      deep: true,
      handler(newVal,oldVal) {
        // console.log('这样都行- -');
        this.startCount = (this.listCount(newVal,"checked",false).nums);
        this.endCount = this.listCount(newVal,"checked",true).nums;
      }
　　 }
  　　 
　　}
}


</script>

<style lang="scss">
body {margin:0;padding:0;font-size:16px;background: #CDCDCD;}
header {height:50px;background:#333;background:rgba(47,47,47,0.98);}
section{margin:0 auto;}
label{float:left;width:100px;line-height:50px;color:#DDD;font-size:24px;cursor:pointer;font-family: "Helvetica Neue",Helvetica,Arial,sans-serif;}
header input{float:right;width:60%;height:24px;margin-top:12px;text-indent:10px;border-radius:5px;box-shadow: 0 1px 0 rgba(255,255,255,0.24), 0 1px 6px rgba(0,0,0,0.45) inset;border:none}
input:focus{outline-width:0}
h2{position:relative;}
span{position:absolute;top:2px;right:5px;display:inline-block;padding:0 5px;height:20px;border-radius:20px;background:#E6E6FA;line-height:22px;text-align:center;color:#666;font-size:14px;}
ol,ul{padding:0;list-style:none;}
li input{position:absolute;top:2px;left:10px;width:22px;height:22px;cursor:pointer;}
p{margin: 0;}
li p input{top:3px;left:40px;width:70%;height:20px;line-height:14px;text-indent:5px;font-size:14px;}
li{height:32px;line-height:32px;background: #fff;position:relative;margin-bottom: 10px;
	padding:0 45px;border-radius:3px;border-left: 5px solid #629A9C;box-shadow: 0 1px 2px rgba(0,0,0,0.07);}
ol li{cursor:move;}
ul li{border-left: 5px solid #999;opacity: 0.5;}
li a{position:absolute;top:2px;right:5px;display:inline-block;width:14px;height:12px;border-radius:14px;border:6px double #FFF;background:#CCC;line-height:14px;text-align:center;color:#FFF;font-weight:bold;font-size:14px;cursor:pointer;}
footer{color:#666;font-size:14px;text-align:center;}
footer a{color:#666;text-decoration:none;color:#999;}
@media screen and (max-device-width: 620px) {section{width:96%;padding:0 2%;}}
@media screen and (min-width: 620px) {section{width:600px;padding:0 10px;}}
@media screen and (max-device-width: 620px) {section{width:96%;padding:0 2%;}}
@media screen and (min-width: 620px) {section{width:600px;padding:0 10px;}}

</style>
