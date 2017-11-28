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
# <a name="how-toosend-email-using-sendgrid-from-java"></a><span data-ttu-id="c035e-104">Hogyan tooSend E-mail használatával SendGrid a Javával</span><span class="sxs-lookup"><span data-stu-id="c035e-104">How tooSend Email Using SendGrid from Java</span></span>
<span data-ttu-id="c035e-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok a sendgrid e-mail szolgáltatás az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="c035e-105">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="c035e-106">hello minták Java nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="c035e-106">hello samples are written in Java.</span></span> <span data-ttu-id="c035e-107">hello tárgyalt forgatókönyvekben szerepel a **hozhat létre, e-mailek**, **e-mailek küldéséhez**, **mellékletek hozzáadása**, **szűrőkkel**, és **tulajdonságainak frissítése**.</span><span class="sxs-lookup"><span data-stu-id="c035e-107">hello scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="c035e-108">A SendGrid és e-mailek küldéséhez további információkért lásd: hello [további lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c035e-108">For more information on SendGrid and sending email, see hello [Next steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="c035e-109">Mi az az üdvözlő E-mail szolgáltatás SendGrid?</span><span class="sxs-lookup"><span data-stu-id="c035e-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="c035e-110">SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt.</span><span class="sxs-lookup"><span data-stu-id="c035e-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="c035e-111">Gyakori SendGrid használati forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="c035e-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="c035e-112">Visszaigazolások toocustomers automatikus küldése</span><span class="sxs-lookup"><span data-stu-id="c035e-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="c035e-113">Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája</span><span class="sxs-lookup"><span data-stu-id="c035e-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="c035e-114">Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése</span><span class="sxs-lookup"><span data-stu-id="c035e-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="c035e-115">Toohelp azonosíthatja a trendeket jelentések létrehozása</span><span class="sxs-lookup"><span data-stu-id="c035e-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="c035e-116">Ügyfél-lekérdezések továbbítása</span><span class="sxs-lookup"><span data-stu-id="c035e-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="c035e-117">E-mail értesítések az alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="c035e-117">Email notifications from your application</span></span>

<span data-ttu-id="c035e-118">További információkért lásd: <http://sendgrid.com>.</span><span class="sxs-lookup"><span data-stu-id="c035e-118">For more information, see <http://sendgrid.com>.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="c035e-119">A SendGrid-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c035e-119">Create a SendGrid account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-hello-javaxmail-libraries"></a><span data-ttu-id="c035e-120">Hogyan: hello javax.mail tár</span><span class="sxs-lookup"><span data-stu-id="c035e-120">How to: Use hello javax.mail libraries</span></span>
<span data-ttu-id="c035e-121">Megjelenítheti hello javax.mail szalagtárak, például a <http://www.oracle.com/technetwork/java/javamail> és importálhatja őket a kódot.</span><span class="sxs-lookup"><span data-stu-id="c035e-121">Obtain hello javax.mail libraries, for example from <http://www.oracle.com/technetwork/java/javamail> and import them into your code.</span></span> <span data-ttu-id="c035e-122">Egy magas szintű: hello hello javax.mail könyvtár toosend e-mailek SMTP használatával történik, toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="c035e-122">At a high-level, hello process for using hello javax.mail library toosend email using SMTP is toodo hello following:</span></span>

1. <span data-ttu-id="c035e-123">Adja meg a hello SMTP értékek hello SMTP-kiszolgáló, amely a SendGrid smtp.sendgrid.net is.</span><span class="sxs-lookup"><span data-stu-id="c035e-123">Specify hello SMTP values, including hello SMTP server, which for SendGrid is smtp.sendgrid.net.</span></span>

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

1. <span data-ttu-id="c035e-124">Hello kiterjesztése *javax.mail.Authenticator* osztály, és a megvalósítását az *getPasswordAuthentication* metódus, a SendGrid felhasználónevet és jelszót szabad visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="c035e-124">Extend hello *javax.mail.Authenticator* class, and in your implementation of the *getPasswordAuthentication* method, return your SendGrid user name and password.</span></span>  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. <span data-ttu-id="c035e-125">A hitelesített e-mail-munkamenetet létrehozni egy *javax.mail.Session* objektum.</span><span class="sxs-lookup"><span data-stu-id="c035e-125">Create an authenticated email session through a *javax.mail.Session* object.</span></span>  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. <span data-ttu-id="c035e-126">Hozza létre az üzenetet, és rendelje hozzá **való**, **a**, **tulajdonos** és értékek tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="c035e-126">Create your message and assign **To**, **From**, **Subject** and content values.</span></span> <span data-ttu-id="c035e-127">Ezt mutatják be hello [Útmutató: hozzon létre egy e-mailt](#how-to-create-an-email) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c035e-127">This is shown in hello [How To: Create an Email](#how-to-create-an-email) section.</span></span>
4. <span data-ttu-id="c035e-128">Hello keresztül küldött egy *javax.mail.Transport* objektum.</span><span class="sxs-lookup"><span data-stu-id="c035e-128">Send hello message through a *javax.mail.Transport* object.</span></span> <span data-ttu-id="c035e-129">Ezt mutatják be hello [Útmutató: az e-mailek küldése] [hogyan: egy e-mailek küldése] szakasz.</span><span class="sxs-lookup"><span data-stu-id="c035e-129">This is shown in hello [How To: Send an Email][How to: Send an Email] section.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="c035e-130">Hogyan: hozzon létre egy e-mailt</span><span class="sxs-lookup"><span data-stu-id="c035e-130">How to: Create an email</span></span>
<span data-ttu-id="c035e-131">hello következő bemutatja, hogyan toospecify értékei egy e-mailt.</span><span class="sxs-lookup"><span data-stu-id="c035e-131">hello following shows how toospecify values for an email.</span></span>

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

## <a name="how-to-send-an-email"></a><span data-ttu-id="c035e-132">Útmutató: az e-mailek küldése</span><span class="sxs-lookup"><span data-stu-id="c035e-132">How to: Send an email</span></span>
<span data-ttu-id="c035e-133">a következő bemutatja hogyan hello toosend egy e-mailt.</span><span class="sxs-lookup"><span data-stu-id="c035e-133">hello following shows how toosend an email.</span></span>

    Transport transport = mailSession.getTransport();
    // Connect hello transport object.
    transport.connect();
    // Send hello message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close hello connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="c035e-134">Hogyan: hozzá mellékletet</span><span class="sxs-lookup"><span data-stu-id="c035e-134">How to: Add an attachment</span></span>
<span data-ttu-id="c035e-135">hello következő kód bemutatja, hogyan tooadd mellékletet.</span><span class="sxs-lookup"><span data-stu-id="c035e-135">hello following code shows you how tooadd an attachment.</span></span>

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

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="c035e-136">Hogyan: használata szűrők tooenable láblécben, nyomon követési és elemzés</span><span class="sxs-lookup"><span data-stu-id="c035e-136">How to: Use filters tooenable footers, tracking, and analytics</span></span>
<span data-ttu-id="c035e-137">További e-mail funkcionalitást hello használata biztosítja a SendGrid *szűrők*.</span><span class="sxs-lookup"><span data-stu-id="c035e-137">SendGrid provides additional email functionality through hello use of *filters*.</span></span> <span data-ttu-id="c035e-138">Ezek a beállítások tooan e-mailt ahhoz, hogy bizonyos funkciók, például kattintson nyomon követése, Google analytics, nyomon követése, előfizetés adható hozzá és így tovább.</span><span class="sxs-lookup"><span data-stu-id="c035e-138">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="c035e-139">Szűrők teljes listáját lásd: [szűrőbeállítások][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="c035e-139">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

* <span data-ttu-id="c035e-140">hello következő bemutatja, hogyan tooinsert élőláb szűrheti, hogy eredmények küldi hello e-mail hello alján megjelenő HTML szöveg.</span><span class="sxs-lookup"><span data-stu-id="c035e-140">hello following shows how tooinsert a footer filter that results in HTML text appearing at hello bottom of hello email being sent.</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* <span data-ttu-id="c035e-141">Egy másik példa a szűrő nyomon követési van kattintson.</span><span class="sxs-lookup"><span data-stu-id="c035e-141">Another example of a filter is click tracking.</span></span> <span data-ttu-id="c035e-142">Tegyük fel, hogy az e-mailek szöveg tartalmazza-e a hivatkozás parancsra hello következő, és azt szeretné, mint például tootrack hello aránya:</span><span class="sxs-lookup"><span data-stu-id="c035e-142">Let’s say that your email text contains a hyperlink, such as hello following, and you want tootrack hello click rate:</span></span>

      messagePart.setContent(
          "Hello,
          <p>This is hello body of hello message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* <span data-ttu-id="c035e-143">tooenable hello kattintson nyomon követése, a következő kód használható hello:</span><span class="sxs-lookup"><span data-stu-id="c035e-143">tooenable hello click tracking, use hello following code:</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a><span data-ttu-id="c035e-144">Útmutató: frissítés e-mailes tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c035e-144">How to: Update email properties</span></span>
<span data-ttu-id="c035e-145">Néhány e-mail tulajdonság használatával felülírható  **beállítása*tulajdonság***, illetve használatával  **hozzáadása*tulajdonság***.</span><span class="sxs-lookup"><span data-stu-id="c035e-145">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span>

<span data-ttu-id="c035e-146">Például toospecify **ReplyTo** címek, használja a hello következő:</span><span class="sxs-lookup"><span data-stu-id="c035e-146">For example, toospecify **ReplyTo** addresses, use hello following:</span></span>

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

<span data-ttu-id="c035e-147">tooadd egy **Cc** címzett, használjon hello következő:</span><span class="sxs-lookup"><span data-stu-id="c035e-147">tooadd a **Cc** recipient, use hello following:</span></span>

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="c035e-148">Útmutató: további SendGrid szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="c035e-148">How to: Use additional SendGrid services</span></span>
<span data-ttu-id="c035e-149">SendGrid kínál webes API-k, hogy tooleverage további SendGrid funkciót használhatja az Azure alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="c035e-149">SendGrid offers web-based APIs that you can use tooleverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="c035e-150">Teljes további információkért lásd: hello [SendGrid API-JÁNAK dokumentációja][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="c035e-150">For full details, see hello [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="c035e-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c035e-151">Next steps</span></span>
<span data-ttu-id="c035e-152">Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="c035e-152">Now that you’ve learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="c035e-153">Minta azt mutatja be, az Azure-telepítés SendGrid használatával: [hogyan toosend a levelezéshez SendGrid Java az Azure-telepítés](store-sendgrid-java-how-to-send-email-example.md)</span><span class="sxs-lookup"><span data-stu-id="c035e-153">Sample that demonstrates using SendGrid in an Azure deployment: [How toosend email using SendGrid from Java in an Azure deployment](store-sendgrid-java-how-to-send-email-example.md)</span></span>
* <span data-ttu-id="c035e-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span><span class="sxs-lookup"><span data-stu-id="c035e-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span></span>
* <span data-ttu-id="c035e-155">SendGrid API dokumentációjának: <https://sendgrid.com/docs/API_Reference/index.html></span><span class="sxs-lookup"><span data-stu-id="c035e-155">SendGrid API documentation: <https://sendgrid.com/docs/API_Reference/index.html></span></span>
* <span data-ttu-id="c035e-156">SendGrid különleges ajánlat Azure ügyfeleknek: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="c035e-156">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

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
