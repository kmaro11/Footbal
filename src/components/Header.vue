<template>
    <nav>
        <div class="h-16 bg-purple-darkest w-full flex items-center justify-between px-4">
            <div v-for="logos in logo" class="w-10">
                <img :src='logos.team_logo' alt="" class="w-full">
            </div>
        </div>
        <div class="flex bg-green mb-16 pl-12">
            <div v-for="menu in headerMenu" class="p-3 text-purple-darkest font-bold cursor-pointer hover:bg-red-prime">
                {{menu.menu}}
            </div>
        </div>
    </nav>
</template>
<script>

    export default {
        props: {
            logo: {
                type: Array,
                required: true
            },
        },
        data() {
            return {
                headerMenu: [
                    {menu: 'Statistic', action: 'statistics'},
                    {menu: 'Standing', action: 'standings'},
                ],
            }
        },
    }
</script>