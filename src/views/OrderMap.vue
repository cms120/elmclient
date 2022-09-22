<template>
	<div class="map">
		<baidu-map class="mapId" :center="center" :zoom="zoom" :scroll-wheel-zoom="true" @moving="syncCenterAndZoom"
			@moveend="syncCenterAndZoom" @zoomend="syncCenterAndZoom" @ready="readyMap">

			<!-- 用户订单派送地址 -->
			<bm-marker :position="{lng:orders.deliveryaddress.longitudes , lat: orders.deliveryaddress.latitudes}" :dragging="true">
				<bm-label content="UserAddress" :labelStyle="{color: 'black', fontSize : '4px'}"
					:offset="{width: -35, height: 30}" />

			</bm-marker>
			
			<!-- 商家地址 -->
			<bm-marker  :position="{lng: orders.business.Address.longitude, lat: orders.business.Address.latitudes}" :dragging="true"
				@click="toBusinessInfo(orders.business.businessId)">
				<bm-label :content="orders.business.businessName" :labelStyle="{color: 'black', fontSize : '4px'}"
					:offset="{width: -35, height: 30}" />
			</bm-marker>
			
			

			<!-- 缩放比例尺 -->
			<bm-navigation anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-navigation>

			<bm-geolocation anchor="BMAP_ANCHOR_BOTTOM_RIGHT" :showAddressBar="true" :autoLocation="true">
			</bm-geolocation>

			<!-- <bm-marker :position="{lng:117.32064, lat: 39.005848}"></bm-marker> -->
		</baidu-map>
	</div>
</template>

<script>
	export default {
		name: 'OrderMap',
		data() {
			return {
				center: {
					lng: 117.31851,
					lat: 39.003545
				}, //初始的中心位置
				zoom: 15, //缩放级别
				//百度地图初始化数据
				baidumapSwitch: false,
				orderId: this.$route.query.orderId,
				orders: {
					business: {},
					deliveryaddress:{}
				},
				user: {}
			}
		},
		created() {
			this.user = this.$getSessionStorage('user');
			//获取所有商家信息
			this.$axios.post('OrdersController/getOrdersById', this.$qs.stringify({
				orderId: this.orderId
			})).then(response => {
				this.orders = response.data;
				// alert(response.data);
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
			toBusinessInfo(businessId) {
				this.$router.push({
					path: '/businessInfo',
					query: {
						businessId: businessId
					}
				});
			}
		}

	}
</script>
<style lang="scss" scoped>
	.map {
		background-color: #32d0ab;

		.mapId {
			height: 900px;
			width: 100%;
		}
	}
</style>
