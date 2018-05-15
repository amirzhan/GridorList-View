<template>
  <div :class="['card', isList ? 'flex-row justify-content-between align-items-center flex-nowrap' : 'grid-view']">
    <router-link :to="productLink" :class="['card-title', isList ? 'card-title_list-view' : 'card-title_grid-view']">{{title}}</router-link>
    <div :class="['card-image', isList ? 'card-image_list-view' : 'card-image_grid-view']">
      <router-link class="card-image_link" :to="productLink">
        <img v-if="hash" :src="getImageUrl(hash)" :alt="title">
        <img v-else src="/images/image-placeholder.svg" :alt="title">
      </router-link>
    </div>
    <div :class="['card-body', isList ? 'card-body_list-view' : 'card-body_grid-view']">
      <div v-if="isList" class="list-view-icons">
        <button class="card-body_actions-item card-body_actions-item__make_starred">
          <i class="icon icon-make-starred"></i>
        </button>
        <button class="card-body_actions-item card-body_actions-item__compare">
          <i class="icon icon-compare"></i>
        </button>
      </div>
      <router-link v-if="hasPrice && !isList" :class="['card-body_price', isList ? 'card-body_price_list-view' : 'card-body_price_grid-view']" :to="productLink"><span>{{priceThousand}}</span> {{priceHundred}} ₸</router-link>
      <router-link v-if="hasPrice" :class="['card-body_oneclick', isList ? 'card-body_oneclick_list-view' : 'card-body_oneclick_grid-view']" :to="productLink">купить в один клик</router-link>
      <div :class="['card-body_actions', {'flex-column card-body_actions_list-view': isList}]">
        <router-link v-if="hasPrice && isList" :class="['card-body_price', isList ? 'card-body_price_list-view' : 'card-body_price_grid-view']" :to="productLink"><span>{{priceThousand}}</span> {{priceHundred}} ₸</router-link>
        <button v-if="!isList" class="card-body_actions-item card-body_actions-item__make_starred">
          <i class="icon icon-make-starred"></i>
        </button>
        <button :class="['card-body_actions-item', 'card-body_actions-item__add-to-cart', isList ? 'card-body_actions__add-to-cart_list-view' : 'card-body_actions__add-to-cart_grid-view']" @click="addToCart">В корзину</button>
        <button v-if="!isList" class="card-body_actions-item card-body_actions-item__compare">
          <i class="icon icon-compare"></i>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    productId: {
      type: Number,
      required: true
    },
    categoryName: String,
    categoryId: Number,
    title: {
      type: String,
      required: true
    },
    hash: String,
    text: String,
    price: Number,
    featured: Boolean,
    offers: Array,
    isList: Boolean
  },
  methods: {
    getImageUrl (hash) {
      return hash.indexOf('http') !== -1 ? hash : 'https://file-service.ktstore.kz/files/' + hash + '/170x170'
    },
    addToCart () {
      let productUnitId = this.offers[0].id
      let merchantId = this.offers[0].merchantId
      this.$toasted.info('Товар успешно добавлен в корзину', {
        duration: 5000,
        icon: 'check',
        iconPack: 'fontawesome',
        action: [
          {
            text: 'Перейти в корзину',
            onClick: () => {
              this.$router.push('/cart')
            }
          }
        ]
      })
    }
  },
  computed: {
    productLink () {
      return {path: '/product/' + this.productId}
    },
    hasOffers () {
      if (this.offers && this.offers !== undefined && this.offers.length !== 0) {
        return true
      }
      return false
    },
    hasPrice () {
      if (this.price && this.price !== undefined) {
        return true
      }
      return false
    },
    priceThousand () {
      return this.price.toLocaleString().slice(0, -4)
    },
    priceHundred () {
      return this.price.toLocaleString().slice(-3)
    }
  }
}
</script>
<style scoped>
.card-body {
  padding: 0;
}
.card-title_list-view {
  margin: 0 10px;
}
</style>
