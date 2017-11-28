---
title: "Twilio használ hang, a VoIP és az SMS-üzenetkezelés az Azure-ban"
description: "Útmutató a telefonhívás, és a Twilio API szolgáltatás SMS üzenet küldése az Azure-on. A Kódminták a Node.js-ben."
services: 
documentationcenter: nodejs
author: devinrader
manager: wpickett
editor: 
ms.assetid: f558cbbd-13d2-416f-b9b1-33a99c426af9
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 11/25/2014
ms.author: wpickett
ms.openlocfilehash: 44ec97812130d41d75be98fc8e2d846b7cb5c913
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a><span data-ttu-id="ef922-104">Twilio használ hang, a VoIP és az SMS-üzenetkezelés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ef922-104">Using Twilio for Voice, VoIP, and SMS Messaging in Azure</span></span>
<span data-ttu-id="ef922-105">Ez az útmutató bemutatja, hogyan kommunikálni Twilio és az Azure-on node.js alkalmazásokat lehet készíteni.</span><span class="sxs-lookup"><span data-stu-id="ef922-105">This guide demonstrates how to build apps that communicate with Twilio and node.js on Azure.</span></span>

<a id="whatis"/>

## <a name="what-is-twilio"></a><span data-ttu-id="ef922-106">Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="ef922-106">What is Twilio?</span></span>
<span data-ttu-id="ef922-107">Twilio egy API-platform, amely megkönnyíti a fejlesztők és telefonhívásokat fogadja, küldése és a szöveges üzeneteket, és beágyazási böngészőalapú és natív mobil alkalmazásokba VoIP hívása.</span><span class="sxs-lookup"><span data-stu-id="ef922-107">Twilio is an API platform that makes it easy for developers to make and receive phone calls, send and receive text messages, and embed VoIP calling into browser-based and native mobile applications.</span></span> <span data-ttu-id="ef922-108">Most röviden ismerteti ennek működéséről belevágna előtt.</span><span class="sxs-lookup"><span data-stu-id="ef922-108">Let's briefly go over how this works before diving in.</span></span>

### <a name="receiving-calls-and-text-messages"></a><span data-ttu-id="ef922-109">Hívások és a szöveges üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="ef922-109">Receiving Calls and Text Messages</span></span>
<span data-ttu-id="ef922-110">Twilio lehetővé teszi a fejlesztők számára [programozható telefonszámok beszerzési] [ purchase_phone] való küldenie és fogadnia hívások és a szöveges üzenetek is használható.</span><span class="sxs-lookup"><span data-stu-id="ef922-110">Twilio allows developers to [purchase programmable phone numbers][purchase_phone] which can be used to both send and receive calls and text messages.</span></span> <span data-ttu-id="ef922-111">Ha a Twilio-számnak fogad egy bejövő hívás vagy szöveges, Twilio elküld a webes alkalmazás egy HTTP POST vagy GET kérés, kéri a hívást vagy SMS kezeléséről további tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="ef922-111">When a Twilio number receives an inbound call or text, Twilio will send your web application an HTTP POST or GET request, asking you for instructions on how to handle the call or text.</span></span> <span data-ttu-id="ef922-112">A kiszolgáló válaszol a Twilio tartozó HTTP-kérelem [TwiML][twiml], egyszerű hívás vagy SMS kezelésére vonatkozó útmutatást tartalmazó XML-címkék-készlettel.</span><span class="sxs-lookup"><span data-stu-id="ef922-112">Your server will respond to Twilio's HTTP request with [TwiML][twiml], a simple set of XML tags that contain instructions on how to handle a call or text.</span></span> <span data-ttu-id="ef922-113">Kis türelmet a TwiML példái lesz látható.</span><span class="sxs-lookup"><span data-stu-id="ef922-113">We will see examples of TwiML in just a moment.</span></span>

### <a name="making-calls-and-sending-text-messages"></a><span data-ttu-id="ef922-114">Hívások és a szöveges üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="ef922-114">Making Calls and Sending Text Messages</span></span>
<span data-ttu-id="ef922-115">HTTP-kérelmekre, így a Twilio webszolgáltatási API, fejlesztők szöveges üzenetek küldéséhez vagy kimenő telefonhívást kezdeményezni.</span><span class="sxs-lookup"><span data-stu-id="ef922-115">By making HTTP requests to the Twilio web service API, developers can send text messages or initiate outbound phone calls.</span></span> <span data-ttu-id="ef922-116">Kimenő hívások a fejlesztői is meg kell adnia egy URL-címet, amely visszaadja az TwiML című cikkben talál útmutatást a kimenő hívás kezelésére, amennyiben van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="ef922-116">For outbound calls, the developer must also specify a URL that returns TwiML instructions for how to handle the outbound call once it is connected.</span></span>

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a><span data-ttu-id="ef922-117">VoIP képességek beágyazása a felhasználói felület kódot (JavaScript, iOS vagy Android)</span><span class="sxs-lookup"><span data-stu-id="ef922-117">Embedding VoIP Capabilities in UI code (JavaScript, iOS, or Android)</span></span>
<span data-ttu-id="ef922-118">Twilio biztosít egy ügyféloldali SDK, amely bármely asztali böngészőben, iOS-alkalmazás vagy Android-alkalmazás lehet kikapcsolni, a VoIP-telefont.</span><span class="sxs-lookup"><span data-stu-id="ef922-118">Twilio provides a client-side SDK which can turn any desktop web browser, iOS app, or Android app into a VoIP phone.</span></span> <span data-ttu-id="ef922-119">Ebben a cikkben tárgyaljuk használata VoIP hívja a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="ef922-119">In this article, we will focus on how to use VoIP calling in the browser.</span></span> <span data-ttu-id="ef922-120">Kívül a *Twilio JavaScript SDK* a böngésző fut, a kiszolgálóoldali alkalmazás (a node.js-alkalmazás) kell használni egy "funkció token" kiadni a JavaScript-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ef922-120">In addition to the *Twilio JavaScript SDK* running in the browser, a server-side application (our node.js application) must be used to issue a "capability token" to the JavaScript client.</span></span> <span data-ttu-id="ef922-121">További VoIP használata node.js [a Twilio-fejlesztői blog][voipnode].</span><span class="sxs-lookup"><span data-stu-id="ef922-121">You can read more about using VoIP with node.js [on the Twilio dev blog][voipnode].</span></span>

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a><span data-ttu-id="ef922-122">Regisztrálás a Twilio (a Microsoft kedvezményes)</span><span class="sxs-lookup"><span data-stu-id="ef922-122">Sign Up For Twilio (Microsoft Discount)</span></span>
<span data-ttu-id="ef922-123">Twilio-szolgáltatások használatához először [egy fiók][signup].</span><span class="sxs-lookup"><span data-stu-id="ef922-123">Before using Twilio services, you must first [sign up for an account][signup].</span></span> <span data-ttu-id="ef922-124">Microsoft Azure-ügyfelek kap egy különös kedvezményes - [ügyeljen arra, hogy itt regisztrálhat][signup]!</span><span class="sxs-lookup"><span data-stu-id="ef922-124">Microsoft Azure customers receive a special discount - [be sure to sign up here][signup]!</span></span>

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a><span data-ttu-id="ef922-125">Létrehozhat és telepíthet egy node.js Azure-webhely</span><span class="sxs-lookup"><span data-stu-id="ef922-125">Create and Deploy a node.js Azure Website</span></span>
<span data-ttu-id="ef922-126">A következő szüksége lesz egy Azure-on futó node.js-webhelyet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ef922-126">Next, you will need to create a node.js website running on Azure.</span></span> <span data-ttu-id="ef922-127">[Ezzel a hivatalos dokumentációját a következő helyen található][azure_new_site].</span><span class="sxs-lookup"><span data-stu-id="ef922-127">[The official documentation for doing this is located here][azure_new_site].</span></span> <span data-ttu-id="ef922-128">Magas szinten, akkor lesz kell a következő módon:</span><span class="sxs-lookup"><span data-stu-id="ef922-128">At a high level, you will be doing the following:</span></span>

* <span data-ttu-id="ef922-129">Regisztrál az Azure-fiók esetén, ha még nem rendelkezik már</span><span class="sxs-lookup"><span data-stu-id="ef922-129">Signing up for an Azure account, if you don't have one already</span></span>
* <span data-ttu-id="ef922-130">Új webhely létrehozása az Azure felügyeleti konzolját használja</span><span class="sxs-lookup"><span data-stu-id="ef922-130">Using the Azure admin console to create a new website</span></span>
* <span data-ttu-id="ef922-131">Forrás-vezérlés támogatásának (indulunk követte git) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ef922-131">Adding source control support (we will assume you used git)</span></span>
* <span data-ttu-id="ef922-132">A fájl létrehozásakor a következő `server.js` az egyszerű node.js-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="ef922-132">Creating a file `server.js` with a simple node.js web application</span></span>
* <span data-ttu-id="ef922-133">Az Azure-bA az egyszerű alkalmazás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="ef922-133">Deploying this simple application to Azure</span></span>

<a id="twiliomodule"/>

## <a name="configure-the-twilio-module"></a><span data-ttu-id="ef922-134">A Twilio-modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ef922-134">Configure the Twilio Module</span></span>
<span data-ttu-id="ef922-135">Megkezdődik a következő azt írni egy egyszerű node.js-alkalmazást, amely a Twilio API-t használja.</span><span class="sxs-lookup"><span data-stu-id="ef922-135">Next, we will begin to write a simple node.js application which makes use of the Twilio API.</span></span> <span data-ttu-id="ef922-136">Az első lépések, igazolnia kell a Twilio-fiók hitelesítő adatainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ef922-136">Before we begin, we need to configure our Twilio account credentials.</span></span>

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a><span data-ttu-id="ef922-137">Rendszerszintű környezeti változókat a Twilio-hitelesítő adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ef922-137">Configuring Twilio Credentials in System Environment Variables</span></span>
<span data-ttu-id="ef922-138">Ahhoz, hogy a Twilio-háttér elleni hitelesített kéréseket, igazolnia kell a fiók SID azonosítója és a hitelesítési jogkivonat, mely a felhasználónevet és jelszót a Twilio-fiókjában funkciót.</span><span class="sxs-lookup"><span data-stu-id="ef922-138">In order to make authenticated requests against the Twilio back end, we need our account SID and auth token, which function as the username and password set for our Twilio account.</span></span> <span data-ttu-id="ef922-139">A legbiztonságosabb konfigurálásukkal használható az Azure-ban a csomópont modullal módja keresztül rendszerszintű környezeti változókat, amelyek közvetlenül az Azure felügyeleti konzolon állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="ef922-139">The most secure way to configure these for use with the node module in Azure is via system environment variables, which you can set directly in the Azure admin console.</span></span>

<span data-ttu-id="ef922-140">Jelölje ki a node.js-webhelyet, és kattintson a "Beállítása" hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="ef922-140">Select your node.js website, and click the "CONFIGURE" link.</span></span>  <span data-ttu-id="ef922-141">Ha görgessen lefelé kissé a folyamatot, akkor jelenik meg egy olyan területre, amelyen meg a konfigurációs tulajdonságok az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="ef922-141">If you scroll down a bit, you will see an area where you can set configuration properties for your application.</span></span>  <span data-ttu-id="ef922-142">Adja meg a Twilio-fiók hitelesítő adatait ([megtalálható-e a Twilio-konzol][twilio_console]) látható - ügyeljen arra, hogy azok neve `TWILIO_ACCOUNT_SID` és `TWILIO_AUTH_TOKEN`, illetve:</span><span class="sxs-lookup"><span data-stu-id="ef922-142">Enter your Twilio account credentials ([found on your Twilio Console][twilio_console]) as shown - make sure to name them `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`, respectively:</span></span>

![Az Azure felügyeleti konzol][azure-admin-console]

<span data-ttu-id="ef922-144">Miután konfigurálta a változókat, indítsa újra az alkalmazást az Azure-konzolon.</span><span class="sxs-lookup"><span data-stu-id="ef922-144">Once you have configured these variables, restart your application in the Azure console.</span></span>

### <a name="declaring-the-twilio-module-in-packagejson"></a><span data-ttu-id="ef922-145">A Twilio-modul a Package.JSON kódjában deklaráló</span><span class="sxs-lookup"><span data-stu-id="ef922-145">Declaring the Twilio module in package.json</span></span>
<span data-ttu-id="ef922-146">Következő lépésként létre kell hoznunk egy package.json kezelheti a csomópont modul függőségek keresztül [npm].</span><span class="sxs-lookup"><span data-stu-id="ef922-146">Next, we need to create a package.json to manage our node module dependencies via [npm].</span></span> <span data-ttu-id="ef922-147">Ugyanazon a szinten az `server.js` létrehozott fájlt a *Azure/node.js* oktatóanyag, hozzon létre egy fájlt `package.json`.</span><span class="sxs-lookup"><span data-stu-id="ef922-147">At the same level as the `server.js` file you created in the *Azure/node.js* tutorial, create a file named `package.json`.</span></span>  <span data-ttu-id="ef922-148">Ez a fájl helye a következő:</span><span class="sxs-lookup"><span data-stu-id="ef922-148">Inside this file, place the following:</span></span>

```json
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node server"
  },
  "dependencies": {
    "body-parser": "^1.16.1",
    "ejs": "^2.5.5",
    "errorhandler": "^1.5.0",
    "express": "^4.14.1",
    "morgan": "^1.8.1",
    "twilio": "^2.11.1"
  }
}
```

<span data-ttu-id="ef922-149">Ez a twilio-modul deklarál, egy függőségi, valamint a népszerű [webes keretrendszer Express] [ express] és a EJS sablon motor.</span><span class="sxs-lookup"><span data-stu-id="ef922-149">This declares the twilio module as a dependency, as well as the popular [Express web framework][express] and the EJS template engine.</span></span>  <span data-ttu-id="ef922-150">Rendben most azt Mindennel - most kódírást!</span><span class="sxs-lookup"><span data-stu-id="ef922-150">Okay, now we're all set - let's write some code!</span></span>

<a id="makecall"/>

## <a name="make-an-outbound-call"></a><span data-ttu-id="ef922-151">Egy kimenő hívást</span><span class="sxs-lookup"><span data-stu-id="ef922-151">Make an Outbound Call</span></span>
<span data-ttu-id="ef922-152">Hozzon létre helyezi el a szám választjuk hívás egyszerű űrlapot.</span><span class="sxs-lookup"><span data-stu-id="ef922-152">Let's create a simple form that will place a call to a number we choose.</span></span> <span data-ttu-id="ef922-153">Nyissa meg `server.js`, és írja be a következő kódot.</span><span class="sxs-lookup"><span data-stu-id="ef922-153">Open up `server.js`, and enter the following code.</span></span> <span data-ttu-id="ef922-154">Megjegyzés: Ha "CHANGE_ME" - felirat látható az azure-webhely neve nincs put:</span><span class="sxs-lookup"><span data-stu-id="ef922-154">Note where it says "CHANGE_ME" - put the name of your azure website there:</span></span>

```javascript
// Module dependencies
const express = require('express');
const path = require('path');
const http = require('http');
const twilio = require('twilio');
const logger = require('morgan');
const bodyParser = require('body-parser');
const errorHandler = require('errorhandler');
const accountSid = process.env.TWILIO_ACCOUNT_SID;
const authToken = process.env.TWILIO_AUTH_TOKEN;
// Create Express web application
const app = express();

// Express configuration
app.set('port', process.env.PORT || 3000);
app.set('views', __dirname + '/views');
app.set('view engine', 'ejs');
app.use(logger('tiny'));
app.use(bodyParser.urlencoded({ extended: false }))
app.use(bodyParser.json())
app.use(express.static(path.join(__dirname, 'public')));

if (app.get('env') !== 'production') {
  app.use(errorHandler());
}

// Render an HTML user interface for the application's home page
app.get('/', (request, response) => response.render('index'));

// Handle the form POST to place a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call to this number
    to:request.body.number,

    // Change to a Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" to your app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});

// Generate TwiML to handle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message to the call's receiver
  twiml.say('hello - thanks for checking out Twilio and Azure', {
      voice:'woman'
  });

  response.set('Content-Type', 'text/xml');
  response.send(twiml.toString());
});

// Start server
app.listen(app.get('port'), function(){
  console.log(`Express server listening on port ${app.get('port')}`);
});
```

<span data-ttu-id="ef922-155">Ezután hozzon létre egy könyvtárat `views` - belül ebben a könyvtárban, hozzon létre egy fájlt `index.ejs` a következő tartalommal:</span><span class="sxs-lookup"><span data-stu-id="ef922-155">Next, create a directory called `views` - inside this directory, create a file named `index.ejs` with the following contents:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
  <title>Twilio Test</title>
  <style>
    input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
  </style>
</head>
<body>
  <h1>Twilio Test</h1>
  <form action="/call" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input type="submit" value="Call the number above"/>
  </form>
</body>
</html>
```

<span data-ttu-id="ef922-156">Most a webhely telepítése az Azure-ba, és nyissa meg az új kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="ef922-156">Now, deploy your website to Azure and open your home.</span></span> <span data-ttu-id="ef922-157">A szövegmezőbe írja be a megadott telefonszámot, és a felhívja a Twilio több képesnek kell lennie!</span><span class="sxs-lookup"><span data-stu-id="ef922-157">You should be able to enter your phone number in the text field, and receive a call from your Twilio number!</span></span>

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a><span data-ttu-id="ef922-158">SMS küldése</span><span class="sxs-lookup"><span data-stu-id="ef922-158">Send an SMS Message</span></span>
<span data-ttu-id="ef922-159">Most állítsa be a felhasználói felület és az űrlap logika kezelése szöveges üzenetet küldeni.</span><span class="sxs-lookup"><span data-stu-id="ef922-159">Now, let's set up a user interface and form handling logic to send a text message.</span></span> <span data-ttu-id="ef922-160">Nyissa meg `server.js`, és adja hozzá a következő kódot után utolsó hívása `app.post`:</span><span class="sxs-lookup"><span data-stu-id="ef922-160">Open up `server.js`, and add the following code after the last call to `app.post`:</span></span>

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text to this number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // The body of the text message
      body: request.body.message

  }, () => {
      // Go back to the home page
      response.redirect('/');
  });
});
```

<span data-ttu-id="ef922-161">A `views/index.ejs`, egy másik űrlapon egy számot és egy szöveges üzenetet küldeni az elsőt a hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ef922-161">In `views/index.ejs`, add another form under the first one to submit a number and a text message:</span></span>

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message to send" name="message"/>
  <br/>
  <input type="submit" value="Send text to the number above"/>
</form>
```

<span data-ttu-id="ef922-162">Hozza létre újra az alkalmazás az Azure-ba, és meg kell küldeni, amelyek kialakításához, és saját kezűleg (vagy bármely legközelebbi ismerősei) tartalmazó szöveges üzenetet küldjön!</span><span class="sxs-lookup"><span data-stu-id="ef922-162">Re-deploy your application to Azure, and you should now be able to submit that form and send yourself (or any of your closest friends) a text message!</span></span>

<a id="nextsteps"/>

## <a name="next-steps"></a><span data-ttu-id="ef922-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef922-163">Next Steps</span></span>
<span data-ttu-id="ef922-164">Most megtanulta, node.js és Twilio kommunikáló alkalmazások használatának alapjaival.</span><span class="sxs-lookup"><span data-stu-id="ef922-164">You have now learned the basics of using node.js and Twilio to build apps that communicate.</span></span> <span data-ttu-id="ef922-165">De ezek a példák alig ideiglenes a felületet, hogy a Twilio és node.js lehetséges.</span><span class="sxs-lookup"><span data-stu-id="ef922-165">But these examples barely scratch the surface of what's possible with Twilio and node.js.</span></span> <span data-ttu-id="ef922-166">Twilio használata node.js további információkért tekintse meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="ef922-166">For more information using Twilio with node.js, check out the following resources:</span></span>

* <span data-ttu-id="ef922-167">[Hivatalos modul docs][docs]</span><span class="sxs-lookup"><span data-stu-id="ef922-167">[Official module docs][docs]</span></span>
* <span data-ttu-id="ef922-168">[Node.js-alkalmazások a VoIP-oktatóanyag][voipnode]</span><span class="sxs-lookup"><span data-stu-id="ef922-168">[Tutorial on VoIP with node.js applications][voipnode]</span></span>
* <span data-ttu-id="ef922-169">[Votr - egy valós idejű SMS szavazás alkalmazás a node.js és CouchDB (három rész)][votr]</span><span class="sxs-lookup"><span data-stu-id="ef922-169">[Votr - a real-time SMS voting application with node.js and CouchDB (three parts)][votr]</span></span>
* <span data-ttu-id="ef922-170">[A böngészőben a node.js pár programozás][pair]</span><span class="sxs-lookup"><span data-stu-id="ef922-170">[Pair programming in the browser with node.js][pair]</span></span>

<span data-ttu-id="ef922-171">Reméljük, node.js és Twilio támadásoknak Azure kedvelt!</span><span class="sxs-lookup"><span data-stu-id="ef922-171">We hope you love hacking node.js and Twilio on Azure!</span></span>

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service-web/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
<span data-ttu-id="ef922-172">[npm]: http://npmjs.org</span><span class="sxs-lookup"><span data-stu-id="ef922-172">[npm]: http://npmjs.org</span></span>
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
