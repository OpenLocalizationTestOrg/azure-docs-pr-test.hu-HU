---
title: "A MQ connector használata az Azure Logic Apps |} Microsoft Docs"
description: "Csatlakozás egy helyszíni vagy a Tallózás, fogadására és üzenetek küldése WebSphere MQ a logic app munkafolyamat Azure MQ server"
services: logic-apps
author: valthom
manager: anneta
documentationcenter: 
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 06/01/2017
ms.author: valthom; ladocs
ms.openlocfilehash: 17c651585b56dae186286f5d8c68c363ae9c524d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="connect-to-an-ibm-mq-server-from-logic-apps-using-the-mq-connector"></a>A logic Apps alkalmazásokból az MQ-összekötővel IBM MQ kiszolgálóhoz kapcsolódni 

Microsoft Connector MQ küld, és beolvassa a MQ Server helyszíni, vagy az Azure-ban tárolja. Ez az összekötő tartalmazza a Microsoft MQ-ügyfél, amely a TCP/IP-hálózaton keresztül egy távoli IBM MQ-kiszolgálóval kommunikál. Ez a dokumentum egy alapszintű útmutató a MQ-összekötő használatára. Azt javasoljuk, hogy először keresse meg a várólista egyetlen üzenet, és az egyéb műveletek majd közben.    

A MQ az összekötő tartalmazza a következő műveleteket. Nincsenek nincsenek eseményindítók.

-   Egyetlen üzenet Tallózás az IBM MQ kiszolgálóról az üzenet törlése nélkül
-   Keresse meg az üzenetkötegek anélkül, hogy az üzenetek az IBM MQ Server való törlésekor
-   Egy üzenetet kap, és az üzenet törlése az IBM MQ Server kiszolgálóhoz
-   Az üzenetkötegek kap, és törölje az üzenetek az IBM MQ Server kiszolgálóhoz
-   Egyetlen üzenet küldése az IBM MQ Server kiszolgálóhoz 

## <a name="prerequisites"></a>Előfeltételek

* Ha helyszíni MQ kiszolgálót, [az helyszíni átjáró telepítése](../logic-apps/logic-apps-gateway-install.md) a hálózaton belül a kiszolgálón. Ha a MQ Server nyilvánosan elérhető, vagy Azure-ban érhető el, majd az átjáró nem használt vagy szükséges.

    > [!NOTE]
    > A kiszolgáló, amelyen telepítve van-e az On-Premises Data Gateway is rendelkeznie kell a .net keretrendszer 4.6-os függvény MQ-összekötő telepítve.

* A helyszíni adatok Gateway - Azure-erőforrás létrehozása [az átjáró-kapcsolat beállítása](../logic-apps/logic-apps-gateway-connection.md).

* Hivatalos támogatott IBM WebSphere MQ-verziók:
   * 7.5 MQ
   * MQ 8.0

## <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása

1. Az a **Azure start Bizottsága**, jelölje be  **+**  (plusz jelre), **Web + mobil**, majd **logikai alkalmazás**. 
2. Adja meg a **neve**, például MQTestApp, **előfizetés**, **erőforráscsoport**, és **hely** (használhatja azt a helyet ahol a helyszíni Data Gateway kapcsolat van konfigurálva). Válassza ki **rögzítés az irányítópulton**, és válassza ki **létrehozása**.  
![Logikai alkalmazás létrehozása](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Egy eseményindító hozzáadása

> [!NOTE]
> Az MQ-összekötő nem rendelkezik a eseményindítókat. Igen, a másik eseményindító segítségével indítsa el a logikai alkalmazás, például a **ismétlődési** eseményindító. 

1. A **Logic Apps Designer** megnyílik, jelölje be **ismétlődési** közös eseményindítók listájában.
2. Válassza ki **szerkesztése** belül az ismétlődési eseményindító. 
3. Állítsa be a **gyakoriság** való **nap**, és állítsa be a **időköz** való **7**. 

## <a name="browse-a-single-message"></a>Egyetlen üzenet tallózása
1. Válassza ki **+ új lépés**, és válassza ki **művelet hozzáadása**.
2. Írja be a keresőmezőbe, `mq`, majd válassza ki **MQ - Tallózás üzenet**.  
![Keresse meg az üzenet](media/connectors-create-api-mq/Browse_message.png)

3. Ha nincs meglévő kapcsolat MQ, majd hozza létre a kapcsolat:  

    1. Válassza ki **keresztül, a helyszíni adatátjáró**, és írja be a MQ-kiszolgáló tulajdonságait.  
    A **Server**, adja meg a MQ-kiszolgáló nevét, vagy adja meg az IP-cím, egy kettőspontot és a port számát. 
    2. A **átjáró** legördülő lista ismerteti a meglévő átjáró kapcsolatokat, amelyek vannak konfigurálva. Válassza ki az átjárót.
    3. Válassza ki **létrehozása** befejezésekor. A kapcsolat az alábbihoz hasonlít:   
    ![Kapcsolat tulajdonságai](media/connectors-create-api-mq/Connection_Properties.png)

4. A művelet tulajdonságait, a következőket teheti:  

    * Használja a **várólista** tulajdonság egy másik várólista neve, mint a kapcsolat meghatározott eléréséhez
    * Használja a **MessageId**, **CorrelationId**, **GroupId**, és egyéb tulajdonságok alapján különböző MQ üzenet üzenet tallózással
    * Állítsa be **IncludeInfo** való **igaz** azzal az információval a kimenet további üzenet. Vagy állítsa az értékét **hamis** nem tartalmazza a kimenet további információt.
    * Adjon meg egy **időtúllépés** elemnél várakozási érkezésére üres várólistában lévő üzenetek mennyi ideig. Ha nem ad meg, a várólista első üzenetébe lekérésére, és nincs várakozás egy hibaüzenet jelenik meg az eltelt idő.  
    ![Keresse meg az üzenet tulajdonságai](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Mentés** a módosításokat, majd **futtatása** a Logic Apps alkalmazást:  
![Mentse és futtatása](media/connectors-create-api-mq/Save_Run.png)

6. Néhány másodperc múlva Futtatás lépéseket is látható, és megnézheti a kimenetet. Válassza ki az egyes lépések részletes zöld pipa. Válassza ki **tekintse meg a nyers kimenetek** kattintva további információt olvashat a kimeneti adatok.  
![Keresse meg a kimeneti üzenet](media/connectors-create-api-mq/Browse_message_output.png)  

    Nyers kimenete:  
    ![Keresse meg a nyers kimeneti üzenet](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Ha a **IncludeInfo** beállítás értéke igaz, a következő eredmény jelenik meg:  
![Tallózás hibaüzenet tartalmazza adatai](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Keresse meg a több üzenetet
A **keresse meg az üzenetek** művelet tartalmaz egy **BatchSize** beállítást annak jelzésére, hogy hány üzenetek vissza kell adni az üzenetsorból.  Ha **BatchSize** nem tartozik bejegyzés, a rendszer visszairányítja az összes üzenet. A visszaadott eredménye üzenetek tömbjét.

1. Hozzáadásakor a **keresse meg az üzenetek** művelet, az első csatlakozás konfigurált alapértelmezettként van beállítva. Válassza ki **kapcsolat módosítása** hozzon létre egy új kapcsolatot, vagy válasszon egy másik kapcsolat.

2. Tallózással keresse meg a kimeneti üzenetek megjelenítése:  
![Keresse meg az üzenetek kimeneti](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Egyetlen üzenet
A **Receive üzenet** művelet ugyanazokkal a be van, és kiírja, mint a **Tallózás üzenet** művelet. Használata esetén **Receive üzenet**, az üzenet törlődik az üzenetsorból.

## <a name="receive-multiple-messages"></a>Több üzenetet
A **üzeneteket fogadni** művelet ugyanazokkal a be van, és kiírja, mint a **keresse meg az üzenetek** művelet. Használata esetén **üzeneteket fogadni**, az üzenetek törlődnek a várólistából.

Ha nincsenek üzenetek a várólista a Tallózás gombra vagy a fogadás során, a lépés sikertelen lesz, a következő eredménnyel:  
![MQ nem üzenet hiba](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>Üzenet küldése
1. Hozzáadásakor a **üzenet küldése** művelet, az első csatlakozás konfigurált alapértelmezettként van beállítva. Válassza ki **kapcsolat módosítása** hozzon létre egy új kapcsolatot, vagy válasszon egy másik kapcsolat. Az érvényes **üzenettípusok** vannak **Datagram**, **válasz**, vagy **kérelem**.  
![Üzenet tulajdonságai küldése](media/connectors-create-api-mq/Send_Msg_Props.png)

2. Üzenet küldése kimenete a következőképpen néznek:  
![Üzenet kimenetként](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és a swagger definiált műveletek megtekintése, és semmilyen határnak a Lásd még: a [connector részleteket](/connectors/mq/).

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Az egyéb rendelkezésre álló összekötők Logic Apps, megismerkedhet a [API-k lista](apis-list.md).
