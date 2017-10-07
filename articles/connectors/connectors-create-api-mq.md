---
title: "aaaLearn hogyan toouse hello Azure Logic Apps MQ-összekötőjét |} Microsoft Docs"
description: "A logic app munkafolyamat toobrowse tooan a helyszíni vagy Azure MQ Server kiszolgálóhoz kapcsolódó, fogadására és üzenetek tooWebSphere MQ küldése"
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
ms.openlocfilehash: 8b36d53b457ced1a7461c229aecfcf8e4ae668a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-ibm-mq-server-from-logic-apps-using-hello-mq-connector"></a>IBM MQ server tooan csatlakozzon a logic Apps alkalmazásokból hello MQ-összekötő segítségével 

Microsoft Connector MQ küld, és beolvassa a MQ Server helyszíni, vagy az Azure-ban tárolja. Ez az összekötő tartalmazza a Microsoft MQ-ügyfél, amely a TCP/IP-hálózaton keresztül egy távoli IBM MQ-kiszolgálóval kommunikál. Ez a dokumentum egy olyan alapszintű útmutató toouse hello MQ összekötő. Javasoljuk, hogy keresse meg a várólista egyetlen üzenet megkezdése és más műveletek majd próbált hello.    

hello MQ összekötő a következő műveletek hello tartalmazza. Nincsenek nincsenek eseményindítók.

-   Egyetlen üzenet Tallózás üdvözlőüzenetére eltávolítása IBM MQ Server hello nélkül
-   Keresse meg az üzenetkötegek köszönőüzenetei eltávolítása IBM MQ Server hello nélkül
-   Egy üzenetet kap, és törölni üdvözlőüzenetére hello IBM MQ Server kiszolgálóhoz
-   Az üzenetkötegek kap, és törölni köszönőüzenetei hello IBM MQ Server kiszolgálóhoz
-   Egy egyszeri üzenet toohello IBM MQ Server küldése 

## <a name="prerequisites"></a>Előfeltételek

* Ha egy helyszíni MQ server használatával [hello helyszíni adatátjáró telepítése](../logic-apps/logic-apps-gateway-install.md) egy kiszolgálón, a hálózaton belül. Ha hello MQ Server nyilvánosan elérhető, vagy Azure-ban érhető el, majd hello adatátjáró nem használt vagy szükséges.

    > [!NOTE]
    > hello kiszolgáló, ahol telepítve van a helyszíni adatátjáró hello is rendelkeznie kell a .net keretrendszer 4.6-os hello MQ összekötő toofunction telepítve.

* Hozzon létre hello Azure-erőforrás hello a helyszíni adatok átjáró - [hello data gateway kapcsolat](../logic-apps/logic-apps-gateway-connection.md).

* Hivatalos támogatott IBM WebSphere MQ-verziók:
   * 7.5 MQ
   * MQ 8.0

## <a name="create-a-logic-app"></a>Logikai alkalmazás létrehozása

1. A hello **Azure start Bizottsága**, jelölje be  **+**  (pluszjel) **Web + mobil**, majd **Logic App**. 
2. Adja meg a hello **neve**, MQTestApp, például **előfizetés**, **erőforráscsoport**, és **hely** (használhatnak hello adott hello a helyszíni Data Gateway kapcsolat van konfigurálva). Válassza ki **PIN-kód toodashboard**, és válassza ki **létrehozása**.  
![Logikai alkalmazás létrehozása](media/connectors-create-api-mq/Create_Logic_App.png)

## <a name="add-a-trigger"></a>Egy eseményindító hozzáadása

> [!NOTE]
> hello MQ-összekötő nem rendelkezik a eseményindítókat. Igen, használja a logikai alkalmazás, például a hello az egy másik eseményindító toostart **ismétlődési** eseményindító. 

1. Hello **Logic Apps Designer** megnyílik, jelölje be **ismétlődési** közös eseményindítók hello listájában.
2. Válassza ki **szerkesztése** hello ismétlődési eseményindító belül. 
3. Set hello **gyakoriság** túl**nap**, és a set hello **időköz** túl**7**. 

## <a name="browse-a-single-message"></a>Egyetlen üzenet tallózása
1. Válassza ki **+ új lépés**, és válassza ki **művelet hozzáadása**.
2. Hello keresési mezőbe, írja be a `mq`, majd válassza ki **MQ - Tallózás üzenet**.  
![Keresse meg az üzenet](media/connectors-create-api-mq/Browse_message.png)

3. Ha nincs meglévő kapcsolat MQ, majd hello kapcsolat létrehozása:  

    1. Válassza ki **keresztül, a helyszíni adatátjáró**, és adja meg a MQ Server hello tulajdonságait.  
    A **Server**, adja meg a hello MQ kiszolgálónevet, vagy adjon meg egy kettőspontot és hello portszám követ hello IP-cím. 
    2. Hello **átjáró** legördülő lista ismerteti a meglévő átjáró kapcsolatokat, amelyek vannak konfigurálva. Válassza ki az átjárót.
    3. Válassza ki **létrehozása** befejezésekor. A kapcsolat a következőhöz hasonló toohello következő:   
    ![Kapcsolat tulajdonságai](media/connectors-create-api-mq/Connection_Properties.png)

4. Hello művelet tulajdonságai, a következőket teheti:  

    * Használjon hello **várólista** tulajdonság tooaccess hello kapcsolat meghatározott mint egy másik várólista neve
    * Használjon hello **MessageId**, **CorrelationId**, **GroupId**, és egyéb tulajdonságok toobrowse hello különböző MQ üzenet tulajdonságai alapján üzenet
    * Állítsa be **IncludeInfo** túl**igaz** tooinclude további üzenetadatok hello kimenet. Vagy állítsa be úgy túl**hamis** toonot is tartalmazza. további üzenet hello kimeneti.
    * Adjon meg egy **időtúllépés** érték toodetermine az egy üres várólistában lévő üzenetek tooarrive mennyi ideig toowait. Ha nem ad meg, hello hello várólista első üzenetébe lekérésére, és nincs várakozás a egy üzenet tooappear töltött idő.  
    ![Keresse meg az üzenet tulajdonságai](media/connectors-create-api-mq/Browse_message_Props.png)

5. **Mentés** a módosításokat, majd **futtatása** a Logic Apps alkalmazást:  
![Mentse és futtatása](media/connectors-create-api-mq/Save_Run.png)

6. Néhány másodperc múlva futtassa hello hello lépésein is látható, és megnézheti hello kimeneti. Válassza ki a hello zöld pipa toosee részletek minden egyes lépést. Válassza ki **tekintse meg a nyers kimenetek** toosee további részleteket a hello kimeneti adatokat.  
![Keresse meg a kimeneti üzenet](media/connectors-create-api-mq/Browse_message_output.png)  

    Nyers kimenete:  
    ![Keresse meg a nyers kimeneti üzenet](media/connectors-create-api-mq/Browse_message_raw_output.png)

7. Ha hello **IncludeInfo** tootrue beállítás, a következő kimeneti hello jelenik meg:  
![Tallózás hibaüzenet tartalmazza adatai](media/connectors-create-api-mq/Browse_message_Include_Info.png)

## <a name="browse-multiple-messages"></a>Keresse meg a több üzenetet
Hello **keresse meg az üzenetek** művelet tartalmaz egy **BatchSize** beállítás tooindicate üzenetek számának vissza kell adni az hello üzenetsorból.  Ha **BatchSize** nem tartozik bejegyzés, a rendszer visszairányítja az összes üzenet. hello érkezett kimeneti üzenetek tömbjét.

1. Hello hozzáadásakor **keresse meg az üzenetek** hello első kapcsolat van konfigurálva, a művelet egyben az alapértelmezett. Válassza ki **kapcsolat módosítása** toocreate új kapcsolatot, vagy válasszon ki egy másik kapcsolat.

2. Tallózás hello kimenete üzenetek megjelenítése:  
![Keresse meg az üzenetek kimeneti](media/connectors-create-api-mq/Browse_messages_output.png)

## <a name="receive-a-single-message"></a>Egyetlen üzenet
Hello **Receive üzenet** művelet rendelkezik hello azonos bemenetekhez és kimenetekhez, hello **Tallózás üzenet** művelet. Használata esetén **Receive üzenet**, üdvözlőüzenetére hello várólista törlődik.

## <a name="receive-multiple-messages"></a>Több üzenetet
Hello **üzeneteket fogadni** művelet rendelkezik hello azonos bemenetekhez és kimenetekhez, hello **keresse meg az üzenetek** művelet. Használata esetén **üzeneteket fogadni**, köszönőüzenetei hello várólista törlődnek.

Ha nincsenek üzenetek hello várólista végrehajtásakor a Tallózás gombra, vagy egy fogadási, hello lépés meghiúsul, és a következő kimeneti hello:  
![MQ nem üzenet hiba](media/connectors-create-api-mq/MQ_No_Msg_Error.png)

## <a name="send-a-message"></a>Üzenet küldése
1. Hello hozzáadásakor **üzenet küldése** hello első kapcsolat van konfigurálva, a művelet egyben az alapértelmezett. Válassza ki **kapcsolat módosítása** toocreate új kapcsolatot, vagy válasszon ki egy másik kapcsolat. érvényes hello **üzenettípusok** vannak **Datagram**, **válasz**, vagy **kérelem**.  
![Üzenet tulajdonságai küldése](media/connectors-create-api-mq/Send_Msg_Props.png)

2. hello kimeneti üzenet küldése a következőhöz hasonló hello:  
![Üzenet kimenetként](media/connectors-create-api-mq/Send_Msg_Output.png)

## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/mq/).

## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md). Fedezze fel más rendelkezésre álló összekötők Logic Apps: hello a [API-k lista](apis-list.md).
