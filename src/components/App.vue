<template>
  <div>
    <MyModal @close="closeModal" v-if="isDrawModal">
      <template slot="footer">
        <p>引き分けです!</p>
        <button @click="doClose">閉じる</button>
      </template>
    </MyModal>
    <MyModal @close="closeModal" v-else-if="isWinModal">
      <template slot="footer">
        <p v-bind:class="[onSelect, getWinnerId]">{{ playerIds[this.getWinnerId()] }}さんの勝ちです。おめでとうございます。!</p>
        <button @click="doClose">閉じる</button>
      </template>
    </MyModal>
    
    <h1>Tic Tac Toe</h1>
      <table id ="table">
        <tr v-for="(row,rowsIndex) in states" v-bind:key="rowsIndex">
          <td v-for="(state,colsIndex) in row" v-bind:key="colsIndex" v-on:click="onSelect(rowsIndex,colsIndex)">
            <div class="player-id-1" v-if="state === 1">○</div>
            <div class="player-id-2" v-if="state === 2">×</div>
          </td>
        </tr>
      </table>
      <div class="player-id-1" id="player-comment" v-if="playerID === 1">「○ プレーヤーさん、マスを選択してください。」</div>
      <div class="player-id-2" id="player-comment" v-if="playerID === 2">「× プレーヤーさん、マスを選択してください。」</div>
  </div>
</template>

<script>
import MyModal from './MyModal.vue';

export default{
  components: { MyModal },

  data: function() {
    return {
      states: [],
      playerID: 1,
      isWinModal: false,
      isDrawModal: false,
    }
  },
  methods: {
    openModal() {
      this.modal = true
    },
    closeModal() {
      this.isWinModal = false,
      this.isDrawModal = false,
      this.init();
    },
    doClose() {
      this.closeModal()
    },
    init: function(){
      this.states = [
        [-1,-1,-1],
        [-1,-1,-1],
        [-1,-1,-1]
      ];
    },
    onSelect: function(rowsIndex, colsIndex) {
      if(this.states[rowsIndex][colsIndex] != -1) {
        alert('そのマスはすでに選択されています。');
      } else {
        let states = JSON.parse(JSON.stringify(this.states))
        states[rowsIndex][colsIndex] = this.playerID;
        // console.log(typeof(this.playerID)) number
        
        this.states = states;
        
        this.playerID = (this.playerID === 1) ? 2 : 1;

        let winnerId = this.getWinnerId();
        console.log(winnerId)
        if(winnerId != -1) {
          
          this.playerIds = {
            1:'○',
            2:'×'
          };
            // this.init();
            this.isWinModal = true;
          // alert(this.playerIds[winnerId] + 'さんの勝ちです。おめでとうございます。!');
        }else{
          if(this.isDraw()){ //isDraw === trueの場合以下がを実行する
            this.isDrawModal = true;
          }
        }
        
      }
    },
    getWinnerId: function() {
      for(let i =0; i < 3; i++) {

        let row = this.states[i];
        if(this.isStatesFilled(row)) {
          return row[0];
        }

        let col = [
          this.states[0][i],
          this.states[1][i],
          this.states[2][i]
        ];
        if(this.isStatesFilled(col)) {
          // bug fix: col[0][i]をcol[0]へ。
          return col[0];
        }
      }

      let skew1 = [
        this.states[0][0],
        this.states[1][1],
        this.states[2][2]
      ];
      console.log(skew1[0])
      if(this.isStatesFilled(skew1)) {
        // bug fix: skew1[0][0]をskew1[0]へ。
        return skew1[0];
      }

      let skew2 = [
        this.states[0][2],
        this.states[1][1],
        this.states[2][0]
      ];
      if(this.isStatesFilled(skew2)) {
        // bug fix: skew2[0][0]をskew2[0]へ。
        return skew2[0];
      }

      return -1;

    },
    isStatesFilled: function(states) {
      if(states[0] === -1) {
        return false;
      }
      if(states[0] != states[1]) {
        return false;
      }
      if(states[1] != states[2]) {
        return false;
      }

      return true;
    },
    isDraw: function(){
      let flag = true;
      this.states.map(row => {
        row.map(state => {
          if(state === -1) {
            flag = false;
          }
        })
      })
      return(flag);
    },
    isWin: function() {
      let flag = false;
      if(this.winnerId != -1) {
        return true;
      }
      return(flag);
    }
  },

  // created() {
  //   console.log(this.states)
  // },
  mounted: function(){
    this.init();
  },
  updated: function(){
    // console.log(this.states)
    // console.log(typeof(this.getWinnerId()))
  }
};

</script>

<style scoped>
  h1 {
    text-align: center;
  }

  #table {
    margin: 0 auto;
    border-collapse: collapse;
    border: 3px solid #ccc;
  }

  #table td {
    border: 1px solid #ccc;
    height: 80px;
    width: 80px;
    text-align: center;
    vertical-align: middle;
    font-size: 50px;
    cursor: pointer;
  }

  .player-id-1 {
    color: #f00;
  }

  .player-id-2 {
    color: #00f;
  }

  #player-comment {
    margin: 20px 0 0 0;
    text-align: center;
  }
</style>
