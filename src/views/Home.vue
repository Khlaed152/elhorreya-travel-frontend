<template>
  <div id="home">
    <flux-parallax class="parallax-header home-header" v-if="imageHeader != ''" :src="imageHeader" height="450px" offset="200%" type="relative">
      <div class="header-content center">
        <div class="header-center">
          <h1 class="header-title text-center">
            احجز رحلتك الآن بأفضل الأسعار
            <br>
            مع
            <span class="mark-main-color" v-text="$settings.site_name_ar"></span>
          </h1>
        </div>
      </div>
    </flux-parallax>

    <!-- <section class="box-search" v-show="$store.state.showSearchBoxPages">
      <b-container>
        <search-box></search-box>
      </b-container>
    </section> -->

    <!-- content-page -->
    <section class="content-page">

      <!-- search-content -->

      <!-- <travel-boxs-page
        id="search-content"
        v-if="$store.state.showSearchResult"
        :showLoading="$store.state.showLoading"

        :changeToPrice="changeToPrice"
        :changeFromPrice="changeFromPrice"
        :changeStars="changeStars"
        :changeHotel="changeHotel"
        :sorting="sorting"

        category="search"
        :paginateClick="goToPageInSearch"
        :paginateData="$store.state.searchResult"
      ></travel-boxs-page> -->

      <!-- ./search-content -->


      <!-- home-content -->
      <div class="home-content main-content" v-if="!$store.state.showSearchResult">

          <!-- ==================================================================================== -->


          <!-- wrapper-travels-offer -->
          <div class="wrapper-section wrapper-travels-offer">
            <b-container>
              <div class="section-title-center">
                <h2 class="text">
                  {{ $settings.offers_best_tabs_home_title }}
                </h2>
              </div>

              <div class="tabs">
                <ul class="list-tabs">

                  <li
                    v-for="(tab, idx) in allDataTravelsOffer"
                    :key="tab.id"
                    class="tab"
                    :class="{active: currentTravelsOffer === (idx), disbaled: showCarouselTravelsLoading}"
                    @click="toggleTabTravels(idx)"
                    v-text="tab.name"
                  ></li>

                </ul>
              </div>

              <div class="wrapper-travels">
                <div class="loading" v-show="showCarouselTravelsLoading">
                  <span class="icon">
                    <i class="fas fa-circle-notch fa-spin"></i>
                  </span>
                </div>
                <div v-show="!showCarouselTravelsLoading">
                    <!-- :center="true" -->
                  <carousel
                    v-if="showCarouselTravels"
                    class="my-carousel"
                    ref="carouselTravels"
                    :margin="15"
                    :autoplayHoverPause="true"
                    :autoplay="true"
                    :autoplaySpeed="800"
                    :autoplayTimeout="3500"
                    :nav="navCarouselTravels"
                    :items="itemsCarouselTravels"
                    :dots="dotsCarouselTravels"
                    :smartSpeed="500"
                    :navText="[`<span class='waves-effect'><i class='fas fa-chevron-left'></i></span>`, `<span class='waves-effect'><i class='fas fa-chevron-right'></i></span>`]"
                  >
                    <div class="travel box-flat" v-for="travel in currentDataTravelsOffer.travels" :key="travel.id">
                      <router-link
                        class="wrapper-link"
                        :to="{name: 'travel', params: {id: travel.id, travel: travel}}"
                        :style="{'background-image': `url('${$domain + travel.image}')`}"
                      >
                        <div class="discount" v-if="travel.discount != null && travel.discount != ''">
                          %
                          <span class="number">{{ travel.discount }}</span>
                          خصم
                        </div>
                        <div class="content">
                          <h4 class="name" v-text="travel.name"></h4>
                          <span class="address" v-text="travel.hotel.address"></span>
                        </div>
                      </router-link>
                    </div>
                  </carousel>
                </div>
              </div>

            </b-container>
          </div>
          <!-- ./wrapper-travels-offer -->
          <!-- ==================================================================================== -->


          <div class="wrapper-section wrapper-hotels-offer">
            <b-container>
              <div class="section-title-center">
                <h2 class="text">
                  {{ $settings.hotels_best_home_title }}
                </h2>
                <div class="show-all">
                  <router-link :to="{name: 'hotels'}" class="text">
                    عرض الكل
                  </router-link>
                </div>
              </div>
              <div class="wrapper-hotels">
                <b-row>
                  <b-col sm="6" lg="4" v-for="hotel in this.$store.state.hotels.slice(0, 6)" :key="hotel.id">
                    <div class="hotel box-flat">
                      <router-link
                        class="wrapper-link"
                        :to="{name: 'hotel', params: {id: hotel.id, hotel: hotel}}"
                        :style="{'background-image': `url('${$domain + hotel.image}')`}"
                      >
                        <div class="content">
                          <h4 class="name" v-text="'فندق ' + hotel.name"></h4>
                          <span class="address" v-text="hotel.address"></span>
                        </div>
                      </router-link>
                    </div>
                  </b-col>
                </b-row>

                <div class="show-more-after-section">
                  <router-link :to="{name: 'hotels'}">للمزيد</router-link>
                </div>
              </div>
            </b-container>
          </div>
          <!-- ./wrapper-hotels-offer -->
          <!-- ==================================================================================== -->


          <!-- wrapper-fly-offer -->
          <!-- <div class="wrapper-section wrapper-fly-offer">
            <b-container>
              <div class="section-title-center">
                <h2 class="text">
                  أفضل عروض الطيران
                </h2>
                <div class="show-all">
                  <router-link :to="{name: 'flight'}" class="text">
                    عرض الكل
                  </router-link>
                </div>
              </div>

              <div class="wrapper-fly">
                <div class="loading" v-show="!showTravelsExternalLoading">
                  <span class="icon">
                    <i class="fas fa-circle-notch fa-spin"></i>
                  </span>
                </div>

                <div v-show="showTravelsExternalLoading">
                  <b-row>
                    <b-col lg="6" v-for="(travels, idx) in allDataTravelsExternal" :key="idx">
                      <div class="box-travel-external">
                        <div class="side-right">
                          <div class="image" :style="{'background-image': `url('${$domain + travels[0].hotel.image}')`}"></div>
                        </div>
                        <div class="side-left">
                          <h6 class="title">
                            رحلات الذهاب والعودة إلى
                            {{  travels[0].hotel.address }}
                          </h6>
                          <ul class="list-travels">
                            <li v-for="travel in travels" :key="travel.id">
                              <div class="address-from" v-text="travel.address_from"></div>

                              <div class="sort">
                                <img :src="require('@/assets/images/sort.svg')">
                              </div>

                              <div class="address-to" v-text="travel.hotel.address"></div>

                              <div class="booking">
                                <b-button block size="sm" variant="warning" :to="{name: 'travel', params: {id: travel.id, travel: travel}}">حجز</b-button>
                                <div class="wrapper-details-booking">
                                  <h4 class="title">
                                    للحجز يرجى التواصل على
                                  </h4>
                                  <div class="box" v-if="$settings.mobile_1 != null && $settings.mobile_1 != ''">
                                    <span class="icon">
                                      <i class="fas fa-phone"></i>
                                    </span>
                                    <span class="value">
                                      {{ $settings.mobile_1 }}
                                    </span>
                                  </div>
                                  <div class="box" v-if="$settings.mobile_2 != null && $settings.mobile_2 != ''">
                                    <span class="icon">
                                      <i class="fas fa-phone"></i>
                                    </span>
                                    <span class="value">
                                      {{ $settings.mobile_2 }}
                                    </span>
                                  </div>
                                  <div class="box" v-if="$settings.email != null && $settings.email != ''">
                                    <span class="icon">
                                      <i class="fas fa-envelope"></i>
                                    </span>
                                    <span class="value">
                                      <a :href="'mailto:' + $settings.email" v-text="$settings.email"></a>
                                    </span>
                                  </div>
                                </div>
                              </div>
                            </li>
                          </ul>
                        </div>
                      </div>
                    </b-col>
                  </b-row>

                  <div class="show-more-after-section">
                    <router-link :to="{name: 'flight'}">للمزيد</router-link>
                  </div>
                </div>
              </div>
            </b-container>
          </div> -->
          <!-- ./wrapper-fly-offer -->
          <!-- ==================================================================================== -->


          <!-- wrapper-why-we -->
          <div class="wrapper-section wrapper-testimonials">
            <b-container>
              <div class="section-title-center">
                <h2 class="text">
                  <router-link :to="{name: 'images-category', params: {id: 'testimonials'}}">
                    {{ $settings.testimonials_home_title }}
                  </router-link>
                </h2>
              </div>

              <div class="wrapper-carousel-testimonials">
                <div class="loading" v-show="showCarouselTestimonialsLoading">
                  <span class="icon">
                    <i class="fas fa-circle-notch fa-spin"></i>
                  </span>
                </div>
                <div v-show="!showCarouselTestimonialsLoading">
                    <!-- :center="true" -->
                  <carousel
                    v-if="showCarouselTestimonials"
                    class="my-carousel"
                    ref="carouselTestimonials"
                    :margin="15"
                    :autoplay="true"
                    :autoplayHoverPause="true"
                    :autoplaySpeed="500"
                    :autoplayTimeout="3500"
                    :nav="false"
                    :items="itemsCarouselTestimonials"
                    :dots="true"
                    :smartSpeed="300"
                  >
                    <div class="testimonials-box box-flat" v-for="testimonial in testimonialsImages" :key="testimonial.id">
                      <a
                        class="wrapper-link"
                        :href="$domain + testimonial.name"
                        target="_blank"
                        :style="{'background-image': `url('${$domain + testimonial.name}')`}"
                      >
                      </a>
                    </div>
                  </carousel>
                </div>
              </div>
            </b-container>
          </div>
          <!-- ./wrapper-why-we -->
          <!-- ==================================================================================== -->

          <!-- wrapper-why-we -->
          <div class="wrapper-section wrapper-why-we">
            <b-container>
              <div class="section-title-center">
                <h2 class="text">
                  {{ $settings.why_we_home_title }}
                </h2>
              </div>

              <b-row>
                <b-col sm="6" lg="3">
                  <div class="box">
                    <div class="image">
                      <img :src="require('@/assets/images/why-we/customer-service.svg')" >
                    </div>
                    <h4 class="title">دعم العملاء</h4>
                    <p class="desc">
                      خدمة دعم العملاء 24 ساعة
                    </p>
                  </div>
                </b-col>

                <b-col sm="6" lg="3">
                  <div class="box">
                    <div class="image">
                      <img :src="require('@/assets/images/why-we/tag.svg')" >
                    </div>
                    <h4 class="title">أفضل العروض</h4>
                    <p class="desc">
                      أفضل العروض على الرحلات والفنادق
                    </p>
                  </div>
                </b-col>

                <b-col sm="6" lg="3">
                  <div class="box">
                    <div class="image">
                      <img :src="require('@/assets/images/why-we/security.svg')" >
                    </div>
                    <h4 class="title">الأمان والخصوصية</h4>
                    <p class="desc">
                      طريقة السداد آمنة
                      <br>
                      كما نضمن الخصوصية التامة للعميل
                    </p>
                  </div>
                </b-col>

                <b-col sm="6" lg="3">
                  <div class="box">
                    <div class="image">
                      <img :src="require('@/assets/images/why-we/smile.svg')" >
                    </div>
                    <h4 class="title">أراء عملائنا</h4>
                    <p class="desc">
                      أكثر من 7000 عميل راضيين عن الحجوزات
                    </p>
                  </div>
                </b-col>

              </b-row>
            </b-container>
          </div>
          <!-- ./wrapper-why-we -->
          <!-- ==================================================================================== -->





      </div>
      <!-- ./home-content -->

    </section>
    <!-- ./content-page -->

  </div>
</template>



<script>

// import SearchBox from '@/components/SearchBox'
// import TravelBoxsPage from '@/components/TravelBoxsPage'
import carousel from 'vue-owl-carousel2'

export default {
  name: 'home',
  components: {
    // SearchBox,
    // TravelBoxsPage,
    carousel
  },
  data () {
    return {
      imageHeader: '',
      itemsCarouselTravels: 3,
      navCarouselTravels: true,
      dotsCarouselTravels: true,
      showCarouselTravels: true,
      showCarouselTravelsLoading: true,

      itemsCarouselTestimonials: 4,
      showCarouselTestimonials: true,
      showCarouselTestimonialsLoading: true,
      testimonialsImages: [],


      currentTravelsOffer: -1,
      currentDataTravelsOffer: [],
      allDataTravelsOffer: {},

      allDataHotels: [],

      allDataTravelsExternal: [],
      showTravelsExternalLoading: false,

    }
  },

  methods: {
    // search methods

    // changeFromPrice(price) {
    //   this.$store.state.searchForm.filter_from_price = price
    //   this.$store.state.searchForm.page = 1
    //   this.$store.dispatch('getSearchData', [this, this.$store.state.searchForm])
    // },

    // changeToPrice(price) {
    //   this.$store.state.searchForm.filter_to_price = price
    //   this.$store.state.searchForm.page = 1
    //   this.$store.dispatch('getSearchData', [this, this.$store.state.searchForm])
    // },

    // changeStars(star) {
    //   this.$store.state.searchForm.filter_stars = star
    //   this.$store.state.searchForm.page = 1
    //   this.$store.dispatch('getSearchData', [this, this.$store.state.searchForm])
    // },

    // changeHotel(hotelId) {
    //   // this.$store.state.searchForm.filter_hotel = hotelId
    //   this.$store.state.searchForm.page = 1
    //   this.$store.dispatch('getSearchData', [this, this.$store.state.searchForm])
    // },

    // sorting(sortBy) {
    //   this.$store.state.searchForm.sortBy = sortBy
    //   this.$store.state.searchForm.page = 1
    //   this.$store.dispatch('getSearchData', [this, this.$store.state.searchForm])
    // },

    // goToPageInSearch(pageNum) {
    //   this.$store.state.searchForm.page = pageNum
    //   this.$store.dispatch('getSearchData', [this, this.$store.state.searchForm])
    // },
    // ======================================================================================

    // tabs travels offer
    responsiveCarouselTravels() {
      const mdMax = window.matchMedia("(max-width: 991.98px)")
      const smMax = window.matchMedia("(max-width: 767.98px)")
      const mobMax = window.matchMedia("(max-width: 575.98px)")

       if (mobMax.matches) {
          this.itemsCarouselTravels = 1
          this.navCarouselTravels = false
          this.dotsCarouselTravels = true
        } else if (smMax.matches) {
          this.itemsCarouselTravels = 2
          this.navCarouselTravels = false
          this.dotsCarouselTravels = true
        } else if (mdMax.matches) {
          this.itemsCarouselTravels = 2
          this.navCarouselTravels = true
          this.dotsCarouselTravels = true
        } else {
          this.itemsCarouselTravels = 3
          this.navCarouselTravels = true
          this.dotsCarouselTravels = true
        }
    },


    toggleTabTravels (currentTab) {
      if (this.currentTravelsOffer === currentTab) return
      if (!this.$refs.carouselTravels) return

      this.currentTravelsOffer = currentTab
      this.$refs.carouselTravels.$el.classList.remove('active')
      setTimeout(() => {
        this.showCarouselTravels = false

        this.currentDataTravelsOffer = this.allDataTravelsOffer[currentTab]

        this.responsiveCarouselTravels()
        setTimeout(() => this.showCarouselTravels = true)
        setTimeout(() => this.$refs.carouselTravels.$el.classList.add('active'))
      }, 250)
    },


    getTravelsOffer() {
      axios.get('/best-travels-offer').then(response => {
        const data = response.data
        if (typeof data === 'object') {
          this.allDataTravelsOffer = data.tabs
          setTimeout(() => this.toggleTabTravels(0))
          this.showCarouselTravelsLoading = false
        } else {
          setTimeout(() => this.getTravelsOffer(), 500)
        }
      }).catch(error => {
        setTimeout(() => this.getTravelsOffer(), 500)
      })
    },


    getTravelsExternal() {
      const getHotelsAddressFromState =  setInterval(() => {
        if (this.hotelsAddress.length) {
          clearInterval(getHotelsAddressFromState)

          axios.get('/best-travels-external', {params: {hotels_addresses: this.hotelsAddress}}).then(response => {
            const data = response.data
            if (typeof data === 'object') {
              this.allDataTravelsExternal = data.travels
              this.showTravelsExternalLoading = true
            } else {
              setTimeout(() => this.getTravelsExternal(), 500)
            }
          }).catch(error => {
            setTimeout(() => this.getTravelsExternal(), 500)
          })

        }
      }, 300);
    },
    // ======================================================================================

    // ======================================================================================

    responsiveCarouselTestimonials() {
      const mdMax = window.matchMedia("(max-width: 991.98px)")
      const smMax = window.matchMedia("(max-width: 767.98px)")
      const mobMax = window.matchMedia("(max-width: 575.98px)")

       if (mobMax.matches) {
          this.itemsCarouselTestimonials = 1
        } else if (smMax.matches) {
          this.itemsCarouselTestimonials = 2
        } else if (mdMax.matches) {
          this.itemsCarouselTestimonials = 3
        } else {
          this.itemsCarouselTestimonials = 4
        }
    },
    // ======================================================================================

    getTestimonials() {
      axios.get('/images/testimonials').then(response => {
        const data = response.data
        if (typeof data === 'object') {
          this.testimonialsImages = data.images
          this.showCarouselTestimonials = false
          setTimeout(() => {
            this.responsiveCarouselTestimonials()
            setTimeout(() => this.showCarouselTestimonials = true)
          })
          this.showCarouselTestimonialsLoading = false
        } else {
          setTimeout(() => this.getTestimonials(), 500)
        }
      }).catch(error => {
        setTimeout(() => this.getTestimonials(), 500)
      })
    },
    // ======================================================================================
  },

  created () {
    this.$nextTick(() => {
      this.imageHeader = ''
      setTimeout(() => {
        this.imageHeader = this.$settings.home_page_bg != null ? this.$domain + this.$settings.home_page_bg : ''
      })

      // get travels offer from db
      this.getTravelsOffer()

      // this.getTravelsExternal()


      this.getTestimonials()
    })
  },


  watch: {
    // '$store.state.showSearchResult' (newVal) {
    //   if (!newVal) {
    //     this.$nextTick(() => {
    //       this.currentTravelsOffer = 2
    //       this.toggleTabTravels(1)
    //     })
    //   }
    // },
  },

  computed: {
    hotelsAddress() {
      return this.$store.state.hotels.map(hotel => {
        return hotel.address
      }).filter((value, index, self) => {
          return self.indexOf(value) === index;
      })
    },

    // firstCategoryExternal () {
    //   const categories = []
    //   this.$menuList.forEach(program => {
    //     program.categories.forEach(category => {
    //       categories.push(category)
    //     })
    //   })
    //   return categories.find(item => {
    //     return item.type == 4
    //   })
    // }
  },

  // beforeRouteEnter (to, from, next) {
  //   next(vm => {
  //     if (to.query.search && to.query.search == 'search' && Object.keys(to.query).length > 1) {
  //       vm.$store.dispatch('updateSearchForm', to.query)
  //       if (!to.params.search) {
  //         vm.$store.dispatch('getSearchData', [vm, to.query])
  //       }
  //     } else {
  //       vm.$store.state.showSearchResult = false
  //       vm.$store.state.showLoading = false
  //     }
  //   })
  // }
}
</script>

