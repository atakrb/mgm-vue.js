<template>
  <v-app style="background-color: #e8eff3">
    <!-- HEADER -->
    <v-container>
      <v-row dense>
        <v-col cols="12" class="justify-end d-flex justify-between">

          <div
              v-for="(logos, i) in logo"
              :key="i"
              class="ma-1"
               style="background: transparent;">
            <a :href=" logos.url "><v-icon>{{ logos.title }}</v-icon></a>

          </div>
        </v-col>
      </v-row>
    </v-container>

    <!-- ICONLAR VE KARTLAR -->
    <section style="background-color: white; ">
      <v-container class="pa-0">
        <v-row dense>
          <!-- Sol Kartlar -->
          <v-col cols="4" >
            <v-row>
              <v-col cols="5">
                <v-card-text>
                  <img src="/images/csblogo-final.svg" alt="" style="width:120px">
                </v-card-text>
              </v-col>

              <v-divider vertical class="my-auto" style="height:60px; background-color:black; opacity:.2;"></v-divider>

              <v-col cols="6">
                <v-card-text>
                  <img src="/images/mgmlogo-final.svg" alt="" style="width:120px">
                </v-card-text>
              </v-col>
            </v-row>
          </v-col>

          <!-- Sağdaki Iconlu Linkler -->
          <v-col cols="12" sm="" class="d-flex align-center justify-space-around flex-wrap ">
            <div
                v-for="(item, index) in icons"
                :key="index"
                class="d-flex flex-column align-center justify-center ma-2 "
            >
              <a :href="item.url" :target="item.target" class="text-decoration-none text-black text-center">
                <div class="red-btn">
                  <v-icon class="mb-1 ">{{ item.name }}</v-icon><br>
                  <span class="text-overline ">{{ item.text }}</span>
                </div>
              </a>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- ARAMA & HAVA DURUMU -->
    <v-container>
      <v-row>
        <!-- Sol: arama ve hava bilgisi -->
        <v-col cols="12" md="9" >
          <v-row>
            <v-col cols="12">
              <v-text-field
                  flat
                  v-model="name"
                  :rules="nameRules"
                  label="İl, ilçe veye Havalimanı Ara..."
                  outlined
                  dense
                  solo
                  required
                  append-icon="mdi-magnify"
                  class="mt-3 ms-3 mb-0"
              />
            </v-col>
          </v-row>

          <v-row>
            <v-col cols="12" >
              <v-card color: elevation="0" outlined style="border-radius:12px;">
                <v-col cols="12">

                  <v-row>
                    <v-col class="text-subtitle-2 pt-0 mt-0 ps-10">
                      <span style="font-weight:150">{{ formattedDate }} - {{ formattedTime }}</span>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col class="text-subtitle-2 pt-0 ps-10">
                      <span style="font-weight:300; font-size:30px">Ankara</span>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col class="text-subtitle-2 pt-2 ps-9">
                      <span style="font-weight:350; font-size:55px">33,9 °C</span>
                    </v-col>
                  </v-row>
                </v-col>
                <v-col cols="12">
                  <WeatherChart />
                </v-col>
              </v-card>
            </v-col>


          </v-row>




          <!-- GÜNLÜK KARTLAR -->
          <v-row class="mt-0 d-flex align-center justify-space-around flex-wrap">
            <v-col
                elevation="0"
                outlined
                dense
                class="mx-auto mb-2"
                v-for="(d, i) in days"
                :key="i"
            >
              <v-card class="day-card" elevation="0">
                <!-- Başlık -->
                <div class="day-header text-center py-2">
                  {{ d.name }}
                </div>

                <v-divider></v-divider>

                <!-- İkon + açıklama -->
                <div class="text-center mt-3">
                  <v-icon size="35" color="amber darken-1">{{ d.icon }}</v-icon>
                  <div class="subtitle-2 mt-1 grey--text text--darken-1">
                    {{ d.title }}
                  </div>
                </div>

                <!-- Min / Max -->
                <v-row class="px-4 pb-3 pt-4">
                  <v-col cols="6" class="text-center">
                    <span class="min-temp">{{ d.min }}°c</span>
                  </v-col>
                  <v-col cols="6" class="text-center">
                    <span class="max-temp">{{ d.max }}°c</span>
                  </v-col>
                </v-row>
              </v-card>
            </v-col>
          </v-row>


        </v-col>

        <!-- Sağ: yetkililer + toggle + uyarılar -->
        <v-col  lg="3" sm="12" md="6">
          <v-row v-for="person in people" :key="person.name">
            <v-col class="ata" cols="12">
              <v-card outlined>
                <v-row>
                  <v-col cols="3">
                    <a href="#">
                      <v-avatar tile size="80">
                        <img :src="person.image" :alt="person.name">
                      </v-avatar>
                    </a>
                  </v-col>
                  <v-col cols="9">
                    <v-card-subtitle class="text-start">
                      <h4>{{ person.name }}</h4>
                      <hr>
                      {{ person.title }}
                    </v-card-subtitle>
                  </v-col>
                </v-row>
              </v-card>
            </v-col>
          </v-row>

          <!-- Bugün / Yarın toggle -->

            <v-row no-gutters style="flex-wrap: nowrap;" class="mt-6">
              <v-col cols="6" class="flex-grow-0 flex-shrink-0">
                <v-btn
                    block
                    depressed
                    :color="is('today') ? '#1eb6d2' : 'white'"
                    :class="is('today') ? 'white--text' : 'grey--text text--darken-1'"
                    @click="day = 'today'"
                >
                  Bugün
                </v-btn>
              </v-col>

              <v-col cols="6" style="min-width:100px;" class="flex-grow-0 flex-shrink-1">
                <v-btn
                    block
                    depressed
                    :color="is('tomorrow') ? '#1eb6d2' : 'white'"
                    :class="is('tomorrow') ? 'white--text' : 'grey--text text--darken-1'"
                    @click="day = 'tomorrow'"
                >
                  Yarın
                </v-btn>
              </v-col>
            </v-row>


          <v-row>
            <v-col cols="6" sm="12" class="pt-sm-0 mt-sm-0 pb-sm-0 mb-sm-16 ata">
              <a href="#" ><img src="/images/trharita/yesil-tr-map.jpg" alt=""></a>
            </v-col>
          </v-row>

          <v-row>
            <v-col cols="12">
              <v-card elevation="0" outlined class="mb-1 d-flex align-center pa-2">
                <v-icon color="orange darken-2" class="mr-2">mdi-alert</v-icon>
                <span>Deprem Bölgesi Hava Tahmini</span>
              </v-card>

              <v-card elevation="0" outlined class="mb-1 d-flex align-center pa-2">
                <v-icon color="orange darken-2" class="mr-2">mdi-alert</v-icon>
                <span>Kuvvetli Meteorolojik Hadiseler için Alınacak Tedbirler</span>
              </v-card>

              <!-- Kırmızı uyarı kutusu -->
              <v-card elevation="0" outlined class="d-flex">
                <!-- Sol kırmızı kutu (YANIP SÖNEN ARKA PLAN) -->
                <div class="uyari-kutu d-flex flex-column align-center justify-center px-3 py-2">
                  <v-icon class="white--text" large>mdi-exclamation</v-icon>
                  <small class="white--text font-weight-bold">UYARI</small>
                </div>

                <!-- Sağ içerik -->
                <div class="pa-3 d-flex align-center">
                  <strong>Denizlerimizde (Kuzey Ege) Fırtına Bekleniyor!</strong>
                </div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </v-container>

    <!-- FOOTER -->
    <v-footer style="background-color: #1eb6d2">
      <v-col class="text-center" style="background-color: #1eb6d2">© {{ new Date().getFullYear() }} Ata'nın Sitesi</v-col>
    </v-footer>
  </v-app>
</template>

<script>
import WeatherChart from '@/components/WeatherChart.vue'

export default {
  components: { WeatherChart },
  name: 'HomePage',
  data() {
    return {
      day: 'today',
      name: '',
      nameRules: [],
      now: new Date(),
      icons: [
        { name: 'mdi-file-document-outline', url: 'https://www.mgm.gov.tr/kurumsal/birimler.aspx', target: '', text: 'Kurumsal' },
        { name: 'mdi-chart-line',           url: 'https://www.mgm.gov.tr/sondurum/turkiye.aspx', target: '',       text: 'Tahminler' },
        { name: 'mdi-clock-outline',        url: 'https://www.mgm.gov.tr/tahmin/il-ve-ilceler.aspx', target: '',       text: 'Son Durum' },
        { name: 'mdi-airplane',             url: 'https://hezarfen.mgm.gov.tr', target: '',       text: 'Havacılık' },
        { name: 'mdi-sail-boat',            url: 'https://pirireis.mgm.gov.tr/tahmin-raporlari/ucgunluk-tahmin', target: '',       text: 'Denizcilik' },
        { name: 'mdi-leaf',                 url: 'https://tarim.mgm.gov.tr', target: '',       text: 'Ziraat' },
        { name: 'mdi-magnify',              url: 'https://www.mgm.gov.tr/veridegerlendirme/sicaklik-analizi.aspx', target: '',       text: 'Analizler' },
        { name: 'mdi-email-outline',        url: 'https://www.mgm.gov.tr/site/iletisim.aspx', target: '',       text: 'İletişim' }
      ],
      logo: [
        {title: "mdi-facebook", url: 'https://www.facebook.com'},
        {title: "mdi-twitter", url: 'https://www.x.com'},
        {title: "mdi-linkedin", url: 'https://www.linkedin.com/in/atakarabulut'},
        {title: "mdi-instagram", url: 'https://www.instagram.com/ata.krb'},
        {title: "mdi-eye-off-outline", url: '#'},
        {title: "mdi-microphone", url: '#'},

        {title: "mdi-cog",}

      ],
      people: [
        { name: 'Murat KURUM',           image: '/images/yk/muratkurum.jpg',        title: 'Çevre ve Şehircilik Bakanı' },
        { name: 'Volkan Mutlu COŞKUN',   image: '/images/yk/volkanmutlucoskun.jpg', title: 'Meteoroloji Genel Müdürü' }
      ],
      days: [

        { name: 'Pazartesi', title: 'Az Bulutlu', min: 19, max: 33, icon: 'mdi-cloud'},
        { name: 'Salı',      title: 'Güneşli', min: 19, max: 33, icon: 'mdi-white-balance-sunny'},
        { name: 'Çarşamba',  title: 'Az Bulutlu', min: 19, max: 33, icon: 'mdi-cloud'},
        { name: 'Perşembe',     title: 'Gök Gürültülü', min: 20, max: 33, icon: 'mdi-weather-lightning-rainy'},
        { name: 'Cuma',     title: 'Güneşli', min: 20, max: 33, icon: 'mdi-white-balance-sunny'},
      ]
    };
  },
  computed: {
    formattedDate() {
      return this.now.toLocaleDateString('tr-TR', { month: 'long', day: 'numeric' });
    },
    formattedTime() {
      return this.now.toLocaleTimeString('tr-TR', { hour: '2-digit', minute: '2-digit' });
    }
  },
  methods: {
    is(v) { return this.day === v; }
  },
  mounted() {
    this._timer = setInterval(() => { this.now = new Date(); }, 1000);
  },
  beforeDestroy() {
    clearInterval(this._timer);
  },

};
</script>

<style scoped>
.text-decoration-none { text-decoration: none; }
.text-black { color: black; }

.ata { max-height: 85px; }

.uyari-kutu {
  background-color: #d32f2f;
  min-width: 70px;
  animation: asd 0.7s infinite;
}
@keyframes asd {
  0%   { background-color: #ff0000; }
  50%  { background-color: #750000; }
  100% { background-color: #fb0000; }
}

.red-btn:hover .v-icon {
  color: red !important;
  transition: ease .2ms !important;
}
::v-deep .red-btn:hover { color: red !important; }

.day-card{
  border:10px;
  border-radius:12px;
  background:#fff;
}

.day-header{
  font-weight:600;
  color:#5f6672;
  background:#fff;
}

/* Görseldeki renklere yakın tonlar */
.min-temp{
  color:#12b7d6;        /* turkuaz */
  font-weight:600;
  font-size:1.1rem;
  text-transform: lowercase;
}

.max-temp{
  color:#ff3b30;        /* kırmızı */
  font-weight:600;
  font-size:1.0rem;
  text-transform: lowercase;
}
.v-application{
  line-height: 1;
}
</style>
