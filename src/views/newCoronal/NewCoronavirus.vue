<script lang="ts" name="newCoronavirus" setup>
import * as echarts from 'echarts'
import 'echarts-liquidfill'
import chinaJSON from '@/assets/json/china.json'
import { nextTick, onMounted, reactive, ref, watchEffect } from 'vue'

const theme = ref<string>('dark')
const screenWidth = ref<number>(0)

const cardData = reactive([
  { name: '现有确诊', icon: 'CaretBottom', num: -100, count: 3375, countColor: 'primary' },
  { name: '累计确诊', icon: 'CaretTop', num: 72, count: 110001 },
  { name: '境外输入', icon: 'CaretTop', num: 23, count: 7941 },
  { name: '无症状感染者', icon: 'CaretTop', num: 8, count: 534 },
  { name: '累计治愈', icon: 'CaretTop', num: 173, count: 102454 },
  { name: '累计死亡', icon: 'CaretTop', num: 12, count: 4892, countColor: 'danger' }
])

const rankingRef = ref()
const rankingBar = () => {
  const rankingName = ['湖北', '香港', '澳门', '广东', '上海', '北京', '安徽', '江西', '浙江', '江苏']
  const rankingValue = [239, 181, 154, 144, 135, 117, 74, 72, 67, 55]

  if (rankingRef.value === null) return
  echarts.dispose(rankingRef.value)

  const myChart = echarts.init(rankingRef.value, theme.value, { renderer: 'svg' })
  myChart.setOption({
    backgroundColor: 'transparent',
    grid: {
      top: 0,
      right: '2%',
      bottom: 0,
      left: '2%',
      containLabel: true
    },
    xAxis: {
      show: false,
      type: 'value'
    },
    yAxis: [
      {
        type: 'category',
        inverse: true,
        axisLabel: {
          color: '#fff',
        },
        splitLine: {
          show: false
        },
        axisTick: {
          show: false
        },
        axisLine: {
          show: false
        },
        data: rankingName
      },
      {
        type: 'category',
        inverse: true,
        axisTick: 'none',
        axisLine: 'none',
        show: true,
        axisLabel: {
          color: '#fff',
          fontSize: '12'
        },
        data: rankingValue
      }
    ],
    series: [
      {
        name: '累计排名',
        type: 'bar',
        showBackground: true,
        backgroundStyle: {
          color: '#1a1f45',
          borderRadius: 30
        },
        itemStyle: {
          borderRadius: 30,
          color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
            offset: 0,
            color: '#1094ff'
          }, {
            offset: 1,
            color: '#6c4dfc'
          }]),
        },
        barWidth: 16,
        data: rankingValue
      }
    ]
  })
}

const proportionRef = ref()
const proportionPie = (params: any) => {
  const placeHolderStyle = {
    label: { show: false },
    labelLine: { show: false },
    color: 'transparent',
    borderWidth: 0
  }
  const dataStyle = {
    position: 'center',
    fontSize: 16,
    color: '#fff'
  }

  if (proportionRef.value === null) return
  echarts.dispose(proportionRef.value)

  const myChart = echarts.init(proportionRef.value, theme.value, { renderer: 'svg' })
  myChart.setOption({
    backgroundColor: 'transparent',
    title: [
      {
        text: params.title1,
        left: '14%',
        top: '80%',
        textAlign: 'center',
        textStyle: {
          color: '#fff',
          fontSize: 14,
          textAlign: 'center'
        }
      },
      {
        text: params.title2,
        left: '49%',
        top: '80%',
        textAlign: 'center',
        textStyle: {
          color: '#fff',
          fontSize: 14,
          textAlign: 'center'
        }
      },
      {
        text: params.title3,
        left: '84%',
        top: '80%',
        textAlign: 'center',
        textStyle: {
          color: '#fff',
          fontSize: 14,
          textAlign: 'center'
        }
      }
    ],
    series: [
      {
        type: 'pie',
        emphasis: { scale: false },
        radius: ['75%', '95%'],
        center: ['18%', '50%'],
        startAngle: 225,
        labelLine: { show: false },
        label: { show: false },
        data: [
          {
            value: 100,
            itemStyle: { color: '#1a1f45', borderRadius: 30 },
          },
          {
            value: 35,
            itemStyle: placeHolderStyle,
          }
        ]
      },
      {
        type: 'pie',
        emphasis: { scale: false },
        radius: ['75%', '95%'],
        center: ['18%', '50%'],
        startAngle: 225,
        labelLine: { show: false },
        label: { ...dataStyle, formatter: `${params.value1}%` },
        data: [
          {
            value: params.value1,
            itemStyle: {
              borderRadius: 30,
              color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
                offset: 0,
                color: '#fb886b'
              }, {
                offset: 1,
                color: '#d6365c'
              }])
            },
          },
          {
            value: 135 - params.value1,
            itemStyle: placeHolderStyle,
          }
        ]
      },
      {
        type: 'pie',
        emphasis: { scale: false },
        radius: ['75%', '95%'],
        center: ['50%', '50%'],
        startAngle: 225,
        labelLine: { show: false },
        label: { show: false },
        data: [
          {
            value: 100,
            itemStyle: { color: '#1a1f45', borderRadius: 30 }
          },
          {
            value: 35,
            itemStyle: placeHolderStyle,
          }
        ]
      },
      {
        type: 'pie',
        emphasis: { scale: false },
        radius: ['75%', '95%'],
        center: ['50%', '50%'],
        startAngle: 225,
        labelLine: { show: false },
        label: { ...dataStyle, formatter: `${params.value2}%` },
        data: [
          {
            value: params.value2,
            itemStyle: {
              borderRadius: 30,
              color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
                offset: 0,
                color: '#fb886b'
              }, {
                offset: 1,
                color: '#d6365c'
              }])
            }
          },
          {
            value: 135 - params.value2,
            itemStyle: placeHolderStyle,
          }
        ]
      },
      {
        type: 'pie',
        emphasis: { scale: false },
        radius: ['75%', '95%'],
        center: ['82%', '50%'],
        startAngle: 225,
        labelLine: { show: false },
        label: { show: false },
        data: [
          {
            value: 100,
            itemStyle: { color: '#1a1f45', borderRadius: 30 }
          },
          {
            value: 35,
            itemStyle: placeHolderStyle,
          },
        ]
      },
      {
        type: 'pie',
        emphasis: { scale: false },
        radius: ['75%', '95%'],
        center: ['82%', '50%'],
        startAngle: 225,
        labelLine: { show: false },
        label: { ...dataStyle, formatter: `${params.value3}%` },
        data: [
          {
            value: params.value3,
            itemStyle: {
              borderRadius: 30,
              color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
                offset: 0,
                color: '#fb886b'
              }, {
                offset: 1,
                color: '#d6365c'
              }])
            }
          },
          {
            value: 135 - params.value3,
            itemStyle: placeHolderStyle,
          },
        ]
      },
    ]
  })
}

const cureRef = ref()
const cureBar = () => {
  const xData = ['07-06', '07-07', '07-08', '07-09', '07-10', '07-11', '07-12', '07-13', '07-14', '07-15']
  const cureValue = [68203, 103489, 100034, 104970, 131744, 130230, 234890, 329034, 204970, 231744,]
  const diagnoseValue = [119325, 130438, 148000, 131594, 174141, 231807, 331000, 491594, 334141, 451807]

  if (cureRef.value === null) return
  echarts.dispose(cureRef.value)

  const myChart = echarts.init(cureRef.value, theme.value, { renderer: 'svg' })
  myChart.setOption({
    backgroundColor: 'transparent',
    legend: {
      right: 0
    },
    grid: {
      top: '10%',
      right: 0,
      bottom: '2%',
      left: '2%',
      containLabel: true
    },
    xAxis: [
      {
        type: 'category',
        axisTick: { show: false },
        axisLine: { show: true, lineStyle: { color: '#203345' } },
        axisLabel: { interval: 0, rotate: 30 },
        data: xData,
      }
    ],
    yAxis: [
      {
        type: 'value',
        axisTick: { show: false },
        axisLine: { show: true, lineStyle: { color: '#203345' } },
        splitLine: { show: false }
      }
    ],
    series: [
      {
        name: '累计治愈',
        type: 'bar',
        itemStyle: {
          borderRadius: 30,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#7e81d9'
          }, {
            offset: 1,
            color: '#42aae9'
          }]),
        },
        barWidth: 14,
        data: cureValue
      },
      {
        name: '累计诊断',
        type: 'bar',
        barGap: '-100%',
        z: 1,
        itemStyle: {
          borderRadius: 30,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#43279d'
          }, {
            offset: 1,
            color: '#1c1e3e'
          }]),
        },
        barWidth: 14,
        data: diagnoseValue
      }
    ]
  })
}

const mapRef = ref()
const chinaMap = () => {
  const mapData = [
    { name: '北京', value: 652 },
    { name: '天津', value: 328 },
    { name: '上海', value: 3000 },
    { name: '重庆', value: 7432 },
    { name: '河北', value: 567 },
    { name: '河南', value: 1275 },
    { name: '云南', value: 679 },
    { name: '辽宁', value: 476 },
    { name: '黑龙江', value: 2000 },
    { name: '湖南', value: 1378 },
    { name: '安徽', value: 687 },
    { name: '山东', value: 561 },
    { name: '新疆', value: 800 },
    { name: '江苏', value: 792 },
    { name: '浙江', value: 590 },
    { name: '江西', value: 832 },
    { name: '湖北', value: 5000 },
    { name: '广西', value: 290 },
    { name: '甘肃', value: 132 },
    { name: '山西', value: 487 },
    { name: '内蒙古', value: 356 },
    { name: '陕西', value: 1287 },
    { name: '吉林', value: 789 },
    { name: '福建', value: 1284 },
    { name: '贵州', value: 103 },
    { name: '广东', value: 1000 },
    { name: '青海', value: 52 },
    { name: '西藏', value: 67 },
    { name: '四川', value: 1428 },
    { name: '宁夏', value: 562 },
    { name: '海南', value: 154 },
    { name: '台湾', value: 2349 },
    { name: '香港', value: 1389 },
    { name: '澳门', value: 1459 },
    { name: '南海诸岛', value: 781 },
  ]

  if (mapRef.value === null) return
  echarts.dispose(mapRef.value)

  const myChart = echarts.init(mapRef.value, theme.value, { renderer: 'svg' })
  echarts.registerMap('china', chinaJSON as any)
  myChart.setOption({
    backgroundColor: 'transparent',
    visualMap: {
      left: 'left',
      pieces: [
        { min: 1500, label: '>1500人', color: '#39026b' },
        { min: 500, max: 1500, label: '500-1500人', color: '#210068' },
        { min: 200, max: 500, label: '200-500人', color: '#131c93' },
        { min: 100, max: 200, label: '100-200人', color: '#1444a6' },
        { min: 0, max: 100, label: '<100人', color: '#1769c1' },
      ],
    },
    geo: {
      map: 'china',
      roam: true,
      zoom: 1.2,
      label: {
        show: true,
        color: '#ccc',
        fontSize: 12,
      },
      itemStyle: {
        areaColor: '#fbfbfb',
        borderColor: '#3399ff'
      },
      select: {
        label: {
          show: true,
          color: '#fff'
        },
        itemStyle: {
          areaColor: '#CC9933',
          shadowColor: 'rgba(51, 153, 255, .4)',
          shadowBlur: 16

        }
      },
      emphasis: {
        label: {
          show: true,
          color: '#fff'
        },
        itemStyle: {
          areaColor: '#CC9933',
          shadowColor: 'rgba(51, 153, 255, .4)',
          shadowBlur: 16
        }
      },
    },
    series: [
      {
        name: "全国确诊新冠肺炎病例",
        type: 'map',
        geoIndex: 0,
        // map: 'china',
        // roam: true,
        // zoom: 1.2,
        // label: {
        //   position: 'center',
        //   color: '#fff',
        //   fontSize: 14,
        // },
        // itemStyle: {
        //   borderColor: 'rgba(0, 0, 0, .2)',
        //   shadowColor: 'rgba(0, 0, 0, .3)',
        //   shadowBlur: 16
        // },
        // emphasis: {
        //   label: { 
        //     show: true,
        //     color: '#fff',
        //   },
        // },
        data: mapData
      }
    ]

  })
}

const cureDeathRef = ref()
const cureDeathLiquidFill = () => {
  const value = 0.9046
  const value1 = 0.0473

  const labelParams = {
    formatter: (params: any): string => {
      return `{ratio|${params.value * 100}%}\n{name|${params.seriesName}}`
    },
    rich: {
      name: {
        fontSize: '14px',
        color: '#fff',
        lineHeight: 30
      },
      ratio: {
        fontSize: '18px',
        color: '#fff',
        fontWeight: 600
      }
    }
  }

  if (cureDeathRef.value === null) return
  echarts.dispose(cureDeathRef.value)

  const myChart = echarts.init(cureDeathRef.value, theme.value, { renderer: 'svg' })
  myChart.setOption({
    backgroundColor: 'transparent',
    series: [
      {
        type: 'liquidFill',
        name: '治愈率',
        radius: '80%',
        center: ['25%', '50%'],
        data: [value, value],
        label: labelParams,
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#18cccb'
          }, {
            offset: 1,
            color: '#00003f'
          }]),
        },
        backgroundStyle: {
          borderWidth: 0,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#157eff'
          }, {
            offset: 1,
            color: '#00003f'
          }]),
        },
        outline: {
          borderDistance: 0,
          itemStyle: {
            borderWidth: 6,
            borderColor: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
              offset: 0,
              color: '#10152b'
            }, {
              offset: 1,
              color: '#30309a'
            }]),
          },
        },
      },
      {
        type: 'liquidFill',
        name: '死亡率',
        radius: '80%',
        center: ['75%', '45%'],
        color: [
          {
            type: 'linear',
            x: 0,
            y: 0,
            x2: 0,
            y2: 1,
            colorStops: [
              {
                offset: 0,
                color: '#2aa1e3',
              },
              {
                offset: 1,
                color: '#08bbc9',
              },
            ],
            globalCoord: false,
          },
        ],
        data: [value1, value1],
        label: labelParams,
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#18cccb'
          }, {
            offset: 1,
            color: '#00003f'
          }]),
        },
        backgroundStyle: {
          borderWidth: 0,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#157eff'
          }, {
            offset: 1,
            color: '#00003f'
          }]),
        },
        outline: {
          borderDistance: 0,
          itemStyle: {
            borderWidth: 6,
            borderColor: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
              offset: 0,
              color: '#10152b'
            }, {
              offset: 1,
              color: '#30309a'
            }]),
          },
        },
      },
    ],
  })
}

const addRef = ref()
const addLine = () => {

  if (addRef.value === null) return
  echarts.dispose(addRef.value)

  const myChart = echarts.init(addRef.value, theme.value, { renderer: 'svg' })
  myChart.setOption({
    backgroundColor: 'transparent',
    legend: {
      icon: 'roundRect',
      itemWidth: 16,
      itemHeight: 10,
      right: 0,
      textStyle: {
        fontSize: 12
      },
      data: ['现有确诊新增', '境外输入新增', '无症状感染者']
    },
    grid: {
      top: '12%',
      right: 0,
      bottom: '4%',
      left: '2%',
      containLabel: true
    },
    xAxis: [
      {
        type: 'category',
        boundaryGap: false,
        axisLine: { show: true, lineStyle: { color: '#666' } },
        axisLabel: { interval: 0, rotate: 30 },
        data: ['05-13', '05-20', '05-27', '06-03', '06-10', '06-17', '06-24', '07-01', '07-08', '07-15']
      }
    ],
    yAxis: [
      {
        type: 'value',
        splitLine: { show: false }
      }
    ],
    series: [
      {
        type: 'line',
        name: '现有确诊新增',
        showSymbol: false,
        smooth: true,
        lineStyle: { width: 2 },
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
            offset: 0,
            color: '#c34f27'
          }, {
            offset: 1,
            color: '#f1d443'
          }])
        },
        areaStyle: {
          opacity: 0.2,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#c34f27'
          }, {
            offset: 1,
            color: 'transparent'
          }])
        },
        emphasis: { focus: 'series' },
        data: [220, 202, 371, -234, -58, -375, 790, 332, 542, 389]
      },
      {
        type: 'line',
        name: '境外输入新增',
        showSymbol: false,
        smooth: true,
        lineStyle: { width: 2 },
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
            offset: 0,
            color: '#0072e3'
          }, {
            offset: 1,
            color: '#4001ba'
          }])
        },
        areaStyle: {
          opacity: 0.2,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#005bb6'
          }, {
            offset: 1,
            color: 'transparent'
          }])
        },
        emphasis: { focus: 'series' },
        data: [157, 173, 184, 193, 158, 250, 172, 162, 156, 178]
      },
      {
        type: 'line',
        name: '无症状感染者',
        showSymbol: false,
        smooth: true,
        lineStyle: { width: 2 },
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [{
            offset: 0,
            color: '#006aff'
          }, {
            offset: 1,
            color: '#01edfd'
          }])
        },
        areaStyle: {
          opacity: 0.2,
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [{
            offset: 0,
            color: '#409eff'
          }, {
            offset: 1,
            color: 'transparent'
          }])
        },
        emphasis: { focus: 'series' },
        data: [0, 0, 40, 12, 6, 8, 56, 0, 4, 2]
      },
    ]
  })
}

interface ColsProps {
  label: string,
  prop: string,
  [x: string]: any
}
interface TableProps {
  index?: number
  sf: string
  qz: number
  zy: number
  sw: number
  [x: string]: any
}

const cols: ColsProps[] = [
  { label: '排序', prop: 'index' },
  { label: '省份', prop: 'sf' },
  { label: '累计确诊', prop: 'qz' },
  { label: '累计治愈', prop: 'zy' },
  { label: '累计死亡', prop: 'sw' },
]

const tableData: TableProps[] = [
  { index: 1, sf: '湖北', qz: 68188, zy: 63648, sw: 4512 },
  { index: 2, sf: '台湾', qz: 15346, zy: 12083, sw: 759 },
  { index: 3, sf: '香港', qz: 11955, zy: 11669, sw: 212 },
  { index: 4, sf: '广东', qz: 2791, zy: 2721, sw: 8 },
  { index: 5, sf: '上海', qz: 2257, zy: 2185, sw: 7 },
  { index: 6, sf: '黑龙江', qz: 1612, zy: 1599, sw: 13 },
  { index: 7, sf: '浙江', qz: 1388, zy: 1334, sw: 1 },
  { index: 8, sf: '河南', qz: 1324, zy: 1291, sw: 22 },
  { index: 9, sf: '河北', qz: 1317, zy: 1310, sw: 7 },
  { index: 10, sf: '四川', qz: 1121, zy: 1076, sw: 3 },

]

onMounted(() => {
  window.onresize = () => {
    return (() => {
      screenWidth.value = window.innerWidth || document.documentElement.clientWidth || document.body.clientWidth
    })()
  }

  nextTick(() => {
    rankingBar()
    proportionPie({
      title1: '现有确诊',
      value1: 9,
      title2: '境外输入',
      value2: 24,
      title3: '累计治愈',
      value3: 89
    })

    cureBar()
    chinaMap()

    cureDeathLiquidFill()
    addLine()
  })
})

// 自动监听 `screenWidth.value` 的变化
watchEffect(() => {
  if (screenWidth.value) {
    rankingBar()
    proportionPie({
      title1: '现有确诊',
      value1: 9,
      title2: '境外输入',
      value2: 24,
      title3: '累计治愈',
      value3: 89
    })

    cureBar()
    chinaMap()

    cureDeathLiquidFill()
    addLine()
  }
})

</script>

<template>
  <div class="pages">

    <div class="header header_item aic grid_cols_item-24 grid_rows_item-1">
      <div class="header_infos grid_cols_item-6"></div>
      <h2 class="title header_title grid_cols_item-12 flex jcc aic">全国新冠肺炎疫情数据大屏</h2>
      <div class="header_time update_time grid_cols_item-6">
        <span class="desc flex jfe">此数据为真实数据，数据来源：腾讯新闻</span>
        <span class="desc flex jfe">更新时间：2021-07-15 20:39:11</span>
      </div>
    </div>

    <div class="ranking_item modules grid_cols_item-5 grid_rows_item-10">
      <h3 class="item_title">累计排名（TOP10）</h3>
      <div ref="rankingRef" id="ranking" style="width: 100%;height: calc(100% - 40px);" />
    </div>

    <ul class="count_list grid_cols_item-13 grid_rows_item-2">
      <li v-for="(item, index) in cardData" :key="index" class="count_num_item grid_cols_item-1 modules">
        <p class="info flex jcc aic" :class="item.num > 0 ? 'top' : 'bottom'">
          <span class="text">{{ item.name }}</span>
          <el-icon class="icon" :class="item.num > 0 ? 'top' : 'bottom'">
            <component :is="item.icon" />
          </el-icon>
          <span class="num"> {{ item.num }}</span>
        </p>
        <p class="count flex jcc aic" :class="item.countColor">
          <CountTo :endVal="item.count" />
        </p>
      </li>
    </ul>

    <div class="treatment_death_item modules grid_cols_item-6 grid_rows_item-5">
      <h3 class="item_title">治疗率和死亡率</h3>
      <div id="cureDeath" ref="cureDeathRef" style="width: 100%; height: calc(100% - 40px);" />
    </div>

    <div class="map_item modules_nobg grid_cols_item-13 grid_rows_item-21">
      <div id="map" ref="mapRef" style="width: 100%; height: 100%;" />
    </div>

    <div class="add_item modules grid_cols_item-6 grid_rows_item-8">
      <h3 class="item_title">新增趋势</h3>
      <div id="add" ref="addRef" style="width: 100%; height: calc(100% - 40px);" />
    </div>

    <div class="proportion_item modules grid_cols_item-5 grid_rows_item-4">
      <h3 class="item_title">占比</h3>
      <div id="proportion" ref="proportionRef" style="width: 100%;height: calc(100% - 40px);" />
    </div>

    <div class="provinces_count_treatment_item modules grid_cols_item-6 grid_rows_item-10">
      <h3 class="item_title">各省累计确诊</h3>
      <el-table class="modules_table" ref="tableRef" :data="tableData">
        <el-table-column v-for="item in cols" :key="item.prop" :label="item.label" :prop="item.prop"
          :width="item.width" />
      </el-table>
    </div>

    <div class="latest_week_cure modules grid_cols_item-5 grid_rows_item-9">
      <h3 class="item_title">最近一周累计治愈</h3>
      <div id="cure" ref="cureRef" style="width: 100%;height: calc(100% - 40px);" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.modules {
  background: #10152b;
  border-radius: 8px;
  padding: 8px 12px;

  &_nobg {
    background: transparent;
  }

  &_table {
    width: 100%;
    height: calc(100% - 40px);

    &.el-table {
      background: transparent !important;

      :deep(.el-table__inner-wrapper::before) {
        display: none;
      }

      :deep(.el-table__header) {
        width: 100% !important;
      }

      :deep(.el-table__body) {
        width: 100% !important
      }

      :deep(tr) {
        background: transparent !important;
      }

      :deep(td.el-table__cell) {
        border: none !important;
        background: transparent !important;

        .cell {
          color: #fff;
          line-height: unset;
        }
      }

      :deep(th.el-table__cell) {
        border: none !important;
        background: transparent !important;

        .cell {
          color: #fff;
          line-height: unset;
        }
      }
    }
  }

  .item_title {
    font-size: 18px;
    font-weight: 600;
    height: 30px;
    margin-bottom: 10px;
    color: #fff;
  }
}

.pages {
  width: 100%;
  height: 100%;

  display: grid;
  grid-template-rows: 50px repeat(23, 1fr);
  grid-template-columns: repeat(24, 1fr);
  gap: 1rem;
  grid-auto-flow: row dense;

  h2 {
    font-size: 30px;
    font-weight: 600;
  }

  span {
    font-size: 14px;
  }

  .header {
    display: grid;
    grid-template-columns: repeat(24, 1fr);
    gap: 1rem;
  }

  .count_list {
    display: grid;
    grid-template-columns: repeat(6, 1fr);
    gap: 10px;
  }

  .count_num_item {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;

    .info {
      .text {
        margin-right: 10px;
      }

      // #d22e5a #63f662 #348be2
      &.bottom {

        .icon,
        .num {
          color: #d22e5a;
        }
      }

      &.top {

        .icon,
        .num {
          color: #63f662;
        }
      }

      &.danger {

        .icon,
        .num {
          color: #d22e5a;
        }
      }
    }
  }

  .count {
    font-size: 2.3rem;
    font-weight: 700;
    color: #fff;

    &.primary {
      color: #409eff;
    }

    &.danger {
      color: #d22e5a
    }
  }

}
</style>
