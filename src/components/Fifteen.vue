<template>
    <div class="wrapper" :key="componentKey">
        <button @click="refreshGame" class="button">Перезапустить игру</button>
        <div v-if="win" class="won">Вы выиграли!!!</div>
        <div ref="field" class="field">
            <div 
            class="cell" 
            v-for="(item, i) in array" 
            :key="item"
            :style="{left: getLeft(i+1) * 100 + 'px', top: getTop(i+1) * 100 + 'px'}"
            @click="clickItem"
            :data-index="i+1"
            >{{item}}</div>
        </div>
    </div>
</template>

<script>

export default {

  data() {
    return {
      componentKey: 0,
      array:[],
      cellSize: 100,
      empty: {
        top: 0,
        left: 0,
        index: 0
      },
      win: false
    }
  },

  computed: {

  },
  mounted () {
    this.generateArr()

  },

  methods: {
    refreshGame(){
        this.componentKey += 1; 
        this.empty = {
            top: 0,
            left: 0,
            index: 0
        }
        this.generateArr()
        
    },

    generateArr(){
      return this.array = [...Array(15).keys()].map(x => x + 1).sort(()=>Math.random()-0.5)
      // return this.array = [...Array(15).keys()].map(x => x + 1)
    },

    getLeft(item){
      return item % 4
    },

    getTop(item) {
      let result = (item - this.getLeft(item)) / 4
      return result
    },

    move(cell){
      let left = cell.style.left
      let top = cell.style.top
      let index = cell.dataset.index

      cell.style.left = this.empty.left + 'px'
      cell.style.top = this.empty.top + 'px'
      cell.dataset.index = this.empty.index
      
      this.empty.left = parseFloat(left)
      this.empty.top = parseFloat(top)
      this.empty.index = index

      this.isWin() ? this.win = true : this.win = false

    },

    clickItem(event) {
      let cell = event.target
      const leftDiff = Math.abs(this.empty.left - parseFloat(cell.style.left))
      const topDiff = Math.abs(this.empty.top - parseFloat(cell.style.top))
      
      if(leftDiff + topDiff == 100){
        this.move(cell)
      }
    },

    isEqual(data1, tata2){
      
    },

    isWin(){

      let fields = this.$refs.field.querySelectorAll("div[data-index]")
      let arr = []
      fields.forEach(elem =>{
        let obj = {}
        obj.key = elem.dataset.index
        obj.value = elem.innerHTML
        arr.push(obj)
      })
    
      return arr.every( elem => elem.key == elem.value)
      
    }

    
  },
}
</script>

<style lang="scss">
.wrapper {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.button {
    margin-bottom:20px
}

.won {
  width: 400px;
  box-sizing: border-box;
  padding: 20px;
  margin-bottom: 20px;
  background-color: green;
  color: white;
  font-size: 20px;
}

.field {
  width: 400px;
  height: 400px;
  border: 1px solid black;
  border-radius: 10px;
  position: relative;
}

.cell {
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 3px solid blue;
  border-radius: 10px;
  position: absolute;
  font-size: 50px;
  cursor: pointer;
  transition: all 0.3s;
}



</style>