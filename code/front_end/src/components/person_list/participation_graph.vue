<template>
  <div class="participation_graph">
    <div id="participation_graph">
      <img class="visual_conBot_l" src="../../assets/img/ksh42.png">
		  <img class="visual_conBot_2" src="../../assets/img/ksh43.png">
		  <img class="visual_conBot_3" src="../../assets/img/ksh44.png">
		  <img class="visual_conBot_4" src="../../assets/img/ksh45.png">
    </div>
  </div>
</template>

<script>
export default {
  name: "participation_graph",
  methods: {
    getParticipationGraph() {
      let myChart = this.$echarts.init(
        document.getElementById("participation_graph")
      );
      let option;

      myChart.showLoading();
      let query = this.$route.query;
      this.$axios.get("retweet/detail_relation_graph?tag_task_id=" + query.tag_task_id).then((res) => {
          let graph = res.data.data;
        console.log(graph)
        myChart.hideLoading();
          graph.nodes_list.forEach(function (node) {
            node.label = {
              show: node.symbolSize > 5,
            };
          });
          option = {
            title: {
              text: "Participation Graph",
              subtext: "Default layout",
              top: "bottom",
              left: "right",
            },
            tooltip: {},
            legend: [
              {
                // selectedMode: 'single',
                data: graph.categories.map(function (a) {
                  return a.name;
                }),
              },
            ],
            animationDuration: 1500,
            animationEasingUpdate: "quinticInOut",
            series: [
              {
                name: "Participation Graph",
                type: "graph",
                layout: "force",
                data: graph.nodes_list,
                links: graph.links_list,
                categories: graph.categories,
                roam: true,
                label: {
                  position: "right",
                  formatter: "{b}",
                },
                lineStyle: {
                  color: "source",
                  curveness: 0.3,
                },
                emphasis: {
                  focus: "adjacency",
                  lineStyle: {
                    width: 10,
                  },
                },
              },
            ],
          };
          myChart.setOption(option);
        });
      option && myChart.setOption(option);
    },
  },
  mounted(){
      this.getParticipationGraph()
  }
};
</script>

<style scoped>
/* .participation_graph {
  width: 1080px;
  height: 840px;
  background-color: transparent;
  box-shadow:inset 0 0 9px 1px white;
} */
#participation_graph{
    width: 1080px;
    height: 840px;
    background-color:transparent;
    box-shadow:inset 0 0 7px 1px #6eb6ff;
    border-radius: 10px;
}
</style>