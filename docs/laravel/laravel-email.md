## Laravel发送邮件的两种方式

### Mail::raw()

```php

Mail::raw('邮件内容测试', function($message) {
    $message->from('oksvip@sina.com', '慕课网');
    $message->subject('邮件主题 测试');
    $message->to('oksvip@qq.com');
});

```

### Mail::send()

```php

/**
 * template.email模板文件路径为：views/template/email.blade.php
 * 传递subject, receiver两个变量到模板中
 */
Mail::send('template.email', [
    'subject' => '邮件主题 测试',
    'receiver' => 'Tom',
    ], function($message){
    $message->to('oksvip@qq.com');
});

```
