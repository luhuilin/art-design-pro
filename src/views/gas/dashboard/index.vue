<template>
  <div class="gas-dashboard">
    <!-- Row 1: 3D Viewer + Multi-Line Chart (equal horizontal split) -->
    <el-row :gutter="20">
      <el-col :xl="8" :lg="8" :md="24" :xs="24">
        <div class="viewer-wrapper">
          <Art3DViewer />
        </div>
      </el-col>
      <el-col :xl="16" :lg="16" :md="24" :xs="24">
        <div class="chart-card">
          <div class="chart-card__header">
            <span class="chart-card__title">Multi-Gas Trend (24h)</span>
            <span class="chart-card__subtitle">8 sensors · 6 time points</span>
          </div>
          <ArtLineChart
            :data="multiLineData"
            :x-axis-data="timeAxisData"
            :show-legend="true"
            :smooth="true"
            height="25rem"
          />
        </div>
      </el-col>
    </el-row>

    <!-- Row 2: Stats Cards -->
    <el-row :gutter="16" class="mt-20">
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe70c;"
          title="Gas Concentration"
          :count="gasConcentration"
          description="CH4 · ppm"
        />
      </el-col>
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe6f7;"
          title="Temperature"
          :count="temperature"
          description="°C · Ambient"
        />
      </el-col>
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe6d0;"
          title="Humidity"
          :count="humidity"
          description="%RH · Ambient"
        />
      </el-col>
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe6c2;"
          title="Today Alerts"
          :count="todayAlerts"
          description="Events · 24h"
          icon-color="#e6a23c"
        />
      </el-col>
    </el-row>

    <!-- Row 3: Donut Chart + Alert List -->
    <el-row :gutter="20" class="mt-20">
      <el-col :xl="12" :lg="12" :md="24" :xs="24">
        <ArtDonutChartCard
          :value="78"
          title="Gas Composition"
          :percentage="78"
          percentage-label="Primary"
          current-value="78"
          previous-value="65"
          :height="10"
          :data="donutData"
        />
      </el-col>
      <el-col :xl="12" :lg="12" :md="24" :xs="24">
        <ArtDataListCard
          title="Latest Alerts"
          subtitle="Real-time monitoring events"
          :list="alertList"
          :max-count="4"
          :show-more-button="true"
        />
      </el-col>
    </el-row>
  </div>
</template>

<script setup lang="ts">
  import type { LineDataItem } from '@/types/component/chart'
  import { ref } from 'vue'
  import Art3DViewer from '@/components/custom/art-3d-viewer/index.vue'
  import ArtLineChart from '@/components/core/charts/art-line-chart/index.vue'
  import ArtStatsCard from '@/components/core/cards/art-stats-card/index.vue'
  import ArtDonutChartCard from '@/components/core/cards/art-donut-chart-card/index.vue'
  import ArtDataListCard from '@/components/core/cards/art-data-list-card/index.vue'

  defineOptions({ name: 'GasDashboard' })

  // Stats data
  const gasConcentration = ref(1280)
  const temperature = ref(24.6)
  const humidity = ref(58)
  const todayAlerts = ref(3)

  // Time axis (6 time points over 24h)
  const timeAxisData = ref(['00:00', '04:00', '08:00', '12:00', '16:00', '20:00'])

  // Multi-line chart data: 8 gas sensors
  const multiLineData = ref<LineDataItem[]>([
    {
      name: 'CH4 Sensor A',
      data: [1150, 1200, 1280, 1320, 1250, 1180],
      areaStyle: { startOpacity: 0.15, endOpacity: 0 }
    },
    {
      name: 'CH4 Sensor B',
      data: [1080, 1120, 1190, 1240, 1180, 1100],
      areaStyle: { startOpacity: 0.15, endOpacity: 0 }
    },
    {
      name: 'H2S Sensor A',
      data: [42, 48, 55, 50, 45, 40],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'H2S Sensor B',
      data: [38, 42, 50, 47, 41, 36],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'CO Sensor A',
      data: [320, 350, 380, 360, 340, 310],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'CO Sensor B',
      data: [300, 330, 360, 345, 320, 295],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'O2 Sensor',
      data: [209, 208, 207, 207, 208, 209],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'NH3 Sensor',
      data: [15, 18, 22, 20, 17, 14],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    }
  ])

  // Donut chart data
  const donutData = ref<[number, number]>([78, 22])

  // Alert list
  const alertList = ref([
    {
      title: 'CH4 concentration exceeded threshold',
      status: 'warning',
      time: '10:32 AM',
      class: 'warning',
      icon: '&#xe6c2;'
    },
    {
      title: 'Sensor #03 calibration check passed',
      status: 'success',
      time: '09:15 AM',
      class: 'success',
      icon: '&#xe621;'
    },
    {
      title: 'New firmware v3.2.1 deployed',
      status: 'info',
      time: '08:00 AM',
      class: 'info',
      icon: '&#xe81a;'
    },
    {
      title: 'H2S trace detected below threshold',
      status: 'info',
      time: '06:45 AM',
      class: 'info',
      icon: '&#xe81a;'
    }
  ])
</script>

<style lang="scss" scoped>
  .gas-dashboard {
    padding: 20px;
    padding-bottom: 30px;

    .mt-20 {
      margin-top: 20px;
    }

    .viewer-wrapper {
      width: 100%;
      height: 400px;
      overflow: hidden;
      background: linear-gradient(135deg, #1a2332 0%, #1a2a3a 100%);
      border-radius: var(--custom-radius);
    }

    .chart-card {
      padding: 20px;
      background-color: var(--art-main-bg-color);
      border-radius: var(--custom-radius);

      &__header {
        display: flex;
        align-items: baseline;
        justify-content: space-between;
        margin-bottom: 8px;
      }

      &__title {
        font-size: 16px;
        font-weight: 500;
        color: var(--art-text-color-primary);
      }

      &__subtitle {
        font-size: 12px;
        color: var(--art-text-color-secondary);
      }
    }
  }
</style>
