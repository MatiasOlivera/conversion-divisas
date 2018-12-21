<template>

  <div>
    <section class="section">
      <div class="container">

        <div class="columns">
          <div class="column is-full has-text-centered">
            <h1 class="title">
              Conversión de divisas
            </h1>
            <p class="subtitle">
              Dolares estadounidenses > divisa seleccionada
            </p>
          </div>
        </div>

        <div class="columns">
          <div class="column is-half is-offset-one-quarter">
            <div class="columns">

              <div class="column">
                <div class="field">
                  <div class="control">
                    <input
                      v-model="dolares"
                      class="input is-primary is-medium is-rounded"
                      type="number"
                      placeholder="100 U$D"
                      min="0">
                  </div>
                </div>
              </div>

              <div class="column">
                <div class="select is-medium is-rounded">
                    <select name="divisas" v-model.number="seleccionada">
                      <option value="">Seleccione una divisa</option>
                      <option v-for="divisa in divisas" :key="divisa.codigo" :value="divisa">
                        {{ divisa.codigo }}
                      </option>
                    </select>
                </div>
              </div>

            </div>

            <div class="box has-text-centered" v-if="existenAmbos">
              <p class="is-size-4 has-text-primary">
                {{ dolares }} dolares equivale a {{ resultado }} {{ seleccionada.codigo }}
              </p>
              <p class="is-size-5 has-text-grey-light">
                1 dolar = {{ seleccionada.ratio }} {{ seleccionada.codigo }}
              </p>
            </div>
            
          </div>
        </div>
      </div>
    </section>
  </div>

</template>

<script>
export default {
  name: "app",

  created() {
    this.getDivisas();
  },

  data() {
    return {
      divisas: [],
      dolares: 0,
      seleccionada: ""
    };
  },

  computed: {
    existenAmbos() {
      return this.dolares && this.seleccionada;
    },

    resultado() {
      return this.existenAmbos
        ? (this.dolares * this.seleccionada.ratio).toFixed(2)
        : 0;
    }
  },

  methods: {
    getDivisas() {
      fetch("https://api.exchangeratesapi.io/latest?base=USD")
        .then(respuesta => respuesta.json())
        .then(objeto => {
          const arregloDivisas = Object.entries(objeto.rates).map(
            ([codigo, ratio]) => ({ codigo, ratio })
          );

          const arregloOrdenado = arregloDivisas.sort((a, b) => {
            if (a.codigo > b.codigo) return 1;
            if (a.codigo < b.codigo) return -1;
            return 0;
          });

          this.divisas = arregloOrdenado;
        });
    }
  }
};
</script>

<style scoped>
</style>
