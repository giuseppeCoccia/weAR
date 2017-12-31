<template>
  <el-card class="box-card">
    <el-table height="500" :data="shopcartData">
      <el-table-column
        label="Item"
        >
        <template slot-scope="scope">
          <div class="shopRowWrapper">
            <div class="itemWrapper">
              <img width="70px" height="70px" :src="scope.row.assets[0]" :alt="scope.row.alt" :href="scope.row.href"/>
            </div>
            <div class="itemWrapper">
              <a style="text-decoration: none" :href="scope.row.href"><span class="itemName">{{ scope.row.name }}</span></a>
              <span class="itemDescription">{{ scope.row.description }}</span>
            </div>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        label="Price"
        width="100">
        <template slot-scope="scope">
          <div class="shopRowWrapper">
            <span>{{ scope.row.price }}</span>
            <i>€</i>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        label="Quantity"
        width="150">
        <template slot-scope="scope">
          <div class="shopRowWrapper">
            <el-input-number v-model="quantities[scope.row.id]" @change="changeQuantity(scope.row.id, $event)" size="mini" controls-position="right" min="0"></el-input-number>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        label="Remove"
        width="70">
        <template slot-scope="scope">
          <div class="shopRowWrapper">
            <el-button size="mini" type="danger" icon="el-icon-remove"
            @click="deleteCartItem(scope.row.id)"></el-button>
          </div>
        </template>
      </el-table-column>
    </el-table>

    <div class="shopCartTableFooter" v-if="loggedIn && shopcartData.length > 0">
      <h1>Total: {{ total.toFixed(2) }}€</h1>
      <el-button @click="visible = true">Checkout</el-button>
      <el-dialog
        title="Confirm purchase"
        :visible.sync="visible"
        :before-close="handleClose">
        <span>Do you want to confirm your purchases?</span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="visible = false">Cancel</el-button>
          <el-button type="primary" @click="visible = false">Confirm</el-button>
        </span>
      </el-dialog>
    </div>
  </el-card>
</template>

<script>
export default {
  name: 'wear-shop-cart',
  props: ['visible'],
  data: function() {
    return {
      shopcartData: getShopCart(),
      quantities: []
    }
  },
  created: function() {
    //initialize quantities with localStorage content
    //localStorage is not responsive, we need an object defined in the vue instance
    for(var i = 0; i < this.shopcartData.length; i++) {
      this.quantities[this.shopcartData[i].id] = localStorage[this.shopcartData[i].id];
    }
  },
  methods: {
    changeQuantity(id, value) {
      localStorage[id] = value;
    },
    deleteCartItem(id) {
      deleteCartItem(id);
      this.shopcartData = getShopCart();
    }
  },
  computed: {
    total: function() {
      var total;
      for(var i = 0, total = 0; i < this.shopcartData.length; i++) {
        total += this.shopcartData[i].price*this.quantities[this.shopcartData[i].id];
      }
      return total;
    },
    loggedIn: function() {
      if(localStorage.loggedIn == 1)
        return true;
      else return false;
    }
  }
}
</script>

<style>
.shopRowWrapper {
  float: left;
}
.shopRowWrapper .itemWrapper {
  display: inline-block;
  height: 100%;
}
.shopRowWrapper .itemWrapper .itemName {
  font-size: 22px;
}
.shopRowWrapper .itemWrapper .itemDescription {
  font-size: 12px;
}
.shopCartTableFooter {
  float: right;
  margin-bottom: 10px;
}
</style>