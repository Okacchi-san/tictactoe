<template>
  <div>
    <MyModal v-if="isDrawState">
      {{ }}
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
      modal: false,
      isDrawState: false,
    }
  },
  methods: {
    openModal() {
      this.modal = true
    },
    closeModal() {
      this.modal = false
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
        this.states = states;
        this.playerID = (this.playerID === 1) ? 2 : 1;

        let winnerId = this.getWinnerId();

        if(winnerId != -1) {
          this.init();
          this.playerIds = {
            1:'○',
            2:'×'
          };
          alert(this.playerIds[winnerId] + 'さんの勝ちです。おめでとうございます。!');
        }else{
          if(this.isDraw()){
            this.isDrawState = true;
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
          return col[0][i];
        }
      }

      let skew1 = [
        this.states[0][0],
        this.states[1][1],
        this.states[2][2]
      ];
      if(this.isStatesFilled(skew1)) {
        return skew1[0][0];
      }

      let skew2 = [
        this.states[0][2],
        this.states[1][1],
        this.states[2][0]
      ];
      if(this.isStatesFilled(skew2)) {
        return skew2[0][2];
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

  created() {
    console.log(this.states)
  },
  mounted: function(){
    this.init();
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
