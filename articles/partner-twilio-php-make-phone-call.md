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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-php-application-on-azure"></a><span data-ttu-id="b315f-104">Hogyan tooMake a telefonhívás használatával Twilio a PHP-alkalmazások az Azure-on</span><span class="sxs-lookup"><span data-stu-id="b315f-104">How tooMake a Phone Call Using Twilio in a PHP Application on Azure</span></span>
<span data-ttu-id="b315f-105">hello következő példa bemutatja, hogyan használható Twilio toomake weblapról PHP Azure-ban üzemeltetett hívásakor.</span><span class="sxs-lookup"><span data-stu-id="b315f-105">hello following example shows you how you can use Twilio toomake a call from a PHP web page hosted in Azure.</span></span> <span data-ttu-id="b315f-106">hello eredményül kapott alkalmazás fogja kérni a telefonhívás értékeket hello felhasználót, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="b315f-106">hello resulting application will prompt hello user for phone call values, as shown in hello following screen shot.</span></span>

![Az Azure hívás űrlap Twilio és a PHP használatával][twilio_php]

<span data-ttu-id="b315f-108">Szüksége lesz a toodo hello következő toouse hello kód ebben a témakörben:</span><span class="sxs-lookup"><span data-stu-id="b315f-108">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="b315f-109">Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat a a [Twilio-konzol][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="b315f-109">Acquire a Twilio account and authentication token from your [Twilio Console][twilio_console].</span></span> <span data-ttu-id="b315f-110">lépések a Twilio, tooget kiértékelése árképzési [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="b315f-110">tooget started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="b315f-111">Regisztrálhat a próbafiókra: [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="b315f-111">You can sign up for a trial account at [https://www.twilio.com/try-twilio][try_twilio].</span></span>
2. <span data-ttu-id="b315f-112">Szerezze be a hello [Twilio-könyvtár a PHP](https://github.com/twilio/twilio-php) vagy KÖRTEFÁK csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b315f-112">Obtain hello [Twilio library for PHP](https://github.com/twilio/twilio-php) or install it as a PEAR package.</span></span> <span data-ttu-id="b315f-113">További információkért lásd: hello [információs fájl](https://github.com/twilio/twilio-php/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="b315f-113">For more information, see hello [readme file](https://github.com/twilio/twilio-php/blob/master/README.md).</span></span>
3. <span data-ttu-id="b315f-114">Telepítse a PHP hello Azure SDK-t.</span><span class="sxs-lookup"><span data-stu-id="b315f-114">Install hello Azure SDK for PHP.</span></span> <span data-ttu-id="b315f-115">Az hello SDK és telepítéséhez az áttekintést lásd: [PHP hello Azure SDK beállítása](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span><span class="sxs-lookup"><span data-stu-id="b315f-115">For an overview of hello SDK and instructions on installing it, see [Set up hello Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="b315f-116">A következő hívással webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="b315f-116">Create a web form for making a call</span></span>
<span data-ttu-id="b315f-117">a következő HTML hello hogyan code mutat be toobuild weblap (**callform.html**), amely lekérdezi a felhasználó adatai egy hívás:</span><span class="sxs-lookup"><span data-stu-id="b315f-117">hello following HTML code shows how toobuild a web page (**callform.html**) that retrieves user data for making a call:</span></span>

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

## <a name="create-hello-code-toomake-hello-call"></a><span data-ttu-id="b315f-118">Hello kód toomake hello hívás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b315f-118">Create hello code toomake hello call</span></span>
<span data-ttu-id="b315f-119">a következő kód bemutatja hogyan hello toobuild **makecall.php**, amely más néven, amikor hello felhasználó elküldi által megjelenített hello űrlapot **callform.html**.</span><span class="sxs-lookup"><span data-stu-id="b315f-119">hello following code shows how toobuild **makecall.php**, which is called when hello user submits hello form displayed by **callform.html**.</span></span> <span data-ttu-id="b315f-120">alább látható hello kód hívás üdvözlőüzenetére hoz létre, és hello hívás állít elő.</span><span class="sxs-lookup"><span data-stu-id="b315f-120">hello code shown below creates hello call message and generates hello call.</span></span> <span data-ttu-id="b315f-121">Is a Twilio-fiókja és -hitelesítési jogkivonatot a hello meg arról, hogy toouse [Twilio-konzol] [ twilio_console] helyett túl hozzárendelt hello helyőrző értékeket**$sid** és **$token** a hello kódot.</span><span class="sxs-lookup"><span data-stu-id="b315f-121">Also, be sure toouse your Twilio account and authentication token from hello [Twilio Console][twilio_console] instead of hello placeholder values assigned too**$sid** and **$token** in hello code below.</span></span>

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

<span data-ttu-id="b315f-122">Ezenkívül toomaking hello hívás, **makecall.php** megjeleníti az egyes hívás metaadatait, hello az alábbi képen látható módon.</span><span class="sxs-lookup"><span data-stu-id="b315f-122">In addition toomaking hello call, **makecall.php** displays some call metadata, as is shown in hello image below.</span></span> <span data-ttu-id="b315f-123">Hívás metaadatok kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span><span class="sxs-lookup"><span data-stu-id="b315f-123">For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span></span>

![Azure hívási válasz Twilio és a PHP használatával][twilio_php_response]

## <a name="run-hello-application"></a><span data-ttu-id="b315f-125">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="b315f-125">Run hello application</span></span>
<span data-ttu-id="b315f-126">következő lépés hello toodeploy van az alkalmazás tooAzure webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="b315f-126">hello next step is toodeploy your application tooAzure Websites.</span></span> <span data-ttu-id="b315f-127">hello következő cikkeket tartalmaz hello adatokat egy webhely létrehozása és telepítése a Git-, FTP, vagy WebMatrix kódot, (bár nem minden cikkben szereplő információk vonatkozó):</span><span class="sxs-lookup"><span data-stu-id="b315f-127">hello following articles contain hello information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):</span></span>

* [<span data-ttu-id="b315f-128">A PHP-MySQL Azure-webhely létrehozása és telepítése a Git használatával</span><span class="sxs-lookup"><span data-stu-id="b315f-128">Create a PHP-MySQL Azure Web Site and deploy using Git</span></span>](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [<span data-ttu-id="b315f-129">A PHP-MySQL az Azure webhelyén létrehozása és telepítése a FTP használatával</span><span class="sxs-lookup"><span data-stu-id="b315f-129">Create a PHP-MySQL Azure Web Site and Deploy Using FTP</span></span>](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a><span data-ttu-id="b315f-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b315f-130">Next steps</span></span>
<span data-ttu-id="b315f-131">Ez a kód lett megadva tooshow Ön alapvető funkciókat Twilio használatát a PHP az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="b315f-131">This code was provided tooshow you basic functionality using Twilio in PHP on Azure.</span></span> <span data-ttu-id="b315f-132">Mielőtt éles környezetben tooAzure, érdemes lehet tooadd további hibakezelés és egyéb szolgáltatások esetében.</span><span class="sxs-lookup"><span data-stu-id="b315f-132">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="b315f-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="b315f-133">For example:</span></span>

* <span data-ttu-id="b315f-134">Egy webes űrlap használata helyett sikerült Azure storage blobsba vagy SQL adatbázis toostore telefonszámokat, és hívja a szöveg.</span><span class="sxs-lookup"><span data-stu-id="b315f-134">Instead of using a web form, you could use Azure storage blobs or SQL Database toostore phone numbers and call text.</span></span> <span data-ttu-id="b315f-135">További információ az Azure storage blobs használata a PHP: [Azure Storage használata PHP-alkalmazások][howto_blob_storage_php].</span><span class="sxs-lookup"><span data-stu-id="b315f-135">For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php].</span></span> <span data-ttu-id="b315f-136">SQL-adatbázis a PHP használatával kapcsolatos információkért lásd: [SQL-adatbázisok használata PHP-alkalmazások][howto_sql_azure_php].</span><span class="sxs-lookup"><span data-stu-id="b315f-136">For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].</span></span>
* <span data-ttu-id="b315f-137">Hello **makecall.php** használ a Twilio-megadott URL-címet ([http://twimlets.com/message][twimlet_message_url]) tooprovide Twilio Markup Language (TwiML) választ, amely arról értesíti a Twilio hogyan tooproceed hello hívással.</span><span class="sxs-lookup"><span data-stu-id="b315f-137">hello **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) tooprovide a Twilio Markup Language (TwiML) response that informs Twilio how tooproceed with hello call.</span></span> <span data-ttu-id="b315f-138">Hello visszaadott TwiML tartalmazhat például egy `<Say>` művelet, amely éppen szóbeli toohello hívás címzettje szöveg eredményez.</span><span class="sxs-lookup"><span data-stu-id="b315f-138">For example, hello TwiML that is returned can contain a `<Say>` verb that results in text being spoken toohello call recipient.</span></span> <span data-ttu-id="b315f-139">Hello Twilio által megadott URL-cím helyett a saját szolgáltatás toorespond tooTwilio kérelem; létrehozása sikerült További információkért lásd: [hogyan tooUse hang-és a PHP SMS lehetővé tevő szolgáltatásaival Twilio][howto_twilio_voice_sms_php].</span><span class="sxs-lookup"><span data-stu-id="b315f-139">Instead of using hello Twilio-provided URL, you could build your own service toorespond tooTwilio's request; for more information, see [How tooUse Twilio for Voice and SMS Capabilities in PHP][howto_twilio_voice_sms_php].</span></span> <span data-ttu-id="b315f-140">További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml], és további információ a `<Say>` és egyéb Twilio művelet található a [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="b315f-140">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="b315f-141">Olvassa el a hello Twilio biztonsági előírásait a [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="b315f-141">Read hello Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="b315f-142">További információ a Twilio: [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="b315f-142">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="b315f-143">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b315f-143">See Also</span></span>
* [<span data-ttu-id="b315f-144">Hogyan tooUse Twilio hang-és a PHP SMS képességei</span><span class="sxs-lookup"><span data-stu-id="b315f-144">How tooUse Twilio for Voice and SMS Capabilities in PHP</span></span>](partner-twilio-php-how-to-use-voice-sms.md)

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
