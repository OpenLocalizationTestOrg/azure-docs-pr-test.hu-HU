---
title: aaaAutomate Azure Application Insights a PowerShell-lel |} Microsoft Docs
description: "A PowerShell használata Azure Resource Manager-sablon létrehozása erőforrás, a riasztás és a rendelkezésre állási tesztek automatizálásához."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: bwren
ms.openlocfilehash: ebd336eafba58a690a0e8ffbd1c74f7e93dbb682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="create-application-insights-resources-using-powershell"></a><span data-ttu-id="39b61-103">Hozzon létre egy Application Insights-erőforrást PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="39b61-103">Create Application Insights resources using PowerShell</span></span>
<span data-ttu-id="39b61-104">Ez a cikk bemutatja, hogyan tooautomate hello létrehozása és frissítése [Application Insights](app-insights-overview.md) erőforrások automatikusan az Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="39b61-104">This article shows you how tooautomate hello creation and update of [Application Insights](app-insights-overview.md) resources automatically by using Azure Resource Management.</span></span> <span data-ttu-id="39b61-105">Akkor lehet hasznos, például így a felépítési folyamat részeként.</span><span class="sxs-lookup"><span data-stu-id="39b61-105">You might, for example, do so as part of a build process.</span></span> <span data-ttu-id="39b61-106">Hello alapvető Application Insights-erőforrást, valamint létrehozhat [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md), állítsa be a következőt [riasztások](app-insights-alerts.md), beállíthatja hello [séma árképzési](app-insights-pricing.md), és az egyéb Azure létrehozása erőforrások.</span><span class="sxs-lookup"><span data-stu-id="39b61-106">Along with hello basic Application Insights resource, you can create [availability web tests](app-insights-monitor-web-app-availability.md), set up [alerts](app-insights-alerts.md), set hello [pricing scheme](app-insights-pricing.md), and create other Azure resources.</span></span>

<span data-ttu-id="39b61-107">Ezeket az erőforrásokat JSON-sablonokat kulcs toocreating hello [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="39b61-107">hello key toocreating these resources is JSON templates for [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md).</span></span> <span data-ttu-id="39b61-108">A ez már a vég, hello eljárás van: a meglévő erőforrások; hello JSON-definíciók letöltése egyes értékek nevét; például parametrizálja majd futtassa a hello sablon, ha azt szeretné, hogy egy új erőforrást toocreate.</span><span class="sxs-lookup"><span data-stu-id="39b61-108">In a nutshell, hello procedure is: download hello JSON definitions of existing resources; parameterize certain values such as names; and then run hello template whenever you want toocreate a new resource.</span></span> <span data-ttu-id="39b61-109">Egyszerre több erőforrás, azok minden együtt Ugrás – például toocreate, egy alkalmazás figyelő rendelkezésreállás figyelésére szolgáló tesztek, értesítések és a folyamatos exportálás-tároló csomagot.</span><span class="sxs-lookup"><span data-stu-id="39b61-109">You can package several resources together, toocreate them all in one go - for example, an app monitor with availability tests, alerts, and storage for continuous export.</span></span> <span data-ttu-id="39b61-110">Nincsenek néhány subtleties toosome hello parameterizations, amely azt ismertetjük, itt a.</span><span class="sxs-lookup"><span data-stu-id="39b61-110">There are some subtleties toosome of hello parameterizations, which we'll explain here.</span></span>

## <a name="one-time-setup"></a><span data-ttu-id="39b61-111">Egyszeri beállítása</span><span class="sxs-lookup"><span data-stu-id="39b61-111">One-time setup</span></span>
<span data-ttu-id="39b61-112">Ha még nem használta PowerShell Azure-előfizetéséhez előtt:</span><span class="sxs-lookup"><span data-stu-id="39b61-112">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="39b61-113">Hello Azure Powershell modul telepítése toorun hello parancsfájlok, ahová hello gépen:</span><span class="sxs-lookup"><span data-stu-id="39b61-113">Install hello Azure Powershell module on hello machine where you want toorun hello scripts:</span></span>

1. <span data-ttu-id="39b61-114">Telepítés [Microsoft Webplatform-telepítővel (v5 vagy magasabb)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="39b61-114">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
2. <span data-ttu-id="39b61-115">A Microsoft Azure Powershell tooinstall használni.</span><span class="sxs-lookup"><span data-stu-id="39b61-115">Use it tooinstall Microsoft Azure Powershell.</span></span>

## <a name="create-an-azure-resource-manager-template"></a><span data-ttu-id="39b61-116">Az Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="39b61-116">Create an Azure Resource Manager template</span></span>
<span data-ttu-id="39b61-117">Hozzon létre egy új .JSON kiterjesztésű fájlt – tegyük neki `template1.json` ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="39b61-117">Create a new .json file - let's call it `template1.json` in this example.</span></span> <span data-ttu-id="39b61-118">A tartalom másolása be:</span><span class="sxs-lookup"><span data-stu-id="39b61-118">Copy this content into it:</span></span>

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter hello application name."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "HockeyAppBridge",
                    "other"
                ],
                "metadata": {
                    "description": "Enter hello application type."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "East US",
                "allowedValues": [
                    "South Central US",
                    "West Europe",
                    "East US",
                    "North Europe"
                ],
                "metadata": {
                    "description": "Enter hello application location."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "1 = Basic, 2 = Enterprise"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 24,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 too23). Values outside hello range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter hello % value of daily quota after which warning mail toobe sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```



## <a name="create-application-insights-resources"></a><span data-ttu-id="39b61-119">Application Insights-erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="39b61-119">Create Application Insights resources</span></span>
1. <span data-ttu-id="39b61-120">PowerShell, a bejelentkezés tooAzure:</span><span class="sxs-lookup"><span data-stu-id="39b61-120">In PowerShell, sign in tooAzure:</span></span>
   
    `Login-AzureRmAccount`
2. <span data-ttu-id="39b61-121">Ehhez hasonló parancs futtatása:</span><span class="sxs-lookup"><span data-stu-id="39b61-121">Run a command like this:</span></span>
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * <span data-ttu-id="39b61-122">`-ResourceGroupName`hello csoport ahol van toocreate hello új erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="39b61-122">`-ResourceGroupName` is hello group where you want toocreate hello new resources.</span></span>
   * <span data-ttu-id="39b61-123">`-TemplateFile`hello egyéni paraméterek előfordulása esetén.</span><span class="sxs-lookup"><span data-stu-id="39b61-123">`-TemplateFile` must occur before hello custom parameters.</span></span>
   * <span data-ttu-id="39b61-124">`-appName`hello erőforrás toocreate hello neve.</span><span class="sxs-lookup"><span data-stu-id="39b61-124">`-appName` hello name of hello resource toocreate.</span></span>

<span data-ttu-id="39b61-125">Más paramétereket adhat hozzá, mert a leírásuk hello sablon hello paraméterek szakaszban találhat.</span><span class="sxs-lookup"><span data-stu-id="39b61-125">You can add other parameters - you'll find their descriptions in hello parameters section of hello template.</span></span>

## <a name="tooget-hello-instrumentation-key"></a><span data-ttu-id="39b61-126">tooget hello instrumentation kulcs</span><span class="sxs-lookup"><span data-stu-id="39b61-126">tooget hello instrumentation key</span></span>
<span data-ttu-id="39b61-127">Az alkalmazás erőforrás létrehozása után érdemes hello instrumentation kulcs:</span><span class="sxs-lookup"><span data-stu-id="39b61-127">After creating an application resource, you'll want hello instrumentation key:</span></span> 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-hello-price-plan"></a><span data-ttu-id="39b61-128">Set hello ár terv</span><span class="sxs-lookup"><span data-stu-id="39b61-128">Set hello price plan</span></span>

<span data-ttu-id="39b61-129">Beállíthatja a hello [ár terv](app-insights-pricing.md).</span><span class="sxs-lookup"><span data-stu-id="39b61-129">You can set hello [price plan](app-insights-pricing.md).</span></span>

<span data-ttu-id="39b61-130">egy alkalmazás-erőforrást hello vállalati ár csomaggal sablonnal hello fenti toocreate:</span><span class="sxs-lookup"><span data-stu-id="39b61-130">toocreate an app resource with hello Enterprise price plan, using hello template above:</span></span>

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|<span data-ttu-id="39b61-131">priceCode</span><span class="sxs-lookup"><span data-stu-id="39b61-131">priceCode</span></span>|<span data-ttu-id="39b61-132">Terv</span><span class="sxs-lookup"><span data-stu-id="39b61-132">plan</span></span>|
|---|---|
|<span data-ttu-id="39b61-133">1</span><span class="sxs-lookup"><span data-stu-id="39b61-133">1</span></span>|<span data-ttu-id="39b61-134">Basic</span><span class="sxs-lookup"><span data-stu-id="39b61-134">Basic</span></span>|
|<span data-ttu-id="39b61-135">2</span><span class="sxs-lookup"><span data-stu-id="39b61-135">2</span></span>|<span data-ttu-id="39b61-136">Enterprise</span><span class="sxs-lookup"><span data-stu-id="39b61-136">Enterprise</span></span>|

* <span data-ttu-id="39b61-137">Ha csak toouse hello alapértelmezett alapár terv, kihagyhatja hello CurrentBillingFeatures erőforrás hello sablonból.</span><span class="sxs-lookup"><span data-stu-id="39b61-137">If you only want toouse hello default Basic price plan, you can omit hello CurrentBillingFeatures resource from hello template.</span></span>
* <span data-ttu-id="39b61-138">Ha azt szeretné toochange hello ár terv hello összetevő erőforrás létrehozása után, használhatja a sablont, amely kihagyja a hello "microsoft.insights/components" erőforrás.</span><span class="sxs-lookup"><span data-stu-id="39b61-138">If you want toochange hello price plan after hello component resource has been created, you can use a template that omits hello "microsoft.insights/components" resource.</span></span> <span data-ttu-id="39b61-139">Emellett nincs megadva hello `dependsOn` erőforrás számlázási hello csomópontot.</span><span class="sxs-lookup"><span data-stu-id="39b61-139">Also, omit hello `dependsOn` node from hello billing resource.</span></span> 

<span data-ttu-id="39b61-140">tooverify hello frissített ár terv, nézze meg "Funkciók + díjszabás" hello panel hello böngészőben.</span><span class="sxs-lookup"><span data-stu-id="39b61-140">tooverify hello updated price plan, look at hello "Features+pricing" blade in hello browser.</span></span> <span data-ttu-id="39b61-141">**Hello böngésző nézet frissítése** toomake, hogy hello aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="39b61-141">**Refresh hello browser view** toomake sure you see hello latest state.</span></span>



## <a name="add-a-metric-alert"></a><span data-ttu-id="39b61-142">A metrika-riasztások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39b61-142">Add a metric alert</span></span>

<span data-ttu-id="39b61-143">tooset be a metrika riasztás a következő hello azonos idő, az alkalmazás-erőforrást, egyesítési kód ehhez hasonló hello sablon fájlba:</span><span class="sxs-lookup"><span data-stu-id="39b61-143">tooset up a metric alert at hello same time as your app resource, merge code like this into hello template file:</span></span>

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after hello app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

<span data-ttu-id="39b61-144">Ez a paraméter Tetszőlegesen felvehet hello sablon indításakor:</span><span class="sxs-lookup"><span data-stu-id="39b61-144">When you invoke hello template, you can optionally add this parameter:</span></span>

    `-responseTime 2`

<span data-ttu-id="39b61-145">Természetesen a más mezők lehet paraméterezni.</span><span class="sxs-lookup"><span data-stu-id="39b61-145">You can of course parameterize other fields.</span></span> 

<span data-ttu-id="39b61-146">hello típusnevek és más riasztási szabályok konfigurációs részleteit toofind manuálisan hozzon létre egy szabályt, és majd vizsgálja meg azt a [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="39b61-146">toofind out hello type names and configuration details of other alert rules, create a rule manually and then inspect it in [Azure Resource Manager](https://resources.azure.com/).</span></span> 


## <a name="add-an-availability-test"></a><span data-ttu-id="39b61-147">Elérhetőségi teszt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39b61-147">Add an availability test</span></span>

<span data-ttu-id="39b61-148">Ez a példa egy ping teszt (tootest egyoldalas) van.</span><span class="sxs-lookup"><span data-stu-id="39b61-148">This example is for a ping test (tootest a single page).</span></span>  

<span data-ttu-id="39b61-149">**Két részből áll** az elérhetőségi teszt: hello teszt magát, és hello riasztást, amely értesíti a felhasználót a hibákat.</span><span class="sxs-lookup"><span data-stu-id="39b61-149">**There are two parts** in an availability test: hello test itself, and hello alert that notifies you of failures.</span></span>

<span data-ttu-id="39b61-150">Hello kód fájlba hello sablon hello alkalmazást hoz létre a következő egyesítési.</span><span class="sxs-lookup"><span data-stu-id="39b61-150">Merge hello following code into hello template file that creates hello app.</span></span>

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures hello test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after hello app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies hello existence of hello specified text in hello response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, hello alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

<span data-ttu-id="39b61-151">toodiscover hello kódok más teszt helyét, vagy tooautomate hello létrehozása összetettebb webes tesztjeinek használatát, hozzon létre manuálisan egy példa, és majd parametrizálja hello kódot [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="39b61-151">toodiscover hello codes for other test locations, or tooautomate hello creation of more complex web tests, create an example manually and then parameterize hello code from [Azure Resource Manager](https://resources.azure.com/).</span></span>

## <a name="add-more-resources"></a><span data-ttu-id="39b61-152">További erőforrások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="39b61-152">Add more resources</span></span>

<span data-ttu-id="39b61-153">tooautomate hello létrehozása bármilyen típusú erőforrás hozzon létre egy példa manuálisan, majd másolja ki és a kód parametrizálja [Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="39b61-153">tooautomate hello creation of any other resource of any kind, create an example manually, and then copy and parameterize its code from [Azure Resource Manager](https://resources.azure.com/).</span></span> 

1. <span data-ttu-id="39b61-154">Nyissa meg [az Azure Resource Manager](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="39b61-154">Open [Azure Resource Manager](https://resources.azure.com/).</span></span> <span data-ttu-id="39b61-155">Lefelé navigálnak `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour alkalmazás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="39b61-155">Navigate down through `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour application resource.</span></span> 
   
    ![Az Azure erőforrás-kezelőben navigációs](./media/app-insights-powershell/01.png)
   
    <span data-ttu-id="39b61-157">*Összetevők* hello alapvető Application Insights-erőforrások, az alkalmazások vannak.</span><span class="sxs-lookup"><span data-stu-id="39b61-157">*Components* are hello basic Application Insights resources for displaying applications.</span></span> <span data-ttu-id="39b61-158">Nincsenek külön erőforrások hello társított riasztási szabályok és a rendelkezésre állási webes tesztjeinek használatát.</span><span class="sxs-lookup"><span data-stu-id="39b61-158">There are separate resources for hello associated alert rules and availability web tests.</span></span>
2. <span data-ttu-id="39b61-159">Másolás hello JSON hello összetevő hello megfelelő helyen lévő `template1.json`.</span><span class="sxs-lookup"><span data-stu-id="39b61-159">Copy hello JSON of hello component into hello appropriate place in `template1.json`.</span></span>
3. <span data-ttu-id="39b61-160">Törli ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="39b61-160">Delete these properties:</span></span>
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. <span data-ttu-id="39b61-161">Nyissa meg a hello webtests és alertrules szakaszok, és az egyes elemek JSON hello másolja a sablonba.</span><span class="sxs-lookup"><span data-stu-id="39b61-161">Open hello webtests and alertrules sections and copy hello JSON for individual items into your template.</span></span> <span data-ttu-id="39b61-162">(Nincs másolás hello webtests vagy alertrules csomópontból: Nyissa meg a hello elemek csoportokba tartozó.)</span><span class="sxs-lookup"><span data-stu-id="39b61-162">(Don't copy from hello webtests or alertrules nodes: go into hello items under them.)</span></span>
   
    <span data-ttu-id="39b61-163">Minden webalkalmazás-teszt tartozó riasztási szabály, rendelkezik, így a toocopy mindkét azok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="39b61-163">Each web test has an associated alert rule, so you have toocopy both of them.</span></span>
   
    <span data-ttu-id="39b61-164">Riasztások a metrikák is használható.</span><span class="sxs-lookup"><span data-stu-id="39b61-164">You can also include alerts on metrics.</span></span> <span data-ttu-id="39b61-165">[Metrika neve](app-insights-powershell-alerts.md#metric-names).</span><span class="sxs-lookup"><span data-stu-id="39b61-165">[Metric names](app-insights-powershell-alerts.md#metric-names).</span></span>
5. <span data-ttu-id="39b61-166">A sor beszúrása az egyes erőforrások:</span><span class="sxs-lookup"><span data-stu-id="39b61-166">Insert this line in each resource:</span></span>
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-hello-template"></a><span data-ttu-id="39b61-167">Hello sablon parametrizálja</span><span class="sxs-lookup"><span data-stu-id="39b61-167">Parameterize hello template</span></span>
<span data-ttu-id="39b61-168">Most már rendelkezik tooreplace hello egyedi nevek paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="39b61-168">Now you have tooreplace hello specific names with parameters.</span></span> <span data-ttu-id="39b61-169">túl[sablon parametrizálja](../azure-resource-manager/resource-group-authoring-templates.md), kifejezések használatával írhat egy [segítő funkciók](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="39b61-169">too[parameterize a template](../azure-resource-manager/resource-group-authoring-templates.md), you write expressions using a [set of helper functions](../azure-resource-manager/resource-group-template-functions.md).</span></span> 

<span data-ttu-id="39b61-170">Nem lehet paraméterezni csak részét, így `concat()` toobuild karakterláncok.</span><span class="sxs-lookup"><span data-stu-id="39b61-170">You can't parameterize just part of a string, so use `concat()` toobuild strings.</span></span>

<span data-ttu-id="39b61-171">Példa a hello helyettesítések érdemes toomake.</span><span class="sxs-lookup"><span data-stu-id="39b61-171">Here are examples of hello substitutions you'll want toomake.</span></span> <span data-ttu-id="39b61-172">Nincsenek minden helyettesítés többszöri felbukkanását.</span><span class="sxs-lookup"><span data-stu-id="39b61-172">There are several occurrences of each substitution.</span></span> <span data-ttu-id="39b61-173">Szükség lehet mások a sablonban.</span><span class="sxs-lookup"><span data-stu-id="39b61-173">You might need others in your template.</span></span> <span data-ttu-id="39b61-174">Ezekben a példákban hello paraméterek és változók meghatározott hello sablon hello tetején.</span><span class="sxs-lookup"><span data-stu-id="39b61-174">These examples use hello parameters and variables we defined at hello top of hello template.</span></span>

| <span data-ttu-id="39b61-175">Keresés</span><span class="sxs-lookup"><span data-stu-id="39b61-175">find</span></span> | <span data-ttu-id="39b61-176">cserélje le</span><span class="sxs-lookup"><span data-stu-id="39b61-176">replace with</span></span> |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| <span data-ttu-id="39b61-177">`"myappname"`(nagybetűs)</span><span class="sxs-lookup"><span data-stu-id="39b61-177">`"myappname"` (lower case)</span></span> |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/><span data-ttu-id="39b61-178">Törölje a GUID azonosítót és azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="39b61-178">Delete Guid and Id.</span></span> |

### <a name="set-dependencies-between-hello-resources"></a><span data-ttu-id="39b61-179">Hello erőforrások közötti függőségek beállítása</span><span class="sxs-lookup"><span data-stu-id="39b61-179">Set dependencies between hello resources</span></span>
<span data-ttu-id="39b61-180">Azure hello erőforrások szigorú sorrendben kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="39b61-180">Azure should set up hello resources in strict order.</span></span> <span data-ttu-id="39b61-181">toomake meg arról, hogy egy telepítés befejeződése előtt hello mellett kezdődik, adja hozzá a függőség sorokat:</span><span class="sxs-lookup"><span data-stu-id="39b61-181">toomake sure one setup completes before hello next begins, add dependency lines:</span></span>

* <span data-ttu-id="39b61-182">Hello rendelkezésre állási erőforrás teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="39b61-182">In hello availability test resource:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* <span data-ttu-id="39b61-183">A riasztás-erőforrást hello elérhetőségi teszt:</span><span class="sxs-lookup"><span data-stu-id="39b61-183">In hello alert resource for an availability test:</span></span>
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a><span data-ttu-id="39b61-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39b61-184">Next steps</span></span>
<span data-ttu-id="39b61-185">Más automatizálási cikkek:</span><span class="sxs-lookup"><span data-stu-id="39b61-185">Other automation articles:</span></span>

* <span data-ttu-id="39b61-186">[Az Application Insights-erőforrás létrehozása](app-insights-powershell-script-create-resource.md) -nélkül sablon használatával gyorsan elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="39b61-186">[Create an Application Insights resource](app-insights-powershell-script-create-resource.md) - quick method without using a template.</span></span>
* [<span data-ttu-id="39b61-187">Riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="39b61-187">Set up Alerts</span></span>](app-insights-powershell-alerts.md)
* [<span data-ttu-id="39b61-188">Webteszt létrehozása</span><span class="sxs-lookup"><span data-stu-id="39b61-188">Create web tests</span></span>](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [<span data-ttu-id="39b61-189">Azure Diagnostics tooApplication Insights küldése</span><span class="sxs-lookup"><span data-stu-id="39b61-189">Send Azure Diagnostics tooApplication Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="39b61-190">A Githubból tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="39b61-190">Deploy tooAzure from GitHub</span></span>](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [<span data-ttu-id="39b61-191">Kiadási jegyzetek írására</span><span class="sxs-lookup"><span data-stu-id="39b61-191">Create release annotations</span></span>](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

