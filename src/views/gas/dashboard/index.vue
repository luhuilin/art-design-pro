<template>
  <div class="gas-dashboard">
    <!-- Row 1: 3D Viewer (1/3) + Multi-Line Chart (2/3) -->
    <el-row :gutter="20" class="row-equal-height">
      <el-col :xl="8" :lg="8" :md="24" :xs="24" class="col-stretch">
        <div class="viewer-wrapper">
          <Art3DViewer />
        </div>
      </el-col>
      <el-col :xl="16" :lg="16" :md="24" :xs="24" class="col-stretch">
        <div class="chart-card">
          <div class="chart-card__header">
            <span class="chart-card__title">多气体趋势 (24h)</span>
            <span class="chart-card__subtitle">8 传感器 · 6 时间点</span>
          </div>
          <div class="chart-card__body">
            <div class="chart-legend-list">
              <div
                v-for="(item, index) in multiLineData"
                :key="item.name"
                class="chart-legend-item"
              >
                <span
                  class="legend-dot"
                  :style="{ backgroundColor: legendColors[index % legendColors.length] }"
                ></span>
                <span class="legend-label">{{ item.name }}</span>
              </div>
            </div>
            <div class="chart-wrapper">
              <ArtLineChart
                :data="multiLineData"
                :x-axis-data="timeAxisData"
                :show-legend="false"
                :smooth="true"
                height="25rem"
              />
            </div>
          </div>
        </div>
      </el-col>
    </el-row>

    <!-- Row 2: Stats Cards (Sinicized) -->
    <el-row :gutter="16" class="mt-20">
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe70c;"
          title="气体浓度"
          :count="gasConcentration"
          description="甲烷 · ppm"
        />
      </el-col>
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe6f7;"
          title="温度"
          :count="temperature"
          description="°C · 环境温度"
        />
      </el-col>
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard icon="&#xe6d0;" title="湿度" :count="humidity" description="%RH · 环境湿度" />
      </el-col>
      <el-col :xl="6" :lg="6" :md="12" :xs="12">
        <ArtStatsCard
          icon="&#xe6c2;"
          title="今日告警"
          :count="todayAlerts"
          description="事件 · 24小时"
          icon-color="#e6a23c"
        />
      </el-col>
    </el-row>

    <!-- Row 3: Art Bot Chat Button -->
    <el-row :gutter="20" class="mt-20">
      <el-col :span="24">
        <div class="chat-launch-card">
          <div class="chat-launch-card__info">
            <div class="chat-launch-card__icon">
              <i class="iconfont-sys">&#xe89a;</i>
            </div>
            <div class="chat-launch-card__text">
              <h4>Art Bot 智能助手</h4>
              <p>点击启动 AI 对话，获取实时气体监测分析与建议</p>
            </div>
          </div>
          <el-button type="primary" size="large" round @click="openChatBot">
            <i class="iconfont-sys" style="margin-right: 6px">&#xe89a;</i>
            启动 Art Bot
          </el-button>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script setup lang="ts">
  import type { LineDataItem } from '@/types/component/chart'
  import { ref } from 'vue'
  import { mittBus } from '@/utils/sys'
  import Art3DViewer from '@/components/custom/art-3d-viewer/index.vue'
  import ArtLineChart from '@/components/core/charts/art-line-chart/index.vue'
  import ArtStatsCard from '@/components/core/cards/art-stats-card/index.vue'

  defineOptions({ name: 'GasDashboard' })

  // Stats data
  const gasConcentration = ref(1280)
  const temperature = ref(24.6)
  const humidity = ref(58)
  const todayAlerts = ref(3)

  // Time axis (6 time points over 24h)
  const timeAxisData = ref(['00:00', '04:00', '08:00', '12:00', '16:00', '20:00'])

  // Legend colors (match ECharts default palette)
  const legendColors = [
    '#409eff',
    '#4ABEFF',
    '#EDF2FF',
    '#14DEBA',
    '#FFAF20',
    '#FA8A6C',
    '#FFAF20',
    '#67C23A'
  ]

  // Multi-line chart data: 8 gas sensors
  const multiLineData = ref<LineDataItem[]>([
    {
      name: 'CH4 传感器 A',
      data: [1150, 1200, 1280, 1320, 1250, 1180],
      areaStyle: { startOpacity: 0.15, endOpacity: 0 }
    },
    {
      name: 'CH4 传感器 B',
      data: [1080, 1120, 1190, 1240, 1180, 1100],
      areaStyle: { startOpacity: 0.15, endOpacity: 0 }
    },
    {
      name: 'H2S 传感器 A',
      data: [42, 48, 55, 50, 45, 40],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'H2S 传感器 B',
      data: [38, 42, 50, 47, 41, 36],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'CO 传感器 A',
      data: [320, 350, 380, 360, 340, 310],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'CO 传感器 B',
      data: [300, 330, 360, 345, 320, 295],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'O2 传感器',
      data: [209, 208, 207, 207, 208, 209],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    },
    {
      name: 'NH3 传感器',
      data: [15, 18, 22, 20, 17, 14],
      areaStyle: { startOpacity: 0.1, endOpacity: 0 }
    }
  ])

  // Open Art Bot chat window via mittBus
  const openChatBot = () => {
    mittBus.emit('openChat')
  }
</script>

<style lang="scss" scoped>
  .gas-dashboard {
    padding: 20px;
    padding-bottom: 30px;

    .mt-20 {
      margin-top: 20px;
    }

    // Equal height row for 3D + chart
    .row-equal-height {
      align-items: stretch;
    }

    .col-stretch {
      display: flex;

      > * {
        flex: 1;
      }
    }

    .viewer-wrapper {
      width: 100%;
      min-height: 100%;
      overflow: hidden;
      background: linear-gradient(135deg, #1a2332 0%, #1a2a3a 100%);
      border-radius: var(--custom-radius);
    }

    .chart-card {
      display: flex;
      flex-direction: column;
      width: 100%;
      padding: 20px;
      background-color: var(--art-main-bg-color);
      border-radius: var(--custom-radius);

      &__header {
        display: flex;
        flex-shrink: 0;
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

      &__body {
        display: flex;
        flex: 1;
        min-height: 0;
      }
    }

    .chart-legend-list {
      display: flex;
      flex-direction: column;
      flex-shrink: 0;
      gap: 4px;
      justify-content: center;
      width: 140px;
      padding-right: 12px;
      overflow-y: auto;
    }

    .chart-legend-item {
      display: flex;
      gap: 6px;
      align-items: center;
      padding: 2px 0;
      cursor: default;
    }

    .legend-dot {
      flex-shrink: 0;
      width: 10px;
      height: 10px;
      border-radius: 50%;
    }

    .legend-label {
      overflow: hidden;
      font-size: 12px;
      color: var(--art-text-color-secondary);
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    .chart-wrapper {
      flex: 1;
      min-width: 0;
    }

    // Art Bot chat launch card
    .chat-launch-card {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 20px 28px;
      background-color: var(--art-main-bg-color);
      border-radius: var(--custom-radius);

      &__info {
        display: flex;
        gap: 16px;
        align-items: center;
      }

      &__icon {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 56px;
        height: 56px;
        background: linear-gradient(135deg, #409eff 0%, #67c23a 100%);
        border-radius: 14px;

        i {
          font-size: 28px;
          color: #fff;
        }
      }

      &__text {
        h4 {
          margin: 0 0 4px;
          font-size: 16px;
          font-weight: 500;
          color: var(--art-text-color-primary);
        }

        p {
          margin: 0;
          font-size: 13px;
          color: var(--art-text-color-secondary);
        }
      }
    }
  }
</style>
