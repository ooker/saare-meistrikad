<template>
  <details style="text-align: left;" class="settings">
    <summary style="cursor : pointer;">Seaded</summary>
    <div class="settings-content">
      <div>
        <label for="input-competition">Võistlus <small>(Metrixi ID)</small></label>
        <input type="text" v-model="eventInput" placeholder="Metrixi ID" id="input-competition" />
        <button  @click="setEvent">MUUDA</button>
      </div>
      <div>
        <label for="input-players">Mängijad <small>(eralda komadega)</small></label>
        <textarea v-model="dudesInput" cols="48" rows="4" id="input-players" placeholder="Eesnimi Perekonnanimi, Komaga Eraldatud, Samamis Metrixis"></textarea>
        <br><button @click="setDudes">MUUDA</button>
      </div>
    </div>
  </details>
    
  <div v-if="event" class="events">
    <h1>💩</h1>
    <h2 v-html="event.Competition.Name"></h2>
    <h4><i>MEIE TÜÜPIDE VIRTUAALNE TABEL</i></h4>
    <template v-if="dudesData">
      <div v-for="(d, key) in dudesData" :key="'k' + key">
        <Player :playerData="d"   />
      </div>
    </template>
  </div>
  <div>
    <button class="button" @click="fetchData" v-if="eventInput && dudesInput">Uuenda</button>
  </div>
  <div class="timestamp">Viimased andmed: {{lastUpdate}}</div>
</template>

<script>

import { ref, onMounted } from 'vue';
import Player from './Player.vue'

export default {
  name: 'DiscgolfRound',
  components: {
    Player
  },
  setup(){
    const event = ref(null);
    const eventInput = ref(null);
    const dudes = ref(null);
    const dudesData = ref(null);
    const dudesInput = ref(null);
    const lastUpdate = ref("")
    const query = ref(null);

    const fetchData = async () => {
      query.value = `https://discgolfmetrix.com/api.php?content=result&id=${eventInput.value}`;
      // query.value = `https://discgolfmetrix.com/api.php?content=result&id=1915436`;
      try {
        await fetch(query.value)
          .then(response => response.json())
          .then(data => {
            event.value = data;
            lastUpdate.value = new Date().toString();
            // console.log("siin on ok");
            
            dudesData.value = data.Competition.TourResults.filter(
              player => findNames( dudes.value, player.Name)
              ).sort(function (a, b) {
                return a.Total - b.Total;
              });
        });
      } catch (e) {
        //error.value = e;
        console.error("Nahhui, datat ei tule ju?!?", query.value);
      }
    };

    function findNames(arr, val) {
      return arr.some(arrVal => val.toUpperCase() === arrVal.toUpperCase());
    }

    function getEvent() {
      // console.log("getDudes", localStorage.getItem("savedDudes"));
      eventInput.value = localStorage.getItem("savedEvent");
      // dudes.value = localStorage.getItem("savedDudes").split(", ");
    }
    function setEvent() { 
      localStorage.setItem("savedEvent", eventInput.value);
      // console.log("setDudes" + localStorage.getItem("savedDudes"));
      // dudes.value = localStorage.getItem("savedDudes").split(", ");
      fetchData();
    }
    function getDudes() {
      // console.log("getDudes", localStorage.getItem("savedDudes"));
      dudesInput.value = localStorage.getItem("savedDudes");
      dudes.value = localStorage.getItem("savedDudes").split(", ");
    }
    function setDudes() { 
      localStorage.setItem("savedDudes", dudesInput.value);
      // console.log("setDudes" + localStorage.getItem("savedDudes"));
      dudes.value = localStorage.getItem("savedDudes").split(", ");
      fetchData();
    }

    function getStuffReady(){
      getEvent();
      getDudes();
    }

    onMounted( getStuffReady );

    return {
      event, setEvent, eventInput,
      dudes, dudesData, dudesInput, setDudes, getDudes, 
      fetchData, lastUpdate
    }
  }
}
</script>

<style scoped>
  .events {
    margin-bottom: 5vh;
  }
  h1 {
    font-size: 8vh;
    margin: 3vh 0 0 0;

  }
  h4 {
    font-weight: 400;
    margin: 1vh 0 5vh 0;
  }
  .button{
    padding: 0.5rem 1rem;
    color: var(--col-white);
    background: var(--col-primary);
    font-size: 1.5rem;
    border-radius: 0.25rem;
  }
  .timestamp {
    padding-top: 1rem;
    font-size: 0.75rem;
    font-style: italic;
  }

  .settings {
    position: absolute;
    top:0; left:0;
    width: 100%;
    padding: 0;
    border-radius: 0.5rem;
    background: var(--col-white);
    box-shadow: 0 0.25rem 0.25rem hsla(0, 0%, 0%, 0.15);
  }
  .settings-content {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 1rem;
    padding: 3vmin;
  }
  .settings label {
    display: block;
    margin-bottom: 0.25rem;
  }
  .settings summary {
    padding: 1rem;
    background: var(--col-gray-light);
  }
  .settings button {
    display: block;
    margin-top: 0.5rem;
  }
</style>
