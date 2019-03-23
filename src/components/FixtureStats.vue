<template>
    <div>
        <div>
            <div>Goals</div>
            <div class="flex">
                <!--home-->
                <div class="w-1/2">
                    <div v-for="goals in matchStatus.goalscorers" class="flex flex-col items-end" v-if="goals.home_scorer">
                        <div>
                            <span>{{goals.home_scorer}}</span>  <span>{{goals.time}}</span>
                        </div>
                    </div>
                </div>
                <!--away-->
                <div class="w-1/2">
                    <div v-for="goals in matchStatus.goalscorers" class="flex flex-col items-start" v-if="goals.away_scorer">
                        <div>
                            <span>{{goals.time}}</span> <span>{{goals.away_scorer}}</span>
                        </div>
                    </div>

                </div>
            </div>
            <div v-if="matchStatus.cards.length">Yellow Cards</div>
            <div>

            </div>
            <div v-if="matchStatus.cards.length">Red Cards</div>


        </div>
    </div>
</template>
<script>


  export default{

    props: {
      matchStatus: {
        type: Object,
        required: true
      }
    },
    data () {
      return {

      }
    },

  }
</script>