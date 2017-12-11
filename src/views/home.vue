//index.vue
<template>
  <div>
    <el-col :span='24' class='banner'>
      <router-link to="/index/blog">
      <el-button class='blog-btn' type="warning">博客
        <span class='blog-num'>{{data.blogNum|notNull}}</span>
      </el-button>
      </router-link>
      <el-button class='blog-btn' type="info" @click="allcommetAdd">总评论
        <span class='blog-num'>{{data.allComment|notNull}}</span>
      </el-button>
      <el-button class='blog-btn' type="success" >总访问量
        <span class='blog-num'>{{data.totalVisit|notNull}}</span>
      </el-button>
      <el-button class='blog-btn' type="danger">留言数
        <span class='blog-num'>{{data.leaveword|notNull}}</span>
      </el-button>
      <el-button class='blog-btn today' type="warning" @click="addtodayvisit">今日访问量
        <span class='blog-num'>{{data.todayVisit|notNull}}</span>
      </el-button>
      <statistics></statistics>
    </el-col>
  </div>
</template>

<script>
import statistics from "./../components/statistics.vue";
export default {
  name: "home",
  data() {
    return {
      data: "",
    };
  },
  created() {},
  mounted() {
    var that = this;
    this.$ajax
      .post(
        this.$api + "/get_msg",
        this.$qs.stringify({ id: sessionStorage.getItem("name") })
      )
      .then(res => {
        that.data = res.data;
        that.$help.$emit("statis", { arr: res.data.visit_arr });
      });
  },
  components: {
    statistics
  },
  methods: {
    allcommetAdd() {
      var that = this;
      that.data.allComment++;
    },

    addtodayvisit() {
      var that = this;
      this.$ajax
        .post(
          this.$api + "/user_visit",
          this.$qs.stringify({ id: sessionStorage.getItem("name") })
        )
        .then(res => {
          that.data.todayVisit ++;
          
        });
    }
  }
};
</script>
<style lang='less'>
.blog-btn {
  font-size: 16px;
  border-radius: 10px;
  margin-left: 20px;
  margin-top: 20px;
  padding: 10px 40px;
  .blog-num {
    font-size: 22px;
    line-height: 40px;
    display: block;
  }
}

.today {
  background-color: #f0ad4e;
  border-color: #f0ad4e;
}
</style>
