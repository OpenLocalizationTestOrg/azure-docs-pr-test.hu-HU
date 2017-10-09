---
title: "a Web Apps aaaGet elindítva, az Azure Mobile Engagement |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések a Web Apps."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
ms.openlocfilehash: a84c96cac13bf3b85e72aef55da5c91693e1766c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Ismerkedés az Azure Mobile Engagement Web webalkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand a webes alkalmazások használatáról.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Ez az oktatóanyag hello következő szükséges:

* Visual Studio 2015 vagy bármely másik választott szerkesztő
* [Web SDK](http://aka.ms/P7b453)

A webszolgáltatási SDK jelenleg előzetes verzióban érhető és csak hello pillanatban támogatja az elemzés és még nem támogatja a küldő böngésző vagy alkalmazásbeli leküldéses értesítéseket. 

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>A Mobile Engagement beállítása a webalkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt," hello minimálisan szükséges toocollect adatok pedig mutat.

Létre fogunk hozni egy alapszintű webalkalmazást a Visual Studio toodemonstrate hello integrációja, ha a Visual Studio kívül is létre webes alkalmazásokkal együtt hello lépések végrehajtásával. 

### <a name="create-a-new-web-app"></a>Új webalkalmazás létrehozása
hello következő lépések azt feltételezik hello Visual Studio 2015 használatát, ha hello lépések hasonlóak a Visual Studio korábbi verzióiban. 

1. Indítsa el a Visual Studio és a hello **Home** képernyőn válassza ki **új projekt**.
2. Hello előugró ablakban válassza **webes** -> **ASP.Net Web Application**. Töltse ki a hello app **neve**, **hely** és **megoldás neve**, és kattintson a **OK**.
3. A hello **válasszon olyan sablont,** előugró ablakban válasszon **üres** alatt **ASP.Net 4.5 sablonok** kattintson **OK**. 

Most létrehozott egy új üres webalkalmazás projekt amelybe integrálni fogjuk a hello Azure Mobile Engagement webes SDK-t.

### <a name="connect-your-app-toomobile-engagement-backend"></a>Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez
1. Hozzon létre egy új nevű **javascript** a megoldásban, és adja hozzá a hello webes SDK JS fájl **azure-engagement.js** bele. 
2. Adjon hozzá egy új fájlt nevű **main.js** hello a következő kódot a javascript mappában. Győződjön meg arról, hogy tooupdate hello kapcsolati karakterláncot. Ez `azureEngagement` objektum használt tooaccess webes SDK módszerek lesz. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio .js fájlokkal][1]

## <a name="enable-real-time-monitoring"></a>Valós idejű figyelés engedélyezése
Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy tevékenység toohello. A webalkalmazás kontextusában hello tevékenység egy olyan weblap. 

1. Hozzon létre egy új lapot nevű **home.html** a megoldás és a webalkalmazás azt hello kiindulási lapon. 
2. Hello két JavaScript-kódok hello következő hello törzs címkén belül hozzáadásával a lap korábbi részében hozzáadott tartalmazza. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Hello törzs címke toocall EngagementAgent tartozó frissítése `startActivity` módszer
   
        <body onload="engagement.agent.startActivity('Home')">
4. A **home.html** lapnak így kell kinéznie
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a>Az elemzés kiterjesztése
Itt érhetők el minden hello módszerek jelenleg elemzéséhez használható webes SDK-val:

1. Tevékenységek/weblapok:
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. Események
   
        engagement.agent.sendEvent(name, extras);
3. Hibák
   
        engagement.agent.sendError(name, extras);
4. Feladatok
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

