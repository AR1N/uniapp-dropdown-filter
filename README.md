# ren-dropdown-filter

### 简介
> 一款下拉筛选菜单，目前只在uni-app项目中使用，其他平台适配兼容未知，如有帮助到你不胜荣幸，欢迎Star！
> [DCloud插件市场地址](https://ext.dcloud.net.cn/plugin?id=2315)


### 调用示例

```
<view class="root">
    <ren-dropdown-filter :filterData='filterData' :defaultIndex='defaultIndex'
    @onSelected='onSelected' @dateChange='dateChange'></ren-dropdown-filter>
</view>
```

```
import RenDropdownFilter from '@/components/ren-dropdown-filter/ren-dropdown-filter.vue'
	export default {
        components:{
            RenDropdownFilter
        },
		data() {
			return {
				filterData:[
                    [{ text: '全部状态', value: '' }, { text: '状态1', value: 1 }, { text: '状态2', value: 2 }, { text: '状态3', value: 3 }],
                    [{ text: '全部类型', value: '' }, { text: '类型1', value: 1 }, { text: '类型2', value: 2 }, { text: '类型3', value: 3 }]
                ],
                defaultIndex:[0,0]
			}
		},
		onLoad() {

		},
		methods: {
            onSelected(res){
                console.log(res)
            },
            dateChange(d){
               uni.showToast({
                   icon:'none',
                   title:d
               })
            }
		}
	}
```

### 属性说明

| 属性名 | 类型   | 默认值 | 说明   |
| :----- | :----- | :----- | :----- |
| filterData | Array | - | 筛选条件（二维数组） 格式：[[text: '全部状态', value: '1'],[text: '全部类型', value: '2']]|
| defaultIndex | Array | [0] | 默认选中条件索引 |
| height | Number | 108 | 筛选栏高度（单位rpx） |
| top | String | calc(var(--window-statsu-bar) + 44px) | 筛选栏距离顶部的高度 |
| border | Boolean | false | 筛选栏顶部边框 |


### 事件说明

| 事件名 | 说明   | 
| :----- | :----- | 
| onSelected | 筛选事件，返回筛选结果   |
| dateChange | 日期筛选事件，返回筛选日期   |

### Tips

* 如果不用日期筛选可自行更改组件源代码来满足需求

### 预览

![ren-dropdown-filter](https://i.loli.net/2020/07/15/LPkWjuDlw1tgTYo.png)
![ren-dropdown-filter](https://i.loli.net/2020/07/15/tAkJy24buCKrSIo.png)
![ren-dropdown-filter](https://i.loli.net/2020/07/15/ZdOVzB8Lc13Qphe.png)
![ren-dropdown-filter](https://i.loli.net/2020/07/15/7xm8Aj2fNKYB45s.png)
