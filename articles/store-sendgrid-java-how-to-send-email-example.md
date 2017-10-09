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
# <a name="how-toosend-email-using-sendgrid-from-java-in-an-azure-deployment"></a><span data-ttu-id="f806e-103">Hogyan tooSend e-mailek, használja az Azure-telepítés Java SendGrid</span><span class="sxs-lookup"><span data-stu-id="f806e-103">How tooSend Email Using SendGrid from Java in an Azure Deployment</span></span>
<span data-ttu-id="f806e-104">hello alábbi példa bemutatja, hogyan használhatja az Azure-ban üzemeltetett weblapok SendGrid toosend e-maileket.</span><span class="sxs-lookup"><span data-stu-id="f806e-104">hello following example shows you how you can use SendGrid toosend emails from a web page hosted in Azure.</span></span> <span data-ttu-id="f806e-105">hello eredményül kapott alkalmazás kérni fogja a hello felhasználói e-mailek értékek, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="f806e-105">hello resulting application will prompt hello user for email values, as shown in hello following screen shot.</span></span>

![E-mailek képernyő][emailform]

<span data-ttu-id="f806e-107">e-mailek eredő hello hasonló toohello alábbi képernyőfelvételen fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="f806e-107">hello resulting email will look similar toohello following screen shot.</span></span>

![E-mailt][emailsent]

<span data-ttu-id="f806e-109">Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:</span><span class="sxs-lookup"><span data-stu-id="f806e-109">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="f806e-110">Szerezzen be hello javax.mail JAR-fájlok kivételével, például a <http://www.oracle.com/technetwork/java/javamail/index.html>.</span><span class="sxs-lookup"><span data-stu-id="f806e-110">Obtain hello javax.mail JARs, for example from <http://www.oracle.com/technetwork/java/javamail/index.html>.</span></span>
2. <span data-ttu-id="f806e-111">Adja hozzá a hello JAR-fájlok kivételével tooyour Java build elérési útja.</span><span class="sxs-lookup"><span data-stu-id="f806e-111">Add hello JARs tooyour Java build path.</span></span>
3. <span data-ttu-id="f806e-112">Ha az Eclipse toocreate a Java-alkalmazást használ, hello SendGrid szalagtárak is megadhat a az alkalmazás központi telepítési fájl (WAR) Eclipse meg központi telepítési szerelvény funkció használata.</span><span class="sxs-lookup"><span data-stu-id="f806e-112">If you are using Eclipse toocreate this Java application, you can include hello SendGrid libraries in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="f806e-113">Ha az Eclipse toocreate a Java-alkalmazás nem használ, győződjön meg arról, hello szalagtárak megtalálhatók a hello a Java-alkalmazás, és a hozzáadott toohello osztály az alkalmazás elérési útjaként azonos Azure szerepkör.</span><span class="sxs-lookup"><span data-stu-id="f806e-113">If you are not using Eclipse toocreate this Java application, ensure hello libraries are included within hello same Azure role as your Java application, and added toohello class path of your application.</span></span>

<span data-ttu-id="f806e-114">A saját SendGrid felhasználónév és jelszó, toobe képes toosend hello e-mailt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f806e-114">You must also have your own SendGrid username and password, toobe able toosend hello email.</span></span> <span data-ttu-id="f806e-115">a SendGrid, használatába tooget lásd: [hogyan toosend a levelezéshez a Java SendGrid](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="f806e-115">tooget started with SendGrid, see [How toosend email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

<span data-ttu-id="f806e-116">Ezenkívül információkat, hello ismeretét [egy Hello World alkalmazásról az Azure létrehozása az eclipse-ben](http://msdn.microsoft.com/library/windowsazure/hh690944), vagy az egyéb technikák üzemeltetéséhez Java-alkalmazások az Azure-ban, ha nem használja az eclipse-ben, erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f806e-116">Additionally, familiarity with hello information at [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-sending-email"></a><span data-ttu-id="f806e-117">E-mailek küldéséhez a webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="f806e-117">Create a web form for sending email</span></span>
<span data-ttu-id="f806e-118">hello a következő kód bemutatja, hogyan toocreate egy webes űrlap tooretrieve felhasználói adatok az e-mailek küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="f806e-118">hello following code shows how toocreate a web form tooretrieve user data for sending email.</span></span> <span data-ttu-id="f806e-119">A tartalom célját, a hello JSP-fájl neve **emailform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="f806e-119">For purposes of this content, hello JSP file is named **emailform.jsp**.</span></span>

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

## <a name="create-hello-code-toosend-hello-email"></a><span data-ttu-id="f806e-120">Hello kód toosend hello e-mail létrehozása</span><span class="sxs-lookup"><span data-stu-id="f806e-120">Create hello code toosend hello email</span></span>
<span data-ttu-id="f806e-121">hello alábbi kód, amely nevezik emailform.jsp hello űrlap befejezésekor, hello e-mail-üzenetet hoz létre, és elküldi.</span><span class="sxs-lookup"><span data-stu-id="f806e-121">hello following code, which is called when you complete hello form in emailform.jsp, creates hello email message and sends it.</span></span> <span data-ttu-id="f806e-122">A tartalom célját, a hello JSP-fájl neve **sendemail.jsp**.</span><span class="sxs-lookup"><span data-stu-id="f806e-122">For purposes of this content, hello JSP file is named **sendemail.jsp**.</span></span>

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

<span data-ttu-id="f806e-123">Továbbá toosending hello e-mailek emailform.jsp biztosít egy eredmény hello felhasználó; Példa: a következő képernyőfelvétel hello:</span><span class="sxs-lookup"><span data-stu-id="f806e-123">In addition toosending hello email, emailform.jsp provides a result for hello user; an example is hello following screen shot:</span></span>

![Mail eredmény küldése][emailresult]

## <a name="next-steps"></a><span data-ttu-id="f806e-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f806e-125">Next steps</span></span>
<span data-ttu-id="f806e-126">Az alkalmazás toohello compute emulator telepítése és egy böngészőből emailform.jsp futtassa, adja meg az értékeket hello képernyőn, kattintson az **az e-mailek küldése**, és olvassa el a sendemail.jsp eredményez.</span><span class="sxs-lookup"><span data-stu-id="f806e-126">Deploy your application toohello compute emulator and within a browser run emailform.jsp, enter values in hello form, click **Send this email**, and then see results in sendemail.jsp.</span></span>

<span data-ttu-id="f806e-127">Ez a kód lett megadva tooshow, hogyan toouse SendGrid Azure Java nyelven.</span><span class="sxs-lookup"><span data-stu-id="f806e-127">This code was provided tooshow you how toouse SendGrid in Java on Azure.</span></span> <span data-ttu-id="f806e-128">Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében.</span><span class="sxs-lookup"><span data-stu-id="f806e-128">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="f806e-129">Példa:</span><span class="sxs-lookup"><span data-stu-id="f806e-129">For example:</span></span> 

* <span data-ttu-id="f806e-130">Használhatja az Azure storage blobs szolgáltatásban vagy az SQL-adatbázis toostore e-mail-címeket és az e-mailek, egy webes űrlap használata helyett.</span><span class="sxs-lookup"><span data-stu-id="f806e-130">You could use Azure storage blobs or SQL Database toostore email addresses and email messages, instead of using a web form.</span></span> <span data-ttu-id="f806e-131">Az Azure storage-blobot, amely Java használatával kapcsolatos információkért lásd: [hogyan tooUse hello Blob Storage szolgáltatással való Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span><span class="sxs-lookup"><span data-stu-id="f806e-131">For information about using Azure storage blobs in Java, see [How tooUse hello Blob Storage Service from Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span></span> <span data-ttu-id="f806e-132">SQL-adatbázis a Java használatával kapcsolatos információkért lásd: [SQL-adatbázis használata a Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span><span class="sxs-lookup"><span data-stu-id="f806e-132">For information about using SQL Database in Java, see [Using SQL Database in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span></span>
* <span data-ttu-id="f806e-133">Használhat `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid felhasználónevet és jelszót a telepítési konfigurációt, helyett hello webes űrlap tooretrieve ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f806e-133">You could use `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid username and password from your deployment's configuration settings, instead of using hello web form tooretrieve those values.</span></span> <span data-ttu-id="f806e-134">Hello információt `RoleEnvironment` osztály című [Using hello Azure szolgáltatás futásidejű kódtár a JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) és hello Azure szolgáltatás futásidejű csomag dokumentációját a <http://dl.windowsazure.com/javadoc>.</span><span class="sxs-lookup"><span data-stu-id="f806e-134">For information about hello `RoleEnvironment` class, see [Using hello Azure Service Runtime Library in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) and hello Azure Service Runtime package documentation at <http://dl.windowsazure.com/javadoc>.</span></span>
* <span data-ttu-id="f806e-135">Java nyelven SendGrid használatával kapcsolatos további információkért lásd: [hogyan toosend a levelezéshez a Java SendGrid](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="f806e-135">For more information about using SendGrid in Java, see [How toosend email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
