<template>
  <div class="container">
    <button @click="isList=!isList">Toggle</button>
    <div class="row">
      <div :class="isList ? 'col-md-12 mb-4' : 'col-md-3 mb-4'" v-for="(product, index) in products" :key="index">
        <Card
            :productId="product.id"
            :title="product.nameRus"
            :hash="product.photo[0]"
            :categoryId="product.categoryId"
            :categoryName="product.categoryNameRus"
            :price="product.price"
            :offers="product.units"
            :isList="isList"
          />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Card from '@/components/Card'

export default {
  name: 'Home',
  data () {
    return {
      products: [],
      isList: true
    }
  },
  components: {
    Card
  },
  beforeMount () {
    axios.get('https://shop-service.ktstore.kz/product/top-random')
      .then(response => {
        this.products = response.data
      })
  }
}
</script>
