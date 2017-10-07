---
title: "aaaCreate, elkészítéséhez és központi telepítése a Visual Studio - Azure Logic Apps a logic apps |} Microsoft Docs"
description: "Visual Studio-projektek, tervezése, elkészítéséhez és központi telepítése az Azure Logic Apps létrehozása"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 5154cb05f9a48e9f0f2381a6953947217f7bb114
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Tervezési, elkészítéséhez és központi telepítése az Azure Logic Apps a Visual Studióban

Bár hello [Azure-portálon](https://portal.azure.com/) nagyszerű lehetőséget nyújt az Ön toocreate és kezelése az Azure Logic Apps, tervezési, kiépítési és a logic Apps alkalmazások telepítése a Visual Studio is használhatja. A Visual Studio eszközöket biztosít a gazdag hello Logic App Designer például akkor toocreate a logic apps, telepítés és az automatizálás sablonok konfigurálása és telepítése tooany környezetben. 

További lépések az Azure Logic Apps tooget [hogyan toocreate hello Azure-portálon első logika alkalmazását](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Telepítés lépései

tooinstall és a Visual Studio eszközök konfigurálása az Azure Logic Apps, kövesse az alábbi lépéseket.

### <a name="prerequisites"></a>Előfeltételek

* [A Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) vagy a Visual Studio 2015-höz
* [Legfrissebb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Hozzáférés toohello webes hello beágyazott designer használata esetén

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Telepítheti a Visual Studio tools for Azure Logic Apps

Miután telepítette a hello Előfeltételek:

1. Nyissa meg a Visual Studiót. A hello **eszközök** menü **bővítmények és frissítések**.
2. Bontsa ki a hello **Online** kategória online kereséséhez.
3. Keresse meg **Logic Apps** amíg meg nem látja **Azure Logic Apps Tools for Visual Studio**.
4. toodownload és a telepítés hello kiterjesztéssel, kattintson a **letöltése**.
5. Indítsa újra a Visual Studio telepítése után.

> [!NOTE]
> Emellett letöltheti [Azure Logic Apps-eszközök Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) és hello [Azure Logic Apps eszközök a Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) hello Visual Studio Marketplace-ről.

A telepítés befejezése után a hello Azure erőforráscsoport-projekt Logic App-tervezővel is használhatja.

## <a name="create-your-project"></a>A projekt létrehozása

1. A hello **fájl** menü Ugrás túl**új**, és válassza ki **projekt**. Vagy tooadd túl meglévő megoldás, nyissa meg a projekt-tooan**Hozzáadás**, és válassza ki **új projekt**.

    ![Fájl menü](./media/logic-apps-deploy-from-vs/filemenu.png)

2. A hello **új projekt** ablakban található **felhő**, és válassza ki **Azure erőforráscsoport**. Nevezze el a projektet, és kattintson a **OK**.

    ![Új projekt hozzáadása](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Jelölje be hello **logikai alkalmazás** sablon, amely létrehozza a központi telepítési sablont üres logic app, toouse. A sablon megadása után kattintson az **OK**.

    ![Válassza ki a Logic App-sablon](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Most már felvett a logic app projektet tooyour megoldás. 
    A Solution Explorer hello a telepítési fájlt meg kell jelennie.

    ![Telepítési fájl](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>A Logic App tervezővel a logikai alkalmazás létrehozása

Ha egy Azure erőforráscsoport-projekt, amely tartalmazza a logic app, megnyithatja hello Logic App Designer a Visual Studio toocreate a munkafolyamat. 

> [!NOTE]
> hello Tervező szükséges az internetkapcsolat túl összekötők rendelkezésre álló tulajdonságok és az adatok lekérdezése. Például ha hello Dynamics CRM Online-összekötő használata esetén hello designer lekérdezi a CRM példány tooshow rendelkezésre álló egyéni és az alapértelmezett tulajdonságokat.

1. Kattintson a jobb gombbal a `<template>.json` fájlt, és válassza ki **nyissa meg a Logic App tervezővel**. (`Ctrl+L`)

2. Válassza ki az Azure-előfizetés, erőforráscsoportot és helyet a központi telepítési sablon.

    > [!NOTE]
    > Logikai alkalmazás kialakítása API-kapcsolat erőforrások azokhoz a tulajdonságokhoz lekérdezés során létrehoz Tervező. A Visual Studio ezeket a kapcsolatokat használ a kiválasztott erőforrás csoport toocreate tervezés során. tooview vagy módosítsa a API-kapcsolatokat nyissa meg toohello Azure-portálon, és tallózással keresse meg **API kapcsolatok**.

    ![Előfizetés kiválasztása](./media/logic-apps-deploy-from-vs/designer_picker.png)

    hello designer hello definition használja a hello `<template>.json` fájl a megjelenítéshez.

4. Hozzon létre, és a logikai alkalmazás kialakítása. A módosításokat a központi telepítési sablont frissül.

    ![A Visual Studio programot Tervező](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

A Visual Studio hozzáadja `Microsoft.Web/connections` erőforrások túl az erőforrás fájlt a kapcsolatokat a Logic Apps alkalmazást kell toofunction. A kapcsolati tulajdonságok kell telepítésekor, és állítsa a telepítése után felügyelt **API kapcsolatok** a hello Azure-portálon.

### <a name="switch-toojson-code-view"></a>TooJSON kód nézetre váltani

a Logic Apps alkalmazást, jelölje be hello JSON-megjelenítés tooshow hello **kódnézetben** hello designer hello alján fülre.

tooswitch toohello teljes körű erőforrást JSON biztonsági, kattintson a jobb gombbal a hello `<template>.json` fájlt, és válassza ki **nyitott**.

### <a name="add-references-for-dependent-resources-toovisual-studio-deployment-templates"></a>Mutató hivatkozásokat adhat a tőle függő erőforrások tooVisual Studio központi telepítési sablonok

Ha azt szeretné, hogy a logic app tooreference tőle függő erőforrások, [Azure Resource Manager sablonfüggvényei](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) logic app központi telepítési sablonba. Például érdemes a logic app tooreference egy Azure-funkció vagy integrációs kívánt fiók toodeploy mellett a Logic Apps alkalmazást. Az alábbiakra kapcsolatos hogyan toouse paramétereit a központi telepítési sablont úgy, hogy a Logic App Designer hello megfelelően képezi le. 

Eseményindítók és műveletek az ilyen típusú logic app paramétereket használhatja:

*   Alárendelt munkafolyamat
*   Függvény alkalmazás
*   APIM hívás
*   API kapcsolat futásidejű URL-címe
*   API-kapcsolati útvonal

És használhatja a sablont funkciókat, például a paraméterek, változókat, resourceId, concat, stb. Például ez hogyan lecserélheti hello Azure-függvény erőforrás-azonosító:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

És ahol paraméterek szeretné használni:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Egy másik példa Service Bus küldési üzenet művelethez hello is parametrizálja:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl nem kötelező, és eltávolíthatja a sablon ha van ilyen.
> 


> [!NOTE] 
> Hello Logic App Designer toowork paraméterek, használatakor meg kell adnia az alapértelmezett értékeket, például:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>A logikai alkalmazás mentése

toosave a Logic Apps alkalmazást bármikor, nyissa meg túl**fájl** > **mentése**. (`Ctrl+S`) 

Ha a Logic Apps alkalmazást hibák az alkalmazás mentésekor, megjelennek a Visual Studio hello **kimenetek** ablak.

## <a name="deploy-your-logic-app-from-visual-studio"></a>A Logic Apps alkalmazást a Visual Studio telepítése

Az alkalmazás konfigurálását, telepítheti közvetlenül a Visual Studio csak néhány lépésben. 

1. A Megoldáskezelőben kattintson jobb gombbal a projektre, és válassza a túl**telepítés** > **új központi telepítési...**

    ![Új központi telepítés](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. Amikor a rendszer kéri, jelentkezzen be Azure-előfizetés tooyour. 

3. Most már ki kell választania, ahová toodeploy a Logic Apps alkalmazást hello erőforráscsoport hello részleteit. Amikor elkészült, válassza ki a **telepítés**.

    > [!NOTE]
    > Győződjön meg arról, hogy ki kell választania hello megfelelő sablon és paraméterfájl hello erőforráscsoport. Ha azt szeretné, hogy toodeploy tooa éles környezetben, például válassza a hello éles paraméterek fájl.

    ![Tooresource csoport központi telepítése](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    hello központi telepítési állapot jelenik meg hello **kimeneti** ablak. 
    Előfordulhat, hogy tooselect **Azure kiépítés** a hello **megjelenítése kimenetét** lista.

    ![Központi telepítési állapot kimeneti](./media/logic-apps-deploy-from-vs/output.png)

Jövőbeli hello szerkessze a Logic Apps alkalmazást a verziókövetési rendszerrel, és használja a Visual Studio toodeploy új verziók.

> [!NOTE]
> Közvetlenül hello definíciójában hello Azure-portálon módosítja, ezeket a módosításokat a rendszer felülírja központi telepítésekor a Visual Studio eszközből legközelebb. 

## <a name="add-your-logic-app-tooan-existing-resource-group-project"></a>Adja hozzá a logic app tooan meglévő erőforráscsoport-projekt

Ha egy meglévő erőforráscsoport-projekt, adhat hozzá a logic app toothat projekt hello JSON-vázlat ablak. Egy másik logikai alkalmazást a korábban létrehozott hello alkalmazás mellett azt is megteheti.

1. Nyissa meg hello `<template>.json` fájlt.

2. tooopen hello JSON-vázlat ablak, nyissa meg túl**nézet** > **más Windows** > **JSON-vázlat**.

3. egy erőforrás toohello sablonfájl tooadd kattintson **erőforrás hozzáadása** el hello hello JSON-vázlat ablak tetején. Vagy hello JSON-vázlat ablak, kattintson a jobb gombbal **erőforrások**, és válassza ki **új erőforrás hozzáadása**.

    ![JSON-vázlat ablak](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. A hello **erőforrás hozzáadása** párbeszédpanel, keresése és kijelölése **logikai alkalmazás**. A logikai alkalmazás neve, és válassza a **Hozzáadás**.

    ![Erőforrás hozzáadása](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Következő lépések

* [A Visual Studio Cloud Explorer logic Apps-alkalmazások kezelése](logic-apps-manage-from-vs.md)
* [Gyakori példák és felhasználási helyzetek megtekintése](logic-apps-examples-and-scenarios.md)
* [Ismerje meg, hogyan tooautomate üzleti dolgozza fel az Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Megtudhatja, hogyan toointegrate a rendszer az Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
