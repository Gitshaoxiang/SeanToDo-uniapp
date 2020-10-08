<template>
	<view class="container">
		<drawer-menu ref='drawerMenu' :groupList='groupList' @addGroup="addGroup" @delGroup="delGroup" @selectGroup="selectGroup"></drawer-menu>
		<view class="nav">	
			<uni-icons type="arrowthinleft" size="22" color="#fff" @click="$refs.drawerMenu.toggleDrawer()"></uni-icons>
			<view>
				<uni-icons type="personadd-filled" size="22" color="#0000"></uni-icons>
				<uni-icons type="more-filled" @click="$refs.drawerMenu.toggleDrawer()" size="24" color="#fff"></uni-icons>
			</view>
		</view>
		<item-list :currentGroup="currentGroup.data" @updateItem="updateItem"></item-list>
		<view class="foot">
			<view id="addBtn" @click="addItem">
				<icon type="clear" size="45" color="#ddd"/>
			</view>
		</view>
		<uni-popup ref="popup" type="dialog">
			<view>
				<lx-calendar style="background-color: white;margin-bottom: 25px;" @change='calendarChange' :value = "calendarData"></lx-calendar>
				<uni-popup-dialog style="margin: auto; margin-bottom: 30px;" title="添加新任务" mode="input" @confirm="confirm"></uni-popup-dialog>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"groupList": [
					{
						groupName : '默认分组',
						itemCount: '0',
						id: 'item->0',
						itemList: [
					
						],
						doneList: [
							
						]
					}
				],
				"currentGroup" : {
					data : {
							groupName : '默认分组',
							itemCount: '0',
							id: 'item->0',
							itemList: [
						
							],
							doneList: [
								
							]
						},
					index: 0
				},
				"calendarData" : ""
			};
		},
		methods: {
			addItem () {
				this.$refs.popup.open()
			},
			confirm (done,value) {
				console.log(value)
				if (value.trim() != ''){
					var currentItem = this.groupList[this.currentGroup.index]
					currentItem.itemList.push({
						"id": new Date().toLocaleDateString()+new Date().toLocaleTimeString(),
						"title" : value,
						"time" : this.calendarData
					})
					currentItem.itemCount = currentItem.itemList.length.toString()
					this.currentGroup.data = this.groupList[this.currentGroup.index]
					this.dataSave()
					done()
				}
			},
			calendarChange (val) {
				console.log(val)
				this.calendarData = val.fulldate
			},
			addGroup (val) {
				this.groupList.push(
					{
						groupName : val,
						itemCount: '0',
						id: 'item->' + this.groupList.length.toString(),
						itemList: [

						],
						doneList: [
							
						]
					}
				)
				this.currentGroup = {
					data : this.groupList[this.groupList.length-1],
					index : this.groupList.length-1
				}
				this.$refs.drawerMenu.toggleDrawer()
				this.dataSave()
			},
			delGroup (index) {
				if (this.currentGroup.index == index) {
					this.currentGroup = {
						data : '',
						index : ''
					}
				}else if (this.currentGroup.index > index){
					this.currentGroup.index = this.currentGroup.index - 1
					console.log("current -1")
				}
				this.groupList.splice(index,1)
				this.dataSave()
			},
			updateItem (val) {
				console.log(val)
				this.groupList[this.currentGroup.index] = val
				this.currentGroup.data = val
				this.dataSave()
			},
			selectGroup (val) {
				console.log(val)
				this.currentGroup = val
				this.dataSave()
				this.$refs.drawerMenu.toggleDrawer()
			},
			dataSave() {
				uni.setStorage({
					key: 'groupList',
					data: this.groupList,
					success: function () {
						console.log('groupList save success');
					}
				})
				uni.setStorage({
					key: 'currentGroup',
					data: this.currentGroup,
					success: function () {
						console.log('currentGroup save success');
					}
				})
			}
		},
		created () {
			uni.getStorage({
			    key: 'groupList',
			    success: (res) => {
					this.groupList = res.data
			    },
				fail() {
					console.log('get groupList error')
				}
			})
			uni.getStorage({
			    key: 'currentGroup',
			    success: (res) => {
					this.currentGroup = res.data
			    },
				fail() {
					console.log('get currentGroup error')
				}
			})
			let date = new Date();
			let nowMonth = date.getMonth() + 1,
				nowYear = date.getFullYear(),
				nowDay = date.getDate()
			this.calendarData = nowYear+'-'+nowMonth+'-'+nowDay
		}
	}
</script>

<style scoped>
	.container {
		padding: 0px;
		font-size: 14px;
		line-height: 24px;
		height: 100vh;
		display: flex;
		flex-direction: column;
	}
	.nav {
		margin-top: 70rpx;
		padding: 30rpx 30rpx 10rpx 30rpx;
		display: flex;
		justify-content: space-between;
	}
	.nav > view {
		display:flex; 
		width: 60px;
		justify-content: space-between;
	}

	
	.foot {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		height: 160rpx;
		background-color: rgb(0 0 0 / 20%);
		
	}
		
	#addBtn {
		position: absolute;
		right: 46rpx;
		bottom: 38rpx;
		transform: rotate(45deg);
	}
	
</style>
