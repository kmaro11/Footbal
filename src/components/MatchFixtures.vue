<template>
    <div>
        <div @click="openStats = !openStats" class="flex bg-purple-darkest my-4 justify-between text-white items-center">
            <div class="py-4 w-48 pl-4">{{fixt.event_home_team}}</div>
            <div class="w-10 text-white">{{fixt.event_final_result}}</div>
            <div class="py-4 w-48 pr-4 text-right">{{fixt.event_away_team}}</div>
        </div>
        <FixtureStats :match-status="fixt" v-if="openStats"/>
    </div>
</template>

<script>
  import FixtureStats from './FixtureStats'

  export default {

    props: {

      fixt: {
        type: Object,
        required: true
      }
    },
    data () {
      return {
        openStats: false
      }
    },
    components: {
      FixtureStats
    }

  }
</script>