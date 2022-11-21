<template>

    <div>
            <h3> {{burger.name}} </h3>
            <button v-on:click=increase type="button">
              +
            </button>
            <button v-on:click=decrease type="button">
              -
            </button>
            <h4> Antal beställda: {{ amountOrdered }}</h4>
                
                <img id ="burgersize" v-bind:src = "burger.url">

                  <ul>
                      <li>{{burger.calories}} kCal</li>
                      <li v-if="burger.gluten">Innehåller<span id="allergies"> gluten</span> </li>
                      <li v-if="burger.lactose">Innehåller<span id="allergies"> laktos</span></li>
                    
                  </ul>
  

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
        }},
methods: {
        increase: function() {
          this.amountOrdered += 1;
          this.$emit('orderedBurger', {name: this.burger.name
                                      ,amount: this.amountOrdered
                                    }
    );
  },
      decrease: function() {
          if(this.amountOrdered >= 1){
          this.amountOrdered -= 1 }
          this.$emit('orderedBurger', {name: this.burger.name
                                      ,amount: this.amountOrdered
                                    }
          )
                                        
  
}
      }
    }

  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

#allergies { 
    font-weight: bold;
  } 

#burgersize {
  height: 150px;
  width: 40%;
} 

.wrapper  {
    display: grid;
    grid-template-columns: 33% 33% 33%;
}
#burgers { 
    background-color: black;
    color: white;
    margin: 20px;
    border: 2px dashed white;
    padding: 20px;  
  }
  
  </style>
  