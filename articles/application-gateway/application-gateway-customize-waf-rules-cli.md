---
title: "aaaCustomize webes alkalmazás tűzfalszabályokat az Azure Application Gateway - Azure CLI 2.0 |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocustomize webalkalmazási tűzfal szabályai Alkalmazásátjáró az Azure CLI 2.0 hello."
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
ms.openlocfilehash: b83ffb9f6a7e0d0c8c970885d2bcb3b63d32581c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-cli-20"></a><span data-ttu-id="9c674-103">Webes alkalmazás tűzfalszabályok keresztül hello Azure CLI 2.0 testreszabása</span><span class="sxs-lookup"><span data-stu-id="9c674-103">Customize web application firewall rules through hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9c674-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9c674-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="9c674-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c674-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="9c674-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9c674-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="9c674-107">hello Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít.</span><span class="sxs-lookup"><span data-stu-id="9c674-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="9c674-108">A védelem hello megnyitása webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) által biztosított.</span><span class="sxs-lookup"><span data-stu-id="9c674-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="9c674-109">Néhány szabály okozhat a vakriasztások és valós adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="9c674-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="9c674-110">Emiatt a Application Gateway hello funkció toocustomize csoportok és a szabályok biztosít.</span><span class="sxs-lookup"><span data-stu-id="9c674-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="9c674-111">Adott hello csoportok és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="9c674-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="9c674-112">A szabály csoportok megtekintése és szabályok</span><span class="sxs-lookup"><span data-stu-id="9c674-112">View rule groups and rules</span></span>

<span data-ttu-id="9c674-113">a következő példák hello hogyan tooview szabályok és a szabály konfigurálható csoportok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="9c674-113">hello following code examples show how tooview rules and rule groups that are configurable.</span></span>

### <a name="view-rule-groups"></a><span data-ttu-id="9c674-114">A szabály csoportok megtekintése</span><span class="sxs-lookup"><span data-stu-id="9c674-114">View rule groups</span></span>

<span data-ttu-id="9c674-115">hello a következő példa bemutatja, hogyan tooview hello csoportok:</span><span class="sxs-lookup"><span data-stu-id="9c674-115">hello following example shows how tooview hello rule groups:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --type OWASP
```

<span data-ttu-id="9c674-116">a következő kimeneti hello példa megelőző hello csonkolt válaszára:</span><span class="sxs-lookup"><span data-stu-id="9c674-116">hello following output is a truncated response from hello preceding example:</span></span>

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  },
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_2.2.9",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
   "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "crs_20_protocol_violations",
        "rules": null
      },
      ...
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "2.2.9",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

### <a name="view-rules-in-a-rule-group"></a><span data-ttu-id="9c674-117">A szabály csoport szabályok megtekintése</span><span class="sxs-lookup"><span data-stu-id="9c674-117">View rules in a rule group</span></span>

<span data-ttu-id="9c674-118">hello a következő példa bemutatja, hogyan tooview szabályok egy adott szabály csoportban:</span><span class="sxs-lookup"><span data-stu-id="9c674-118">hello following example shows how tooview rules in a specified rule group:</span></span>

```azurecli-interactive
az network application-gateway waf-config list-rule-sets --group "REQUEST-910-IP-REPUTATION"
```

<span data-ttu-id="9c674-119">a következő kimeneti hello példa megelőző hello csonkolt válaszára:</span><span class="sxs-lookup"><span data-stu-id="9c674-119">hello following output is a truncated response from hello preceding example:</span></span>

```
[
  {
    "id": "/subscriptions//resourceGroups//providers/Microsoft.Network/applicationGatewayAvailableWafRuleSets/",
    "location": null,
    "name": "OWASP_3.0",
    "provisioningState": "Succeeded",
    "resourceGroup": "",
    "ruleGroups": [
      {
        "description": "",
        "ruleGroupName": "REQUEST-910-IP-REPUTATION",
        "rules": [
          {
            "description": "Rule 910011",
            "ruleId": 910011
          },
          ...
        ]
      }
    ],
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "tags": null,
    "type": "Microsoft.Network/applicationGatewayAvailableWafRuleSets"
  }
]
```

## <a name="disable-rules"></a><span data-ttu-id="9c674-120">Szabályok letiltása</span><span class="sxs-lookup"><span data-stu-id="9c674-120">Disable rules</span></span>

<span data-ttu-id="9c674-121">hello következő példa letiltja a szabályok `910018` és `910017` az Alkalmazásátjáró:</span><span class="sxs-lookup"><span data-stu-id="9c674-121">hello following example disables rules `910018` and `910017` on an application gateway:</span></span>

```azurecli-interactive
az network application-gateway waf-config set --resource-group AdatumAppGatewayRG --gateway-name AdatumAppGateway --enabled true --rule-set-version 3.0 --disabled-rules 910018 910017
```

## <a name="next-steps"></a><span data-ttu-id="9c674-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c674-122">Next steps</span></span>

<span data-ttu-id="9c674-123">Miután konfigurálta a letiltott szabályok, áttekintheti, hogyan tooview a WAF naplókat.</span><span class="sxs-lookup"><span data-stu-id="9c674-123">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="9c674-124">További információkért lásd: [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="9c674-124">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
