<template>
	<div class="map">
		<baidu-map class="mapId" :center="center" :zoom="zoom" :scroll-wheel-zoom="true" @moving="syncCenterAndZoom"
			@moveend="syncCenterAndZoom" @zoomend="syncCenterAndZoom" @ready="readyMap">

			<li v-for="item in businessArr" @click="infoWindowOpen(item.businessId)">
				<bm-marker :position="{lng: item.lng, lat: item.lat}" :dragging="true"
					animation="BMAP_ANIMATION_BOUNCE">
					<bm-label content=item.businessName :labelStyle="{color: 'black', fontSize : '24px'}" />
				</bm-marker>
			</li>
			<!-- 缩放比例尺 -->
			<bm-navigation anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-navigation>

			<bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" :showAddressBar="true" :autoLocation="true">
			</bm-geolocation>

			<bm-marker :position="{lng:117.31851, lat: 39.003545}"></bm-marker>
		</baidu-map>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				center: {
					lng: 117.31851,
					lat: 39.003545
				}, //初始的中心位置
				zoom: 15, //缩放级别
				// points: [], //打点的地方
				//百度地图初始化数据
				baidumapSwitch: false,
				businessArr: [],
				user: {}
			}
		},
		created() {
			this.user = this.$getSessionStorage('user');
			//获取所有商家信息
			this.$axios.post('BusinessController/listBusinessByOrderTypeId').then(response => {
				this.businessArr = response.data;

			}).catch(error => {
				console.error(error);
			});
		},
		methods: {
			//地图实例:初始化加载地图
			readyMap({
				BMap,
				map
			}) {
				map.enableScrollWheelZoom();
			},
			syncCenterAndZoom(e) {
				const {
					lng,
					lat
				} = e.target.getCenter()
				this.center.lng = lng
				this.center.lat = lat
				this.zoom = e.target.getZoom()
				console.log(this.zoom);
			},
			infoWindowOpen(businessId){
				
			}
		}

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
