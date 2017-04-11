<template>
  <div class="input-group"
       :style="'position:relative;padding-bottom:0px;width:'+(width?width:250)+'px;'">
    <div @click="openSelect"
         class="bootstrap-tagsinput"
         style="width:100%"
         id="search-select">
      <template v-if="!selectItems.length">
        请选择...
      </template>
      <template v-if="selectItems.length">
        <span class="tag label label-default text-muted qc-label"
              v-for="(item,index) in selectItems">{{item.label}}
                                                        <a href="javascript:;"class="qc-tag-x" @click.stop="delSelect(index)">x</a>
                                                  </span>
      </template>
      <data-item-list v-bind="$props"></data-item-list>
    </div>
  </div>
</template>
<script>
import DataItemList from './components/DataList.vue';
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