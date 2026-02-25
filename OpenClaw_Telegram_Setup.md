## Telegram doesn't response, even openclaw has been reset

The telegram bot may use the previous web hook, now the gateway port has been changed, but telegram bot 
still send the message to the old adress, you need clean the webhook from telegram bot api site, use 
following command:


```
https://api.telegram.org/bot<你的token>/deleteWebhook
```


