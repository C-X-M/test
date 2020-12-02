<template>
  <div id="app">
    <h1>我的博客</h1>
    <comment-form @add-comment="doAdd" />
    <comment-list title="热门评论" :comments="comments" @delete-comment="doDelete" />
  </div>
</template>

<script>
  import CommentForm from './components/comment-form.vue';
  import CommentList from './components/comment-list.vue';
  import axios from 'axios';

  export default {
    name: 'App',
    data() {
      return  {
        comments: [
          { author: "张三", body: "今天是个好日子" },
          { author: "王五", body: "上课睡觉真舒服" }
        ]
      }
    },
    methods: {
      doDelete:  function(id){
        axios({
          url:"/api/task/del?id="+id,
          method: "post",
          responseType:"json"
        }).then(resp =>{
          this.loadcomment();
        })
      },
      doAdd: function (data) {
        let rsp = new URLSearchParams();
        rsp.append("author",data.author);
        rsp.append("body",data.body);

        axios({
          url:"api/task/add",
          method:"post",
          data:rsp
        }).then(resp => {
          this.comments.push(resp.data);
        })
      },
      loadcomment:function () {
        axios({
          url:"api/task/list"
        }).then(resp =>{
          this.comments = resp.data;
        })

      }
    },
    created() {
      this.loadcomment();
    },
    components: {
      CommentForm, CommentList
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
