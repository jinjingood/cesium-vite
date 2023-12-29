<template>
    <div id="cesiumContainer">
    </div>
    <div class="options">
        <checkbox @click="flyto">摄像头</checkbox>
        <checkbox @click="show()" id="map">显示中国</checkbox>
    </div>
    <div id="copyright">copyright</div>
</template>

<script setup>
import * as Cesium from 'cesium'//全量导出，as重命名
// import { Viewer } from "cesium";//与上面这种2选1，都是在引入cesium
import { onMounted } from 'vue';

//修改地图源及其属性，参考网址https://cloud.tencent.com/developer/article/2340505?areaId=106001
const provider = new Cesium.ImageryLayer.fromProviderAsync(
    Cesium.ArcGisMapServerImageryProvider.fromBasemapType(
        Cesium.ArcGisBaseMapType.SATELLITE
    ), { saturation: -1 })
const geoJson = Cesium.GeoJsonDataSource.load("https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json", {
    stroke: Cesium.Color.WHITE,
    fill: Cesium.Color.RED.withAlpha(0.5),
    strokeWidth: 3,
})

//必须用let设置viewer，否则将报 赋值常量 错误
let viewer = null
const initViewer = () => {
    viewer = new Cesium.Viewer("cesiumContainer");
};

onMounted(() => {
    // // // 密钥
    // let defaultAccessToken  = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJjZjJiMTQyMS02MjdkLTQwY2UtYjg5OS03OTgyZWNhMDJjZjUiLCJpZCI6MTYzNzE0LCJpYXQiOjE2OTM0NjkyMTJ9.5-54WqrgLpJJve9sp2zQV6wVNHYrkweircvsncA_R9k';
    // // // 设置
    // Cesium.Ion.defaultAccessToken = defaultAccessToken;
    initViewer()

    // const viewer = new Cesium.Viewer('cesiumContainer', {
    //     homeButton: false,
    //     // baseLayer: provider,
    //     // baseLayer: Cesium.ImageryLayer.fromProviderAsync(
    //     //     Cesium.ArcGisMapServerImageryProvider.fromBasemapType(
    //     //         Cesium.ArcGisBaseMapType.SATELLITE
    //     //     ))//官网提供的方法1，可用
    //     // baseLayer: new Cesium.ImageryLayer(new Cesium.OpenStreetMapImageryProvider({
    //     //     url: "https://tile.openstreetmap.org/"
    //     // })),//官网提供的方法2，可用
    //     creditContainer: 'copyright',//logo上层加一个id为copyright的div

    // })
    viewer.cesiumWidget.creditContainer.style.display = 'none'// 隐藏自带的logo信息
    viewer.cesiumWidget.creditContainer.style.background = '#ff0000'// 修改#copyright这个div的背景色
    viewer.dataSources.add(geoJson)
})


const flyto = () => {
    viewer.scene.camera.flyTo('cesiumContainer', {
        destination: Cesium.Cartesian3.fromDegrees(-117.16, 32.71, 50000.0),
    })
    return
}

// switchshow = document.getElementById("map")
// switchshow.checked = false
// switchshow.addEventListener('click', () => {
//     viewer.dataSources.add(geoJson).show = switchshow.checked
// })


</script>

<style>
#cesiumContainer {
    position: absolute;
    width: 100%;
    height: 100%;
    border: 10px solid rgb(28, 71, 124);
}

.options {
    position: absolute;
    top: 20px;
    left: 20px;
}
</style>
