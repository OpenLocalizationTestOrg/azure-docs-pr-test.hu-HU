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
# <a name="customize-web-application-firewall-rules-through-hello-azure-portal"></a>Webes alkalmazás tűzfalszabályok hello Azure-portálon keresztül testreszabása

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-customize-waf-rules-portal.md)
> * [PowerShell](application-gateway-customize-waf-rules-powershell.md)
> * [Azure CLI 2.0](application-gateway-customize-waf-rules-cli.md)

hello Azure Application Gateway webalkalmazási tűzfal (WAF) webalkalmazások védelmet biztosít. A védelem hello megnyitása webes alkalmazás biztonsági Project (OWASP) Core szabály beállítása (CRS) által biztosított. Néhány szabály okozhat a vakriasztások és valós adatforgalmat. Emiatt a Application Gateway hello funkció toocustomize csoportok és a szabályok biztosít. Adott hello csoportok és a szabályok további információkért lásd: [webes alkalmazás tűzfal CRS csoportok és a szabályok listája](application-gateway-crs-rulegroups-rules.md).

>[!NOTE]
> Ha az alkalmazás átjáró hello WAF réteg nem használ, hello beállítás tooupgrade hello alkalmazás átjáró toohello WAF réteg hello jobb oldali ablaktáblán jelenik meg. 

![WAF engedélyezése][fig1]

## <a name="view-rule-groups-and-rules"></a>A szabály csoportok megtekintése és szabályok

**tooview csoportok és szabályok**
   1. Keresse meg az Alkalmazásátjáró toohello, és válassza ki **webalkalmazási tűzfal**.  
   2. Válassza ki **speciális konfiguráció**.  
   Ebben a nézetben látható egy tábla összes hello szabály csoportok szabálykészlet kiválasztott hello hello oldalán. Hello szabály jelölőnégyzetekből ki van jelölve.

![Letiltott szabályok konfigurálása][1]

## <a name="search-for-rules-toodisable"></a>Szabályok toodisable keresése

Hello **webes alkalmazás tűzfalbeállítások** panel hello képességet biztosít toofilter hello szabályok segítségével a szöveges keresés. csak a hello csoportok és a szabályt, amely tartalmazza a keresett szöveget hello hello eredményt jeleníti meg.

![Szabályok keresése][2]

## <a name="disable-rule-groups-and-rules"></a>Tiltsa le a csoportok és szabályok

Ha az még szabályok letiltása, letilthatja egy teljes csoport vagy a egy vagy több szabály csoportok meghatározott szabályokat. 

**toodisable csoportok vagy speciális szabályok**

   1. Hello szabályok vagy a megjeleníteni kívánt toodisable csoportok kereséséhez.
   2. Hello szabályok, amelyet az toodisable, törölje a hello jelölőnégyzetet. 
   2. Kattintson a **Mentés** gombra. 

![Módosítások mentése][3]

## <a name="next-steps"></a>Következő lépések

Miután konfigurálta a letiltott szabályok, áttekintheti, hogyan tooview a WAF naplókat. További információkért lásd: [Alkalmazásátjáró diagnosztika](application-gateway-diagnostics.md#diagnostic-logging).

[fig1]: ./media/application-gateway-customize-waf-rules-portal/1.png
[1]: ./media/application-gateway-customize-waf-rules-portal/figure1.png
[2]: ./media/application-gateway-customize-waf-rules-portal/figure2.png
[3]: ./media/application-gateway-customize-waf-rules-portal/figure3.png
