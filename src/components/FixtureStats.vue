<template>
    <div>
        <div>
            <div>Goals</div>
            <div class="flex">
                <!--home-->
                <div>
                    <div v-for="goals in matchStatus.goalscorers" class="flex flex-col" v-if="goals.home_scorer">
                        <div>
                            <span>{{goals.home_scorer}}</span>  <span>{{goals.time}}</span>
                        </div>
                    </div>
                </div>
                <!--away-->
                <div>
                    <div v-for="goals in matchStatus.goalscorers" class="flex flex-col" v-if="goals.away_scorer">
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