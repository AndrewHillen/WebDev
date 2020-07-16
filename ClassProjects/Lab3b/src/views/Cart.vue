<template>
<div class="wrapper">
  <div class="cart" v-if="NotEmpty">
    <div class="item" v-for="item in $root.$data.cart" :key="item.id">
      <div class="info">
        <h1>{{item.name}}</h1>
        <p>x{{item.quantity}}</p>
      </div>
      <div class="image">
        <img :src="'/images/products/'+item.image">
      </div>
      <div class="price">
        <h2>{{TotalPrice(item)}}</h2>
        <!-- Adding an a click to the button -->
        <button class="auto" @click="Remove(item)">Remove 1</button>
      </div>
    </div>
  </div>
  <div v-else>
    <p>Your cart is empty</p>
  </div>
</div>
</template>


<script>
export default {
  name: 'Cart',
computed:
{
  NotEmpty()
  {
    return this.$root.$data.cart.length > 0;
  },
},
methods:
{
  TotalPrice(item)
  {
    let tempItem = Object.assign({},item);
    var price = tempItem.price.substring(1);
    price = Number(price);
    return Math.round(price * item.quantity * 100) / 100;
  },
  Remove(item)
  {
    if(item.quantity === 1)
    {
      var index;
      for(let i = 0; i < this.$root.$data.cart.length; i++)
      {
        if(item.id === this.$root.$data.cart[i].id)
        {
          index = i;
          break;
        }
      }
      this.$root.$data.cart.splice(index, 1);
    }
    else
    {
      item.quantity -= 1;
    }
  },
},

}
</script>

<style scoped>
.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}

.products {
  margin-top: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.product {
  margin: 10px;
  margin-top: 50px;
  width: 200px;
}

.product img {
  border: 2px solid #333;
  height: 250px;
  width: 200px;
  object-fit: cover;
}

.product .image {
  display: flex;
  justify-content: center;
  margin-bottom: 5px;
}

.info {
  background: #F2921D;
  color: #000;
  padding: 10px 30px;
  height: 80px;
}

.info h1 {
  font-size: 16px;
}

.info h2 {
  font-size: 14px;
}

.info p {
  margin: 0px;
  font-size: 10px;
}


.price {
  display: flex;
}

button {
  height: 50px;
  background: #000;
  color: white;
  border: none;
}

.auto {
  margin-left: auto;
}
</style>
