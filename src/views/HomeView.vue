<script setup lang="ts">
  // type notes = {
  //   name: 'Do' | 'Re'
  // }
  // const notes
</script>

<template>
  <main>
    <ul class="board">
      <li class="note"></li>
      <li class="note"></li>
      <li class="note"></li>
      <li class="note"></li>
    </ul>
  </main>
</template>


<style lang="scss" scoped>
@use 'sass:math';
$height: 40px;
ul.board {
  @apply flex;
  margin: math.div($height,2) 0;

  .note {
    @apply relative;
    height: $height*5;
    width: $height;
    background: repeating-linear-gradient(
      0deg,
      transparent math.div($height,2),
      black math.div($height,2) math.div($height,2)+1,
      transparent math.div($height,2)+1 $height+math.div($height,2)
    );

    &::before {
      content: '';
      @apply absolute block rounded-full;
      width: $height;
      height: $height;
      @apply bg-black;
      top: 0;
      left:0
    }
  }
}
</style>