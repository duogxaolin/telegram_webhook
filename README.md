# telegram_webhook
**Webhook demo xem tại file json**

1: thêm webhook:
```
https://api.telegram.org/bot{$botToken}/setWebhook?url={$url}
```

2: lấy dữ liệu webhook gửi về
```
<?php
$update = json_decode(file_get_contents("php://input"), true); // Nhận json
$chatId = $update["message"]["chat"]["id"];                   // thông tin người gửi
$message_id = $update["message"]["message_id"];               // id của tin nhắn đó
$rawmessage = $update["message"]["text"];                     // nội dung tin nhắn
$etype = $update["message"]["entities"][0]['type'];           // kiểu nhắn tin nếu đặc biệt : url,phone_number
$cleanedMessage = str_replace(['\\'], '', $rawmessage);       // xoá mã hoá ký tự trong văn bản
$dataToSave = json_encode($update, JSON_PRETTY_PRINT) . "\n\n"; 
file_put_contents("webhook_data.txt", $dataToSave, FILE_APPEND);    //lưu lại webhook để tiện test
?>
```
3: thực hiện

```
<?php
if($etype == 'url'){  // kiểm tra nếu là url

}else if($etype == 'phone_number'){ // kiểm tra văn bản sẽ chỉ có số (sdt hoặc stk)

}else if (strpos($cleanedMessage, "/bot") === 0){  //kiểm tra nếu là văn bản thì trong đó có ký tự gì đặc biệt không

}
?>
```
4: gửi
```
<?php
$sendMessageEndpoint = "https://api.telegram.org/bot{$botToken}/sendMessage";

$sendMessageData = [
    "reply_to_message_id" => $message_id,  //sẽ trả lời tin nhắn trước đó
    "chat_id" => $chatId,                  //id của cuộc hội thoại
    "parse_mode" => "HTML",                // định dạng html
    "text" => $response                    // văn bản gửi đi
];

// Use cURL to send the message
$ch = curl_init($sendMessageEndpoint);
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $sendMessageData);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$result = curl_exec($ch);
curl_close($ch);

echo $result;
?>
```
