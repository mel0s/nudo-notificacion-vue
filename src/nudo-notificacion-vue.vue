
<template>
  <div>
    <b-dropdown
      block
      variant="transparen"
      class="m-2"
      menu-class="nudo-cuerpo-notificacion"
    >
      <template #button-content>
        <font-awesome-icon icon="bell" />
      </template>
      <template>
        <b-dropdown-item
          v-for="(item, index) in notificacionesOrdenas"
          :key="index"
          href="javascript:void(0)"
        >
          <div class="nudo-notificacion">
            <font-awesome-icon
              v-if="item.visto"
              class="text-default"
              icon="bell"
              size="2x"
            />
            <font-awesome-icon
              v-else
              class="text-danger"
              icon="bell"
              size="2x"
            />
            <div class="contenido">
              <div title="Prueba">
                {{ item.mensaje }}
              </div>
              <small> {{ fecha(item.fechaMovimiento) }}</small>
            </div>
          </div>
        </b-dropdown-item>
      </template>
    </b-dropdown>
  </div>
</template>

<script>
import Vue from "vue";
import { BootstrapVue, IconsPlugin } from "bootstrap-vue";

import { library } from "@fortawesome/fontawesome-svg-core";
import {
  faUserSecret,
  faBell,
  faClock,
} from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(faUserSecret);
library.add(faBell);
library.add(faClock);

Vue.component("font-awesome-icon", FontAwesomeIcon);

Vue.use(BootstrapVue);
Vue.use(IconsPlugin);

export default {
  name: "NudoNotificacionVue",
  data() {
    return {
      counter: 5,
      initCounter: 5,
      message: {
        action: null,
        amount: null,
      },
    };
  },
  props: {
    notificaciones: {
      type: Array,
      default() {
        return new Array();
      },
    },
  },
  computed: {
    changedBy() {
      const { message } = this;
      if (!message.action) return "initialized";
      return `${message.action} ${message.amount || ""}`.trim();
    },
  },
  methods: {
    fecha(f) {
      let ahora = new Date();
      f = new Date(f);
      let t = ahora - f;

      console.log(t);

      if (t < 300000) {
        return "Ahora";
      } else {
        let minutos = obtenerMinutos(t);
        console.log(minutos);

        if (minutos < 60) {
          return `Hace ${minutos} minutos`;
        } else {
          let horas = obtenerHoras(minutos);
          if (horas < 24) {
            return `Hace ${horas} horas`;
          } else {
            let dias = obtenerDias(horas);
            if (dias < 7) {
              return `Hace ${dias} dias`;
            }
            return f.toLocaleString();
          }
        }
      }
      return f;

      // if (t < 600000) {

      // } else if (t < 3600000) {
      //   return "Hace 5 minutos";
      // }

      function obtenerMinutos(ms) {
        return Math.round(ms / 1000 / 60);
      }

      function obtenerHoras(minutos) {
        return Math.round(minutos / 60);
      }
      function obtenerDias(horas) {
        return Math.round(horas / 24);
      }
    },
  },
  computed: {
    notificacionesOrdenas() {
      return this.notificaciones.sort(function (a, b) {
        return new Date(b.fechaMovimiento) - new Date(a.fechaMovimiento);
      });
    },
  },
};
</script>

<style >
.nudo-cuerpo-notificacion {
  height: 70vh !important;
  overflow: auto;
}

.nudo-cuerpo-notificacion::-webkit-scrollbar {
  width: 10px;
}

/* Track */
.nudo-cuerpo-notificacion::-webkit-scrollbar-track {
  background: #f1f1f1;
}

/* Handle */
.nudo-cuerpo-notificacion::-webkit-scrollbar-thumb {
  background: #888;
}

/* Handle on hover */
.nudo-cuerpo-notificacion::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>



<style scoped>
.nudo-notificacion {
  display: flex;
  border-bottom: solid thin rgb(233, 233, 233);
  padding: 5px;
}
.nudo-notificacion svg {
  margin-top: 10px;
}
.nudo-notificacion .contenido {
  display: flex;
  flex-direction: column;
  padding: 5px 10px;
  max-width: 100%;
}

.nudo-notificacion .contenido div {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.nudo-notificacion .contenido small {
  padding: 0.7em 0;
  font-size: 0.7em;
}
</style>
