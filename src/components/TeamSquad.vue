<template>
   <div>
       <h2 class="text-purple-darkest my-4">Goalkeepers</h2>
       <div class="flex flex-wrap items-center">
           <div v-for="player in team.players"
                v-if="player.player_type === 'Goalkeepers'"
                class="text-black flex mr-4 h-6 bg-white flex items-center justify-center mb-2 h-full">
               <div class="flex items-center">
                   <span class="text-green bg-purple-darkest py-2 w-10 text-center">{{player.player_number}}</span>
                   <span class="p-2"> {{player.player_name}}</span>
               </div>
           </div>
       </div>
       <h2 class="text-purple-darkest my-4">Defenders</h2>
       <div class="flex flex-wrap">
           <div v-for="player in team.players"
                v-if="player.player_type === 'Defenders'"
                class="text-black flex mr-4 h-6 bg-white flex items-center justify-center mb-2 h-full">
               <span class="text-green bg-purple-darkest py-2 w-10 text-center">{{player.player_number}}</span>
               <span class="p-2"> {{player.player_name}}</span>
           </div>
       </div>
       <h2 class="text-purple-darkest my-4">Midfielders</h2>
       <div class="flex flex-wrap">
           <div v-for="player in team.players"
                v-if="player.player_type === 'Midfielders'"
                class="text-black flex mr-4 h-6 bg-white flex items-center justify-center mb-2 h-full">
               <span class="text-green bg-purple-darkest py-2 w-10 text-center">{{player.player_number}}</span>
               <span class="p-2"> {{player.player_name}}</span>
           </div>
       </div>
       <h2 class="text-purple-darkest my-4">Forwards</h2>
       <div class="flex flex-wrap">
           <div v-for="player in team.players"
                v-if="player.player_type === 'Forwards'"
                class="text-black flex mr-4 h-6 bg-white flex items-center justify-center mb-2 h-full">
               <span class="text-green bg-purple-darkest py-2 w-10 text-center">{{player.player_number}}</span>
               <span class="p-2"> {{player.player_name}}</span>
           </div>
       </div>
   </div>
</template>
<script>

  export default{

    props: {

      team: {
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