<template>
  <div>
    <Navbar />
    <b-container class='bg-white'>
      <b-row class='mt-3'>
        <b-col>
          <b-breadcrumb :items='items'></b-breadcrumb>
        </b-col>
      </b-row>
    </b-container>
    <b-container class='bg-white'>
      <b-row class='mt-3' align-h='end'>
        <b-col md='3'>
          <b-form @submit='onSubmit' @reset='onReset'>
            <b-input-group>
              <b-form-input v-model='search' placeholder='Search....'></b-form-input>
              <b-input-group-append>
                <b-button variant='info' type='submit'>
                  <fa :icon="['fas', 'search']" />
                  Search
                </b-button>
              </b-input-group-append>
            </b-input-group>
          </b-form>

        </b-col>
      </b-row>
    </b-container>
    <b-container class='bg-white'>
      <b-row class='mt-3'>
        <b-col>
          <b-card>
            <b-row class='mt-3'>
              <b-col>
                <h5>ร้านอาหารแนะนำ</h5>
              </b-col>
            </b-row>
            <b-row v-if='listData' class='mt-3'>
              <b-col md='12'>
                <div class='overflow-auto'>
                  <b-pagination
                    v-bind:v-model='listData.current_page'
                    v-bind:total-rows='listData.total'
                    v-bind:per-page='listData._perpage'
                    aria-controls='my-table'
                    v-on:page-click='changePaginate'
                  ></b-pagination>
                  <p class='mt-3'>Current Page: {{ listData.current_page }}</p>
                </div>
              </b-col>
              <b-col v-for='(val, index) in listData.data' v-bind:key='index' md='3' class='pointer'>
                <b-card
                  v-bind:title='val.name'
                  v-bind:img-src='val.photo ? val.photo : "~/assets/biz-promotion.svg"'
                  img-alt='Image'
                  img-top
                  tag='article'
                  style='max-width: 20rem;'
                  class='mb-2'
                  v-on:click='selectItem(val.place_id)'
                >
                  <b-card-text v-if='val.types'>
                    <b-badge class='m-1' variant='info' v-for='(type) in JSON.parse(val.types)' v-bind:key='type'>
                      {{ typeRestaurant(type) }}
                    </b-badge>
                  </b-card-text>
                  <b-badge variant='primary'>
                    {{ val.rating }}
                    <fa :icon="['fas', 'star']" />
                  </b-badge>
                  {{ val.user_ratings_total }} รีวิว
                </b-card>
              </b-col>
            </b-row>

          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>

</template>

<script>
import axios from 'axios';
export default {
  name: 'IndexPage',
  async created() {
    this.search = this.$route.query.search ? this.$route.query.search : ''
    this.page = this.$route.query.page;
    if(this.$route.query.search){ // breadcrumb
      this.items[1] = {
        text: this.$route.query.search,
        href: '#'
      }
    }
    await this.loadListData(this.search, this.page)
  },
  methods: {
    async loadListData(search = '', page = 1) { // load card data
      if (!page) {
        page = 1
      }
      await axios.get(this.$axios.defaults.baseURL + '/api/search?search=' + search + '&page=' + page).then(res => {
        console.log('res', res.data)
        const resData = res.data
        this.listData = resData
      })
    },
    onSubmit(event) { // search
      event.preventDefault()
      window.location.href = '/?search=' + this.search
    },
    onReset(event) {
      event.preventDefault()
    },
    selectItem(id) { // on click card
      console.log(id)
      window.location.href = '/restaurants/' + id
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
    changePaginate(event, page) {
      // console.log('page', page)
      let url = ''
      if (page) {
        url = '/?page=' + page
      }
      if (this.search) {
        url += '&search=' + this.search
      }
      window.location.href = url
    }
  },
  data() {
    return {
      listData: {},
      search: '',
      items: [
        {
          text: 'หน้าแรก',
          href: '/'
        },

      ]
    }
  }
}
</script>
<style>
.card-img-top {
  height: 200px;
  min-width: 200px;
  object-fit: cover;
}

.pointer {
  cursor: pointer;
}
</style>
