---
title: "a Twilio (PHP) telefonhívást aaaHow toomake |} Microsoft Docs"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. A mintákat a PHP-alkalmazás."
documentationcenter: php
services: 
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 44e35adc-be06-4700-beee-8c9e2c20c540
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: e6fecc345bf9ae787d14d533bd8d96b175c2453b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Hogyan tooMake a telefonhívás használatával Twilio a PHP-alkalmazások az Azure-on
hello következő példa bemutatja, hogyan használható Twilio toomake weblapról PHP Azure-ban üzemeltetett hívásakor. hello eredményül kapott alkalmazás fogja kérni a telefonhívás értékeket hello felhasználót, ahogy az alábbi képernyőfelvétel a hello.

![Az Azure hívás űrlap Twilio és a PHP használatával][twilio_php]

Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:

1. Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat a a [Twilio-konzol][twilio_console]. lépések a Twilio, tooget kiértékelése árképzési [http://www.twilio.com/pricing][twilio_pricing]. Regisztrálhat a próbafiókra: [https://www.twilio.com/try-twilio][try_twilio].
2. Szerezze be a hello [Twilio-könyvtár a PHP](https://github.com/twilio/twilio-php) vagy KÖRTEFÁK csomag telepítéséhez. További információkért lásd: hello [információs fájl](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Telepítse a PHP hello Azure SDK-t. Az hello SDK és telepítéséhez az áttekintést lásd: [PHP hello Azure SDK beállítása](app-service-web/web-sites-php-mysql-deploy-use-git.md)

## <a name="create-a-web-form-for-making-a-call"></a>A következő hívással webes űrlap létrehozása
a következő HTML hello hogyan code mutat be toobuild weblap (**callform.html**), amely lekérdezi a felhasználó adatai egy hívás:

```html
<!DOCTYPE html>
<html>
<head>
  <title>Automated call form</title>
</head>
<body>
  <h1>Automated Call Form</h1>
  <p>Fill in all fields and click <b>Make this call</b>.</p>
  <form action="makecall.php" method="post">
    <table>
      <tr>
        <td>To:</td>
        <td><input name="callTo" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>From:</td>
        <td><input name="callFrom" size="50" type="text" value=""></td>
      </tr>
      <tr>
        <td>Call message:</td>
        <td><input name="callText" size="100" type="text" value="Hello. This is hello call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-hello-code-toomake-hello-call"></a>Hello kód toomake hello hívás létrehozása
a következő kód bemutatja hogyan hello toobuild **makecall.php**, amely más néven, amikor hello felhasználó elküldi által megjelenített hello űrlapot **callform.html**. alább látható hello kód hívás üdvözlőüzenetére hoz létre, és hello hívás állít elő. Is a Twilio-fiókja és -hitelesítési jogkivonatot a hello meg arról, hogy toouse [Twilio-konzol] [ twilio_console] helyett túl hozzárendelt hello helyőrző értékeket**$sid** és **$token** a hello kódot.

```html
<html>
<head><title>Making call...</title></head>
<body>
<p>Your call is being made.</p>

<?php
require_once 'path/to/vendor/autoload.php';

$sid   = "your_account_sid";
$token = "your_authentication_token";

$from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
$to_number   = $_POST['callTo'];
$message     = $_POST['callText'];

$client = new Twilio\Rest\Client($sid, $token);

$call = $client->calls->create(
            $to_number,
            $from_number,
            array('url' => http://twimlets.com/message?Message=' . urlencode($message))
        );

echo "Call status: " . $call->status . "<br />";
echo "URI resource: " . $call->uri . "<br />";
?>
</body>
</html>
```

Ezenkívül toomaking hello hívás, **makecall.php** megjeleníti az egyes hívás metaadatait, hello az alábbi képen látható módon. Hívás metaadatok kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Azure hívási válasz Twilio és a PHP használatával][twilio_php_response]

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
következő lépés hello toodeploy van az alkalmazás tooAzure webhelyeket. hello következő cikkeket tartalmaz hello adatokat egy webhely létrehozása és telepítése a Git-, FTP, vagy WebMatrix kódot, (bár nem minden cikkben szereplő információk vonatkozó):

* [A PHP-MySQL Azure-webhely létrehozása és telepítése a Git használatával](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [A PHP-MySQL az Azure webhelyén létrehozása és telepítése a FTP használatával](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a>Következő lépések
Ez a kód lett megadva tooshow Ön alapvető funkciókat Twilio használatát a PHP az Azure-on. Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében. Példa:

* Egy webes űrlap használata helyett sikerült Azure storage blobsba vagy SQL adatbázis toostore telefonszámokat, és hívja a szöveg. További információ az Azure storage blobs használata a PHP: [Azure Storage használata PHP-alkalmazások][howto_blob_storage_php]. SQL-adatbázis a PHP használatával kapcsolatos információkért lásd: [SQL-adatbázisok használata PHP-alkalmazások][howto_sql_azure_php].
* Hello **makecall.php** használ a Twilio-megadott URL-címet ([http://twimlets.com/message][twimlet_message_url]) tooprovide Twilio Markup Language (TwiML) választ, amely arról értesíti a Twilio hogyan tooproceed hello hívással. Hello visszaadott TwiML tartalmazhat például egy `<Say>` művelet, amely éppen szóbeli toohello hívás címzettje szöveg eredményez. Hello Twilio által megadott URL-cím helyett a saját szolgáltatás toorespond tooTwilio kérelem; létrehozása sikerült További információkért lásd: [hogyan tooUse hang-és a PHP SMS lehetővé tevő szolgáltatásaival Twilio][howto_twilio_voice_sms_php]. További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml], és további információ a `<Say>` és egyéb Twilio művelet található a [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].
* Olvassa el a hello Twilio biztonsági előírásait a [https://www.twilio.com/docs/security][twilio_docs_security].

További információ a Twilio: [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Lásd még:
* [Hogyan tooUse Twilio hang-és a PHP SMS képességei](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_console]: https://www.twilio.com/console
[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/docs/api
[verify_phone]: https://www.twilio.com/console/phone-numbers/verified
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
