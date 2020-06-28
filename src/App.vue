<template>
  <div id="app">
    <nav id="navbar" class="navbar-expand-lg navbar-light">
      <div class="container text-center">
        <h1>
          <i id="logo1" class="fas fa-dragon"></i
          ><a id="titulo" href="index.html">Lucha-ficcion online</a
          ><i id="logo2" class="fas fa-dungeon"></i>
        </h1>
      </div>
    </nav>

    <div class="container-fluid">
      <div class="row">
        <div id="banner" class="col text-center">
          <h2>Personaje</h2>
          <div>
            <b-button v-b-toggle.sidebar-1 id="tutorialbtn"
              >Instrucciones</b-button
            >
            <b-sidebar id="sidebar-1" title="Instrucciones" shadow>
              <div class="px-3 py-2">
                <p id="tutorial">
                  DESTREZA, RESISTENCIA Y SUERTE
                  <br />
                  <br />
                  Tira un dado. Suma 6 al número obtenido y anota el total en la
                  casilla de DESTREZA. Tira dos dados. Suma 12 al número
                  obtenido y anota el total en la casilla de RESISTENCIA. Tira
                  un dado, suma 6 al número que te salga y anota el total en la
                  casilla de SUERTE.
                  <br />
                  <br />
                  INVENTARIO
                  <br />
                  <br />
                  Escribe en tu inventario los objetos que vallas encontrando
                  durante tu aventura o aquellos que compres. Si un objeto
                  modifica tus atributos no olvides modificarlos.
                  <br />
                  <br />
                  COMBATE
                  <br />
                  <br />
                  Los combates se desarrollan automaticamente, en estos, tu
                  destreza se suma a la tirada de un dado d6 y tu rival hace lo
                  mismo, el resultado se compara, y el perdedor resta 2 puntos a
                  su resistencia, el ciclo continua hasta que la resistencia de
                  uno de ellos es 0.
                  <br />
                  <br />
                  COMBATE AVANZADO
                  <br />
                  <br />
                  En ciertos combates durante tu aventura existiran
                  modificadores especiales que pueden restarte o sumarte
                  caracteristicas, Tambien existen combates contra varios
                  enemigos a la vez a los que haras frente por separado.
                </p>
              </div>
            </b-sidebar>
          </div>
          <div class="form-group">
            <form>
              <label for="name">Nombre</label>
              <input type="text" class="form-control" v-model="nombre" />
              <div class="row">
                <div class=" col-md col-auto-xs">
                  <label id="des" for="name">Destreza</label>
                  <input
                    type="number"
                    class="form-control"
                    v-model="destreza"
                    disabled
                  />
                </div>
                <div class="col-md col-auto-xs">
                  <label for="name">Resistencia</label>
                  <input
                    type="number"
                    class="form-control"
                    v-model="resistencia"
                    disabled
                  />
                </div>
                <div class="col-md col-auto-xs">
                  <label for="name">Suerte</label>
                  <input
                    type="number"
                    class="form-control"
                    v-model="suerte"
                    disabled
                  />
                </div>
              </div>
              <label for="exampleFormControlTextarea1">Inventario</label>
              <textarea
                id="inventory"
                class="form-control overflow-auto"
                rows="3"
              ></textarea>
              <label for="name">Oro</label>
              <input type="number" class="form-control" v-model="oro" />
            </form>
          </div>
        </div>
        <div
          id="middleColumn"
          class="col-6 overflow-auto"
          v-if="parrafos.length > 0"
        >
          <div class="container-fluid text-center">
            <img
              v-if="parrafoActual.img"
              id="portadaLibro"
              class="img-fluid"
              :src="parrafoActual.img"
              alt=""
            />
          </div>
          <div
            v-if="parrafoActual.parrafo"
            v-html="parrafoActual.parrafo"
          ></div>

          <div class="container text-center" v-if="parrafoActual.acciones">
            <button
              v-for="(item, index) in parrafoActual.acciones"
              :key="index"
              v-on:click="irAPagina(item.destino)"
              type="button"
              id="boton"
              class="btn btn-outline-light"
            >
              {{ item.pregunta }}
            </button>
          </div>
          <div></div>
          <div class="container text-center" v-if="!personajeCreado">
            <button
              v-on:click="tirarDados()"
              type="button"
              id="boton"
              class="btn btn-outline-light"
            >
              Tirar dados
            </button>
          </div>
        </div>
        <div id="banner" class="col text-center d-xs-none">
          <h2>Mapa</h2>
          <div class="container-fluid">
            <img
              class="img-fluid max-width: 100% height:auto"
              id="mapa"
              src="/img/icons/mapa bosque tenebroso.png"
              alt=""
            />
          </div>
        </div>
      </div>
    </div>
    <footer id="footer" class="text-center">
      <p id="footer-text">
        Esta web a sido creada como un trabajo escolar y no tiene animo de lucro
      </p>
    </footer>
  </div>
</template>
<script>
import aventura from "./assets/static/parrafos.json";
import enemigos from "./assets/static/enemy.json";
import personaje from "./assets/static/personaje.json";
export default {
  name: "app",
  data() {
    return {
      parrafos: [],
      personaje: [],
      rondas: [],
      pagina: 0,
      nombre: "",
      destreza: "",
      resistencia: "",
      suerte: "",
      oro: 30,
      personajeCreado: false,
    };
  },
  computed: {
    parrafoActual() {
      if (this.parrafos.length === 0) {
        return null;
      }
      return this.parrafos[this.pagina];
    },
  },
  methods: {
    irAPagina(paginaDestinoId) {
      if (!this.personajeCreado) {
        alert("Hay que crear el personaje");
      } else if (this.personajeCreado) {
        let paginaDestino = this.parrafos.findIndex(
          (item) => item.id === paginaDestinoId
        );
        let accion = this.parrafos[paginaDestino].acciones;
        console.log("accion ---> ", accion);
        if (paginaDestino !== -1) {
          this.pagina = paginaDestino;
        }

        if (accion[0].pregunta === "Has ganado") {
          console.log("ESTO ES UN COMBATE");
          let idEnemigo = accion[0].idEnemigo;
          this.combate(enemigos[idEnemigo], personaje);
        }
      }
    },

    tirarDados() {
      if (this.nombre === "") {
        alert("Introduce el nombre para tirar los dados");
      } else {
        this.resistencia = Math.floor(Math.random() * 12 + 1) + 12;
        this.destreza = Math.floor(Math.random() * 6 + 1) + 6;
        this.suerte = Math.floor(Math.random() * 6 + 1) + 6;
        this.personajeCreado = true;

        personaje.nombre = this.nombre;
        personaje.destreza = this.destreza;
        personaje.resistencia = this.resistencia;
        personaje.suerte = this.suerte;
        personaje.oro = this.oro;
      }
    },

    combate(enemigo, personaje) {
      console.log("personaje.resistencia ---> ", personaje.resistencia);
      console.log("enemigo.resistencia ---> ", enemigo.resistencia);

      if (personaje.resistencia > 0 && enemigo.resistencia > 0) {
        // AQUI SIGUE EL COMBATE
        this.restarVida(enemigo, personaje);
      } else if (enemigo.resistencia <= 0) {
        //GANAS EL COMBATE
        for (let i = 0; i < this.rondas.length; i++) {
          if (this.rondas[i].danioA === "enemigo") {
            alert(
              `
              Ronda ${i + 1}
              Has atacado, el enemigo ha sufrido ${this.rondas[i].danioSufrido}, su vida ahora es de ${this.rondas[i].vidaEnemigo}
              `
            );
          } else if (this.rondas[i].danioA === "personaje") {
            alert(
              `
              Ronda ${i + 1}
              El enemigo ataca, el personaje ha sufrido ${this.rondas[i].danioSufrido}, tu vida ahora es de ${this.rondas[i].vidaPersonaje}
              `
            );
                   this.resistencia = personaje.resistencia;
          } else if (this.rondas[i].danioA === "empate") {
            alert(
              `
              Ronda ${i + 1}
              Ambos bloqueais el ataque, y no sufris daño alguno
              `
            );
          }
        }
        alert("Has ganado el combate");
        this.rondas = [];

        this.resistencia = personaje.resistencia;
      } else if (personaje.resistencia <= 0) {
        //PIERDES EL COMBATE

        for (let i = 0; i < this.rondas.length; i++) {
          if (this.rondas[i].danioA === "enemigo") {
            alert(
              `
              Ronda ${i + 1}
              Has atacado, el enemigo ha sufrido ${this.rondas[i].danioSufrido}, su vida ahora es de ${this.rondas[i].vidaEnemigo}
              `
            );
          } else if (this.rondas[i].danioA === "personaje") {
            alert(
              `
              Ronda ${i + 1}
              El enemigo ataca, el personaje ha sufrido ${this.rondas[i].danioSufrido}, tu vida ahora es de ${this.rondas[i].vidaPersonaje}
              `
            );
                   this.resistencia = personaje.resistencia;
          } else if (this.rondas[i].danioA === "empate") {
            alert(
              `
              Ronda ${i + 1}
              Ambos bloqueais el ataque, y no sufris daño alguno
              `
            );
          }
        }
        alert("Has perdido el combate");
        this.irAPagina(0);
        this.nombre = ""
        this.destreza = ""
        this.resistencia = ""
        this.suerte = ""
        this.rondas = [];
        this.personajeCreado = false
      }
    },

    restarVida(enemigo, personaje) {
      let fuerzaJugador = Math.floor(Math.random() * 6 + 1) + personaje.destreza;
      let fuerzaEnemigo = Math.floor(Math.random() * 6 + 1) + enemigo.destreza;

      console.log("fuerzaJugador ---> ", fuerzaJugador);
      console.log("fuerzaEnemigo ---> ", fuerzaEnemigo);

      if (fuerzaJugador > fuerzaEnemigo) {
        enemigo.resistencia = enemigo.resistencia - 2;
        this.rondas.push({
          danioA: "enemigo",
          vidaEnemigo: enemigo.resistencia,
          danioSufrido: "-2",
        });
        this.combate(enemigo, personaje);
      } else if (fuerzaEnemigo > fuerzaJugador) {
        personaje.resistencia = personaje.resistencia - 2;
 
        this.rondas.push({
          danioA: "personaje",
          vidaPersonaje: personaje.resistencia,
          danioSufrido: "-2",
        });
        this.combate(enemigo, personaje);
      } else if ((fuerzaEnemigo = fuerzaJugador)) {
        this.rondas.push({
          danioA: "empate",
          empate: "empate",
          danioSufrido: "0",
        });
        this.combate(enemigo, personaje);
      }
    },
  },
  mounted() {
    console.info(enemigos);
    this.parrafos = aventura;
  },
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
#navbar {
  background-color: #d95054;
  padding-bottom: 1px;
  padding-top: 5px;
}

#titulo {
  color: whitesmoke;
}

h2 {
  color: whitesmoke;
  padding-top: 10px;
}
p {
  margin-left: 10px;
  margin-right: 10px;
  margin-top: 10px;
  margin-bottom: 0rem;
  padding-top: 6px;
  color: whitesmoke;
}
#tutorialbtn {
  background-color: #d95054;
  border-color: #c98386;
}
#tutorial {
  color: black;
}

#logo1 {
  display: inline-block;
  padding-right: 10px;
}
#logo2 {
  display: inline-block;
  padding-left: 10px;
}

#banner {
  height: 525px;
  background-color: #c98386;
}

#middleColumn {
  background: linear-gradient(
    180.07deg,
    #f7ce8f,
    82.49%,
    rgba(249, 183, 41) 100.97%
  );
  background-image: url(/img/icons/backgroundPapiro.jpg);
  max-height: 525px;
}

#portadaLibro {
  height: 325px;
  width: 250px;
  margin-top: 45px;
  margin-bottom: 35px;
}

#inventory {
  height: 75px;
}
#mapa {
  width: 300px;
}
label {
  color: whitesmoke;
}

#footer {
  margin-bottom: 0px;
  height: 38px;
  background-color: #d95054;
}

#footer-text {
  margin-top: 0px;
}

#boton {
  height: 50px;
  margin-left: 35px;
  margin-bottom: 20px;
  margin-top: 20px;
}
</style>
