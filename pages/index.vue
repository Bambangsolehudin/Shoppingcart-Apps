<template>
  <div class="container">
    <nav class="navbar navbar-light bg-light">
      <a class="navbar-brand">Shoppingcart-App</a>
      <form class="form-inline">
        <NuxtLink to="/cart">
          <i class="fas fa-shopping-cart text-primary icon-3x mx-2"></i><span class="badge badge-warning">{{ getCart.length }}</span> 
        </NuxtLink>
      </form>
    </nav>
    <div class="row justify-content-center">
    
      <template v-for="(product, index) in products">
        <div class="col-4 p-4" :key="index">
            <div class="card card-tes">
              <img class="card-img-top" :src="product.images[0]" alt="Card image cap">
              <div class="desc">
                <p class="text-white px-4 pt-1">{{ product.category }}</p>
              </div>
              <div class="card-body row">
               <div class="col-12">
                <p class="text-secondary h6">{{ product.title }}</p>
                 <p class="text-dark">{{product.description}}</p>
                 <h5>Rp. {{ product.discountPercentage ? product.price - (product.price * product.discountPercentage/100) : product.discountPercentage }} $</h5>
                 <div class="d-flex">
                    <p class="bg-danger1 px-1 py-1 text-danger">{{ product.discountPercentage }} %</p>
                    <div class="" v-if="product.discountPercentage">
                      <div class="p-1 px-2" style="line-height: 19px;">
                        <p class="sub-text">Rp. {{ product.price }} $</p>
                      </div>
                    </div>
                  </div>

                </div>
                <div class="col-12">
                  <i class="fas fa-star text-warning mt-1 mr-1"></i> <span class="text-warning"> {{ product.rating }} </span>
                </div>
                <div class="col-12 pt-4">
                  <div class="d-flex justify-content-center align-items-center" v-if="getQty(product.id) && getQtyValue(product.id) > 0"> 
                    <button class="btn btn-custom" @click.prevent="minusQty(product.id)"> <span> - </span> </button>   
                      <span class="mx-4"> {{ getQtyValue(product.id) }}  </span>  
                    <button class="btn btn-custom " @click.prevent="plusQty(product.id)"><span> + </span></button>
                  </div>
                  <div v-else>
                    <a  href="" @click.prevent="addToCart(product, index)" class="btn text-white bg-lightblue btn-lg bg-primary w-100">
                      <i class="fas fa-shopping-cart text-white icon-1x"></i> <span class="font-weight-bold" style="font-size:14px;"> Add To Cart  </span> 
                    </a>
                  </div>
                </div>
              </div>
            </div>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data(){
    return{
      products : [],
      cart: [],
      getCart : []
    }
  },
  methods:{
    plusQty(id){
      this.getCart.map((e, index) => {
        if(e.id == id){
          this.getCart[index].qty += 1
        }
      })
      localStorage.setItem("cart", JSON.stringify(this.getCart))
      this.getData()

    },
    minusQty(id){
      this.getCart.map((e, index) => {
        if(e.id == id){
          if(this.getCart[index].qty > 0){
            this.getCart[index].qty -= 1
          }
        }
      })
      localStorage.setItem("cart", JSON.stringify(this.getCart))
      this.getData()

    },

    getQty(id){
      let isAvailable = this.getCart.find(e => e.id == id)
      if(isAvailable){
        return true
      }
    },
    getQtyValue(id){
      let isAvailable = this.getCart.find(e => e.id == id)
      if(isAvailable){
        return isAvailable.qty
      }
    },
    getData(){
       axios.get('https://dummyjson.com/products').then(data => 
        {
          this.products = data.data.products 
          console.log(data.data.products);
        })
        let data = JSON.parse(localStorage.getItem("cart")) || [] 
        
          data = data.filter(e => e.qty != 0)
          localStorage.setItem("cart", JSON.stringify(data))
          this.getCart = data
    },
    addToCart(product){
      product.priceFix = product.discountPercentage ? product.price - (product.price * product.discountPercentage/100) : product.price
      product.qty = 1
      product.sub_total = product.discountPercentage ? product.price - (product.price * product.discountPercentage/100) : product.price
      
      let old = JSON.parse(localStorage.getItem("cart")) || []

      if(old){
        let isSame = old.find(e => e.id == product.id) 
        if(isSame){
          old.forEach((e, i) => {
            if(e.id == product.id){
              old[i].qty += 1
              old[i].sub_total = old[i].sub_total * old[i].qty
            }
          })
        }else{
          old.push(product)
        }
        localStorage.setItem("cart", JSON.stringify(old))
      }else{
        localStorage.setItem("cart", JSON.stringify([product]))
      }

      this.$router.push("/cart")
    }
  },
  mounted(){
   this.getData()
  }
}

</script>

<style scoped>
.card{
  background: #FFFFFF !important;
  box-shadow: 0px 15px 50px rgba(108, 146, 181, 0.15) !important;
  border-radius: 25px !important;
}

img{
  border-radius: 25px 25px 0px 0px;
  width: 100%;
  height: 200px;
}

.desc{
  background: #31C7CE;
  border-radius: 0px 0px 45px 0px;
}

.bg-danger1{
  background: #FFDBE2;
  border-radius: 4px;
}



.sub-text{
  -webkit-text-decoration-line: line-through; /* Safari */
   text-decoration-line: line-through; 
   color: #B6B6B6;
}

.bg-lightblue{
  background: linear-gradient(311.55deg, #01ACC4 7.49%, #29C7CF 50.69%, #4CCBCE 111.84%) !important;
border-radius: 15px;
}

.btn-custom{
  background: #DAF6F4 !important;
  width: 53.33px !important;
  height: 47px !important;
  color: #5BCCD4 !important;
}



</style>