<template>
  <div class="light" :class="{
    on: on || permanent,
  }" />
</template>


<script>
export default {
  props: ['on', 'permanent'],
};
</script>

<style>
.light {
  width: 16px;
  height: 16px;
  border-radius: 8px;
  background: rgba(0, 0, 0, 0.7);
  box-shadow: 0px 0px 8px 1px rgba(30, 30, 0, 0.5);
}
.on {
  box-shadow: 0px 0px 8px 1px #ff0;
  background: rgba(255, 255, 0, 0.5);
}
.permanent {
  background: rgba(255, 0, 0, 0.8);
}
</style>
