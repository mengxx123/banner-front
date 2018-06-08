<template>
    <my-page title="首页">
        <!--<img src="/static/img/banner-1.jpg">-->
        <!--<br>-->
        <canvas class="canvas" id="canvas"></canvas>
        <br>
        <div v-if="text">
            <ui-text-field class="input" v-model="text.text0" />
            <br>
            <ui-text-field class="input" v-model="text.text1" />
        </div>
        <div>

        </div>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                data: null,
//                text: null,
                text: {
                    text0: '招募Logo',
                    text1: '不限时间，不限地点'
                },
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.init()
        },
        methods: {
            init() {
                let data = {
                    id: '1',
                    image: '/static/img/banner-1-bg.jpg',
                    texts: [
                        {
                            text: '招募Logo设计师',
                            x: 208,
                            y: 46,
                            style: 'bold 40px 微软雅黑',
                            color: '#fff'
                        },
                        {
                            text: '不限时间，不限地点，在家一样享受高薪酬',
                            x: 170,
                            y: 80,
                            style: '20px 微软雅黑',
                            color: '#fff'
                        }
                    ]
                }

                this.data = data
                let idx = 0
//                this.text = {}
                for (let text of this.data.texts) {
                    this.$set(this.text, 'text' + idx++, text.text)
                }
                this.draw()
            },
            draw() {
                let canvas = document.getElementById('canvas')
                let ctx = canvas.getContext('2d')
                let img = new Image()
                ctx.globalAlpha = 0.1
                img.src = this.data.image
                console.log(this.text)
                img.onload = e => {
                    canvas.setAttribute('width', img.width)
                    canvas.setAttribute('height', img.height)
                    ctx.width = img.width
                    ctx.height = img.height
                    ctx.drawImage(img, 0, 0)
                    for (let text of this.data.texts) {
                        ctx.font = text.style
                        ctx.fillStyle = text.color
                        ctx.fillText(text.text, text.x, text.y)
                    }
                }
            }
        },
        watch: {
            text: {
                deep: true,
                handler() {
                    console.log('变化')
                    let idx = 0
                    for (let text of this.data.texts) {
                        text.text = this.text['text' + idx++]
                    }
                    this.draw()
                }
            }
        }
    }
</script>

<style scoped>
    .canvas {
        border: 1px solid #eee;
        /*background-image: url("/static/img/banner-1.jpg");*/
    }
    .input {
        width: 100%;
        max-width: 320px;
    }
</style>
