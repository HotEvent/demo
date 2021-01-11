<template>
  <div class="chat_box">
    <div class="message_box" id="message_box">
      <ul id="chatContent1" style="display: none">
        <li id="msgtmp1" class="li1">
          <div class="name_box">
            <span ff="name" class="sp1"></span>
            <span ff="time" class="sp2"></span>
          </div>
          <div class="txt" ff="content"></div>
        </li>
      </ul>
      <ul id="chatContent2" style="display: none">
        <li id="msgtmp2" class="li2">
          <div class="name_box" align="right" style="margin-right: -8px">
            <span ff="time" class="sp2"></span>
            <span ff="name" class="sp1"></span>
          </div>
          <div class="txt" ff="content"></div>
        </li>
      </ul>
      <ul id="chatContent3"></ul>
    </div>
    <div class="send_box">
      <textarea id="txt" class="txt_box" name="reworkmes" rows="2"></textarea>
      <div class="send" id="send" v-on:click="handSendClick">发送</div>
    </div>
  </div>
</template>
<script>
/* eslint-disable */

function init(type, userId, toUserId) {
  //   var type = 1; //type 0游客 1客户 2客服 3代练员
  //   var userId = 1000; //当前用户id 没有传0
  //   var toUserId = 1; //对方用户id 没有传0
  $(function () {});
}
export default {
  name: "Chat",
  props: {
    type: {
      type: Number,
      required: true,
    },
    userId: {
      type: Number,
      required: true,
    },
    toUserId: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
    //   type: 1,
    //   userId: 1000,
    //   toUserId: 1,
    };
  },
  methods: {
    addMessage(msg) {
      var data = JSON.parse(msg);
      if (data.type == 1) {
        console.log(data.data);
        var data = data.data;
        var box = $("#msgtmp1").clone(); //复制一份模板，取名为box
        box.find('[ff="name"]').html("代练员"); //在box中设置昵称
        if (this.userId == data.userId) {
          box = $("#msgtmp2").clone(); //复制一份模板，取名为box
          box.find('[ff="name"]').html("顾客"); //在box中设置昵称
        }
        box
          .find('[ff="time"]')
          .html(
            data.createTime.year +
              "." +
              data.createTime.monthValue +
              "." +
              data.createTime.dayOfMonth +
              " " +
              data.createTime.hour +
              ":" +
              data.createTime.minute
          ); //在box中设置昵称
        box.find('[ff="content"]').html(data.text); //在box中设置内容
        box.appendTo("#chatContent3");
        var ele = document.getElementById("message_box");
        ele.scrollTop = ele.scrollHeight + 9999;
      }
      if (data.type == 2) {
        $.each(data.data, function (key, value) {
          console.log(value);
          var data = value;
          var box = $("#msgtmp1").clone(); //复制一份模板，取名为box
          box.find('[ff="name"]').html("顾客"); //在box中设置昵称
          if (this.userId == data.userId) {
            box = $("#msgtmp2").clone(); //复制一份模板，取名为box
            box.find('[ff="name"]').html("代练员"); //在box中设置昵称
          }
          box
            .find('[ff="time"]')
            .html(
              data.createTime.year +
                "." +
                data.createTime.monthValue +
                "." +
                data.createTime.dayOfMonth +
                " " +
                data.createTime.hour +
                ":" +
                data.createTime.minute
            ); //在box中设置昵称
          box.find('[ff="content"]').html(data.text); //在box中设置内容
          box.appendTo("#chatContent3");
        });
        var ele = document.getElementById("message_box");
        ele.scrollTop = ele.scrollHeight + 9999;
      }
    },
    handSendClick() {
      //获取输入框的内容
      var txt = $("#txt").val();
      if (txt.length > 0) {
        //构建一个标准格式的JSON对象
        var obj = JSON.stringify({
          text: txt,
        });
        // 发送消息
        this.send(obj);
        // 清空消息输入框
        $("#txt").val("");
        $("#txt").attr("rows", 1);
      }
    },
    initWebSocket() {
      if (window.WebSocket) {
        let socket = new WebSocket(
          "ws://dailian.demo2.luzhenyu.cn/websocket/" +
            this.type +
            "/" +
            this.toUserId +
            "/" +
            this.userId
        );
        socket.onmessage = (ev) => {
          this.addMessage(ev.data);
        };
        socket.onopen = (e) => {
          console.log(e);
        };
        socket.onclose = (e) => {
          console.log(e);
        };
        socket.onerror = (e) => {
          console.log(e);
        };
        this.socket = socket;
      } else {
      }
    },
    send(obj) {
      var data = () => {
        this.socket.send(obj);
      };
      if (this.socket.readyState !== 1) {
        this.socket.close();
        this.initWebSocket();
        setTimeout(function () {
          data();
        }, 250);
      } else {
        data();
      }
    },
  },
  mounted() {
    let type = this.type;
    let userId = this.userId;
    let toUserId = this.toUserId;
    this.initWebSocket();
    var socket;

    var send = (obj) => {};

    // $("#send").click(function () {
    //   //获取输入框的内容
    //   var txt = $("#txt").val();
    //   if (txt.length > 0) {
    //     //构建一个标准格式的JSON对象
    //     var obj = JSON.stringify({
    //       text: txt,
    //     });
    //     // 发送消息
    //     send(obj);
    //     // 清空消息输入框
    //     $("#txt").val("");
    //     $("#txt").attr("rows", 1);
    //   }
    // });
  },
};
</script>
<style lang="less">
.chat_box {
  width: 350px;
  height: 480px;
  border: 1px solid #f0f8ff;
}
.head_box {
  width: 100%;
  height: 8%;
  background-color: burlywood;
}
.head_box .head_box_1 {
  margin-left: 20%;
}
.message_box {
  width: 100%;
  height: 80%;
  overflow: auto;
}
.message_box::-webkit-scrollbar {
  display: none; /*隐藏滚动条*/
}
ul {
  width: 86%;
  height: auto;
}
.li1 {
  width: 70%;
  height: auto;
  padding: 3px;
  margin-left: -8%;
  list-style-type: none;
  float: left;
}
.li2 {
  width: 70%;
  height: auto;
  padding: 3px;
  float: right;
  list-style-type: none;
  float: right;
}
.name_box {
  padding: 2px;
  margin-left: -5%;
}
.name_box .sp1 {
  font-size: 18px;
}
.name_box .sp2 {
  font-size: 15px;
}
.li1 .txt {
  font-size: 16px;
  background-color: aliceblue;
  padding: 3px;
}
.li2 .txt {
  font-size: 16px;
  background-color: aliceblue;
  padding: 3px;
}
.send_box {
  width: 100%;
  height: 12%;
  background-color: #f0f8ff;
}
.txt_box {
  background-color: transparent;
  width: 76%;
  font-size: 22px;
  margin: 2px 2px;
  border: solid 0px;
  outline: none;
  resize: none;
}
.send {
  float: right;
  font-size: 15px;
  text-align: center;
  width: 50px;
  height: 25px;
  border-radius: 10px;
  margin: 16px 10px;
  overflow: hidden;
  background-color: beige;
}
</style>