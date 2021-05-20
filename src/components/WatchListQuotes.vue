<template>
  <ListQuotes :quotes="quotes" />
  <div class="mt-2 text-right">
    <cite class="text-small"> Atualizara novamente em <b>{{ nextUpdate }} segundos</b> </cite>
  </div>
</template>

<script>
import { ref } from "vue";
import ListQuotes from "./ListQuotes.vue";
export default {
  components: { ListQuotes },
  setup() {
    const quotes = ref({});
    const nextUpdate = ref(30)

    return { quotes, nextUpdate };
  },
};
</script>