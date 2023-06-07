<template>
  <section class="score">
    Player
    <span class="player-score">{{ this.winCount }}</span>
    x
    <span class="computer-score">{{ this.loseCount }}</span>
    Computer
  </section>
</template>

<script>
  export default {
    props: [
      'winCount',
      'loseCount'
    ]
  }
    
  
</script>

<style lang="scss">
.score{
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  widows: 100%;
  text-align: center;
  font-size: 18px;
  padding-bottom: 20px;
  border-bottom: 1px solid #000000;
  span{
    font-weight: 600;
    padding: 5px;
    border: 1px solid #000000;
    &.player-score{
      color: #1867C0;
    }
    &.computer-score{
      color: #F24444;
    }
  }
}
</style>
