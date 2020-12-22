<template>
  <div>
    <h1>Tic Toc Toe</h1>
      <table id ="table">
        <tr>
          <td></td>
          <td></td>
          <td></td>
        </tr>
      </table>
  </div>
</template>

<script>
export default{

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
</style>