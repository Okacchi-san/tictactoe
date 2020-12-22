<template>
  <div>
    <h1>Tic Toc Toe</h1>
      <table id ="table">
        <tr v-for="(row,rowsIndex) in states" v-bind:key="rowsIndex">
          <td v-for="(state,colsIndex) in row" v-bind:key="colsIndex">
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
export default{
  data: function() {
    return {
      states: [
        [2,2,2],
        [-1,-1,-1],
        [1,1,1]
      ],
      playerID: 1
    }
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
    font-size: 75px;
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