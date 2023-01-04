<template>
  <div>
    <Navbar/>
    <b-container class='bg-white'>
      <b-row class='mt-3'>
        <b-col>
          <b-breadcrumb :items='items'></b-breadcrumb>
        </b-col>
      </b-row>
    </b-container>
    <b-container class='bg-white' v-if='isNotFound'>
      <b-row class='mt-3'>
        <b-col class='text-center'>
          <h1>Not Found</h1>
        </b-col>
      </b-row>
    </b-container>
    <b-container class='bg-white' v-if='Object.keys(data).length > 0'>
      <b-row class='mt-3'>
        <b-col>
          <b-card>
            <b-row class='mt-3'>
              <b-col md='3'>
                <b-img v-bind:src="data.photo" thumbnail fluid alt=""></b-img>
              </b-col>
              <b-col md='9'>
                <h3>{{data.name}}</h3>
                <label>{{data.vicinity}}</label>
                <div>
                  <b-card-text v-if='data.types'>
                    <b-badge class='m-1' variant='info' v-for='(type) in JSON.parse(data.types)' v-bind:key='type'>
                      {{ typeRestaurant(type) }}
                    </b-badge>
                  </b-card-text>
                  <b-badge variant='primary'>
                    {{ data.rating }}
                    <fa :icon="['fas', 'star']" />
                  </b-badge>
                  {{ data.user_ratings_total }} รีวิว
                </div>
              </b-col>
            </b-row>
          </b-card>
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>


<script>
import axios from 'axios'

export default {
  name: 'IndexPage',
  created() {
    this.loadRestaurants(this.$route.params.id);
  },
  methods: {
    async loadRestaurants(id) {
      await axios.get('http://127.0.0.1:8000/api/find-by-id/'+ id).then(res => {
        console.log('res', res.data)
        const resData = res.data;
        this.data = resData.data;
        this.items[1].text = this.data ? this.data.name : '';
        if(this.data.length === 0){
          this.isNotFound = true;
        }
        console.log('data', this.data);
      })
    },
    typeRestaurant(type) {
      switch (type) {
        case 'restaurant':
          return 'ร้านอาหาร'
        case 'cafe':
          return 'Cafe'
        case 'meal_delivery':
          return 'กลับบ้าน'
        case 'point_of_interest':
          return 'ร้านดัง'
        case 'food':
          return 'อาหาร'
        case 'establishment':
          return 'สถานประกอบการ'
        default:
          return '-'
      }
    },

  },
  data() {
    return {
      data: {},
      titleName:'',
      isNotFound: false,
      items: [
        {
          text: 'หน้าแรก',
          href: '/'
        },
        {
          text: '',
          href: '#'
        }
      ]
    }
  }
}
</script>
