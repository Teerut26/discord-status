<template>
    <div>
        <div class="container mt-3">
            <button class="btn btn-success" @click="selectType('all')">ALL</button>
            <button class="btn btn-success ml-2" @click="selectType('user')">User</button>
            <button class="btn btn-success ml-2" @click="selectType('bot')">Bot</button>
            <div class="table-responsive">
            <table class="table table-striped table-bordered mt-2">
                <thead class="thead-dark">
                    <tr>
                        <th>Img</th>
                        <th>Username</th>
                        <th>mute</th>
                        <th>status</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="item in noBot" :key="item">
                        <td style="padding: 0px; width: 50px; height: 50px;"><img :src="item.avatar_url" alt="" srcset="" width="50" height="50"></td>
                        <td>{{item.username}}</td>
                        <td>
                            <strong v-if="item.self_deaf === true" class="badge badge-danger">
                                <span class="fa-stack fa-1x">
                                    <i class="fas fa-slash fa-stack-1x fa-2x"></i>
                                    <i class="fas fa-headphones fa-stack-1x fa-2x"></i>
                                </span>
                            </strong>
                            <strong v-else-if="item.self_deaf === false" class="badge badge-success">
                                <span class="fa-stack fa-1x">
                                    <i class="fas fa-headphones fa-stack-1x fa-1x fa-2x"></i>
                                </span>
                            </strong>

                            <strong v-if="item.self_mute === true" class="badge badge-danger ml-2">
                                <span class="fa-stack fa-1x">
                                    <i class="fas fa-microphone-slash fa-stack-1x fa-2x"></i>
                                </span>
                            </strong>
                            <strong v-else-if="item.self_mute === false" class="badge badge-success ml-2">
                                <span class="fa-stack fa-1x">
                                    <i class="fas fa-microphone-alt fa-stack-1x fa-2x"></i>
                                </span>
                                </strong>
                            <strong v-else class="badge badge-secondary">ไม่ได้อยู่ในช่องเสียง</strong>
                        </td>
                        <td>
                            <strong v-if="item.status === 'online'" class="badge badge-success">Online</strong>
                            <strong v-else-if="item.status === 'dnd' " class="badge badge-danger">ห้ามรบกวน</strong>
                            <strong v-else class="badge badge-warning">ไม่อยู่</strong>
                        </td>
                    </tr>
                </tbody>
            </table>
            </div>
        </div>
    </div>
</template>

<script>
    // @ is an alias to /src
    const axios = require('axios');
    // import Card from '@/components/Card.vue'

    export default {
        name: 'Discord',
        components: {
        },
        data() {
            return {
                NavbarTitle: "Discord",
                card_title: "Discord",
                discordData: [],
                discordDataAll: [],
                gettype: "all"
            }
        },
        computed: {
            noBot: function () {
                return this.discordData.filter(item => {
                    if (item.username !== 'Bot เปิดเพลง' & this.gettype === 'user') {
                        return true
                    } else if (item.username !== '' & this.gettype === 'all') {
                        return true
                    }else if (item.username === 'Bot เปิดเพลง' & this.gettype === 'bot') {
                        return true
                    } else {
                        return false
                    }
                })
            }
        },
        methods: {
            getFirst: function () {
                axios
                    .get('https://discord.com/api/guilds/574794024712405003/widget.json')
                    .then((response) => {
                        this.discordData = response.data.members
                        this.discordDataAll = response.data
                    })
                    .catch((error) => {
                        console.error(error)
                    });
            },
            selectType: function (type) {
                this.getFirst();
                this.gettype = type
            }

        },
        created() {
            this.getFirst();
        }
    }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Kanit&display=swap');

  #app,
  body {
    font-family: 'Kanit', sans-serif;
  }

  .inputTrackid {
    width: 110px;
  }
</style>