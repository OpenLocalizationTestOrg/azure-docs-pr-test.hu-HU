---
title: "aaaUsing Twilio a hang, a VoIP és az SMS-üzenetkezelés az Azure-ban"
description: "Ismerje meg, hogyan toomake telefonhívást, majd küldje el a SMS üzenet hello Twilio API szolgáltatásban az Azure-on. A Kódminták a Node.js-ben."
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
ms.openlocfilehash: 6c44d60e217fcdf51e69fd2a8197f979afbb507a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a><span data-ttu-id="a53ed-104">Twilio használ hang, a VoIP és az SMS-üzenetkezelés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a53ed-104">Using Twilio for Voice, VoIP, and SMS Messaging in Azure</span></span>
<span data-ttu-id="a53ed-105">Ez az útmutató bemutatja, hogyan toobuild alkalmazásokat, amelyek kommunikálni Twilio és a node.js az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="a53ed-105">This guide demonstrates how toobuild apps that communicate with Twilio and node.js on Azure.</span></span>

<a id="whatis"/>

## <a name="what-is-twilio"></a><span data-ttu-id="a53ed-106">Mi az, hogy a Twilio?</span><span class="sxs-lookup"><span data-stu-id="a53ed-106">What is Twilio?</span></span>
<span data-ttu-id="a53ed-107">Twilio egy API-platform, amely megkönnyíti a fejlesztők toomake és telefonhívásokat fogadja, küldése és a szöveges üzeneteket, és beágyazási böngészőalapú és natív mobil alkalmazásokba VoIP hívása.</span><span class="sxs-lookup"><span data-stu-id="a53ed-107">Twilio is an API platform that makes it easy for developers toomake and receive phone calls, send and receive text messages, and embed VoIP calling into browser-based and native mobile applications.</span></span> <span data-ttu-id="a53ed-108">Most röviden ismerteti ennek működéséről belevágna előtt.</span><span class="sxs-lookup"><span data-stu-id="a53ed-108">Let's briefly go over how this works before diving in.</span></span>

### <a name="receiving-calls-and-text-messages"></a><span data-ttu-id="a53ed-109">Hívások és a szöveges üzenetek fogadása</span><span class="sxs-lookup"><span data-stu-id="a53ed-109">Receiving Calls and Text Messages</span></span>
<span data-ttu-id="a53ed-110">Twilio lehetővé teszi a fejlesztők túl[programozható telefonszámok beszerzési] [ purchase_phone] amely lehet használt tooboth és küldésére és fogadására hívások szöveges üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="a53ed-110">Twilio allows developers too[purchase programmable phone numbers][purchase_phone] which can be used tooboth send and receive calls and text messages.</span></span> <span data-ttu-id="a53ed-111">Amikor Twilio-számnak fogad egy bejövő hívás vagy szöveges, Twilio küld a webes alkalmazás egy HTTP POST vagy GET kérés kéri a hogyan toohandle hello hívást vagy SMS utasításokat.</span><span class="sxs-lookup"><span data-stu-id="a53ed-111">When a Twilio number receives an inbound call or text, Twilio will send your web application an HTTP POST or GET request, asking you for instructions on how toohandle hello call or text.</span></span> <span data-ttu-id="a53ed-112">A kiszolgáló válaszol a tooTwilio tartozó HTTP-kérelem [TwiML][twiml], egy utasításokat tartalmazó XML-címkék egyszerű készlete toohandle hívás vagy szöveges.</span><span class="sxs-lookup"><span data-stu-id="a53ed-112">Your server will respond tooTwilio's HTTP request with [TwiML][twiml], a simple set of XML tags that contain instructions on how toohandle a call or text.</span></span> <span data-ttu-id="a53ed-113">Kis türelmet a TwiML példái lesz látható.</span><span class="sxs-lookup"><span data-stu-id="a53ed-113">We will see examples of TwiML in just a moment.</span></span>

### <a name="making-calls-and-sending-text-messages"></a><span data-ttu-id="a53ed-114">Hívások és a szöveges üzenetek küldése</span><span class="sxs-lookup"><span data-stu-id="a53ed-114">Making Calls and Sending Text Messages</span></span>
<span data-ttu-id="a53ed-115">Azáltal, hogy a HTTP-kérelmek toohello Twilio webszolgáltatási API, fejlesztők szöveges üzenetek küldéséhez vagy kimenő telefonhívást kezdeményezni.</span><span class="sxs-lookup"><span data-stu-id="a53ed-115">By making HTTP requests toohello Twilio web service API, developers can send text messages or initiate outbound phone calls.</span></span> <span data-ttu-id="a53ed-116">Kimenő hívások hello fejlesztői is meg kell adnia egy URL-címet, amely visszaadja az TwiML utasításokat hogyan toohandle hello kimenő hívása után van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="a53ed-116">For outbound calls, hello developer must also specify a URL that returns TwiML instructions for how toohandle hello outbound call once it is connected.</span></span>

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a><span data-ttu-id="a53ed-117">VoIP képességek beágyazása a felhasználói felület kódot (JavaScript, iOS vagy Android)</span><span class="sxs-lookup"><span data-stu-id="a53ed-117">Embedding VoIP Capabilities in UI code (JavaScript, iOS, or Android)</span></span>
<span data-ttu-id="a53ed-118">Twilio biztosít egy ügyféloldali SDK, amely bármely asztali böngészőben, iOS-alkalmazás vagy Android-alkalmazás lehet kikapcsolni, a VoIP-telefont.</span><span class="sxs-lookup"><span data-stu-id="a53ed-118">Twilio provides a client-side SDK which can turn any desktop web browser, iOS app, or Android app into a VoIP phone.</span></span> <span data-ttu-id="a53ed-119">Ebben a cikkben tárgyaljuk hogyan toouse VoIP hello böngészőben hívja.</span><span class="sxs-lookup"><span data-stu-id="a53ed-119">In this article, we will focus on how toouse VoIP calling in hello browser.</span></span> <span data-ttu-id="a53ed-120">A hozzáadása toohello *Twilio JavaScript SDK* hello böngésző fut, a kiszolgálóoldali alkalmazás (a node.js-alkalmazás) használt tooissue egy "funkció token" toohello JavaScript ügyfél kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a53ed-120">In addition toohello *Twilio JavaScript SDK* running in hello browser, a server-side application (our node.js application) must be used tooissue a "capability token" toohello JavaScript client.</span></span> <span data-ttu-id="a53ed-121">További VoIP használata node.js [hello Twilio fejlesztői blog][voipnode].</span><span class="sxs-lookup"><span data-stu-id="a53ed-121">You can read more about using VoIP with node.js [on hello Twilio dev blog][voipnode].</span></span>

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a><span data-ttu-id="a53ed-122">Regisztrálás a Twilio (a Microsoft kedvezményes)</span><span class="sxs-lookup"><span data-stu-id="a53ed-122">Sign Up For Twilio (Microsoft Discount)</span></span>
<span data-ttu-id="a53ed-123">Twilio-szolgáltatások használatához először [egy fiók][signup].</span><span class="sxs-lookup"><span data-stu-id="a53ed-123">Before using Twilio services, you must first [sign up for an account][signup].</span></span> <span data-ttu-id="a53ed-124">Microsoft Azure-ügyfelek kap egy különös kedvezményes - [kell itt meg arról, hogy toosign][signup]!</span><span class="sxs-lookup"><span data-stu-id="a53ed-124">Microsoft Azure customers receive a special discount - [be sure toosign up here][signup]!</span></span>

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a><span data-ttu-id="a53ed-125">Létrehozhat és telepíthet egy node.js Azure-webhely</span><span class="sxs-lookup"><span data-stu-id="a53ed-125">Create and Deploy a node.js Azure Website</span></span>
<span data-ttu-id="a53ed-126">Ezt követően kell toocreate egy Azure-on futó node.js webhelyet.</span><span class="sxs-lookup"><span data-stu-id="a53ed-126">Next, you will need toocreate a node.js website running on Azure.</span></span> <span data-ttu-id="a53ed-127">[hello dokumentációs ehhez a következő helyen található][azure_new_site].</span><span class="sxs-lookup"><span data-stu-id="a53ed-127">[hello official documentation for doing this is located here][azure_new_site].</span></span> <span data-ttu-id="a53ed-128">Magas szinten akkor lesz aki hello következő:</span><span class="sxs-lookup"><span data-stu-id="a53ed-128">At a high level, you will be doing hello following:</span></span>

* <span data-ttu-id="a53ed-129">Regisztrál az Azure-fiók esetén, ha még nem rendelkezik már</span><span class="sxs-lookup"><span data-stu-id="a53ed-129">Signing up for an Azure account, if you don't have one already</span></span>
* <span data-ttu-id="a53ed-130">Hello Azure felügyeleti konzol toocreate új webhely használatával</span><span class="sxs-lookup"><span data-stu-id="a53ed-130">Using hello Azure admin console toocreate a new website</span></span>
* <span data-ttu-id="a53ed-131">Forrás-vezérlés támogatásának (indulunk követte git) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a53ed-131">Adding source control support (we will assume you used git)</span></span>
* <span data-ttu-id="a53ed-132">A fájl létrehozásakor a következő `server.js` az egyszerű node.js-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a53ed-132">Creating a file `server.js` with a simple node.js web application</span></span>
* <span data-ttu-id="a53ed-133">Ez egyszerű alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="a53ed-133">Deploying this simple application tooAzure</span></span>

<a id="twiliomodule"/>

## <a name="configure-hello-twilio-module"></a><span data-ttu-id="a53ed-134">Hello Twilio-modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a53ed-134">Configure hello Twilio Module</span></span>
<span data-ttu-id="a53ed-135">A következő lesz az első lépések toowrite hello Twilio API használatát egy egyszerű node.js-alkalmazás, amely lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="a53ed-135">Next, we will begin toowrite a simple node.js application which makes use of hello Twilio API.</span></span> <span data-ttu-id="a53ed-136">Az első lépések, igazolnia kell tooconfigure a Twilio-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a53ed-136">Before we begin, we need tooconfigure our Twilio account credentials.</span></span>

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a><span data-ttu-id="a53ed-137">Rendszerszintű környezeti változókat a Twilio-hitelesítő adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a53ed-137">Configuring Twilio Credentials in System Environment Variables</span></span>
<span data-ttu-id="a53ed-138">A sorrend hitelesített toomake kéréseket a meghatározott hello Twilio-háttér igazolnia kell a fiók SID azonosítója és a hitelesítési jogkivonat, mely funkciót hello felhasználónevet és jelszót a Twilio-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="a53ed-138">In order toomake authenticated requests against hello Twilio back end, we need our account SID and auth token, which function as hello username and password set for our Twilio account.</span></span> <span data-ttu-id="a53ed-139">hello legbiztonságosabb módja tooconfigure ezek hello csomópont modul az Azure-ban való használatra van keresztül rendszerszintű környezeti változókat, amelyek közvetlenül hello Azure felügyeleti konzolon állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="a53ed-139">hello most secure way tooconfigure these for use with hello node module in Azure is via system environment variables, which you can set directly in hello Azure admin console.</span></span>

<span data-ttu-id="a53ed-140">Jelölje ki a node.js-webhelyet, és hello "Beállítása" hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a53ed-140">Select your node.js website, and click hello "CONFIGURE" link.</span></span>  <span data-ttu-id="a53ed-141">Ha görgessen lefelé kissé a folyamatot, akkor jelenik meg egy olyan területre, amelyen meg a konfigurációs tulajdonságok az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="a53ed-141">If you scroll down a bit, you will see an area where you can set configuration properties for your application.</span></span>  <span data-ttu-id="a53ed-142">Adja meg a Twilio-fiók hitelesítő adatait ([megtalálható-e a Twilio-konzol][twilio_console]) látható – győződjön meg arról, hogy tooname őket `TWILIO_ACCOUNT_SID` és `TWILIO_AUTH_TOKEN`, illetve:</span><span class="sxs-lookup"><span data-stu-id="a53ed-142">Enter your Twilio account credentials ([found on your Twilio Console][twilio_console]) as shown - make sure tooname them `TWILIO_ACCOUNT_SID` and `TWILIO_AUTH_TOKEN`, respectively:</span></span>

![Az Azure felügyeleti konzol][azure-admin-console]

<span data-ttu-id="a53ed-144">Miután konfigurálta a változókat, indítsa újra az alkalmazás hello Azure-konzolon.</span><span class="sxs-lookup"><span data-stu-id="a53ed-144">Once you have configured these variables, restart your application in hello Azure console.</span></span>

### <a name="declaring-hello-twilio-module-in-packagejson"></a><span data-ttu-id="a53ed-145">Deklaráló package.json hello Twilio-modulja</span><span class="sxs-lookup"><span data-stu-id="a53ed-145">Declaring hello Twilio module in package.json</span></span>
<span data-ttu-id="a53ed-146">A következő igazolnia kell a package.json toomanage toocreate a csomópont modul függőségek keresztül [npm].</span><span class="sxs-lookup"><span data-stu-id="a53ed-146">Next, we need toocreate a package.json toomanage our node module dependencies via [npm].</span></span> <span data-ttu-id="a53ed-147">Hello: azonos szinten, hello `server.js` hello létrehozott fájl *Azure/node.js* oktatóanyag, hozzon létre egy fájlt `package.json`.</span><span class="sxs-lookup"><span data-stu-id="a53ed-147">At hello same level as hello `server.js` file you created in hello *Azure/node.js* tutorial, create a file named `package.json`.</span></span>  <span data-ttu-id="a53ed-148">Ez a fájl helye hello következő:</span><span class="sxs-lookup"><span data-stu-id="a53ed-148">Inside this file, place hello following:</span></span>

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

<span data-ttu-id="a53ed-149">Ez azt deklarálja hello twilio-modul, egy függőségi, valamint a hello népszerű [webes keretrendszer Express] [ express] és hello EJS sablon motor.</span><span class="sxs-lookup"><span data-stu-id="a53ed-149">This declares hello twilio module as a dependency, as well as hello popular [Express web framework][express] and hello EJS template engine.</span></span>  <span data-ttu-id="a53ed-150">Rendben most azt Mindennel - most kódírást!</span><span class="sxs-lookup"><span data-stu-id="a53ed-150">Okay, now we're all set - let's write some code!</span></span>

<a id="makecall"/>

## <a name="make-an-outbound-call"></a><span data-ttu-id="a53ed-151">Egy kimenő hívást</span><span class="sxs-lookup"><span data-stu-id="a53ed-151">Make an Outbound Call</span></span>
<span data-ttu-id="a53ed-152">Hozzon létre egy egyszerű űrlapot választjuk hívás tooa több helyezi el.</span><span class="sxs-lookup"><span data-stu-id="a53ed-152">Let's create a simple form that will place a call tooa number we choose.</span></span> <span data-ttu-id="a53ed-153">Nyissa meg `server.js`, és írja be a kódját a következő hello.</span><span class="sxs-lookup"><span data-stu-id="a53ed-153">Open up `server.js`, and enter hello following code.</span></span> <span data-ttu-id="a53ed-154">Vegye figyelembe, ha "CHANGE_ME" - hello a azure webhely neve nincs put felirat látható:</span><span class="sxs-lookup"><span data-stu-id="a53ed-154">Note where it says "CHANGE_ME" - put hello name of your azure website there:</span></span>

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

// Render an HTML user interface for hello application's home page
app.get('/', (request, response) => response.render('index'));

// Handle hello form POST tooplace a call
app.post('/call', (request, response) => {
  var client = twilio(accountSid, authToken);

  client.makeCall({
    // make a call toothis number
    to:request.body.number,

    // Change tooa Twilio number you bought - see:
    // https://www.twilio.com/console/phone-numbers/incoming
    from:'+15558675309',

    // A URL in our app which generates TwiML
    // Change "CHANGE_ME" tooyour app's name
    url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
  }, () => {
      // Go back toohello home page
      response.redirect('/');
  });
});

// Generate TwiML toohandle an outbound call
app.post('/outbound_call', (request, response) => {
  var twiml = new twilio.TwimlResponse();

  // Say a message toohello call's receiver
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

<span data-ttu-id="a53ed-155">Ezután hozzon létre egy könyvtárat `views` - belül ebben a könyvtárban, hozzon létre egy fájlt `index.ejs` a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a53ed-155">Next, create a directory called `views` - inside this directory, create a file named `index.ejs` with hello following contents:</span></span>

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
      <input type="submit" value="Call hello number above"/>
  </form>
</body>
</html>
```

<span data-ttu-id="a53ed-156">Most a webhely tooAzure telepítése, és nyissa meg az új kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="a53ed-156">Now, deploy your website tooAzure and open your home.</span></span> <span data-ttu-id="a53ed-157">Legyen képes tooenter hello szövegmezőben a telefonszámát, és hívás fogadása a Twilio-számot!</span><span class="sxs-lookup"><span data-stu-id="a53ed-157">You should be able tooenter your phone number in hello text field, and receive a call from your Twilio number!</span></span>

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a><span data-ttu-id="a53ed-158">SMS küldése</span><span class="sxs-lookup"><span data-stu-id="a53ed-158">Send an SMS Message</span></span>
<span data-ttu-id="a53ed-159">Most állítsa be a felhasználói felület és az űrlap logika toosend kezelése szöveges üzenetet.</span><span class="sxs-lookup"><span data-stu-id="a53ed-159">Now, let's set up a user interface and form handling logic toosend a text message.</span></span> <span data-ttu-id="a53ed-160">Nyissa meg `server.js`, és adja hozzá a kód túl hello utolsó hívása után következő hello`app.post`:</span><span class="sxs-lookup"><span data-stu-id="a53ed-160">Open up `server.js`, and add hello following code after hello last call too`app.post`:</span></span>

```javascript
app.post('/sms', (request, response) => {
  const client = twilio(accountSid, authToken);

  client.sendSms({
      // send a text toothis number
      to:request.body.number,

      // A Twilio number you bought - see:
      // https://www.twilio.com/console/phone-numbers/incoming
      from:'+15558675309',

      // hello body of hello text message
      body: request.body.message

  }, () => {
      // Go back toohello home page
      response.redirect('/');
  });
});
```

<span data-ttu-id="a53ed-161">A `views/index.ejs`, adja hozzá egy másik űrlapon hello először egy toosubmit a szám és a szöveges üzenet:</span><span class="sxs-lookup"><span data-stu-id="a53ed-161">In `views/index.ejs`, add another form under hello first one toosubmit a number and a text message:</span></span>

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message toosend" name="message"/>
  <br/>
  <input type="submit" value="Send text toohello number above"/>
</form>
```

<span data-ttu-id="a53ed-162">Hozza létre újra az alkalmazás tooAzure, és képes toosubmit alkotnak, és saját kezűleg (vagy bármely legközelebbi ismerősei) tartalmazó szöveges üzenetet küldjön kell!</span><span class="sxs-lookup"><span data-stu-id="a53ed-162">Re-deploy your application tooAzure, and you should now be able toosubmit that form and send yourself (or any of your closest friends) a text message!</span></span>

<a id="nextsteps"/>

## <a name="next-steps"></a><span data-ttu-id="a53ed-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a53ed-163">Next Steps</span></span>
<span data-ttu-id="a53ed-164">Most megtanulta, node.js és Twilio toobuild alkalmazások kommunikáló hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="a53ed-164">You have now learned hello basics of using node.js and Twilio toobuild apps that communicate.</span></span> <span data-ttu-id="a53ed-165">De ezek a példák alig ideiglenes hello felületet, hogy a Twilio és node.js lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a53ed-165">But these examples barely scratch hello surface of what's possible with Twilio and node.js.</span></span> <span data-ttu-id="a53ed-166">Twilio használata node.js további információkért tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="a53ed-166">For more information using Twilio with node.js, check out hello following resources:</span></span>

* <span data-ttu-id="a53ed-167">[Hivatalos modul docs][docs]</span><span class="sxs-lookup"><span data-stu-id="a53ed-167">[Official module docs][docs]</span></span>
* <span data-ttu-id="a53ed-168">[Node.js-alkalmazások a VoIP-oktatóanyag][voipnode]</span><span class="sxs-lookup"><span data-stu-id="a53ed-168">[Tutorial on VoIP with node.js applications][voipnode]</span></span>
* <span data-ttu-id="a53ed-169">[Votr - egy valós idejű SMS szavazás alkalmazás a node.js és CouchDB (három rész)][votr]</span><span class="sxs-lookup"><span data-stu-id="a53ed-169">[Votr - a real-time SMS voting application with node.js and CouchDB (three parts)][votr]</span></span>
* <span data-ttu-id="a53ed-170">[A node.js hello böngészőben pár programozás][pair]</span><span class="sxs-lookup"><span data-stu-id="a53ed-170">[Pair programming in hello browser with node.js][pair]</span></span>

<span data-ttu-id="a53ed-171">Reméljük, node.js és Twilio támadásoknak Azure kedvelt!</span><span class="sxs-lookup"><span data-stu-id="a53ed-171">We hope you love hacking node.js and Twilio on Azure!</span></span>

[purchase_phone]: https://www.twilio.com/console/phone-numbers/search
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: app-service-web/app-service-web-get-started-nodejs.md
[twilio_console]: https://www.twilio.com/console
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png
