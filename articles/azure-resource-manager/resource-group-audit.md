---
title: "Azure tevékenységi naplóit figyelését erőforrások megtekintése |} Microsoft Docs"
description: "Használja a tevékenységi naplóit felülvizsgálati felhasználói műveletek és a hibák. Az Azure portál PowerShell, az Azure CLI és REST jeleníti meg."
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
ms.openlocfilehash: ecfb7f726d5447710948405b2dd83fcd1db3dff2
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/08/2017
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Az erőforrás műveletek naplózása tevékenység naplók megtekintése
Keresztül tevékenységi naplóit meghatározhatja:

* milyen műveleteket az előfizetésében erőforrásokon elvégzett
* a művelet kik kezdeményeztek, (bár a háttérszolgáltatás által kezdeményezett műveletek nem adják vissza egy felhasználó a hívó)
* Ha a művelet történt
* A művelet állapotát
* Az értékeket, amelyek segíthetnek tulajdonságokat vizsgálja meg a műveletet

[!INCLUDE [resource-manager-audit-limitations](../../includes/resource-manager-audit-limitations.md)]

Adatok lekérését a portálon, a PowerShell, az Azure parancssori felület, Insights REST API-t a tevékenységi naplóit vagy [Insights .NET kódtár](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portál
1. Válassza ki, ha a tevékenység-naplókat a portálon keresztül **figyelő**.
   
    ![Válassza ki a tevékenységi naplóit](./media/resource-group-audit/select-monitor.png)

   Vagy egy adott erőforráshoz vagy az erőforráscsoport a műveletnapló automatikusan szűréséhez válassza **tevékenységnapló**. Figyelje meg, hogy a műveletnapló automatikusan úgy szűri a kiválasztott erőforrás.
   
    ![Szűrés erőforrás szerint](./media/resource-group-audit/filtered-by-resource.png)
2. Az a **tevékenységnapló**, megjelenik a legutóbbi műveletek összegzését.
   
    ![műveletek megjelenítéséhez](./media/resource-group-audit/audit-summary.png)
3. A megjelenített műveletek korlátozása érdekében különböző feltételek kiválasztása. Például az alábbi képen látható a **Timespan** és **esemény által kezdeményezett** mezők módosítani egy adott felhasználó vagy az elmúlt hónapban az alkalmazás által végrehajtott műveletek megjelenítéséhez. Válassza ki **alkalmaz** a lekérdezés eredményeinek megtekintése.
   
    ![szűrő beállításainak megadása](./media/resource-group-audit/set-filter.png)

4. Ha később újra futtatni a lekérdezést van szüksége, válassza ki a **mentése** és nevezze el a lekérdezést.
   
    ![lekérdezés mentése](./media/resource-group-audit/save-query.png)
5. Gyorsan futtatni a lekérdezést, hogy közül választhat a beépített lekérdezések, például a sikertelen központi telepítéssel.

    ![Válassza ki a lekérdezés](./media/resource-group-audit/select-quick-query.png)

   A kijelölt lekérdezés automatikusan beállítja a szükséges szűrőértékek.

    ![telepítési hibák megtekintése](./media/resource-group-audit/view-failed-deployment.png)   

6. Válassza ki a műveletek az esemény összegzését.

    ![nézet művelet](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell
1. A naplóbejegyzések lekéréséhez futtassa a **Get-AzureRmLog** parancsot. A bejegyzések szűréséhez további paramétereket megadnia. Ha nem ad meg egy kezdő és záró idő, visszaadja a bejegyzéseket az elmúlt egy óra. Például beolvasni a művelet erőforráscsoport során az elmúlt egy órában fusson:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    A következő példa bemutatja, hogyan kutatási műveletek a megadott idő alatt végrehajtott tevékenység napló használatára. A kezdő és befejező dátumok meg van adva a dátumformátum.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Vagy dátum funkciók segítségével adja meg a dátumtartományt, például az elmúlt 14 napban.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Attól függően, hogy a megadott kezdési idejét az előző parancsot az erőforráscsoport műveletek listája túl hosszú adhat vissza. Szűrheti az eredményeket a mi keres, adja meg a keresési feltételeknek. Például ha kutatás hogyan webalkalmazás le lett állítva, a következő parancsot futtathatja:

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

3. Egy adott felhasználó, egy erőforráscsoport, amely már nem létezik még a végrehajtott műveleteket is kereshet.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. A sikertelen műveleteket végezhet.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Egy hiba történt az állapotüzenet tartalmazza az adott bejegyzés megtekintésével összpontosíthat.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Amely adja vissza:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI
* Naplóbejegyzéseket lekéréséhez futtassa a **azure-csoportok napló megjelenítése** parancsot.

  ```azurecli
  azure group log show ExampleGroup --json
  ```


## <a name="rest-api"></a>REST API
A REST műveleteinek használata a műveletnapló részét képezik a [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Tevékenység naplóeseményeket lekéréséhez lásd: [listában szereplő előfizetés felügyeleti események](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Következő lépések
* Az Azure tevékenységi naplóit segítségével a Power BI információt kaphat a nagyobb az előfizetésében szereplő műveleteket. Lásd: [megtekintése és elemzése a Power bi-ban és több Azure tevékenységi naplóit](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Biztonsági házirendek beállításával kapcsolatos további tudnivalókért lásd: [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).
* Az üzembe helyezési műveleteinek megtekintése a parancsokkal kapcsolatban további tudnivalókért lásd: [üzembe helyezési műveleteinek megtekintése](resource-manager-deployment-operations.md).
* Az összes felhasználó erőforrás törlésének megakadályozására, lásd: [erőforrások az Azure Resource Manager zárolása](resource-group-lock-resources.md).
* Egyes Microsoft Azure Resource Manager szolgáltatók elérhető műveletek listájának megtekintéséhez lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](~/articles/active-directory/role-based-access-control-resource-provider-operations.md)

