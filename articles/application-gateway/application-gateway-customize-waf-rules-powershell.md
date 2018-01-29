---
title: "Webes alkalmazás tűzfalszabályok Azure Application Gateway - PowerShell testreszabása |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogy miként web Application Gateway a PowerShell alkalmazás tűzfalszabályok testreszabásához."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: 4595864a7bc624375ba2ff6ace09ebae5b0f843a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/09/2018
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a>Webes alkalmazás tűzfalszabályok Powershellen keresztül testreszabása

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

Az Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít. A védelem által a nyitott webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) vannak megadva. Néhány szabály okozhat a vakriasztások és valós adatforgalmat. Emiatt Application Gateway lehetővé teszi a csoportok és a szabályok testreszabásához. Az adott szabály csoportokról és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).

## <a name="view-rule-groups-and-rules"></a>A szabály csoportok megtekintése és szabályok

A következő kód példák szemléltetik a szabályok és konfigurálható egy WAF-kompatibilis alkalmazások átjárón csoportok megtekintése.

### <a name="view-rule-groups"></a>A szabály csoportok megtekintése

A következő példa bemutatja, hogyan szabály csoportok megtekintése:

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

A következő az előző példából csonkolt válasz kimenete:

```
OWASP (Ver. 3.0):

    REQUEST-910-IP-REPUTATION:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            910011     Rule 910011
            910012     Rule 910012
            ...        ...

    REQUEST-911-METHOD-ENFORCEMENT:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            911011     Rule 911011
            ...        ...

OWASP (Ver. 2.2.9):

    crs_20_protocol_violations:
        Description:
            
        Rules:
            RuleId     Description
            ------     -----------
            960911     Invalid HTTP Request Line
            981227     Apache Error: Invalid URI in Request.
            960000     Attempted multipart/form-data bypass
            ...        ...
```

## <a name="disable-rules"></a>Szabályok letiltása

A következő példa letiltja a szabályok `910018` és `910017` az Alkalmazásátjáró:

```powershell
$disabledrules=New-AzureRmApplicationGatewayFirewallDisabledRuleGroupConfig -RuleGroupName REQUEST-910-IP-REPUTATION -Rules 910018,910017
Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -ApplicationGateway $gw -Enabled $true -FirewallMode Detection -RuleSetVersion 3.0 -RuleSetType OWASP -DisabledRuleGroups $disabledrules
```

## <a name="next-steps"></a>További lépések

Miután konfigurálta a letiltott szabályok, megismerheti a WAF naplók megtekintéséhez. További információkért lásd: [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
