---
title: "Hogyan telefonhívás a Twilio (PHP) |} Microsoft Docs"
description: "Útmutató a telefonhívás, és a Twilio API szolgáltatás SMS üzenet küldése az Azure-on. A mintákat a PHP-alkalmazás."
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
ms.openlocfilehash: f35450ace02727ddf392dbbe857b934a45ee022a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a><span data-ttu-id="4083d-104">Hogyan telefonhívás Twilio használatát a PHP-alkalmazások az Azure-on</span><span class="sxs-lookup"><span data-stu-id="4083d-104">How to Make a Phone Call Using Twilio in a PHP Application on Azure</span></span>
<span data-ttu-id="4083d-105">A következő példa bemutatja, hogyan használható fel a Twilio weblapról PHP Azure-ban üzemeltetett hívást.</span><span class="sxs-lookup"><span data-stu-id="4083d-105">The following example shows you how you can use Twilio to make a call from a PHP web page hosted in Azure.</span></span> <span data-ttu-id="4083d-106">Az eredményül kapott alkalmazás fogja kérni a felhasználót a telefonhívás-értékeket, az az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="4083d-106">The resulting application will prompt the user for phone call values, as shown in the following screen shot.</span></span>

![Az Azure hívás űrlap Twilio és a PHP használatával][twilio_php]

<span data-ttu-id="4083d-108">A következő használatára az ebben a témakörben lévő lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="4083d-108">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="4083d-109">Szerezzen be egy Twilio-fiókja és a hitelesítési jogkivonat a a [Twilio-konzol][twilio_console].</span><span class="sxs-lookup"><span data-stu-id="4083d-109">Acquire a Twilio account and authentication token from your [Twilio Console][twilio_console].</span></span> <span data-ttu-id="4083d-110">Twilio megkezdéséhez, értékelje ki a következő árképzési [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="4083d-110">To get started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="4083d-111">Regisztrálhat a próbafiókra: [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="4083d-111">You can sign up for a trial account at [https://www.twilio.com/try-twilio][try_twilio].</span></span>
2. <span data-ttu-id="4083d-112">Szerezze be a [Twilio-könyvtár a PHP](https://github.com/twilio/twilio-php) vagy KÖRTEFÁK csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4083d-112">Obtain the [Twilio library for PHP](https://github.com/twilio/twilio-php) or install it as a PEAR package.</span></span> <span data-ttu-id="4083d-113">További információkért lásd: a [információs fájl](https://github.com/twilio/twilio-php/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="4083d-113">For more information, see the [readme file](https://github.com/twilio/twilio-php/blob/master/README.md).</span></span>
3. <span data-ttu-id="4083d-114">Telepítse a PHP az Azure SDK-t.</span><span class="sxs-lookup"><span data-stu-id="4083d-114">Install the Azure SDK for PHP.</span></span> <span data-ttu-id="4083d-115">Az SDK-t és a telepítéséhez áttekintését lásd: [PHP az Azure SDK beállítása](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span><span class="sxs-lookup"><span data-stu-id="4083d-115">For an overview of the SDK and instructions on installing it, see [Set up the Azure SDK for PHP](app-service-web/web-sites-php-mysql-deploy-use-git.md)</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="4083d-116">A következő hívással webes űrlap létrehozása</span><span class="sxs-lookup"><span data-stu-id="4083d-116">Create a web form for making a call</span></span>
<span data-ttu-id="4083d-117">A következő HTML kód bemutatja, hogyan hozhat létre weblapot (**callform.html**), amely lekérdezi a felhasználó adatai egy hívás:</span><span class="sxs-lookup"><span data-stu-id="4083d-117">The following HTML code shows how to build a web page (**callform.html**) that retrieves user data for making a call:</span></span>

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
        <td><input name="callText" size="100" type="text" value="Hello. This is the call text. Good bye."></td>
      </tr>
      <tr>
        <td colspan="2"><input type="submit" value="Make this call"></td>
      </tr>
    </table>
  </form><br>
</body>
</html>
```

## <a name="create-the-code-to-make-the-call"></a><span data-ttu-id="4083d-118">A kód a híváshoz létrehozása</span><span class="sxs-lookup"><span data-stu-id="4083d-118">Create the code to make the call</span></span>
<span data-ttu-id="4083d-119">A következő kód bemutatja, hogyan hozhat létre **makecall.php**, amely más néven, amikor a felhasználó ad meg az űrlap által megjelenített **callform.html**.</span><span class="sxs-lookup"><span data-stu-id="4083d-119">The following code shows how to build **makecall.php**, which is called when the user submits the form displayed by **callform.html**.</span></span> <span data-ttu-id="4083d-120">Az alábbi kódot a hívás-üzenetet hoz létre, és állít elő, a hívást.</span><span class="sxs-lookup"><span data-stu-id="4083d-120">The code shown below creates the call message and generates the call.</span></span> <span data-ttu-id="4083d-121">Is, ügyeljen arra, hogy a Twilio-fiókja és -hitelesítés használata a jogkivonat a [Twilio-konzol] [ twilio_console] helyett a helyőrző értékeket rendelt **$sid** és ** $token** az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="4083d-121">Also, be sure to use your Twilio account and authentication token from the [Twilio Console][twilio_console] instead of the placeholder values assigned to **$sid** and **$token** in the code below.</span></span>

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

<span data-ttu-id="4083d-122">Azonkívül, hogy a hívás **makecall.php** megjeleníti az egyes hívás metaadatait, az alábbi képen látható módon.</span><span class="sxs-lookup"><span data-stu-id="4083d-122">In addition to making the call, **makecall.php** displays some call metadata, as is shown in the image below.</span></span> <span data-ttu-id="4083d-123">Hívás metaadatok kapcsolatos további információkért lásd: [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span><span class="sxs-lookup"><span data-stu-id="4083d-123">For more information about call metadata, see [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].</span></span>

![Azure hívási válasz Twilio és a PHP használatával][twilio_php_response]

## <a name="run-the-application"></a><span data-ttu-id="4083d-125">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4083d-125">Run the application</span></span>
<span data-ttu-id="4083d-126">A következő lépés, hogy az alkalmazást az Azure Websitesra helyezi üzembe.</span><span class="sxs-lookup"><span data-stu-id="4083d-126">The next step is to deploy your application to Azure Websites.</span></span> <span data-ttu-id="4083d-127">A következő cikkeket tartalmaz az adatokat egy webhely létrehozása és telepítése a Git-, FTP, vagy WebMatrix kódot, (bár nem minden cikkben szereplő információk vonatkozó):</span><span class="sxs-lookup"><span data-stu-id="4083d-127">The following articles contain the information for creating a website and deploying your code with Git, FTP, or WebMatrix (though not all information in each article is relevant):</span></span>

* [<span data-ttu-id="4083d-128">A PHP-MySQL Azure-webhely létrehozása és telepítése a Git használatával</span><span class="sxs-lookup"><span data-stu-id="4083d-128">Create a PHP-MySQL Azure Web Site and deploy using Git</span></span>](app-service-web/web-sites-php-mysql-deploy-use-git.md)
* [<span data-ttu-id="4083d-129">A PHP-MySQL az Azure webhelyén létrehozása és telepítése a FTP használatával</span><span class="sxs-lookup"><span data-stu-id="4083d-129">Create a PHP-MySQL Azure Web Site and Deploy Using FTP</span></span>](app-service-web/web-sites-php-mysql-deploy-use-ftp.md)

## <a name="next-steps"></a><span data-ttu-id="4083d-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4083d-130">Next steps</span></span>
<span data-ttu-id="4083d-131">Ez a kód Twilio használata Azure PHP alapvető funkciókat mutatjuk be lett megadva.</span><span class="sxs-lookup"><span data-stu-id="4083d-131">This code was provided to show you basic functionality using Twilio in PHP on Azure.</span></span> <span data-ttu-id="4083d-132">Mielőtt telepítené az Azure éles környezetben, érdemes lehet további hibakezelés vagy egyéb szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="4083d-132">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="4083d-133">Példa:</span><span class="sxs-lookup"><span data-stu-id="4083d-133">For example:</span></span>

* <span data-ttu-id="4083d-134">Egy webes űrlap használata helyett segítségével az Azure storage blobsba vagy SQL-adatbázis tárolja a telefonszámokat, és hívja meg a szöveg.</span><span class="sxs-lookup"><span data-stu-id="4083d-134">Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text.</span></span> <span data-ttu-id="4083d-135">További információ az Azure storage blobs használata a PHP: [Azure Storage használata PHP-alkalmazások][howto_blob_storage_php].</span><span class="sxs-lookup"><span data-stu-id="4083d-135">For information about using Azure storage blobs in PHP, see [Using Azure Storage with PHP Applications][howto_blob_storage_php].</span></span> <span data-ttu-id="4083d-136">SQL-adatbázis a PHP használatával kapcsolatos információkért lásd: [SQL-adatbázisok használata PHP-alkalmazások][howto_sql_azure_php].</span><span class="sxs-lookup"><span data-stu-id="4083d-136">For information about using SQL Database in PHP, see [Using SQL Database with PHP Applications][howto_sql_azure_php].</span></span>
* <span data-ttu-id="4083d-137">A **makecall.php** használ a Twilio-megadott URL-címet ([http://twimlets.com/message][twimlet_message_url]), amely arról értesíti a Twilio hogyan Twilio Markup Language (TwiML) választ a a hívás folytatásához.</span><span class="sxs-lookup"><span data-stu-id="4083d-137">The **makecall.php** code uses Twilio-provided URL ([http://twimlets.com/message][twimlet_message_url]) to provide a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call.</span></span> <span data-ttu-id="4083d-138">A visszaadott TwiML tartalmazhat például egy `<Say>` művelet, amely a hívás címzett felolvasását szöveg eredményez.</span><span class="sxs-lookup"><span data-stu-id="4083d-138">For example, the TwiML that is returned can contain a `<Say>` verb that results in text being spoken to the call recipient.</span></span> <span data-ttu-id="4083d-139">Ahelyett, hogy a Twilio-megadott URL-címet, a saját Twilio tartozó kérelem; szolgáltatás létrehozása sikerült További információkért lásd: [hang-és a PHP SMS képességek használatát Twilio hogyan][howto_twilio_voice_sms_php].</span><span class="sxs-lookup"><span data-stu-id="4083d-139">Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [How to Use Twilio for Voice and SMS Capabilities in PHP][howto_twilio_voice_sms_php].</span></span> <span data-ttu-id="4083d-140">További információ a TwiML található [http://www.twilio.com/docs/api/twiml][twiml], és további információ a `<Say>` és egyéb Twilio művelet található a [http:// www.twilio.com/docs/API/twiml/Say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="4083d-140">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about `<Say>` and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="4083d-141">Olvassa el a Twilio biztonsági irányelvek a [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="4083d-141">Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="4083d-142">További információ a Twilio: [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="4083d-142">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="4083d-143">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4083d-143">See Also</span></span>
* [<span data-ttu-id="4083d-144">Hang-és a PHP SMS lehetővé tevő szolgáltatásaival Twilio használata</span><span class="sxs-lookup"><span data-stu-id="4083d-144">How to Use Twilio for Voice and SMS Capabilities in PHP</span></span>](partner-twilio-php-how-to-use-voice-sms.md)

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
