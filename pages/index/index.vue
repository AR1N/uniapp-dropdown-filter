<template>
	<view>
		<ren-dropdown-filter ref='renDropdownFilterRef' :filterData='filterData' :defaultIndex='defaultIndex'
		@onSelected='onSelected'>
		<template #other>
			<view class="filters">
				<view>自定义筛选条件:</view>
				<view class="items"><view class="item" @click="onConfirm(index)" v-for="(_,index) in 6" :key='index.toString()'>选项{{index}}</view></view>
			</view>
		</template>
		</ren-dropdown-filter>
		<view class='result'>筛选数据：</view>
		<view>{{result}}</view>
	</view>
</template>

<script setup lang="ts">
	import {ref} from 'vue'
	import RenDropdownFilter from '@/components/ren-dropdown-filter/ren-dropdown-filter.vue'
	const filterData = ref([
                    [{ text: '全部状态', value: '' }, { text: '状态1', value: 1 }, { text: '状态2', value: 2 }, { text: '状态3', value: 3 }, { text: '状态4', value: 4 }, { text: '状态5', value: 5 }, { text: '状态6', value: 6 },, { text: '状态7', value: 7 }],
                    [{ text: '全部类型', value: '' }, { text: '类型1', value: 1 }, { text: '类型2', value: 2 }, { text: '类型3', value: 3 }],
					[{ text: '其他条件', slotName: 'other' }]
                ])
				const defaultIndex = ref([0,0,0])
				const renDropdownFilterRef = ref()
				const result = ref([])
				const onSelected = (res)=>{
                result.value.splice(0,2,...res)
            }
			const onConfirm = (val)=>{
				result.value[2]={text:'其他条件',value:val}
				renDropdownFilterRef.value.closeMask()
			}
</script>

<style scoped>
	.result{
		padding-top: 200px;
	}
	.filters{
		padding: 24rpx;
	}
	.items{
		display: flex;
		flex-wrap: wrap;
	}
	.item{
		display: flex;
		margin: 20rpx;
		padding: 0 12rpx;
		border: 1px solid #53CC98;
		border-radius: 5rpx;
	}
</style>

