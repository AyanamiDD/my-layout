<template>
  <div class="notify-container">
    <div class="icon-wrapper">
      <i class="el-icon-message-solid"></i>
    </div>
    <div class="content">
      <el-tooltip placement="top">
        <div slot="content">
          道路<span style="color: red">{{ data.place }}</span>
          行驶过未处理违章车辆：<span style="color: red">
            {{ data.licensePlateNumber }}</span>
          </div>
        <div class="title text-cut">
          道路<span style="color: red">{{ data.place }}</span>
          行驶过未处理违章车辆：<span style="color: red">
            {{ data.licensePlateNumber }}</span>
        </div>
      </el-tooltip>
      <div class="time">{{ data.date }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "NotifyItem",
  props: ["data"],
};
</script>

<style lang="scss" scoped>
.notify-container {
  padding: 10px;
  display: flex;
  justify-content: flex-start;
  .icon-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    min-width: 30px;
    max-width: 30px;
    height: 30px;
    border-radius: 50%;
    color: currentColor;
    background-color: rgb(254, 92, 87);
    font-size: 18px;
  }
  .content {
    flex: 1;
    margin-left: 3px;
    cursor: pointer;
    .title {
      width: 230px;
      font-size: 14px;
      text-overflow:ellipsis;
      color: currentColor;
    }
    .time {
      margin-top: 2px;
      font-size: 12px;
    }
  }
}
</style>
