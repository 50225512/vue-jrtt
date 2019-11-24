<template>
  <div class="app-container">
    <!-- 顶部header -->
    <mt-header fixed title="龙君网-Vue项目">
      <router-link to="/" slot="left">
        <mt-button class="message"> 
          <!-- <img src="./images/msg-box.png" height="24" width="24"> -->
          </mt-button>
      </router-link>
      <router-link to="/" slot="right">
        <mt-button class="search"> 
          </mt-button>
      </router-link>
    </mt-header>

    <!-- 内容区-->
		<transition>
    	<router-view></router-view>
		</transition>
    
    <!-- 底部Tabbar -->
    <nav class="mui-bar mui-bar-tab">
			<router-link class="mui-tab-item-llb" to="/home">
				<span class="mui-icon mui-icon-home"></span>
				<span class="mui-tab-label">首页</span>
			</router-link>
			<router-link class="mui-tab-item-llb" to="/member">
				<span class="mui-icon mui-icon-contact"></span>
				<span class="mui-tab-label">会员</span>
			</router-link>
			<router-link class="mui-tab-item-llb" to="/shopcar">
				<span class="mui-icon mui-icon-extra mui-icon-extra-cart">
          <span class="mui-badge">0</span>
        </span>
				<span class="mui-tab-label">购物车</span>
			</router-link>
			<router-link class="mui-tab-item-llb" to="/search">
				<span class="mui-icon mui-icon-search"></span>
				<span class="mui-tab-label">搜索</span>
			</router-link>
		</nav>

  </div>
</template>

<script>

</script>


<style lang="scss" scoped>
.app-container{
  padding-top: 40px;
	padding-bottom: 50px;
	overflow-x: hidden;
}

.v-enter{
	opacity: 0;
	transform: translateX(100%);
	
}

.v-leave-to{
	opacity: 0;
	transform: translateX(-100%);
	position: absolute;
}

.v-enter-active,
.v-leave-active{
	transition: all 0.5s ease;
}

// 该类名，解决 tabbar 点击无法切换的问题
.mui-bar-tab .mui-tab-item-llb.mui-active {
    color: #d43d3d;
}

.mui-bar-tab .mui-tab-item-llb {
    display: table-cell;
    overflow: hidden;
    width: 1%;
    height: 50px;
    text-align: center;
    vertical-align: middle;
    white-space: nowrap;
    text-overflow: ellipsis;
    color: #929292;
}

.mui-bar-tab .mui-tab-item-llb .mui-icon {
    top: 3px;
    width: 24px;
    height: 24px;
    padding-top: 0;
    padding-bottom: 0;
}

.mui-bar-tab .mui-tab-item-llb .mui-icon~.mui-tab-label {
    font-size: 11px;
    display: block;
    overflow: hidden;
    text-overflow: ellipsis;
}
.mint-header {
  background-color: #d43d3d
}

mint-header-button is-right{
  width: 30px;
}

a.mui-active{
  width: 30px;
  display: inline-block;
}

.message{
  background: url(./images/msg-box.png) no-repeat;
  background-size: contain;
  width: 28px;
  height: 24px;
  margin-left: 12px;
  display: block;
}

.search{
  background: url(./images/search.png) no-repeat;
  background-size: contain;
  width: 28px;
  height: 24px;
  margin-right: 12px;
  display: block;
}

</style>
