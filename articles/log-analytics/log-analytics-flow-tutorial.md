---
title: "aaaAutomate Azure Naplóelemzés dolgozza fel a Microsoft Flow"
description: "Ismerje meg, hogyan használhatja a Microsoft Flow tooquickly ismételhető folyamatok automatizálása hello Azure Log Analytics-összekötő használatával."
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.openlocfilehash: 52026df968682842cc9f8d55f6f9875c5f9c10b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-log-analytics-processes-with-hello-connector-for-microsoft-flow"></a>Hello Connector Log Analytics-folyamatok automatizálása a Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) lehetővé teszi több száz műveletek segítségével számos különböző szolgáltatások toocreate automatizált munkafolyamatok. Egy művelet kimenete változtatás nélkül használhatók bemeneti tooanother, hogy lehetővé teszi a különböző szolgáltatások toocreate integrációjával.  Microsoft Flow hello Azure Log Analytics-összekötő lehetővé teszik a napló megkeresi a Naplóelemzési által beolvasott adatokat tartalmazó toobuild munkafolyamatok.

Például az Office 365 e-mailben értesítést a Microsoft Flow toouse Naplóelemzési adatok felhasználásával, hozzon létre egy hibajelentést a Visual Studio Team Services, vagy közzététele a Slack üzenetet.  Olyan egyszerű ütemezés, vagy az egyes művelet egy csatlakoztatott szolgáltatással, például amikor egy e-mail vagy egy tweetet érkezik a munkafolyamat indíthat el.  


> [!NOTE]
> Microsoft Flow hello Azure Log Analytics-összekötő szükséges a munkaterület frissítése toobe toohello új Log Analytics lekérdezési nyelv. További tudnivalók hello új nyelv, és hello eljárás tooupgrade lekérése a munkaterületen a [az Azure Naplóelemzés munkaterület toonew napló keresés frissítése](log-analytics-log-search-upgrade.md).  

Ebben a cikkben hello oktatóanyag bemutatja, hogyan toocreate egy folyamatot, amely automatikusan elküldi hello e-mailben, hogyan használhatja a Microsoft Flow Naplóelemzési csak egy példa a Log Analyticshez napló keresési eredményeket. 


## <a name="step-1-create-a-flow"></a>1. lépés: A folyamat létrehozása
1. Jelentkezzen be a túl[Microsoft Flow](http://flow.microsoft.com), és válassza ki **saját Forgalomáramlás**.
2. Kattintson a **+ hozza létre az üres**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>2. lépés: A folyamat eseményindító létrehozása
1. Kattintson a **keresés több száz összekötők és eseményindítók**.
2. Típus **ütemezés** hello Keresés mezőbe.
3. Válassza ki **ütemezés**, majd válassza ki **ütemezés - ismétlődési**.
4. A hello **gyakoriság** mezőben jelölje be **nap** és hello **időköz** adja meg a **1**.<br><br>![Microsoft Flow eseményindító párbeszédpanel](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>3. lépés: A Naplóelemzési művelet hozzáadása
1. Kattintson a **+ új lépés**, és kattintson a **művelet hozzáadása**.
2. Keresse meg **Analytics jelentkezzen**.
3. Kattintson a **Azure Log Analytics-lekérdezés futtatása, és eredményeinek képi megjelenítése**.<br><br>![A Naplóelemzési lekérdezési ablakban futtassa](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-hello-log-analytics-action"></a>4. lépés: Hello Naplóelemzési művelet konfigurálása

1. Adja meg a munkaterület, beleértve a hello előfizetési azonosító, erőforráscsoport, és a munkaterület neve hello adatait.
2. Adja hozzá a következő Naplóelemzési lekérdezés toohello hello **lekérdezés** ablak.  Ez csak a mintalekérdezést, és lecserélheti bármely más, hogy adatokat ad vissza.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. Válassza ki **HTML tábla** a hello **diagramtípus**.<br><br>![Napló Analytics művelet](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-hello-flow-toosend-email"></a>5. lépés: Hello folyamat toosend e-mail konfigurálása

1. Kattintson a **új lépés**, és kattintson a **+ új művelet**.
2. Keresse meg **Office 365 Outlook**.
3. Kattintson a **Office 365 Outlook – az e-mailek küldése**.<br><br>![Az Office 365 Outlook kiválasztási ablaka](media/log-analytics-flow-tutorial/flow04.png)

4. Adjon meg egy címzett e-mail címét hello hello **való** ablakot, és az üdvözlő e-mail tárgyát **tulajdonos**.
5. Kattintson bárhova hello **törzs** mezőbe.  A **dinamikus tartalom** értékeket az előző műveletekből ablak nyílik meg.  
6. Válassza ki **törzs**.  Ez a hello Naplóelemzési művelet hello lekérdezést hello eredményeit.
6. Kattintson a **speciális beállítások megjelenítése**.
7. A hello **HTML** mezőben válassza **Igen**.<br><br>![Az Office 365 e-mailek konfigurációs ablak](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>6. lépés: Mentéséhez és a folyamat tesztelése
1. A hello **folyamat nevének** mezőbe, vegye fel a folyamat nevét, majd kattintson **folyamat létrehozása**.<br><br>![Mentés folyamata](media/log-analytics-flow-tutorial/flow06.png)
2. hello folyamata megtörtént, és amely megadott hello ütemezés naponta után fog futni. 
3. tooimmediately teszt hello folyamata, kattintson a **Futtatás most** , majd **folyamat futtatása**.<br><br>![Futtatási folyamata](media/log-analytics-flow-tutorial/flow07.png)
3. Hello folyamat befejezése után ellenőrizze a megadott hello címzett hello mail.  Kell egy e-mail törzse hasonló toohello következőre érkezett:<br><br>![Példa e-mailre](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Következő lépések

- További információ [Log Analytics-e jelentkezni a keresések](log-analytics-log-search-new.md).
- További információ [Microsoft Flow](https://ms.flow.microsoft.com).



