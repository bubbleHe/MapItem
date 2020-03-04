<template>
  <div class="hello">
    <!-- 地图盒子 -->
    <div ref="mymapbox" class="box"></div>
  </div>
</template>

<script>
  import echarts from "echarts"; //引进echarts
  import "echarts/map/js/china.js";
  import jsonp from "jsonp";
  const option = {
    title: {
      text: "实时疫情地图"
    },
    series: [
      {
        name: "确诊人数",
        type: "map", //图形类型
        map: "china", //地图类型 中国
        label: {
          show: true,
          fontSize: 10
        },
        zoom: 1.2, //控制地图放大缩小

        roam:true,
        itemStyle: {
          areaColor: "white",
          borderColor: "green"
        },
        emphasis: {
          //鼠标移动时样式
          itemStyle: {
            areaColor: "pink"
          }
        },
        data: [] //放数据
      }
    ],
    visualMap: [
      {
        type: "piecewise", //显示地图下 分段条
        splitNumber: 4, //分段条数量
        pieces: [
          {
            min: 10000
          },
          {
            min: 1000,
            max: 9999
          },
          {
            min: 100,
            max: 999
          },
          {
            min: 10,
            max: 99
          }
        ],
        align: "right", //分段图形位置
        inRange: {
          symbol: "rect",
          color: ["#BC8F8F", "#CD5C5C", "#CD5C5C", "#FF0000"]
        }
      }
    ],

    tooltip: {
      trigger: "item"
    }
  };
  export default {
    name: "HelloWorld",

    mounted() {
      this.gerDatas();
      this.myecharts = echarts.init(this.$refs.mymapbox);
      this.myecharts.setOption(option);
    },
    methods: {
      gerDatas() {
        jsonp(
          "http://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427",
          {},
          (err, data) => {
            if (!err) {
              // console.log(data)
              const list = data.data.list.map(item => ({
                name: item.name,
                value: item.value
              }));
              // const list =data.data.list.map(item=> {
              //   return {name:item.name,value:item.value}
              // })
              option.series[0].data = list;
              this.myecharts.setOption(option);
            }
          }
        );
      }
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .box {
    width: 800px;
    height: 600px;
    /* background-color: blue; */
    margin: 0 auto;
    padding: 100px;
  }
</style>
