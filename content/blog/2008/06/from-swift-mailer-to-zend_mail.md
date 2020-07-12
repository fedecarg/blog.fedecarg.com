---
title: "From Swift Mailer to Zend_Mail"
date: "2008-06-27"
tags: [programming,webdev]
---

I've recently switched from Swift Mailer to Zend\_Mail and to be honest, I'm loving it. Finally someone developed a lightweight, powerful and easy to use email component! Zend\_Mail creates and sends e-mail messages with the format text or HTML and it allows e-mail to be sent in an easy form while preventing mail injection. Zend\_Mime is a support set for handling multi-part MIME messages.

### Simple email with Zend\_Mail

```
$mail = new Zend_Mail();
$mail->setBodyText("This is the text of the mail");
$mail->setFrom("sender@example.com", "Some Sender");
$mail->addTo("recipient@example.com", "Some Recipient");
$mail->setSubject("Test Subject");
$mail->send();
```

For security reasons, Zend\_Mail filters all header fields to prevent header injection with newline (\\n) characters. You also can use most methods of the Zend\_Mail object with a convenient fluent interface. A fluent interface means that each method returns a reference to the object on which it was called, so you can immediately call another method.

### Zend\_Mail fluent interface

```
$mail = new Zend_Mail();
$mail->setBodyText("This is the text of the mail.")
    ->setFrom("sender@example.com", "Some Sender")
    ->addTo("recipient@example.com", "Some Recipient")
    ->setSubject("TestSubject")
    ->send();
```

Zend\_Mail messages can be sent via SMTP, so Zend\_Mail\_Transport\_SMTP needs to be created and registered with Zend\_Mail before the send() method is called.

### Sending email via SMTP

```
$tr = new Zend_Mail_Transport_Smtp("mail.example.com")
Zend_Mail::setDefaultTransport($tr);

$mail = new Zend_Mail();
$mail->setBodyText("This is the text of the mail.");
$mail->setFrom("sender@example.com", "Some Sender")
$mail->addTo("recipient@example.com", "Some Recipient");
$mail->setSubject("Test Subject");
$mail->send();
```

### Sending multiple emails per SMTP connection

```
$mail = new Zend_Mail();
$tr = new Zend_Mail_Transport_Smtp("mail.example.com");
Zend_Mail::setDefaultTransport($tr);

$tr->connect();
for ($i = 0; $i < 5; $i++) {
        $mail->setBodyText("This is the text of the mail.");
        $mail->setFrom("sender@example.com", "Some Sender");
        $mail->addTo("recipient@example.com", "Some Recipient");
        $mail->setSubject("Test Subject");
        $mail->send();
}
$tr->disconnect();
```

To send an e-mail in HTML format, set the body using the method setBodyHTML() instead of setBodyText(). The MIME content type will automatically be set to text/html then. If you use both HTML and Text bodies, a multipart/alternative MIME message will automatically be generated:

### Sending an HTML email

```
$mail = new Zend_Mail();
$mail->setBodyText("My Nice Test Text");
$mail->setBodyHtml("My Nice Test Text");
$mail->setFrom("sender@example.com", "Some Sender");
$mail->addTo("recipient@example.com", "Some Recipient");
$mail->setSubject("TestSubject");
$mail->send();
```

How easy was that!? If you haven't used any of the Zend Framework components yet, then this is a great opportunity to do so.

### Articles

- [Sending Emails with the Zend Framework](http://www.talkphp.com/vbarticles.php?do=article&articleid=51&title=sending-emails-with-the-zend-framework)
- [More information about Zend\_Mail](http://framework.zend.com/manual/en/zend.mail.html)
