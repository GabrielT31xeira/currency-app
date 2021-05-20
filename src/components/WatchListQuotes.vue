<template>
  <ListQuotes :quotes="quotes" :listenQuotes="listenQuotes" @unlisten="onUnlisten"/>
  <div class="mt-2 text-right">
    <cite class="text-small"> Atualizara novamente em <b>{{ nextUpdate }} segundos</b> </cite>
  </div>
</template>

<script>
import { onMounted, onUnmounted, reactive, ref, toRefs, watch } from "vue";
import ListQuotes from "./ListQuotes.vue";
import api from "@/services/api"
const CURRENT_UPDATE_TIME = 30

export default {
  components: { ListQuotes },
  props:{
    listenQuotes:{ type: Array, required:true },
  },
  emits: ['unlisten'],
  setup(props, { emit }) {
    const interval = ref(null)
    const quotes = ref({})
    let nextUpdate = ref(CURRENT_UPDATE_TIME)
    

    const methods = reactive({
      onUnlisten(code) {
        emit('unlisten',code)
      },
      restartIterval(){
        clearInterval(interval.value)
        nextUpdate.value = CURRENT_UPDATE_TIME
        interval.value = setInterval(()=>{
          nextUpdate.value -= 1
          if(nextUpdate.value === 0){
            nextUpdate = CURRENT_UPDATE_TIME
            this.refresh()
          }
        },1000)
      },
      async refresh(){
        const {data} = await api.listen(props.listenQuotes)
        quotes.value = data
      }
    });
    onMounted(()=>{
      methods.refresh()
      methods.restartIterval();
    });
    onUnmounted(()=>{
      clearInterval(interval.value)
    });
    watch(props,()=>{
      methods.refresh()
    })
    return { 
      ...toRefs(methods),
      quotes, 
      nextUpdate };
  },
};
</script>