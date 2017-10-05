---
title: "Webes alkalmazás tűzfalszabályok Azure Application Gateway - PowerShell testreszabása |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogy miként web Application Gateway a PowerShell alkalmazás tűzfalszabályok testreszabásához."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: gwallace
ms.openlocfilehash: 681625e40035b05c593c6161236cb80b7db576b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a><span data-ttu-id="2596b-103">Webes alkalmazás tűzfalszabályok Powershellen keresztül testreszabása</span><span class="sxs-lookup"><span data-stu-id="2596b-103">Customize web application firewall rules through PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2596b-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2596b-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="2596b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2596b-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="2596b-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="2596b-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="2596b-107">Az Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít.</span><span class="sxs-lookup"><span data-stu-id="2596b-107">The Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="2596b-108">A védelem által a nyitott webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="2596b-108">These protections are provided by the Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="2596b-109">Néhány szabály okozhat a vakriasztások és valós adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="2596b-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="2596b-110">Emiatt Application Gateway lehetővé teszi a csoportok és a szabályok testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="2596b-110">For this reason, Application Gateway provides the capability to customize rule groups and rules.</span></span> <span data-ttu-id="2596b-111">Az adott szabály csoportokról és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="2596b-111">For more information on the specific rule groups and rules, see [List of web application firewall CRS Rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="2596b-112">A szabály csoportok megtekintése és szabályok</span><span class="sxs-lookup"><span data-stu-id="2596b-112">View rule groups and rules</span></span>

<span data-ttu-id="2596b-113">A következő kód példák szemléltetik a szabályok és konfigurálható egy WAF-kompatibilis alkalmazások átjárón csoportok megtekintése.</span><span class="sxs-lookup"><span data-stu-id="2596b-113">The following code examples show how to view rules and rule groups that are configurable on a WAF-enabled application gateway.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="2596b-114">A szabály csoportok megtekintése</span><span class="sxs-lookup"><span data-stu-id="2596b-114">View rule groups</span></span>

<span data-ttu-id="2596b-115">A következő példa bemutatja, hogyan szabály csoportok megtekintése:</span><span class="sxs-lookup"><span data-stu-id="2596b-115">The following example shows how to view rule groups:</span></span>

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

<span data-ttu-id="2596b-116">A következő az előző példából csonkolt válasz kimenete:</span><span class="sxs-lookup"><span data-stu-id="2596b-116">The following output is a truncated response from the preceding example:</span></span>

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

## <a name="disable-rules"></a><span data-ttu-id="2596b-117">Szabályok letiltása</span><span class="sxs-lookup"><span data-stu-id="2596b-117">Disable rules</span></span>

<span data-ttu-id="2596b-118">A következő példa letiltja a szabályok `910018` és `910017` az Alkalmazásátjáró:</span><span class="sxs-lookup"><span data-stu-id="2596b-118">The following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="2596b-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2596b-119">Next steps</span></span>

<span data-ttu-id="2596b-120">Miután konfigurálta a letiltott szabályok, megismerheti a WAF naplók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2596b-120">After you configure your disabled rules, you can learn how to view your WAF logs.</span></span> <span data-ttu-id="2596b-121">További információkért lásd: [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="2596b-121">For more information, see [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
