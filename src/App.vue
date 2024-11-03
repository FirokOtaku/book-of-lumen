
<style scoped>
.trans, .trans > *
{
  transition: all ease-in-out 0.5s;
}
.tiny-space
{
  margin: calc(1px * var(--scale, 1));
}
.small-space
{
  margin: calc(2px * var(--scale, 1));
}
.space
{
  margin: calc(4px * var(--scale, 1));
}
</style>

<template>
  <div @click="clickWhiteSpace" class="full">

    <div class="fixed middle-align center-align trans"
         :style="{
           width: `${ps.baseWidth}px`,
           height: `${ps.baseHeight}px`,
           marginTop: `${ps.baseMarginY}px`,
           marginLeft: `${ps.baseMarginX}px`,
           fontSize: `calc(8px * ${ps.coreScale}) !important`,
           '--scale': ps.coreScale,
         }"
    >
      <div class="trans"
           :style="{
             width: `${ps.coreWidth}px`,
             height: `${ps.coreHeight}px`,
           }"
      >
        <div class="absolute row no-space center-align black-text trans"
             :style="{
               left: `${ps.coreOffsetX}px`
             }">

          <div class="book-page-left center-align"
               :class="displayState"
               @click.self.capture.prevent="clickBookPageLeft">
            <div class="center-align">
              <img src="/avatar-lumen.avif" alt=""
                   class="trans"
                   :style="{ width: `${ps.coreScale * 60}px`, height: `${ps.coreScale * 60}px` }">
            </div>
            <div style="font-size: 25%" class="grey-text ">* 可爱图片仅供参考, 请以抽象主播实物为准</div>

            <div class="small-space"></div>

            <div>
              <a href="https://space.bilibili.com/3546608903915553/" target="_blank" style="font-size: 50%; " class="no-line">
                <span class="icon-bilibili"></span>
                银禾 Lumen 的 Bilibili 个人空间
              </a>
            </div>
            <div class=""></div>
            <div>
              <a href="https://live.bilibili.com/31770358" target="_blank"  style="font-size: 50%;" class="no-line">
                <span class="icon-bilibili"></span>
                银禾 Lumen 的直播间
              </a>
            </div>

            <div class="small-space"></div>

            <div>
              银禾 Lumen 可爱喵
            </div>
            <div class="tiny-space"></div>
            <div>
              关注银禾 Lumen 喵
            </div>
            <div class="tiny-space"></div>
            <div>
              关注银禾 Lumen 谢谢喵
            </div>
          </div>

          <div class="book-page-right"
               :class="displayState"
               @click.self.capture.prevent="clickBookPageRight">
            <div class="middle center center-align">

              <template v-if="dayAfterLastMove === 0">

                <div>
                  {{ textToday }}
                </div>
                <div class="space"></div>
                <div>
                  今天听银禾小姐唱歌了喵!
                </div>
                <div class="space"></div>
                <div>
                  银禾小姐唱歌好听喵!
                </div>
                <div class="space"></div>
                <div>
                  以后也要听银禾小姐唱歌喵!
                </div>
                <div class="space"></div>
                <div>
                  记在小本本上
                </div>

              </template>
              <template v-else-if="dayAfterLastMove < 7">

                <div>
                  {{ textToday }}
                </div>
                <div class="space"></div>
                <div>
                  今天是银禾小姐
                </div>
                <div class="tiny-space"></div>
                <div>
                  没给我们唱歌的
                </div>
                <div class="small-space"></div>
                <div class="tertiary-text">
                  第 {{ dayAfterLastMove }} 天
                </div>
                <div class="small-space"></div>
                <div>
                  猫好, 银禾坏
                </div>
                <div class="tiny-space"></div>
                <div>
                  记在小本本上喵
                </div>

              </template>
              <template v-else>

                <div>
                  {{ textToday }}
                </div>
                <div class="space"></div>
                <div class="tertiary-text">
                  已经 {{ dayAfterLastMove }} 天
                </div>
                <div class="space"></div>
                <div>
                  没听银禾小姐唱歌了喵
                </div>
                <div class="space"></div>
                <div>
                  想听银禾小姐唱歌喵
                </div>
                <div class="space"></div>
                <div>
                  记在小本本上喵
                </div>

              </template>

            </div>


            <div class="absolute right bottom">
              <div class="inkwell trans"></div>
            </div>
          </div>
        </div>
      </div>

    </div>

  </div>

  <div class="fixed right bottom right-align grey-text small-text tiny-line small-opacity">
    <a href="https://github.com/FirokOtaku/book-of-lumen" target="_blank">
      <div>
        <div>
          Book of Lumen v{{ version }} (2024-11-03)
        </div>
        <div>
          Design of Firok
        </div>
        <div>
          Made with love and driven by passion
        </div>
        <div>
          Open source on GitHub under Mulan PSL v2 license
        </div>
      </div>
    </a>
  </div>

</template>

<script setup>

import {version} from '../package.json'
import {ref, onBeforeMount, onBeforeUnmount, computed, shallowRef,} from 'vue'
import { debounce } from 'lib-opal/lib/util.js'

const displayState = ref('all')
const windowSize = shallowRef({ width: window.innerWidth, height: window.innerHeight })

const ps = computed(() => {
  const windowSizeValue = windowSize.value
  const displayStateValue = displayState.value

  const screenX = windowSizeValue.width, screenY = windowSizeValue.height // 屏幕尺寸
  const pageWidth = 136, pageHeight = 180, bookWidth = 272 // 原始显示区域尺寸
  const coreOriginWidth = displayStateValue === 'all' ? bookWidth : pageWidth, coreOriginHeight = pageHeight // 基础显示区域尺寸
  const baseScale = Math.min( screenX / coreOriginWidth, screenY / coreOriginHeight ).toFixed(2) // 基础显示缩放
  const baseWidth = coreOriginWidth * baseScale, baseHeight = coreOriginHeight * baseScale // 基础显示区域尺寸
  const baseMarginX = (screenX - baseWidth) / 2, baseMarginY = (screenY - baseHeight) / 2 // 基础显示区域边距

  const coreMargin = 60 // 核心显示区域边距
  const coreWidth = baseWidth - coreMargin, coreHeight = baseHeight - coreMargin // 核心显示区域尺寸
  const coreScale = coreWidth / coreOriginWidth // 核心显示缩放
  const coreOffsetX = displayStateValue === 'right' ? - coreScale * pageWidth : 0

  return {
    baseWidth, baseHeight,
    baseMarginX, baseMarginY,
    coreScale, coreWidth, coreHeight, coreOffsetX,
  }
})


function _onResizeInternal()
{
  windowSize.value = {
    width: window.innerWidth,
    height: window.innerHeight,
  }
}
function onResize()
{
  // 重新计算页面尺寸和布局
  debounce('window-resize', _onResizeInternal, 200)()
}

function clickBookPageRight(event)
{
  event.stopPropagation()
  displayState.value = displayState.value === 'right' ? 'all' : 'right'
}
function clickBookPageLeft(event)
{
  event.stopPropagation()
  displayState.value = displayState.value === 'left' ? 'all' : 'left'
}
function clickWhiteSpace()
{
  displayState.value = 'all'
}

// 暂时先这么写
const LumenMoves = [
  {
    date: '2024-10-31',
  },
]
const dayAfterLastMove = computed(() => {

  const lastMove = LumenMoves[LumenMoves.length - 1]
  const lastMoveDate = new Date(lastMove.date)
  const now = new Date()
  return Math.floor((now - lastMoveDate) / (1000 * 60 * 60 * 24)) // 计算从最后一次活动到今天之间的天数差
})

function padStart(num)
{
  return num.toString().padStart(2, '0')
}
const today = new Date()
const textToday = `${today.getFullYear()}-${padStart(today.getMonth() + 1)}-${padStart(today.getDate())}`

onBeforeMount(() => {
  window.addEventListener('resize', onResize)
})
onBeforeUnmount(() => {
  window.removeEventListener('resize', onResize)
})

</script>
