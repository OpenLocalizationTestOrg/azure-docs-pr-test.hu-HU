---
title: "aaaProtecting a hálózat az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum címek, amelyek segítenek az Azure Security Center javaslatait az Azure-hálózat védelme és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 96c55a02-afd6-478b-9c1f-039528f3dea0
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: terrylan
ms.openlocfilehash: 053738da432edf13b40172fb44d2044702dd8211
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-network-in-azure-security-center"></a>Az Azure Security Centerben a hálózat védelme
Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, hello folyamatán hello szükséges vezérlők, amelyek javaslatokat hoz létre.  Javaslatok érvényesek tooAzure erőforrástípusok: virtuális gépek (VM), hálózati, SQL és alkalmazások.

Ez a cikk foglalkozik tooyour hálózati vonatkozó javaslatokat.  Hálózati javaslatok központ következő generációs tűzfal, a hálózati biztonsági csoportok, az konfigurálása bejövő forgalomra vonatkozó szabályokat és több.  Használjon hello az alábbi táblázatban, egy hivatkozás toohelp ismeri hello rendelkezésre álló hálózati javaslatok és minden egyes funkciója Ha alkalmazása.

## <a name="available-network-recommendations"></a>Rendelkezésre álló hálózati javaslatok
| Ajánlás | Leírás |
| --- | --- |
| [Újgenerációs tűzfal hozzáadása](security-center-add-next-generation-firewall.md) |Javasolja, hogy ad hozzá a következő generációs tűzfal (NGFW) az egy Microsoft partnert tooincrease a biztonsági védelmet. |
| [Csak az újgenerációs tűzfalon keresztül haladjon a forgalom](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Javasolja, hogy konfigurálja a hálózati biztonsági csoport (NSG) szabályok, amelyek a bejövő forgalom tooyour keresztül az NGFW VM kényszerítése. |
| [Hálózati biztonsági csoportok engedélyezi az alhálózatokat vagy a virtuális gépek](security-center-enable-network-security-groups.md) |Az alhálózatok és virtuális gépek NSG-k engedélyezését javasolja. |
| [Internetre irányuló végpont-en keresztüli hozzáférés korlátozása](security-center-restrict-access-through-internet-facing-endpoints.md) |Azt javasolja, hogy az NSG-ket konfigurálhat a bejövő forgalomra vonatkozó szabályokat. |

## <a name="see-also"></a>Lásd még:
További információ az tooother Azure erőforrástípusok, vonatkozó javaslatok toolearn hello következő lásd:

* [Az Azure Security Centerben a virtuális gépek védelme](security-center-virtual-machine-recommendations.md)
* [Az alkalmazások az Azure Security Centerben védelme](security-center-application-recommendations.md)
* [Az Azure Security Centerben az Azure SQL-szolgáltatás védelme](security-center-sql-service-recommendations.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
