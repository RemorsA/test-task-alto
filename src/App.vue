<template>
  <div class="container">
    <div class="container-items">

      <div class="item" v-for="item in items" :key="item.id" > 
        <div class="container-item">

          <img class="cover" :width="item.image_width" :height="item.image_height" :src="item.image_url">
          <div class="availability" v-if="item.availability === 1" :style="`background-color: #70C486FF`">В наличии</div>
          <div class="availability" v-if="item.availability === 0" :style="`background-color: #969593FF`">Под заказ</div>
          <div class="container-name">
            <a href="#" class="name">{{item.name}}</a>
          </div>
          <div class="container-price">
            <h3 class="price">{{item.price}} ₽</h3>
          </div>
          <div class="container-color">
            <h5 class="color">Цвет - {{item.color}}</h5>
          </div>
          <div class="container-info" >
            <h5 class="info" @click="openClose_shortDesc = !openClose_shortDesc">Краткое описание товара</h5>
            <div class="text">
              <h5 v-if="openClose_shortDesc">{{item.short_desc}}</h5>
            </div>
          </div>
          <div class="btn-buy">
            В корзину
            <div class="icon"></div>
          </div>

        </div>
      </div>
      
    </div>
    <div class="container-pagination">
      <div class="pagination" v-for="p in pageCount" :key="p">
        <div @click="setPage(p)" class="pages">{{p}}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [],
      openClose_shortDesc: false,
      page: 1,
      productsPerPage: 4,
      pageCount: 1,
    }
  },
  methods: {
    async fetchData() {
      const skip = (this.page - 1) * this.productsPerPage
      const result = await fetch(`http://testtask.alto.codes/front-products.php?skip=${skip}`)
      const data  = await result.json()
      this.items = data.products
      this.pageCount = Math.ceil(data.totalCount / this.productsPerPage)
    },
    setPage(p) {
      this.page = p
      this.fetchData()
    }
  },
  mounted() {
    this.fetchData()
  }
}
</script>

<style>
  @import './styles/style.css';
</style>
