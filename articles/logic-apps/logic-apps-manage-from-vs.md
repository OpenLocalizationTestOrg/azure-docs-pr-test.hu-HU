---
title: a Visual Studio - Azure Logic Apps aaaManage a logic apps |} Microsoft Docs
description: "A logic apps és az egyéb Azure eszközök Visual Studio Cloud Explorer kezelése"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 419f83eb062b56e4ac2642dea4de1a025f747521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>A Visual Studio Cloud Explorer a logic Apps alkalmazásokat kezeléséhez

Bár hello [Azure-portálon](https://portal.azure.com/) nagyszerű lehetőséget nyújt az Ön toodesign és kezelése az Azure Logic Apps, használhatja a Visual Studio Cloud Explorer sok Azure eszközök, beleértve a logic Apps alkalmazásokat kezeléséhez. Visual Studio Cloud Explorer lehetővé teszi, hogy a Tallózás gombra, kezeléséhez, szerkesztése, és letöltés a logic apps közzé. Felügyeleti feladatok előzményeinek megtekintése, futtassa, engedélyezése és letiltása. 

Mielőtt eléréséhez, és a Visual Studio a logic Apps alkalmazásokat kezeléséhez, telepítése és a Visual Studio eszközök konfigurálása az Azure Logic Apps. 

## <a name="prerequisites"></a>Előfeltételek

* [Visual Studio 2015-öt vagy a Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Legfrissebb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)
* [A Visual Studio Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Hozzáférés toohello webes hello beágyazott designer használata esetén

## <a name="install-visual-studio-tools-for-logic-apps"></a>A Logic Apps a Visual Studio eszközök telepítése

Hello előfeltételek telepítése után töltse le, és a Visual Studio hello Azure Logic Apps eszközök telepítése.

1. Nyissa meg a Visual Studiót. A hello **eszközök** menü **bővítmények és frissítések**.
2. Bontsa ki a hello **Online** kategória hello Visual Studio galériában online kereséséhez.
3. Keresse meg **Logic Apps** amíg meg nem látja **Azure Logic Apps Tools for Visual Studio**.
4. toodownload és a telepítés hello kiterjesztéssel, kattintson a **letöltése**.
5. Indítsa újra a Visual Studio telepítése után.

> [!NOTE]
> toodownload hello Azure Logic Apps Tools for Visual Studio közvetlenül, nyissa meg toohello [Visual Studio piactér](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Tallózással keresse meg a logic apps Cloud Explorerben

1.  tooopen Cloud Explorer, a hello **nézet** menüben válasszon **Cloud Explorer**.
2.  Tallózással keresse meg a Logic Apps alkalmazást, erőforráscsoporthoz vagy erőforrástípus. 

    * Ha tallózással erőforrástípusok szerint, jelölje ki az Azure-előfizetéshez, bontsa ki a hello **Logic Apps** szakaszt, és válassza ki a Logic Apps alkalmazást. 
    * Ha tallózással erőforráscsoport, bontsa ki a Logic Apps alkalmazást tartalmazó erőforráscsoportot hello, és válassza ki a Logic Apps alkalmazást.

    tooview parancsok a logikai alkalmazásnak, vagy kattintson a jobb gombbal a Logic Apps alkalmazást, vagy a Cloud Explorer hello alján hello választhat **műveletek** menü.

    ![Tallózással keresse meg a Logic Apps alkalmazást](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>A Logic Apps alkalmazást a Logic Apps-tervezővel szerkesztése

A Cloud Explorer megnyitható a most üzembe helyezett logikai alkalmazást a hello azonos designer, amelyek hello Azure-portál használatával. 

* tooedit a Logic Apps alkalmazást, a Cloud Explorerben (Megoldáskezelőben) kattintson a jobb gombbal a Logic Apps alkalmazást, és válassza ki **nyissa meg a Logic App szerkesztő**. 

* a felhő a frissítések toohello toopublish, válassza a **közzététel**. 

* Válasszon egy új futtató toostart **futtatása eseményindító**.

![Logic Apps-Tervező](./media/logic-apps-manage-from-vs/designer.png)

Hello designer alkalmazásból is **letöltése** logikai alkalmazás. Ez a művelet automatikusan parameterizes hello logic app-definíciót, majd hello definition menti az Azure Resource Manager központi telepítési sablont. A központi telepítési sablon tooyour Azure erőforráscsoport-projekt adhat hozzá.

## <a name="browse-your-logic-app-run-history"></a>Keresse meg a Logic Apps alkalmazást futtatási előzményei

tooview hello futtatási előzményei a logikai alkalmazásnak, kattintson a jobb gombbal a Logic Apps alkalmazást, és válassza ki **nyílt futtatási előzményei**. tooreorder a futtatási előzményei alapján hello tulajdonságok látható, jelölje be hello oszlop fejlécére.

![futtatási előzményei](media/logic-apps-manage-from-vs/runs.png)

futtatási előzményei-példányok esetében, áttekintheti az eredményeket, többek között a hello bemenetekhez és kimenetekhez minden lépésben hello tooshow hello kattintson duplán a hello példányán fusson.

![Futtassa a előzménylistát, bemenetei és kimenetei lépéseiből](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Következő lépések

* [Az első logikai alkalmazás létrehozása](logic-apps-create-a-logic-app.md)
* [Tervezési, elkészítéséhez és logikai alkalmazások telepítése a Visual Studio](logic-apps-deploy-from-vs.md)
* [Gyakori példák és felhasználási helyzetek megtekintése](logic-apps-examples-and-scenarios.md)
* [Videó: Az Azure Logic Apps üzleti folyamatok automatizálása](http://channel9.msdn.com/Events/Build/2016/T694)
* [Videó: Rendszerintegráció az Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
