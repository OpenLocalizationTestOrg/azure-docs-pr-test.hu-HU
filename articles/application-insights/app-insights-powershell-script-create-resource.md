---
title: "Az Application Insights-erőforrás létrehozása a PowerShell parancsfájl |} Microsoft Docs"
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
ms.openlocfilehash: a828af9c7d207dd84cc626fc70206018fd67e2dd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="powershell-script-to-create-an-application-insights-resource"></a><span data-ttu-id="7da25-103">PowerShell-parancsfájl egy Application Insights-erőforrás létrehozásához</span><span class="sxs-lookup"><span data-stu-id="7da25-103">PowerShell script to create an Application Insights resource</span></span>


<span data-ttu-id="7da25-104">Ha figyelni kívánt új alkalmazás - vagy egy alkalmazás új verziójának – a [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), állít be egy új erőforrást a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7da25-104">When you want to monitor a new application - or a new version of an application - with [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), you set up a new resource in Microsoft Azure.</span></span> <span data-ttu-id="7da25-105">Ehhez az erőforráshoz, ahol az alkalmazásból a telemetriai adatok elemzése és jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7da25-105">This resource is where the telemetry data from your app is analyzed and displayed.</span></span> 

<span data-ttu-id="7da25-106">Egy új erőforrást a PowerShell segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="7da25-106">You can automate the creation of a new resource by using PowerShell.</span></span>

<span data-ttu-id="7da25-107">Például ha egy mobileszköz-alkalmazást fejleszt, valószínű, hogy tetszőleges időpontban lesz közzétett változatának ügyfelei az alábbiakra használhatják az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7da25-107">For example, if you are developing a mobile device app, it's likely that, at any time, there will be several published versions of your app in use by your customers.</span></span> <span data-ttu-id="7da25-108">Nem szeretnénk vegyes különböző verzióiból a telemetriai adatok eredményt.</span><span class="sxs-lookup"><span data-stu-id="7da25-108">You don't want to get the telemetry results from different versions mixed up.</span></span> <span data-ttu-id="7da25-109">Ezért az egyes buildekhez új erőforrás létrehozása a felépítési folyamat kap.</span><span class="sxs-lookup"><span data-stu-id="7da25-109">So you get your build process to create a new resource for each build.</span></span>

> [!NOTE]
> <span data-ttu-id="7da25-110">Ha szeretne létrehozni az erőforráscsoport összes egyszerre, fontolja meg a [létrehozása az Azure-sablon alapján erőforrások](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="7da25-110">If you want to create a set of resources all at the same time, consider [creating the resources using an Azure template](app-insights-powershell.md).</span></span>
> 
> 

## <a name="script-to-create-an-application-insights-resource"></a><span data-ttu-id="7da25-111">Hozzon létre egy Application Insights-erőforrást parancsprogram</span><span class="sxs-lookup"><span data-stu-id="7da25-111">Script to create an Application Insights resource</span></span>
<span data-ttu-id="7da25-112">Tekintse meg a megfelelő parancsmag specifikációk:</span><span class="sxs-lookup"><span data-stu-id="7da25-112">See the relevant cmdlet specs:</span></span>

* [<span data-ttu-id="7da25-113">Új AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="7da25-113">New-AzureRmResource</span></span>](https://msdn.microsoft.com/library/mt652510.aspx)
* [<span data-ttu-id="7da25-114">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="7da25-114">New-AzureRmRoleAssignment</span></span>](https://msdn.microsoft.com/library/mt678995.aspx)

<span data-ttu-id="7da25-115">*PowerShell-parancsfájl*</span><span class="sxs-lookup"><span data-stu-id="7da25-115">*PowerShell Script*</span></span>  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount / Login-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 

$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

# Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource


$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ applicationType = "web"; applicationName = $applicationTagName} `
  -ResourceType "Microsoft.Insights/components" `
  -Location "East US" `  # or North Europe, West Europe, South Central US
  -PropertyObject @{"Application_Type"="web"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


# Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a><span data-ttu-id="7da25-116">Mi a teendő, ha a iKey</span><span class="sxs-lookup"><span data-stu-id="7da25-116">What to do with the iKey</span></span>
<span data-ttu-id="7da25-117">Az egyes erőforrások azonosítja a rendszerállapot-kulcsok (iKey).</span><span class="sxs-lookup"><span data-stu-id="7da25-117">Each resource is identified by its instrumentation key (iKey).</span></span> <span data-ttu-id="7da25-118">A iKey az erőforrás-létrehozási parancsfájl kimenete.</span><span class="sxs-lookup"><span data-stu-id="7da25-118">The iKey is an output of the resource creation script.</span></span> <span data-ttu-id="7da25-119">A build script kell biztosítania az Application Insights SDK iKey az alkalmazásba ágyazott.</span><span class="sxs-lookup"><span data-stu-id="7da25-119">Your build script should provide the iKey to the Application Insights SDK embedded in your app.</span></span>

<span data-ttu-id="7da25-120">Két módon lehet elérhetővé tenni a iKey az SDK-val:</span><span class="sxs-lookup"><span data-stu-id="7da25-120">There are two ways to make the iKey available to the SDK:</span></span>

* <span data-ttu-id="7da25-121">A [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span><span class="sxs-lookup"><span data-stu-id="7da25-121">In [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md):</span></span> 
  * <span data-ttu-id="7da25-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span><span class="sxs-lookup"><span data-stu-id="7da25-122">`<instrumentationkey>`*ikey*`</instrumentationkey>`</span></span>
* <span data-ttu-id="7da25-123">Vagy a [inicializálási kód](app-insights-api-custom-events-metrics.md):</span><span class="sxs-lookup"><span data-stu-id="7da25-123">Or in [initialization code](app-insights-api-custom-events-metrics.md):</span></span> 
  * <span data-ttu-id="7da25-124">`Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span><span class="sxs-lookup"><span data-stu-id="7da25-124">`Microsoft.ApplicationInsights.Extensibility.
TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`</span></span>

## <a name="see-also"></a><span data-ttu-id="7da25-125">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7da25-125">See also</span></span>
* [<span data-ttu-id="7da25-126">Az Application Insights és webes teszt erőforrások létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="7da25-126">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="7da25-127">A PowerShell segítségével az Azure diagnosztikai figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="7da25-127">Set up monitoring of Azure diagnostics with PowerShell</span></span>](app-insights-powershell-azure-diagnostics.md) 
* [<span data-ttu-id="7da25-128">Értesítések beállítása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7da25-128">Set alerts by using PowerShell</span></span>](app-insights-powershell-alerts.md)

