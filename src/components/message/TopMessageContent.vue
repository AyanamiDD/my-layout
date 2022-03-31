<template>
  <div style="width: 280px">
    <el-tabs v-model="activeName">
      <el-tab-pane :label="`通知${notifyNum}`" name="notify">
        <div class="info-main">
          <NotifyItem
            class="item"
            v-for="(item, index) in notifyData"
            :key="index"
            :data="item"
          />
        </div>

        <div class="bottom-wrapper">
          <el-link
            :underline="false"
            class="flex-sub text-center"
            @click="clear('notify')"
            >清空通知</el-link
          >
          <div class="text-gray">|</div>
          <el-link
            :underline="false"
            class="flex-sub text-center"
            @click="more('notify')"
            >查看更多</el-link
          >
        </div>
      </el-tab-pane>
      <el-tab-pane :label="`消息${messageNum}`" name="message">
        <div class="info-main">
          <MessageItem class="item" />
          <MessageItem class="item" />
          <MessageItem class="item" />
          <MessageItem class="item" />
          <MessageItem class="item" />
        </div>
        <div class="bottom-wrapper">
          <el-link :underline="false" class="action" @click="clear('message')"
            >清空消息</el-link
          >
          <div>|</div>
          <el-link :underline="false" class="action" @click="more('message')"
            >查看更多</el-link
          >
        </div>
      </el-tab-pane>
      <el-tab-pane :label="`待办${todoNum}`" name="todo">
        <div class="info-main">
          <TodoItem class="item" />
          <TodoItem class="item" />
          <TodoItem class="item" />
          <TodoItem class="item" />
          <TodoItem class="item" />
        </div>
        <div class="bottom-wrapper">
          <el-link
            :underline="false"
            class="flex-sub text-center"
            @click="clear('todo')"
            >清空待办</el-link
          >
          <div class="text-gray">|</div>
          <el-link
            :underline="false"
            class="flex-sub text-center"
            @click="more('todo')"
            >查看更多</el-link
          >
        </div>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import NotifyItem from "./components/NotifyItem";
import MessageItem from "./components/MessageItem";
import TodoItem from "./components/TodoItem";
import { requestGet } from "../../../../../src/utils/request/request";
export default {
  name: "MessageContent",
  components: {
    NotifyItem,
    MessageItem,
    TodoItem,
  },
  data() {
    return {
      activeName: "",
      notifyNum: "(4)",
      notifyData: [],
      messageNum: "(4)",
      todoNum: "(4)",
    };
  },
  created() {
    this.$socket.registerCallBack("getInfo", this.getInfo);
    this.getNotifyData()
  },
  methods: {
    update() {
      this.activeName = this.activeName === "0" ? "notify" : this.activeName;
    },
    clear(type) {
      if (!this[type + "Num"]) {
        return;
      }
      this[type + "Num"] = "";
      this.$emit("clear-num", 1);
    },
    more() {
      this.$successMsg("click more");
    },
    getNotifyData() {
      requestGet({
        url: "/getIllegalVehicle",
      }).then((res) => {
        this.notifyData = res.data.msg
        this.notifyNum = '('+ res.data.msg.length +')'
      });
    },
    getInfo(res) {
      this.notifyData.splice(0, 0, res);
      this.$notify({
        title: "警告",
        dangerouslyUseHTMLString: true,
        message:
          '道路<span style="color:red;">' +
          res.place +
          '</span>行驶过未处理违章车辆：<span style="color:red;">' +
          res.licensePlateNumber +
          "</span>",
        type: "warning",
        duration: 5000,
      });
      this.getNotifyData()
    },
  },
  destroyed() {
    this.$socket.unRegisterCallBack("getInfo");
  },
};
</script>

<style scoped>
.info-main {
  max-height: 333px;
  overflow-x: hidden;
}
.info-main::-webkit-scrollbar {
  /*滚动条整体样式*/
  width: 6px; /*高宽分别对应横竖滚动条的尺寸*/
  height: 1px;
}
.info-main::-webkit-scrollbar-thumb {
  /*滚动条里面小方块*/
  border-radius: 10px;
  background-color: skyblue;
  background-image: -webkit-linear-gradient(
    45deg,
    rgba(255, 255, 255, 0.2) 25%,
    transparent 25%,
    transparent 50%,
    rgba(255, 255, 255, 0.2) 50%,
    rgba(255, 255, 255, 0.2) 75%,
    transparent 75%,
    transparent
  );
}
.info-main::-webkit-scrollbar-track {
  /*滚动条里面轨道*/
  box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
  background: #ededed;
  border-radius: 10px;
}
</style>

<style lang="scss" scoped>
.bottom-wrapper {
  display: flex;
  justify-content: space-around;
  padding-top: 10px;
  border-top: 1px solid #f5f5f5;
  .action {
    flex: 1;
    text-align: center;
  }
}
</style>
