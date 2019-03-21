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
                            <a class="w-full flex"><span class="w-10 mr-1">{{team.standing_place}}.</span><span
                                    class="w-32 mr-2">{{team.standing_team}}</span><span
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
                    </div>
                    <div>

                        <div v-if="selectedMenu === 'players'">
                            <TeamSquad :team="team"/>
                        </div>
                        <div v-if="selectedMenu === 'fixture'" class="overflow-y-scroll h-70 mt-10">
                            <Fixture :teamPlayed="teamPlayed"/>
                        </div>
                        <div v-if="selectedMenu === 'performance'">
                            <div v-for="stats in statistics">

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
  import TeamSquad from '@/components/TeamSquad'
  import Fixture from '@/components/Fixture'

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
        statistics: [],
        allFixtures: [],
        teamPlayed: [],
      }
    },
    components: {
      TeamSquad,
      Fixture
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
          this.teamTopPerformances(this.openedTeam.result[0].players)
          this.teamFixtures(this.openedTeam.result[0].team_key)

        })
      },
      async loadFixtures () {
        await axios.get('https://allsportsapi.com/api/football/', {
          params: {
            met: 'Fixtures',
            from: '2018-06-01',
            to: '2019-06-01',
            APIkey: this.apiKey,
            leagueId: '148'
          }
        }).then(response => {
          this.allFixtures.push(response.data.result)
        })
      },
      showTeamInfoMenu (action) {
        this.selectedMenu = action
        if (this.selectedMenu === 'fixture') {
        }
      },
      teamTopPerformances (team) {
        let allPlayersStats = []
        team.forEach( players => {
          console.log(players)
          allPlayersStats.push(players)

        })
        // this.teamTopScorer (allPlayersStats)
        // this.teamTopRedCards (allPlayersStats)
        // this.topYellowCards (allPlayersStats)
      },
      // teamTopScorer (goals) {
      //   console.log(goals.player_country)
      // },
      // teamTopRedCards (redCards) {
      //
      // },
      // topYellowCards (yellowCards) {
      //
      // },
      teamFixtures (teamId) {
        this.teamPlayed = []
        this.allFixtures.forEach( item => {
          item.forEach( fixture => {
            if (fixture.away_team_key === teamId || fixture.home_team_key === teamId) {
              this.teamPlayed.push(fixture)
            }
          })
        })
      }

    },
    beforeMount () {
      this.premearLeague()
      this.loadFixtures()
    },

  }

</script>

<style type="text/css">
    .img-position {
        top: -65px;
    }
</style>
