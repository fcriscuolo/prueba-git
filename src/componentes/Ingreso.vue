<template>

  <section class="src-componentes-ingreso">
    <div class="jumbotron">
      <h2>Ingreso de Gastos</h2>
      <hr>

      <vue-form :state="formstate" @submit.prevent="enviar()">
        
        <!-- Campo nombre -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" v-model="formData.nombre" required name="nombre" autocomplete="off" class="form-control" />
    
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>

        <!-- Campo DNI -->
        <validate tag="div">
          <label for="dni">DNI</label>
          <input type="text" id="dni" v-model="formData.dni" required name="dni" autocomplete="off" class="form-control" />
    
          <field-messages name="dni" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>
        
        <!-- Campo deuda -->
        <validate tag="div">
          <label for="deuda">Deuda</label>
          <input type="number" id="deuda" v-model.number="formData.deuda" required name="deuda" autocomplete="off" class="form-control" />
    
          <field-messages name="deuda" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>

        <br>
        
        <!-- Campo pago -->
        <validate tag="div">
          <label for="pago">Pago</label>
          <input type="number" id="pago" v-model.number="formData.pago" required name="pago" autocomplete="off" class="form-control" />
    
          <field-messages name="pago" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          </field-messages>
        </validate>
        <br>

        <button class="btn btn-success my-3" :disabled="formstate.$invalid" type="submit">Enviar</button>
      </vue-form>
      <br>
      <hr>

      <!-- Tabla para representar los datos ingresados -->
      <h2>Detalle de Gastos</h2>
      <br>

      <!-- <pre>{{ gastos }}</pre> -->

      <div v-if="gastos.length" class="table-responsive">
        <table class="table">
          <tr>
            <th>Nombre</th>
            <th>DNI</th>
            <th>Deuda</th>
            <th>Pago</th>
            <th>Fecha</th>
            <th>Saldo</th>
          </tr>
          <tr v-for="(gasto,index) in gastos" :key="index" :style="{color: analizarSaldo(gasto).color }">
            <td>{{ gasto.nombre }}</td>
            <td>{{ gasto.dni }}</td>
            <td>{{ gasto.deuda }}</td>
            <td>{{ gasto.pago }}</td>
            <td>{{ gasto.fecha }}</td>
            <td>{{ analizarSaldo(gasto).valor }}</td>
          </tr>
        </table>
      </div>
      <h3 v-else class="alert alert-info">No hay gastos ingresados</h3>

    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-ingreso',
    props: [],
    mounted () {

    },
    data () {
      return {
        formstate : {},
        formData : this.getInitialData(),
        gastos : []
      }
    },
    methods: {
      getInitialData() {
        return {
          nombre : null,
          dni: null,
          deuda: null,
          pago: null
        }
      },
      enviar() {
        let gasto = {...this.formData}
        gasto.fecha = new Date().toLocaleString()

        console.log(gasto)
        this.gastos.push(gasto)

        this.formData = this.getInitialData()
        this.formstate._reset()
      },
      analizarSaldo(gasto) {
        let dif = gasto.pago - gasto.deuda
        let color = '#080'
        if(dif > 0) color = '#00F'
        if(dif < 0) color = '#F00'
        return {
          valor : dif,
          color
        }
      }
    },
    computed: {
    }
}


</script>

<style scoped lang="css">
  .src-componentes-ingreso {

  }

  .jumbotron {
    background-color: lightyellow;
    color: brown;
  }

  hr {
    background-color: #eee;
  }

  pre {
    color: white;
  }
</style>
