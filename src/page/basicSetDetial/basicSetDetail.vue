<template>
  <div class="basic-detail query sample animated fadeIn">
    <div class="query-bar">
      <AppSidebar fixed style="color: #000;">
        <div class="tool-expand">
          <button class="sidebar-minimizer" type="button" @click="toggleLeftBar()"></button>
        </div>
        <nav class="sidebar-nav">
          <VuePerfectScrollbar class="scroll-area" :settings="psSettings">
            <div class="title-2">数据集名称</div>
            <div class="retract-text large-text regular-vertical-padding">{{ setInfo.name }}</div>
            <div class="title-2">数据集描述</div>
            <div class="retract-text large-text regular-vertical-padding">{{ setInfo.desc }}</div>
            <div class="title-2">数据采集时间</div>
            <div class="retract-text large-text regular-vertical-padding bright-blue-text">{{ setInfo.time }}</div>
            <div class="title-2">分类体系统计</div>
            <div class="chart" id="chart"></div>
            <el-button type="primary" @click="back">返回</el-button>
          </VuePerfectScrollbar>
        </nav>
      </AppSidebar>
    </div>
    <b-row>
      <div class="map" id='map' style="height: calc(100vh - 101px);"></div>
    </b-row>
    <div style="position: fixed; bottom: 20px; right: 20px; display: flex; flex-direction: column; align-items: center">
      <img @click="switchBase(0)" style="width: 80px; height: 80px;" src="../../assets/img/satellite.jpg" >
      <el-checkbox v-model="showSatellite" @change="switchBase(1)">显示遥感影像</el-checkbox>
    </div>
  </div>
</template>
<script>
import { Sidebar as AppSidebar } from '@coreui/vue'
import VuePerfectScrollbar from 'vue-perfect-scrollbar'
import 'leaflet.pm'
import { sampleDistributionOption } from '../sampleData'
import echarts from 'echarts'
import 'leaflet.vectorgrid/dist/Leaflet.VectorGrid.bundled.js'
import { beautySub } from 'src/utils/factory'
//
import mapboxgl from 'mapbox-gl'
import 'mapbox-gl/dist/mapbox-gl.css'

const mapboxToken = 'pk.eyJ1IjoidnY1NDU0NTQiLCJhIjoiY2o4NDZwcHY2MDV6MzMzczV5eTBtbnZybyJ9.LhlZtGKozugZK7_bWSKgOQ'
// const mapboxgl = window.mapboxgl
mapboxgl.accessToken = mapboxToken
const L = window.L

export default {
  name: 'basicSetDetail',
  components: {
    AppSidebar,
    VuePerfectScrollbar
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      this.setId = this.$route.params.id
      this.$nextTick(() => {
        this.initMapBoxMap()
      })
    },
    // TODO: 获取基础样本集的信息包括：名称（name, desc, time, classDis(类别分布), tileUrl(矢量瓦片地址).
    getSetDetail () {
      this.setInfo.classDis = sampleDistributionOption
      let url = this.$api.sample + '/sp/basic/sampleset/' + this.setId
      let _this = this
      this.$http.get(url).then((response) => {
        if (response && response.status === 200) {
          let rawData = response.data.data
          if (rawData) {
            _this.setInfo.name = rawData.name
            // this.setInfo.desc = rawData.desc
            _this.setInfo.vectorTile = rawData.vectorTile
            if (rawData.classTypeGroupSum) {
              _this.setInfo.classDis.yAxis.data = rawData.classTypeGroupSum.map((o) => beautySub(o.type, 6))
              _this.setInfo.classDis.series[0].data = rawData.classTypeGroupSum.map((o) => o.count)
              _this.initChart()
            }
            _this.renderVectorTile(rawData)
          }
        } else {
          this.$notify.error({ title: '错误', message: response.message })
        }
      }).catch((response) => {
        // console.log(response)
      })
    },
    // TODO: 加载后台返回的矢量瓦片地址
    renderTileUrl () {
    },
    initChart () {
      let myChart = echarts.init(document.getElementById('chart'))
      myChart.setOption(this.setInfo.classDis)
    },
    initMapBoxMap () {
      let container = 'map'
      mapboxgl.accessToken = 'pk.eyJ1Ijoibmlld3poIiwiYSI6ImNqbjRvM2F4ODA5ZDEzd2xkd2oxZnB4Y2UifQ.phMvmLw9t9lDxobKyYVbPw'
      let map = new mapboxgl.Map({
        container: container,
        style: 'mapbox://styles/mapbox/dark-v10',
        center: [-86.21315, 32.392138],
        zoom: 10
      })
      this.map = map
      map.on('load', () => {
        map.addSource('earth', {
          type: 'raster',
          tiles: [
            'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}'
          ],
          'minzoom': 2,
          'maxzoom': 18,
          tileSize: 256
        })
        map.addLayer({
          id: 'earth',
          type: 'raster',
          source: 'earth'
        })
        this.map.setLayoutProperty('earth', 'visibility', 'none')
        this.getSetDetail()
      })
    },
    renderVectorTile (basicSet) {
      let map = this.map
      if (basicSet.bounds) {
        let bounds = JSON.parse(basicSet.bounds)
        map.fitBounds(bounds)
      }
      let source = 'source-' + basicSet.name
      let layer = 'dataset-vector' + basicSet.name
      this.vectorLayerId = layer
      map.addSource(source, {
        'type': 'vector',
        // url: 'mapbox://examples.8fgz4egr',
        'tiles': [
          basicSet.vectorTile
        ],
        'minzoom': 1,
        'maxzoom': 18
      })
      // 添加主图层
      map.addLayer(
        {
          'id': layer,
          'type': 'fill',
          'source': 'source-' + basicSet.name,
          'source-layer': basicSet.layer,
          'paint': {
            'fill-color': '#b13939',
            'fill-opacity': 1
          }
        }
      )
      // console.log('aaa')
    },
    switchBase (flag) {
      if (!flag) {
        this.showSatellite = !this.showSatellite
      }
      if (this.showSatellite) {
        // 用户选择看地图的时候，显示地图，隐藏黑图
        this.map.setLayoutProperty('earth', 'visibility', 'visible')
        // this.map.setLayoutProperty('black', 'visibility', 'none')
        this.map.setPaintProperty(this.vectorLayerId, 'fill-opacity', 0.4)
        // this.map.setPaintProperty(this.vectorLayerId, 'line-color', '#b13939')
        // 分层设色，看看行不行得通
        // this.map.setPaintProperty(this.vectorLayerId, 'line-color', ["get", 'color'])
      } else {
        // 用户选择看地图的时候，显示黑图，隐藏底图
        // this.map.setLayoutProperty('black', 'visibility', 'visible')
        this.map.setLayoutProperty('earth', 'visibility', 'none')
        this.map.setPaintProperty(this.vectorLayerId, 'fill-opacity', 1)
        // this.map.setPaintProperty(this.vectorLayerId, 'fill-opacity', ['get', 'color'])
        // 分层设色，看看行不行得通
        // this.map.setPaintProperty(this.vectorLayerId, 'line-color', ['get', 'color'])
      }
    },
    back () {
      this.$router.push({ name: 'basicSet' })
    }
  },
  data () {
    return {
      // 用来修改矢量图层的样式
      vectorLayerId: '',
      setId: false,
      setInfo: {
        name: '',
        desc: '本样本集创建于2021-01-03，包含美国加州所有地物的信息，地物信息采集于2018年',
        time: '2018/01/03 -- 2019/01/01',
        classDis: false
      },
      showSatellite: false,
      psSettings: {
        maxScrollbarLength: 200,
        minScrollbarLength: 40,
        suppressScrollX: true,
        wheelPropagation: false,
        interceptRailY: styles => ({ ...styles, height: 0 })
      }
    }
  }
}
</script>
<style>
@import "../../../node_modules/leaflet.pm/dist/leaflet.pm.css";
</style>
<style lang="scss">
@import "../sampleManager";
@import "../sampleSetManager/sampleSetManager";
@import "basicSetDetail";
@import "../layout";
</style>
