---
title: "Azure SQL aaaProtecting szolgáltatás és az adatok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum címek, amelyek segítenek az Azure Security Center javaslatait védeni kell az adatok és az Azure SQL-szolgáltatás, és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 75d782d3c2418f9645139e4cd6ecb7765c488f91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Azure SQL-szolgáltatás és az Azure Security Center adatainak védelme
Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, hello folyamatán hello szükséges vezérlők, amelyek javaslatokat hoz létre.  Javaslatok érvényesek tooAzure erőforrástípusok: virtuális gépek (VM), hálózati, SQL és az adatokhoz és alkalmazásokhoz.

Ez a cikk foglalkozik tooAzure SQL-szolgáltatás és az adatok vonatkozó javaslatokat. Javaslatok center körül naplózás engedélyezése az Azure SQL-kiszolgálók és adatbázisok, SQL-adatbázisokhoz tartozó titkosítási és az Azure storage-fiók engedélyezése titkosításának engedélyezése.  Használjon hello az alábbi táblázatban a hivatkozás toohelp, ismeri hello elérhető SQL szolgáltatás és az adatok javaslatok és minden egyes funkciója Ha alkalmazza azt.

## <a name="available-sql-service-and-data-recommendations"></a>Elérhető az SQL szolgáltatás és az adatok javaslatok
| Ajánlás | Leírás |
| --- | --- |
| [Naplózás és fenyegetésészlelés engedélyezése az SQL-kiszolgálókon](security-center-enable-auditing-on-sql-servers.md) |Javasolja, hogy kapcsolja be az Azure SQL-kiszolgálókra (Azure SQL-szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL) a naplózás és a fenyegetések észlelésére. |
| [Naplózás és fenyegetésészlelés engedélyezése az SQL-adatbázisokon](security-center-enable-auditing-on-sql-databases.md) |Javasolja, hogy kapcsolja be az Azure SQL-adatbázisok (Azure SQL-szolgáltatás csak; nem tartalmazza a virtuális gépeken futó SQL) a naplózás és a fenyegetések észlelésére. |
| [Az átlátható adattitkosítási engedélyezése az SQL-adatbázisok](security-center-enable-transparent-data-encryption.md) |Titkosítás az SQL-adatbázisok (csak Azure SQL szolgáltatás) engedélyezését javasolja. |

## <a name="see-also"></a>Lásd még:
További információ az tooother Azure erőforrástípusok, vonatkozó javaslatok toolearn hello következő lásd:

* [Az Azure Security Centerben a virtuális gépek védelme](security-center-virtual-machine-recommendations.md)
* [Az alkalmazások az Azure Security Centerben védelme](security-center-application-recommendations.md)
* [Az Azure Security Centerben a hálózat védelme](security-center-network-recommendations.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
