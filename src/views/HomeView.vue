<template>
    <div>
      <img id="header-image" src="https://as1.ftcdn.net/v2/jpg/03/08/47/58/1000_F_308475881_m7o04LTdIbwWWnSJDqjFTjOnWvU0yj8b.jpg" > 
      <h1 id="headline"> Welcome to Rachels burger restaurant! </h1> 

      <div>
        <div class="wrapper">
          <Burger v-for="burger in burgers" 
                  v-bind:burger="burger" 
                  v-bind:key="burger.name"
                  v-on:orderedBurger="addToOrder($event)"/>
        </div>

        <div id="map-wrapper">
          <div id="map" v-on:click="setLocation">
            <div id="T-position" v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}"> T </div>
          </div>
        </div>
      </div>

    <section id="order">  
      <form>
        <h2> Order information: </h2> 
          <p>
            <label for="firstname">Name</label><br>
            <input type="text" id="firstname" v-model="firstname" required="required" placeholder="Name">
          </p>

          <p>
            <label for="Email">Email</label><br>
            <input type="email" id="Email" v-model="email" required="required" placeholder="E-mail address">
          </p>

              <legend>Select gender:</legend>

                <input type="radio" id="contactChoice1" v-model="Female" value="Female" />
                <label for="genderChoice1">Female</label>
          
                <input type="radio" id="contactChoice2" v-model="Male" value="Male" />
                <label for="genderChoice2">Male</label>

                <input type="radio" id="contactChoice2" v-model="Other" value="Other" />
                <label for="genderChoice3">Other</label>

          <p>
            <label for="Payment Method">Payment Method</label>
            <select id="Payment Method" name="rcp" >
                <option selected=selected>Credit Card</option>
                <option>Swish</option>
                <option>Klarna</option>
            </select>
          </p>

            <button v-on:click=printOrder type="submit">
              <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Mr._Smiley_Face.svg/1024px-Mr._Smiley_Face.svg.png" style="width: 20px; height: 20px;"  >
              Submit your order!
            </button>        
      </form>
    </section>
  </div> 
</template>

<script>
import menu from '../assets/menu.json'
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'

const socket = io();

/*
//Object Constructor Function
function MenuItem(name, ingredient1, ingredient2, ingredient3, kCal, gluten, lactose, img) {
    this.ingredient1 = ingredient1;
    this.ingredient2 = ingredient2;
    this.ingredient3 = ingredient3;
    this.name = name; 
    this.kCal = kCal;
    this.gluten = gluten;
    this.lactose = lactose;
    this.img = img; 
}

const burger1 = new MenuItem('Jalapeno Burger', "Chicken", "Jalapenos","Hot Sauce", 120, true, false, "https://d2v9mhsiek5lbq.cloudfront.net/eyJidWNrZXQiOiJsb21hLW1lZGlhLXVrIiwia2V5IjoiZm9vZG5ldHdvcmstaW1hZ2UtZjk1YzdkZTYtYzQ0NS00YTBiLWI2MWYtY2MwYTdlYjczN2U1LmpwZWciLCJlZGl0cyI6eyJyZXNpemUiOnsiZml0IjoiY292ZXIiLCJ3aWR0aCI6NzUwLCJoZWlnaHQiOjQyMn0sImpwZWciOnsicXVhbGl0eSI6NTUsInByb2dyZXNzaXZlIjp0cnVlfX19"); 
const burger2 = new MenuItem('Rachels Burger', "Beef", "Tomatoes", "Lettuce", 240, false, false, "https://assets.biggreenegg.eu/app/uploads/2019/03/28145521/topimage-classic-hamburger-2019m04-800x534-600x401.jpg"); 
const burger3 = new MenuItem('Hilmas Burger', "Halloumi", "Yolk","Olives", 480, true, true, "https://image.elle.se/grilla-hamburgare-ibl-4822649.jpg?imageId=4822649&width=1060&height=605&compression=80");
const burger_array = [burger1, burger2, burger3];
console.log(burger_array)
*/

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu, 
      orderedBurgers: {},
      location: { x: 0,
            y: 0
          }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset= {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
                  },
    setLocation: function(event) {
       var offset = {x: event.currentTarget.getBoundingClientRect().left,
          y: event.currentTarget.getBoundingClientRect().top};
            this.location = {x:event.clientX - 10 - offset.x,
            y: event.clientY - 10 - offset.y
            }
          },
      printOrder: function () {
        socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y,
                                           name: this.firstname,  
                                           email: this.email,
                                          },
                                orderItems: this.orderedBurgers,
                              }
                       );
        console.log(this.firstname, this.email, this.orderedBurgers, this.location)
      },
        addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
  }
}

</script>
<style>

#headline {
    position: absolute; 
    padding-left: 100px;
    padding-left: 30px;
    padding-bottom: 150px; 
    margin-top: -150px;
}

#header-image{
    position: relative;
    opacity: 0.6;
    width: 100%; 
    height: 250px;
}


  .head {
    overflow: hidden;
    width: auto;
}

section { 
  width: auto;
  border: 3px dashed rgb(0, 0, 0);
  padding-left: 50px;
  padding-bottom: 20px;
  margin-bottom: 25px;
}

div { 
    padding-left: 100px;
    padding: 1px;
}

body {
    font-family: "Times New Roman";
    font-weight: normal;
    font-style: normal;
 }

 #idname {
    text-transform: uppercase;
 }

 .lactose, .gluten {
  font-weight: bold;
 }
 
 #burger {
    background-color: black;
    color: white;
 } 

 button:hover {
    cursor: pointer;
 }

 .wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(3, 1fr);
    background-color: rgb(0, 0, 0);
    padding-left: 50px;
    padding-bottom: 20px;
}

.box {
    background-color: rgb(0, 0, 0);
    color: rgb(255, 255, 255);
    border-radius: 0px;
    padding: 50px;
    font-size: 100%;
}

#map-wrapper {
  overflow: scroll;
  width: 730px;
  height: 400px;
  margin-left: 300px;
  margin-right: 300px;
}

#map  {
    background: url("../../public/img/polacks.jpg");
    width: 1920px;
    height: 1078px;
    position: relative;
  }

  #T-position  {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
</style>