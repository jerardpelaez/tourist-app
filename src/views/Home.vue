<template>
  <div class="home">
    <v-container>
      <h1 class="text-center font-weight-light">Explore the Wonders of Japan</h1>
      <v-layout row>
        <v-flex xs6 md4 v-for="info in infos" :key="info.title" class="mt-10">
          <v-card class="ma-3" max-width="400">
            <v-img class="white--text align-end" height="200px" :src="info.src">
              <v-card-title class="headline font-weight-bold">{{info.title}}</v-card-title>
            </v-img>

            <v-card-text class="text--primary">
              <div class="title font-weight-light">{{info.location}}</div>
            </v-card-text>

            <v-card-actions>
              <v-btn color="red lighten-3" text @click="modal(info)">Explore</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>

    <v-row justify="center">
      <v-dialog v-model="dialog" max-width="600px">
        <v-card>
          <v-img
            :src="city.img"
            height="300px"
            dark
          >
            <v-row class="fill-height">
              <v-card-title class="white--text pl-12 pt-12">
                <div class="display-1 pl-12 pt-12">{{ city.name }}</div>
              </v-card-title>
            </v-row>
          </v-img>

          <v-list two-line>
            <v-list-item>
              <v-img :src="`http://openweathermap.org/img/wn/${city.weather.icon}@2x.png`"/>
              <v-list-item-content>
                <v-list-item-title>{{ city.weather.main }}</v-list-item-title>
                <v-list-item-subtitle>{{ city.weather.description }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <v-divider inset></v-divider>

            <v-list-item v-for="(item, index) in city.details" :key="index">
              <v-list-item-content>
                <v-list-item-title>{{ item.name }}</v-list-item-title>
                <v-list-item-subtitle>{{ item.location.address }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>

            <v-divider></v-divider>
          </v-list>
          <v-card-action>
            <v-btn text @click="dialog = false"> 
              close
            </v-btn>
          </v-card-action>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: "home",
  components: {},
  data() {
    return {
      city: {
        name: '',
        img: '',
        weather: []
      },
      dialog: false,
      infos: [
        {
          src:
            "https://images.unsplash.com/photo-1536098561742-ca998e48cbcc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=762&q=80",
          location: "Tokyo Tower",
          title: "Tokyo"
        },
        {
          src:
            "https://images.unsplash.com/photo-1571267468911-a8d098c0f900?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80",
          location: "Prefectura de Kanagawa",
          title: "Yokohama"
        },
        {
          src:
            "https://images.unsplash.com/photo-1558862107-d49ef2a04d72?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80",
          location: "Fushimi Inari",
          title: "Kyoto"
        },
        {
          src:
            "https://images.unsplash.com/photo-1564839324965-32af59c0435a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1190&q=80",
          location: "Osaka Tower",
          title: "Osaka"
        },
        {
          src:
            "https://images.unsplash.com/photo-1519105467443-4779d0fb729d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1050&q=80",
          location: "Sapporo Ice Festival",
          title: "Sapporo"
        },
        {
          src: "https://www.japan-guide.com/g18/3300_11.jpg",
          location: "Nagoya Castle",
          title: "Nagoya"
        }
      ]
    };
  },
  methods: {
    modal(item) {
      this.dialog = true;
      this.city.name = item.title;
      this.city.img = item.src
      this.getAPI(item.title)
    },
    getAPI(city){
      const apiKey = 'a0beb7aeb3f0c7871f11d37066cac05a'
      const api = `http://api.openweathermap.org/data/2.5/forecast?q=${city}&appid=${apiKey}`
      
      const client_id = 'TIUCRXEY4V3K42C1X5FXD0VUA0BECILI1SN5HB454XA5S1D0'
      const client_secret = 'LX12MHCXH1CVKBYRJSR3AV2D4BYDCE2DWSIDYRKUQFHIUZSX'
      const setDetail = `https://api.foursquare.com/v2/venues/search?near=${city}&client_id=${client_id}&client_secret=${client_secret}&v=20191203`

      axios.get(api).then(({ data }) => {
          this.city.weather = data.list[0].weather[0]
        }
      );

      axios.get(setDetail).then(({data})=>{
        this.city.details = data.response.venues
      })
    }
  }
};
</script>