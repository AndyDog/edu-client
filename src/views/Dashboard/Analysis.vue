<script setup lang="ts">
import PanelGroup from './components/PanelGroup.vue'
import { ElRow, ElCol, ElCard, ElSkeleton } from 'element-plus'
import { Echart } from '@/components/Echart'
import { pieOptions, barOptions, lineOptions } from './echarts-data'
import { ref, reactive } from 'vue'
import {
  getUserAccessSourceApi,
  getWeeklyUserActivityApi,
  getMonthlySalesApi
} from '@/api/dashboard/analysis'
import { set } from 'lodash-es'
import { EChartsOption } from 'echarts'
import { useI18n } from '@/hooks/web/useI18n'

const { t } = useI18n()

const loading = ref(true)

const pieOptionsData = reactive<EChartsOption>(pieOptions) as EChartsOption

// 用户来源
const getUserAccessSource = async () => {
  const res = await getUserAccessSourceApi().catch(() => {})
  if (res) {
    set(
      pieOptionsData,
      'legend.data',
      res.data.map((v) => t(v.name))
    )
    pieOptionsData!.series![0].data = res.data.map((v) => {
      return {
        name: t(v.name),
        value: v.value
      }
    })
  }
}

const barOptionsData = reactive<EChartsOption>(barOptions) as EChartsOption

// 周活跃量
const getWeeklyUserActivity = async () => {
  const res = await getWeeklyUserActivityApi().catch(() => {})
  if (res) {
    set(
      barOptionsData,
      'xAxis.data',
      res.data.map((v) => t(v.name))
    )
    set(barOptionsData, 'series', [
      {
        name: t('analysis.activeQuantity'),
        data: res.data.map((v) => v.value),
        type: 'bar'
      }
    ])
  }
}

const lineOptionsData = reactive<EChartsOption>(lineOptions) as EChartsOption

// 每月销售总额
const getMonthlySales = async () => {
  const res = await getMonthlySalesApi().catch(() => {})
  if (res) {
    set(
      lineOptionsData,
      'xAxis.data',
      res.data.map((v) => t(v.name))
    )
    set(lineOptionsData, 'series', [
      {
        name: t('analysis.estimate'),
        smooth: true,
        type: 'line',
        data: res.data.map((v) => v.estimate),
        animationDuration: 2800,
        animationEasing: 'cubicInOut'
      },
      {
        name: t('analysis.actual'),
        smooth: true,
        type: 'line',
        itemStyle: {},
        data: res.data.map((v) => v.actual),
        animationDuration: 2800,
        animationEasing: 'quadraticOut'
      }
    ])
  }
}

const getAllApi = async () => {
  await Promise.all([getUserAccessSource(), getWeeklyUserActivity(), getMonthlySales()])
  loading.value = false
}

getAllApi()
</script>

<template>
  <div class="banner">
    <img src="../../assets/imgs/banner.png" alt="" />
  </div>
  <div>
    <div data-v-70d7b318="" class="section-part notice-part bg-white"
      ><div data-v-70d7b318="" class="tabs-info-banner"
        ><div
          data-v-70d7b318=""
          id="demoPlayer"
          class="xgplayer xgplayer-pc xgplayer-skin-default xgplayer-playing xgplayer-volume-muted xgplayer-inactive"
          style="width: 100%; height: 100%"
          ><video
            class=""
            autoplay=""
            playsinline="true"
            webkit-playsinline="true"
            x5-playsinline="true"
            x5-video-player-type="h5-page"
            tabindex="2"
            mediatype="video"
            loop="loop"
            src="http://vod.jj-edu.cn/promotional.mp4"
          ></video
          ><xg-controls class="xgplayer-controls" unselectable="on" onselectstart="return false"
            ><xg-placeholder class="xgplayer-placeholder" /><xg-progress
              class="xgplayer-progress"
              tabindex="1"
              ><xg-outer class="xgplayer-progress-outer">
                <xg-cache class="xgplayer-progress-cache" style="width: 100%" />
                <xg-played class="xgplayer-progress-played" style="width: 31.4577%">
                  <xg-progress-btn class="xgplayer-progress-btn" />
                  <xg-point class="xgplayer-progress-point xgplayer-tips" />
                  <xg-thumbnail class="xgplayer-progress-thumbnail xgplayer-tips" />
                </xg-played> </xg-outer></xg-progress
            ><xg-play class="xgplayer-play"
              ><xg-icon class="xgplayer-icon">
                <div class="xgplayer-icon-play"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="40"
                    height="40"
                    viewBox="0 0 40 40"
                  >
                    <path
                      transform="translate(2,2) scale(0.0320625 0.0320625)"
                      d="M576,363L810,512L576,661zM342,214L576,363L576,661L342,810z"
                    />
                  </svg>
                </div>
                <div class="xgplayer-icon-pause"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="40"
                    height="40"
                    viewBox="0 0 40 40"
                  >
                    <path
                      transform="translate(2,2) scale(0.0320625 0.0320625)"
                      d="M598,214h170v596h-170v-596zM256 810v-596h170v596h-170z"
                    />
                  </svg>
                </div> </xg-icon
              ><xg-tips class="xgplayer-tips"
                ><span class="xgplayer-tip-play">播放</span>
                <span class="xgplayer-tip-pause">暂停</span></xg-tips
              ></xg-play
            ><xg-fullscreen class="xgplayer-fullscreen"
              ><xg-icon class="xgplayer-icon">
                <div class="xgplayer-icon-requestfull"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="40"
                    height="40"
                    viewBox="0 0 40 40"
                  >
                    <path
                      transform="scale(0.0320625 0.0320625)"
                      d="M598 214h212v212h-84v-128h-128v-84zM726 726v-128h84v212h-212v-84h128zM214 426v-212h212v84h-128v128h-84zM298 598v128h128v84h-212v-212h84z"
                    />
                  </svg>
                </div>
                <div class="xgplayer-icon-exitfull"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="40"
                    height="40"
                    viewBox="0 0 40 40"
                  >
                    <path
                      transform="scale(0.0320625 0.0320625)"
                      d="M682 342h128v84h-212v-212h84v128zM598 810v-212h212v84h-128v128h-84zM342 342v-128h84v212h-212v-84h128zM214 682v-84h212v212h-84v-128h-128z"
                    />
                  </svg>
                </div> </xg-icon
              ><xg-tips class="xgplayer-tips"
                ><span class="xgplayer-tip-requestfull">进入全屏</span>
                <span class="xgplayer-tip-exitfull">退出全屏</span></xg-tips
              ></xg-fullscreen
            ><xg-volume class="xgplayer-volume"
              ><xg-icon class="xgplayer-icon">
                <div class="xgplayer-icon-large"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="28"
                    height="28"
                    viewBox="0 0 28 28"
                  >
                    <path
                      transform="scale(0.0220625 0.0220625)"
                      d="M358.4 358.4h-204.8v307.2h204.8l256 256v-819.2l-256 256z"
                    />
                    <path
                      transform="scale(0.0220625 0.0220625)"
                      d="M940.632 837.632l-72.192-72.192c65.114-64.745 105.412-154.386 105.412-253.44s-40.299-188.695-105.396-253.424l-0.016-0.016 72.192-72.192c83.639 83.197 135.401 198.37 135.401 325.632s-51.762 242.434-135.381 325.612l-0.020 0.020zM795.648 693.248l-72.704-72.704c27.756-27.789 44.921-66.162 44.921-108.544s-17.165-80.755-44.922-108.546l0.002 0.002 72.704-72.704c46.713 46.235 75.639 110.363 75.639 181.248s-28.926 135.013-75.617 181.227l-0.021 0.021z"
                    />
                  </svg>
                </div>
                <div class="xgplayer-icon-small"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="28"
                    height="28"
                    viewBox="0 0 28 28"
                  >
                    <path
                      transform="scale(0.0220625 0.0220625)"
                      d="M358.4 358.4h-204.8v307.2h204.8l256 256v-819.2l-256 256z"
                    />
                    <path
                      transform="scale(0.0220625 0.0220625)"
                      d="M795.648 693.248l-72.704-72.704c27.756-27.789 44.921-66.162 44.921-108.544s-17.165-80.755-44.922-108.546l0.002 0.002 72.704-72.704c46.713 46.235 75.639 110.363 75.639 181.248s-28.926 135.013-75.617 181.227l-0.021 0.021zM795.648 693.248l-72.704-72.704c27.756-27.789 44.921-66.162 44.921-108.544s-17.165-80.755-44.922-108.546l0.002 0.002 72.704-72.704c46.713 46.235 75.639 110.363 75.639 181.248s-28.926 135.013-75.617 181.227l-0.021 0.021z"
                    />
                  </svg>
                </div>
                <div class="xgplayer-icon-muted"
                  ><svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="28"
                    height="28"
                    viewBox="0 0 28 28"
                  >
                    <path
                      transform="scale(0.0220625 0.0220625)"
                      d="M358.4 358.4h-204.8v307.2h204.8l256 256v-819.2l-256 256z"
                    />
                    <path
                      transform="scale(0.0220625 0.0220625)"
                      d="M920.4 439.808l-108.544-109.056-72.704 72.704 109.568 108.544-109.056 108.544 72.704 72.704 108.032-109.568 108.544 109.056 72.704-72.704-109.568-108.032 109.056-108.544-72.704-72.704-108.032 109.568z"
                    />
                  </svg>
                </div>
              </xg-icon>
              <xg-slider class="xgplayer-slider" tabindex="2">
                <xg-bar class="xgplayer-bar">
                  <xg-drag class="xgplayer-drag" style="height: 0px" />
                </xg-bar> </xg-slider></xg-volume
            ><xg-time class="xgplayer-time"
              ><span class="xgplayer-time-current">00:04</span><span>00:13</span></xg-time
            ><xg-playbackrate class="xgplayer-playbackrate"
              ><ul
                ><li cname="2" class="">2x</li
                ><li cname="1.5" class="">1.5x</li
                ><li cname="1" class="selected">1x</li
                ><li cname="0.5" class="">0.5x</li></ul
              ><p class="name">1x</p></xg-playbackrate
            ><xg-pip class="xgplayer-pip" tabindex="9"
              ><p class="name"><span>画中画</span></p></xg-pip
            ></xg-controls
          ><xg-enter class="xgplayer-enter"
            ><div class="xgplayer-enter-spinner">
              <div class="xgplayer-enter-bar1"></div><div class="xgplayer-enter-bar2"></div
              ><div class="xgplayer-enter-bar3"></div><div class="xgplayer-enter-bar4"></div
              ><div class="xgplayer-enter-bar5"></div><div class="xgplayer-enter-bar6"></div
              ><div class="xgplayer-enter-bar7"></div><div class="xgplayer-enter-bar8"></div
              ><div class="xgplayer-enter-bar9"></div><div class="xgplayer-enter-bar10"></div
              ><div class="xgplayer-enter-bar11"></div
              ><div class="xgplayer-enter-bar12"></div> </div></xg-enter
          ><xg-poster
            class="xgplayer-poster"
            style="
              background-image: url('https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png');
            "
          /><xg-start class="xgplayer-start"
            ><div class="xgplayer-icon-play"
              ><svg xmlns="http://www.w3.org/2000/svg" width="70" height="70" viewBox="0 0 70 70">
                <path
                  transform="translate(15,15) scale(0.04,0.04)"
                  d="M576,363L810,512L576,661zM342,214L576,363L576,661L342,810z"
                />
              </svg>
            </div>
            <div class="xgplayer-icon-pause"
              ><svg xmlns="http://www.w3.org/2000/svg" width="70" height="70" viewBox="0 0 70 70">
                <path
                  transform="translate(15,15) scale(0.04 0.04)"
                  d="M598,214h170v596h-170v-596zM256 810v-596h170v596h-170z"
                />
              </svg> </div></xg-start
          ><xg-loading class="xgplayer-loading"
            ><svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100">
              <path d="M100,50A50,50,0,1,1,50,0" />
            </svg> </xg-loading
          ><xg-replay class="xgplayer-replay"
            ><svg
              class="xgplayer-replay-svg"
              xmlns="http://www.w3.org/2000/svg"
              width="78"
              height="78"
              viewBox="0 0 78 78"
            >
              <path
                d="M8.22708362,13.8757234 L11.2677371,12.6472196 C11.7798067,12.4403301 12.3626381,12.6877273 12.5695276,13.1997969 L12.9441342,14.1269807 C13.1510237,14.6390502 12.9036264,15.2218816 12.3915569,15.4287712 L6.8284538,17.6764107 L5.90126995,18.0510173 C5.38920044,18.2579068 4.80636901,18.0105096 4.5994795,17.49844 L1.97723335,11.0081531 C1.77034384,10.4960836 2.0177411,9.91325213 2.52981061,9.70636262 L3.45699446,9.33175602 C3.96906396,9.12486652 4.5518954,9.37226378 4.75878491,9.88433329 L5.67885163,12.1615783 C7.99551726,6.6766934 13.3983951,3 19.5,3 C27.7842712,3 34.5,9.71572875 34.5,18 C34.5,26.2842712 27.7842712,33 19.5,33 C15.4573596,33 11.6658607,31.3912946 8.87004692,28.5831991 C8.28554571,27.9961303 8.28762719,27.0463851 8.87469603,26.4618839 C9.46176488,25.8773827 10.4115101,25.8794641 10.9960113,26.466533 C13.2344327,28.7147875 16.263503,30 19.5,30 C26.127417,30 31.5,24.627417 31.5,18 C31.5,11.372583 26.127417,6 19.5,6 C14.4183772,6 9.94214483,9.18783811 8.22708362,13.8757234 Z"
              />
            </svg>

            <xg-replay-txt class="xgplayer-replay-txt">重播</xg-replay-txt> </xg-replay
          ><xg-error class="xgplayer-error"
            ><span class="xgplayer-error-text"
              >请<span class="xgplayer-error-refresh">刷新</span>试试</span
            ></xg-error
          ></div
        ></div
      ><div data-v-70d7b318="" class="default-shadow tabs-card"
        ><ul data-v-70d7b318="" class="my-tabs el-menu-demo"
          ><li data-v-70d7b318="" class="active"><span data-v-70d7b318=""> 资讯动态 </span></li
          ><li data-v-70d7b318="" class=""><span data-v-70d7b318=""> 培训通知 </span></li
          ><li data-v-70d7b318="" class=""><span data-v-70d7b318=""> 政策文件 </span></li
          ><li data-v-70d7b318="" class="pc-more"
            ><a
              data-v-70d7b318=""
              href="/static/openAnnouncementList?activeName=%E8%B5%84%E8%AE%AF%E5%8A%A8%E6%80%81"
              class="more"
              >更多&gt;&gt;</a
            ></li
          ></ul
        ><ul data-v-70d7b318="" class="tabs-text-box" style=""
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title">
              川渝浙省域急救教育实施启动会在北京外国语大学成功召开 </p
            ><span data-v-70d7b318="">12-23</span></li
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title"
              ><a data-v-70d7b318="" href="javascript:;"
                >国家卫生健康委关于印发《突发事件医疗应急工作管理办法（试行）》的通知</a
              ></p
            ><span data-v-70d7b318="">12-13</span></li
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title"
              ><a data-v-70d7b318="" href="javascript:;"
                >教育部 国家统计局 财政部 关于2022年全国教育经费执行情况统计公告</a
              ></p
            ><span data-v-70d7b318="">12-05</span></li
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title"
              ><a data-v-70d7b318="" href="javascript:;">河南郑州：将心肺复苏纳入中考体育项目</a></p
            ><span data-v-70d7b318="">11-29</span></li
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title"
              ><a data-v-70d7b318="" href="javascript:;"
                >财政部关于印发《中央财政县级基本财力保障机制奖补资金管理办法》的通知</a
              ></p
            ><span data-v-70d7b318="">11-27</span></li
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title"
              ><a data-v-70d7b318="" href="javascript:;">深圳：首批500辆出租车配备AED</a></p
            ><span data-v-70d7b318="">11-27</span></li
          ><li data-v-70d7b318=""
            ><p data-v-70d7b318="" class="title"
              ><a data-v-70d7b318="" href="javascript:;">北京市重点公共场所AED电子地图发布</a></p
            ><span data-v-70d7b318="">11-22</span></li
          ></ul
        ><div data-v-70d7b318="" class="tabs-text-box" style="display: none"
          ><div data-v-70d7b318="" class="el-empty"
            ><div class="el-empty__image" style="width: 80px"
              ><svg
                viewBox="0 0 79 86"
                version="1.1"
                xmlns="http://www.w3.org/2000/svg"
                xmlns:xlink="http://www.w3.org/1999/xlink"
              >
                <defs>
                  <linearGradient
                    id="linearGradient-1-24"
                    x1="38.8503086%"
                    y1="0%"
                    x2="61.1496914%"
                    y2="100%"
                  >
                    <stop stop-color="#FCFCFD" offset="0%" />
                    <stop stop-color="#EEEFF3" offset="100%" />
                  </linearGradient>
                  <linearGradient id="linearGradient-2-24" x1="0%" y1="9.5%" x2="100%" y2="90.5%">
                    <stop stop-color="#FCFCFD" offset="0%" />
                    <stop stop-color="#E9EBEF" offset="100%" />
                  </linearGradient>
                  <rect id="path-3-24" x="0" y="0" width="17" height="36" />
                </defs>
                <g
                  id="Illustrations"
                  stroke="none"
                  stroke-width="1"
                  fill="none"
                  fill-rule="evenodd"
                >
                  <g id="B-type" transform="translate(-1268.000000, -535.000000)">
                    <g id="Group-2" transform="translate(1268.000000, 535.000000)">
                      <path
                        id="Oval-Copy-2"
                        d="M39.5,86 C61.3152476,86 79,83.9106622 79,81.3333333 C79,78.7560045 57.3152476,78 35.5,78 C13.6847524,78 0,78.7560045 0,81.3333333 C0,83.9106622 17.6847524,86 39.5,86 Z"
                        fill="#F7F8FC"
                      />
                      <polygon
                        id="Rectangle-Copy-14"
                        fill="#E5E7E9"
                        transform="translate(27.500000, 51.500000) scale(1, -1) translate(-27.500000, -51.500000) "
                        points="13 58 53 58 42 45 2 45"
                      />
                      <g
                        id="Group-Copy"
                        transform="translate(34.500000, 31.500000) scale(-1, 1) rotate(-25.000000) translate(-34.500000, -31.500000) translate(7.000000, 10.000000)"
                      >
                        <polygon
                          id="Rectangle-Copy-10"
                          fill="#E5E7E9"
                          transform="translate(11.500000, 5.000000) scale(1, -1) translate(-11.500000, -5.000000) "
                          points="2.84078316e-14 3 18 3 23 7 5 7"
                        />
                        <polygon
                          id="Rectangle-Copy-11"
                          fill="#EDEEF2"
                          points="-3.69149156e-15 7 38 7 38 43 -3.69149156e-15 43"
                        />
                        <rect
                          id="Rectangle-Copy-12"
                          fill="url(#linearGradient-1-24)"
                          transform="translate(46.500000, 25.000000) scale(-1, 1) translate(-46.500000, -25.000000) "
                          x="38"
                          y="7"
                          width="17"
                          height="36"
                        />
                        <polygon
                          id="Rectangle-Copy-13"
                          fill="#F8F9FB"
                          transform="translate(39.500000, 3.500000) scale(-1, 1) translate(-39.500000, -3.500000) "
                          points="24 7 41 7 55 -3.63806207e-12 38 -3.63806207e-12"
                        />
                      </g>
                      <rect
                        id="Rectangle-Copy-15"
                        fill="url(#linearGradient-2-24)"
                        x="13"
                        y="45"
                        width="40"
                        height="36"
                      />
                      <g id="Rectangle-Copy-17" transform="translate(53.000000, 45.000000)">
                        <mask id="mask-4-24" fill="white"><use xlink:href="#path-3-24" /></mask>
                        <use
                          id="Mask"
                          fill="#E0E3E9"
                          transform="translate(8.500000, 18.000000) scale(-1, 1) translate(-8.500000, -18.000000) "
                          xlink:href="#path-3-24"
                        />
                        <polygon
                          id="Rectangle-Copy"
                          fill="#D5D7DE"
                          mask="url(#mask-4-24)"
                          transform="translate(12.000000, 9.000000) scale(-1, 1) translate(-12.000000, -9.000000) "
                          points="7 0 24 0 20 18 -1.70530257e-13 16"
                        />
                      </g>
                      <polygon
                        id="Rectangle-Copy-18"
                        fill="#F8F9FB"
                        transform="translate(66.000000, 51.500000) scale(-1, 1) translate(-66.000000, -51.500000) "
                        points="62 45 79 45 70 58 53 58"
                      />
                    </g>
                  </g>
                </g></svg></div
            ><div class="el-empty__description"><p>暂无数据</p></div
            ><!----></div
          ></div
        ><div data-v-70d7b318="" class="mobile-more"
          ><a
            data-v-70d7b318=""
            href="/static/openAnnouncementList?activeName=%E8%B5%84%E8%AE%AF%E5%8A%A8%E6%80%81"
            class=""
            >更多&gt;&gt;</a
          ></div
        ></div
      ></div
    >
  </div>
  <!-- <PanelGroup />
  <ElRow :gutter="20" justify="space-between">
    <ElCol :xl="10" :lg="10" :md="24" :sm="24" :xs="24">
      <ElCard shadow="hover" class="mb-20px">
        <ElSkeleton :loading="loading" animated>
          <Echart :options="pieOptionsData" :height="300" />
        </ElSkeleton>
      </ElCard>
    </ElCol>
    <ElCol :xl="14" :lg="14" :md="24" :sm="24" :xs="24">
      <ElCard shadow="hover" class="mb-20px">
        <ElSkeleton :loading="loading" animated>
          <Echart :options="barOptionsData" :height="300" />
        </ElSkeleton>
      </ElCard>
    </ElCol>
    <ElCol :span="24">
      <ElCard shadow="hover" class="mb-20px">
        <ElSkeleton :loading="loading" animated :rows="4">
          <Echart :options="lineOptionsData" :height="350" />
        </ElSkeleton>
      </ElCard>
    </ElCol>
  </ElRow> -->
</template>

<style lang="less" scoped>
.banner {
  img {
    object-fit: contain;
    width: 100%;
  }
}
</style>
