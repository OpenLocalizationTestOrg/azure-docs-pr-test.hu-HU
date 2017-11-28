---
title: "aaaCustomize webes alkalmazás tűzfalszabályokat az Azure Application Gateway - PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocustomize webalkalmazási tűzfal szabályai Alkalmazásátjáró a PowerShell használatával."
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
ms.openlocfilehash: f320e687b0f621515255469dac8e375cdd900dda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-powershell"></a><span data-ttu-id="36aa5-103">Webes alkalmazás tűzfalszabályok Powershellen keresztül testreszabása</span><span class="sxs-lookup"><span data-stu-id="36aa5-103">Customize web application firewall rules through PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="36aa5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="36aa5-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="36aa5-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="36aa5-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="36aa5-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="36aa5-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="36aa5-107">hello Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít.</span><span class="sxs-lookup"><span data-stu-id="36aa5-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="36aa5-108">A védelem hello megnyitása webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) által biztosított.</span><span class="sxs-lookup"><span data-stu-id="36aa5-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="36aa5-109">Néhány szabály okozhat a vakriasztások és valós adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="36aa5-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="36aa5-110">Emiatt a Application Gateway hello funkció toocustomize csoportok és a szabályok biztosít.</span><span class="sxs-lookup"><span data-stu-id="36aa5-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="36aa5-111">Adott hello csoportok és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="36aa5-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS Rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="36aa5-112">A szabály csoportok megtekintése és szabályok</span><span class="sxs-lookup"><span data-stu-id="36aa5-112">View rule groups and rules</span></span>

<span data-ttu-id="36aa5-113">a következő példák hello hogyan tooview szabályok és a szabály konfigurálható egy WAF-kompatibilis alkalmazások átjárón csoportok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="36aa5-113">hello following code examples show how tooview rules and rule groups that are configurable on a WAF-enabled application gateway.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="36aa5-114">A szabály csoportok megtekintése</span><span class="sxs-lookup"><span data-stu-id="36aa5-114">View rule groups</span></span>

<span data-ttu-id="36aa5-115">a következő példa azt mutatja meg hogyan hello tooview csoportok:</span><span class="sxs-lookup"><span data-stu-id="36aa5-115">hello following example shows how tooview rule groups:</span></span>

```powershell
Get-AzureRmApplicationGatewayAvailableWafRuleSets
```

<span data-ttu-id="36aa5-116">a következő kimeneti hello példa megelőző hello csonkolt válaszára:</span><span class="sxs-lookup"><span data-stu-id="36aa5-116">hello following output is a truncated response from hello preceding example:</span></span>

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

## <a name="disable-rules"></a><span data-ttu-id="36aa5-117">Szabályok letiltása</span><span class="sxs-lookup"><span data-stu-id="36aa5-117">Disable rules</span></span>

<span data-ttu-id="36aa5-118">hello következő példa letiltja a szabályok `910018` és `910017` az Alkalmazásátjáró:</span><span class="sxs-lookup"><span data-stu-id="36aa5-118">hello following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="36aa5-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36aa5-119">Next steps</span></span>

<span data-ttu-id="36aa5-120">Miután konfigurálta a letiltott szabályok, áttekintheti, hogyan tooview a WAF naplókat.</span><span class="sxs-lookup"><span data-stu-id="36aa5-120">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="36aa5-121">További információkért lásd: [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="36aa5-121">For more information, see [Application Gateway Diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
