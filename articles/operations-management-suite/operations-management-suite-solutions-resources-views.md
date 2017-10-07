---
title: "az Operations Management Suite (OMS) megoldások aaaViews |} Microsoft Docs"
description: "Az Operations Management Suite (OMS) megoldások általában egy vagy több nézetek toovisualize adatait tartalmazzák.  Ez a cikk ismerteti, hogyan tooexport nézet által létrehozott hello adatforrásnézet-tervezőből, és adja hozzá a megoldásra. "
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 303861465014a27289f831332b3d95925c0ae66d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Nézetek az Operations Management Suite (OMS) megoldások (előzetes verzió)
> [!NOTE]
> Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak. Az alábbiakban semmilyen sémát tulajdonos toochange.    
>
>

[Az Operations Management Suite (OMS) megoldások](operations-management-suite-solutions.md) általában egy vagy több nézetek toovisualize adatait tartalmazzák.  Ez a cikk ismerteti, hogyan tooexport nézet által létrehozott hello [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) adja hozzá a megoldásra.  

> [!NOTE]
> hello minták ebben a cikkben használható paraméterek és változók vagy kötelező vagy közös toomanagement megoldások és ismertetett [létrehozása kezelési megoldásai Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy most már tudja, hogyan túl[felügyeleti megoldás létrehozása](operations-management-suite-solutions-creating.md) és a megoldás fájl hello struktúrája.

## <a name="overview"></a>Áttekintés
tooinclude megoldásra nézet, hozzon létre egy **erőforrás** ki azt a hello [megoldásfájlt](operations-management-suite-solutions-creating.md).  hello hello nézet részletes konfigurációs leíró JSON pedig általában összetett, ha valami nem, hogy egy tipikus megoldás Szerző manuálisan lenne képes toocreate.  hello leggyakoribb módja toocreate hello nézet használatával hello [adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md), exportálni, és adja meg a részletes konfigurációs toohello megoldás.

hello lépéseken tooadd nézet tooa megoldás a következők:  Minden lépés az alábbi hello szakaszokban részletesen ismertetett.

1. Hello nézet tooa fájl exportálása.
2. Hozzon létre hello nézet erőforrás hello megoldás.
3. Adja hozzá a hello részleteinek megtekintése.

## <a name="export-hello-view-tooa-file"></a>Hello nézet tooa fájl exportálása
Hajtsa végre a hello található utasítások segítségével: [napló Analytics adatforrásnézet-tervezőből](../log-analytics/log-analytics-view-designer.md) tooexport nézet tooa fájl.  hello exportált fájlt fog kell JSON formátumban hello azonos [hello megoldásfájlt az elemek](operations-management-suite-solutions-solution-file.md).  

Hello **erőforrások** hello nézet fájl eleme lesz típussal rendelkező erőforrás **Microsoft.OperationalInsights/workspaces** , hogy jelöli hello OMS-munkaterület.  Ez az elem lesz a subelement típussal rendelkező **nézetek** , amely hello nézet jelöli, és a részletes kapcsolatos konfigurációját tartalmazza.  Másolja az ehhez az elemhez hello részleteit lesz, és átmásolja a megoldás.

## <a name="create-hello-view-resource-in-hello-solution"></a>Hello megoldásban hello nézet erőforrás létrehozása
Adja hozzá a következő nézet erőforrás toohello hello **erőforrások** a megoldásfájlt eleme.  Ez a változókat, amelyek az alábbiakban található, hogy hozzá kell adnia is használja.  Vegye figyelembe, hogy hello **irányítópult** és **OverviewTile** tulajdonságok jelölik hello vonatkozó tulajdonságok hello exportált nézet fájlból felülírja.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

Adja hozzá a következő változók toohello változók elem hello megoldásfájl hello, és cserélje le a hello értékek toothose megoldást.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of hello view."
    "ViewName": "Provide a name for hello view here."


Fontos megjegyezni, hogy hello teljes nézet erőforrás sikerült másolni az exportált nézet fájl kellene módosítások hozzá a következő toomake hello toowork a megoldásban.  

* Hello **típus** hello nézet erőforrás kell változása toobe **nézetek** túl**Microsoft.OperationalInsights/workspaces**.
* Hello **neve** hello nézet erőforráshoz tartozó kell módosítani toobe tooinclude hello munkaterület nevét.
* hello munkaterület hello függőség kell eltávolítani, mert hello munkaterület erőforrás nincs definiálva a hello megoldás toobe.
* **DisplayName** tulajdonság igények toobe hozzáadott toohello nézet.  Hello **azonosító**, **neve**, és **DisplayName** összes egyeznie kell.
* Meg kell változtatni a paraméterneveknek toomatch hello szükséges beállítása olyan paraméterek összessége.
* Változók hello megoldás definiált legyen, és megfelelő tulajdonságok hello szerepel.

## <a name="add-hello-view-details"></a>Adja hozzá a hello részleteinek megtekintése
hello nézet erőforrás hello az exportált fájlt fogja tartalmazni hello két elemét nézet **tulajdonságok** nevű elem **irányítópult** és **OverviewTile** hello tartalmazó részletes konfigurációs hello nézet.  Ez a két elem és a tartalom másolása hello **tulajdonságok** elem hello nézet erőforrás a megoldásfájlt.

## <a name="example"></a>Példa
A következő minta hello például egy egyszerű megoldást fájl nézetet jeleníti meg.  Folytatást jelző pontokra (...) jelennek meg a hello **irányítópult** és **OverviewTile** terület okokból tartalmát.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Következő lépések
* További részleteket létrehozásának [megoldások](operations-management-suite-solutions-creating.md).
* Tartalmaznak [a felügyeleti megoldás az automatizálási runbookok](operations-management-suite-solutions-resources-automation.md).
