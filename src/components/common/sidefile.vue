<template>
  <div
    class="sidefile"
    @click.stop="
      showchild();
      markpath();
    "
    @mouseover.stop="over"
    @mouseleave.stop="leave"
  >
    <div class="hov">
      <img :src="img" alt="" /> <span>{{ name }}</span>
    </div>
    <div class="childsidefile" v-show="state.ifshow">
      <div v-for="(item, index) in childfiles" :key="index">
        <sidefile
          :name="item.name"
          :img="fileimg[item.type - 1]"
          :childfiles="item.files"
          :fileimg="fileimg"
          :file="item"
          @markpath="markpath"
        ></sidefile>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, defineComponent, reactive } from "vue";
import { useStore } from "vuex";
export default defineComponent({
  name: "sidefile",
  props: {
    name: String,
    img: String,
    childfiles: Array,
    fileimg: Array,
    file: Object,
  },
  emits: ["markpath"],
  setup(props, { emit }) {
    let state = reactive({
      store: useStore(),
      ifshow: false,
      ifover: false,
      overflag: false,
    });
    const showchild = () => {
      state.ifshow = !state.ifshow;
      state.store.commit("refreshcontentfile", props.childfiles);
    };
    const markpath = () => {
      if (state.store.state.pathflag) {
        state.store.commit("endpushpath");
        state.store.commit("clearpath");
        state.store.commit("changeflag");
      }
      state.store.commit("reditpath", props.file);
      emit("markpath");
    };
    return {
      state,
      showchild,
      markpath,
    };
  },
});
</script>

<style lang="scss">
.sidefile {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-wrap: wrap;
  padding: 5px 0;
  padding-left: 25px;
  .hov {
    &:hover {
      background-color: rgb(230, 227, 224);
    }
    display: flex;
    flex-wrap: nowrap;
    align-items: center;
    img {
      vertical-align: middle;
      margin: 3px;
      width: 25px;
      height: 25px;
    }
    span {
      font-size: 18px;
      vertical-align: middle;
    }
  }

  .childsidefile {
    width: 100%;
  }
}
.sidefile1 {
  @extend .sidefile;
  background-color: rgb(230, 227, 224);
}
</style>