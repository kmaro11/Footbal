<template>
    <div class="flex flex">
        <div>
            <div v-for="menu in sidebarMenu" class="flex-col">
                <div
                        @click="topPerformances(menu.action)"
                        class="p-4 bg-purple-darkest text-white my-2 text-center hover:bg-green">
                    {{menu.menu}}
                </div>
            </div>
        </div>
        <div class="w-64 border-purple-darkest border-solid border mx-auto my-0">
            <div v-for="(stats, i) in teamPerformance"
                 v-if="i < 5"
                 class="text-white flex justify-between">
                <div class="p-3">{{stats.player_name}}</div>
                <div class="p-3">{{stats[sideBarMenuStatus]}}</div>
            </div>
        </div>
    </div>
</template>
<script>

    export default {
        props: {
            openedTeamPerformance: {
                type: Array,
                required: true
            },
        },
        data() {
            return {
                sidebarMenu: [
                    {menu: 'Top Score', action: 'player_goals'},
                    {menu: 'Most Red', action: 'player_red_cards'},
                    {menu: 'Most Yellow', action: 'player_yellow_cards'},
                ],
                teamPerformance: [],
                sideBarMenuStatus: '',
            }
        },
        methods: {
            topPerformances(sortBy) {
                this.sideBarMenuStatus = sortBy
                this.teamPerformance = this.openedTeamPerformance.sort((a, b) => (parseInt(a[sortBy]) < parseInt(b[sortBy])) ? 1 : -1)
            },
        },
        beforeMount() {
            this.topPerformances('player_goals')
        }
    }
</script>