---
title: "az Operations Management Suite (OMS) megoldások aaaCreating |} Microsoft Docs"
description: "Megoldások bővíthetők hello Operations Management Suite (OMS), adja meg a csomagolt felügyeleti lehetőségeket, hogy az ügyfelek tootheir OMS-munkaterület adhat hozzá.  Ez a cikk ismerteti, hogyan hozhat létre felügyeleti megoldások toobe részleteinek a saját környezetben használt, illetve mikor elérhető tooyour ügyfelek."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f408df1b21f519fd1eb2cbeb19cca18f6c4161f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a>A felügyeleti megoldás fájl létrehozása az Operations Management Suite (OMS) (előzetes verzió)
> [!NOTE]
> Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak. Az alábbiakban semmilyen sémát tulajdonos toochange.  

Az Operations Management Suite (OMS) megoldások használják, mint [Resource Manager-sablonok](../azure-resource-manager/resource-manager-template-walkthrough.md).  Hogyan tooauthor megoldások hogyan túl tanulási tanulási fő feladat hello[sablon szerzői](../azure-resource-manager/resource-group-authoring-templates.md).  Ez a cikk részletesen bemutatja a egyedi használt megoldások sablonjainak és hogyan tooconfigure szokásos megoldás erőforrásokat.


## <a name="tools"></a>Eszközök

Bármely szöveg szerkesztő toowork megoldásfájlok használható, de azt javasoljuk, ami hello szolgáltatásai a Visual Studio vagy Visual Studio Code hello a következő cikkekben ismertetett módon.

- [Létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [A Visual Studio Code Azure Resource Manager-sablonok használata](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a>struktúra
hello alapszintű struktúrát egy felügyeleti megoldás fájl van hello ugyanaz, mint egy [Resource Manager-sablon](../azure-resource-manager/resource-group-authoring-templates.md#template-format) Ez az alábbiak szerint.  Egyes hello lentebbi hello legfelső szintű elemeket ismerteti és és azok tartalmát, a megoldás.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Paraméterek
[Paraméterek](../azure-resource-manager/resource-group-authoring-templates.md#parameters) értékek, amelyekre szüksége van a hello felhasználó hello felügyeleti megoldás telepítésekor.  Standard paramétert, amely minden megoldás fog rendelkezni, és az adott megoldáshoz szükség szerint további paramétereket is hozzáadhat.  Hogyan felhasználók nyújtják a paraméterértékek való telepítésekor a megoldás hello adott paraméter és hello megoldás telepítési módjának függ.

Amikor a felhasználó telepíti a felügyeleti megoldás keresztül hello [Azure piactér](operations-management-suite-solutions.md#finding-and-installing-management-solutions) vagy [Azure gyors üzembe helyezési sablonokat](operations-management-suite-solutions.md#finding-and-installing-management-solutions) rákérdezéses tooselect egy [OMS munkaterületet, és az Automation-fiók ](operations-management-suite-solutions.md#oms-workspace-and-automation-account).  Ezek a használt toopopulate hello értékek egyes hello szabványos paraméterek.  hello nem felhasználótól toodirectly hello szabványos paraméterek értékének megadására, ugyanakkor felszólító tooprovide semmilyen további paraméterek értékeit.

Ha hello felhasználó telepíti a megoldás [egy másik módszer](operations-management-suite-solutions.md#finding-and-installing-management-solutions), meg kell adniuk egy értéket az összes szabványos és az összes további paraméterei.

Az alábbiakban látható egy minta paraméter.  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

hello a következő táblázatban a paraméter hello attribútumait ismerteti.

| Attribútum | Leírás |
|:--- |:--- |
| type |Hello paraméter adattípusa. hello bemeneti vezérlő hello felhasználó számára megjelenített hello adatok típusától függ.<br><br>logikai érték - a legördülő listából<br>karakterlánc - szövegmező<br>int - szövegmező<br>SecureString - jelszó mező<br> |
| category |Nem kötelező kategória hello paraméter.  Az azonos kategóriába sorolhatók hello paraméterek. |
| vezérlő |További funkciók karakterlánc-paraméter.<br><br>datetime - Datetime vezérlő jelenik meg.<br>GUID - Guid-érték automatikusan jön létre, és hello paraméter nem jelenik meg. |
| leírás |Hello paraméter nem kötelező leírása.  Megjelenik az információkat a buborékban megjelenő következő toohello paraméter. |

### <a name="standard-parameters"></a>Szabványos paraméterek
hello alábbi táblázat az összes felügyeleti megoldások szabványos paramétereinek hello.  Ezek az értékek helyett adatkérés el a megoldás hello Azure piactér vagy gyorsindítási sablonok alapján telepítésekor hello felhasználó fel van töltve.  Ha hello megoldás telepítve van egy másik módszerrel hello felhasználói értékeket kell adnia a számukra.

> [!NOTE]
> hello felhasználói felületének hello Azure piactér és gyorsindítási sablonok által várt paraméterekkel hello paraméterneveknek hello táblában.  Ha különböző paraméternevek majd hello kéri a felhasználótól a számukra, és azok nem automatikusan tölti fel.
>
>

| Paraméter | Típus | Leírás |
|:--- |:--- |:--- |
| Fióknév |Karakterlánc |Azure Automation-fiók nevét. |
| pricingTier |Karakterlánc |A Naplóelemzési munkaterület- és Azure Automation-fiók tarifacsomagot. |
| regionId |Karakterlánc |Azure Automation-fiók hello területet. |
| Megoldás neve |Karakterlánc |Hello megoldás nevét.  Központi telepítése a megoldás gyorsindítási sablonok keresztül, majd meg kell határozni megoldás neve paraméterként, egy karakterlánc, ehelyett igénylő hello felhasználói toospecify egy definiálhat. |
| workspaceName |Karakterlánc |Napló Analytics munkaterület neve. |
| workspaceRegionId |Karakterlánc |Hello Naplóelemzési munkaterület területet. |


Az alábbiakban az hello szerkezete hello szabványos paraméterek másolja és illessze be a megoldásfájlt.  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of hello Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of hello Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


Tekintse meg a hello megoldás hello szintaxissal egyéb elemeinek értékei tooparameter **paraméter ("név" paraméternek)**.  Ha például tooaccess hello munkaterület nevét, szeretné használni **parameters('workspaceName')**

## <a name="variables"></a>Változók
[Változók](../azure-resource-manager/resource-group-authoring-templates.md#variables) hello többi hello felügyeleti megoldást használni kívánt érték.  Ezek az értékek nincsenek kitett toohello felhasználó hello megoldás is telepíthet.  Tervezett tooprovide hello Szerző rendelkező meg egy helyet, ahol kezelésére használható többször hello megoldás teljes értékek. El kell helyezni egy értékek adott tooyour megoldásba változók kódolása őket a hello megakadályozását toohard, **erőforrások** elemet.  Ez megkönnyíti a hello kód olvashatóbbá, és lehetővé teszi a tooeasily módosítani ezeket az értékeket az újabb verziókban.

Az alábbiakban látható egy példa egy **változók** elem megoldások használt általános paraméterekkel.

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Tekintse meg a toovariable értékek hello megoldással hello szintaxissal **változók ("változó neve")**.  Ha például tooaccess hello megoldás neve változó, szeretné használni **variables('SolutionName')**.

Azt is megadhatja, komplex változók értékeinek beállítja, hogy több.  Ezek akkor igazán hasznosak-kezelési megoldásokban ahol több különböző típusú erőforrások tulajdonság meghatározásakor.  Például sikerült átalakítása hello megoldás változók toohello következő fent látható.

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Ebben az esetben tekintse meg az toovariable értékek hello megoldással hello szintaxissal **variables('variable name').property**.  Ha például tooaccess hello megoldásnév változó, szeretné használni **variables('Solution'). Név**.

## <a name="resources"></a>Erőforrások
[Erőforrások](../azure-resource-manager/resource-group-authoring-templates.md#resources) meghatározása hello különböző erőforrások, amelyek a felügyeleti megoldás telepíteni és konfigurálni fog.  Ez lesz a legnagyobb hello és hello sablon legösszetettebb része.  Hello struktúra és a teljes leírását az erőforrás-elemek [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md#resources).  Különböző erőforrások, amelyek általában meghatározzák részletes leírást talál további cikkeit a jelen dokumentációban. 


### <a name="dependencies"></a>Függőségek
Hello **dependsOn** elemek megadja egy [függőségi](../azure-resource-manager/resource-group-define-dependencies.md) egy másik erőforrás.  Hello megoldás telepítésekor egy erőforrás nem jön létre, amíg az összes függősége létrejött.  A megoldás lehet például [runbookot](operations-management-suite-solutions-resources-automation.md#runbooks) használatával telepített egy [erőforrás feladat](operations-management-suite-solutions-resources-automation.md#automation-jobs).  hello feladat erőforrás lenne a függő hello runbook erőforrás toomake meg arról, hogy az adott hello runbook jön létre, mielőtt hello feladat jön létre.

### <a name="oms-workspace-and-automation-account"></a>OMS-munkaterület és Automation-fiók
Megoldások szükséges egy [OMS-munkaterület](../log-analytics/log-analytics-manage-access.md) toocontain nézetek és egy [Automation-fiók](../automation/automation-security-overview.md#automation-account-overview) toocontain runbookok és a kapcsolódó erőforrások.  Ezek hello hello megoldásban erőforrások jönnek létre, és nem lehet megadni, maga hello megoldásban előtt elérhetőnek kell lennie.  hello felhasználó fog [adjon meg egy munkaterület és a fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) amikor azok a megoldás üzembe helyezéséhez, de hello Szerző vegye figyelembe a következő pontok hello.

## <a name="solution-resource"></a>Megoldás erőforrás
Minden egyes megoldáshoz szükségesek egy erőforrás bejegyzés hello **erőforrások** elem, amely meghatározza a hello megoldás magát.  Ez lesz az olyan típusú **Microsoft.OperationsManagement/solutions** és a következő struktúra hello rendelkezik. Ez magában foglalja [szabványos paraméterek](#parameters) és [változók](#variables) , amelyek általánosan használt toodefine tulajdonságok hello megoldás.


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a>Függőségek
hello megoldás erőforrás rendelkeznie kell egy [függőségi](../azure-resource-manager/resource-group-define-dependencies.md) hello megoldás, mert azokat tooexist hello megoldás létrehozása előtt minden más erőforráson.  Ehhez egy bejegyzést az egyes erőforrások hozzáadása a hello **dependsOn** elemet.

### <a name="properties"></a>Tulajdonságok
hello megoldás erőforrás a következő táblázat hello hello tulajdonságokkal rendelkezik.  Ez magában foglalja a hello erőforrások hivatkozott és hello megoldás, amely meghatározza, hogyan hello erőforrás felügyelt hello megoldás telepítése után által tartalmazott.  Az egyes erőforrások hello megoldásban szerepelnie kell vagy hello **referencedResources** vagy hello **containedResources** tulajdonság.

| Tulajdonság | Leírás |
|:--- |:--- |
| workspaceResourceId |Hello formában hello Naplóelemzési munkaterület azonosítója  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Munkaterületnevet\>*. |
| referencedResources |Az erőforrások listájához a hello megoldás, nem lehet eltávolítani, hello megoldás eltávolításakor. |
| containedResources |Az erőforrások listájához a hello megoldás, amely hello megoldás eltávolításakor el kell távolítani. |

hello fenti vonatkozik, amely runbook, egy ütemezést, és tekintse meg a megoldást.  hello ütemezés és a runbook *hivatkozott* a hello **tulajdonságok** elem, ezért nem eltávolítva hello megoldás eltávolításakor.  hello nézet *tartalmazott* , az hello megoldás eltávolításakor törlődik.

### <a name="plan"></a>Felkészülés
Hello **terv** hello megoldás erőforrás entitás hello tulajdonságokkal rendelkezik a következő táblázat hello.

| Tulajdonság | Leírás |
|:--- |:--- |
| név |Hello megoldás nevét. |
| Verzió |Hello megoldás hello Szerző alapján verziója. |
| A termék |Egyedi karakterlánc tooidentify hello megoldás. |
| Közzétevő |A Publisher hello megoldás. |



## <a name="sample"></a>Minta
Megoldásfájlok megoldás erőforrással mintát, az alábbi helyek hello tekintheti meg.

- [Automation-erőforrások](operations-management-suite-solutions-resources-automation.md#sample)
- [Keresés és a riasztás erőforrások](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a>Következő lépések
* [Adja hozzá a mentett keresések és riasztások](operations-management-suite-solutions-resources-searches-alerts.md) tooyour felügyeleti megoldás.
* [Nézetek hozzáadása](operations-management-suite-solutions-resources-views.md) tooyour felügyeleti megoldás.
* [Adja hozzá a runbookok és egyéb automatizálási erőforrások](operations-management-suite-solutions-resources-automation.md) tooyour felügyeleti megoldás.
* Ismerje meg, hello részleteit [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).
* Keresési [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/documentation/templates) példákért különböző Resource Manager-sablonok.
