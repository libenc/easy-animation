<template>
  <div id="trackBlock">
    <hr />
    <div id="timeLine">
      <p>TimeLine</p>
      <div id="timeContainer">
        <div class="time-item">00:00.000"</div>
        <div class="time-item">00:00.500"</div>
        <div class="time-item">00:01.000"</div>
        <div class="time-item">00:01.500"</div>
        <div class="time-item">00:02.000"</div>
        <div class="time-item">00:02.500"</div>
      </div>
    </div>
    <div id="frameTrack">
      <div
        class="frame"
        v-for="item in timeData"
        :key="item.index"
        :time="item.time"
      >
        {{ item.order }}
      </div>
      <div id="plusBtn" @click="addFrame">+</div>
    </div>
    <el-dialog title="[ADD]new frame" :visible.sync="dialogVisible" width="30%">
      <form ref="addForm" id="addForm">
        <div class="form-item">
          <p class="form-item-head">Set Time</p>
          <input type="number" max="60" min="0" name="timeMinute" />
          <span id="timeMiddle">:</span>
          <input type="number" max="60" min="0" name="timeSecond" />
          <span id="timeMiddle">.</span>
          <input type="number" max="1000" min="0" name="timemillisecond" />
        </div>
      </form>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm">confirm</el-button>
        <el-button @click="dialogVisible = false">cancel</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import verify from "@/utils/rules.js";
export default {
  name: "trackBlock",
  data() {
    return {
      timeData: [
        {
          order: 0,
          time: new Date(0)
        }
      ],
      dialogVisible: false,
      maxTime: new Date(2500)
    };
  },
  components: {},
  mounted() {},
  computed: {
    timeLineData: function() {}
  },

  methods: {
    addFrame: function(done) {
      this.dialogVisible = true;
    },
    submitForm: function() {
      this.dialogVisible = false;
      let form = this.$refs["addForm"];
      let min = form.timeMinute.value || 0;
      if(!verify(min, "range", "0,60",this.onVerifyErrot))return;
      let second = form.timeSecond.value || 0;
      if(!verify(second, "range", "0,60",this.onVerifyErrot))return;
      let ms = form.timemillisecond.value || 0;
      if(!verify(ms, "range", "0,1000",this.onVerifyErrot))return;

      let allTime = new Date(min * 60 * 1000 + second * 1000 + ms * 1);
      this.timeData.push({
        order: this.timeData.length,
        time: allTime
      });
      if (this.maxTime <= allTime) {
        this.maxTime = allTime;
      }
    },
    onVerifyErrot:function(message){
        this.$message({message, type: 'warning'});
    },
  }
};
</script>

<style lang="scss" scoped>
#trackBlock {
  margin-top: 1rem;
  #timeLine {
    display: flex;
    p {
      width: 5rem;
      border-right: 1px solid white;
      padding-right: 1rem;
    }
    #timeContainer {
      flex-grow: 1;
      display: flex;
      justify-content: space-between;
      margin: 0 1rem;
    }
  }
  #frameTrack {
    margin-left: 6rem;
    margin-top: 0.5rem;
    display: flex;
    .frame {
      cursor: pointer;
      width: 2rem;
      border-radius: 1rem;
      color: rgb(58, 13, 13);
      background: rgb(250, 249, 249);
      text-align: center;
    }
    #plusBtn {
      cursor: pointer;
      margin-left: 0.5rem;
    }
  }
}
#addForm {
  padding: 0rem;
  .form-item {
    color: white;
    .form-item-head {
      font-size: 1rem;
      margin-bottom: 5px;
    }
    #timeMiddle {
      font-size: 1rem;
      font-weight: 900;
      margin: 0 0.5rem;
    }
    input {
      text-align: center;
      width: 3rem;
      height: 1.5rem;
      color: black;
      border-radius: 10px;
      background: rgb(255, 255, 255);
    }
  }
}
::v-deep .el-dialog {
  background: #34495e;
  .el-dialog__title {
    color: white;
  }
}
</style>