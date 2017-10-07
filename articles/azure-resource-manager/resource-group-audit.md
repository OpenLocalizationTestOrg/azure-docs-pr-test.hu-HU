---
title: "Azure tevékenység aaaView naplózza toomonitor erőforrások |} Microsoft Docs"
description: "Használja a hello tevékenység naplók tooreview felhasználói műveletek és a hibák. Az Azure portál PowerShell, az Azure CLI és REST jeleníti meg."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: tomfitz
ms.openlocfilehash: 8430ed2a9c1dfe5f13423a55d358e590b0facb22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-activity-logs-tooaudit-actions-on-resources"></a>Tevékenység megtekintése naplózza tooaudit műveleteket az egyes erőforrások
Keresztül tevékenységi naplóit meghatározhatja:

* milyen műveleteket is végezhet a hello erőforrást az előfizetésében
* kik kezdeményeztek hello művelet (bár a háttérszolgáltatás által kezdeményezett műveletek nem adják vissza egy felhasználó hello hívó)
* Ha a hello művelet történt
* hello művelet hello állapotát
* hello értékek, amelyek segíthetnek tulajdonságokat kutatás hello művelet

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

Adatok lekérését hello tevékenységi naplóit hello portálon, a PowerShell, az Azure parancssori felület, Insights REST API-t vagy [Insights .NET kódtár](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portál
1. Válassza ki a tooview hello tevékenységi naplóit hello portálon keresztül **figyelő**.
   
    ![Válassza ki a tevékenységi naplóit](./media/resource-group-audit/select-monitor.png)

   Vagy tooautomatically szűrő hello tevékenységnapló egy adott erőforrás vagy egy erőforráscsoport, jelölje be a **tevékenységnapló** adott erőforráspanelen. Figyelje meg, hogy hello tevékenységnapló hello kiválasztott erőforrás automatikusan szűrve van.
   
    ![Szűrés erőforrás szerint](./media/resource-group-audit/filtered-by-resource.png)
2. A hello **tevékenységnapló** panelen megjelenik a legutóbbi műveletek összegzését.
   
    ![műveletek megjelenítéséhez](./media/resource-group-audit/audit-summary.png)
3. jelenik meg, műveletek száma toorestrict hello különböző feltételek kiválasztása. Például hello következő kép bemutatja hello **Timespan** és **esemény által kezdeményezett** mezők megváltozott tooview hello műveleteit egy adott felhasználó vagy alkalmazás hello az elmúlt hónapban. Válassza ki **alkalmaz** tooview hello a lekérdezés eredményét.
   
    ![szűrő beállításainak megadása](./media/resource-group-audit/set-filter.png)

4. Ha később kell toorun hello lekérdezés, válassza ki a **mentése** és nevezze el hello lekérdezés.
   
    ![lekérdezés mentése](./media/resource-group-audit/save-query.png)
5. a lekérdezés futtatásához tooquickly, válasszon egyet hello beépített lekérdezések, például a sikertelen központi telepítéssel.

    ![Válassza ki a lekérdezés](./media/resource-group-audit/select-quick-query.png)

   hello kijelölt lekérdezés automatikusan beállítja a szükséges hello szűrőértékek.

    ![telepítési hibák megtekintése](./media/resource-group-audit/view-failed-deployment.png)   

6. Válasszon ki egy hello műveletek toosee hello esemény összegzését.

    ![nézet művelet](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell
1. tooretrieve naplóbejegyzések, futtassa a hello **Get-AzureRmLog** parancsot. További paraméterek toofilter hello listáját bejegyzések tartalmazzák. Ha nem ad meg egy kezdő és záró idő, az elmúlt egy órában hello bejegyzések megjeleníti. Például a tooretrieve hello művelet erőforráscsoport során hello elmúlt egy órában fusson:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    hello a következő példa bemutatja, hogyan toouse hello tevékenységet naplózni tooresearch műveletek a megadott időtartam során. hello kezdő és befejező dátumok meg van adva a dátumformátum.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Vagy dátum funkciók toospecify hello dátumtartományt, például a hello elmúlt 14 napban is használhat.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Attól függően, hogy a megadott hello kezdete hello parancsokat adhat vissza hello erőforráscsoport műveletek listája túl hosszú. Szűrheti a hello eredményeit mi keres, adja meg a keresési feltételeket. Például ha tooresearch hogyan webalkalmazás le lett állítva, a következő parancs hello futtathatja:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    Amely ehhez a példához mutatja, hogy egy leállítási műveletet végzett el someone@contoso.com. 

  ```powershell 
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. Megtekintheti egy adott felhasználó, egy erőforráscsoport, amely már nem létezik még a hello műveleteit.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. A sikertelen műveleteket végezhet.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Egy hiba hello állapotüzenet az adott bejegyzés megtekintésével összpontosíthat.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Amely adja vissza:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI
* hello futtatja tooretrieve naplóbejegyzések, **azure-csoportok napló megjelenítése** parancsot.

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a>REST API
hello REST műveleteinek használata hello tevékenységnapló hello részét képező [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). napló tooretrieve tevékenységesemények, lásd: [hello felügyeleti események egy előfizetésben listában](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Következő lépések
* Azure tevékenységi naplóit is használható a Power BI toogain nagyobb mélységben hello műveletekről az előfizetésben. Lásd: [megtekintése és elemzése a Power bi-ban és több Azure tevékenységi naplóit](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Tekintse meg a biztonsági házirendek beállításával kapcsolatos toolearn [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).
* toolearn telepítési műveleteit, megtekintésre hello parancsokkal kapcsolatban lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* Hogyan tooprevent törlése az összes felhasználó számára, erőforrás: toolearn [erőforrások az Azure Resource Manager zárolása](resource-group-lock-resources.md).

