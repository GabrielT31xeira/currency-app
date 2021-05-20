<template>
  <div class="container grid-lg my-2 py-2">
    <div class="card mb-2" v-if="listenQuotes.length > 0">
      <div class="card-header">
        <div class="h4">Acompanhando</div>
      </div>
      <div class="card-body">
        <WatchListQuotes :listenQuotes="listenQuotes" @unlisten ="onUnlisten"/>
      </div>
    </div>
    <div class="card">
      <div class="card-header">
        <div class="h4">Todas as Moedas</div>
      </div>
      <div class="card-body">
        <ListQuotes :quotes="quotes" :listenQuotes="listenQuotes" @listen="onListen" @unlisten="onUnlisten"/>
      </div>
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, toRefs } from "vue";
import api from "@/services/api";
import ListQuotes from "./components/ListQuotes.vue";
import WatchListQuotes from "./components/WatchListQuotes.vue";

export default {
  components: { ListQuotes, WatchListQuotes },
  name: "App",
  setup() {
    const data = reactive({
      quotes: {},
      listenQuotes: [],
    });

    onMounted(async () => {
      const response = await api.all();
      data.quotes = response.data;
    });
    function onListen(code){
      data.listenQuotes.push(code)
    }
    function onUnlisten(code){
      data.listenQuotes = data.listenQuotes.filter(key => key !== code)
    }

    return { ...toRefs(data),onListen,onUnlisten };
  },
};
</script>
