<template>
    <div class="game">
        <div class="content">
            <div class="top">
                <h2>2048</h2>
                <div class="right">
                    <p>Your score: <span>{{score}}</span></p>
                    <p>Best score: <span>{{bestScore}}</span></p>
                </div>
            </div>
            <div class="btns">
                <button class="newGame">New game</button>
                <button class="scoreList">Score list</button>
            </div>
            <canvas ref="canvas" class="canvas"></canvas>
        </div>
    </div>
</template>

<script>
export default {
    data: () => ({
        ctx: null,
        score: 0,
        params: {
            h: 540,
            w: 540,
            rows: 4,
            cols: 4,
            spaceBetween: 20,
            cellHeight: null,
            cellWidth: null
        }
    }),
    computed: {
        bestScore() {
            return this.getSpaces(9402)
        }
    },
    mounted() {
        this.$nextTick(() => {
            this.init();
        })
    },
    methods: {
        getSpaces(num) {
            return num.toLocaleString();
        },
        init() {
            this.canvas = this.$refs.canvas;
            this.ctx = this.canvas.getContext('2d');

            this.canvas.width = this.params.w;
            this.canvas.height = this.params.h;
            this.canvas.style.minWidth = `${this.params.w}px`;
            this.canvas.style.minHeight = `${this.params.h}px`;
            this.canvas.style.maxWidth = `${this.params.w}px`;
            this.canvas.style.maxHeight = `${this.params.h}px`;

            this.params.cellHeight = (this.params.h - (this.params.spaceBetween * 2) - ((this.params.cols - 1) * this.params.spaceBetween)) / this.params.cols;
            this.params.cellWidth = (this.params.w - (this.params.spaceBetween * 2) - ((this.params.rows - 1) * this.params.spaceBetween)) / this.params.rows;

            this.ctx.fillStyle = '#BBADA0';
            this.ctx.fillRect(0, 0, this.params.w, this.params.h);

            console.log(this.params);

            for (let i = 0; i <= this.params.rows; i++) {
                for (let j = 0; j <= this.params.cols; j++) {
                    this.ctx.fillStyle = '#CDC1B4';
                    this.roundRect(
                        this.ctx,
                        this.params.spaceBetween + (j * this.params.cellWidth) + (this.params.spaceBetween * j),
                        this.params.spaceBetween + (i * this.params.cellHeight) + (this.params.spaceBetween * i), 
                        this.params.cellWidth, 
                        this.params.cellHeight, 
                        10,
                        '#CDC1B4'
                    );
                }
            }
        },
        roundRect(ctx, x, y, width, height, radius, fill) {
            if (typeof radius === 'undefined') {
                radius = 10;
            }
            if (typeof radius === 'number') {
                radius = {tl: radius, tr: radius, br: radius, bl: radius};
            } else {
                let defaultRadius = {tl: 0, tr: 0, br: 0, bl: 0};
                for (let side in defaultRadius) {
                    radius[side] = radius[side] || defaultRadius[side];
                }
            }
            ctx.beginPath();
            ctx.moveTo(x + radius.tl, y);
            ctx.lineTo(x + width - radius.tr, y);
            ctx.quadraticCurveTo(x + width, y, x + width, y + radius.tr);
            ctx.lineTo(x + width, y + height - radius.br);
            ctx.quadraticCurveTo(x + width, y + height, x + width - radius.br, y + height);
            ctx.lineTo(x + radius.bl, y + height);
            ctx.quadraticCurveTo(x, y + height, x, y + height - radius.bl);
            ctx.lineTo(x, y + radius.tl);
            ctx.quadraticCurveTo(x, y, x + radius.tl, y);
            ctx.closePath();
            if (fill) {
                ctx.strokeStyle = fill;
                ctx.fillStyle = fill;
                ctx.fill();
            }
        },
    }
}
</script>

<style lang="scss" scoped>
.game{
    overflow-x: hidden;
    width: 100%;
    height: 100%;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    .content{
        padding: 20px;
        width: 100%;
        height: 100%;
        max-width: 640px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        .top{
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            h2{
                font-weight: 700;
                font-size: 50px;
                color: #F74931;
            }
            .right{
                display: flex;
                flex-direction: column;
                align-items: flex-end;
                p{
                    margin-top: 7.5px;
                    margin-bottom: 7.5px;
                    font-weight: 400;
                    font-size: 25px;
                    span{
                        font-weight: 700;
                    }
                }
            }
        }
        .btns{
            width: 100%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 30px;
            margin-bottom: 30px;
            button{
                height: 45px;
                min-height: 45px;
                background: #FFD332;
                box-shadow: inset -2px -2px 4px rgba(0, 0, 0, 0.25);
                border-radius: 10px;
                display: flex;
                justify-content: center;
                align-items: center;
                border: 0;
                outline: 0;
                width: calc(50% - 10px);
                font-weight: 700;
                font-size: 20px;
                line-height: 23px;
                color: #FFFFFF;
            }
        }
        .canvas{
            width: 100%;
            max-width: 540px;
            min-width: 540px;
            height: 100%;
            max-height: 540px;
            min-height: 540px;
            border-radius: 10px;
            overflow: hidden;
        }
    }
}
</style>