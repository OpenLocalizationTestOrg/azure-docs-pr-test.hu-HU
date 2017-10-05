---
title: "Webes alkalmazás tűzfalszabályok Azure Application Gateway - Azure-portál testreszabása |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogy miként webes alkalmazás tűzfalszabályokat az alkalmazás átjáró és az Azure portál testreszabásához."
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
ms.openlocfilehash: cdcbadbc3765dfc583c26e1b1453863d421c9a72
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="customize-web-application-firewall-rules-through-the-azure-portal"></a><span data-ttu-id="a32fd-103">Webes alkalmazás tűzfalszabályokat az Azure portálon keresztül testreszabása</span><span class="sxs-lookup"><span data-stu-id="a32fd-103">Customize web application firewall rules through the Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a32fd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a32fd-104">Azure portal</span></span>](application-gateway-customize-waf-rules-portal.md)
> * [<span data-ttu-id="a32fd-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a32fd-105">PowerShell</span></span>](application-gateway-customize-waf-rules-powershell.md)
> * [<span data-ttu-id="a32fd-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="a32fd-106">Azure CLI 2.0</span></span>](application-gateway-customize-waf-rules-cli.md)

<span data-ttu-id="a32fd-107">Az Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít.</span><span class="sxs-lookup"><span data-stu-id="a32fd-107">The Azure Application Gateway web application firewall (WAF) provides protection for web applications.</span></span> <span data-ttu-id="a32fd-108">A védelem által a nyitott webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="a32fd-108">These protections are provided by the Open Web Application Security Project (OWASP) Core Rule Set (CRS).</span></span> <span data-ttu-id="a32fd-109">Néhány szabály okozhat a vakriasztások és valós adatforgalmat.</span><span class="sxs-lookup"><span data-stu-id="a32fd-109">Some rules can cause false positives and block real traffic.</span></span> <span data-ttu-id="a32fd-110">Emiatt Application Gateway lehetővé teszi a csoportok és a szabályok testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="a32fd-110">For this reason, Application Gateway provides the capability to customize rule groups and rules.</span></span> <span data-ttu-id="a32fd-111">Az adott szabály csoportokról és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).</span><span class="sxs-lookup"><span data-stu-id="a32fd-111">For more information on the specific rule groups and rules, see [List of web application firewall CRS rule groups and rules](application-gateway-crs-rulegroups-rules.md).</span></span>

>[!NOTE]
> <span data-ttu-id="a32fd-112">Az Alkalmazásátjáró nem használja a WAF rétegben, ha az Alkalmazásátjáró WAF csomagra frissítési lehetőség a jobb oldali ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a32fd-112">If your application gateway is not using the WAF tier, the option to upgrade the application gateway to the WAF tier appears in the right pane.</span></span> 

![WAF engedélyezése][fig1]

## <a name="view-rule-groups-and-rules"></a><span data-ttu-id="a32fd-114">A szabály csoportok megtekintése és szabályok</span><span class="sxs-lookup"><span data-stu-id="a32fd-114">View rule groups and rules</span></span>

<span data-ttu-id="a32fd-115">**Csoportok és a szabályok megtekintése**</span><span class="sxs-lookup"><span data-stu-id="a32fd-115">**To view rule groups and rules**</span></span>
   1. <span data-ttu-id="a32fd-116">Keresse meg az Alkalmazásátjáró, és válassza ki **webalkalmazási tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="a32fd-116">Browse to the application gateway, and then select **Web application firewall**.</span></span>  
   2. <span data-ttu-id="a32fd-117">Válassza ki **speciális konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="a32fd-117">Select **Advanced rule configuration**.</span></span>  
   <span data-ttu-id="a32fd-118">Ez a nézet a lapon megadott a kijelölt szabálykészletet az összes szabály csoportot táblázatát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a32fd-118">This view shows a table on the page of all the rule groups provided with the chosen rule set.</span></span> <span data-ttu-id="a32fd-119">A szabály jelölőnégyzetekből ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="a32fd-119">All of the rule's check boxes are selected.</span></span>

![Letiltott szabályok konfigurálása][1]

## <a name="search-for-rules-to-disable"></a><span data-ttu-id="a32fd-121">Tiltsa le a szabályok keresése</span><span class="sxs-lookup"><span data-stu-id="a32fd-121">Search for rules to disable</span></span>

<span data-ttu-id="a32fd-122">A **webes alkalmazás tűzfalbeállítások** panel lehetővé teszi a szabályok egy szöveges keresés keresztül szűrése.</span><span class="sxs-lookup"><span data-stu-id="a32fd-122">The **Web application firewall settings** blade provides the capability to filter the rules through a text search.</span></span> <span data-ttu-id="a32fd-123">Az eredmény csak a csoportok és a szabályt, amely tartalmazza a keresett szöveget jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a32fd-123">The result displays only the rule groups and rules that contain the text you searched for.</span></span>

![Szabályok keresése][2]

## <a name="disable-rule-groups-and-rules"></a><span data-ttu-id="a32fd-125">Tiltsa le a csoportok és szabályok</span><span class="sxs-lookup"><span data-stu-id="a32fd-125">Disable rule groups and rules</span></span>

<span data-ttu-id="a32fd-126">Ha az még szabályok letiltása, letilthatja egy teljes csoport vagy a egy vagy több szabály csoportok meghatározott szabályokat.</span><span class="sxs-lookup"><span data-stu-id="a32fd-126">When your're disabling rules, you can disable an entire rule group or specific rules under one or more rule groups.</span></span> 

<span data-ttu-id="a32fd-127">**Csoportok és a speciális szabályok letiltása**</span><span class="sxs-lookup"><span data-stu-id="a32fd-127">**To disable rule groups or specific rules**</span></span>

   1. <span data-ttu-id="a32fd-128">A szabályok vagy letiltani kívánt csoportok kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="a32fd-128">Search for the rules or rule groups that you want to disable.</span></span>
   2. <span data-ttu-id="a32fd-129">Törölje a jelölőnégyzet a letiltani kívánt.</span><span class="sxs-lookup"><span data-stu-id="a32fd-129">Clear the check boxes for the rules that you want to disable.</span></span> 
   2. <span data-ttu-id="a32fd-130">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a32fd-130">Select **Save**.</span></span> 

![Módosítások mentése][3]

## <a name="next-steps"></a><span data-ttu-id="a32fd-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a32fd-132">Next steps</span></span>

<span data-ttu-id="a32fd-133">Miután konfigurálta a letiltott szabályok, megismerheti a WAF naplók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a32fd-133">After you configure your disabled rules, you can learn how to view your WAF logs.</span></span> <span data-ttu-id="a32fd-134">További információkért lásd: [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md#diagnostic-logging).</span><span class="sxs-lookup"><span data-stu-id="a32fd-134">For more information, see [Application Gateway diagnostics](application-gateway-diagnostics.md#diagnostic-logging).</span></span>

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
