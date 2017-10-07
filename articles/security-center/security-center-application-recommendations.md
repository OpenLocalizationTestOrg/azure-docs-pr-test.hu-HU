---
title: "aaaProtecting az alkalmazások az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum címek, amelyek segítenek az Azure Security Center javaslatait az Azure-alkalmazások védelmét, és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: b5fc7a9e-24b1-415f-b3b5-62a53f5dd424
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: terrylan
ms.openlocfilehash: da5e02cc2bad55c64e4da14e4e10efd6ddeab39e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-applications-in-azure-security-center"></a>Az alkalmazások az Azure Security Centerben védelme
Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, hello folyamatán hello szükséges vezérlők, amelyek javaslatokat hoz létre.  Javaslatok érvényesek tooAzure erőforrástípusok: virtuális gépek (VM), hálózati, SQL és alkalmazások.

Ez a cikk foglalkozik tooapplications vonatkozó javaslatokat.  Alkalmazás javaslatok center körül webalkalmazási tűzfal központi telepítését.  Használjon hello az alábbi táblázatban a hivatkozás toohelp, ismeri hello rendelkezésre álló javaslatok és minden egyes funkciója Ha alkalmazza azt.

## <a name="available-application-recommendations"></a>Rendelkezésre álló javaslatok
| Ajánlás | Leírás |
| --- | --- |
| [Webalkalmazási tűzfal hozzáadása](security-center-add-web-application-firewall.md) |Javasolja, hogy telepít egy webalkalmazási tűzfal (WAF) webszolgáltatási végpontok. WAF ajánlás bármely nyilvános felé néző IP-cím (példány szint IP vagy terhelés eloszlik IP), amely rendelkezik egy társított hálózati biztonsági csoportot a nyissa meg a bejövő webes portokkal (80,443) jelenik meg.</br></br>A Security Center javasolja, hogy egy WAF toohelp kiépítése a webalkalmazások virtuális gépek és az App Service Environment-környezet célzó támadások elleni védelemre-e. Az App Service környezetben (ASE) van egy [prémium](https://azure.microsoft.com/pricing/details/app-service/) terv lehetőséget az Azure App Service egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására az Azure App Service apps biztosító szolgáltatás. toolearn ASE, kapcsolatos további információkért lásd: hello [App Service környezetben dokumentáció](../app-service/app-service-app-service-environments-readme.md).</br></br>A Security Center több webalkalmazás védelmet biztosíthat a következő alkalmazások tooyour meglévő WAF központi telepítések hozzáadásával. |
| [Alkalmazásvédelem véglegesítése](security-center-add-web-application-firewall.md#finalize-application-protection) |WAF, forgalom toocomplete hello konfigurálása átirányított toohello WAF készüléknek kell lennie. Ez a javaslat a következő befejezése hello szükséges telepítőfájlokat módosításokat. |

## <a name="see-also"></a>Lásd még:
További információ az tooother Azure erőforrástípusok, vonatkozó javaslatok toolearn hello következő lásd:

* [Az Azure Security Centerben a virtuális gépek védelme](security-center-virtual-machine-recommendations.md)
* [Az Azure Security Centerben a hálózat védelme](security-center-network-recommendations.md)
* [Az Azure Security Centerben az Azure SQL-szolgáltatás védelme](security-center-sql-service-recommendations.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
