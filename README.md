## Google Voice 保号/自动发送及回复信息

### 一、自动发送信息

1、注册登录 [IFTTT](https://ifttt.com)       然后使用[保号程序](https://ifttt.com/applets/SMGSYPzw-google-voice)   使用此保号程序可以与第二步兼容，也可以忽略第二步

2、配置 [Keep Google Voice Active (Send Messege)](https://ifttt.com/applets/SsnxTYZJ-keep-google-voice-active-send-messege) (时区注意选择BeiJing。可以自己定义发送的时间及发送信息的内容。)

3、设置好后即可自动给你的 GV 码发送信息了。（你可以设置一下离你现在时间最近的时间测试。功能是已经测试过的没有问题的。）


### 二、自动回复信息

1、登入 GV，先在 GV 里面设置里面把“将消息转发到电子邮件”打开。

2、登入 Gmail，在设置里选择“过滤器和屏蔽的地址” --> “创建新的过滤器” --> 在发件人处填写 “@txt.voice.google.com”。如下图所示：

![1](https://raw.githubusercontent.com/veip007/GV_GoogleVoice/master/1.png)

3、点击“创建过滤器”，在弹出的对话框点击“选择标签” --> “新建标签”，输入标签名为“autoreply”，点击创建即可。

![2](https://raw.githubusercontent.com/veip007/GV_GoogleVoice/master/2.png)

4、选择如下图所示后点击“创建过滤器”即可。

![3](https://raw.githubusercontent.com/veip007/GV_GoogleVoice/master/3.png)


5、登录 Google Drive，单击左上角的“新建”。按下图新建一个 Google App Script。(如未找到可以在“关联更多应用”里面查找“Google Apps Script”关联一下就有了。)

![4](https://raw.githubusercontent.com/veip007/GV_GoogleVoice/master/4.png)

6、复制下面的代码替换现有的代码。
```bash
function autoReplier() {
  var labelObj = GmailApp.getUserLabelByName('autoreply');
  var gmailThreads;
  var messages;
  var messagecount;
  var sender;
  var num = 9;  //设置连续自动回复邮件的次数（为防止两人都是自动回复，当发送次数达到 9 时将不自动回复）。
  var hours = 12;  //如果自动回复次数超过了上面设置的值，过了多少小时后又可以自动回复。
    
  for (var gg = 0; gg < labelObj.getUnreadCount(); gg++) {
    gmailThreads = labelObj.getThreads()[gg];
    messages = gmailThreads.getMessages();
    messagecount = gmailThreads.getMessageCount();
    for (var ii = 0; ii < messages.length; ii++) {
      
      if (messages[ii].isUnread()) {
        
        msg = messages[ii].getPlainBody();
        sender = messages[ii].getFrom(); 
 
        if (messagecount < num){
          MailApp.sendEmail(sender, "Auto Reply", "Hi, 您好！这是一条自动回复短信！本短信由 Google Apps Script 自动发出。");
        }else if( (messages[messagecount - 1].getDate().getTime() - messages[messagecount - num].getDate().getTime()) > hours * 60 * 60 * 1000 ){
          MailApp.sendEmail(sender, "Auto Reply", "Hi, 您好！这是一条自动回复短信！本短信由 Google Apps Script 自动发出。");
        }
        messages[ii].markRead();
        messages[ii].moveToTrash();
 
      }
    }
  }
}
```

7、点击保存，在弹出的对话框中输出你要显示的名称，例如：autoReplier。再单击“调试”会提示你授权，你按提示授权即可。授权完后会提示没有找到文件之类的，不用管。

8、再次点击“调试”，如果没有任何提示说明脚本没有错误。你也可以在“查看” --> “日志” --> “Apps 脚本信息中心”中查看脚本运行状态。如果显示状态为已完成则表示脚本没有错误。

![5](https://raw.githubusercontent.com/veip007/GV_GoogleVoice/master/5.png)

9、单击“修改” --> “当前项目的触发器” --> 右下角的“添加触发器”，按下图设置好保存即可。

![6](https://raw.githubusercontent.com/veip007/GV_GoogleVoice/master/6.png)

10、好了，现在你可以给自己发一条短信试试了。
