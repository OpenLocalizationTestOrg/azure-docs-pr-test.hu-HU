---
title: "Mobile Engagement webes SDK – áttekintés aaaAzure |} Microsoft Docs"
description: "Azure Mobile Engagement hello legújabb frissítésekről és hello webes SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
ms.openlocfilehash: 9e60a232b5eb2c41c405041a88e09d7137563513
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk"></a>Az Azure Mobile Engagement webes SDK-t
Az összes hello részleteit itt Start toointegrate Azure Mobile Engagement egy webalkalmazásban. Ha azt szeretné, hogy toogive azt egy try saját webes alkalmazáshoz, az első lépések előtt tekintse meg a [15 perces oktatóanyag](mobile-engagement-web-app-get-started.md).

## <a name="integration-procedures"></a>Integráció eljárások
1. Ismerje meg, [módját a webalkalmazás a Mobile Engagement toointegrate](mobile-engagement-web-integrate-engagement.md).
2. Címke terv végrehajtásához további [hogyan toouse hello speciális API szerinti címkézését a webalkalmazás a Mobile Engagement](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Kibocsátási megjegyzések
### <a name="202-10182016"></a>2.0.2 (10/18/2016)
* Rögzített összeomlási a privát böngészés (Safari).
* Rögzített összeomlási a böngésző cookie-k le van tiltva.

Az összes verzió, lásd: hello [végezze el a kibocsátási megjegyzések](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Frissítési eljárások
### <a name="upgrade-from-121-too200"></a>Frissítés a 1.2.1-es too2.0.0
hello következő részek a hogyan toomigrate egy Mobile Engagement webes SDK-integráció hello Capptain szolgáltatásból által kínált Capptain SAS tooan Azure Mobile Engagement-alkalmazást. Ha telepít át a korábbi 1.2.1-es, mint tekintse át először hello Capptain webhely toomigrate too1.2.1, és ezután alkalmazza az alábbi eljárásokat hello.

Mobile Engagement webes SDK hello ezen verziója nem támogatja a Samsung intelligens TV, Opera TV, webOS vagy hello Reach szolgáltatás.

> [!IMPORTANT]
> Capptain és Azure Mobile Engagement nem hello ugyanazt a szolgáltatást, és a következő eljárások hello jelölje ki, hogyan csak toomigrate hello ügyfélalkalmazás. Áttelepítése a Mobile Engagement webes SDK hello alkalmazásban hello rendszer nem telepíti át az adatokat egy Capptain server tooa a Mobile Engagement kiszolgálóról.
> 
> 

#### <a name="javascript-files"></a>JavaScript-fájlok
Csere hello fájl capptain-sdk.js hello az azure-engagement.js fájlt, és ennek megfelelően módosítsa a parancsfájl importálja.

#### <a name="remove-capptain-reach"></a>Capptain Reach eltávolítása
Mobile Engagement webes SDK hello ezen verziója nem támogatja a hello Reach szolgáltatás. Ha az alkalmazás rendelkezik Capptain Reach integrálva, tooremove kell azt.

Távolítsa el a hello elérni CSS importálása a lapról, és törölje a hello kapcsolódó .css-fájljában (capptain-reach.css, alapértelmezés szerint).

Törölje a következő Reach erőforrások hello: hello Bezárás kép (capptain-close.png, alapértelmezés szerint) és hello márka ikon (capptain-értesítés-ikonra, alapértelmezés szerint).

Távolítsa el a hello UI elérni az alkalmazásbeli értesítések. hello alapértelmezett elrendezési így néz ki:

    <!-- capptain notification -->
    <div id="capptain_notification_area" class="capptain_category_default">
      <div class="icon">
        <img src="capptain-notification-icon.png" alt="icon" />
      </div>
      <div class="content">
        <div class="title" id="capptain_notification_title"></div>
        <div class="message" id="capptain_notification_message"></div>
      </div>
      <div id="capptain_notification_image"></div>
      <div>
        <button id="capptain_notification_close">Close</button>
      </div>
    </div>

Távolítsa el a hello UI elérni a szöveg és a webes hirdetmények és szavazások esetén. hello alapértelmezett elrendezési így néz ki:

    <div id="capptain_overlay" class="capptain_category_default">
      <button id="capptain_overlay_close">x</button>
      <div id="capptain_overlay_title"></div>
      <div id="capptain_overlay_body"></div>
      <div id="capptain_overlay_poll"></div>
      <div id="capptain_overlay_buttons">
        <button id="capptain_overlay_exit"></button>
        <button id="capptain_overlay_action"></button>
      </div>
    </div>

Távolítsa el a hello `reach` a konfigurációját, ha van ilyen objektum. Néz ki:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Távolítsa el bármilyen egyéb Reach testreszabása: például kategóriák.

#### <a name="remove-deprecated-apis"></a>Távolítsa el az elavult API-k
Néhány API-kat Capptain a Mobile Engagement webes SDK hello elavultak.

Távolítsa el a következő API-k hívások toohello: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.

A következő visszahívások a Capptain konfigurációból hello törlését: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguráció
A Mobile Engagement egy kapcsolati karakterlánc tooconfigure SDK azonosítók, például hello azonosítót használja.

Hello Alkalmazásazonosító cserélje le a kapcsolati karakterláncot. Vegye figyelembe, hogy hello globális objektum hello SDK konfigurációs módosul a `capptain` túl`azureEngagement`.

Mielőtt áttelepítése:

    window.capptain = {
      appId: ...,
      [...]
    };

: Az áttelepítés után

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      [...]
    };

hello kapcsolati karakterlánc az alkalmazás hello Azure-portálon jelenik meg.

#### <a name="javascript-apis"></a>JavaScript API-k
hello globális JavaScript objektum `window.capptain` át lett nevezve `window.azureEngagement`, de használhat hello `window.engagement` alias az API-hívásokhoz. Az alias toodefine hello SDK konfigurációs nem használható.

Például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`, és így tovább.

Ha egy korábbi hello Azure Mobile Engagement webes SDK már a az alkalmazásba integrált része, kérjük, olvassa el kapcsolatos [eljárások frissítése](mobile-engagement-web-upgrade-procedure.md).

