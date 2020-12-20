<template>
    <div>
        <baidu-map class="map" :center="map.center" :zoom="map.zoom" @ready="handler">
            <!--缩放-->
            <bm-navigation anchor="BMAP_ANCHOR_TOP_LEFT"></bm-navigation>
            <!--定位-->
            <bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" :showAddressBar="true" :autoLocation="true"></bm-geolocation>
            <!--点-->
            <bm-marker :position="map.center" animation="BMAP_ANIMATION_DROP">
            </bm-marker>
        </baidu-map>
    </div>
</template>

<script>
export default {
  name: "demo",
  data: () => ({
    map: {
      center: {lng: 117.087013, lat: 36.200089},
      zoom: 15,
      show: true,
      dragging: true
    },
  }),
  methods: {
    handler ({BMap, map}) {
      console.log(BMap, map)
      // 鼠标缩放
      map.enableScrollWheelZoom(true);
      // 点击事件获取经纬度
      map.addEventListener('click', function (e) {
        console.log(e.point.lng, e.point.lat)
      })
    }
  }
}
</script>

<style scoped>
    .map {
        width: 100%;
        height: 400px;
    }
</style>
