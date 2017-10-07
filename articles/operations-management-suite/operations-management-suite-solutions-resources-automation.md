---
title: "aaaAzure Automation erőforrások az OMS-megoldások |} Microsoft Docs"
description: "Az OMS megoldások rendszerint az Azure Automation tooautomate folyamatoknak, mint például a összegyűjtése és figyelési adatok feldolgozása runbookok tartalmazza.  Ez a cikk ismerteti, hogyan tooinclude runbookok és a kapcsolódó erőforrások megoldásban."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 5281462e-f480-4e5e-9c19-022f36dce76d
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 82a156f89bf77ce25e52e5e4596261ec07a24dae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-azure-automation-resources-tooan-oms-management-solution-preview"></a>Azure Automation erőforrások tooan OMS felügyeleti megoldás (előzetes verzió) hozzáadása
> [!NOTE]
> Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak. Az alábbiakban semmilyen sémát tulajdonos toochange.   


[Az OMS megoldások](operations-management-suite-solutions.md) runbookok rendszerint tartalmazza az Azure Automation tooautomate folyamatoknak, mint például a összegyűjtése és figyelési adatok feldolgozása.  Ezenkívül toorunbooks, Automation-fiók tartalmaz erőforrásokat, például a változók és ütemezéseket, amely támogatja a használt hello runbookokat hello megoldásban.  Ez a cikk ismerteti, hogyan tooinclude runbookok és a kapcsolódó erőforrások megoldásban.

> [!NOTE]
> hello minták ebben a cikkben használható paraméterek és változók vagy kötelező vagy közös toomanagement megoldások és ismertetett [létrehozása kezelési megoldásai Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy már ismeri a következő információ hello.

- Hogyan túl[felügyeleti megoldás létrehozása](operations-management-suite-solutions-creating.md).
- hello szerkezete egy [megoldásfájlt](operations-management-suite-solutions-solution-file.md).
- Hogyan túl[Resource Manager-sablonok készítésének](../azure-resource-manager/resource-group-authoring-templates.md)

## <a name="automation-account"></a>Automation-fiók
Az Azure Automationben összes erőforrás található egy [Automation-fiók](../automation/automation-security-overview.md#automation-account-overview).  A [OMS munkaterületet, és az Automation-fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello Automation-fiók nem tartalmazza a hello felügyeleti megoldás, de már léteznie kell hello megoldás telepítve van.  Ha nem érhető el, majd hello megoldás telepítése sikertelen lesz.

az egyes Automation erőforrások hello neve hello az Automation-fiók nevét tartalmazza.  Hello hello megoldást ezt **accountName** paraméter a következő példa egy runbook erőforrás hello hasonlóan.

    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Runbookok
Tartalmaznia kell a runbookokat hello megoldás hello megoldásfájl használja, hogy hello megoldás telepítésekor jönnek létre.  Így kell közzé tenni hello runbook tooa nyilvános helyre ahol hozzáférhetők bármely felhasználó telepíti a megoldás azonban nem tartalmazhatja a hello hello runbook hello sablon törzsét.

[Azure Automation-runbook](../automation/automation-runbook-types.md) erőforrások típusa lehet **Microsoft.Automation/automationAccounts/runbooks** és a következő struktúra hello rendelkezik. Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 

    {
        "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
        "type": "Microsoft.Automation/automationAccounts/runbooks",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "location": "[parameters('regionId')]",
        "tags": { },
        "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
                "uri": "[variables('Runbook').Uri]",
                "version": [variables('Runbook').Version]"
            }
        }
    }


a következő táblázat hello runbookokat hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| a kötelező runbookType |Meghatározza a hello runbook hello típusait. <br><br> Parancsfájl - PowerShell-parancsfájl <br>PowerShell - PowerShell munkafolyamat <br> GraphPowerShell - grafikus PowerShell-parancsfájl forgatókönyv <br> GraphPowerShellWorkflow - grafikus PowerShell-munkafolyamati forgatókönyv |
| logProgress |Megadja, hogy [rekordok előrehaladás](../automation/automation-runbook-output-and-messages.md) hello runbook elő kell állítani. |
| logVerbose |Megadja, hogy [részletes rekordok](../automation/automation-runbook-output-and-messages.md) hello runbook elő kell állítani. |
| leírás |Hello runbook megadhat egy leírást. |
| publishContentLink |Hello hello runbook tartalmának megadása <br><br>URI - hello runbook tartalmának Uri toohello.  Ez lesz a PowerShell és a parancsfájl runbookok .ps1 fájl, és az exportált grafikus forgatókönyvnek fájl, a Graph-forgatókönyvek esetében.  <br> verzió - hello forgatókönyv saját nyomon követésére verzióját. |


## <a name="automation-jobs"></a>Automatizálási feladatok
Amikor elindít egy forgatókönyvet az Azure Automationben, létrehoz egy automation-feladat.  Hozzáadhat egy automatizálási feladat erőforrás tooyour megoldás tooautomatically start egy runbook hello felügyeleti megoldás telepítésekor.  Ez a metódus általánosan használt toostart runbookokat hello megoldás kezdeti konfiguráció használt.  toostart egy runbook rendszeres időközönként, hozzon létre egy [ütemezés](#schedules) és egy [feladat ütemezése](#job-schedules)

Feladat erőforrások típusa lehet **Microsoft.Automation/automationAccounts/jobs** és a következő struktúra hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 

    {
      "name": "[concat(parameters('accountName'), '/', parameters('Runbook').JobGuid)]",
      "type": "Microsoft.Automation/automationAccounts/jobs",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
      ],
      "tags": { },
      "properties": {
        "runbook": {
          "name": "[variables('Runbook').Name]"
        },
        "parameters": {
          "Parameter1": "[[variables('Runbook').Parameter1]",
          "Parameter2": "[[variables('Runbook').Parameter2]"
        }
      }
    }

az automatizálási feladatok hello tulajdonságok hello a következő táblázat ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| a runbook |Egyetlen entitás hello runbook toostart hello nevét. |
| paraméterek |Az entitás minden egyes hello runbook által igényelt paraméterérték. |

hello feladat hello runbook neve és a paramétert értékek toobe toohello runbook küldött tartalmaz.  hello feladatot kell [függő](operations-management-suite-solutions-solution-file.md#resources) hello runbook óta hello runbook indítása feladat hello előtt létre kell hozni.  Ha több runbook el kell indítani, megadhatja a sorrendjük azzal, hogy egy feladat minden más, fusson első feladat függ.

egy feladat erőforrás hello nevének tartalmaznia kell egy GUID, amely általában egy paraméter által hozzárendelt.  További a GUID-paraméterekkel kapcsolatos [megoldások létrehozása az Operations Management Suite (OMS)](operations-management-suite-solutions-solution-file.md#parameters).  


## <a name="certificates"></a>Tanúsítványok
[Azure Automation-tanúsítványok](../automation/automation-certificates.md) típusú **Microsoft.Automation/automationAccounts/certificates** és a következő struktúra hello rendelkezik. Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
      "type": "Microsoft.Automation/automationAccounts/certificates",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "base64Value": "[variables('Certificate').Base64Value]",
        "thumbprint": "[variables('Certificate').Thumbprint]"
      }
    }



a következő táblázat hello tanúsítványok erőforrás hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| base64Value |Base 64 érték hello tanúsítvány. |
| ujjlenyomat |Hello tanúsítványának ujjlenyomata. |



## <a name="credentials"></a>Hitelesítő adatok
[Azure Automation hitelesítő adataival](../automation/automation-credentials.md) típusú **Microsoft.Automation/automationAccounts/credentials** és a következő struktúra hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 


    {
      "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
      "type": "Microsoft.Automation/automationAccounts/credentials",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "userName": "[parameters('credentialUsername')]",
        "password": "[parameters('credentialPassword')]"
      }
    }

a következő táblázat hello Credential erőforrás hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| Felhasználónév |Hello hitelesítő felhasználó neve. |
| jelszó |Jelszó az hello tartozó hitelesítő adatok. |


## <a name="schedules"></a>Ütemezések
[Azure Automation-ütemezések](../automation/automation-schedules.md) típusú **Microsoft.Automation/automationAccounts/schedules** , és a következő struktúra hello hello. Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
      "type": "microsoft.automation/automationAccounts/schedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Schedule').Description]",
        "startTime": "[parameters('scheduleStartTime')]",
        "timeZone": "[parameters('scheduleTimeZone')]",
        "isEnabled": "[variables('Schedule').IsEnabled]",
        "interval": "[variables('Schedule').Interval]",
        "frequency": "[variables('Schedule').Frequency]"
      }
    }

a következő táblázat hello ütemezés erőforrás hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| leírás |Hello ütemezés megadhat egy leírást. |
| startTime |Adja meg az ütemezés kezdő időpontjának hello DateTime objektumként. Egy karakterlánc is megadható, átalakított tooa használható, ha érvényes a DateTime típusú érték. |
| IsEnabled |Meghatározza, hogy engedélyezve van-e a hello ütemezés. |
| interval |hello típusa hello ütemezés intervallumát.<br><br>nap<br>óra |
| frequency |Ütemezés hello gyakorisága a napok vagy órák száma kell elindítani. |

A kezdő időpont hello nagyobb értéket az aktuális idő ütemezések.  Nem adja meg ezt az értéket egy változóhoz, mert semmilyen módon nem tudhatja, hogy telepítve, amelyet toobe kellene lennie.

Hello ütemezés erőforrások a megoldás használata esetén a következő két stratégiák egyikét használhatja.

- A paraméter használható hello hello ütemezés kezdési idejét.  Hello megoldás telepítésekor ez fogja kérni hello felhasználói tooprovide értéket.  Ha egyszerre több ütemezés, használhat egy egyetlen paraméterérték egynél több.
- Hello ütemezések használatával, amely akkor kezdődik, amikor hello megoldás van telepítve runbook létrehozása.  Ezzel megszünteti a hello követelményt, de nem tartalmazhat hello felhasználói toospecify hello ütemezése a megoldásban, ezért el lesz távolítva, hello megoldás eltávolításakor.


### <a name="job-schedules"></a>Feladatütemezések
Feladat ütemezése erőforrások ütemezés runbookokhoz hivatkozásra.  Olyan típusú rendelkeznek **Microsoft.Automation/automationAccounts/jobSchedules** és a következő struktúra hello hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 

    {
      "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
      "type": "microsoft.automation/automationAccounts/jobSchedules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "location": "[parameters('regionId')]",
      "dependsOn": [
        "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
        "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
      ],
      "tags": {
      },
      "properties": {
        "schedule": {
          "name": "[variables('Schedule').Name]"
        },
        "runbook": {
          "name": "[variables('Runbook').Name]"
        }
      }
    }


hello a következő táblázat ismerteti a feladatok ütemezésének hello tulajdonságait.

| Tulajdonság | Leírás |
|:--- |:--- |
| az ütemezés nevének |Egyetlen **neve** hello ütemterv hello nevű entitás. |
| Runbook neve  |Egyetlen **neve** hello runbook hello nevű entitás.  |



## <a name="variables"></a>Változók
[Azure Automation-változók](../automation/automation-variables.md) típusú **Microsoft.Automation/automationAccounts/variables** és a következő struktúra hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
      "type": "microsoft.automation/automationAccounts/variables",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "tags": { },
      "dependsOn": [
      ],
      "properties": {
        "description": "[variables('Variable').Description]",
        "isEncrypted": "[variables('Variable').Encrypted]",
        "type": "[variables('Variable').Type]",
        "value": "[variables('Variable').Value]"
      }
    }

a következő táblázat hello változó erőforrás hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| leírás | Hello változó megadhat egy leírást. |
| isEncrypted | Megadja a hello változó titkosítani kell-e. |
| type | Ezt a tulajdonságot jelenleg nincs hatása.  hello adattípus hello változó hello kezdeti érték határozza meg. |
| érték | Hello változó értékét. |

> [!NOTE]
> Hello **típus** tulajdonság nincs hatással a hello változó létrehozása folyamatban van.  hello változó adattípust hello hello érték határozza meg.  

Ha hello kezdeti hello változó értékét, akkor be kell állítani hello megfelelő adattípusú értékként.  a következő táblázat hello hello különböző adattípusokkal engedélyezett és a szintaxis biztosít.  Vegye figyelembe, hogy a JSON-ban értékei várt tooalways hello ajánlatok belül a különleges karaktereket az idézőjelek.  Például egy olyan karakterláncértéket szeretné megadni hello karakterlánc idézőjelbe (hello escape-karakter használatával (\\)) egy numerikus érték lenne megadva, az idézőjelek közé foglalt egy készletével.

| Adattípus | Leírás | Példa | Oldja fel a túl|
|:--|:--|:--|:--|
| Karakterlánc   | Érték tegye idézőjelek közé foglalt.  | "\"Hello world\"" | "Hello world" |
| Numerikus  | Szimpla idézőjelben numerikus értéket.| "64" | 64 |
| Logikai érték  | **Igaz** vagy **hamis** idézőjelben.  Vegye figyelembe, hogy ez az érték kisbetűnek kell lennie. | "true" | Igaz |
| Dátum és idő | A szerializált dátumértéket.<br>Használható hello ConvertTo-Json parancsmag PowerShell toogenerate ezt az értéket egy adott dátumot.<br>Példa: get-date "5/24/2017 13:14:57" \| ConvertTo-Json | "\\/Date(1495656897378)\\/" | 2017-05-24 13:14:57 |

## <a name="modules"></a>Modulok
A felügyeleti megoldás nem kell toodefine [globális modulok](../automation/automation-integration-modules.md) a runbookok által használt, mert azok mindig elérhető lesz az Automation-fiók.  Bármely más, a runbookok által használt modul tooinclude erőforrás szükséges.

[Integrációs modulok](../automation/automation-integration-modules.md) típusú **Microsoft.Automation/automationAccounts/modules** és a következő struktúra hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei.

    {
      "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
      "type": "Microsoft.Automation/automationAccounts/modules",
      "apiVersion": "[variables('AutomationApiVersion')]",
      "dependsOn": [
      ],
      "properties": {
        "contentLink": {
          "uri": "[variables('Module').Uri]"
        }
      }
    }


a következő táblázat hello modul erőforrás hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| contentLink |Megadja a hello modul hello tartalmát. <br><br>URI - hello modul tartalmának Uri toohello.  Ez lesz a PowerShell és a parancsfájl runbookok .ps1 fájl, és az exportált grafikus forgatókönyvnek fájl, a Graph-forgatókönyvek esetében.  <br> verzió - hello modul a saját követési verzióját. |

hello runbook hello modul erőforrás tooensure előtt hello runbook létrehozva függ.

### <a name="updating-modules"></a>Modulok frissítése
Ha egy felügyeleti megoldás, amely tartalmazza a runbook által használt ütemezés szerint frissíti, és a megoldás hello új verziója, hogy a runbook által használt új modul, hello runbook hello modul régi verziója hello használhat.  A megoldás forgatókönyve a következő hello tartalmazza, és hozzon létre egy feladat toorun őket, mielőtt más runbookokat.  Ezzel biztosíthatja, hogy a modul előtt szükséges hello runbookok be van töltve, frissülnek.

* [Frissítés-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) biztosítja, hogy a megoldás a runbookok által használt hello modulok összes hello legújabb verziójára.  
* [ReRegisterAutomationSchedule-MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) újraregisztrálása összes hello ütemezés erőforrások tooensure, hogy a runbookokat hello használata hello legújabb modulok toothem társítva.




## <a name="sample"></a>Minta
Az alábbiakban látható egy minta a megoldás, amely tartalmazza, amely tartalmazza a következő erőforrások hello:

- Runbook.  Ez a példa runbook egy nyilvános GitHub-tárházban tárolt.
- Automation-feladat, amely hello runbook kezdődik, amikor hello megoldás telepítve van.
- Az ütemezés és a feladat ütemezése toostart hello runbook rendszeres időközönként.
- Tanúsítvány.
- Hitelesítő adatok.
- Változó.
- A modul.  Ez a hello [OMSIngestionAPI modul](https://www.powershellgallery.com/packages/OMSIngestionAPI/1.5) írás Analytics adatok tooLog. 

minta használ hello [szokásos megoldást paraméterek](operations-management-suite-solutions-solution-file.md#parameters) változókat, amelyek a megoldás, mint a gyakran használni ellenezte toohardcoding értékek hello erőforrás-definíciókban.


    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "workspaceName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Log Analytics workspace."
          }
        },
        "accountName": {
          "type": "string",
          "metadata": {
            "Description": "Name of Automation account."
          }
        },
        "workspaceregionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Log Analytics workspace."
          }
        },
        "regionId": {
          "type": "string",
          "metadata": {
            "Description": "Region of Automation account."
          }
        },
        "pricingTier": {
          "type": "string",
          "metadata": {
            "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account."
          }
        },
        "certificateBase64Value": {
          "type": "string",
          "metadata": {
            "Description": "Base 64 value for certificate."
          }
        },
        "certificateThumbprint": {
          "type": "securestring",
          "metadata": {
            "Description": "Thumbprint for certificate."
          }
        },
        "credentialUsername": {
          "type": "string",
          "metadata": {
            "Description": "Username for credential."
          }
        },
        "credentialPassword": {
          "type": "securestring",
          "metadata": {
            "Description": "Password for credential."
          }
        },
        "scheduleStartTime": {
          "type": "string",
          "metadata": {
            "Description": "Start time for shedule."
          }
        },
        "scheduleTimeZone": {
          "type": "string",
          "metadata": {
            "Description": "Time zone for schedule."
          }
        },
        "scheduleLinkGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello schedule link toorunbook.",
            "control": "guid"
          }
        },
        "runbookJobGuid": {
          "type": "string",
          "metadata": {
            "description": "GUID for hello runbook job.",
            "control": "guid"
          }
        }
      },
      "variables": {
        "SolutionName": "MySolution",
        "SolutionVersion": "1.0",
        "SolutionPublisher": "Contoso",
        "ProductName": "SampleSolution",
    
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31",
    
        "Runbook": {
          "Name": "MyRunbook",
          "Description": "Sample runbook",
          "Type": "PowerShell",
          "Uri": "https://raw.githubusercontent.com/user/myrepo/master/samples/MyRunbook.ps1",
          "JobGuid": "[parameters('runbookJobGuid')]"
        },
    
        "Certificate": {
          "Name": "MyCertificate",
          "Base64Value": "[parameters('certificateBase64Value')]",
          "Thumbprint": "[parameters('certificateThumbprint')]"
        },
    
        "Credential": {
          "Name": "MyCredential",
          "UserName": "[parameters('credentialUsername')]",
          "Password": "[parameters('credentialPassword')]"
        },
    
        "Schedule": {
          "Name": "MySchedule",
          "Description": "Sample schedule",
          "IsEnabled": "true",
          "Interval": "1",
          "Frequency": "hour",
          "StartTime": "[parameters('scheduleStartTime')]",
          "TimeZone": "[parameters('scheduleTimeZone')]",
          "LinkGuid": "[parameters('scheduleLinkGuid')]"
        },
    
        "Variable": {
          "Name": "MyVariable",
          "Description": "Sample variable",
          "Encrypted": 0,
          "Type": "string",
          "Value": "'This is my string value.'"
        },
    
        "Module": {
          "Name": "OMSIngestionAPI",
          "Uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
        }
      },
      "resources": [
        {
          "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
          "location": "[parameters('workspaceRegionId')]",
          "tags": { },
          "type": "Microsoft.OperationsManagement/solutions",
          "apiVersion": "[variables('LogAnalyticsApiVersion')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
            "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
            "referencedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/modules/', parameters('accountName'), variables('Module').Name)]"
            ],
            "containedResources": [
              "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobs/', parameters('accountName'), variables('Runbook').JobGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/certificates/', parameters('accountName'), variables('Certificate').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/credentials/', parameters('accountName'), variables('Credential').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]",
              "[resourceId('Microsoft.Automation/automationAccounts/jobSchedules/', parameters('accountName'), variables('Schedule').LinkGuid)]",
              "[resourceId('Microsoft.Automation/automationAccounts/variables/', parameters('accountName'), variables('Variable').Name)]"
            ]
          },
          "plan": {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
            "Version": "[variables('SolutionVersion')]",
            "product": "[variables('ProductName')]",
            "publisher": "[variables('SolutionPublisher')]",
            "promotionCode": ""
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').Name)]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "[variables('Runbook').Type]",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('Runbook').Description]",
            "publishContentLink": {
              "uri": "[variables('Runbook').Uri]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Runbook').JobGuid)]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('Runbook').Name)]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('Runbook').Name]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Certificate').Name)]",
          "type": "Microsoft.Automation/automationAccounts/certificates",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "Base64Value": "[variables('Certificate').Base64Value]",
            "Thumbprint": "[variables('Certificate').Thumbprint]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Credential').Name)]",
          "type": "Microsoft.Automation/automationAccounts/credentials",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "userName": "[variables('Credential').UserName]",
            "password": "[variables('Credential').Password]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').Name)]",
          "type": "microsoft.automation/automationAccounts/schedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Schedule').Description]",
            "startTime": "[variables('Schedule').StartTime]",
            "timeZone": "[variables('Schedule').TimeZone]",
            "isEnabled": "[variables('Schedule').IsEnabled]",
            "interval": "[variables('Schedule').Interval]",
            "frequency": "[variables('Schedule').Frequency]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Schedule').LinkGuid)]",
          "type": "microsoft.automation/automationAccounts/jobSchedules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[resourceId('Microsoft.Automation/automationAccounts/runbooks/', parameters('accountName'), variables('Runbook').Name)]",
            "[resourceId('Microsoft.Automation/automationAccounts/schedules/', parameters('accountName'), variables('Schedule').Name)]"
          ],
          "tags": {
          },
          "properties": {
            "schedule": {
              "name": "[variables('Schedule').Name]"
            },
            "runbook": {
              "name": "[variables('Runbook').Name]"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Variable').Name)]",
          "type": "microsoft.automation/automationAccounts/variables",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "tags": { },
          "dependsOn": [
          ],
          "properties": {
            "description": "[variables('Variable').Description]",
            "isEncrypted": "[variables('Variable').Encrypted]",
            "type": "[variables('Variable').Type]",
            "value": "[variables('Variable').Value]"
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('Module').Name)]",
          "type": "Microsoft.Automation/automationAccounts/modules",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "properties": {
            "contentLink": {
              "uri": "[variables('Module').Uri]"
            }
          }
        }
    
      ],
      "outputs": { }
    }




## <a name="next-steps"></a>Következő lépések
* [Nézet tooyour megoldás hozzáadása](operations-management-suite-solutions-resources-views.md) toovisualize összegyűjtött adatokat.
