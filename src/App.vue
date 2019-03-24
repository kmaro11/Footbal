<template>
    <div id="app">
        <Header :logo="headerTeamsLogo"/>
        <div class="flex">
            <StandingTable :sport="sport" v-on:changeTeam="showTeamInfo($event)"/>
            <div class="bg-red-prime mx-10 max-w-lg w-full p-6">
                <div v-for="team in openedTeam.result" class="w-full ml-auto">
                    <div class="flex items-center flex-col justify-center relative mb-10">
                        <img :src='team.team_logo' alt="" class="img-position absolute">
                        <h1 class="text-white mt-10">{{team.team_name}}</h1>
                    </div>
                    <div class="flex items-center w-full bg-purple-darkest mb-10">
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
                            <TeamTopPerformance :openedTeamPerformance="openedTeamPerformance"/>
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
    import Header from '@/components/Header'
    import TeamTopPerformance from '@/components/TeamTopPerformance';
    import StandingTable from "./components/StandingTable";

    export default {
        data() {
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
                openedTeamPerformance: [],
                headerTeamsLogo: []
            }
        },
        components: {
            StandingTable,
            TeamTopPerformance,
            TeamSquad,
            Fixture,
            Header,
        },
        computed: {},
        methods: {
            async premearLeague() {
                await axios.get('https://allsportsapi.com/api/football/', {
                    params: {
                        met: 'Standings',
                        leagueId: '148',
                        APIkey: this.apiKey
                    }
                }).then(response => {
                    this.sport = response.data
                    for (let i = 0; i < this.sport.result.total.length; i++) {
                        this.allTeams.push(`https://allsportsapi.com/api/football/?&met=Teams&teamId=${this.sport.result.total[i].team_key}&APIkey=${this.apiKey}`)
                    }
                    this.EnglandTeams()
                })
            },
            async EnglandTeams() {
                await axios.all(this.allTeams.map(l => axios.get(l)))
                    .then(axios.spread((...res) => {

                        this.sortLogos(res)
                    }))
            },
            async showTeamInfo(team) {
                await axios.get('https://allsportsapi.com/api/football/', {
                    params: {
                        met: 'Teams',
                        teamId: team,
                        APIkey: this.apiKey
                    }
                }).then(response => {
                    this.openedTeam = response.data
                    this.openedTeamPerformance = this.openedTeam.result[0].players
                    this.teamFixtures(this.openedTeam.result[0].team_key)
                })
            },
            async loadFixtures() {
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
            showTeamInfoMenu(action) {
                this.selectedMenu = action
                if (this.selectedMenu === 'fixture') {
                }
            },

            teamFixtures(teamId) {
                this.teamPlayed = []
                this.allFixtures.forEach(item => {
                    item.forEach(fixture => {
                        if (fixture.away_team_key === teamId || fixture.home_team_key === teamId) {
                            this.teamPlayed.push(fixture)
                        }
                    })
                })
            },
            sortLogos(teamsLogos) {
                teamsLogos.forEach(logos => {
                    logos.data.result.forEach( logo => this.headerTeamsLogo.push(logo))
                })

            }

        },
        beforeMount() {
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
