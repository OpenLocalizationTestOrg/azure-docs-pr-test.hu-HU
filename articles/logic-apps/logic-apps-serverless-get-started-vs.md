---
title: "egy kiszolgáló nélküli alkalmazást a Visual Studio aaaBuild |} Microsoft Docs"
description: "Ez az útmutató a létrehozás, telepítés és a Visual Studio hello alkalmazás kezelése az első kiszolgáló nélküli alkalmazást az első lépései."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 74530eea6060ffe2139f7c9d6daab8a46f808162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-serverless-app-in-visual-studio-with-logic-apps-and-functions"></a>Egy kiszolgáló nélküli alkalmazást a Visual Studio és a Logic Apps és függvények létrehozása

Kiszolgáló nélküli eszközök és a képességek az Azure-ban, hogy a felhőalapú alkalmazások telepítéséhez és gyors fejlesztést.  A dokumentum fő témáját hogyan tooget a Visual Studio egy kiszolgáló nélküli alkalmazást felépítése megkezdődött.  Az Azure-ban kiszolgáló nélküli áttekintést [ebben a cikkben található](logic-apps-serverless-overview.md).

## <a name="getting-everything-ready"></a>Felkészülés mindent

Az alábbiakban hello szükséges előfeltételeket toobuild egy kiszolgáló nélküli alkalmazást a Visual Studio eszközből:

* [A Visual Studio 2017](https://www.visualstudio.com/vs/) vagy a Visual Studio 2015 - Közösség, Professional és Enterprise
* [Logic Apps Tools for Visual Studio](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551)
* [Legfrissebb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* [Az Azure Functions Core eszközök](https://www.npmjs.com/package/azure-functions-core-tools) toodebug helyileg működik
* A beágyazott logikai alkalmazás designer hozzáférés toohello webes hello használata esetén

## <a name="getting-started-with-a-deployment-template"></a>A központi telepítési sablont első lépések

Az Azure-ban kezelése erőforrások erőforráscsoporton belül történik.  Egy erőforráscsoportot az erőforrások logikai csoportosítása.  Erőforrás-csoportok lehetővé teszik, telepítését és az erőforrások gyűjteményeinek kezelését.  Egy kiszolgáló nélküli alkalmazást az Azure-ban az erőforráscsoport Azure Logic Apps, mind az Azure Functions tartalmazza.  Hello erőforráscsoport-projektet a Visual Studio használatával képes toodevelop dolgozunk, kezelése és hello teljes alkalmazás egyetlen eszközként telepítése.

### <a name="create-a-resource-group-project-in-visual-studio"></a>A Visual Studio erőforráscsoport-projekt létrehozása

1. A Visual Studióban, kattintson a tooadd egy **új projekt**
1. A hello **felhő** kategória, jelölje be toocreate egy Azure **erőforráscsoport** projekt  
 * Ha nem látja hello kategóriát vagy a felsorolt projekt, ne feledje hello Azure SDK for Visual Studio telepítve van
1. Adjon hello projekt nevét és helyét, és válassza ki **Ok** toocreate Visual Studio tooselect sablon kéri.  Kiválaszthatja a toostart üres, a Logic App kezdődik vagy egyéb erőforrásokat.  Azonban ebben az esetben használjuk az Azure gyors üzembe helyezés sablon tooget velünk használatába egy kiszolgáló nélküli alkalmazást.
1. Válassza ki a tooshow sablonok **Azure gyors üzembe helyezési** ![kiválasztásával Azure gyors üzembe helyezési sablonok][1]
1. Jelölje be hello kiszolgáló nélküli gyorsindítási sablonon: **101-logic-app-and-function-app** kattintson **Ok**

az erőforráscsoport-projekt hello gyorsindítási sablonon létrehoz a központi telepítési sablont.  a sablonban hello egyszerű logikai alkalmazás, amely meghívja az Azure Functions, és hello eredményt adja vissza.  Ha megnyitja hello `azuredeploy.json` fájlt a Solution Explorer hello úgy is, hogy hello erőforrások hello kiszolgáló nélküli alkalmazást.

## <a name="deploying-hello-serverless-application"></a>Hello kiszolgáló nélküli alkalmazást telepítése

A Visual Studio megnyitásához hello logikai alkalmazás vizuális tervező használatára, szükség van a toobe előre telepített Azure-erőforráscsoportot.  Ez lehetővé teszi hello Tervező toocreate és a kapcsolatok tooresources használatát és a szolgáltatások hello logikai alkalmazás.  tooget elindult, egyszerűen kell létrehozott toodeploy hello megoldást.

1. Kattintson a jobb gombbal hello projektre a Visual Studióban, válassza ki **telepítés**, és hozzon létre egy **új** telepítési ![kiválasztása az új erőforrások telepítése][2]
1. Adjon meg érvényes Azure-előfizetés és az erőforráscsoport
1. Válassza ki a túl**telepítés** hello megoldás
1. Írja be a logikai alkalmazás hello hello nevét és hello Azure függvény alkalmazás.  hello Azure-függvény nevét kell toobe globálisan egyedi

a megadott erőforráscsoport hello hello kiszolgáló nélküli megoldás központilag telepíti.  Ha megnézzük hello **kimeneti** a Visual Studio hello hello központi telepítés állapota látható.

## <a name="editing-hello-logic-app-in-visual-studio"></a>A Visual Studio hello logikai alkalmazás szerkesztése

Hello megoldás bármely erőforráscsoport be van telepítve, ha hello vizuális tervező használatára használt tooedit kell, és ellenőrizze a módosítások toohello logikai alkalmazás.

1. Kattintson a jobb gombbal hello `azuredeploy.json` hello Megoldáskezelőben fájlt, és válassza ki **a Logic Apps tervező megnyitása**
1. Jelölje be hello **erőforráscsoport** és **hely** hello megoldást telepített tooand válassza **OK**

hello logikai alkalmazás vizuális tervező használatára kell a Visual Studio látható.  Tooadd lépések, hello munkafolyamat módosítása, és mentse a módosításokat.  A Visual Studio eszközből a logic apps is létrehozhat.  Ha ezt a hello **erőforrások** hello sablon navigator, tooadd választhat egy **logikai alkalmazás** toohello projekt.  Hello a vizuális tervező használatára, nem üres a logic apps betölteni egy előre üzembe helyezés egy erőforráscsoportot.

### <a name="managing-and-viewing-run-history-for-a-deployed-logic-app"></a>Kezelése és megtekintése a központilag telepített programot alkalmazások futtatási előzményei

Is kezelése és az Azure szolgáltatásba telepített logikai alkalmazások futtatása hello előzményeinek megtekintése.  Ha megnyitja hello **Cloud Explorer** eszköz a Visual Studio, a logikai alkalmazás gombbal és válassza a tooedit, tiltsa le, tulajdonságainak megtekintése vagy nézet futtatási előzményei.  A Szerkesztés is lehetővé teszi toodownload egy közzétett logikai alkalmazást a Visual Studio erőforráscsoport-projektben.  Ez azt jelenti, hogy akkor is, ha összeállítani saját logikai alkalmazását az Azure-portálon hello használatához is importálja, és a Visual Studio az adatbázis felügyeletét.

## <a name="developing-an-azure-function-in-visual-studio"></a>A Visual Studio egy Azure függvény fejlesztése

hello központi telepítési sablont telepíti bármely Azure Functions hello git-tárház hello megadott hello-megoldásban tárolt `azuredeploy.json` változók.  Létrehozhat egy függvény projekt belül hello megoldás, ha az adatforrás-vezérlő (GitHub, a Visual Studio Team Services stb.) jelölje be, és hello frissítése `repo` változó, hello sablon telepíti hello Azure-függvény.

### <a name="creating-an-azure-function-project"></a>Azure-függvény projekt létrehozása

JavaScript, Python, F #, Bash, kötegelt vagy PowerShell használata esetén kövesse a hello [hello funkciók CLI szükséges lépések](../azure-functions/functions-run-local.md) toocreate projekt.  Ha egy függvényt a C# fejleszt, használhatja a [C# osztálytár](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/16/publishing-a-net-class-library-as-a-function-app/) lehetőségre hello aktuális hello Azure-függvény.

## <a name="next-steps"></a>Következő lépések

* [Megtudhatja, hogyan toobuild egy kiszolgáló nélküli közösségi irányítópult](logic-apps-scenario-social-serverless.md)
* [A Visual Studio Cloud Explorer logikai alkalmazás kezelése](logic-apps-manage-from-vs.md)
* [Logic App munkafolyamatdefiníciós nyelve](logic-apps-workflow-definition-language.md)

<!-- Image references -->
[1]: ./media/logic-apps-serverless-get-started-vs/select-template.png
[2]: ./media/logic-apps-serverless-get-started-vs/deploy.png
