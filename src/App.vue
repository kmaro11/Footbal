<template>
    <div id="app">
        <nav class="h-16 bg-purple-darkest w-full mb-16">
            <!--<div class="flex" v-for="team in openedTeam.result">-->
            <!--<a> <img :src="team.team_logo" alt=""></a>-->
            <!--</div>-->
        </nav>
        <div class="flex">
            <div class=" bg-purple-darkest flex flex-col p-8 mx-16">
                <div class="flex mb-4">
                    <span class="text-white w-10 mr-1">Pos</span>
                    <span class="text-white w-32 mr-2">Club</span>
                    <span class="text-white w-10">Pts</span>
                </div>
                <div v-for="item in sport">
                    <ul v-for="team in item.total" class="list-reset flex flex-col">
                        <li @click="showTeamInfo(team.team_key)" class="text-white w-full pt-2 pb-2">
                            <a class="w-full flex"><span class="w-10 mr-1">{{team.standing_place}}.</span><span class="w-32 mr-2">{{team.standing_team}}</span><span
                                    class="w-10">{{team.standing_PTS}}</span></a>
                        </li>
                    </ul>
                </div>
            </div>
            <div class="bg-red-prime mx-10 max-w-lg w-full p-6">
                <div v-for="team in openedTeam.result" class="w-full ml-auto">
                    <div class="flex items-center flex-col justify-center relative mb-10">
                        <img :src='team.team_logo' alt="" class="img-position absolute">
                        <h1 class="text-white mt-10">{{team.team_name}}</h1>
                    </div>
                    <div class="flex items-center w-full bg-purple-darkest">
                        <a v-for="menu in teamsMenu"
                           class="text-white text-center uppercase w-1/3 hover:bg-green py-2 cursor-pointer"
                           @click="showTeamInfoMenu(menu.action)">
                            {{menu.menu}}
                        </a>

                        <!--<a class="text-white text-center uppercase w-1/3 hover:bg-green py-2 cursor-pointer">Players</a>-->
                        <!--<a class="text-white text-center uppercase w-1/3 hover:bg-green py-2 cursor-pointer">Fixture</a>-->
                        <!--<a class="text-white text-center uppercase w-1/3 hover:bg-green py-2 cursor-pointer">Top performance</a>-->
                    </div>
                    <div>
                        <div v-if="selectedMenu === 'players'">
                            <h2 class="text-purple-darkest my-4">Goalkeepers</h2>
                            <div class="flex flex-wrap items-center">
                                <div v-for="player in team.players"
                                     v-if="player.player_type === 'Goalkeepers'"
                                     class="text-black flex mr-4 h-6 bg-white flex items-center justify-center p-2 mb-2">
                                    <span> {{player.player_name}}</span>
                                </div>
                            </div>
                            <h2 class="text-purple-darkest my-4">Defenders</h2>
                            <div class="flex flex-wrap">
                                <div v-for="player in team.players"
                                     v-if="player.player_type === 'Defenders'"
                                     class="text-black flex mr-4 h-6 bg-white flex items-center justify-center p-2 mb-2">
                                    {{player.player_name}}
                                </div>
                            </div>
                            <h2 class="text-purple-darkest my-4">Midfielders</h2>
                            <div class="flex flex-wrap">
                                <div v-for="player in team.players"
                                     v-if="player.player_type === 'Midfielders'"
                                     class="text-black flex mr-4 h-6 bg-white flex items-center justify-center p-2 mb-2">
                                    {{player.player_name}}
                                </div>
                            </div>
                            <h2 class="text-purple-darkest my-4">Forwards</h2>
                            <div class="flex flex-wrap">
                                <div v-for="player in team.players"
                                     v-if="player.player_type === 'Forwards'"
                                     class="text-black flex mr-4 h-6 bg-white flex items-center justify-center p-2 mb-2">
                                    {{player.player_name}}
                                </div>
                            </div>
                        </div>
                        <div v-if="selectedMenu === 'performance'">
                            <div v-for="goals in mostGoals">
                                {{goals.name}}
                                {{goals.goals}}
                            </div>

                        </div>
                    </div>
                </div>
            </div>
        </div>
        <!--<teamSquad/>-->
        <!--<router-link to="/">Home</router-link> |-->
        <!--<router-link to="/about">About</router-link>-->
    </div>
    <!--<router-view/>-->
</template>
<script>
  import axios from 'axios';
  import '@/assets/css/tailwind.css'
  // import teamSquad from 'components/TeamSquad'

  export default {
    data () {
      return {
        sport: [],
        apiKey: '5b6339a51368e998ca64d8eea1032bd7fe1095678c5e49ae41c19247e9893548',
        openedTeam: [],
        allTeams: [],
        teamsMenu: [
          {menu: 'players', action: 'players'},
          {menu: 'fixture', action: 'fixture'},
          {menu: 'Top performance', action: 'performance'},
        ],
        selectedMenu: 'players',
        mostGoals: []
      }
    },
    component: {
      // teamSquad
    },
    computed: {},
    methods: {
      async premearLeague () {
        await axios.get(`https://allsportsapi.com/api/football/?&met=Standings&leagueId=148&APIkey=${this.apiKey}`).then(response => {
          this.sport = response.data
          for (let i = 0; i < this.sport.result.total.length; i++) {
            this.allTeams.push(this.sport.result.total[i].team_key)
          }
          this.EnglandTeams()
        })
      },
      EnglandTeams () {

      },
      async showTeamInfo (team) {
        await axios.get('https://allsportsapi.com/api/football/', {
          params: {
            met: 'Teams',
            teamId: team,
            APIkey: this.apiKey
          }
        }).then(response => {
          this.openedTeam = response.data
          this.teamTopScorer(this.openedTeam)
        })
      },
      showTeamInfoMenu (action) {
        this.selectedMenu = action
      },
      teamTopScorer (team) {
        this.mostGoals = []
        team.result.forEach(item => {
          // item.players.forEach(topScore => {
          //   console.log(topScore)
          //   // this.mostGoals.push({goals:topScore.player_goals,name:topScore.player_name})
          //   // console.log(this.mostGoals)
          // })
        })

      },

    },
    beforeMount () {
      this.premearLeague()
    },

  }

</script>

<style type="text/css">
    .img-position {
        top: -65px;
    }
</style>

