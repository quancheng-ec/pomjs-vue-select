<template>
  <div style="position: absolute;">
    <div class="well qc-well"
         v-if="open"
         style="width :100%;">
      <div class="form-group"
           style="width:100%;padding:5px;"
           v-if="url">
        <div class="input-group "
             style="width:100%;">
          <input type="text"
                 focus
                 v-model="keyword"
                 class="form-control "
                 :placeholder="placeholder?placeholder:'搜索'">
          <a href="javascript:;"
             class="qc-search"
             @click="search">
            <i class="fa fa-search "></i>
          </a>
        </div>
      </div>
      <div class="list-group qc-searchselect-listgroup ">
  
        <template v-for="it in data">
          <div v-if="onlyLeaf === 'true'"
               class="list-group-title">
            <strong>{{it.label}} </strong>
          </div>
          <a v-if="onlyLeaf !== 'true'"
             href="javascript:void(0)"
             class="list-group-item"
             @click="selectItem(it)">
            <strong>{{it.label}} </strong>
          </a>
          <template v-if="it.children&&it.children.length">
            <li class="qc-divider"></li>
            <a href="javascript:void(0)"
               v-for="itt in it.children"
               @click="selectItem(itt)"
               :class="selectKeys[itt.id]?' list-group-item active':' list-group-item'"
               style="padding-left:35px;"> {{itt.label}} </a>
          </template>
  
        </template>
      </div>
    </div>
    <div v-if="open"
         @click="open=false;"
         style="z-index:9998;position:fixed;top:0;left:0;width:100%;height:100%"></div>
  
  </div>
</template>

<script>

import debounce from 'lodash/debounce';

export default {
  props: ['label', 'chosenList', 'url', 'isSingle', 'items', 'width', 'placeholder', 'onlyLeaf'],
  data() {
    return {
      open: false,
      keyword: '',
      top: 39,
      searching: false,
      selectItems: [],
      data: [],
      selectKeys: {}
    }
  },
  created() {
    this.init();
  },
  watch: {
    keyword: debounce(function () {
      this.search();
    },
      250
    ),
    selectItems: function () {
      if (this.chosenList) {
        this.selectKeys = {};
        this.chosenList.splice(0, this.chosenList.length);
        this.selectItems.forEach(it => {
          if (!this.selectKeys[it.id]) {
            this.chosenList.push(it);
            this.selectKeys[it.id] = it;
          }

        });
      }
      if (this.el) {
        // const rect = this.el.getBoundingClientRect();
        // this.$nextTick(r => {
        //   // 由于dom操作是异步的，所以 用 nextTick 强制同步
        //   this.top = this.el.offsetHeight;
        // });

        this.isElementInViewport(this.el);
      }
    },
    url: function () {
      this.search();
    }
  },
  mounted() {
    this.search();
  },
  methods: {
    init() {
      if (this.items && this.items.length > 0) {
        this.items.forEach(it => {
          this.data.push(it);
        });
      }

      if (this.chosenList && this.chosenList.length > 0) {
        this.chosenList.forEach((c, index) => {
          this.selectItems.push(c);
        });
      }
    },
    delSelect(index) {
      this.selectItems.splice(index, 1);
    },
    selectItem(item) {
      if (this.isSingle === 'true') {
        this.selectItems.splice(0, this.selectItems.length);
        this.open = false;
      }
      this.selectItems.push(item);

    },
    search() {
      if (!this.url) {
        return;
      }
      if (this.searching) {
        return;
      }
      this.searching = true;
      this.$http.post(this.url, {
        keyword: this.keyword
      }).then(res => {
        //this.open = true;
        this.searching = false;
        this.data = res.data.data.selectData;
        // each({}, this.data, false);
      });
    },
    openSelect(e) {
      if (e && e.target) {
        this.el = e.currentTarget;
        this.isElementInViewport(this.el);
        console.log(this.el, this.el);
        this.open = true;
      }
      //this.search();
    },
    isElementInViewport: function (el) {

      this.$nextTick(r => {
        let windowHeight = (window.innerHeight || document.documentElement.clientHeight);
        var rect = el.getBoundingClientRect();
        this.top = rect.height;
        if (rect.bottom + 200 > windowHeight) {
          this.top = '-239px';
        }
      });

    }
  },
  components: {

  }

}

</script>

<style lang="stylus" rel="stylesheet/stylus">

.qc-well{
  z-index:9999;
  position:absolute;
  border: 1px solid #e4e7ea;
  background: #fff;
  padding: 0px;
  min-width:250px;
}

.qc-well .list-group a{
  padding:8px 20px;
}


.qc-divider{
    height: 1px;
    margin: 1px 0;
    overflow: hidden;
    background-color: #e5e5e5;
}

.qc-search {
    position: absolute;
    z-index:10000;
    top: 30%;
    width:32px;
    height:32px;
    right: -10px;
    margin-top: -2px;
    vertical-align: middle;
}

.qc-searchselect-listgroup{
  max-height:200px;
  overflow-y:auto;
  overflow-x:hidden;
}

.list-group-title{
  padding: 8px 20px;
  padding-left: 35px;
}

.qc-label{
  margin:2px;
  display:inline-block;
}
.qc-tag-x {
  padding: 0px 0px 0px 8px;
  margin-right: -8px;
}
</style>