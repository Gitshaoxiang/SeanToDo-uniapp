<template>
	<view>
		<view class="listTitle">
			<h1>{{currentGroup.groupName}}</h1>
		</view>
		<uni-list class="item-list" :border="false">
			<uni-swipe-action>
				<uni-swipe-action-item :key="item.id" class="item" v-for="item,index in currentGroup.itemList" :leftOptions="leftOptions" :rightOptions="rightOptions"  @click="onClick($event, index)" @change="swipeChange($event, index)">
					<uni-list-item :disabled="false" :title="item.title" :rightText="item.time"></uni-list-item>
				</uni-swipe-action-item>
				<view v-show="currentGroup.doneList !== undefined  &&  currentGroup.doneList.length > 0 " class="done-list" @click="openDonelist">
					<p>Finished</p>
					<uni-icons type="arrowright" :class="{'arrowActive': showDoneList}" class='arrowfault'  size="16" color="#fff"></uni-icons>
				</view>
				<view :class="{'doneListActive': showDoneList}" class='doneListdefault'>
					<uni-swipe-action-item v-show="true" :key="item.id" class="item" v-for="item,index in currentGroup.doneList" :leftOptions='redoOptions' :rightOptions='redoOptions' @click="onClick($event, index)" @change="swipeChange($event, index)">
						<uni-list-item :disabled="true" :title="item.title"></uni-list-item>
					</uni-swipe-action-item>
				</view>
			</uni-swipe-action>
		</uni-list>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				"showDoneList": false,
				"leftOptions": [
					{
						text: '完成',
						style: {
							backgroundColor: '#007aff'
						}
					}, {
						text: '置顶',
						style: {
							backgroundColor: '#ababab'
						}
					}
				],
				"rightOptions": [ 
					{
						text: '删除',
						style: {
							backgroundColor: '#dd524d'
						}
					}
				],
				"redoOptions": [
					{
						text: '恢复',
						style: {
							backgroundColor: '#0095b7'
						}
					}
				]
			};
		},
		props: [
			'currentGroup'
		],
		methods: {
			onClick(e,index){
			  console.log(e)
			  setTimeout(()=>{
			  var updateData = this.currentGroup
			  // this.currentGroup = ''
			  if (e.content.text === '删除'){  
				updateData.itemList.splice(index,1)
			  } else if (e.content.text === '完成') {
				var item = updateData.itemList.splice(index,1)[0]
				updateData.doneList.unshift(item)
			  } else if (e.content.text === '置顶') {
				var item = updateData.itemList.splice(index,1)[0]
				updateData.itemList.unshift(item)
			  } else if (e.content.text === '恢复') {
				var item = updateData.doneList.splice(index,1)[0]
				updateData.itemList.push(item)
			  }
				updateData.itemCount = updateData.itemList.length.toString()
				this.$emit('updateItem', updateData)  
			  },200)
			},
			swipeChange(e,index){
			  console.log(e)
			},
			openDonelist(){
				this.showDoneList = !this.showDoneList
			}
		}
	}
</script>

<style>
	.listTitle {
		color: white;
		margin-top: 20rpx;
		padding: 0rpx 30rpx 20rpx 30rpx;
	}
	.item-list {
		flex: 1;
		overflow: scroll;
		margin: 10rpx 30rpx 0rpx 30rpx;
		padding-top: 10rpx;
		background-color: transparent;
		border: none;
	}
	.item-list .item {
		margin-bottom: 10px;
		border-radius: 10rpx;
	}
	
	.item-list .item:last-child {
		margin-bottom: 140rpx;
	}
	
	.item-list .uni-list-item {
		background-color: rgb(255 255 255 / 80%);
		width: 100%;
	}

	
	.done-list {
		background-color: rgb(0 0 0 / 40%);
		padding: 4px 6px;
		width: 80px;
		color: white;
		display: flex;
		justify-content: space-between;
		margin-bottom: 10px;
	}
	
	.doneListdefault {
		transition: all .25s ease !important;
		height: 0px;
		overflow: hidden;
	}

	
	.doneListActive {
		transition: all .25s ease !important;
		height: 100%;
	}
	
	.arrowdefault {
		transition: all .25s ease !important;
		transform: rotate(0deg);
	}
	
	.arrowActive {
		transition: all .25s ease !important;
		transform: rotate(90deg)!important;
	}
	
</style>



