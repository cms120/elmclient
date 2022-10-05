<template>
  <div class="wrapper">
    <!-- header部分 -->
    <header>
      <p>编辑送货地址</p>
    </header>
    <!-- 表单部分 -->
    <ul class="form-box">
      <li>
        <div class="title">
          联系人：
        </div>
        <div class="content">
          <input type="text" v-model="deliveryAddress.contactName" placeholder="联系人
姓名">
        </div>
      </li>
      <li>
        <div class="title">
          性别：
        </div>
        <div class="content" style="font-size: 3vw;">
          <input type="radio" v-model="deliveryAddress.contactSex" value="1" style="width:6vw;height:3.2vw;"
                 checked>男
          <input type="radio" v-model="deliveryAddress.contactSex" value="0" style="width:6vw;height:3.2vw;">女
        </div>
      </li>
      <li>
        <div class="title">
          电话：
        </div>
        <div class="content">
          <input type="tel" v-model="deliveryAddress.contactTel" placeholder="电话">
        </div>
      </li>
      <li>
        <div class="title">
          收货地址：
        </div>
        <div class="sel01">
          <select v-model="selectClassEnd" @change="selectClass($event)">
            <option value="NONE">未选择</option>
            <option v-for="(item,index) in addressArr" :key="index" :value="item.addressId">
              {{ item.addressExplain }}
            </option>
          </select>
        </div>
      </li>
      <!-- <li>
        <div class="title">
          纬度：
        </div>
        <div class="content">
          <input type="number" v-model="deliveryAddress.address.latitudes" placeholder="纬度">
        </div>
      </li>
      <li>
        <div class="title">
          经度：
        </div>
        <div class="content">
          <input type="number" v-model="deliveryAddress.address.longitudes" placeholder="经度">
        </div>
      </li> -->
    </ul>
    <div class="button-add">
      <button @click="editUserAddress">更新</button>
    </div>
    <!-- 底部菜单部分 -->
    <Footer></Footer>
  </div>
</template>
<script>
import Footer from '../components/Footer.vue';

export default {
  name: 'EditUserAddress',
  data() {
    return {
      addressArr: [],
      selectClassEnd: "NONE",//类别默认选择
      select_class_id: "",//类别id提交报名需要

      businessId: this.$route.query.businessId,
      daId: this.$route.query.daId,
      user: {},
      deliveryAddress: {}
    }
  },
  created() {
    this.user = this.$getSessionStorage('user');
    this.$axios.post('DeliveryAddressController/getDeliveryAddressById',
        this.$qs.stringify({
          daId: this.daId
        })).then(response => {
      this.deliveryAddress = response.data;
    }).catch(error => {
      console.error(error);
    });
    this.$axios.post('AddressController/listAddress').then(response => {
      this.addressArr = response.data;
    }).catch(error => {
      console.error(error);
    });
  },
  components: {
    Footer
  },
  methods: {
    editUserAddress() {
      if (this.deliveryAddress.contactName == '') {
        alert('联系人姓名不能为空！');
        return;
      }
      if (this.deliveryAddress.contactTel == '') {
        alert('联系人电话不能为空！');
        return;
      }
      this.$axios.post('DeliveryAddressController/updateDeliveryAddress',
          this.$qs.stringify(
              this.deliveryAddress
          )).then(response => {
        if (response.data > 0) {
          this.$router.push({
            path: '/userAddress',
            query: {
              businessId: this.businessId
            }
          });
        } else {
          alert('更新地址失败！');
        }
      }).catch(error => {
        console.error(error);
      });
    },
    selectClass(event) {
      this.select_class_id = event.target.value; //获取option对应的value值 select_class_id是后台约定的提交数据的名称
      this.deliveryAddress.addressId = event.target.value;
      // alert(this.deliveryAddress.addressId)
    }
  }
}
</script>
<style scoped>
/*************** 总容器 ***************/
.wrapper {
  width: 100%;
  height: 100%;
}

/*************** header ***************/
.wrapper header {
  width: 100%;
  height: 12vw;
  background-color: #0097FF;
  display: flex;
  justify-content: space-around;
  align-items: center;
  color: #fff;
  font-size: 4.8vw;
  position: fixed;
  left: 0;
  top: 0;
  /*保证在最上层*/
  z-index: 1000;
}

/*************** （表单信息） ***************/
.wrapper .form-box {
  width: 100%;
  margin-top: 12vw;
}

.wrapper .form-box li {
  box-sizing: border-box;
  padding: 4vw 3vw 0vw 3vw;
  display: flex;
}

.wrapper .form-box li .title {
  flex: 0 0 18vw;
  font-size: 3vw;
  font-weight: 700;
  color: #666;

}

.wrapper .form-box li .content {
  flex: 1;
  display: flex;
  align-items: center;
}

.wrapper .form-box li .content input {
  border: none;
  outline: none;
  width: 100%;
  height: 4vw;
  font-size: 3vw;
}

.wrapper .form-box li .sel01 {
  display: inline-block;
  position: relative;
  z-index: 2;
  font-size: 3vw;
  height: 4vw;
  line-height: 4vw;
  width: 100%;
  flex: 1;
  background: #fff;
  box-sizing: border-box;
  border-radius: .5rem;
}

.wrapper .form-box li .sel01:before {
  content: "";
  position: absolute;
  width: 0;
  height: 0;
  border: .5rem solid transparent;
  border-top-color: #0097FF;
  top: 50%;
  right: 1rem;
  cursor: pointer;
  z-index: -2;
  margin-top: -0.25rem;
}

.wrapper .form-box li .sel01 select {
  width: 100%;
  border: none;
  height: 4vw;
  background: transparent;
  background-image: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  vertical-align: top;
  padding-left: 1rem;
}

.wrapper .form-box li .sel01 select:focus {
  outline: none;
}


.wrapper .button-add {
  box-sizing: border-box;
  padding: 4vw 3vw 0vw 3vw;
}

.wrapper .button-add button {
  width: 100%;
  height: 10vw;
  font-size: 3.8vw;
  font-weight: 700;
  border: none;
  outline: none;
  border-radius: 4px;
  color: #fff;
  background-color: #38CA73;
}
</style>
