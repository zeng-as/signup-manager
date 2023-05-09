<template>
    <div>
        <head-top></head-top>
        <el-row v-for="item in videoList" class="row-video">
            <el-col>
                <Video
                    v-show="true"
                    :videoSrc="item.video"
                    :width="800"
                    :height="450"
                    :autoplay="false"
                    :controls="true"
                    :loop="false"
                    :muted="false"
                    preload="auto"
                    :showPlay="true"
                    :playWidth="96"
                    zoom="contain"
                />
                <div>{{item.name}}课程回放，有效时间: {{item.videoValidStart}} - {{item.videoValidEnd}}</div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
    import headTop from '../components/headTop'
    import Video from "../components/video";
    import {getSignupClasses} from '@/api/getData'
    import dtime from "time-formater";

    export default {
        name: "videoList",
        data(){
            return {
                videoList: [
                ]
            }
        },
        components: {
            headTop,
            Video
        },
        mounted(){
            this.initData();
        },
        computed: {

        },
        methods: {
            async initData() {
                Promise.resolve(getSignupClasses()).then(res => {
                    console.log(res)
                    this.videoList = res.data
                }).catch(err => {
                    this.$notify.error({
                        title: '错误',
                        message: err,
                        offset: 100
                    });
                })
            }
        }
    }
</script>

<style scoped>
.row-video {
    padding: 20px;
    text-align: center;
}
</style>
