---
title: "az Azure Logic Apps aaaCreate központi telepítési sablonok |} Microsoft Docs"
description: "A logic apps Azure Resource Manager sablonok üzembe helyezési és kiadás felügyeleti létrehozása"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 2f09445f10a376a745d6acbba94ca29d5f79fc09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-templates-for-logic-apps-deployment-and-release-management"></a>A logic apps-sablonok létrehozása központi telepítési és a kiadási felügyeleti

A logikai alkalmazás létrehozása után toocreate érdemes az Azure Resource Manager-sablonként.
Így könnyen telepíthet hello logic app tooany környezetben vagy az erőforráscsoport lehet szükség.
A Resource Manager-sablonok kapcsolatban bővebben lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) és [erőforrások telepítése Azure Resource Manager-sablonok segítségével](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Logikai alkalmazás központi telepítési sablont

A logikai alkalmazás három alapvető részből áll:

* **Logic app erőforrás**: többek között a terv, a hely és a munkafolyamat-definíciót hello díjszabási információkat tartalmaz.
* **Munkafolyamat-definíciót**: a Logic Apps alkalmazást munkafolyamat lépéseket, és hogyan hello Logic Apps motor végre kell hajtani hello munkafolyamat ismerteti.
Ez a definíció meg tudja tekinteni a logikai alkalmazás **kódnézetben** ablak.
Hello logic app erőforrás megtalálható ez a definíció hello `definition` tulajdonság.
* **Kapcsolatok**: biztonságosan tárolni a összekötő-kapcsolatokat, például egy kapcsolati karakterláncot és a hozzáférési token metaadatainak tooseparate erőforrások hivatkozik.
Hello logic app erőforrás, a logikai alkalmazás hivatkozik hello erőforrásainak `parameters` szakasz.

Egy eszköz, például a meglévő logic Apps alkalmazások az összes adatot megtekintheti [Azure erőforrás-kezelő](http://resources.azure.com).

a sablon toomake a logic app toouse az erőforrás-csoport központi telepítésével, a kell hello erőforrások meghatározása, és szükség szerint parametrizálja.
Például ha tooa fejlesztői, tesztelési és éles környezetben telepít, valószínűleg érdemes toouse különböző kapcsolati karakterláncok tooa SQL-adatbázis minden környezetben.
Vagy, érdemes toodeploy különböző előfizetésekhez vagy erőforráscsoportok belül.  

## <a name="create-a-logic-app-deployment-template"></a>Hozzon létre egy logic app központi telepítési sablont

hello legegyszerűbb módja toohave egy érvényes logikai alkalmazás központi telepítési sablont toouse a [Visual Studio eszközök a Logic Apps](logic-apps-deploy-from-vs.md).
hello Visual Studio eszközök, amelyek segítségével bármely előfizetés vagy a hely érvényes központi telepítési sablont hoz létre.

Néhány más eszközök is segítséget nyújt logic app központi telepítési sablont hoz létre.
Manuálisan is létrehozhat, ez azt jelenti, hogy hello segítségével erőforrások már az itt tárgyalt toocreate paraméterek igény szerint.
Másik lehetőség is toouse egy [logic app sablon létrehozó](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-modult. A nyílt forráskódú modul először kiértékeli hello logikai alkalmazás és a kapcsolatokat, hogy azt használja, és akkor hoz létre a központi telepítés hello szükséges paraméterekkel sablonerőforrás.
Például ha egy logikai alkalmazás, amely egy üzenetet kapott egy Azure Service Bus-üzenetsorba, és hozzáadja az tooan Azure SQL-adatbázist, hello eszköz megőrzi az összes hello vezénylési logika és parameterizes hello SQL és a Service Bus kapcsolati karakterláncokat, hogy azok állítható be a központi telepítés.

> [!NOTE]
> Kapcsolatok hello belül kell lennie erőforráscsoportjában hello logikai alkalmazást.
>
>

### <a name="install-hello-logic-app-template-powershell-module"></a>Hello logic app sablon PowerShell moduljának telepítése
hello legegyszerűbb módja tooinstall hello modul hello keresztül az [PowerShell-galériában](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), hello paranccsal `Install-Module -Name LogicAppTemplate`.  

Is telepíthet hello PowerShell modul manuálisan:

1. Töltse le a legújabb kiadásának hello hello [logic app sablon létrehozó](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
2. Bontsa ki a PowerShell modul mappa hello mappájába (általában `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Hello modul toowork a bérlők és az előfizetés elérését a token, azt javasoljuk, hogy hello használata [ARMClient](https://github.com/projectkudu/ARMClient) parancssori eszközt.  Ez [blogbejegyzés](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient ismerteti részletesen.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>A logic app-sablon létrehozása a PowerShell használatával
PowerShell telepítése után létrehozhat egy sablont hello a következő parancs használatával:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`hello Azure-előfizetés-azonosító. Először lekérdezi egy hozzáférési jogkivonat segítségével ARMClient, majd kiszolgálókészletéhez azt toohello PowerShell-parancsfájl használatával, és ezután hoz létre a hello sablon egy JSON-fájl.

## <a name="add-parameters-tooa-logic-app-template"></a>Paraméterek tooa logic app-sablon hozzáadása
A logic app-sablon létrehozása után továbbra is tooadd, vagy módosítsa a paramétereket, amelyek hasznosak lehetnek. Például ha a definíciója tartalmaz egy erőforrás-azonosító tooan Azure függvény vagy beágyazott munkafolyamatot, hogy egyetlen központi telepítés toodeploy tervezi, hozzáadhat további erőforrások tooyour sablon és parametrizálja azonosítók, igény szerint. hello Ugyanez vonatkozik tooany hivatkozások toocustom API-k vagy Swagger végpontot az egyes erőforráscsoportokban toodeploy várt.

## <a name="deploy-a-logic-app-template"></a>A logic app sablon üzembe helyezése

A sablon olyan eszközöket, például a PowerShell, a REST API segítségével telepíthet [Visual Studio Team Services Kiadáskezelés](#team-services), és a sablon-üzembehelyezés hello Azure-portálon keresztül.
Emellett toostore hello paraméter értékét, azt javasoljuk, hogy hozzon létre egy [paraméterfájl](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Ismerje meg, hogyan túl[erőforrások az Azure Resource Manager-sablonok és a PowerShell telepítése](../azure-resource-manager/resource-group-template-deploy.md) vagy [telepítése Azure Resource Manager-sablonok erőforrásokhoz és hello Azure-portálon](../azure-resource-manager/resource-group-template-deploy-portal.md).

### <a name="authorize-oauth-connections"></a>OAuth-kapcsolatok engedélyezése

A központi telepítést követően hello logikai alkalmazás-végpontok közötti érvényes paraméterekkel működik.
Azonban továbbra is engedélyeznie kell OAuth kapcsolatok toogenerate érvényes jogkivonat.
tooauthorize OAuth kapcsolatok, nyissa meg hello logikai alkalmazás hello Logic Apps Designer, és ezek a kapcsolatok engedélyezéséhez. Vagy az automatikus üzembe helyezés esetén is használhat egy parancsfájl tooconsent tooeach OAuth kapcsolat.
Nincs a githubon példa parancsfájl a [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projekt.

<a name="team-services"></a>
## <a name="visual-studio-team-services-release-management"></a>A Visual Studio Team Services Kiadáskezelés

Egy általános forgatókönyv központi telepítésére és felügyeletére környezet toouse egy eszköz, például a Visual Studio Team Services, a Kiadáskezelés a logic app a központi telepítési sablont. A Visual Studio Team Services tartalmaz egy [telepítése Azure erőforráscsoport](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) , hogy adhat hozzá tooany build vagy csővezeték kiadási tevékenység. Toohave van szüksége egy [egyszerű](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) engedélyezési toodeploy, és aztán hozhat létre a hello kiadás definíciója.

1. A Kiadáskezelés, válassza ki **üres** , hogy egy üres definíció létrehozása.

    ![Üres definíció létrehozása][1]

2. Válassza ki, hogy bármilyen, nagy valószínűséggel többek között a következőket hello logic app sablon manuálisan vagy hello részeként létrehozott szükséges erőforrások létrehozása folyamatban.
3. Adja hozzá egy **Azure erőforrás-csoport központi telepítésének** feladat.
4. Állítson be egy [egyszerű](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/), és hello sablon és a sablon paramétereinek fájlok hivatkozik.
5. Továbbra is el lépéseket toobuild hello kiadás folyamat környezet, automatikus teszt vagy jóváhagyóknak igény szerint.

<!-- Image References -->
[1]: ./media/logic-apps-create-deploy-template/emptyreleasedefinition.png
