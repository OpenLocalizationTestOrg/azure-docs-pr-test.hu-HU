---
title: aaastore-sendgrid-Java-How-to-send-email-example
description: "Hogyan toosend a levelezéshez SendGrid Java az Azure-telepítés"
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: af096a73-6985-4350-92e4-ce1722c8d72f
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com
ms.openlocfilehash: 51fde1fc71467f8252532b30d2f87856ec25067b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Hogyan tooSend e-mailek, használja az Azure-telepítés Java SendGrid
hello alábbi példa bemutatja, hogyan használhatja az Azure-ban üzemeltetett weblapok SendGrid toosend e-maileket. hello eredményül kapott alkalmazás kérni fogja a hello felhasználói e-mailek értékek, ahogy az alábbi képernyőfelvétel a hello.

![E-mailek képernyő][emailform]

e-mailek eredő hello hasonló toohello alábbi képernyőfelvételen fog kinézni.

![E-mailt][emailsent]

Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:

1. Szerezzen be hello javax.mail JAR-fájlok kivételével, például a <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Adja hozzá a hello JAR-fájlok kivételével tooyour Java build elérési útja.
3. Ha az Eclipse toocreate a Java-alkalmazást használ, hello SendGrid szalagtárak is megadhat a az alkalmazás központi telepítési fájl (WAR) Eclipse meg központi telepítési szerelvény funkció használata. Ha az Eclipse toocreate a Java-alkalmazás nem használ, győződjön meg arról, hello szalagtárak megtalálhatók a hello a Java-alkalmazás, és a hozzáadott toohello osztály az alkalmazás elérési útjaként azonos Azure szerepkör.

A saját SendGrid felhasználónév és jelszó, toobe képes toosend hello e-mailt kell rendelkeznie. a SendGrid, használatába tooget lásd: [hogyan toosend a levelezéshez a Java SendGrid](store-sendgrid-java-how-to-send-email.md).

Ezenkívül információkat, hello ismeretét [egy Hello World alkalmazásról az Azure létrehozása az eclipse-ben](http://msdn.microsoft.com/library/windowsazure/hh690944), vagy az egyéb technikák üzemeltetéséhez Java-alkalmazások az Azure-ban, ha nem használja az eclipse-ben, erősen ajánlott.

## <a name="create-a-web-form-for-sending-email"></a>E-mailek küldéséhez a webes űrlap létrehozása
hello a következő kód bemutatja, hogyan toocreate egy webes űrlap tooretrieve felhasználói adatok az e-mailek küldéséhez. A tartalom célját, a hello JSP-fájl neve **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-hello-code-toosend-hello-email"></a>Hello kód toosend hello e-mail létrehozása
hello alábbi kód, amely nevezik emailform.jsp hello űrlap befejezésekor, hello e-mail-üzenetet hoz létre, és elküldi. A tartalom célját, a hello JSP-fájl neve **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%

     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");

     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;

            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {

         // hello SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";

         Properties properties;

         properties = new Properties();

         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");

         // Display hello email fields entered by hello user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");

         // Create hello authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();

         // Create hello mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);

         // Display debug information toostdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);

         // Create hello message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 

         message = new MimeMessage(mailSession);

         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            

         // Specify hello email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);

         // Uncomment hello following if you want tooadd a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");

         // Uncomment hello following if you want tooenable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");

         Transport transport;
         transport = mailSession.getTransport();
         // Connect hello transport object.
         transport.connect();
         // Send hello message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close hello connection.
         transport.close();

        out.println("<p>Email processing completed.</p>");

     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>

    </body>
    </html>

Továbbá toosending hello e-mailek emailform.jsp biztosít egy eredmény hello felhasználó; Példa: a következő képernyőfelvétel hello:

![Mail eredmény küldése][emailresult]

## <a name="next-steps"></a>Következő lépések
Az alkalmazás toohello compute emulator telepítése és egy böngészőből emailform.jsp futtassa, adja meg az értékeket hello képernyőn, kattintson az **az e-mailek küldése**, és olvassa el a sendemail.jsp eredményez.

Ez a kód lett megadva tooshow, hogyan toouse SendGrid Azure Java nyelven. Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében. Példa: 

* Használhatja az Azure storage blobs szolgáltatásban vagy az SQL-adatbázis toostore e-mail-címeket és az e-mailek, egy webes űrlap használata helyett. Az Azure storage-blobot, amely Java használatával kapcsolatos információkért lásd: [hogyan tooUse hello Blob Storage szolgáltatással való Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). SQL-adatbázis a Java használatával kapcsolatos információkért lásd: [SQL-adatbázis használata a Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Használhat `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid felhasználónevet és jelszót a telepítési konfigurációt, helyett hello webes űrlap tooretrieve ezeket az értékeket. Hello információt `RoleEnvironment` osztály című [Using hello Azure szolgáltatás futásidejű kódtár a JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) és hello Azure szolgáltatás futásidejű csomag dokumentációját a <http://dl.windowsazure.com/javadoc>.
* Java nyelven SendGrid használatával kapcsolatos további információkért lásd: [hogyan toosend a levelezéshez a Java SendGrid](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
