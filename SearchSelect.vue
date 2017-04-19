<template>
  <div class="input-group"
       :style="'position:relative;padding-bottom:0px;width:'+(width?width:250)+'px;'">
    <div @click="openSelect"
         class="bootstrap-tagsinput"
         style="width:100%">
      <template v-if="!chosenList.length">
        <span class="tag label label-default text-muted qc-label">{{ label}}</span>
      </template>
      <template v-if="chosenList.length">
        <span class="tag label label-default text-muted qc-label"
              v-for="(item,index) in chosenList">{{ short(item.label) }}
                                                                <a href="javascript:;"class="qc-tag-x" @click.stop="delSelect(index)">x</a>
                                                          </span>
      </template>
    </div>
    <data-item-list v-bind="$props"></data-item-list>
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
    },
    delSelect(index) {
      this.$children[0].delSelect(index);
    },
    short(label) {
      if (label.length > 6) {
        return label.substring(0, 3)+'...';
      }
    }
  },
  components: {
    DataItemList
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
</style>