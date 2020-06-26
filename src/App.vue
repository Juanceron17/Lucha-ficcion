<template>
   <div id="app">
        <nav id="navbar" class="navbar-expand-lg navbar-light">
            <div class="container text-center">
                <h1> <i id="logo1" class="fas fa-dragon"></i><a id="titulo" href="index.html">Lucha-ficcion online</a><i id="logo2"
                        class="fas fa-dungeon"></i>
                </h1>
            </div>
        </nav>
   
    <div class="container-fluid">
        <div class="row">
            <div id="banner" class="col text-center">
                <h2>Personaje</h2>
                  <div>
    <b-button v-b-toggle.sidebar-1 id="tutorialbtn">Instrucciones</b-button>
    <b-sidebar id="sidebar-1" title="Instrucciones" shadow>
      <div class="px-3 py-2">
        <p id="tutorial">
        DESTREZA, RESISTENCIA Y SUERTE
        <br>
        <br> 
Tira un dado. Suma 6 al número obtenido y anota el total en la casilla de DESTREZA. 
Tira dos dados. Suma 12 al número obtenido y anota el total en la casilla de RESISTENCIA. 
Tira un dado, suma 6 al número que te salga y anota el total en la casilla de SUERTE.
        <br>
        <br>
        INVENTARIO
        <br>
        <br>
Escribe en tu inventario los objetos que vallas encontrando durante tu aventura o aquellos que compres.
Si un objeto modifica tus atributos no olvides modificarlos.
        <br>
        <br>
        COMBATE
        <br>
        <br>
Los combates se desarrollan automaticamente, en estos, tu destreza se suma a la tirada de un dado d6 y 
tu rival hace lo mismo, el resultado se compara, y el perdedor resta 2 puntos a su resistencia, el ciclo continua
hasta que la resistencia de uno de ellos es 0.  
        <br>
        <br>
        COMBATE AVANZADO
        <br>
        <br>
En ciertos combates durante tu aventura existiran modificadores especiales que pueden restarte o sumarte caracteristicas,
Tambien existen combates contra varios enemigos a la vez a los que haras frente por separado.       
        
        </p>
                </div>
                 </b-sidebar>
                </div>
                <div class="form-group">
                    <form>
                        <label for="name">Nombre</label>
                        <input type="text" class="form-control">
                        <div class="row">
                            <div class=" col-md col-auto-xs">
                                <label for="name">Destreza</label>
                                <input type="number" class="form-control">
                            </div>
                            <div class="col-md col-auto-xs">
                                <label for="name">Resistencia</label>
                                <input type="number" class="form-control">
                            </div>
                            <div class="col-md col-auto-xs">
                                <label for="name">Suerte</label>
                                <input type="number" class="form-control">
                            </div>
                        </div>
                        <label for="exampleFormControlTextarea1">Inventario</label>
                        <textarea id="inventory" class="form-control overflow-auto" rows="3"></textarea>
                        <label for="name">Oro</label>
                        <input type="number" class="form-control">
                    </form>
                </div>
            </div>
            <div id="middleColumn" class="col-6 overflow-auto" v-if="parrafos.length > 0">
                <!-- <a v-for="item in parrafos" :key="item.index">{{item}}</a> -->
              
                <div class="container-fluid text-center">
                    <img v-if="parrafoActual.img" id="portadaLibro" class="img-fluid" :src="parrafoActual.img" alt="">
                </div>
                <div v-if="parrafoActual.parrafo" v-html="parrafoActual.parrafo">
                </div>
                <div class="container text-center" v-if="parrafoActual.acciones">
                    <button v-for="(item, index) in parrafoActual.acciones" :key="index" v-on:click="irAPagina(item.destino)"  type="button" id="boton" class="btn btn-outline-light">{{item.pregunta}}</button>
                </div>
            </div>
            <div id="actions">
                  <!-- <a v-for="item in json.acciones" @click="cargar(item.destino)">{{item.pregunta}}</a> -->             </div>
            <div id="banner" class="col text-center">
                <h2>Mapa</h2>
                <div  class="container-fluid">
                    <img class="img-fluid max-width: 100% height:auto" id="mapa" src="/img/icons/mapa bosque tenebroso.png" alt="">
                </div>
            </div>
        </div>
    </div>
    <footer id="footer" class="text-center">
        <p id="footer-text">Esta web a sido creada como un trabajo escolar y no tiene animo de lucro</p>
    </footer>
</div>
   
  </template>
  <script>
      import aventura from './assets/static/parrafos.json'

      export default{
          name: "app",
          data(){
              
              return{
                parrafos: [],
                pagina: 0
              }
          },
          computed: {
              parrafoActual(){
                  if(this.parrafos.length === 0){
                      return null
                  }
                  return this.parrafos[this.pagina]
              }
          },
          methods: {
                 irAPagina(paginaDestinoId){
                     //1. crear 2 tipos de acciones (batalla e ir a página) (puedes crear una propiedad que sea "tipo" con valor "batalla" o "destino")
                     //2. distinguir entre una y otra en este metodo
                    console.info("Vamos a la página "+paginaDestino)
                    //3. Si es de tipo destino, ejecutar las siguientes lineas
                    //4. si es de tipo batalla, simular la batalla (tirada de dados y mandar donde corresponda)
                    let paginaDestino = this.parrafos.findIndex(item => item.id === paginaDestinoId)

                    if(paginaDestino !== -1){
                        this.pagina = paginaDestino
                    }

                    //mostrar error o mandar a la página principal
                },
                combate(combateDestinoId){
                    console.info("vamos a la batalla"+ combateDestino)
                    let combateDestino = this.parrafos.findIndex(item => item.id === combateDestinoId)
                }
            
          },
        mounted(){
            console.info(aventura)
            this.parrafos=aventura
            
        }
      }
      
  
  </script>    
    
<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0; 
} 
#navbar{
    background-color: #d95054;
    padding-bottom: 1px;
    padding-top: 5px;
}

#titulo{
    color: whitesmoke;
}

h2{
    color: whitesmoke;
    padding-top: 10px;
}
p{
   margin-left: 10px;
   margin-right: 10px;
   margin-top: 10px;
   margin-bottom: 0rem;
   padding-top: 6px;
   color: whitesmoke;
   
}
#tutorialbtn{
    background-color: #d95054;
    border-color: #C98386;
}
#tutorial{
   color: black; 
}


#logo1{
    display: inline-block;
    padding-right: 10px;
}
#logo2{
    display: inline-block;
    padding-left: 10px;
}

#banner{
    height: 525px;
    background-color: #C98386;
}

#middleColumn{
   background: linear-gradient( 180.07deg, #f7ce8f, 82.49%, rgba(249, 183, 41) 100.97% );
   background-image: url(/img/icons/backgroundPapiro.jpg);
    max-height: 525px;
}     
    

#portadaLibro{
    height: 325px;
    width: 250px;
    margin-top:45px;
    margin-bottom:35px;
}

#inventory{
    height: 75px;
}
#mapa{
    width: 300px;
}
label{
    color: whitesmoke;
}

#footer {
    margin-bottom: 0px;
    height: 38px;
    background-color: #d95054;
}

#footer-text{
    margin-top: 0px;
}    
   

#boton {
        height: 50px;
    margin-left: 35px;
    margin-bottom: 20px;
    margin-top: 20px;
   

}  

</style>
