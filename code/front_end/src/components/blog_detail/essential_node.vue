<template>
  <div class="essential_node">
    <img class="visual_conBot_l" src="../../assets/img/ksh42.png">
		<img class="visual_conBot_2" src="../../assets/img/ksh43.png">
		<img class="visual_conBot_3" src="../../assets/img/ksh44.png">
		<img class="visual_conBot_4" src="../../assets/img/ksh45.png">

    <div class="essential_node_title">关键转发节点</div>
    <div class="essential_node_contents">
      <div
        class="essential_node_content"
        v-for="(essential_node, index) in essential_nodes"
        :key="index"
      >
        <div class="essential_node_number red" v-if="index == 0">
          {{ index + 1 }}
        </div>
        <div class="essential_node_number orange" v-if="index == 1">
          {{ index + 1 }}
        </div>
        <div class="essential_node_number blue" v-if="index == 2">
          {{ index + 1 }}
        </div>
        <div class="essential_node_number" v-if="index >= 3">
          {{ index + 1 }}
        </div>
        <span>{{ essential_node.name }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "essential_node",
  data() {
    return {
      essential_nodes: "",
    };
  },
  methods: {
    getEssentialNode() {
      let query = this.$route.query;
      this.$axios
        .get("comment/key_node?tag_task_id="+query.tag_task_id+"&weibo_id="+query.weibo_id)
        .then((res) => {
          this.essential_nodes = res.data.data;
        });
    },
  },
  mounted() {
    this.getEssentialNode();
  },
};
</script>

<style scoped>
.essential_node {
  position: absolute;
  width: 100%;
  margin-left: 5px;
  top: 58%;
  height: 42%;
  box-shadow:inset 0 0 7px 1px #6eb6ff;
  border-radius: 10px;
  background-color:transparent;
}
.essential_node_title {
  margin: 10px 20px;
  padding: 5px;
  font-weight: 600;
  letter-spacing: 1px;
  color: #fff;
  font-size: 14px;
  margin: 2%;
  letter-spacing: 4px;
}
.essential_node_content {
  margin-left: 20px;
  padding: 5px;
  font-size: 14px;
  color: #fff;
}
.essential_node_number {
  display: inline-block;
  width: 20px;
  height: 20px;
  text-align: center;
  background-color: #ccc;
  color: #fff;
  margin-right: 10px;
}
.red {
  background-color: rgb(255, 0, 0);
}
.orange {
  background-color: rgb(255, 115, 0);
}
.blue {
  background-color: rgb(0, 183, 249);
}
</style>