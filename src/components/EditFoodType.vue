<template>
  <div class="main col-sm-12 col-md-4 p-0">
    <h3>Edit Food type</h3>
    <button class="btn btn-danger float-right mb-2" @click="deleteFoodType">Delete</button>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">Name</th>
          <th scope="col">Reorder threshold</th>
        </tr>
        </thead>
        <tbody>
          <tr>
            <td><input type="text" class="w-100" :value="getName" id="NameInput"></td>  
            <td><input type="text" class="w-50" :value="getReorderThreshold" id="ReorderThresholdInput"></td>                      
          </tr>
        </tbody>
      </table>
    <router-link to="/list-foodtypes" tag="button" class="btn">Cancel</router-link>
    <button class="btn btn-primary" @click.prevent="updateFoodType">Submit</button>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      currentFoodType: null
    }
  },
  computed: {
    getName: function() {
      if (this.currentFoodType !== null) {
        return this.currentFoodType.name
      } else {
        return ""
      }
    },
    getReorderThreshold: function() {
      if (this.currentFoodType !== null) {
        return this.currentFoodType.reorderThreshold
      } else {
        return 0
      }
    }
  },
  methods: {
    updateFoodType: function() {
      var payload = {}
      payload.currentFoodTypeId = this.currentFoodType['key']
      payload.newName = document.getElementById("NameInput").value
      payload.newReorderThreshold = parseFloat(document.getElementById("ReorderThresholdInput").value)

      this.$store.commit('updateFoodType', payload)
      this.$router.replace('/list-foodtypes')
    },
    deleteFoodType: function() {
      var res = confirm("Are you sure you want to delete this food type?")
      var id = this.currentFoodType['key']
      if (res)  {
        var payload = { currentFoodTypeId: id}
        this.$store.commit('deleteFoodType', payload)
        this.$router.replace('/list-foodtypes')
      }
    }
  },
  beforeRouteEnter (to, from, next) {
    next(vm => {
      if (vm.$store.getters.getCurrentUser == null) {
        vm.$router.replace("/login")
      } else {      
        var foods = vm.$store.getters.getFoods

        // Find the item
        var items = foods.filter(function (obj) { 
            return obj['key'] == vm.$route.params.foodtypeid;
        });
        
        if (items.length > 0) {
          vm.currentFoodType = items[0]
        }
      }
    })
  }
}
</script>

<style scoped>
.main {
  margin-top: 50px;
}
table {
  margin-top: 20px;
}
</style>


