---
title: "aaaHow toouse hello SendGrid e-mail szolgáltatás (Java) |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket hello SendGrid e-mail szolgáltatás az Azure-on. A Kódminták Java nyelven."
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: cc75c43e-ede9-492b-98c2-9147fcb92c21
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork
ms.openlocfilehash: 542ce0003e94fc8f5551487d5a3cd6f75d27e8cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-java"></a>Hogyan tooSend E-mail használatával SendGrid a Javával
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok a sendgrid e-mail szolgáltatás az Azure-on. hello minták Java nyelven íródtak. hello tárgyalt forgatókönyvekben szerepel a **hozhat létre, e-mailek**, **e-mailek küldéséhez**, **mellékletek hozzáadása**, **szűrőkkel**, és **tulajdonságainak frissítése**. A SendGrid és e-mailek küldéséhez további információkért lásd: hello [további lépések](#next-steps) szakasz.

## <a name="what-is-hello-sendgrid-email-service"></a>Mi az az üdvözlő E-mail szolgáltatás SendGrid?
SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt. Gyakori SendGrid használati forgatókönyvek a következők:

* Visszaigazolások toocustomers automatikus küldése
* Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája
* Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése
* Toohelp azonosíthatja a trendeket jelentések létrehozása
* Ügyfél-lekérdezések továbbítása
* E-mail értesítések az alkalmazásról

További információkért lásd: <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>A SendGrid-fiók létrehozása
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-hello-javaxmail-libraries"></a>Hogyan: hello javax.mail tár
Megjelenítheti hello javax.mail szalagtárak, például a <http://www.oracle.com/technetwork/java/javamail> és importálhatja őket a kódot. Egy magas szintű: hello hello javax.mail könyvtár toosend e-mailek SMTP használatával történik, toodo hello következő:

1. Adja meg a hello SMTP értékek hello SMTP-kiszolgáló, amely a SendGrid smtp.sendgrid.net is.

```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
               new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
                 properties.put("mail.transport.protocol", "smtp");
                 properties.put("mail.smtp.host", SMTP_HOST_NAME);
                 properties.put("mail.smtp.port", 587);
                 properties.put("mail.smtp.auth", "true");
                 // …
```

1. Hello kiterjesztése *javax.mail.Authenticator* osztály, és a megvalósítását az *getPasswordAuthentication* metódus, a SendGrid felhasználónevet és jelszót szabad visszaadnia.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. A hitelesített e-mail-munkamenetet létrehozni egy *javax.mail.Session* objektum.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. Hozza létre az üzenetet, és rendelje hozzá **való**, **a**, **tulajdonos** és értékek tartalmakat. Ezt mutatják be hello [Útmutató: hozzon létre egy e-mailt](#how-to-create-an-email) szakasz.
4. Hello keresztül küldött egy *javax.mail.Transport* objektum. Ezt mutatják be hello [Útmutató: az e-mailek küldése] [hogyan: egy e-mailek küldése] szakasz.

## <a name="how-to-create-an-email"></a>Hogyan: hozzon létre egy e-mailt
hello következő bemutatja, hogyan toospecify értékei egy e-mailt.

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a>Útmutató: az e-mailek küldése
a következő bemutatja hogyan hello toosend egy e-mailt.

    Transport transport = mailSession.getTransport();
    // Connect hello transport object.
    transport.connect();
    // Send hello message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close hello connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Hogyan: hozzá mellékletet
hello következő kód bemutatja, hogyan tooadd mellékletet.

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify hello local file tooattach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses hello local file name as hello attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a>Hogyan: használata szűrők tooenable láblécben, nyomon követési és elemzés
További e-mail funkcionalitást hello használata biztosítja a SendGrid *szűrők*. Ezek a beállítások tooan e-mailt ahhoz, hogy bizonyos funkciók, például kattintson nyomon követése, Google analytics, nyomon követése, előfizetés adható hozzá és így tovább. Szűrők teljes listáját lásd: [szűrőbeállítások][Filter Settings].

* hello következő bemutatja, hogyan tooinsert élőláb szűrheti, hogy eredmények küldi hello e-mail hello alján megjelenő HTML szöveg.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Egy másik példa a szűrő nyomon követési van kattintson. Tegyük fel, hogy az e-mailek szöveg tartalmazza-e a hivatkozás parancsra hello következő, és azt szeretné, mint például tootrack hello aránya:

      messagePart.setContent(
          "Hello,
          <p>This is hello body of hello message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* tooenable hello kattintson nyomon követése, a következő kód használható hello:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Útmutató: frissítés e-mailes tulajdonságai
Néhány e-mail tulajdonság használatával felülírható  **beállítása*tulajdonság***, illetve használatával  **hozzáadása*tulajdonság***.

Például toospecify **ReplyTo** címek, használja a hello következő:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

tooadd egy **Cc** címzett, használjon hello következő:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Útmutató: további SendGrid szolgáltatás használata
SendGrid kínál webes API-k, hogy tooleverage további SendGrid funkciót használhatja az Azure alkalmazásból. Teljes további információkért lásd: hello [SendGrid API-JÁNAK dokumentációja][SendGrid API documentation].

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.

* Minta azt mutatja be, az Azure-telepítés SendGrid használatával: [hogyan toosend a levelezéshez SendGrid Java az Azure-telepítés](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API dokumentációjának: <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid különleges ajánlat Azure ügyfeleknek: <https://sendgrid.com/windowsazure.html>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[felhőalapú szolgáltatás]: https://sendgrid.com/email-solutions
[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/transactional-email
