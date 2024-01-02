<template>
    <div id="cesiumContainer">
    </div>
    <div class="options">
        <button @click="flyto">摄像头</button>
        <button @click="show()" id="map">显示中国</button>
        <button @click="borderAdd()" id="map">加粗边线</button>
        <button @click="borderThin()" id="map">减小边线</button>
    </div>
    <div id="copyright">copyright</div>
</template>

<script setup>
import * as Cesium from 'cesium'//全量导出，as重命名
// import { Viewer } from "cesium";//与上面这种2选1，都是在引入cesium
import { ref, onMounted } from 'vue';

//必须用let设置viewer，否则将报 赋值常量 错误
let viewer = null
const initViewer = () => {
    viewer = new Cesium.Viewer("cesiumContainer");
    window.viewer = viewer//据说不能用vue绑定cesium的vivwer，网页会卡死，参考来源：https://blog.csdn.net/u011269322/article/details/123473196
};

//修改地图源及其属性，参考网址https://cloud.tencent.com/developer/article/2340505?areaId=106001
const provider = new Cesium.ImageryLayer.fromProviderAsync(
    Cesium.ArcGisMapServerImageryProvider.fromBasemapType(
        Cesium.ArcGisBaseMapType.SATELLITE
    ), { saturation: -1 })

const geoJson = Cesium.GeoJsonDataSource.load("https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json", {
    stroke: Cesium.Color.WHITE,
    fill: Cesium.Color.YELLOW.withAlpha(0.1),
    strokeWidth: 3,
})

window.myDataSource = geoJson//类似viewer，参考来源：https://blog.csdn.net/u011269322/article/details/123473196

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
    window.viewer.cesiumWidget.creditContainer.style.display = 'none'// 隐藏自带的logo信息
    window.viewer.cesiumWidget.creditContainer.style.background = '#ff0000'// 修改#copyright这个div的背景色
    // viewer.scene.camera.flyTo({
    //     destination: Cesium.Cartesian3.fromDegrees(110, 50),
    //     orientation: {
    //         // direction: new Cesium.Cartesian3(-0.04231243104240401, -0.20123236049443421, -0.97862924300734),
    //         up: new Cesium.Cartesian3(-0.47934589305293746, -0.8553216253114552, 0.1966022179118339)
    //     }
    // })
})

let dataSource = null
let entitydata = null
const show = () => {
    //为方便写zoomTo，所以用变量承载地图数据
    if (dataSource == null) {
        // if (window.viewer._dataSourceCollection._dataSources.length === 0) {
        dataSource = window.viewer.dataSources.add(window.myDataSource);
        entitydata = window.myDataSource.entities.values;
        window.viewer.zoomTo(dataSource)//视角切换到地图数据
        console.log("显示", dataSource);
        console.log("entitydata", entitydata);
    }
    else {
        // window.viewer._dataSourceCollection._dataSources[0]._primitives.destroyPrimitives = false//方法2：修改show属性，这句可以省略
        // window.viewer._dataSourceCollection._dataSources[0]._primitives.show = false//方法2：这句必写，包括下面那句
        // window.viewer.dataSources.removeAll(true)//方法1：移除整个数据源，直接写这句就行，上面方法2不用写
        window.viewer.dataSources.removeAll(window.myDataSource)//方法1的变形：直接移除指定的数据源
        dataSource = null
        console.log("隐藏", window.viewer._dataSourceCollection._dataSources);
    }
}

const borderAdd = () => {
    dataSource.strokeWidth = 10
    // if (window.viewer._dataSourceCollection._dataSources.length !== 0) {
    // 
    // let entitiesvalue = entitydata.values
    // for (let i = 0; i < entities.length; i++) {
    //     entity = entities[i];

    // }
    // console.log("加粗entities", entitiesvalue);
}
// const borderThin = () => {
//     window.viewer._dataSourceCollection._dataSources[0].entities.strokeWidth = --1
// }

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
