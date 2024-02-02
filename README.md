# telegram_webhook
**Webhook demo xem tại file json**
1: thêm webhook
https://api.telegram.org/bot[token bên trên]/setWebhook?url=[url]

2: lấy dữ liệu webhook gửi về
$update = json_decode(file_get_contents("php://input"), true);
$chatId = $update["message"]["chat"]["id"];
$message_id = $update["message"]["message_id"];
$rawmessage = $update["message"]["text"];
$etype = $update["message"]["entities"][0]['type'];
$cleanedMessage = str_replace(['\\'], '', $rawmessage);
$dataToSave = json_encode($update, JSON_PRETTY_PRINT) . "\n\n";
file_put_contents("webhook_data.txt", $dataToSave, FILE_APPEND);
