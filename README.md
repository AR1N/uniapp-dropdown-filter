# ren-dropdown-filter

### 简介
> 一款下拉筛选菜单，目前只在uni-app项目中使用，其他平台适配兼容未知，如有帮助到你不胜荣幸，欢迎Star！

> [插件市场地址](https://ext.dcloud.net.cn/plugin?id=2315)


### 调用示例

``` vue
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
```

### 属性说明

| 属性名 | 类型   | 默认值 | 说明   |
| :----- | :----- | :----- | :----- |
| filterData | Array | - | 筛选条件（二维数组） 格式：[[text: '全部状态', value: '1'],[text: '其他条件', slotName: 'other' ]]|
| defaultIndex | Array | [0] | 默认选中条件索引 |
| border | Boolean | false | 筛选栏顶部边框 |


### 事件说明 

| 事件名 | 说明   | 
| :----- | :----- | 
| onSelected | 筛选事件，返回筛选结果   |
| closeMask | 关闭遮罩   |


### 预览

[![ren-dropdown-filter](https://s21.ax1x.com/2025/10/14/pVbHWwQ.png)](https://imgchr.com/i/pVbHWwQ)
[![ren-dropdown-filter](https://s21.ax1x.com/2025/10/14/pVbHgOS.png)](https://imgchr.com/i/pVbHgOS)
[![ren-dropdown-filter](https://s21.ax1x.com/2025/10/14/pVbHReg.png)](https://imgchr.com/i/pVbHReg)