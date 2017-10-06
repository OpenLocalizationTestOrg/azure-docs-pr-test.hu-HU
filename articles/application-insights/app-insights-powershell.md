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
#  <a name="create-application-insights-resources-using-powershell"></a>Hozzon létre egy Application Insights-erőforrást PowerShell használatával
Ez a cikk bemutatja, hogyan tooautomate hello létrehozása és frissítése [Application Insights](app-insights-overview.md) erőforrások automatikusan az Azure Resource Manager használatával. Akkor lehet hasznos, például így a felépítési folyamat részeként. Hello alapvető Application Insights-erőforrást, valamint létrehozhat [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md), állítsa be a következőt [riasztások](app-insights-alerts.md), beállíthatja hello [séma árképzési](app-insights-pricing.md), és az egyéb Azure létrehozása erőforrások.

Ezeket az erőforrásokat JSON-sablonokat kulcs toocreating hello [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md). A ez már a vég, hello eljárás van: a meglévő erőforrások; hello JSON-definíciók letöltése egyes értékek nevét; például parametrizálja majd futtassa a hello sablon, ha azt szeretné, hogy egy új erőforrást toocreate. Egyszerre több erőforrás, azok minden együtt Ugrás – például toocreate, egy alkalmazás figyelő rendelkezésreállás figyelésére szolgáló tesztek, értesítések és a folyamatos exportálás-tároló csomagot. Nincsenek néhány subtleties toosome hello parameterizations, amely azt ismertetjük, itt a.

## <a name="one-time-setup"></a>Egyszeri beállítása
Ha még nem használta PowerShell Azure-előfizetéséhez előtt:

Hello Azure Powershell modul telepítése toorun hello parancsfájlok, ahová hello gépen:

1. Telepítés [Microsoft Webplatform-telepítővel (v5 vagy magasabb)](http://www.microsoft.com/web/downloads/platform.aspx).
2. A Microsoft Azure Powershell tooinstall használni.

## <a name="create-an-azure-resource-manager-template"></a>Az Azure Resource Manager-sablon létrehozása
Hozzon létre egy új .JSON kiterjesztésű fájlt – tegyük neki `template1.json` ebben a példában. A tartalom másolása be:

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



## <a name="create-application-insights-resources"></a>Application Insights-erőforrások létrehozása
1. PowerShell, a bejelentkezés tooAzure:
   
    `Login-AzureRmAccount`
2. Ehhez hasonló parancs futtatása:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName`hello csoport ahol van toocreate hello új erőforrásokat.
   * `-TemplateFile`hello egyéni paraméterek előfordulása esetén.
   * `-appName`hello erőforrás toocreate hello neve.

Más paramétereket adhat hozzá, mert a leírásuk hello sablon hello paraméterek szakaszban találhat.

## <a name="tooget-hello-instrumentation-key"></a>tooget hello instrumentation kulcs
Az alkalmazás erőforrás létrehozása után érdemes hello instrumentation kulcs: 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-hello-price-plan"></a>Set hello ár terv

Beállíthatja a hello [ár terv](app-insights-pricing.md).

egy alkalmazás-erőforrást hello vállalati ár csomaggal sablonnal hello fenti toocreate:

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|Terv|
|---|---|
|1|Basic|
|2|Enterprise|

* Ha csak toouse hello alapértelmezett alapár terv, kihagyhatja hello CurrentBillingFeatures erőforrás hello sablonból.
* Ha azt szeretné toochange hello ár terv hello összetevő erőforrás létrehozása után, használhatja a sablont, amely kihagyja a hello "microsoft.insights/components" erőforrás. Emellett nincs megadva hello `dependsOn` erőforrás számlázási hello csomópontot. 

tooverify hello frissített ár terv, nézze meg "Funkciók + díjszabás" hello panel hello böngészőben. **Hello böngésző nézet frissítése** toomake, hogy hello aktuális állapotát.



## <a name="add-a-metric-alert"></a>A metrika-riasztások hozzáadása

tooset be a metrika riasztás a következő hello azonos idő, az alkalmazás-erőforrást, egyesítési kód ehhez hasonló hello sablon fájlba:

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

Ez a paraméter Tetszőlegesen felvehet hello sablon indításakor:

    `-responseTime 2`

Természetesen a más mezők lehet paraméterezni. 

hello típusnevek és más riasztási szabályok konfigurációs részleteit toofind manuálisan hozzon létre egy szabályt, és majd vizsgálja meg azt a [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Elérhetőségi teszt hozzáadása

Ez a példa egy ping teszt (tootest egyoldalas) van.  

**Két részből áll** az elérhetőségi teszt: hello teszt magát, és hello riasztást, amely értesíti a felhasználót a hibákat.

Hello kód fájlba hello sablon hello alkalmazást hoz létre a következő egyesítési.

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

toodiscover hello kódok más teszt helyét, vagy tooautomate hello létrehozása összetettebb webes tesztjeinek használatát, hozzon létre manuálisan egy példa, és majd parametrizálja hello kódot [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>További erőforrások hozzáadása

tooautomate hello létrehozása bármilyen típusú erőforrás hozzon létre egy példa manuálisan, majd másolja ki és a kód parametrizálja [Azure Resource Manager](https://resources.azure.com/). 

1. Nyissa meg [az Azure Resource Manager](https://resources.azure.com/). Lefelé navigálnak `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, tooyour alkalmazás erőforrás. 
   
    ![Az Azure erőforrás-kezelőben navigációs](./media/app-insights-powershell/01.png)
   
    *Összetevők* hello alapvető Application Insights-erőforrások, az alkalmazások vannak. Nincsenek külön erőforrások hello társított riasztási szabályok és a rendelkezésre állási webes tesztjeinek használatát.
2. Másolás hello JSON hello összetevő hello megfelelő helyen lévő `template1.json`.
3. Törli ezeket a tulajdonságokat:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Nyissa meg a hello webtests és alertrules szakaszok, és az egyes elemek JSON hello másolja a sablonba. (Nincs másolás hello webtests vagy alertrules csomópontból: Nyissa meg a hello elemek csoportokba tartozó.)
   
    Minden webalkalmazás-teszt tartozó riasztási szabály, rendelkezik, így a toocopy mindkét azok rendelkezik.
   
    Riasztások a metrikák is használható. [Metrika neve](app-insights-powershell-alerts.md#metric-names).
5. A sor beszúrása az egyes erőforrások:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-hello-template"></a>Hello sablon parametrizálja
Most már rendelkezik tooreplace hello egyedi nevek paraméterekkel. túl[sablon parametrizálja](../azure-resource-manager/resource-group-authoring-templates.md), kifejezések használatával írhat egy [segítő funkciók](../azure-resource-manager/resource-group-template-functions.md). 

Nem lehet paraméterezni csak részét, így `concat()` toobuild karakterláncok.

Példa a hello helyettesítések érdemes toomake. Nincsenek minden helyettesítés többszöri felbukkanását. Szükség lehet mások a sablonban. Ezekben a példákban hello paraméterek és változók meghatározott hello sablon hello tetején.

| Keresés | cserélje le |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"`(nagybetűs) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Törölje a GUID azonosítót és azonosítóját. |

### <a name="set-dependencies-between-hello-resources"></a>Hello erőforrások közötti függőségek beállítása
Azure hello erőforrások szigorú sorrendben kell beállítani. toomake meg arról, hogy egy telepítés befejeződése előtt hello mellett kezdődik, adja hozzá a függőség sorokat:

* Hello rendelkezésre állási erőforrás teszteléséhez:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* A riasztás-erőforrást hello elérhetőségi teszt:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Következő lépések
Más automatizálási cikkek:

* [Az Application Insights-erőforrás létrehozása](app-insights-powershell-script-create-resource.md) -nélkül sablon használatával gyorsan elvégezhető.
* [Riasztások beállítása](app-insights-powershell-alerts.md)
* [Webteszt létrehozása](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Azure Diagnostics tooApplication Insights küldése](app-insights-powershell-azure-diagnostics.md)
* [A Githubból tooAzure telepítése](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Kiadási jegyzetek írására](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

