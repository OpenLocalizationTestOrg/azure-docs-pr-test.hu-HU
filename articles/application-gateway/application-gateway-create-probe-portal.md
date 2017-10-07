---
title: "egyéni aaaCreate mintavételi - Azure Application Gateway - Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egyéni mintavételi az Alkalmazásátjáró hello portál használatával"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: 9e9309045ef33ba1010868783949b4fde31619ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-probe-for-application-gateway-by-using-hello-portal"></a>Hozzon létre egy egyéni mintavétel az Alkalmazásátjáró hello portál használatával

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Klasszikus Azure PowerShell](application-gateway-create-probe-classic-ps.md)

Ebben a cikkben hozzáadhat egy egyéni mintavételi tooan meglévő Alkalmazásátjáró hello Azure-portálon keresztül. Egyéni mintavételt hasznosak, az alkalmazásokat, amelyek egy adott állapotának ellenőrzése lapon vagy az alkalmazásokat, amelyek nem a sikeres válasz hello alapértelmezett webalkalmazáshoz.

## <a name="before-you-begin"></a>Előkészületek

Ha még nem rendelkezik olyan átjárót, látogasson el [Alkalmazásátjáró létrehozása](application-gateway-create-gateway-portal.md) toocreate egy alkalmazás átjáró toowork együtt.

## <a name="createprobe"></a>Hello mintavétel létrehozása

Mintavételt egy kétlépéses folyamat hello portálon keresztül lehet konfigurálni. első lépés hello toocreate hello mintavételi. Hello a második lépésben hello mintavételi toohello backendhttpsettings osztályhoz hello Alkalmazásátjáró adja hozzá.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). Ha már nincs fiókja, regisztrálhat az egy [ingyenes egy hónapos próbaverzió](https://azure.microsoft.com/free)

1. A hello Azure-portálon Kedvencek ablak kattintson az összes erőforrást. Kattintson a hello hello Alkalmazásátjáró összes erőforrás panel. Hello előfizetés már kiválasztott több erőforrást tartalmaz, a név szerint a szűrőt hello partners.contoso.net írhatja... mezőbe tooeasily hozzáférés hello Alkalmazásátjáró.

1. Kattintson a **vizsgálat** hello kattintson **Hozzáadás** gomb tooadd vizsgálatok.

  ![Adja hozzá a mintavételi panel tölti ki adatokkal][1]

1. A hello **Hozzáadás állapotmintáihoz** panelen hello mintavétel hello szükséges adatok, és kattintson teljes **OK**.

  |**Beállítás** | **Érték** | **Részletek**|
  |---|---|---|
  |**Name (Név)**|customProbe|Ez az érték egy rövid nevet toohello mintavételt hello portálon elérhető.|
  |**Protocol (Protokoll)**|HTTP vagy HTTPS | hello állapotmintáihoz hello protokollt használ.|
  |**Állomás**|Egytényezős contoso.com|Ez az érték hello mintavételhez használt hello állomás nevét. Alkalmazandó csak akkor, ha több hely van beállítva az alkalmazás-átjárón, ellenkező esetben használja a "127.0.0.1". Ez az érték eltér hello virtuális gép állomásnevét.|
  |**Elérési út**|/ vagy egy másik elérési utat|hello teljes URL-címet hello egyéni mintavétel hello maradéka. Érvényes elérési utat kezdődik "/". Hello alapértelmezett elérési útja pedig csak http://contoso.com használja a "/" |
  |**Időtartam (másodperc)**|30|Milyen gyakran hello mintavételi fut az állapotfigyelő toocheck. Nem ajánlott tooset hello alacsonyabb, mint 30 másodperc.|
  |**Időkorlátja (másodperc)**|30|hello mennyi idő hello mintavétel időkorlátját vár. hello időtúllépési időköz igények toobe elég nagy, hogy egy http lehet hívni tooensure hello háttér health vonatkozó lapján érhető el.|
  |**Sérült küszöbérték**|3|A sikertelen kísérletek toobe megfelelő állapotúnak számít száma. 0, akkor a küszöbérték, ha a rendszerállapot-ellenőrzés sikertelen hello háttér-határozza meg. a nem megfelelő azonnal.|

  > [!IMPORTANT]
  > hello állomásnév nem hello ugyanaz, mint a kiszolgáló nevét. Ez az érték hello hello alkalmazás kiszolgálón futó virtuális állomás hello neve. hello mintavételi küldött toohttp: / /(host name):(port from httpsetting)/urlPath

## <a name="add-probe-toohello-gateway"></a>Mintavételi toohello átjáró hozzáadása

Most, hogy hello mintavétel létrehozása után a rendszer idő tooadd azt toohello átjáró. Mintavételi beállítást a hello backendhttpsettings osztályhoz hello Alkalmazásátjáró.

1. Kattintson a **HTTP-beállítások** hello Alkalmazásátjáró, toobring mentése hello konfigurációs panelen kattintson hello aktuális háttér http-beállítások hello ablakban.

  ![HTTPS-beállítások ablak][2]

1. A hello **appGatewayBackEndHttpSettings** beállítások panelen, a jelölőnégyzet hello **használata egyéni tesztműveleti** jelölőnégyzetet, és válassza ki a létrehozott hello hello mintavételi [létrehozás hello mintavételi](#createprobe) szakasz a hello **egyéni mintavételi** legördülő...
Amikor végzett, kattintson **mentése** és hello-beállítások alkalmazása.

hello alapértelmezett mintavétel hello alapértelmezett hozzáférési toohello webalkalmazás ellenőrzi. Most, hogy egy egyéni mintavétel létrehozása után hello Alkalmazásátjáró hello definiált egyéni elérési út toomonitor állapotfigyelő hello háttérkiszolgálók használ. Definiált hello feltétel alapján, hello Alkalmazásátjáró hello elérési út van megadva a hello mintavételi ellenőrzi. Ha hello hívás toohost:Port / elérési út nem ad vissza egy HTTP 200-399 állapotválasz, hello server szükséges kívül Elforgatás hello sérült küszöbérték elérése után. A nem megfelelő állapotú példány toodetermine hello probing folytatódik, amikor a megfelelő válik újra. Hello példány hátsó toohealthy kiszolgálókészlethez hozzáadott, a forgalom kezdődik újra szereplő tooit és a vizsgálathoz használt toohello példány továbbra is fennáll, mint a normál felhasználói megadott időközönként.

## <a name="next-steps"></a>Következő lépések

Hogyan SSL kiszervezésével az Azure Application Gateway, tooconfigure: toolearn [SSL-kiszervezés konfigurálása](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png

