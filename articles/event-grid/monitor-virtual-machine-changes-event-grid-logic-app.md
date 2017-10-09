---
title: "aaaMonitor virtuális gép módosítások - Azure esemény rács & Logic Apps |} Microsoft Docs"
description: "Ellenőrizze a virtuális gépek (VM) konfigurációs változásokat Azure esemény rács és a Logic Apps segítségével"
keywords: "a Logic apps, az esemény rácsok, a virtuális gép, Virtuálisgép"
services: logic-apps
author: ecfan
manager: anneta
ms.assetid: 
ms.workload: logic-apps
ms.service: logic-apps
ms.topic: article
ms.date: 08/16/2017
ms.author: LADocs; estfan
ms.openlocfilehash: f0633e598be6e7880a310e6f8e64f6738cc692b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-virtual-machine-changes-with-azure-event-grid-and-logic-apps"></a>Virtuális gép módosítások Azure esemény rács és logikai alkalmazások figyelése

Megkezdheti az automatizált [logic app munkafolyamat](../logic-apps/logic-apps-what-are-logic-apps.md) amikor adott események következnek be, az Azure-erőforrások és a külső erőforrásokhoz. Ezeket az erőforrásokat is közzéteheti ezeket események tooan [Azure esemény rács](../event-grid/overview.md). Viszont hello esemény rács leküldéses értesítések ezen események toosubscribers, várólisták, webhookokkal, amelyeken vagy [az event hubs](../event-hubs/event-hubs-what-is-event-hubs.md) neve azonosítja. Az előfizető, mint a Logic Apps alkalmazást várja meg, amíg hello esemény rácsban eseményekre programozás nélkül tooperform feladatok - automatizált munkafolyamatok futtatása előtt.

Például az alábbiakban néhány esemény, hogy a közzétevők küldhet toosubscribers hello Azure esemény rács szolgáltatáson keresztül:

* Létrehozása, olvasása, frissítése vagy erőforrás törlése. Például módosításokat, előfordulhat, hogy az Azure-előfizetéshez a költségek, és a számlázási hatással lehet figyelni. 
* Hozzáadása vagy eltávolítása egy személy az Azure-előfizetésből.
* Az alkalmazás egy bizonyos művelet végrehajtása.
* A várólista egy új üzenet jelenik meg.

Ebben az oktatóanyagban létrehoz egy logikai alkalmazás, amely figyeli a módosítások tooa virtuális gép, és ezeket a módosításokat vonatkozó e-mailt küld. Ha egy esemény-előfizetést egy Azure-erőforrás a logikai alkalmazás létrehozásához, események haladjanak keresztül egy esemény rács toohello logikai alkalmazás erőforrás. hello oktatóanyag végigvezeti a logikai alkalmazás létrehozása:

![Áttekintés – esemény rács és a logic app virtuális gép figyelője](./media/monitor-virtual-machine-changes-event-grid-logic-app/monitor-virtual-machine-event-grid-logic-app-overview.png)

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Egy esemény rácsban eseményeket figyelő logikai alkalmazás létrehozása.
> * Adjon hozzá olyan feltétel, amely kifejezetten ellenőrzi a virtuális gép módosításokat.
> * E-mail küldése, ha a virtuális gép vált.

## <a name="prerequisites"></a>Előfeltételek

* Az e-mail-fiók [bármely Azure Logic Apps által támogatott e-mail szolgáltató](../connectors/apis-list.md), például az Office 365 Outlook, Outlook.com-os vagy Gmailes, az értesítések küldésével. Ez az oktatóanyag az Office 365 Outlook használja.

* A [virtuális gép](https://azure.microsoft.com/services/virtual-machines). Ha még nem tette meg, a virtuális gép létrehozása egy [hozzon létre egy virtuális gép oktatóanyag](https://docs.microsoft.com/azure/virtual-machines/). toomake hello virtuális gép közzé az eseményeket, hogy [nem kell semmi mást toodo](../event-grid/overview.md).

## <a name="create-a-logic-app-that-monitors-events-from-an-event-grid"></a>Egy esemény rácsban eseményeket figyelő logikai alkalmazás létrehozása

Először logikai alkalmazás létrehozása és hozzáadása, amely figyeli a virtuális gép hello erőforráscsoport rács eseményindító. 

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). 

2. Hello bal felső sarkában hello Azure főmenü, válassza a **új** > **vállalati integrációs** > **logikai alkalmazás**.

   ![Logikai alkalmazás létrehozása](./media/monitor-virtual-machine-changes-event-grid-logic-app/azure-portal-create-logic-app.png)

3. A logikai alkalmazás létrehozása a következő táblázat hello megadott hello beállításokkal:

   ![Adja meg a logic app adatait](./media/monitor-virtual-machine-changes-event-grid-logic-app/create-logic-app-for-event-grid.png)

   | Beállítás | Ajánlott érték | Leírás | 
   | ------- | --------------- | ----------- | 
   | **Name (Név)** | *{a-logika-alkalmazás-neve}* | Adjon meg egy egyedi logikai alkalmazás neve. | 
   | **Előfizetés** | *{a-Azure-előfizetés}* | Válasszon azonos Azure-előfizetés az összes szolgáltatás ebben az oktatóanyagban hello. | 
   | **Erőforráscsoport** | *{a-Azure-erőforráscsoport}* | Válassza ki az összes szolgáltatás ugyanazon Azure erőforráscsoport ebben az oktatóanyagban hello. | 
   | **Hely** | *{a-Azure-régió}* | Válassza ki az összes szolgáltatás ugyanabban a régióban hello ebben az oktatóanyagban. | 
   | | | 

4. Ha elkészült, válassza ki a **PIN-kód toodashboard**, és válassza a **létrehozása**.

   Most létrehozott egy Azure-erőforrás a logikai alkalmazásnak. 
   Azure a Logic Apps alkalmazást telepít, miután hello Logic Apps Designer azt ismerteti, közös mintára sablonok, gyorsabban kezdheti.

   > [!NOTE] 
   > Ha bejelöli **PIN-kód toodashboard**, a logikai alkalmazás automatikusan megnyílik a Logic Apps-tervezőben. Ellenkező esetben manuálisan is található, majd nyissa meg a Logic Apps alkalmazást.

5. Mostantól kiválaszthatja a logic app-sablon. A **sablonok**, válassza a **üres logikai alkalmazás** , hozhat létre a logikai alkalmazás létrehozása.

   ![Logic app-sablon kiválasztása](./media/monitor-virtual-machine-changes-event-grid-logic-app/choose-logic-app-template.png)

   Logic Apps Designer hello most bemutatja, [ *összekötők* ](../connectors/apis-list.md) és [ *eseményindítók* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) használható toostart a Logic Apps alkalmazást, és műveletek hogy egy eseményindító tooperform feladatok után is hozzáadhat. Egy eseményindítót hoz létre egy logic app-példány, és elindítja a logic app munkafolyamatot esemény. 
   A Logic Apps alkalmazást egy eseményindító hello első elemként kell.

6. Hello keresési mezőbe írja be az "esemény rács" szűrőként. Válassza ki az ehhez az eseményindítóhoz: **Azure esemény rács - erőforrás-esemény**

   ![Válassza ki az eseményindító: "Azure esemény rács - erőforrás esemény"](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger.png)

7. Amikor a rendszer kéri, jelentkezzen be a Azure hitelesítő adataival esemény rács tooAzure.

   ![Jelentkezzen be Azure hitelesítő adataival](./media/monitor-virtual-machine-changes-event-grid-logic-app/sign-in-event-grid.png)

   > [!NOTE]
   > Ha be van jelentkezve be személyes Microsoft-fiókkal, például a @outlook.com vagy @hotmail.com, hello esemény rács eseményindító nem megfelelően jelenik meg. A probléma megoldásához válassza [Connect szolgáltatás egyszerű](/azure-resource-manager/resource-group-create-service-principal-portal.md), vagy az Azure Active Directory társított Azure-előfizetése, például hello tagjaként hitelesítéséhez *felhasználónév-* @emailoutlook.onmicrosoft.com.

8. Most már az előfizetés, a logic app toopublisher események. Az esemény-előfizetés a következő táblázat hello hello részletesen:

   ![Esemény előfizetés részletesen](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details-generic.png)

   | Beállítás | Ajánlott érték | Leírás | 
   | ------- | --------------- | ----------- | 
   | **Előfizetés** | *{virtuális-machine-Azure-előfizetés}* | Válassza ki a hello esemény publisher Azure-előfizetés. A jelen oktatóanyag esetében válassza ki a hello Azure-előfizetés a virtuális géphez. | 
   | **Erőforrás típusa** | Microsoft.Resources.resourceGroups | Válassza ki a hello esemény publisher erőforrás típusát. A jelen oktatóanyag esetében válassza ki, a logikai alkalmazás figyeli csak erőforráscsoportok hello megadott értéket. | 
   | **Erőforrás neve** | *{virtuális-erőforrás-csoport-számítógépnév}* | Válassza ki a hello publisher erőforrás nevét. A jelen oktatóanyag esetében válassza ki a virtuális gép hello hello erőforráscsoport nevét. | 
   | Választható beállítások kiválasztása **speciális beállítások megjelenítése**. | *{leírását lásd:}* | * **Előtag-szűrő**: A jelen oktatóanyag esetében ez a beállítás üresen hagyja. hello alapértelmezett viselkedést összes érték megegyezik. Azonban megadhat egy előtag-karakterlánc szűrőként, például egy elérési utat és egy adott erőforrás paramétert. <p>* **Szűrő utótag**: A jelen oktatóanyag esetében ez a beállítás üresen hagyja. hello alapértelmezett viselkedést összes érték megegyezik. Azonban megadhat egy karakterlánc utótagja szűrőként, például a fájlnév-kiterjesztésűeket, ha azt szeretné, hogy csak bizonyos fájltípusok.<p>* **Előfizetés neve**: Adjon meg egy egyedi nevet az esemény-előfizetéséhez. |
   | | | 

   Amikor elkészült, a rács eseményindító ebben a példában látható:
   
   ![Példa esemény rács eseményindító részletei](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-trigger-details.png)

9. Mentse a Logic Apps alkalmazást. A hello designer eszköztáron kattintson **mentése**. toocollapse és elrejtése egy művelet részleteit a Logic Apps alkalmazást, válassza a hello művelet címsorában.

   ![A logikai alkalmazás mentése](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save.png)

   A Logic Apps alkalmazást a rács eseményindító mentésekor az Azure automatikusan létrehoz egy esemény-előfizetést a logic app tooyour kiválasztott erőforrás. Ezért amikor hello erőforrás tesz közzé egy esemény toohello esemény rács, adott esemény rács automatikusan leküldéses értesítések hello esemény tooyour logikai alkalmazást. Ezt az eseményt váltja ki a Logic Apps alkalmazást, akkor hoz létre, és fut a következő lépések meghatározó hello munkafolyamat példánya.

A Logic Apps alkalmazást élő és hello esemény rácsban tooevents figyeli, de nem semmit műveletek toohello munkafolyamat hozzáadásáig. 

## <a name="add-a-condition-that-checks-for-virtual-machine-changes"></a>Olyan feltétel, amely ellenőrzi a virtuális gép módosításokat hozzáadása

toorun a logic app munkafolyamat csak akkor, ha egy adott esemény történik, adjon hozzá olyan feltétel, amely ellenőrzi, hogy a virtuális gép "write" műveletek. Ez az állapot értéke igaz, amikor elküldi a Logic Apps alkalmazást, e-mailben elküldi a frissített hello virtuális gép adatait.

1. A Logic App Designer, a rács hello eseményindító, válassza a **új lépés** > **feltétel hozzáadása**.

   ![Egy feltétel tooyour logikai alkalmazás hozzáadása](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-add-condition-step.png)

   Logic App Designer hello egy üres feltétel tooyour munkafolyamat, beleértve a művelet elérési utak toofollow alapján, hogy hello feltétele igaz vagy hamis értéket ad.

   ![Üres feltétel](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-add-empty-condition.png)

2. A hello **feltétel** válassza **speciális módban szerkesztése**.
Adja meg a kifejezés:

   `@equals(triggerBody()?['data']['operationName'], 'Microsoft.Compute/virtualMachines/write')`

   A feltétel most néz ki ebben a példában:

   ![Üres feltétel](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-expression.png)

   Ebben a kifejezésben ellenőrzi hello esemény `body` a egy `data` objektumot adott hello `operationName` tulajdonsága hello `Microsoft.Compute/virtualMachines/write` műveletet. 
   További információ [esemény rács esemény séma](../event-grid/event-schema.md).

3. hello feltétel leírását tooprovide válasszon hello **folytatást jelző pontokra** (**...** ) hello feltétel alakzat gombra, majd kattintson a **átnevezése**.

   > [!NOTE] 
   > hello az oktatóanyag későbbi példákat is leírását tartalmazzák hello logic app munkafolyamat lépéseit.

4. Válasszon **alapvető módban szerkesztése** , hogy hello kifejezés automatikusan feloldja a látható módon:

   ![Logic app feltétel](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-1.png)

5. Mentse a Logic Apps alkalmazást.

## <a name="send-email-when-your-virtual-machine-changes"></a>E-mail küldése, ha a virtuális gép vált

Ezután adja hozzá egy [ *művelet* ](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts) , hogy e-mailben kapott hello megadott feltétel teljesül.

1. Hello állapotban **igaz értéke esetén** válassza **művelet hozzáadása**.

   ![Ha a feltétel teljesül-e a művelet hozzáadása](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-condition-2.png)

2. Hello keresési mezőbe írja be az "e-mail" szűrőként. Az e-mailt provider alapján, keresse meg és válassza ki a megfelelő összekötő hello. Ezután válassza ki a Connector hello "e-mail küldési" műveletet. Példa: 

   * Az Azure munkahelyi vagy iskolai fiók, jelölje be hello Office 365 Outlook összekötő. 
   * Személyes Microsoft-fiók jelölje ki a hello Outlook.com-összekötőt. 
   * Gmail fiókok válassza ki a hello Gmail összekötőt. 

   Hello Office 365 Outlook Connector toocontinue az oktatóanyagban módosítjuk. 
   Ha külön szolgáltatót használ, lépéseket maradnak hello hello azonos, de a felhasználói felület különböző jelenhet meg. 

   ![Válassza ki a "e-mail küldési" művelet](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email.png)

3. Ha még nem rendelkezik a kapcsolat az e-mailt Provider, a bejelentkezés tooyour e-mail fiók kérő hitelesítéshez.

4. A következő táblázat hello hello e-mailek részletesen:

   ![Üres e-mail művelet](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-empty-email-action.png)

   > [!TIP]
   > a munkafolyamat elérhető mezők a tooselect kattintson a szerkesztőmező úgy, hogy hello **dinamikus tartalom** megnyílik listában, vagy válasszon **dinamikus tartalom hozzáadása az**. További mezők kiválasztása **további** hello listán minden szakaszhoz. tooclose hello **dinamikus tartalom** menüben válassza ki **dinamikus tartalom hozzáadása az**.

   | Beállítás | Ajánlott érték | Leírás | 
   | ------- | --------------- | ----------- | 
   | **A** | *{címzett – e-mail-cím}* |Hello címzett e-mail címet adjon meg. Tesztelési célokra használhatja az e-mail címét. | 
   | **Tulajdonos** | Erőforrás frissítése: **tulajdonos**| Adja meg hello tartalom hello e-mail tárgyát. Adja meg a jelen oktatóanyag esetében hello javasolt szöveg és select hello esemény **tulajdonos** mező. Az e-mail tárgyát ide, frissített hello erőforrás (virtuális gép) hello nevét tartalmazza. | 
   | **Törzs** | Erőforráscsoport: **témakör** <p>Eseménytípus: **esemény típusa**<p>Eseményazonosító: **azonosítója**<p>Idő: **esemény időpontja** | Adja meg hello tartalom hello e-mail törzse. Adja meg a jelen oktatóanyag esetében hello javasolt szöveg és select hello esemény **témakör**, **eseménytípus**, **azonosító**, és **esemény időpontja** mezők, hogy az e-maileket hello erőforráscsoport-név, esemény típusa, az esemény időbélyegzője és hello frissítés eseményazonosító tartalmazza. <p>a tartalom üres sorok tooadd nyomja le a Shift + Enter. | 
   | | | 

   > [!NOTE] 
   > Ha egy tömb jelölő mező választhat, hello designer automatikusan hozzáadja egy **minden** hurok hello művelet által hivatkozott hello tömb körül. Ily módon a logikai alkalmazás végrehajt egy műveletet minden egyes tömb elemen.

   Most az e-mailek művelettel ebben a példában látható:

   ![Válassza ki a kimenetek tooinclude e-mailben](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-send-email-details.png)

   És a befejezett Logic Apps alkalmazást ebben a példában nézhet ki:

   ![Befejezett logikai alkalmazás](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-completed.png)

5. Mentse a Logic Apps alkalmazást. toocollapse és elrejtése minden művelet részleteit a Logic Apps alkalmazást, válassza a hello művelet címsorában.

   ![A logikai alkalmazás mentése](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-event-grid-save-completed.png)

   A Logic Apps alkalmazást élő, de módosítások tooyour virtuális gép vár, mielőtt bármi. 
   tootest a Logic Apps alkalmazást, továbbra is toohello a következő szakaszban.

## <a name="test-your-logic-app-workflow"></a>Tesztelje a logic app munkafolyamat

1. a meghatározott események, hogy a Logic Apps alkalmazást, hogy már nehézséget hello toocheck, frissítse a virtuális gépet. 

   Például a virtuális gép az Azure-portálon hello átméretezheti vagy [méretezze át a virtuális Gépet az Azure PowerShell](../virtual-machines/windows/resize-vm.md). 

   Néhány másodpercen belül egy e-mailt kell kapnia. Példa:

   ![E-mail-virtuális gép frissítése](./media/monitor-virtual-machine-changes-event-grid-logic-app/email.png)

2. tooreview hello fut, és eseményindító előzményeit a Logic Apps alkalmazást, a logic app menüben válasszon **áttekintése**. tooview további információt a futtató, válassza a hello sor futtató.

   ![Logic Apps alkalmazást futtat előzmények](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history.png)

3. tooview hello bemenetekhez és kimenetekhez minden egyes lépést, bontsa ki a megjeleníteni kívánt tooreview hello lépés. Ez az információ segítségével diagnosztizálhatja és a logikai alkalmazás felmerülő problémák hibakeresését.
 
   ![Futtassa az előzmények részletek logikai alkalmazás](./media/monitor-virtual-machine-changes-event-grid-logic-app/logic-app-run-history-details.png)

Gratulálunk, elkészítette és erőforrás-események az esemény rács keresztül figyeli, és e-mailt küld, ha ezen események logikai alkalmazás futtatására. Is megtanulta, hogyan könnyen, amely automatizálja a folyamatok és rendszerek integrálható a felhőalapú szolgáltatások munkafolyamatokat hozhat létre.

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ez az oktatóanyag erőforrást használ, és költségek az Azure-előfizetéshez a műveletet. Amikor elkészült, hello oktatóanyag és tesztelését, ügyeljen, hogy letiltja, vagy semmilyen erőforráshoz, amennyiben nem szeretné, hogy tooincur díjak törlése.

A logikai alkalmazás fut, és e-mailek küldéséhez hello alkalmazás törlése nélkül állíthatók le. A logic app menüben válasszon **áttekintése**. A hello eszköztárában kattintson **letiltása**.

![A logikai alkalmazás kikapcsolása](./media/monitor-virtual-machine-changes-event-grid-logic-app/turn-off-disable-logic-app.png)

## <a name="faq"></a>GYIK

**A Q**: milyen más virtuális gép figyelési feladatok végrehajthatok esemény rácsok és a logic apps? </br>
**A**: figyelheti egyéb konfigurációs módosítások végrehajtása, például:

* A virtuális gép lekérdezi szerepköralapú hozzáférés-vezérlést (RBAC).
* Módosításai tooa hálózati biztonsági csoport (NSG) egy adott hálózati csatoló (NIC).
* A virtuális gép lemezeinek hozzáadásakor vagy eltávolításakor.
* A nyilvános IP-cím hozzá van rendelve a tooa virtuális gép hálózati adaptert.

## <a name="next-steps"></a>Következő lépések

* [Esemény rács áttekintése](../event-grid/overview.md)
* [Esemény rács fogalmak](../event-grid/concepts.md)
* [Gyors üzembe helyezés: Hozzon létre és útvonal-esemény rácshoz egyéni események](../event-grid/custom-event-quickstart.md)
* [Esemény rács esemény séma](../event-grid/event-schema.md)
* [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)
* [Létrehozott logikai alkalmazás munkafolyamatok előre definiált sablonok](../logic-apps/logic-apps-use-logic-app-templates.md)