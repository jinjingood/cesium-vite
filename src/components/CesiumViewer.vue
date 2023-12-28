<template>
    <div id="cesiumContainer"></div>
</template>

<script setup>
import * as Cesium from 'cesium'
import { onMounted } from 'vue';

// 密钥
let defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJjZjJiMTQyMS02MjdkLTQwY2UtYjg5OS03OTgyZWNhMDJjZjUiLCJpZCI6MTYzNzE0LCJpYXQiOjE2OTM0NjkyMTJ9.5-54WqrgLpJJve9sp2zQV6wVNHYrkweircvsncA_R9k';
// 设置
Cesium.Ion.defaultAccessToken = defaultAccessToken;

onMounted(() => {
    const viewer = new Cesium.Viewer('cesiumContainer', {
        homeButton: false,
        baseLayer: Cesium.ImageryLayer.fromProviderAsync(
            Cesium.ArcGisMapServerImageryProvider.fromBasemapType(
                Cesium.ArcGisBaseMapType.SATELLITE
            )
            //https://cesium.com/learn/cesiumjs/ref-doc/ArcGisMapServerImageryProvider.html?classFilter=ArcGisMapServerImageryProvider
        ),

    })

})
// const bing = await Cesium.BingMapsImageryProvider.fromUrl(
//     "https://dev.virtualearth.net", {
//     key: "get-yours-at-https://www.bingmapsportal.com/",
//     mapStyle: Cesium.BingMapsStyle.AERIAL
// });

</script>
