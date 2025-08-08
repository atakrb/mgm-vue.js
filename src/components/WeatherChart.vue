<template>
  <v-card class="pa-4" elevation="0" style="background:#e8eff3">
    <ApexChart
        type="area"
        height="280"
        :options="chartOptions"
        :series="series"
    />

    <!-- Saat & ikon şeridi -->
    <div class="d-flex justify-space-between mt-n2 px-2">
      <div v-for="(h,i) in hours" :key="i" class="text-center" style="width:100%">
        <v-icon :color="iconColor(h.icon)" small class="mb-1">
          {{ iconMap[h.icon] }}
        </v-icon>
        <div class="caption grey--text">{{ h.label }}</div>
      </div>
    </div>
  </v-card>
</template>

<script>
export default {
  name: 'WeatherChart',
  data() {
    return {
      lastUpdate: '',
      hours: [
        { label: '09:00', icon: 'partly' },
        { label: '12:00', icon: 'partly' },
        { label: '15:00', icon: 'sunny'  },
        { label: '18:00', icon: 'sunny'  },
        { label: '21:00', icon: 'partly' },
        { label: '24:00', icon: 'clear'  },
        { label: '03:00', icon: 'clear'  },
        { label: '06:00', icon: 'partly' },
      ],
      series: [
        {
          name: 'Sıcaklık (°C)',
          data: [
            { x: '09:00', y: 27 },
            { x: '12:00', y: 29 },
            { x: '15:00', y: 34 },
            { x: '18:00', y: 31 },
            { x: '21:00', y: 28 },
            { x: '24:00', y: 25 },
            { x: '03:00', y: 24 },
            { x: '06:00', y: 26 }
          ]
        }
      ],
      iconMap: {
        sunny:  'mdi-weather-sunny',
        partly: 'mdi-weather-partly-cloudy',
        clear:  'mdi-weather-night'
      },
      chartOptions: {
        chart: {
          animations: { enabled: true, easing: 'easeinout', speed: 1000 },
          toolbar: { show: false },
          zoom: { enabled: false }
        },
        stroke: { curve: 'smooth', width: 3 },
        markers: { size: 2 },
        colors: ["gray"],
        dataLabels: { enabled: false },
        fill: {
          type: 'gradient',
          gradient: {
            shadeIntensity: 0.5,
            opacityFrom: 0.35,
            opacityTo: 0.02,
            stops: [0, 80, 100]
          }
        },
        grid: {
          borderColor: '#dfe7ee',
          strokeDashArray: 4,
          padding: { left: 12, right: 12 }
        },

      }
    };
  },
  methods: {
    iconColor(key) {
      return key === 'sunny' ? 'amber darken-2'
          : key === 'partly' ? 'blue-grey'
              : 'indigo';
    },
    async updateData() {
      const next = this.series[0].data.map(v => {
        const d = Math.round((Math.random() - 0.5) * 2);
        return { x: v.x, y: Math.max(15, Math.min(40, v.y + d)) };
      });
      this.series = [{ name: 'Sıcaklık (°C)', data: next }];
      this.lastUpdate = new Date().toLocaleString('tr-TR');
    }
  },
  mounted() {
    this.lastUpdate = new Date().toLocaleString('tr-TR');
    this._timer = setInterval(this.updateData, 5 * 60 * 1000);
  },
  beforeDestroy() { clearInterval(this._timer); }
};
</script>
