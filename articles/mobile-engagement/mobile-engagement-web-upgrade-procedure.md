---
title: "Mobile Engagement webes SDK frissítési eljárásait aaaAzure |} Microsoft Docs"
description: "Azure Mobile Engagement hello legújabb frissítésekről és hello webes SDK eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a20529b4-ec8d-4503-8ae9-09b5f0846d5b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 06/07/2016
ms.author: piyushjo
ms.openlocfilehash: a2df65904c6b56584ce6588ed26a9b79f3aa27ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-web-sdk-upgrade-procedures"></a>Az Azure Mobile Engagement webes SDK frissítési eljárások
Ha hello Azure Mobile Engagement webszolgáltatási SDK egy korábbi verziója már a webes alkalmazás van integrálva, akkor tooconsider hello hello SDK frissítéskor a következő pontok.

Ha több hello Mobile Engagement SDK-t webes verzió kihagyott, akkor toocomplete számos eljárást hello frissítési folyamat során. Például, ha telepít át 1.4.0 too1.6.0, első kövesse hello eljárások tooupgrade a 1.4.0 too1.5.0. Ezután kövesse a 1.5.0 hello eljárások tooupgrade too1.6.0.

Bármelyik verzióra frissít, bármely korábbi verziója hello fájl azure-engagement.js cserélje hello hello fájl legújabb verzióját.

## <a name="upgrade-from-121-too200"></a>Frissítés a 1.2.1-es too2.0.0
Ez a szakasz ismerteti, hogyan toomigrate egy Mobile Engagement webes SDK-integráció hello Capptain szolgáltatásból által kínált Capptain SAS tooan Azure Mobile Engagement-alkalmazást. Ha az áttelepítés egy korábbi adjon tájékoztatást hello Capptain webhely toofirst too1.2.1 át, és ezután alkalmazza az alábbi eljárásokat hello.

Mobile Engagement webes SDK hello ezen verziója nem támogatja a Samsung intelligens TV, Opera TV, webOS vagy hello Reach szolgáltatás.

> [!IMPORTANT]
> Capptain és Azure Mobile Engagement vannak nem hello ugyanazt a szolgáltatást. a következő eljárás hello mutatja be, hogyan csak toomigrate hello ügyfélalkalmazás. Áttelepítése a Mobile Engagement webes SDK hello alkalmazásban hello rendszer nem telepíti át az adatokat egy Capptain server tooa a Mobile Engagement kiszolgálóról.
> 
> 

### <a name="javascript-files"></a>JavaScript-fájlok
Csere hello fájl capptain-sdk.js hello az azure-engagement.js fájlt, és ennek megfelelően módosítsa a parancsfájl importálja.

### <a name="remove-capptain-reach"></a>Capptain Reach eltávolítása
Mobile Engagement webes SDK hello ezen verziója nem támogatja a hello Reach szolgáltatás. Az alkalmazás Capptain Reach integrálva, ha szüksége van-e tooremove azt.

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

### <a name="remove-deprecated-apis"></a>Távolítsa el az elavult API-k
Néhány API-kat Capptain a Mobile Engagement webes SDK hello elavultak.

Távolítsa el a következő API-k hívások toohello: `agent.connect`, `agent.disconnect`, `agent.pause`, és `agent.sendMessageToDevice`.

Távolítsa el a következő visszahívások a Capptain konfigurációból hello minden példányát: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, és `onPushMessageReceived`.

### <a name="configuration"></a>Konfiguráció
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

### <a name="javascript-apis"></a>JavaScript API-k
hello globális JavaScript objektum `window.capptain` át lett nevezve `window.azureEngagement` hello is használhat, de `window.engagement` alias az API-hívásokhoz. Hello alias toodefine hello SDK konfigurációja nem használható.

Például `capptain.deviceId` válik `engagement.deviceId`, `capptain.agent.startActivity` válik `engagement.agent.startActivity`, és így tovább.

