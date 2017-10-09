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
# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Twilio használ hang, a VoIP és az SMS-üzenetkezelés az Azure-ban
Ez az útmutató bemutatja, hogyan toobuild alkalmazásokat, amelyek kommunikálni Twilio és a node.js az Azure-on.

<a id="whatis"/>

## <a name="what-is-twilio"></a>Mi az, hogy a Twilio?
Twilio egy API-platform, amely megkönnyíti a fejlesztők toomake és telefonhívásokat fogadja, küldése és a szöveges üzeneteket, és beágyazási böngészőalapú és natív mobil alkalmazásokba VoIP hívása. Most röviden ismerteti ennek működéséről belevágna előtt.

### <a name="receiving-calls-and-text-messages"></a>Hívások és a szöveges üzenetek fogadása
Twilio lehetővé teszi a fejlesztők túl[programozható telefonszámok beszerzési] [ purchase_phone] amely lehet használt tooboth és küldésére és fogadására hívások szöveges üzeneteket. Amikor Twilio-számnak fogad egy bejövő hívás vagy szöveges, Twilio küld a webes alkalmazás egy HTTP POST vagy GET kérés kéri a hogyan toohandle hello hívást vagy SMS utasításokat. A kiszolgáló válaszol a tooTwilio tartozó HTTP-kérelem [TwiML][twiml], egy utasításokat tartalmazó XML-címkék egyszerű készlete toohandle hívás vagy szöveges. Kis türelmet a TwiML példái lesz látható.

### <a name="making-calls-and-sending-text-messages"></a>Hívások és a szöveges üzenetek küldése
Azáltal, hogy a HTTP-kérelmek toohello Twilio webszolgáltatási API, fejlesztők szöveges üzenetek küldéséhez vagy kimenő telefonhívást kezdeményezni. Kimenő hívások hello fejlesztői is meg kell adnia egy URL-címet, amely visszaadja az TwiML utasításokat hogyan toohandle hello kimenő hívása után van csatlakoztatva.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>VoIP képességek beágyazása a felhasználói felület kódot (JavaScript, iOS vagy Android)
Twilio biztosít egy ügyféloldali SDK, amely bármely asztali böngészőben, iOS-alkalmazás vagy Android-alkalmazás lehet kikapcsolni, a VoIP-telefont. Ebben a cikkben tárgyaljuk hogyan toouse VoIP hello böngészőben hívja. A hozzáadása toohello *Twilio JavaScript SDK* hello böngésző fut, a kiszolgálóoldali alkalmazás (a node.js-alkalmazás) használt tooissue egy "funkció token" toohello JavaScript ügyfél kell lennie. További VoIP használata node.js [hello Twilio fejlesztői blog][voipnode].

<a id="signup"/>

## <a name="sign-up-for-twilio-microsoft-discount"></a>Regisztrálás a Twilio (a Microsoft kedvezményes)
Twilio-szolgáltatások használatához először [egy fiók][signup]. Microsoft Azure-ügyfelek kap egy különös kedvezményes - [kell itt meg arról, hogy toosign][signup]!

<a id="azuresite"/>

## <a name="create-and-deploy-a-nodejs-azure-website"></a>Létrehozhat és telepíthet egy node.js Azure-webhely
Ezt követően kell toocreate egy Azure-on futó node.js webhelyet. [hello dokumentációs ehhez a következő helyen található][azure_new_site]. Magas szinten akkor lesz aki hello következő:

* Regisztrál az Azure-fiók esetén, ha még nem rendelkezik már
* Hello Azure felügyeleti konzol toocreate új webhely használatával
* Forrás-vezérlés támogatásának (indulunk követte git) hozzáadása
* A fájl létrehozásakor a következő `server.js` az egyszerű node.js-webalkalmazás
* Ez egyszerű alkalmazás tooAzure telepítése

<a id="twiliomodule"/>

## <a name="configure-hello-twilio-module"></a>Hello Twilio-modul konfigurálása
A következő lesz az első lépések toowrite hello Twilio API használatát egy egyszerű node.js-alkalmazás, amely lehetővé teszi. Az első lépések, igazolnia kell tooconfigure a Twilio-fiók hitelesítő adatait.

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Rendszerszintű környezeti változókat a Twilio-hitelesítő adatok konfigurálása
A sorrend hitelesített toomake kéréseket a meghatározott hello Twilio-háttér igazolnia kell a fiók SID azonosítója és a hitelesítési jogkivonat, mely funkciót hello felhasználónevet és jelszót a Twilio-fiókjában. hello legbiztonságosabb módja tooconfigure ezek hello csomópont modul az Azure-ban való használatra van keresztül rendszerszintű környezeti változókat, amelyek közvetlenül hello Azure felügyeleti konzolon állíthatja be.

Jelölje ki a node.js-webhelyet, és hello "Beállítása" hivatkozásra.  Ha görgessen lefelé kissé a folyamatot, akkor jelenik meg egy olyan területre, amelyen meg a konfigurációs tulajdonságok az alkalmazáshoz.  Adja meg a Twilio-fiók hitelesítő adatait ([megtalálható-e a Twilio-konzol][twilio_console]) látható – győződjön meg arról, hogy tooname őket `TWILIO_ACCOUNT_SID` és `TWILIO_AUTH_TOKEN`, illetve:

![Az Azure felügyeleti konzol][azure-admin-console]

Miután konfigurálta a változókat, indítsa újra az alkalmazás hello Azure-konzolon.

### <a name="declaring-hello-twilio-module-in-packagejson"></a>Deklaráló package.json hello Twilio-modulja
A következő igazolnia kell a package.json toomanage toocreate a csomópont modul függőségek keresztül [npm]. Hello: azonos szinten, hello `server.js` hello létrehozott fájl *Azure/node.js* oktatóanyag, hozzon létre egy fájlt `package.json`.  Ez a fájl helye hello következő:

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

Ez azt deklarálja hello twilio-modul, egy függőségi, valamint a hello népszerű [webes keretrendszer Express] [ express] és hello EJS sablon motor.  Rendben most azt Mindennel - most kódírást!

<a id="makecall"/>

## <a name="make-an-outbound-call"></a>Egy kimenő hívást
Hozzon létre egy egyszerű űrlapot választjuk hívás tooa több helyezi el. Nyissa meg `server.js`, és írja be a kódját a következő hello. Vegye figyelembe, ha "CHANGE_ME" - hello a azure webhely neve nincs put felirat látható:

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

Ezután hozzon létre egy könyvtárat `views` - belül ebben a könyvtárban, hozzon létre egy fájlt `index.ejs` a tartalom a következő hello:

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

Most a webhely tooAzure telepítése, és nyissa meg az új kezdőlapja. Legyen képes tooenter hello szövegmezőben a telefonszámát, és hívás fogadása a Twilio-számot!

<a id="sendmessage"/>

## <a name="send-an-sms-message"></a>SMS küldése
Most állítsa be a felhasználói felület és az űrlap logika toosend kezelése szöveges üzenetet. Nyissa meg `server.js`, és adja hozzá a kód túl hello utolsó hívása után következő hello`app.post`:

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

A `views/index.ejs`, adja hozzá egy másik űrlapon hello először egy toosubmit a szám és a szöveges üzenet:

```html
<form action="/sms" method="POST">
  <input placeholder="Enter a phone number" name="number"/>
  <br/>
  <input placeholder="Enter a message toosend" name="message"/>
  <br/>
  <input type="submit" value="Send text toohello number above"/>
</form>
```

Hozza létre újra az alkalmazás tooAzure, és képes toosubmit alkotnak, és saját kezűleg (vagy bármely legközelebbi ismerősei) tartalmazó szöveges üzenetet küldjön kell!

<a id="nextsteps"/>

## <a name="next-steps"></a>Következő lépések
Most megtanulta, node.js és Twilio toobuild alkalmazások kommunikáló hello alapjait. De ezek a példák alig ideiglenes hello felületet, hogy a Twilio és node.js lehetséges. Twilio használata node.js további információkért tekintse meg a következő erőforrások hello:

* [Hivatalos modul docs][docs]
* [Node.js-alkalmazások a VoIP-oktatóanyag][voipnode]
* [Votr - egy valós idejű SMS szavazás alkalmazás a node.js és CouchDB (három rész)][votr]
* [A node.js hello böngészőben pár programozás][pair]

Reméljük, node.js és Twilio támadásoknak Azure kedvelt!

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
