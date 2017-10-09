---
title: "aaaProtecting a virtuális gépeket az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum címek, amelyek segítenek az Azure Security Center javaslatait a virtuális gépet véd, és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: 926c04974f61215b4a3e02646f23dafb87c793e8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Az Azure Security Centerben a virtuális gépek védelme
Az Azure Security Center elemzi az Azure-erőforrások hello biztonsági állapotát. Amikor a Security Center a potenciális biztonsági hiányosságokat azonosít, hello folyamatán hello szükséges vezérlők, amelyek javaslatokat hoz létre.  Javaslatok érvényesek tooAzure erőforrástípusok: virtuális gépek (VM), hálózati, SQL és alkalmazások.

Ez a cikk foglalkozik tooVMs vonatkozó javaslatokat.  VM javaslatok center körül adatgyűjtés kiépítése, rendszer frissítéseinek alkalmazása a virtuális gépek lemezei, és több titkosítását.  Használjon hello az alábbi táblázatban a hivatkozás toohelp, tudomásul veszi hello elérhető virtuális gépekre vonatkozó javaslatok, és hogy a minden egyes milyen fogja elvégezni, ha alkalmazza azt.

## <a name="available-vm-recommendations"></a>Rendelkezésre álló virtuális gépekre vonatkozó javaslatok
| Ajánlás | Leírás |
| --- | --- |
| [Adatgyűjtés engedélyezése az előfizetések számára](security-center-enable-data-collection.md) |Javasolja, hogy kapcsolja be hello biztonsági házirend adatgyűjtés minden egyes előfizetésnél és az összes virtuális gépek (VM) az Ön előfizetéseit. |
| [Engedélyezze a titkosítást az Azure Storage-fiók](security-center-enable-encryption-for-storage-account.md) | Az inaktív adatok Azure Storage szolgáltatás titkosítási engedélyezését javasolja. Storage Service Encryption (SSE) működik tooAzure tárolási írása és beolvasása előtt visszafejti hello adatok titkosításával. SSE jelenleg csak hello Azure Blob szolgáltatáshoz érhető el, és nem használható blokkblobokat, lapblobokat, és hozzáfűző blobokat. több, lásd: toolearn [Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md).</br>SSE csak erőforrás-kezelő tárfiókok esetén támogatott. Klasszikus tárfiókokba jelenleg nem támogatottak. toounderstand hello klasszikus és Resource Manager üzembe helyezési modellel, lásd: [Azure üzembe helyezési modellel](../azure-classic-rm.md). |
| [Operációs rendszerek sebezhetőségeinek javítása](security-center-remediate-os-vulnerabilities.md) |Az operációs rendszer azon konfigurációinak igazodni ajánlott a konfigurációs szabályok hello javasolja például nem engedélyezik a mentett jelszavak toobe. |
| [Rendszerfrissítések alkalmazása](security-center-apply-system-updates.md) |Javasolja a hiányzó rendszer biztonsági és kritikus frissítések tooVMs telepíteni. |
| [A közvetlenül időponthoz kötött alkalmazni a hálózati hozzáférés-vezérlés](security-center-just-in-time.md) | Csak a virtuális gép elérhető telepítését javasolja. hello csak az idő a szolgáltatás jelenleg előzetes és hello a Security Center Standard csomagban érhető el. Lásd: [árazás](security-center-pricing.md) toolearn bővebben a Security Center által tarifacsomag szükséges. |
| [Rendszerfrissítések utáni újraindítás](security-center-apply-system-updates.md#reboot-after-system-updates) |Javasolja, hogy indítsa újra a virtuális gép toocomplete hello folyamat a rendszer frissítéseinek alkalmazása. |
| [Endpoint Protection telepítése](security-center-install-endpoint-protection.md) |Javasolja, hogy mértékben kártevőirtó programok tooVMs (csak Windows virtuális gépek esetén). |
| [Endpoint Protection állapotriasztások feloldása](security-center-resolve-endpoint-protection-health-alerts.md) |Javasolja, hogy hárítsa el az Endpoint Protection használatával kapcsolatos hibákat. |
| [Virtuálisgép-ügynök engedélyezése](security-center-enable-vm-agent.md) |Lehetővé teszi, hogy toosee virtuális gépek igénylő hello Virtuálisgép-ügynök. rendelés tooprovision javítás vizsgálatát, Alapterv vizsgálatát, és a kártevőirtó-programok virtuális gépeken hello ügynököt kell telepíteni. Virtuálisgép-ügynök hello alapértelmezés szerint telepítve van a hello Azure Piactérről származó központilag telepített virtuális gépekhez. hello cikk [ügynök és Virtuálisgép-bővítmények – 2. rész](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) bemutatja, hogyan tooinstall hello Virtuálisgép-ügynök. |
| [Lemeztitkosítás alkalmazása](security-center-apply-disk-encryption.md) |Javasolja, hogy végezze el a virtuális gép titkosítását az Azure Disk Encryption használatával (Windows és Linux rendszerű virtuális gépek esetében). Titkosítási ajánlott hello az operációs rendszer, mind az adatkötetek a virtuális gépen. |
| [Operációs rendszer verziójának frissítése](security-center-update-os-version.md) |Azt javasolja, hogy frissíteni hello operációs rendszer verziója a felhőalapú szolgáltatás toohello legfrissebb verzió érhető el az operációsrendszer-család esetében.  További információk a Cloud Services toolearn lásd: hello [felhőalapú szolgáltatások – áttekintés](../cloud-services/cloud-services-choose-me.md). |
| [A sebezhetőségi felmérés nincs telepítve](security-center-vulnerability-assessment-recommendations.md) |Javasolja, hogy telepítsen egy biztonsági rések felmérése szolgáló megoldást a virtuális gépére. |
| [Sebezhetőségek javítása](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Lehetővé teszi a toosee rendszer és az alkalmazás által észlelt biztonsági rések hello biztonsági rés értékelési megoldás a virtuális Gépre telepítve. |

## <a name="see-also"></a>Lásd még:
További információ az tooother Azure erőforrástípusok, vonatkozó javaslatok toolearn hello következő lásd:

* [Az alkalmazások az Azure Security Centerben védelme](security-center-application-recommendations.md)
* [Az Azure Security Centerben a hálózat védelme](security-center-network-recommendations.md)
* [Az Azure Security Centerben az Azure SQL-szolgáltatás védelme](security-center-sql-service-recommendations.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
