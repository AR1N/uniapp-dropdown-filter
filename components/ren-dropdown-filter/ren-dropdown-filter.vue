<template>
  <view
    class="filter-wrapper"
    :style="{ 'borderTop': border ? '1rpx solid #f2f2f2' : 'none' }"
  >
    <view class="inner-wrapper">
      <view
        class="mask"
		:style="{height:sysInfo.windowHeight + 'px'}"
        :class="showMask ? 'show' : 'hide'"
        @tap="closeMask"
      />
      <view class="navs">
        <view
          v-for="(item, index) in navData"
          :key="index.toString()"
          class="c-flex-align c-flex-jcc nav_item"
          :class="{ 'c-flex-center': index > 0, actNav: index === actNav }"
          @tap="navClick(index)"
        >
          <view
            v-for="(child, childx) in item"
            :key="childx"
          >
            <view v-if="child?.select">
              {{ child.text }}
            </view>
          </view>
          <view :class="['triangle_down',index === actNav?'triangle_down_active':'']" />
        </view>
      </view>
      <scroll-view
        scroll-y="true"
        class="popup"
        :class="popupShow ? 'popupShow' : ''"
      >
        <view
          v-for="(item, index) in navData[actNav]"
          :key="index.toString()"
        >
          <template v-if="item?.slotName">
            <slot :name="item.slotName" />
          </template>
          <template v-else>
            <view
              class="item-opt c-flex-align"
              :class="item?.select ? 'actOpt' : ''"
              @tap="handleOpt(index)"
            >
              {{ item.text }}
            </view>
          </template>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script setup lang="ts">
import {ref,onMounted,nextTick,withDefaults} from 'vue'

const props = withDefaults(
  defineProps<{
    border?: boolean
    filterData: Array<any>
    defaultIndex: Array<number>
  }>(),
  {
    border: false,
    filterData: () => [],
    defaultIndex: () => [0],
  },
)

const navData = ref<any>([])
const popupShow = ref(false)
const showMask = ref(false)
const actNav = ref<undefined | number>(undefined)
const selIndex = ref<number[]>([]) // 选中条件索引
const emit = defineEmits(['onSelected'])
const sysInfo = ref()
onMounted(() => {
	sysInfo.value = uni.getWindowInfo()
  nextTick(() => {
    navData.value = props.filterData
    selIndex.value = props.defaultIndex
    keepStatus()
  })
})
const keepStatus = () => {
  navData.value.forEach((itemnavData) => {
    itemnavData.map((child) => {
      child.select = false
    })
    return itemnavData
  })
  for (let i = 0; i < selIndex.value.length; i++) {
    let selidx = selIndex.value[i]
    navData.value[i][selidx].select = true
  }
}

const navClick = (index) => {
  if (index === actNav.value) {
    closeMask()
    return
  }
  popupShow.value = true
  showMask.value = true
  actNav.value = index
}

const handleOpt = (index) => {
  selIndex.value[actNav.value as number] = index
  keepStatus()
  nextTick(() => {
    closeMask()
  })
 handleSelect()
}
const handleSelect = ()=>{
	let selectedData: any = []
	navData.value.forEach((item,idx) => {
	  let sel = item.find((child) => ((child.select || child[props.defaultIndex[idx]]) && !child.slotName))
	  if (sel) {
	    selectedData.push(sel)
	  }
	})
	const res = selectedData.map(i=>({...i})).map(j=>{
	 	delete j.select
	 	return j
	 })
	emit('onSelected', res)
}
const closeMask = () => {
  showMask.value = false
  popupShow.value = false
  actNav.value = undefined
  handleSelect()
}
defineExpose({ closeMask })
const discard = () => {
}
</script>

<style scoped >
.triangle_down{
  width: 0;
  height: 0;
  border-left: 12rpx solid transparent;
  border-right: 12rpx solid transparent;
  border-top: 12rpx solid #cccccc;
  margin-left: 10rpx;
  margin-top: 5rpx;
}
.triangle_down_active{
  border-top-color: #53CC98;
  transform:rotateX(180deg);
  transition:0.2s all;
}
.c-flex-align {
  display: flex;
  align-items: center;
}

.c-flex-jcc {
  display: flex;
  justify-content: center;
}

.c-flex-center {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.filter-wrapper {
  width: 100vw;
  z-index: 999;
}
  .inner-wrapper {
    position: relative;
  }
  .navs {
    position: relative;
    height: 90rpx;
    padding: 0 40rpx;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #fff;
    border-bottom: 2rpx solid #f5f6f9;
    color: rgba(0, 0, 0, 0.65);
    z-index: 999;
    box-sizing: border-box; 
  }
  .nav_item {
    flex: 1;
    height: 100%;
    flex-direction: row;
    z-index: 999;
  }
  .date {
    justify-content: flex-end;
  }
  
  .actNav {
    color: #53cc98;
  }
  .mask {
    z-index: 666;
    position: absolute;
    width: 100vw;
    background-color: rgba(0, 0, 0, 0);
    transition: background-color 0.15s linear;
  }
  .show {
    background-color: rgba(0, 0, 0, 0.4);
  }
  .hide {
    display: none;
  }
  .popup {
    position: absolute;
    max-height: 500rpx;
    background-color: #fff;
    border-bottom-left-radius: 20rpx;
    border-bottom-right-radius: 20rpx;
    overflow: scroll;
    z-index: 999;
    transition: all 1s linear;
    opacity: 0;
    display: none; 
  }
  .item-opt {
    height: 80rpx;
    padding: 0 40rpx;
    color: rgba(0, 0, 0, 0.65);
    border-bottom: 2rpx solid #f5f6f9;
  }
  .actOpt {
    color: #53cc98;
    position: relative;
  }
 .actOpt::after {
    content: '✓';
    font-size: 30rpx;
    position: absolute;
    right: 40rpx;
  }
 .popupShow {
      display: block;
      opacity: 1;
    }
  .icon-triangle {
    width: 16rpx;
    height: 16rpx;
    margin-left: 10rpx;
  }

</style>
