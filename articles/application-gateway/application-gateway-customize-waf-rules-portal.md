---
title: "aaaCustomize webes alkalmazás tűzfalszabályokat az Azure Application Gateway - Azure-portál |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocustomize webalkalmazási tűzfal szabályai Alkalmazásátjáró a hello Azure-portálon."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 1159500b-17ba-41e7-88d6-b96986795084
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: gwallace
ms.openlocfilehash: 36a999279e0370b9f803e12257856a56753b23a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-web-application-firewall-rules-through-hello-azure-portal"></a><span data-ttu-id="19525-103">Webes alkalmazás tűzfalszabályok hello Azure-portálon keresztül testreszabása</span><span class="sxs-lookup"><span data-stu-id="19525-103">Customize web application firewall rules through hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="19525-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="19525-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="19525-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="19525-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="19525-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="19525-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="19525-107">hello Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít.</span><span class="sxs-lookup"><span data-stu-id="19525-107">hello Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="19525-108">A védelem hello megnyitása webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) által biztosított.</span><span class="sxs-lookup"><span data-stu-id="19525-108">These protections are provided by hello Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="19525-109">Néhány szabály okozhat a vakriasztások és valós adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="19525-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="19525-110">Emiatt a Application Gateway hello funkció toocustomize csoportok és a szabályok biztosít.</span><span class="sxs-lookup"><span data-stu-id="19525-110">For this reason, Application Gateway provides hello capability toocustomize rule groups and rules.</span></span> <span data-ttu-id="19525-111">Adott hello csoportok és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="19525-111">For more information on hello specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

>[!NOTE]
> <span data-ttu-id="19525-112">Ha az alkalmazás átjáró hello WAF réteg nem használ, hello beállítás tooupgrade hello alkalmazás átjáró toohello WAF réteg hello jobb oldali ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="19525-112">If your application gateway is not using hello WAF tier, hello option tooupgrade hello application gateway toohello WAF tier appears in hello right pane.</span></span> 

![WAF engedélyezése][fig1]

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="19525-114">A szabály csoportok megtekintése és szabályok</span><span class="sxs-lookup"><span data-stu-id="19525-114">View rule groups and rules</span></span>

<span data-ttu-id="19525-115">**tooview csoportok és szabályok**</span><span class="sxs-lookup"><span data-stu-id="19525-115">**tooview rule groups and rules**</span></span>
   1. <span data-ttu-id="19525-116">Keresse meg az Alkalmazásátjáró toohello, és válassza ki **webalkalmazási tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="19525-116">Browse toohello application gateway, and then select **Web application firewall**.</span></span>  
   2. <span data-ttu-id="19525-117">Válassza ki **speciális konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="19525-117">Select **Advanced rule configuration**.</span></span>  
   <span data-ttu-id="19525-118">Ebben a nézetben látható egy tábla összes hello szabály csoportok szabálykészlet kiválasztott hello hello oldalán.</span><span class="sxs-lookup"><span data-stu-id="19525-118">This view shows a table on hello page of all hello rule groups provided with hello chosen rule set.</span></span> <span data-ttu-id="19525-119">Hello szabály jelölőnégyzetekből ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="19525-119">All of hello rule's check boxes are selected.</span></span>

![Letiltott szabályok konfigurálása][1]

## <a name="search-for-rules-toodisable"></a><span data-ttu-id="19525-121">Szabályok toodisable keresése</span><span class="sxs-lookup"><span data-stu-id="19525-121">Search for rules toodisable</span></span>

<span data-ttu-id="19525-122">Hello **webes alkalmazás tűzfalbeállítások** panel hello képességet biztosít toofilter hello szabályok segítségével a szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="19525-122">hello **Web application firewall settings** blade provides hello capability toofilter hello rules through a text search.</span></span> <span data-ttu-id="19525-123">csak a hello csoportok és a szabályt, amely tartalmazza a keresett szöveget hello hello eredményt jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="19525-123">hello result displays only hello rule groups and rules that contain hello text you searched for.</span></span>

![Szabályok keresése][2]

## <a name="disable-rule-groups-and-rules"></a><span data-ttu-id="19525-125">Tiltsa le a csoportok és szabályok</span><span class="sxs-lookup"><span data-stu-id="19525-125">Disable rule groups and rules</span></span>

<span data-ttu-id="19525-126">Ha az még szabályok letiltása, letilthatja egy teljes csoport vagy a egy vagy több szabály csoportok meghatározott szabályokat.</span><span class="sxs-lookup"><span data-stu-id="19525-126">When your're disabling rules, you can disable an entire rule group or specific rules under one or more rule groups.</span></span> 

<span data-ttu-id="19525-127">**toodisable csoportok vagy speciális szabályok**</span><span class="sxs-lookup"><span data-stu-id="19525-127">**toodisable rule groups or specific rules**</span></span>

   1. <span data-ttu-id="19525-128">Hello szabályok vagy a megjeleníteni kívánt toodisable csoportok kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="19525-128">Search for hello rules or rule groups that you want toodisable.</span></span>
   2. <span data-ttu-id="19525-129">Hello szabályok, amelyet az toodisable, törölje a hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="19525-129">Clear hello check boxes for hello rules that you want toodisable.</span></span> 
   2. <span data-ttu-id="19525-130">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="19525-130">Select **Save**.</span></span> 

![Módosítások mentése][3]

## <a name="next-steps"></a><span data-ttu-id="19525-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19525-132">Next steps</span></span>

<span data-ttu-id="19525-133">Miután konfigurálta a letiltott szabályok, áttekintheti, hogyan tooview a WAF naplókat.</span><span class="sxs-lookup"><span data-stu-id="19525-133">After you configure your disabled rules, you can learn how tooview your WAF logs.</span></span> <span data-ttu-id="19525-134">További információkért lásd: [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="19525-134">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
