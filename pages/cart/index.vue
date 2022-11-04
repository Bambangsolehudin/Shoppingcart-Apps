<template>
  <div class="container">
    <div class="row justify-content-center">
        <div class="col-10">
          <div class="card my-4">
            <div class="card-body">
                <h4 class="my-4">Shooping Cart</h4>
                
                <table class="table">
                    <thead>
                        <tr>
                        <th scope="col">Name</th>
                        <th scope="col">QTY</th>
                        <th scope="col">Price</th>
                        <th scope="col">SubTotal</th>
                        <th scope="col"> Aksi </th>
                        </tr>
                    </thead>
                    <tbody>
                      <template v-if="cart != null">
                        <tr v-for="(item, index) in cart" :key="index">
                            <td>{{ item.title }}</td>
                            <td> {{ item.qty }} </td>
                            <td> $ {{ item.priceFix }} </td>
                            <td> $ {{ item.priceFix * item.qty }} </td>
                            <td> <a href="" class="btn btn-sm btn-danger" @click.prevent="deleteLocalStorage(item.id)">Hapus</a> </td>
                        </tr>
                      </template>
                    </tbody>
                </table>
            </div>
            <span class="border-top mx-4 my-4"></span>
            <div class="row px-4">
              <div class="col-12 d-flex justify-content-between">
                <h4 class="">Total</h4>
                <h4 class=""> $ {{ getTotal() }}</h4>
              </div>
            </div>
            <div class="row px-4 my-4 row justify-content-center">
              <div class="col-12">
                <a href="" class="btn text-white bg-lightblue btn-lg bg-primary w-100">
                    <i class="fas fa-shopping-cart text-white"></i>
                    Checkout
                </a>
              </div>
            </div>
          </div>
        </div>
    </div>

  </div>
</template>

<script>
export default {
    data(){
        return{
            cart: []
        }
    },
    methods:{
      deleteLocalStorage(id){
        let old = JSON.parse(localStorage.getItem("cart")) || []
        if(old){
          let newData = old.filter(e=> e.id != id)
          localStorage.setItem("cart", JSON.stringify(newData))
        }
        this.getData()
      },
      getTotal(){
        let dataTotal = []
        this.cart.map(e => {
          dataTotal.push(e.sub_total)
        })
        return dataTotal.reduce((a, b) => a + b, 0)
      },
      getData(){
        if(JSON.parse(localStorage.getItem("cart"))) {
          let data = JSON.parse(localStorage.getItem("cart"))
          data = data.filter(e => e.qty != 0)
          localStorage.setItem("cart", JSON.stringify(data))
          this.cart = data
         
        }
      }
    },

    mounted(){
      this.getData()
      console.log('data',this.cart);
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
}

.bg-lightblue{
  background: linear-gradient(311.55deg, #01ACC4 7.49%, #29C7CF 50.69%, #4CCBCE 111.84%) !important;
border-radius: 15px;
}
</style>