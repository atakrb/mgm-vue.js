<template>
  <v-app style="background-color: #e8eff3">
    <!-- HEADER -->
    <v-container>
      <v-row dense>
        <v-col cols="5"></v-col>
        <v-col cols="7">
          <v-btn-toggle class="justify-end d-flex" style="background: transparent">
            <v-btn>
              <v-icon>mdi-format-align-left</v-icon>
            </v-btn>
            <v-btn>
              <v-icon>mdi-format-align-center</v-icon>
            </v-btn>
            <v-btn>
              <v-icon>mdi-format-align-right</v-icon>
            </v-btn>
            <v-btn>
              <v-icon>mdi-format-align-justify</v-icon>
            </v-btn>
          </v-btn-toggle>
        </v-col>
      </v-row>
    </v-container>

    <!-- ICONLAR VE KARTLAR -->
    <section style="background-color: white; ">
      <v-container class="pa-0">
        <v-row dense>
          <!-- Sol Kartlar -->
          <v-col cols="4">


            <v-row>
              <v-col cols="5">
                <v-card-text><img src="/images/csblogo-final.svg" alt="" style="width: 120px"></v-card-text>
              </v-col>

              <v-divider
                  vertical
                  class="my-auto"
                  style="height: 60px; background-color: black; opacity: 20%;"
              ></v-divider>

              <v-col cols="6">
                <v-card-text><img src="/images/mgmlogo-final.svg" alt="" style="width: 120px"></v-card-text>
              </v-col>
            </v-row>

          </v-col>

          <!-- Sağdaki Iconlu Linkler -->
          <v-col cols="8" class="d-flex align-center justify-space-around flex-wrap">
            <div v-for="(item, index) in icons" :key="index"
                 class="d-flex flex-column align-center justify-center ma-2">
              <a :href="item.url" :target="item.target" class="text-decoration-none text-black text-center">
                <v-icon class="mb-1">{{ item.name }}</v-icon>
                <br>
                <span class="text-overline" style="color: dimgray">{{ item.text }}</span>
              </a>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </section>

    <!-- ARAMA & HAVA DURUMU -->
    <v-container>
      <v-row>

        <v-col cols="9">
          <v-row>

            <v-text-field
                flat
                v-model="name"
                :rules="nameRules"
                label="İl ilçe ve Havalimanı Ara..."
                outlined
                dense
                solo
                required
                append-icon="mdi-magnify"
                class="mt-3 ms-3 mb-5"

            ></v-text-field>

          </v-row>

          <v-col cols="4">
            <v-row>
              <v-col class="text-subtitle-2 pt-0 mt-0">
                <span style="font-weight: 150">{{ formattedDate }} - {{ formattedTime }}</span>
              </v-col>
            </v-row>
            <v-row>
              <v-col class="text-subtitle-2 pt-0">
                <span style="font-weight: 300; font-size: 30px">Ankara</span>
              </v-col>
            </v-row>
            <v-row>
              <v-col class="text-subtitle-2 pt-0">
                <span style="font-weight: 400; font-size: 35px">33,9 °C</span>
              </v-col>
            </v-row>
          </v-col>

          <v-row>
            <a href="#"><img src="/images/havadurumu-icon/hava-durumu-grafik.PNG" alt=""></a>
          </v-row>

        </v-col>

        <!-- Sağdaki Yetkililer -->
        <v-col cols="3">
          <v-row v-for="person in people" :key="person.name">
            <v-col class="ata" cols="12"
            >
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
          <v-row>
            <v-col cols=6>
              <v-btn width="100%"
                     block
                     elevation="4"
                     color="#1eb6d2"
                     style="color: white"
              >
                <h4>Bugün</h4>
              </v-btn>
            </v-col>
            <v-col cols="6">
              <v-btn width="100%"
                     block
                     elevation="4"
                     color="#1eb6d2"
                     style="color: white">
                <h4>Yarın</h4>

              </v-btn>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <a href="#"><img src="/images/trharita/yesil-tr-map.jpg" alt=""></a>
            </v-col>
          </v-row>

          <v-row>
            <v-col cols="12">
              <v-card class="mb-2 d-flex align-center pa-2">
                <v-icon color="orange darken-2" class="mr-2">mdi-alert</v-icon>
                <span>Deprem Bölgesi Hava Tahmini</span>
              </v-card>

              <v-card class="mb-2 d-flex align-center pa-2">
                <v-icon color="orange darken-2" class="mr-2">mdi-alert</v-icon>
                <span>Kuvvetli Meteorolojik Hadiseler için Alınacak Tedbirler</span>
              </v-card>

              <!-- Kırmızı uyarı kutusu -->
              <v-card class="d-flex">

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
    <v-footer>
      <v-col class="text-center">© {{ new Date().getFullYear() }} Ata'nın Sitesi</v-col>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      name: '',
      nameRules: [],
      now: new Date(),
      icons: [
        {name: 'mdi-file-document-outline', url: '#', target: '_blank', text: 'Kurumsal'},
        {name: 'mdi-chart-line', url: '#', target: '', text: 'Tahminler'},
        {name: 'mdi-clock-outline', url: '#', target: '', text: 'Son Durum'},
        {name: 'mdi-airplane', url: '#', target: '', text: 'Havacılık'},
        {name: 'mdi-sail-boat', url: '#', target: '', text: 'Denizcilik'},
        {name: 'mdi-leaf', url: '#', target: '', text: 'Ziraat'},
        {name: 'mdi-magnify', url: '#', target: '', text: 'Analizler'},
        {name: 'mdi-email-outline', url: '#', target: '', text: 'İletişim'}
      ],
      people: [
        {name: 'Murat KURUM', image: '/images/yk/muratkurum.jpg', title: 'Çevre ve Şehircilik Bakanı'},
        {name: 'Volkan Mutlu COŞKUN', image: '/images/yk/volkanmutlucoskun.jpg', title: 'Meteoroloji Genel Müdürü'}
      ]
    }
  },
  computed: {
    formattedDate() {
      return this.now.toLocaleDateString('tr-TR', {month: 'long', day: 'numeric'});
    },
    formattedTime() {
      return this.now.toLocaleTimeString('tr-TR', {hour: '2-digit', minute: '2-digit'});
    }
  },
  mounted() {
    setInterval(() => {
      this.now = new Date();
    }, 1000);
  }
}
</script>

<style scoped>
.text-decoration-none {
  text-decoration: none;
}

.text-black {
  color: black;
}

.ata {
  max-height: 85px;
}

.uyari-kutu {
  background-color: #d32f2f;
  min-width: 70px;
  animation: yanip-sonme 0.7s infinite;
}

@keyframes yanip-sonme {
  0% {
    background-color: #fd0000;
  }
  50% {
    background-color: #750000;
  }
  /* Açık kırmızı tonu */
  100% {
    background-color: #fb0000;
  }
}
</style>
