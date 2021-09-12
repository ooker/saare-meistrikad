<template>
    <div class="player">
        <div class="player-name">
            <b>{{playerData.Name}}</b>
        </div>
        <div v-for="(e,i) in playerData.EventResults" :key="'k'+i">
            {{e}}
        </div>
        <div>
            <b>{{ playerData.Total }}</b>
        </div>
    </div>
</template>

<script>
// import {ref} from "vue";
export default {
  name: 'Player',
  props: {
    playerData: Object
  },
  setup(){
    //   const playerData = ref(props.playerData);
    //   return {playerData}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .player {
        display: flex;
        justify-content: space-between;
        padding: 1rem 0;
        border-bottom: 1px dotted var(--col-primary);
        
    }
    .player-name {
        flex: 0 0 30%;
        text-align: left;
    }
</style>
