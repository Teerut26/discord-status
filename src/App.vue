<template>
    <div>
        <div class="container mt-3">
            <button class="btn btn-success" @click="selectType('all')">ALL</button>
            <button class="btn btn-success ml-2" @click="selectType('user')">User</button>
            <button class="btn btn-success ml-2" @click="selectType('bot')">Bot</button>
            <button class="btn btn-primary ml-2" @click="selectShow2()" v-if="showTable === 1">ShowImage</button>
            <button class="btn btn-danger ml-2" @click="selectShow2()" v-else>NotShowImage</button>
            <button class="btn btn-primary ml-2 mt-2" @click="screenCap()">CapImage</button>
            <div class="table-responsive" v-if="showTable === 1">
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
                            <td style="padding: 0px; width: 50px; height: 50px;"><img :id="item.id+'_image'" :src="item.avatar_url"
                                    alt="" srcset="" width="50" height="50"></td>
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
                <img :src="imgDataUrl" alt="" srcset="">
            </div>
            <div class="table-responsive" v-if="showTable === 2">
                <table class="table table-striped table-bordered mt-2">
                    <thead class="thead-dark">
                        <tr>
                            <th>Username</th>
                            <th>mute</th>
                            <th>status</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in noBot" :key="item">
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
                <img :src="imgDataUrl" alt="" srcset="">
            </div>
        </div>
    </div>
</template>

<script>
    // @ is an alias to /src
    const axios = require('axios');
    const html2canvas = require('html2canvas');
    // import Card from '@/components/Card.vue'

    export default {
        name: 'Discord',
        components: {},
        data() {
            return {
                NavbarTitle: "Discord",
                card_title: "Discord",
                discordData: [],
                discordDataAll: [],
                gettype: "all",
                imgDataUrl: "",
                showTable:1,
            }
        },
        computed: {
            noBot: function () {
                return this.discordData.filter(item => {
                    if (item.username !== 'Bot เปิดเพลง' & this.gettype === 'user') {
                        return true
                    } else if (item.username !== '' & this.gettype === 'all') {
                        return true
                    } else if (item.username === 'Bot เปิดเพลง' & this.gettype === 'bot') {
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
            },
            selectShow2: function () {
                if (this.showTable === 1) {
                    this.showTable = 2;
                }else{
                    this.showTable = 1;
                }
            },
            screenCap: function () {
                this.showTable = 2;
                var offScreen = document.querySelector('tbody');
                // Use clone with htm2canvas and delete clone
                html2canvas(offScreen).then((canvas) => {
                    
                    var dataURL = canvas.toDataURL();
                    console.log(dataURL);
                    var link = document.createElement('a');
                    link.download = `receiptify.png`;
                    link.href = dataURL;
                    this.imgDataUrl = dataURL;
                    // document.body.appendChild(link);
                    // link.click();
                    // document.body.removeChild(link);
                });
            },

        },
        created() {
            this.getFirst();
        }
    }
</script>

