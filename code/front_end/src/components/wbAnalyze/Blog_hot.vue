<template>
  <div class="blog_hot">
    <img class="visual_conBot_l" src="../../assets/img/ksh42.png">
		<img class="visual_conBot_2" src="../../assets/img/ksh43.png">
		<img class="visual_conBot_3" src="../../assets/img/ksh44.png">
		<img class="visual_conBot_4" src="../../assets/img/ksh45.png">
    <div class="blog_hot_top">
      <svg class="icon" aria-hidden="true">
        <use xlink:href="#icon-redu"></use>
      </svg>
      <span>博文热度前十</span>
      <img src="../../assets/img/ksh33.png">
    </div>
    <div class="ten_hot_blogs">
      <div class="hot_blog" v-for="(hot_blog, index) in hot_blogs" :key="index">
        <div v-if="index == 0" class="serial_number red">{{ index + 1 }}</div>
        <div v-if="index == 1" class="serial_number orange">
          {{ index + 1 }}
        </div>
        <div v-if="index == 2" class="serial_number green">{{ index + 1 }}</div>
        <div v-if="index > 2" class="serial_number">{{ index + 1 }}</div>
        <div class="blog_text" @click="ToBolgDetail(hot_blog.weibo_id)">
          {{ hot_blog.text | snippet }}
        </div>
        <div class="proportional_bar" :style="{ '--width': hot_blog.hot_proportion }"></div>
        <div class="blog_redu">{{ hot_blog.hot_count }}</div>
      </div>
    </div>
    <div class="learnmore">
      <button>查看更多</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "blog_hot",
  data() {
    return {
      hot_blogs: [],
      total_hot: 0,
      tag_task_id: ''
    };
  },
  //定义过滤器超多20个字
  filters: {
    snippet(value) {
      if (value.length > 20) return value.slice(0, 20) + "...";
      return value;
    },
  },
  methods: {
    requsetHotBlog(id) {
      this.$axios
        .get("blog_rank?tag_task_id=" + id)
        .then((res) => {
          this.hot_blogs = res.data.data;
          console.log(this.hot_blogs)
          for (let index in this.hot_blogs) {
            this.total_hot =
              this.total_hot + Number(this.hot_blogs[index].hot_count);
          }
          for (let index in this.hot_blogs) {
            this.hot_blogs[index].hot_proportion = (
              (this.hot_blogs[index].hot_count / this.total_hot) *
              100
            ).toFixed(1);
            this.hot_blogs[index].hot_proportion += "%";
          }
        });
    },
    ToBolgDetail(weibo_id) {
      this.$router.push({
        path: "/blog_detail",
        query: {
          tag_task_id: this.tag_task_id,
          weibo_id: weibo_id
        }
      });
    },
  },
  mounted() {
    this.$bus.$on("send_tag_task_id", (tag_task_id) => {
      this.tag_task_id = tag_task_id;
      this.requsetHotBlog(tag_task_id);
    });
    // this.requsetHotBlog();
  },
  beforeDestroy() {
    this.$bus.$off("send_tag_task_id");
  },
};
</script>

<style scpoed>
.visual_conBot_l{position:absolute;left:0;top:0;}
.visual_conBot_2{position:absolute;right:0;top:0;}
.visual_conBot_3{position:absolute;right:0;bottom:0;}
.visual_conBot_4{position:absolute;left:0;bottom:0;}

.blog_hot_top span {
  color: #fff;
  font-size: 18px;
  line-height: 35px;
  margin: 2%;
  letter-spacing: 4px;
}

.blog_hot_top img {
  width: 100%;
  position: absolute;
  left: 0;
  bottom: 0;
}

.blog_hot {
  position: relative;
  /* background-color: white; */
  box-shadow: inset 0 0 7px 1px #6eb6ff;
  background-color: transparent;
  /* 设置背景透明度 */
  /* background-color: #fff;
  opacity: 0.5; */
  height: 57%;
  border-radius: 10px;
}

.blog_hot_top {
  margin: 0 0 0 20px;
  position: relative;
  height: 12%;
}

.learnmore {
  position: relative;
  float: top;
  height: 12%;
}

.learnmore button {
  display: block;
  margin: 0 auto;
  color: #8330fe;
  width: 15%;
  height: 60%;
  background-color: #fff;
  border: 0.1rem solid #8330fe;
  transition: 0.3s;
}

.learnmore button:hover {
  letter-spacing: 2px;
  color: #fff;
  background-color: #8330fe;
}

.ten_hot_blogs {
  height: 75%;
  position: relative;
  margin-left: 70px;
  margin-top: 10px;
}

.hot_blog {
  float: top;
  margin: 0.7% 0;
  width: 100%;
  height: 8%;
}

.serial_number,
.blog_text,
.blog_redu,
.proportional_bar {
  display: inline-block;
}
.blog_redu{
  color: #8707ff;
}

.serial_number {
  margin-right: 20px;
  width: 20px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 50%;
  color: #fff;
  background-color: #ccc;
}

.red {
  background-color: rgb(255, 0, 0);
}

.orange {
  background-color: rgb(255, 153, 0);
}

.green {
  background-color: rgb(0, 255, 13);
}

.blog_text {
  width: 50%;
  color: #FFFFFF;
  font-size: 14px;
  letter-spacing: 1px;
}

.blog_text:hover {
  cursor: pointer;
  color: #0fbcf9;
  letter-spacing: 1.5px;
}

.proportional_bar {
  height: 50%;
  width: 30%;
  background-color: #b3e2f3;
  border-radius: 6px;
  margin-right: 5px;
}

.proportional_bar::before {
  content: "";
  display: block;
  padding-left: 5px;
  height: 100%;
  max-width: var(--width);
  background-color: #b3e2f3;
  bottom: -28px;
  border-radius: 6px;
}

.proportional_bar::before {
  background-image: linear-gradient(90deg, #6526fa,#8c33ff, #a166fb);
}

.proportional_bar::before {
  animation-duration: 1.2s;
  animation-fill-mode: forwards;
  animation-timing-function: ease-in-out;
}

.proportional_bar::before {
  animation-name: slide;
}

@keyframes slide {
  from {
    width: 0;
  }

  to {
    width: 100%;
  }
}</style>