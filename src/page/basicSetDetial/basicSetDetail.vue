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
        this.getSetDetail()
      })
    },
    // TODO: 获取基础样本集的信息包括：名称（name, desc, time, classDis(类别分布), tileUrl(矢量瓦片地址).
    getSetDetail () {
      this.setInfo.classDis = sampleDistributionOption
      let url = this.$api.sample + '/sp/basic/sampleset/' + this.setId
      this.$http.get(url).then((response) => {
        if (response && response.status === 200) {
          let rawData = response.data.data
          if (rawData) {
            this.setInfo.name = rawData.name
            // this.setInfo.desc = rawData.desc
            this.setInfo.vectorTile = rawData.vectorTile
            if (rawData.classTypeGroupSum) {
              this.setInfo.classDis.yAxis.data = rawData.classTypeGroupSum.map((o) => beautySub(o.type, 6))
              this.setInfo.classDis.series.data = rawData.classTypeGroupSum.map((o) => o.count)
            }
            // this.setInfo.time =
            this.initChart()
            this.renderVectorTile(rawData)
            // this.initMap()
          }
        } else {
          this.$notify.error({ title: '错误', message: response.message })
        }
      }).catch((response) => {
        console.log(response)
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
      let map = new mapboxgl.Map({
        container: container,
        style: {
          version: 8,
          sources: {},
          layers: []
        },
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

        // map.addSource('street-vec', {
        //   type: 'raster',
        //   tiles: [
        //     'http://t0.tianditu.com/vec_w/wmts?tk=ade4530538c006d4b4a3ac8b9138499f&SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=vec&STYLE=default&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&FORMAT=tiles'
        //   ],
        //   'minzoom': 0,
        //   'maxzoom': 18,
        //   tileSize: 256
        // })
        //
        // map.addLayer({
        //   id: 'street-vec',
        //   type: 'raster',
        //   source: 'street-vec'
        // })
        //
        // map.addSource('street-cva', {
        //   type: 'raster',
        //   tiles: [
        //     'http://t0.tianditu.com/cva_w/wmts?tk=ade4530538c006d4b4a3ac8b9138499f&SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cva&STYLE=default&TILEMATRIXSET=w&TILEMATRIX={z}&TILEROW={y}&TILECOL={x}&FORMAT=tiles'
        //   ],
        //   'minzoom': 0,
        //   'maxzoom': 18,
        //   tileSize: 256
        // })
        // map.addLayer({
        //   id: 'street-cva',
        //   type: 'raster',
        //   source: 'street-cva'
        // })
        map.addLayer({
          id: 'earth',
          type: 'raster',
          source: 'earth'
        })
      })
    },
    renderVectorTile (basicSet) {
      let map = this.map

      function addLayer () {
        if (basicSet.bounds) {
          let bounds = JSON.parse(basicSet.bounds)
          map.fitBounds(bounds)
        }
        let source = 'source-' + basicSet.name
        let layer = 'dataset-vector' + basicSet.name
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
      }

      addLayer()
      // if (map.loaded()) {
      //   addLayer()
      // } else {
      //   map.on('load', () => {
      //     addLayer()
      //   })
      // }
    },
    initMap () {
      const _this = this
      try {
        _this.map = L.map('map', {
          // -86.21315,32.392138
          center: [32.392138, -86.21315],
          zoom: 15,
          zoomControl: false
        })
        let osmUrl = 'http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png'
        let osm = new L.TileLayer(osmUrl, { minZoom: 5, maxZoom: 18 })
        _this.map.addLayer(osm)
        let url = this.setInfo.vectorTile
        console.log(url)
        // L.esri.basemapLayer('Imagery').addTo(this.map) // 定义basemapLayer并将其加载到地图容器中
        L.vectorGrid.protobuf(url).addTo(_this.map)
      } catch (e) {
        console.log(e)
      }
    },
    back () {
      this.$router.push({ name: 'basicSet' })
    }
  },
  data () {
    return {
      setId: false,
      setInfo: {
        name: '',
        desc: '本样本集创建于2021-01-03，包含美国加州所有地物的信息，地物信息采集于2018年',
        time: '2018/01/03 -- 2019/01/01',
        classDis: false
      },
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
