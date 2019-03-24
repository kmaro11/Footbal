<template>
    <div class=" bg-purple-darkest flex flex-col p-8 mx-16">
        <div class="flex mb-4">
            <span class="text-white w-10 mr-1">Pos</span>
            <span class="text-white w-32 mr-2">Club</span>
            <span class="text-white w-10">Pts</span>
        </div>
        <div v-for="item in sport">
            <ul v-for="team in item.total" class="list-reset flex flex-col">
                <li @click="changeDisplayTeam(team.team_key)" class="text-white w-full pt-2 pb-2">
                    <a class="w-full flex"><span class="w-10 mr-1">{{team.standing_place}}.</span><span
                            class="w-32 mr-2">{{team.standing_team}}</span><span
                            class="w-10">{{team.standing_PTS}}</span></a>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>

    export default {
        props: {
            sport: {
                type: Object,
                required: true
            },
        },
        data() {
            return {}
        },
        methods: {
            changeDisplayTeam(team) {
                this.$emit('changeTeam', team)

            }
        }
    }
</script>