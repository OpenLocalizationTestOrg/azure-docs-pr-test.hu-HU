---
title: "az Azure Logic Apps helyszíni kiszolgálón tárolt olyan adatforrások aaaAccess |} Microsoft Docs"
description: "Hello helyszíni adatátjáró beállítása, hogy hozzáférhessen a helyszíni adatforrások a logic Apps alkalmazásokból"
keywords: "adatok, a helyszíni, az adatok átvitele, a titkosítás és a adatforrások eléréséhez"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 1d3deaac5a095316ce78e224dab0c08559bc2ff2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-hello-on-premises-data-gateway"></a>Hozzáférés a helyszíni a logic Apps alkalmazásokból hello a helyszíni adatok átjáróval adatforrásokhoz

a logic apps a helyi kiszolgálón tárolt olyan adatforrások tooaccess állítson be egy helyszíni adatátjárót, támogatott összekötők logic Apps alkalmazásokat használhat. hello átjáró működik hídként, amelyen a gyors adatátvitel és a helyszíni adatforrások és a logic Apps alkalmazások között. hello átjáró továbbítja a titkosított csatornákon keresztül hello Azure Service Bus helyszíni forrásból származó adatokat. Az összes forgalom származik, mint hello átjáró ügynök biztonságos kimenő forgalmát. További információ [hello adatátjáró működése](logic-apps-gateway-install.md#gateway-cloud-service). 

hello átjárót a helyszíni kapcsolatok toothese adatforrások támogatja:

*   BizTalk Server 2016
*   DB2  
*   Fájlrendszer
*   Informix
*   MQ
*   MySQL
*   Oracle Database
*   PostgreSQL
*   SAP-alkalmazáskiszolgáló 
*   SAP üzenet kiszolgáló
*   SharePoint
*   SQL Server
*   Teradata

Ezeket a lépéseket hogyan tooset hello fel a helyszíni adatok átjáró toowork a a logic apps megjelenítése. További információ a támogatott összekötők: [az Azure Logic Apps összekötők](../connectors/apis-list.md). 

Hogyan toouse hello átjáró más szolgáltatásokkal kapcsolatos információkért lásd: ezek a cikkek:

*   [Microsoft Power BI helyszíni adatátjáró](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Az Azure Analysis Services helyi adatátjáró](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow helyszíni adatátjáró](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps helyszíni adatátjáró](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a>Követelmények

* Már rendelkeznie kell [hello adatátjáró telepítve a helyi számítógép](logic-apps-gateway-install.md).

* Bejelentkezéskor toohello Azure-portálon, hogy rendelkezik-e toouse hello ugyanazzal a munkahelyi vagy iskolai fiókkal, amely túl lett megadva[hello helyszíni adatátjáró telepítése](logic-apps-gateway-install.md#requirements). A bejelentkezési fiók is rendelkeznie kell egy Azure-előfizetés toouse létrehozásakor egy átjáró-erőforráshoz hello Azure-portálon az átjáró telepítéséhez.

* Az átjáró telepítése már nem engedte, hogy az Azure átjáró-erőforráshoz. Az átjáró telepítési tooonly egy Azure átjáró-erőforráshoz lehet társítani. Hozza létre a hello átjáró erőforrás hello telepítési nem érhető el az egyéb erőforrások jogcím történik.

## <a name="set-up-hello-data-gateway-connection"></a>Hello data gateway kapcsolat beállítása

### <a name="1-install-hello-on-premises-data-gateway"></a>1. Hello helyszíni adatátjáró telepítése

Ha még nem tette meg, kövesse a hello [lépéseket tooinstall hello helyszíni adatátjáró](logic-apps-gateway-install.md). Hello folytatása előtt más lépéseket, győződjön meg arról, hogy a helyi számítógépen telepítve hello adatátjáró.

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-hello-on-premises-data-gateway"></a>2. Hozzon létre egy Azure-erőforrás hello a helyszíni adatok átjáró

Telepítése után hello átjáró a helyi számítógépen, létre kell hoznia az Ön data gateway, az Azure-ban erőforrásként. Ebben a lépésben az Azure-előfizetéshez is társítja az átjáró-erőforráshoz.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon"). Győződjön meg arról, hogy toouse hello Azure ugyanazzal a munkahelyi vagy iskolai e-mail címét használja a tooinstall hello átjáró.

2. Hello bal oldali menüben az Azure-ban, válassza ki a **új** > **vállalati integrációs** > **helyszíni adatátjáró** itt látható módon:

   !["A helyszíni adatok gateway" található](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. A hello **kapcsolat átjáró létrehozása** panelen adja meg a részleteket toocreate a data gateway erőforrás:

    * **Név**: Adjon meg egy nevet az átjáró-erőforráshoz. 

    * **Előfizetés**: Válasszon hello Azure-előfizetés tooassociate az átjáró-erőforráshoz. 
    Ehhez az előfizetéshez kell hello a Logic Apps alkalmazást tárolóként ugyanazt az előfizetést.
   
      hello alapértelmezett előfizetés hello Azure-fiókot, amelyet a toosign használt alapul.

    * **Erőforráscsoport**: hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot üzembe helyezéséhez az átjáró-erőforráshoz. 
    Erőforráscsoportok segítségével gyűjteményként kapcsolódó Azure eszközöket kezelheti.

    * **Hely**: Azure korlátozza a hely toohello ugyanabban a régióban hello átjáró felhőszolgáltatás során kiválasztott [átjáró telepítésének](logic-apps-gateway-install.md). 

      > [!NOTE]
      > Győződjön meg arról, hogy hello átjáró erőforrás helye megegyezik-e a hello átjáró felhőalapú szolgáltatás helyét. Ellenkező esetben az átjáró telepítése nem jelenik meg az Ön tooselect hello telepített átjárók listában hello következő lépésben.
      > 
      > Különböző régiókban is használhatja, az átjáró erőforrás és a logikai alkalmazásnak.

    * **Telepítési neve**: Ha nincs kiválasztva, akkor az átjáró telepítésének, válassza ki a korábban telepített hello átjáró. 

    tooadd hello átjáró erőforrás tooyour Azure irányítópultot, válassza a **PIN-kód toodashboard**. 
    Amikor elkészült, válassza ki a **létrehozása**.

    Példa:

    ![Adja meg a részleteket toocreate a helyszíni adatátjáró](./media/logic-apps-gateway-connection/createblade.png)

    toofind vagy nézet, az Ön data gateway bármikor hello fő Azure bal oldali menüben lépjen túl **több szolgáltatások** > **vállalati integrációs** > **a helyszíni adatokhoz Átjárók**.

    ![Nyissa meg túl "Szolgáltatás", "Vállalati integrációs", "A helyszíni Data Gateways"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-toohello-on-premises-data-gateway"></a>3. Csatlakozás a logic app toohello helyszíni adatátjáró

Most, hogy a data gateway erőforrás elkészítette és az Azure-előfizetéshez társított erőforrás, a logic app és hello data gateway közötti kapcsolat létrehozása.

> [!NOTE]
> Az átjáró kapcsolódási helyet már léteznie kell hello azonos régióban legyen, mint a Logic Apps alkalmazást, de használhatja a data gateway már egy másik régióban.

1. Hozzon létre hello Azure-portálon, vagy nyissa meg a Logic Apps alkalmazást a Logic App tervezőben.

2. Adjon hozzá egy összekötőt, amely támogatja a helyi kapcsolatokat, például az SQL Server.

3. Válassza ki a következő hello sorrendben, **keresztül, a helyszíni adatátjáró**, adjon meg egy egyedi kapcsolatnevet hello szükséges információkat, és válassza a hello data gateway erőforrás, amelyet az toouse. Amikor elkészült, válassza ki a **létrehozása**.

   > [!TIP]
   > Egy egyedi kapcsolatnevet segítségével könnyen később, hogy a kapcsolat azonosítására, különösen akkor, ha több kapcsolatot is létrehozhat. Ha alkalmazható, tartalmazzák a felhasználónévhez hello tartománynév. 

   ![Logic app és az adatok átjáró közötti kapcsolat létrehozása](./media/logic-apps-gateway-connection/blankconnection.png)

Gratulálunk, az átjáró kapcsolat készen áll a a logic app toouse.

## <a name="edit-your-gateway-connection-settings"></a>Az átjáró kapcsolat beállításainak szerkesztése

Miután gateway-kapcsolatot hoz létre a Logic Apps alkalmazást, érdemes lehet toolater frissítési hello beállításait, hogy adott kapcsolat.

1. toofind hello gateway-kapcsolatot:

   * A logic app panelen hello alatt **Fejlesztőeszközök**, jelölje be **API kapcsolatok**. 
   
     Hello **API kapcsolatok** ablaktábla megjeleníti azokat a Logic Apps alkalmazást, beleértve az átjáró kapcsolatokat társított összes API kapcsolat.

     ![Nyissa meg tooyour logikai alkalmazást, jelölje be a "API-kapcsolatok"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Vagy hello fő Azure bal oldali menüben nyissa meg túl **több szolgáltatások** > **webes & Mobile Services** > **API kapcsolatok** minden API-kapcsolatok esetén többek között a következőket átjárókapcsolatokhoz, az Azure-előfizetéshez társított. 

   * Vagy a hello fő Azure bal oldali menüben nyissa meg túl**összes erőforrás** minden API-kommunikációhoz, beleértve az átjáró kapcsolatokat, az Azure-előfizetéshez társított.

2. Válassza ki a hello gateway-kapcsolatot, hogy szeretné, hogy tooview vagy Szerkesztés, és válassza **szerkesztése API-kapcsolat**.

   > [!TIP]
   > Ha a frissítések nem lép érvénybe, próbálja meg [hello átjáró Windows-szolgáltatás leállításával és újraindításával](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a>Váltás, vagy a helyszíni adatok átjáró erőforrás törlése

toocreate egy másik átjáró-erőforráshoz, rendelje hozzá az átjárót egy másik erőforráscsoportban, vagy távolítsa el a hello átjáró-erőforráshoz, az nem befolyásolja a hello átjáró telepítésének hello átjáró erőforrás törlése. 

1. Hello fő Azure bal oldali menüben nyissa meg túl**összes erőforrás**. 
2. Keresse meg és jelölje ki az adatok átjáró-erőforráshoz.
3. Válasszon **helyszíni Data Gateway**, és a hello erőforrás eszköztárában kattintson **törlése**.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Gyakori kérdések

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Következő lépések

* [A logikai alkalmazások védelme](./logic-apps-securing-a-logic-app.md)
* [Gyakori példák és forgatókönyvek a logic Apps alkalmazások](./logic-apps-examples-and-scenarios.md)
