<template>
  <div class="wrapper">
    <div class="section">
      <div class="md-size-50 md-small-size-70 md-xsmall-size-100">

        <div class="section">
        <div class="container">

        </div>
      </div>

        <div class="container">
         <h1 class="text-success title">green light for your order, thank you!</h1>
         <Cart class="md-layout-item" :order="listOrder" @click="removeItem(item)" :total="totalPrice" ></Cart>

        </div>


      <div class="section section-examples">
        <div class="container text-center">

        </div>
      </div>




        <div class="section section-examples">
        <div class="container text-center">

        </div>
      </div>

     </div>
    </div>
  </div>
</template>

<script>


import Cart from '@/components/Cart'
export default {
  data(){
    return {
      orders : {}
    }
  },
  components:{
    Cart

  }, mounted: async function(){

    this.listOrder

    let config = {
          headers: {
            'Authorization': `Bearer ${window.localStorage.getItem("token")}`
          }
        }

    let userId = window.localStorage.getItem("id")
    const response = await this.$axios.get(`http://localhost:8082/orders/${userId}` , config)
    this.orders = response.data
    console.log(this.orders)
  },
  computed: {
    listOrder() {
      return this.$store.state.listOrder
    },
    totalPrice() {
      if (this.listOrder.length === 0) {
        return 0;
      }
      let total = this.listOrder.map(item => item.price * item.quantity);
      total = total.reduce((sum, current) => sum + current);
      return total;
    }
  },
  methods:{

    async sendOrder(){

        let config = {
           headers: {'Authorization': `Bearer ${window.localStorage.getItem("token")}`}
        }
        try {
          let response = await this.$axios.post("http://localhost:8082/orders", this.listOrder , config)
          this.$router.push('/order')
        } catch(err) {
          alert('tienes que registrarte para poder comprar :)')
          console.log('no se conecta')
        }
    }

  },
}
</script>

