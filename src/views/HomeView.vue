<template>
  <div>
    <header class="welcome">
     
         <img id="image" src="https://i.pinimg.com/originals/3f/8c/c4/3f8cc4fe23951bc32de7e97267706916.jpg" alt="span" title="Välkommen">
         <h1 id="title">Välkommen till BEANS&BURGERS</h1>

        </header>
        <main>

        <div id="burgers"> 
                <h2> Välj din hamburgare </h2>
                <nav> Hamburgarmenu </nav>

            <div class = "wrapper">
              
              <Burger v-for="burger in burgers" 
                  v-bind:burger = "burger"
                  v-bind:key = "burger.name"
                  v-on:orderedBurger = "addToOrder($event)"/>
              
  
          </div>
         </div>
            
          <section id="contact">

                <h2> Kundinformation </h2>
                <nav> Nödvändig information </nav>

                 <form>
                        <p>
                            <label for="Namn">Namn</label><br>
                            <input type="text" id="name" v-model="name" required="required" placeholder="För- och efternamn">
                        </p>
                        <p>
                            <label for="E-mail">E-mail</label><br>
                            <input type="email" id="E-mail adress" v-model="Email" placeholder="E-mail adress">
                        </p>
                   <!--  <p>
                           <label for="Gata">Gata</label><br>
                            <input type="text" id="Gatunamn" v-model="Gatunamn" required="required" placeholder="Gatunamn">
                        </p>
                        <p>
                            <label for="Gatunummer">Gatunummer</label><br>
                            <input type="number" id="Gatunummer" v-model="Gatunummer" required="required" placeholder="Gatunummer">
                        </p> </p>-->

                        <p>
                            <label for="Betalmedel">Betalmedel</label><br>
                            <select id="Välj" v-model="btm">
                                <option selected="selected" >Betalkort</option>
                                <option>Swish</option>
                                <option>Klarna</option>
                                <option>Paypal</option>
                                <option>Faktura</option>
                            </select>
                         </p>
                         <label for="Kön">Kön</label><br>
                        
                            <input type="radio" id="Kvinna" v-model="kon" value="Kvinna" required="required">
                            <label for="Kvinna">Kvinna</label><br>
                            <input type="radio" id="Man" v-model="kon" value="Man" required="required">
                            <label for="Man">Man</label><br>
                            <input type="radio" id="Icke-binär" v-model="kon" value="Icke-binär" required="required">
                            <label for="Icke-binär">Icke-binär</label><br>
                            <input type="radio" id="Ange ej" v-model="kon" value="Ange ej" required="required">
                            <label for="Ange ej">Ange ej</label><br>


                 </form>

            </section>

            <div id="scroller">
              <div id="map" v-on:click="setLocation">
                <div v-bind:style ="{left:location.x+'px', top: location.y+'px'}">
                    T
                </div>
             
              </div>
           </div>

            <button v-on:click = "printOrder" type="submit">
                <img src="https://static.vecteezy.com/ti/gratis-vektor/p3/4618623-skicka-meddelande-glyph-ikon-siluett-symbol-e-postbrev-negativt-rymden-vektor-isolerad-illustration-vector.jpg" style="width: 40px; height: 40px;" >
                Slutför beställning!
            </button>

            

         </main>
         <hr>
         <footer>
            &COPY; 2022 BEANS&BURGERS INC
        </footer>

  
  </div>

    
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();
//const menu = JSON.parse()

//console.log(menu)

//function MenuItem(burgername, img, kcal, glu, lac) {
//    this.name = burgername; // The *this* keyword refers to the object itself
//    this.url = img;
//    this.calories = kcal;
//    this.gluten = glu;
//    this.lactose = lac;
//    }

//const burg1 = new MenuItem('Beany delicious', "https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_1200,h_1200,c_lfill/imagevaultfiles/id_193086/cf_259/bonburgare_pa_kidneybonor_med_avokadorora.jpg",'600 kCal',true,true);
//const burg2 = new MenuItem('Beany delicious 1.0', "https://assets.icanet.se/e_sharpen:80,q_auto,dpr_1.25,w_718,h_718,c_lfill/imagevaultfiles/id_220407/cf_259/bon-_och_majsburgare.jpg",'800 kCal',false, true);
//const burg3 = new MenuItem('Beany delicious 2.0', "https://res.cloudinary.com/coopsverige/images/w_1200,h_1200/v1521205971/281117/Bbq-+burgare+på+kidneybönor.jpg",'1000 kCal',true,true);

//const burger = [burg1, burg2, burg3];



export default {
  name: 'HomeView',
  components: {
    Burger

  },

  data: function () {
    return {
      //burgers: [ {name: "small burger", kCal: 250},
        //         {name: "standard burger", kCal: 450},
          //       {name: "large burger", kCal: 850}
            //   ]
         
          burgers:menu,
          name:'',
          Email:'',
          btm:'',
          kon:'',
          orderedBurgers:{},
          location: {x:0, y:0}
    }
  },
  methods: {

    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
                 this.location.x=event.clientX - 10 - offset.x;
                 this.location.y=event.clientY - 10 - offset.y;

    },
    printOrder: function () {
      console.log(this.name, this.Email, this.btm, this.kon)
      console.log(this.orderedBurgers);
      socket.emit("addOrder",{orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        orderItems: this.orderedBurgers,
        customerInformation:[this.name, this.Email, this.btm, this.kon]
      });

    },

    setLocation: function (event) {
      this.location.x=event.clientX - 10 - event.currentTarget.getBoundingClientRect().left;
      this.location.y=event.clientY - 10 - event.currentTarget.getBoundingClientRect().top;
    },

    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
  }

  }

</script>

<style>
  #map {
    width: 1920px;
    height: 1078px;
    background: url("/Users/klarastromqvist/Documents/1md031-lab-2022/public/img/polacks.jpg");
    position:absolute;
    background-repeat: no-repeat;
  }

  body {
    font-family: "Times New Roman";
    font-size: 10pt;
}

.welcome { 
    margin: 20px;
    height: 250px;
    overflow: hidden;
} 

#title { 
    position: absolute;
    font-size: 50px;
    margin: 100px;
    padding: 25px;
    margin-top: -180px;
    left: 200px;
}

#image { 
    opacity: 0.5;
    object-position: center;
    object-fit: cover;
    width: 100%;
    height: 250px;
}

#allergies { 
    font-weight: bold;
} 

#burgers { 
    background-color: black;
    color: white;
    margin: 20px;
    border: 2px dashed white;
    padding: 20px;  
} 

#contact { 
    margin: 20px;
    border: 2px dashed black;
    padding: 20px;
} 
  
.wrapper  {
    display: grid;
    grid-template-columns: 33% 33% 33%;
}

button:hover {
    background-color: green;
    cursor: pointer;
 }

 #scroller {
  margin-left: 1.2em;
  width: 1300px;
  height: 500px;
  overflow:scroll;
  position:relative;
 }
 #map div{
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
 }
 
</style>