<template>
    <div>
        <MatchFixtures v-for="fixt in teamPlayed" :fixt="fixt" :key="fixt.event_key"/>
    </div>
</template>
<script>

  import MatchFixtures from './MatchFixtures'

  export default{

    props: {

      teamPlayed: {
        type: Array,
        required: true
      },
      teamId: String
    },
    data () {
      return {

      }
    },
    components: {
      MatchFixtures
    },

  }
</script>