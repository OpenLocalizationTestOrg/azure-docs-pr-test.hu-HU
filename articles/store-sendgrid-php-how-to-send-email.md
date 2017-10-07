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
# <a name="how-toouse-hello-sendgrid-email-service-from-php"></a>Hogyan tooUse hello SendGrid E-mail szolgáltatás php-ből
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok hello SendGrid az e-mailben Azure-szolgáltatás. hello minták PHP nyelven íródtak.
hello tárgyalt forgatókönyvekben szerepel a **hozhat létre, e-mailek**, **e-mailek küldéséhez**, és **mellékletek hozzáadása**. A SendGrid és e-mailek küldéséhez további információkért lásd: hello [lépések](#next-steps) szakasz.

## <a name="what-is-hello-sendgrid-email-service"></a>Mi az az üdvözlő E-mail szolgáltatás SendGrid?
SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt. Gyakori SendGrid használati forgatókönyvek a következők:

* Visszaigazolások toocustomers automatikus küldése
* Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája
* Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése
* Toohelp azonosíthatja a trendeket jelentések létrehozása
* Ügyfél-lekérdezések továbbítása
* E-mail értesítések az alkalmazásról

További információkért lásd: [https://sendgrid.com][https://sendgrid.com].

## <a name="create-a-sendgrid-account"></a>A SendGrid-fiók létrehozása
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>A PHP-alkalmazás a SendGrid használatával
Nincsenek különleges SendGrid használatát egy Azure PHP-alkalmazás szükséges konfiguráció vagy a kódolást. Mivel a SendGrid szolgáltatás, akkor érhetők el pontosan hello ugyanúgy, ahogy felhő alkalmazás is a helyszíni alkalmazásból.

## <a name="how-to-send-an-email"></a>Útmutató: az e-mailek küldése
E-mailek SMTP- és a Web API SendGrid által biztosított hello küldhet.

### <a name="smtp-api"></a>SMTP API
toosend e-mailek hello SendGrid SMTP API, használat *Swift levelezőprogrammal*, egy összetevő-alapú kódtár a PHP-alkalmazások az e-mailek küldésekor. Letöltheti a hello *Swift levelezőprogrammal* kódtárat [http://swiftmailer.org/download] [ http://swiftmailer.org/download] v5.3.0 (használata [szerkesztő] tooinstall SWIFT levelezőprogrammal). Hello könyvtárhoz e-mailek küldéséhez magában foglalja a példányok a <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_levelezőprogrammal</span>, és <span class="auto-style2">Swift\_üzenet </span> osztályok, a megfelelő tulajdonságokat, és hívja a <span class="auto-style2">Swift\_Mailer::send</span> metódust.

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

### <a name="web-api"></a>Webes API
Használja a PHP tartozó [függvény curl] [ curl function] toosend e-mail használatával hello SendGrid Web API.

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

A SendGrid tartozó webes API nagyon hasonló tooa REST API-t, bár nincs valóban RESTful API, mivel a legtöbb hívások, mind az beszerzése és POST műveletek felcserélhetők.

## <a name="how-to-add-an-attachment"></a>Hogyan: hozzá mellékletet
### <a name="smtp-api"></a>SMTP API
Egy további sornyi kód toohello példaként megadott parancsfájlt a Swift levelezőprogrammal e-mail hello SMTP API használatával melléklet küldése magában foglalja.

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

hello további kódsort a következőképpen történik:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

A kód hívások hello üzletági metódus csatolja a a <span class="auto-style2">Swift\_üzenet</span> objektumra, és használja a statikus metódus <span class="auto-style2">fromPath</span> a a <span class="auto-style2">Swift\_melléklet</span>tooget osztályt, majd csatolja a tooa jelenik.

### <a name="web-api"></a>Webes API
Hello Web API használatával mellékletek nagyon hasonló toosending küld egy e-mailek használatával hello Web API. Vegye figyelembe azonban, hogy a következő hello példában hello paraméter tömbjének tartalmaznia kell ezt az elemet:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Példa:

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

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a>Hogyan: szűrési tooEnable láblécben, nyomon követési és elemzés
SendGrid "szűrők" hello használata további e-mail-funkciókat biztosítja. Ezek a beállítások tooan e-mailt ahhoz, hogy bizonyos funkciók, például kattintson nyomon követése, Google analytics, nyomon követése, előfizetés adható hozzá és így tovább.

Szűrők lehet alkalmazott tooa üzenet hello szűrők tulajdonság használatával. Minden egyes szűrő szűrő-specifikus beállításokat tartalmazó kivonat szerint van megadva. A következő példa engedélyezi a hello élőláb szűrőt, és megadja a szöveges üzenetet, amely lesz hozzáfűzve toohello alsó részén hello e-mailt.
Ehhez a példához használjuk [sendgrid-php könyvtár].
Használjon [szerkesztő] tooinstall könyvtár:

    php composer.phar require sendgrid/sendgrid 2.1.1

Példa:    

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

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.

* SendGrid dokumentáció: <https://sendgrid.com/docs>
* SendGrid PHP könyvtár: <https://github.com/sendgrid/sendgrid-php>
* SendGrid különleges ajánlat Azure ügyfeleknek: <https://sendgrid.com/windowsazure.html>

További információkért lásd még: hello [PHP fejlesztői központ](/develop/php/).

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
