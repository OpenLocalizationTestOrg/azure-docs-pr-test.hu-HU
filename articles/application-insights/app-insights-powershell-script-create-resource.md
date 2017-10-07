---
title: "aaaPowerShell parancsfájl toocreate Application Insights-erőforrás |} Microsoft Docs"
description: "Application Insights-erőforrások létrehozásának automatizálása."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: f0082c9b-43ad-4576-a417-4ea8e0daf3d9
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/19/2016
ms.author: bwren
ms.openlocfilehash: 2ac00376d38026d64c2c5deabfaca60588924510
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-script-toocreate-an-application-insights-resource"></a>PowerShell parancsfájl toocreate Application Insights-erőforrás


Ha azt szeretné toomonitor egy új alkalmazás - vagy egy alkalmazás új verziójának – a [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), állít be egy új erőforrást a Microsoft Azure-ban. Ehhez az erőforráshoz, ahol hello telemetriai adatokat a alkalmazás elemzése és jelenik meg. 

Hello egy új erőforráscsoport létrehozása a PowerShell segítségével automatizálható.

Például ha egy mobileszköz-alkalmazást fejleszt, valószínű, hogy tetszőleges időpontban lesz közzétett változatának ügyfelei az alábbiakra használhatják az alkalmazást. Nem szeretné, hogy tooget hello telemetriai eredményeinek vegyes különböző verziói. Így az összeállítási folyamat toocreate egy új erőforrást az egyes buildekhez.

> [!NOTE]
> Ha azt szeretné, toocreate minden az erőforráscsoport hello azonos időben, javasoljuk, hogy [létrehozása az Azure-sablonnal hello erőforrások](app-insights-powershell.md).
> 
> 

## <a name="script-toocreate-an-application-insights-resource"></a>Az Application Insights-erőforrást parancsprogram toocreate
Lásd: hello vonatkozó parancsmag specifikációk:

* [Új AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [New-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)

*PowerShell-parancsfájl*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before hello first 
# execution toologin toohello Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set hello name of hello Application Insights Resource

$appInsightsName = "TestApp"

# Set hello application name used for hello value of hello Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set hello name of hello Resource Group toouse.  
# Default is hello application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create hello Resource and Output hello name and iKey
###################################################

# Select hello azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create hello App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access toohello team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-toodo-with-hello-ikey"></a>Milyen toodo a hello iKey
Az egyes erőforrások azonosítja a rendszerállapot-kulcsok (iKey). hello iKey hello erőforrás létrehozása parancsfájl kimenete. A build script hello iKey toohello Application Insights SDK az alkalmazásba ágyazott kell biztosítania.

Két módon toomake hello iKey elérhető toohello SDK van:

* A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
  * `<instrumentationkey>`*ikey*`</instrumentationkey>`
* Vagy a [inicializálási kód](app-insights-api-custom-events-metrics.md): 
  * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`

## <a name="see-also"></a>Lásd még:
* [Az Application Insights és webes teszt erőforrások létrehozása sablonból](app-insights-powershell.md)
* [A PowerShell segítségével az Azure diagnosztikai figyelés beállítása](app-insights-powershell-azure-diagnostics.md) 
* [Értesítések beállítása a PowerShell használatával](app-insights-powershell-alerts.md)

