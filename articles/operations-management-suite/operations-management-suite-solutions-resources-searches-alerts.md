---
title: "aaaSaved keresés és a riasztások az OMS-megoldások |} Microsoft Docs"
description: "Az OMS megoldások rendszerint tartalmazza mentett keresések hello megoldás által összegyűjtött Naplóelemzési tooanalyze adatokban.  Akkor is definiálhat a riasztások toonotify hello felhasználói, vagy automatikusan választ tooa kritikus hiba a művelet végrehajtása.  Ez a cikk ismerteti, hogyan toodefine Naplóelemzési mentett keresések és riasztások egy ARM-sablon, azok megoldások tartalmazhat."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93d7c5bbf061473833ca6c0a8e4d8e10d923f3ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-toooms-management-solution-preview"></a>Naplóelemzési hozzáadása mentett keresések és riasztások tooOMS felügyeleti megoldás (előzetes verzió)

> [!NOTE]
> Ez az előzetes dokumentum megoldások létrehozásához az OMS Szolgáltatáshoz, amely jelenleg előzetes verziójúak. Az alábbiakban semmilyen sémát tulajdonos toochange.   


[Az OMS megoldások](operations-management-suite-solutions.md) rendszerint tartalmazza [mentett keresések](../log-analytics/log-analytics-log-searches.md) hello megoldás által összegyűjtött Naplóelemzési tooanalyze adatokban.  Előfordulhat, hogy is definiálhat [riasztások](../log-analytics/log-analytics-alerts.md) toonotify felhasználói hello, vagy automatikusan választ tooa kritikus hiba a művelet végrehajtása.  Ez a cikk ismerteti, hogyan toodefine Naplóelemzési a mentett kereséseket a riasztások egy [erőforrás-kezelés sablon](../resource-manager-template-walkthrough.md) , azok tartalmazhat [megoldások](operations-management-suite-solutions-creating.md).

> [!NOTE]
> hello minták ebben a cikkben használható paraméterek és változók vagy kötelező vagy közös toomanagement megoldások és ismertetett [létrehozása kezelési megoldásai Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)  

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy most már tudja, hogyan túl[felügyeleti megoldás létrehozása](operations-management-suite-solutions-creating.md) és hello szerkezete egy [ARM-sablon](../resource-group-authoring-templates.md) és megoldásfájlt.


## <a name="log-analytics-workspace"></a>A Naplóelemzési munkaterület
Összes erőforrása Naplóelemzési találhatók egy [munkaterület](../log-analytics/log-analytics-manage-access.md).  A [OMS munkaterületet, és az Automation-fiók](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello munkaterület nem tartalmazza a hello felügyeleti megoldás, de már léteznie kell hello megoldás telepítve van.  Ha nem érhető el, majd hello megoldás telepítése sikertelen lesz.

hello név hello munkaterület minden Naplóelemzési erőforrás hello neve van.  Hello hello megoldást ezt **munkaterület** paraméter a következő példa egy savedsearch erőforrás hello hasonlóan.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a>Mentett keresések
Tartalmaznak [mentett keresések](../log-analytics/log-analytics-log-searches.md) egy megoldás tooallow felhasználók tooquery által gyűjtött adatokat a megoldás a.  Mentett keresések jelenik meg a **Kedvencek** az OMS-portálon hello és **mentett keresések** a hello Azure-portálon.  A mentett kereséseket is meg kell adni egy riasztás.   

[Naplóelemzési mentett keresés](../log-analytics/log-analytics-log-searches.md) erőforrások típusa lehet `Microsoft.OperationalInsights/workspaces/savedSearches` és a következő struktúra hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



A mentett kereséseket hello tulajdonságainak mindegyikének hello a következő táblázat ismerteti. 

| Tulajdonság | Leírás |
|:--- |:--- |
| category | hello hello kategóriákhoz mentett keresés.  A mentett keresések hello ugyanahhoz a megoldáshoz gyakran megosztja a egyetlen kategória, azok sorolhatók hello konzol. |
| DisplayName | A hello neve toodisplay hello portálon mentett keresés. |
| lekérdezés | Lekérdezés toorun. |

> [!NOTE]
> Ha a JSON-ként értelmezhetők karaktereket tartalmaz, akkor esetleg hello lekérdezés toouse escape karakter.  Például, ha a lekérdezés **típusa: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, kell írható hello megoldás fájlt a **típusa: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Riasztások
[Naplófájl Analytics riasztások](../log-analytics/log-analytics-alerts.md) mentett keresést futtat rendszeres időközönkénti riasztási szabályok jönnek létre.  Ha hello lekérdezési eredmények hello megfelel a megadott feltételeknek, egy riasztás rekord jön létre, és egy vagy több műveletek futnak.  

A riasztási szabályok megoldásra a következő három különböző erőforrások hello épülnek fel.

- **Mentett keresés.**  Határozza meg a futtatni kívánt hello napló keresése.  Több riasztási szabályok megoszthat egy mentett keresés.
- **Ütemezés.**  Határozza meg, milyen gyakran hello napló keresése fog futni.  Minden riasztási szabályt egy ütemezés fog rendelkezni.
- **A műveletet.**  Minden riasztási szabály lesz olyan típusú erőforrás a művelet **riasztás** , amely meghatározza, hogy hello riasztást egy riasztási rekord jön létre és riasztás súlyossági hello hello feltételek például hello részleteit.  hello művelet erőforrás opcionálisan határozza meg az e-mail és runbook választ.
- **Webhook művelet (nem kötelező).**  Ha hello riasztási szabály fel fogja hívni a webhook, akkor azt igényli, hogy egy további művelet erőforrás típussal rendelkező **Webhook**.    

Erőforrások fent ismertetett Keresés mentése.  hello további erőforrások az alábbiakban található.


### <a name="schedule-resource"></a>Ütemezés erőforrás

A mentett kereséseket rendelkezhet minden ütemezéssel jelző külön riasztási szabály egy vagy több ütemezés. hello ütemezése határozza meg milyen gyakran hello keresési fut és hello időtartam, mely hello keresztül lekért adatmennyiséget.  Ütemezés erőforrások típusa lehet `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` és a következő struktúra hello rendelkezik. Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



a következő táblázat hello ütemezés erőforrás hello tulajdonságait ismerteti.

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| engedélyezve       | Igen | Meghatározza, hogy az hello riasztás létrehozásakor engedélyezett-e. |
| interval      | Igen | Milyen gyakran hello lekérdezés fut perc múlva. |
| queryTimeSpan | Igen | Idő (percben) keresztül mely tooevaluate eredmények hosszát. |

hello ütemezés erőforrás, mielőtt hello ütemezés létrehozása mentett keresés hello függ.


### <a name="actions"></a>Műveletek
Két különböző művelet erőforrás hello által megadott **típus** tulajdonság.  Ütemezés szükséges **riasztás** hello riasztási szabályt, és milyen műveleteket hajtja végre, amikor létrejön egy riasztás hello részleteit meghatározó művelet.  Az is előfordulhat, hogy tartalmazza a **Webhook** művelet, ha egy webhook a hello riasztást kell megnyitni.  

Művelet erőforrások típusa lehet `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

#### <a name="alert-actions"></a>Riasztási műveletek

Minden ütemezés kell egy **riasztási** művelet.  Ez határozza meg a hello riasztást, és opcionálisan értesítési, a javítási műveletek hello részleteit.  Értesítést küld egy e-mailek tooone vagy több címet.  A szervizelés elindít egy Azure Automation tooattempt tooremediate hello észlelhető hiba.

Értesítési műveletek struktúra a következő hello rendelkezik.  Ez magában foglalja közös változók és paramétereket, így másolja és illessze be a következő kódrészletet a megoldásfájlt a és módosítására használható hello paraméterek nevei. 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

a következő táblák hello riasztási művelet erőforrás hello tulajdonságait ismerteti.

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| Típus | Igen | Hello művelet típusa.  Ez lesz **riasztás** riasztási műveletekhez. |
| Név | Igen | Hello riasztás megjelenített neve.  Ez az hello hello riasztási szabály hello konzolon megjelenített neve. |
| Leírás | Nem | Hello riasztás nem kötelező leírása. |
| Súlyosság | Igen | A következő értékek hello riasztási rekordját hello súlyossága:<br><br> **Kritikus**<br>**Figyelmeztetés**<br>**Tájékoztató** |


##### <a name="threshold"></a>Küszöbérték
Ez a szakasz meg kell adni.  Azt határozza meg a riasztási küszöbérték hello hello tulajdonságait.

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| Operátor | Igen | A következő értékek hello hello összehasonlító operátort:<br><br>**gt = nagyobb, mint<br>lt = kisebb, mint** |
| Érték | Igen | hello érték toocompare hello eredmények. |


##### <a name="metricstrigger"></a>MetricsTrigger
Ez a szakasz nem kötelező megadni.  Adja hozzá a metrika mérési riasztás.

> [!NOTE]
> Metrika mérési riasztások jelenleg nyilvános előzetes verziójához. 

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| TriggerCondition | Igen | Megadja, hogy hello küszöbérték megszegése vagy a következő értékek hello egymást követő megszegése teljes száma:<br><br>**Teljes<br>egymást követő** |
| Operátor | Igen | A következő értékek hello hello összehasonlító operátort:<br><br>**gt = nagyobb, mint<br>lt = kisebb, mint** |
| Érték | Igen | Hello időpontokban hello feltételek számának fennállnak tootrigger hello riasztást kell lennie. |

##### <a name="throttling"></a>Szabályozás
Ez a szakasz nem kötelező megadni.  Ez a szakasz tartalmazza, ha a kívánt toosuppress riasztások hello azonos szabály néhány időtartamig riasztás létrehozása után.

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| DurationInMinutes | Igen, ha a sávszélesség-szabályozás elemet tartalmazza | Perc toosuppress értesítések száma után hello közül ugyanazon riasztási szabály létrehozása zajlik. |

##### <a name="emailnotification"></a>EmailNotification
 Ez a szakasz nem kötelező, ha azt szeretné, hello riasztási toosend mail tooone vagy további címzettek Include.

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| Címzettek | Igen | Címek vesszővel tagolt listája az e-mailek toosend értesítést, ha riasztás jön létre, mint a következő példa hello.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Tárgy | Igen | Hello e-mail tárgysora. |
| Melléklet | Nem | Jelenleg nem támogatja a mellékleteket.  Ha ez az elem megtalálható, meg kell **nincs**. |


##### <a name="remediation"></a>Szervizkiszolgáló
Ez a szakasz nem kötelező adja hozzá, ha azt szeretné, hogy egy runbook toostart válasz toohello riasztásban. |

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| RunbookName | Igen | Hello runbook toostart neve. |
| WebhookUri | Igen | Hello runbook hello webhook URI azonosítóját. |
| A lejárati | Nem | Dátum és idő, a szervizelési hello lejár. |

#### <a name="webhook-actions"></a>Webhookműveletek

Webhookműveletek egy folyamat megkezdéséhez hívja az egy URL-cím és a nem kötelezően egy hasznos toobe küldött. Hasonló tooRemediation műveletek kivételével ezek célja a webhookokkal, előfordulhat, hogy aktiválják az Azure Automation-runbook eltérő folyamatok. Hello további lehetőség a tartalom céleszközökre toobe toohello távoli folyamattal is biztosítanak.

Ha a riasztás fel fogja hívni a webhook, akkor el kell egy művelet erőforrás típussal rendelkező **Webhook** hozzáadása toohello a **riasztás** művelet erőforrás.  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

a következő táblák hello Webhook művelet erőforrás hello tulajdonságait ismerteti.

| Elem neve | Szükséges | Leírás |
|:--|:--|:--|
| type | Igen | Hello művelet típusa.  Ez lesz **Webhook** webhook műveletekhez. |
| név | Igen | Megjelenített név hello a művelethez.  Ez nem hello konzolban jelenik meg. |
| wehookUri | Igen | Hello webhook URI. |
| customPayload | Nem | Egyéni adattartalom küldött toobe toohello webhook. hello formátum függvényében adható meg, milyen hello webhook által várt paraméterekkel. |




## <a name="sample"></a>Minta

Az alábbiakban látható egy minta a megoldás, amely tartalmazza, amely tartalmazza a következő erőforrások hello:

- Mentett keresés
- Ütemezés
- Riasztási művelet
- Webhook művelet

minta használ hello [szokásos megoldást paraméterek](operations-management-suite-solutions-solution-file.md#parameters) változókat, amelyek a megoldás, mint a gyakran használni ellenezte toohardcoding értékek hello erőforrás-definíciókban.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for hello email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
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
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
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
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


a következő paraméterfájl hello minták értékeket biztosít ebben a megoldásban.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a>Következő lépések
* [Nézetek hozzáadása](operations-management-suite-solutions-resources-views.md) tooyour felügyeleti megoldás.
* [Adja hozzá az Automation-forgatókönyveket és egyéb erőforrások](operations-management-suite-solutions-resources-automation.md) tooyour felügyeleti megoldás.

