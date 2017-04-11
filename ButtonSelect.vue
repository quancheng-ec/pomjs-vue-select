<template>
  <div class="input-group"
       :style="'position:relative;padding-bottom:0px;width:'+(width?width:250)+'px;'">
  
    <button type="button"
            @click="openSelect"
            class="btn  btn-info btn-outline">{{label}}</button>
  
    <data-item-list v-bind="$props"></data-item-list>
  </div>
</template>

<script>
import DataItemList from './DataList.vue';
export default {
  props: ['label', 'chosenList', 'url', 'isSingle', 'items', 'width', 'placeholder'],
  data() {
    return {
    }
  },
  methods: {
    openSelect(e) {
      this.$children[0].openSelect(e);
    }
  },
  components: {
    DataItemList
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
</style>