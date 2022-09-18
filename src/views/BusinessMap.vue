<template>
	<div class="map">
		<baidu-map class="mapId" :center="center" :zoom="zoom" :scroll-wheel-zoom="false" @moving="syncCenterAndZoom"
			@moveend="syncCenterAndZoom" @zoomend="syncCenterAndZoom" @ready="readyMap">
		</baidu-map>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				center: {
					lng: 117.339417,
					lat: 39.003545
				}, //初始的中心位置
				zoom: 13, //缩放级别
				// points: [], //打点的地方
				//百度地图初始化数据
				baidumapSwitch: false,
			}
		},
		mounted() {

		},
		methods: {
			//地图实例:初始化加载地图
			readyMap({
				BMap,
				map
			}) {
				// console.log(BMap, "BMap");
				// console.log(map, "map");
			},
			// clickHandler(e) {
			// 	console.log(e.point.lng);
			// 	console.log(e.point.lat);
			// 	this.center.lng = e.point.lng;
			// 	this.center.lat = e.point.lat;
			// },
			syncCenterAndZoom(e) {
				const {
					lng,
					lat
				} = e.target.getCenter()
				this.center.lng = lng
				this.center.lat = lat
				this.zoom = e.target.getZoom()
				console.log(this.zoom);
			}
		}
		//添加海量点
		// addPoints() {
		// 	const points = [];
		// 	for (var i = 0; i < n; i++) {
		// 		const position = {
		// 			lng: Math.random() * 40 + 85,
		// 			lat: Math.random() * 30 + 21
		// 		}
		// 		points.push(position)
		// 	}
		// 	this.points = points
		// }

	}
</script>
<style lang="scss" scoped>
	.map {
		background-color: #32d0ab;

		.mapId {
			height: 600px;
			width: 100%;
		}
	}
</style>
