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
     initAMap(AMap) {
      const _this = this;
      AMap.plugin("AMap.Geolocation", function () {
        var geolocation = new AMap.Geolocation({
          // 是否使用高精度定位，默认：true
          enableHighAccuracy: true,
          // 设置定位超时时间，默认：无穷大
          timeout: 10000,
          // 定位按钮的停靠位置的偏移量
          offset: [0,0],
          //  定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
          zoomToAccuracy: true,
          //  定位按钮的排放位置,  RB表示右下
          position: "RB",
        });

        geolocation.getCurrentPosition(function (status, result) {
          if (status == "complete") {
            onComplete(result);
          } else {
            onError(result);
          }
        });
        function onComplete(data) {
          // data是具体的定位信息    data是当前位置
          console.log(data);
          const { lat, lng } = data.position;
          _this.map = new AMap.Map("container", {
            //设置地图容器id
            viewMode: "3D", //是否为3D地图模式
            zoom: 16, //初始化地图级别  把地图绽放到街道级别;
            center: [ lng,lat], //初始化地图中心点位置
            // center 接收一个数组，数组中第一个参数是 经度 第二个是纬度
          });
        }

        function onError(data) {
          // 定位出错
          console.log(data);
        }
      });
    },
    initMap() {
      AMapLoader.load({
        key: "6ec4d154ea3899b2388e892470b0dffe", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: [""], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      })
        .then((AMap) => {
          this.initAMap(AMap);
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
