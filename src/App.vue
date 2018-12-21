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
                      class="input is-primary is-medium is-rounded"
                      type="number"
                      placeholder="100 U$D">
                  </div>
                </div>
              </div>

              <div class="column">
                <div class="select is-medium is-rounded">
                    <select name="divisas">
                      <option value="">Seleccione una divisa...</option>
                    </select>
                </div>
              </div>

            </div>

            <div class="box has-text-centered">
              <p class="is-size-4 has-text-primary">Resultado</p>
              <p class="is-size-5 has-text-grey-light">Descripción</p>
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
      divisas: []
    };
  },

  methods: {
    getDivisas() {
      fetch("https://api.exchangeratesapi.io/latest?base=USD")
        .then(respuesta => respuesta.json())
        .then(objeto => {
          const arregloDivisas = Object.entries(objeto.rates).map(
            ([codigo, ratio]) => ({ codigo, ratio })
          );
          this.divisas = arregloDivisas;
        });
    }
  }
};
</script>

<style scoped>
</style>
