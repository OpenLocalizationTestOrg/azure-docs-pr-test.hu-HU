---
title: "aaaHow toouse hello SendGrid e-mail szolgáltatás (PHP) |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket hello SendGrid e-mail szolgáltatás az Azure-on. Kódminták PHP."
documentationcenter: php
services: 
manager: sendgrid
editor: mollybos
author: thinkingserious
ms.assetid: 51a9928a-4c9e-4b0a-aca8-9a408aeb6f47
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com
ms.openlocfilehash: 0076e56dc185cb8f52e629395e7d2c143cb5cfa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-sendgrid-email-service-from-php"></a><span data-ttu-id="cbd23-104">Hogyan tooUse hello SendGrid E-mail szolgáltatás php-ből</span><span class="sxs-lookup"><span data-stu-id="cbd23-104">How tooUse hello SendGrid Email Service from PHP</span></span>
<span data-ttu-id="cbd23-105">Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello SendGrid az e-mailben Azure-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cbd23-105">This guide demonstrates how tooperform common programming tasks with hello SendGrid email service on Azure.</span></span> <span data-ttu-id="cbd23-106">hello minták PHP nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="cbd23-106">hello samples are written in PHP.</span></span>
<span data-ttu-id="cbd23-107">hello tárgyalt forgatókönyvekben szerepel a **hozhat létre, e-mailek**, **e-mailek küldéséhez**, és **mellékletek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="cbd23-107">hello scenarios covered include **constructing email**, **sending email**, and **adding attachments**.</span></span> <span data-ttu-id="cbd23-108">A SendGrid és e-mailek küldéséhez további információkért lásd: hello [lépések](#next-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="cbd23-108">For more information on SendGrid and sending email, see hello [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="cbd23-109">Mi az az üdvözlő E-mail szolgáltatás SendGrid?</span><span class="sxs-lookup"><span data-stu-id="cbd23-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="cbd23-110">SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt.</span><span class="sxs-lookup"><span data-stu-id="cbd23-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="cbd23-111">Gyakori SendGrid használati forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="cbd23-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="cbd23-112">Visszaigazolások toocustomers automatikus küldése</span><span class="sxs-lookup"><span data-stu-id="cbd23-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="cbd23-113">Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája</span><span class="sxs-lookup"><span data-stu-id="cbd23-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="cbd23-114">Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése</span><span class="sxs-lookup"><span data-stu-id="cbd23-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="cbd23-115">Toohelp azonosíthatja a trendeket jelentések létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbd23-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="cbd23-116">Ügyfél-lekérdezések továbbítása</span><span class="sxs-lookup"><span data-stu-id="cbd23-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="cbd23-117">E-mail értesítések az alkalmazásról</span><span class="sxs-lookup"><span data-stu-id="cbd23-117">Email notifications from your application</span></span>

<span data-ttu-id="cbd23-118">További információkért lásd: [https://sendgrid.com][https://sendgrid.com].</span><span class="sxs-lookup"><span data-stu-id="cbd23-118">For more information, see [https://sendgrid.com][https://sendgrid.com].</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="cbd23-119">A SendGrid-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbd23-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a><span data-ttu-id="cbd23-120">A PHP-alkalmazás a SendGrid használatával</span><span class="sxs-lookup"><span data-stu-id="cbd23-120">Using SendGrid from your PHP Application</span></span>
<span data-ttu-id="cbd23-121">Nincsenek különleges SendGrid használatát egy Azure PHP-alkalmazás szükséges konfiguráció vagy a kódolást.</span><span class="sxs-lookup"><span data-stu-id="cbd23-121">Using SendGrid in an Azure PHP application requires no special configuration or coding.</span></span> <span data-ttu-id="cbd23-122">Mivel a SendGrid szolgáltatás, akkor érhetők el pontosan hello ugyanúgy, ahogy felhő alkalmazás is a helyszíni alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="cbd23-122">Because SendGrid is a service, it can be accessed in exactly hello same way from a cloud application as it can from an on-premises application.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="cbd23-123">Útmutató: az e-mailek küldése</span><span class="sxs-lookup"><span data-stu-id="cbd23-123">How to: Send an Email</span></span>
<span data-ttu-id="cbd23-124">E-mailek SMTP- és a Web API SendGrid által biztosított hello küldhet.</span><span class="sxs-lookup"><span data-stu-id="cbd23-124">You can send email using either SMTP or hello Web API provided by SendGrid.</span></span>

### <a name="smtp-api"></a><span data-ttu-id="cbd23-125">SMTP API</span><span class="sxs-lookup"><span data-stu-id="cbd23-125">SMTP API</span></span>
<span data-ttu-id="cbd23-126">toosend e-mailek hello SendGrid SMTP API, használat *Swift levelezőprogrammal*, egy összetevő-alapú kódtár a PHP-alkalmazások az e-mailek küldésekor.</span><span class="sxs-lookup"><span data-stu-id="cbd23-126">toosend email using hello SendGrid SMTP API, use *Swift Mailer*, a component-based library for sending emails from PHP applications.</span></span> <span data-ttu-id="cbd23-127">Letöltheti a hello *Swift levelezőprogrammal* kódtárat [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (használata [szerkesztő] tooinstall SWIFT levelezőprogrammal).</span><span class="sxs-lookup"><span data-stu-id="cbd23-127">You can download hello *Swift Mailer* library from [http://swiftmailer.org/download][http://swiftmailer.org/download] v5.3.0 (use [Composer] tooinstall Swift Mailer).</span></span> <span data-ttu-id="cbd23-128">Hello könyvtárhoz e-mailek küldéséhez magában foglalja a példányok a <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_levelezőprogrammal</span>, és <span class="auto-style2">Swift\_üzenet </span> osztályok, a megfelelő tulajdonságokat, és hívja a <span class="auto-style2">Swift\_Mailer::send</span> metódust.</span><span class="sxs-lookup"><span data-stu-id="cbd23-128">Sending email with hello library involves creating instances of the <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Mailer</span>, and <span class="auto-style2">Swift\_Message</span> classes, setting the appropriate properties, and calling the <span class="auto-style2">Swift\_Mailer::send</span> method.</span></span>

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create hello body of hello message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of hello email
      * If hello receiver is able tooview html emails then only hello html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name tooAppear');
     // Email recipients
     $too= array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach hello body of hello email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out too'.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a><span data-ttu-id="cbd23-129">Webes API</span><span class="sxs-lookup"><span data-stu-id="cbd23-129">Web API</span></span>
<span data-ttu-id="cbd23-130">Használja a PHP tartozó [függvény curl] [ curl function] toosend e-mail használatával hello SendGrid Web API.</span><span class="sxs-lookup"><span data-stu-id="cbd23-130">Use PHP's [curl function][curl function] toosend email using hello SendGrid Web API.</span></span>

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl toouse HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is hello body of hello POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not tooreturn headers, but do return hello response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

<span data-ttu-id="cbd23-131">A SendGrid tartozó webes API nagyon hasonló tooa REST API-t, bár nincs valóban RESTful API, mivel a legtöbb hívások, mind az beszerzése és POST műveletek felcserélhetők.</span><span class="sxs-lookup"><span data-stu-id="cbd23-131">SendGrid's Web API is very similar tooa REST API, though it is not truly a RESTful API since, in most calls, both GET and POST verbs can be used interchangeably.</span></span>

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="cbd23-132">Hogyan: hozzá mellékletet</span><span class="sxs-lookup"><span data-stu-id="cbd23-132">How to: Add an Attachment</span></span>
### <a name="smtp-api"></a><span data-ttu-id="cbd23-133">SMTP API</span><span class="sxs-lookup"><span data-stu-id="cbd23-133">SMTP API</span></span>
<span data-ttu-id="cbd23-134">Egy további sornyi kód toohello példaként megadott parancsfájlt a Swift levelezőprogrammal e-mail hello SMTP API használatával melléklet küldése magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="cbd23-134">Sending an attachment using hello SMTP API involves one additional line of code toohello example script for sending an email with Swift Mailer.</span></span>

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create hello body of hello message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of hello email
      * If hello reciever is able tooview html emails then only hello html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name tooAppear');

     // Email recipients
     $too= array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';

     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach hello body of hello email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));

     // send message
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out too'.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

<span data-ttu-id="cbd23-135">hello további kódsort a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="cbd23-135">hello additional line of code is as follows:</span></span>

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

<span data-ttu-id="cbd23-136">A kód hívások hello üzletági metódus csatolja a a <span class="auto-style2">Swift\_üzenet</span> objektumra, és használja a statikus metódus <span class="auto-style2">fromPath</span> a a <span class="auto-style2">Swift\_melléklet</span>tooget osztályt, majd csatolja a tooa jelenik.</span><span class="sxs-lookup"><span data-stu-id="cbd23-136">This line of code calls hello attach method on the <span class="auto-style2">Swift\_Message</span> object and uses static method <span class="auto-style2">fromPath</span> on the <span class="auto-style2">Swift\_Attachment</span> class tooget and attach a file tooa message.</span></span>

### <a name="web-api"></a><span data-ttu-id="cbd23-137">Webes API</span><span class="sxs-lookup"><span data-stu-id="cbd23-137">Web API</span></span>
<span data-ttu-id="cbd23-138">Hello Web API használatával mellékletek nagyon hasonló toosending küld egy e-mailek használatával hello Web API.</span><span class="sxs-lookup"><span data-stu-id="cbd23-138">Sending an attachment using hello Web API is very similar toosending an email using hello Web API.</span></span> <span data-ttu-id="cbd23-139">Vegye figyelembe azonban, hogy a következő hello példában hello paraméter tömbjének tartalmaznia kell ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="cbd23-139">However, note that in hello example that follows, hello parameter array must contain this element:</span></span>

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

<span data-ttu-id="cbd23-140">Példa:</span><span class="sxs-lookup"><span data-stu-id="cbd23-140">Example:</span></span>

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';

     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> hello HTML </p>',
         'text' => 'hello plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );

     print_r($params);

     $request = $url.'api/mail.send.json';

     // Generate curl request
     $session = curl_init($request);

     // Tell curl toouse HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);

     // Tell curl that this is hello body of hello POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);

     // Tell curl not tooreturn headers, but do return hello response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);

     // obtain response
     $response = curl_exec($session);
     curl_close($session);

     // print everything out
     print_r($response);

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="cbd23-141">Hogyan: szűrési tooEnable láblécben, nyomon követési és elemzés</span><span class="sxs-lookup"><span data-stu-id="cbd23-141">How to: Use Filters tooEnable Footers, Tracking, and Analytics</span></span>
<span data-ttu-id="cbd23-142">SendGrid "szűrők" hello használata további e-mail-funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="cbd23-142">SendGrid provides additional email functionality through hello use of 'filters'.</span></span> <span data-ttu-id="cbd23-143">Ezek a beállítások tooan e-mailt ahhoz, hogy bizonyos funkciók, például kattintson nyomon követése, Google analytics, nyomon követése, előfizetés adható hozzá és így tovább.</span><span class="sxs-lookup"><span data-stu-id="cbd23-143">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span>

<span data-ttu-id="cbd23-144">Szűrők lehet alkalmazott tooa üzenet hello szűrők tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="cbd23-144">Filters can be applied tooa message by using hello filters property.</span></span> <span data-ttu-id="cbd23-145">Minden egyes szűrő szűrő-specifikus beállításokat tartalmazó kivonat szerint van megadva.</span><span class="sxs-lookup"><span data-stu-id="cbd23-145">Each filter is specified by a hash containing filter-specific settings.</span></span> <span data-ttu-id="cbd23-146">A következő példa engedélyezi a hello élőláb szűrőt, és megadja a szöveges üzenetet, amely lesz hozzáfűzve toohello alsó részén hello e-mailt.</span><span class="sxs-lookup"><span data-stu-id="cbd23-146">The following example enables hello footer filter and specifies a text message that will be appended toohello bottom of hello email message.</span></span>
<span data-ttu-id="cbd23-147">Ehhez a példához használjuk [sendgrid-php könyvtár].</span><span class="sxs-lookup"><span data-stu-id="cbd23-147">For this example we will use [sendgrid-php library].</span></span>
<span data-ttu-id="cbd23-148">Használjon [szerkesztő] tooinstall könyvtár:</span><span class="sxs-lookup"><span data-stu-id="cbd23-148">Use [Composer] tooinstall library:</span></span>

    php composer.phar require sendgrid/sendgrid 2.1.1

<span data-ttu-id="cbd23-149">Példa:</span><span class="sxs-lookup"><span data-stu-id="cbd23-149">Example:</span></span>    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // hello list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request tooSendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify hello names of hello recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of hello above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have
     // enabled hello footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // hello subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy hello user here. If
     // a sender list IS specified above, this email address becomes irrelevant.
     $too= 'john@contoso.com';

     # Create hello body of hello message (a plain-text and an HTML version).
     # text is your plain-text email
     # html is your html version of hello email
     # if hello receiver is able tooview html emails then only hello html
     # email will be displayed

     /*
      * Note hello variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment toocall you at -time- EST toodiscuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html>
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     toocall you at -time- EST toodiscuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach hello body of hello email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a><span data-ttu-id="cbd23-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbd23-150">Next Steps</span></span>
<span data-ttu-id="cbd23-151">Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.</span><span class="sxs-lookup"><span data-stu-id="cbd23-151">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="cbd23-152">SendGrid dokumentáció: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="cbd23-152">SendGrid documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="cbd23-153">SendGrid PHP könyvtár: <https://github.com/sendgrid/sendgrid-php></span><span class="sxs-lookup"><span data-stu-id="cbd23-153">SendGrid PHP library: <https://github.com/sendgrid/sendgrid-php></span></span>
* <span data-ttu-id="cbd23-154">SendGrid különleges ajánlat Azure ügyfeleknek: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="cbd23-154">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

<span data-ttu-id="cbd23-155">További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="cbd23-155">For more information, see also hello [PHP Developer Center](/develop/php/).</span></span>

[https://sendgrid.com]: https://sendgrid.com
[https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
[special offer]: https://www.sendgrid.com/windowsazure.html
[Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
[http://swiftmailer.org/download]: http://swiftmailer.org/download
[curl function]: http://php.net/curl
[felhőalapú szolgáltatás]: https://sendgrid.com/email-solutions
[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/transactional-email
[sendgrid-php könyvtár]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
[szerkesztő]: https://getcomposer.org/download/
