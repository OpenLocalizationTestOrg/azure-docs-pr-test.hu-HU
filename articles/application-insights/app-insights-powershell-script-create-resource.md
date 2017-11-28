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
# <a name="powershell-script-toocreate-an-application-insights-resource"></a><span data-ttu-id="f86b0-103">PowerShell parancsfájl toocreate Application Insights-erőforrás</span><span class="sxs-lookup"><span data-stu-id="f86b0-103">PowerShell script toocreate an Application Insights resource</span></span>


<span data-ttu-id="f86b0-104">Ha azt szeretné toomonitor egy új alkalmazás - vagy egy alkalmazás új verziójának – a [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), állít be egy új erőforrást a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f86b0-104">When you want toomonitor a new application - or a new version of an application - with [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), you set up a new resource in Microsoft Azure.</span></span> <span data-ttu-id="f86b0-105">Ehhez az erőforráshoz, ahol hello telemetriai adatokat a alkalmazás elemzése és jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f86b0-105">This resource is where hello telemetry data from your app is analyzed and displayed.</span></span> 

<span data-ttu-id="f86b0-106">Hello egy új erőforráscsoport létrehozása a PowerShell segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="f86b0-106">You can automate hello creation of a new resource by using PowerShell.</span></span>

<span data-ttu-id="f86b0-107">Például ha egy mobileszköz-alkalmazást fejleszt, valószínű, hogy tetszőleges időpontban lesz közzétett változatának ügyfelei az alábbiakra használhatják az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f86b0-107">For example, if you are developing a mobile device app, it's likely that, at any time, there will be several published versions of your app in use by your customers.</span></span> <span data-ttu-id="f86b0-108">Nem szeretné, hogy tooget hello telemetriai eredményeinek vegyes különböző verziói.</span><span class="sxs-lookup"><span data-stu-id="f86b0-108">You don't want tooget hello telemetry results from different versions mixed up.</span></span> <span data-ttu-id="f86b0-109">Így az összeállítási folyamat toocreate egy új erőforrást az egyes buildekhez.</span><span class="sxs-lookup"><span data-stu-id="f86b0-109">So you get your build process toocreate a new resource for each build.</span></span>

> [!NOTE]
> <span data-ttu-id="f86b0-110">Ha azt szeretné, toocreate minden az erőforráscsoport hello azonos időben, javasoljuk, hogy [létrehozása az Azure-sablonnal hello erőforrások](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="f86b0-110">If you want toocreate a set of resources all at hello same time, consider [creating hello resources using an Azure template](app-insights-powershell.md).</span></span>
> 
> 

## <a name="script-toocreate-an-application-insights-resource"></a><span data-ttu-id="f86b0-111">Az Application Insights-erőforrást parancsprogram toocreate</span><span class="sxs-lookup"><span data-stu-id="f86b0-111">Script toocreate an Application Insights resource</span></span>
<span data-ttu-id="f86b0-112">Lásd: hello vonatkozó parancsmag specifikációk:</span><span class="sxs-lookup"><span data-stu-id="f86b0-112">See hello relevant cmdlet specs:</span></span>

* [<span data-ttu-id="f86b0-113">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="f86b0-113">New-AzureRmResource</span></span>](https://msdn.microsoft.com/library/mt652510.aspx)
* [<span data-ttu-id="f86b0-114">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="f86b0-114">New-AzureRmRoleAssignment</span></span>](https://msdn.microsoft.com/library/mt678995.aspx)

<span data-ttu-id="f86b0-115">*PowerShell-parancsfájl*</span><span class="sxs-lookup"><span data-stu-id="f86b0-115">*PowerShell Script*</span></span>  

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

## <a name="what-toodo-with-hello-ikey"></a><span data-ttu-id="f86b0-116">Milyen toodo a hello iKey</span><span class="sxs-lookup"><span data-stu-id="f86b0-116">What toodo with hello iKey</span></span>
<span data-ttu-id="f86b0-117">Az egyes erőforrások azonosítja a rendszerállapot-kulcsok (iKey).</span><span class="sxs-lookup"><span data-stu-id="f86b0-117">Each resource is identified by its instrumentation key (iKey).</span></span> <span data-ttu-id="f86b0-118">hello iKey hello erőforrás létrehozása parancsfájl kimenete.</span><span class="sxs-lookup"><span data-stu-id="f86b0-118">hello iKey is an output of hello resource creation script.</span></span> <span data-ttu-id="f86b0-119">A build script hello iKey toohello Application Insights SDK az alkalmazásba ágyazott kell biztosítania.</span><span class="sxs-lookup"><span data-stu-id="f86b0-119">Your build script should provide hello iKey toohello Application Insights SDK embedded in your app.</span></span>

<span data-ttu-id="f86b0-120">Két módon toomake hello iKey elérhető toohello SDK van:</span><span class="sxs-lookup"><span data-stu-id="f86b0-120">There are two ways toomake hello iKey available toohello SDK:</span></span>

* <span data-ttu-id="f86b0-121">A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="f86b0-121">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span> 
  * <span data-ttu-id="f86b0-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span><span class="sxs-lookup"><span data-stu-id="f86b0-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span></span>
* <span data-ttu-id="f86b0-123">Vagy a [inicializálási kód](app-insights-api-custom-events-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="f86b0-123">Or in [initialization code](app-insights-api-custom-events-metrics.md):</span></span> 
  * <span data-ttu-id="f86b0-124">`Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span><span class="sxs-lookup"><span data-stu-id="f86b0-124">`Microsoft.ApplicationInsights.Extensibility.
TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span></span>

## <a name="see-also"></a><span data-ttu-id="f86b0-125">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f86b0-125">See also</span></span>
* [<span data-ttu-id="f86b0-126">Az Application Insights és webes teszt erőforrások létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="f86b0-126">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="f86b0-127">A PowerShell segítségével az Azure diagnosztikai figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="f86b0-127">Set up monitoring of Azure diagnostics with PowerShell</span></span>](app-insights-powershell-azure-diagnostics.md) 
* [<span data-ttu-id="f86b0-128">Értesítések beállítása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f86b0-128">Set alerts by using PowerShell</span></span>](app-insights-powershell-alerts.md)

