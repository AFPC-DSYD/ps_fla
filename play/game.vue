<template>
	<div class="container">
        <div class="container">
            <div class="row">
                <div class="col"></div>
                <div class="col-auto">
                        <button type="button" 
                                class="btn btn-danger btn-rounded btn-sm waves-effect" 
                                @click="reset">Reset</button>
                </div>
            </div>
            <div class="row">
                <div class="col-5">
                    <div v-for="(row, rowIndex) in grid" class="row gridRow">
                        <button v-for="(item , index) in row"  :class="['btn', getClass(rowIndex, index, item),'btn-sm','myBtn']"> {{ rowIndex==positionY&&index==positionX ? 'A' : '' }} </button>
                    </div>
                </div>
                <div class="col-3">
                    <div class="controls well">
                        <div class="row controlMid">
                            <button  :class="['btn','btn-secondary','btn-sm','myBtn']" @click="choose(2)">up</button>
                        </div>
                        <div class="row controlDub">
                            <button  :class="['btn','btn-info','btn-sm','myBtn']" @click="choose(1)">left</button>
                            <button  :class="['btn','btn-info','btn-sm','myBtn','controlMid']" @click="choose(3)">right</button>
                        </div>
                        <div class="row controlMid">
                            <button  :class="['btn','btn-secondary','btn-sm','myBtn']" @click="choose(4)">down</button>
                        </div> 
                    </div>
                </div>
                <div class="col-2">
                    <div class="card">
                        <div class="middle"> {{ moves }}</div>
                        <div class="card-body">
                            <h5 class="card-title middleTitle"> {{ card }} </h5>
                        </div>
                    </div>
                </div>
                <div class="col-2">
                    <div class="card">
                        <div class="middle"> {{ level+1 }}/{{ maxLevel }} </div>
                        <div class="card-body">
                            <h5 class="card-title middleTitle"> Level </h5>
                        </div>
                    </div>
                </div>
            </div>
            <div v-if="message" class="row">
                <div class="col-12">
                    <h1 class="messageClass"> {{ message }}  </h1>
                </div>
            </div>
        </div>
        <div class="row">
            
        </div>
	</div>
</template>

<script>

	export default {
        data() {
            return {
               loaded: true,
               grid: [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]],
               positionX: 2,
               positionY: 2,
               goal: [[3,1],[0,1]],
               level: 0,
               moves: 10,
               card: 'Moves',
               win: false,
               lose: false,
               start: false,
               stop: false,
            }
        },
        watch: {
            level:function(val){
                if(val == 1){
                    this.moves+=5
                    this.grid = [[0,1,0,0],[0,1,0,0],[0,1,0,0],[0,1,1,1]]
                }
            }
        },
        computed: {
            cols:function(){
                return this.grid[0].length
            },
            rows:function(){
                return this.grid.length
            }, 
            maxLevel:function(){
                return this.goal.length
            },
            message: function(){
                if (this.win)
                    return 'YOU WIN'
                else if (this.lose)
                    return 'YOU LOSE'
                else return ''
            }
        },
        methods: {
            reset(){
               this.grid= [[0,1,1,0],[0,1,1,0],[0,1,1,0],[0,1,1,0]]
               this.positionX= 2
               this.positionY= 2
               this.goal= [[3,1],[0,1]]
               this.level= 0
               this.moves= 10
               this.card= 'Moves'
               this.win= false
               this.lose= false
               this.start= false
               this.stop= false
            },
            getClass(row, col, allowed){
                if (row==this.positionY&&col==this.positionX)
                    return 'btn-success'
                else if (row==this.goal[this.level][0]&&col==this.goal[this.level][1])
                    return 'btn-warning'
                else if (allowed == 1)
                    return 'btn-'
                else return 'btn-danger';
            },
            choose(val){
                if (!this.start)
                    this.start = true
                if (this.moves > 0 && !this.stop ){
                    if (val == 1 && this.positionX>0 && this.grid[this.positionY][this.positionX-1])
                        this.positionX-=1;
                    else if (val == 3 && this.positionX<this.cols-1 && this.grid[this.positionY][this.positionX+1])
                        this.positionX+=1;
                    else if (val == 2 && this.positionY>0 && this.grid[this.positionY-1][this.positionX])
                        this.positionY-=1;
                    else if (val == 4 && this.positionY<this.rows-1 && this.grid[this.positionY+1][this.positionX])
                        this.positionY+=1;
                    if (this.positionX == this.goal[this.level][1] && this.positionY == this.goal[this.level][0]){
                        if (this.level == this.maxLevel-1){
                            this.win = true
                            this.stop= true
                        }
                        else this.level+=1
                    }
                    this.moves -= 1
                }

                if (this.moves == 0 && !this.win){
                    this.stop= true
                    this.lose = true
                }
            }
        },
        created: function(){
        	console.log('created')

        },
        mounted() {
            console.log('mounted')
    
        },
        beforeUpdate() {
            console.log("beforeupdate")
        },
        beforeDestroy() {
            console.log("beforeDestroy")
        },
        destroyed() {
            console.log("destroyed")
        }
    }

</script>

<style scoped>
    .gridRow button{
        height: 2rem;
        width: 2rem;
    }
    .controlMid{
        margin-left: 3rem;
    }
    .middle{
        margin: 0 30%;
        font-weight: bold;
        font-size: 2rem;
    }

    .middle2{
        margin: 0 20%;
        font-weight: bold;
        font-size: 2.5rem;
    }
    .middleTitle{
        margin: 0 15%;
    }

    .card{
        height: 6rem;
    }
    .middleTitle2{
        margin: 0 20%;
    }
    .messageClass{
        margin: 0 20%;
        font-size: 5rem;
        font-weight: bold;
    }
</style>