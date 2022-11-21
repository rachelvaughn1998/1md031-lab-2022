<template>
  <div id="aburger">
    <h3> {{ burger.name }} </h3>
    <img v-bind:src="burger.img"
      alt="jalapeno_img" title="jalapeno_img" style="width: 250px; height:150px"  >
    <dl> <dt>Ingredients: </dt>
      <dd> {{burger.ingredient1}} </dd> 
      <dd> {{burger.ingredient2}} </dd>
      <dd> {{burger.ingredient3}} </dd>
    </dl> 

    <dl> <dd> {{burger.kCal}} kCal </dd> 
      <dd class="lactose">  {{ burger.lactose ? "lactose" : "no lactose"}} </dd>
      <dd class="gluten">  {{ burger.gluten ? "gluten" : "no gluten"}} </dd> </dl> 
      
    <h4> Beer suggestions: </h4> 
    <ul> <li>IPA</li> <li> Lager</li> </ul>
    <p> {{ amountOrdered }} </p>
    <button v-on:click=increase type="button">
      +
    </button>
    <button v-on:click=decrease type="button">
      -
    </button>
  </div>
</template>
  
<script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    }, 

    data: function () {
      return {
      amountOrdered: 0,
      }
    }, 

    methods: {
        increase: function() {
          this.amountOrdered += 1
          this.$emit('orderedBurger', {name: this.burger.name, 
            amount: this.amountOrdered 
          }
        );
      },
      
      decrease: function() {
        if (this.amountOrdered > 0)
        this.amountOrdered -= 1
      }      
    }
  }
</script>
  
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

#aburger {
    background-color: rgb(0, 0, 0);
    color: #fff;
    border-radius: 0px;
    padding: 1px;
    font-size: 100%;
}
</style>
  