---
title: "aaaHow toouse hello SendGrid e-mail szolgáltatás (Node.js) |} Microsoft Docs"
description: "Megtudhatja, hogyan küldjön e-maileket hello SendGrid e-mail szolgáltatás az Azure-on. A Kódminták hello Node.js API használatával."
services: 
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: 
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: fd617b6aaa656e7b5dd51c51ebb0db1e848450f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-nodejs"></a>Hogyan tooSend E-mail használatával SendGrid Node.js-ből
Ez az útmutató ismerteti, hogyan tooperform leggyakoribb programozási feladatok a sendgrid e-mail szolgáltatás az Azure-on. hello minták hello Node.js API segítségével készül. hello tárgyalt forgatókönyvekben szerepel a **hozhat létre, e-mailek**, **e-mailek küldéséhez**, **mellékletek hozzáadása**, **szűrőkkel**, és **tulajdonságainak frissítése**. A SendGrid és e-mailek küldéséhez további információkért lásd: hello [lépések](#next-steps) szakasz.

## <a name="what-is-hello-sendgrid-email-service"></a>Mi az az üdvözlő E-mail szolgáltatás SendGrid?
SendGrid van egy [felhőalapú szolgáltatás] biztosít megbízható [tranzakciós e-mailben kézbesítésre], a méretezhetőség és a valós idejű elemzési rugalmas API-k, amelyek egyéni integrációs könnyedén együtt. Gyakori SendGrid használati forgatókönyvek a következők:

* Visszaigazolások toocustomers automatikus küldése
* Az ügyfelek havi e-szórólapok és a különleges ajánlatokkal terjesztési felügyelete listája
* Többek között a blokkolt e-mail, és az ügyfél reakcióidőt valós idejű metrikáját gyűjtése
* Toohelp azonosíthatja a trendeket jelentések létrehozása
* Ügyfél-lekérdezések továbbítása
* E-mail értesítések az alkalmazásról

További információkért lásd: [https://sendgrid.com](https://sendgrid.com).

## <a name="create-a-sendgrid-account"></a>A SendGrid-fiók létrehozása
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-nodejs-module"></a>Hivatkozás hello SendGrid Node.js modul
hello SendGrid modul a Node.js hello csomópont Csomagkezelő (npm) keresztül telepíthető hello a következő parancs használatával:

    npm install sendgrid

A telepítést követően a kód a következő hello segítségével megkövetelheti hello modul az alkalmazásban:

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

hello SendGrid modul exportálja hello **SendGrid** és **E-mail** funkciók.
**SendGrid** felelős a webes API-n keresztül e-mail elküldése közben **E-mail** foglalja az e-mailt.

## <a name="how-to-create-an-email"></a>Hogyan: hozzon létre egy e-mailt
Hello SendGrid modullal e-mailt létrehozása magában foglalja először e-mailt hello E-mail funkcióval, és majd küldje el azt a hello SendGrid függvény használatával. hello az alábbiakban látható egy példa egy új üzenetet hello E-mail függvény használatával hoz létre:

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

HTML üzenet hello html tulajdonság beállításával támogató ügyfelek is megadható. Példa:

    html: This is a sample <b>HTML<b> email message.

Ügyfelek, amelyek nem támogatják a HTML-üzenetek mindkét hello szöveg és a html-tulajdonságok beállítása sikeres-e tartalékként történő szöveges tartalom biztosít.

Az összes tulajdonság hello E-mail függvény által támogatott további információkért lásd: [sendgrid-nodejs][sendgrid-nodejs].

## <a name="how-to-send-an-email"></a>Útmutató: az e-mailek küldése
Miután létrehozta az üdvözlő E-mail függvény használatával e-mailt, elküldheti azt hello SendGrid által biztosított webes API használatával. 

### <a name="web-api"></a>Webes API
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> Amíg a fenti példák hello sikeres megjelenítése egy e-mailek objektum és a visszahívás függvényben, e-mailes tulajdonságai közvetlenül megadásával közvetlenül is hívhat meg hello küldési függvény. Példa:  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a>Hogyan: hozzá mellékletet
Mellékletek felveheti tooa üzenet hello hello fájl neve és elérési útjait megadásával **fájlok** tulajdonság. hello a következő példa bemutatja a melléklet küldése:

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used toospecify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> Hello használatakor **fájlok** tulajdonság, hello fájl elérhetőnek kell lennie keresztül [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile). Ha tooattach kívánja hello fájl az Azure Storage, például egy Blob tároló kell másolnia hello toolocal fájltároló vagy tooan Azure meghajtó hello segítségével mellékletként küldése előtt **fájlok** tulajdonság.
> 
> 

## <a name="how-to-use-filters-tooenable-footers-and-tracking"></a>Hogyan: szűrési tooEnable láblécek és nyomon követése
SendGrid szűrők hello használata további e-mail-funkciókat biztosítja. Ezek a beállítások tooan e-mailt ahhoz, hogy bizonyos funkciók, például kattintson nyomon követése, Google analytics, nyomon követése, előfizetés adható hozzá és így tovább. Szűrők teljes listáját lásd: [szűrőbeállítások][Filter Settings].

Szűrők hello segítségével lehet alkalmazott tooa üzenet **szűrők** tulajdonság.
Minden egyes szűrő szűrő-specifikus beállításokat tartalmazó kivonat szerint van megadva.
hello alábbi példák bemutatják, hello élőláb, majd kattintson szűrők nyomon követése:

### <a name="footer"></a>Élőláb
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a>Kattintson a nyomon követése
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a>Útmutató: E-mail tulajdonságainak frissítése
Néhány e-mail tulajdonság használatával felülírható  **beállítása*tulajdonság***, illetve használatával  **hozzáadása*tulajdonság***. Például hozzáadhat további címzetteket használatával

    email.addTo('jeff@contoso.com');

vagy állítsa be a szűrő használatával

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

További információkért lásd: [sendgrid-nodejs][sendgrid-nodejs].

## <a name="how-to-use-additional-sendgrid-services"></a>Útmutató: további SendGrid szolgáltatásokkal
SendGrid kínál webes API-k, hogy tooleverage további SendGrid funkciót használhatja az Azure alkalmazásból. Teljes további információkért lásd: hello [SendGrid API-JÁNAK dokumentációja][SendGrid API documentation].

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a SendGrid E-mail szolgáltatás hello hello alapjait, kövesse a további hivatkozások toolearn.

* SendGrid Node.js modul tárház: [sendgrid-nodejs][sendgrid-nodejs]
* SendGrid API dokumentációjának: <https://sendgrid.com/docs>
* SendGrid különleges ajánlat Azure ügyfeleknek: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[felhőalapú szolgáltatás]: https://sendgrid.com/email-solutions
[tranzakciós e-mailben kézbesítésre]: https://sendgrid.com/transactional-email
