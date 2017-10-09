---
title: "Mobile Engagement webes SDK-integráció aaaAzure |} Microsoft Docs"
description: "hello a legújabb frissítésekről és Azure Mobile Engagement webes SDK hello eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 99613b68b615bec4ddcfcc8e4e0133ce9d887bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integrálása az Azure Mobile Engagement egy webalkalmazásban
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Ez a cikk hello eljárásokat mutatunk be, hello legegyszerűbb módja tooactivate hello elemzés és a monitorozási funkciók az Azure Mobile Engagement a webalkalmazásban.

Hello lépéseket tooactivate hello a jelentések, amelyek a szükséges toocompute kövesse a felhasználók, munkamenetek, tevékenységekhez, összeomlik és technicals összes statisztikája. Az alkalmazás-függő statisztikákat, pl.: eseményeket, hibákat és feladatokat aktiválnia kell a jelentések manuálisan hello Azure Mobile Engagement API használatával. További információt a további [hogyan toouse hello speciális API webalkalmazás szerinti címkézését a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Bevezetés
[Töltse le az Azure Mobile Engagement webes SDK hello](http://aka.ms/P7b453).
Mobile Engagement webes SDK-t egyetlen JavaScript-fájlként van kiadva hello, hogy rendelkezik azure-engagement.js tooinclude a hely vagy a webes alkalmazás egyes lapon.

> [!IMPORTANT]
> Ez a parancsfájl futtatása előtt kell parancsfájllal vagy, hogy az alkalmazás írása a Mobile Engagement tooconfigure részlet kód.
> 
> 

## <a name="browser-compatibility"></a>Böngészőkompatibilitás
Mobile Engagement webes SDK hello natív JSON kódolási és dekódolási, továbbá toocross-tartomány AJAX-kérelmek (hello W3C CORS specification hagyatkoznia) használja. A következő böngészőkben hello kompatibilis:

* A Microsoft Edge 12 +
* Az Internet Explorer 10 +
* Firefox 3.5 +
* Chrome 4 +
* Safari 6 +
* Opera 12 +

## <a name="configure-mobile-engagement"></a>Mobilmarketing konfigurálása
Írni egy parancsfájlt, amely létrehoz egy globális `azureEngagement` JavaScript-objektum, mint például a következő hello. A hely rendelkezhet olyan Többszörösök lapokat, mert ez a példa feltételezi, hogy ez a parancsfájl minden oldalon szerepel-e. Ebben a példában hello JavaScript objektum neve `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Hello `connectionString` értéket az alkalmazás megjelenik hello Azure-portálon.

> [!NOTE]
> `appVersionName`és `appVersionCode` megadása nem kötelező. Azt javasoljuk azonban, hogy konfigurálja azokat, hogy analytics képes a fájlverzió-információkat.
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>A Mobile Engagement parancsfájlok szerepeljenek a lapokon
Adja hozzá a Mobile Engagement parancsfájlok tooyour lapok hello a következő módszerek egyikét:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Vagy ezt:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias
Miután a Mobile Engagement webes SDK parancsfájl hello be van töltve, hello létrehoz **engagement** alias tooaccess hello SDK API-k. Az alias toodefine hello SDK konfigurációs nem használható. Ez az alias lesz a jelen dokumentációban lévő hivatkozás.

Vegye figyelembe, hogy ha hello alapértelmezett alias ütközik egy másik globális változó a lapról, akkor újra meg lehet adni azt hello konfigurációban az alábbiak szerint előtt betölteni a Mobile Engagement webes SDK hello:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Alapvető jelentéskészítési
A Mobile Engagement alapvető jelentéskészítési munkamenet szintű statisztika, például a felhasználók, a munkamenetek, a tevékenységek és az összeomlások statisztikája ismerteti.

### <a name="session-tracking"></a>Munkamenet-követés
A Mobile Engagement munkamenet tevékenységek sorrendje oszlik, minden egyes névvel azonosíthatók.

Klasszikus webhelyen azt javasoljuk, hogy a hely minden oldalon egy másik tevékenység deklarálja. Egy webhelyre vagy webalkalmazásra alkalmazáshoz, mely hello az aktuális lap soha nem változik érdemes lehet tootrack hello tevékenységek kisebb méretű, például hello oldalán.

Vagy módon toostart, vagy módosítsa hello aktuális felhasználói tevékenység, hívás hello `engagement.agent.startActivity` függvény. Példa:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

hello a Mobile Engagement-kiszolgáló automatikusan munkamenetekkel hello alkalmazáslap bezárása után három percen belül befejeződik.

Azt is megteheti, hogy is munkamenet befejezése manuálisan meghívásával `engagement.agent.endActivity`. Hello aktuális felhasználói tevékenység too'Idle állít be. "  hello munkamenet véget ér 10 másodperc múlva, kivéve, ha egy új hívása túl`engagement.agent.startActivity` hello-munkamenet folytatása.

Konfigurálhatja hello 10 másodperces késleltetési hello globális bevonási objektum, a következőképpen:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end hello session as soon as endActivity is called

> [!NOTE]
> Nem használhat `engagement.agent.endActivity` a hello `onunload` visszahívási mert AJAX-hívások nem hajtható végre ezen a ponton.
> 
> 

## <a name="advanced-reporting"></a>Speciális jelentéskészítés
Nem kötelező Ha azt szeretné, tooreport alkalmazásspecifikus eseményeket, hibákat és feladatok, kell toouse hello Mobile Engagement API. Hello keresztül érhető el a Mobile Engagement API hello `engagement.agent` objektum.

Az összes speciális lehetőségeket a Mobile Engagement a Mobile Engagement API hello hello érheti el. hello API hello cikkben található részletes [hogyan toouse hello speciális API webalkalmazás szerinti címkézését a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-hello-urls-used-for-ajax-calls"></a>Az AJAX-hívások használt URL-címek hello testreszabása
Testre szabható URL-címeket, hogy hello Mobile Engagement webes SDK-t használ. Például tooredefine hello napló URL-címe (hello SDK végpont naplózás), hello konfigurációt lehet felülbírálni módja:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Ha az URL-címet adja vissza, amely kezdődik `/`, `//`, `http://`, vagy `https://`, hello alapértelmezett séma nem használatos. Alapértelmezés szerint hello `https://` sémát az URL-címeket szolgál. Ha azt szeretné, hogy toocustomize hello alapértelmezett séma, bírálja felül a hello konfigurálása ehhez hasonló:

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
