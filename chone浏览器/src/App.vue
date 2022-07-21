<template>
  <nav>
    <div id="container"></div>
  </nav>
</template>

<style lang="scss">
body,
div,
h1 {
  margin: 0;
  padding: 0;
}
#container {
  padding: 0px;
  margin: 0px;
  width: 100vw;
  height: 100vh;
}
</style>
<script>
import AMapLoader from "@amap/amap-jsapi-loader";
export default {
  data() {
    return {
      map: null,
    };
  },
  created(){

  },
  methods: {
    initMap() {
      AMapLoader.load({
        key: "dd4f8bf5a92b96ccd483d782c309ddd7", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: ["AMap.Scale"], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      })
        .then((AMap) => {
          // 创建一个 Marker 实例：
          var marker = new AMap.Marker({
              position: new AMap.LngLat(116.39, 39.9),   // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
              title: '北京'
          });
          this.map = new AMap.Map("container", {
            //设置地图容器id
            viewMode: "3D", //是否为3D地图模式
            zoom: 5, //初始化地图级别
            center: [105.602725, 37.076636], //初始化地图中心点位置
          });
          this.map.add(marker);
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted(){
    //DOM初始化完成进行地图初始化
    this.initMap();
  }
};
</script>
