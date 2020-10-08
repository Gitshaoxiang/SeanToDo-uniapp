<template>
	<view class="drawer" :class="{ 'drawer-active' : drawerStatus }">
		<view class="drawer-nav">
			<image src="../../static/p1.jpg" class="avatar" mode="widthFix"></image>
			<uni-icons class="drawer-hide-icon" type="arrowright" size="30" color="#8a8a8a" @click="toggleDrawer"></uni-icons>
		</view>
		<view class="drawerList">
			<uni-list>
				<uni-swipe-action>
					<uni-swipe-action-item :key="item.id" v-for="item,index in groupList" :rightOptions="rightOptions" @click="swipeClick($event, index)">
						<uni-list-item @click="selectGroup(item,index)" style="width: 100%;" clickable :show-extra-icon="true" :extra-icon='groupIcon' :title="item.groupName" :show-badge="true" :badge-text="item.itemCount"></uni-list-item>
					</uni-swipe-action-item>
				</uni-swipe-action>
			</uni-list>
		</view>
		<view class="drawerFoot">
			<view @click="addGroup">
				<uni-icons type="plusempty" size="18" color="#8a8a8a" ></uni-icons>
				<text style="color: 8a8a8a;">新增任务组</text>
			</view>
		</view>
		<uni-popup ref="groupPopup" type="dialog">
			<uni-popup-dialog title="添加任务组" mode="input" @confirm="addGroupComfirm"></uni-popup-dialog>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				'groupIcon' : {
					type :'list',
					size : "18",
					color : "#8a8a8a"
				},
				'drawerStatus' : false,
				"rightOptions": [ 
					{
						text: '删除',
						style: {
							backgroundColor: '#dd524d'
						}
					}
				]
			};
		},
		props: [
			'groupList'
		],
		methods: {
			addGroup () {
				this.$refs.groupPopup.open()
			},
			addGroupComfirm (done,value) {
				if (value.trim() != ''){
					console.log(value)
					this.$emit('addGroup', value)
					done()
				}
			},
			selectGroup (currentGroup,index) {
				console.log(currentGroup)
				this.$emit('selectGroup', {data:currentGroup,index:index})
			},
			toggleDrawer () {
				console.log(123213)
				this.drawerStatus = ! this.drawerStatus
			},
			swipeClick (e,index) {
				setTimeout(()=>{
					this.$emit('delGroup' , index)
					console.log(index)
				}, 200)
			}
		}
	}
</script>

<style>
	.drawer {
		position: fixed;
		width: 80%;
		height: 100%;
		background-color: #fffffffa;
		z-index: 10;
		padding: 10px;
		left: -100%;
		transition: all .25s ease;
		box-shadow: 0 0 20px 4px #a7a7a7;
		display: flex;
		flex-direction: column;
		justify-content: space-between;
	}

	.drawer-active {
		left: 0;
	}

	.drawer-nav {
		 text-align: center;
		 position: relative;
		 margin: 20px 0px;
		 display: flex;
		 justify-content: space-evenly;
	}
	
	.drawer-nav .avatar {
/* 		height: 125px;
		width: 120px;
		border-radius: 20px;
		background-image: linear-gradient(to bottom, #007aff, #0dddff);
		color: white;
		line-height: 125px;
		font-size: 23px; */
		width: 120px;
		border-radius: 50%;
	}
	
	.drawerList {
		flex: 1;
	}
	
	.drawer-hide-icon {
		top: 50%;
		position: absolute;
		right: 0px;
		transform: translateY(-50%);
	}
	
	.drawerFoot {
		height: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
	}

</style>
