<template>
  <table class="table">
      <thead>
          <tr>
              <th>Código</th>
              <th>Nome</th>
              <th>Máximo</th>
              <th>Minimo</th>
              <th>Variação</th>
              <th></th>
          </tr>
      </thead>
      <tbody>
          <tr v-for="(quote, key) in quotes" :key="key">
              <td>{{ key }}</td>
              <td>{{ quote.name }}</td>
              <td>{{ quote.high }}</td>
              <td>{{ quote.low }}</td>
              <td>
                  <span class="label label-rounded text-small" 
                  :class="{'label-error': quote.pctChange < 0, 'label-succes': quote.pctChange > 0 }">
                      {{ quote.pctChange }} %
                  </span>
              </td>
              <td>
                  <a 
                  href="#" 
                  class="btn btn-primary btn-sm tooltip tooltip-left"
                  data-tooltip="Seguir">
                      <i class="icon icon-plus"></i>
                  </a>
              </td>
          </tr>
      </tbody>
  </table>
</template>

<script>
export default {
    props:{
        quotes: {type:Object, required: true},
        listenQuotes: {type:Array, required:true}
    }
}
</script>

