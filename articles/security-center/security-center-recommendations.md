---
title: "aaaManaging biztonsági javaslatok az Azure Security Centerben |} Microsoft Docs"
description: "A dokumentumból megtudhatja, hogyan javaslatok az Azure Security Centerben segítséget nyújtanak az Azure-erőforrások védelme és maradnak meg a biztonsági házirendeknek megfelelően."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: terrylan
ms.openlocfilehash: f6bbe36a7a5636095b339b3e9765b87cc0ab669a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-security-recommendations-in-azure-security-center"></a>Biztonsági javaslatok kezelése az Azure Security Centerben
A dokumentumból megtudhatja, hogyan toouse javaslatok az Azure Security Center toohelp védeni az Azure-erőforrások.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.  Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="what-are-security-recommendations"></a>Mik azok a biztonsági javaslatok?
A Security Center rendszeresen elemzi az Azure-erőforrások biztonsági állapotának hello. A Security Center javaslatokat hoz létre, amikor lehetséges biztonsági réseket észlel. hello javaslatok végigvezetik hello hello szükséges vezérlők konfigurálásának lépésein.

## <a name="implementing-security-recommendations"></a>Biztonsági javaslatok megvalósítása
### <a name="set-recommendations"></a>Set javaslatok
A [biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md), ismerje meg, hogy:

* Biztonsági szabályzatainak konfigurálását.
* Bekapcsolni az adatgyűjtést.
* Válassza ki a biztonsági szabályzat részeként mely ajánlások toosee.

Aktuális javaslatok szabályzatközpontban körül Rendszerfrissítések, alapkonfigurációs szabályok, kártevőirtó-programok, [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md) alhálózatok és a hálózati adapterek, SQL-adatbázis naplózásának, SQL adatbázis átlátható adattitkosítás és webalkalmazási tűzfalak.  [Biztonsági szabályzatok beállítása](security-center-policies.md) minden javaslat beállítás leírását.

### <a name="monitor-recommendations"></a>A figyelő javaslatok
Miután beállította a biztonsági házirendet, a Security Center elemzi az erőforrások tooidentify potenciális biztonsági réseket hello biztonsági állapotát. Hello **javaslatok** hello csempét **Security Center** panel lehetővé teszi, hogy ismeri a Security Center által azonosított javaslatok hello teljes száma.

![Javaslatok csempe][1]

minden ajánlást toosee hello részleteit:

Jelölje be hello **javaslatok csempe** hello a **Security Center** panelen. Hello **javaslatok** panel nyílik meg.

hello javaslatok ahol mindegyik sor jelenti-e egy adott javaslat táblázatos formátumban jelennek meg. a tábla oszlopainak hello a következők:

* **Leírás**: hello javaslat, és mit tooaddress végzett toobe ismerteti azt.
* **ERŐFORRÁS**: hello erőforrások toowhich vonatkozik ez a javaslat sorolja fel.
* **ÁLLAPOT**: hello hello javaslat aktuális állapota ismerteti:
  * **Nyissa meg**: hello javaslat még nem foglalkoztak.
  * **A folyamatban lévő**: hello javaslat jelenleg folyamatban van a alkalmazott toohello erőforrásokat, és nincs szükség felhasználói műveletek is.
  * **Megoldott**: hello javaslat már befejeződött (ebben az esetben hello sor szürkén jelenik meg).
* **SÚLYOSSÁGI**: ismerteti, hogy adott javaslat súlyosságát hello:
  * **Magas**: A biztonsági rés fontos erőforrásnál (például egy alkalmazást, a virtuális gép vagy a hálózati biztonsági csoport) létezik, ezért beavatkozást igényel.
  * **Közepes**: A biztonsági rés és nem kritikus vagy kiegészítő lépések elvégzése szükséges tooeliminate vagy toocomplete folyamat.
  * **Alacsony**: A biztonsági rés kell tömni, de nem igényel azonnali beavatkozást. (Alapértelmezés szerint alacsony súlyosságú javaslatok nem jelenik meg a, de ha azt szeretné, hogy toosee alacsony súlyosságú javaslatok végezhet azokat.)

Használjon hello az alábbi táblázatban, egy hivatkozás toohelp ismeri hello elérhető javaslatok és minden egyes funkciója Ha alkalmazása.

> [!NOTE]
> Érdemes toounderstand hello [klasszikus és Resource Manager üzembe helyezési modell](../azure-classic-rm.md) az Azure-erőforrások.
>
>

| Ajánlás | Leírás |
| --- | --- |
| [Adatgyűjtés engedélyezése az előfizetések számára](security-center-enable-data-collection.md) |Javasolja, hogy kapcsolja be hello biztonsági házirend adatgyűjtés minden egyes előfizetésnél és az összes virtuális gépek (VM) az Ön előfizetéseit. |
| [Operációs rendszerek sebezhetőségeinek javítása](security-center-remediate-os-vulnerabilities.md) |Javasolja, hogy az operációs rendszer azon konfigurációinak megfelel-e a konfigurációs szabályok, például ajánlott hello, ne engedélyezze a mentett jelszavak toobe. |
| [Rendszerfrissítések alkalmazása](security-center-apply-system-updates.md) |Javasolja a hiányzó rendszer biztonsági és kritikus frissítések tooVMs telepíteni. |
| [A közvetlenül időponthoz kötött alkalmazni a hálózati hozzáférés-vezérlés](security-center-just-in-time.md) | Csak a virtuális gép elérhető telepítését javasolja. hello csak az idő a szolgáltatás jelenleg előzetes és hello a Security Center Standard csomagban érhető el. Lásd: [árazás](security-center-pricing.md) toolearn bővebben a Security Center által tarifacsomag szükséges. |
| [Rendszerfrissítések utáni újraindítás](security-center-apply-system-updates.md#reboot-after-system-updates) |Javasolja, hogy indítsa újra a virtuális gép toocomplete hello folyamat a rendszer frissítéseinek alkalmazása. |
| [Webalkalmazási tűzfal hozzáadása](security-center-add-web-application-firewall.md) |Javasolja, hogy telepít egy webalkalmazási tűzfal (WAF) webszolgáltatási végpontok. WAF ajánlás bármely nyilvános felé néző IP-cím (példány szint IP vagy terhelés eloszlik IP), amely rendelkezik egy társított hálózati biztonsági csoportot a nyissa meg a bejövő webes portokkal (80,443) jelenik meg. </br>A Security Center javasolja, hogy egy WAF toohelp kiépítése a webalkalmazások virtuális gépek és az App Service Environment-környezet célzó támadások elleni védelemre-e. Az App Service környezetben (ASE) van egy [prémium](https://azure.microsoft.com/pricing/details/app-service/) terv lehetőséget az Azure App Service egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására az Azure App Service apps biztosító szolgáltatás. toolearn ASE, kapcsolatos további információkért lásd: hello [App Service környezetben dokumentáció](../app-service/app-service-app-service-environments-readme.md).</br>A Security Center több webalkalmazás védelmet biztosíthat a következő alkalmazások tooyour meglévő WAF központi telepítések hozzáadásával. |
| [Alkalmazásvédelem véglegesítése](security-center-add-web-application-firewall.md#finalize-application-protection) |WAF, forgalom toocomplete hello konfigurálása átirányított toohello WAF készüléknek kell lennie. Ez a javaslat a következő befejezése hello szükséges telepítőfájlokat módosításokat. |
| [Újgenerációs tűzfal hozzáadása](security-center-add-next-generation-firewall.md) |Javasolja, hogy ad hozzá a következő generációs tűzfal (NGFW) az egy Microsoft partnert tooincrease a biztonsági védelmet. |
| [Csak az újgenerációs tűzfalon keresztül haladjon a forgalom](security-center-add-next-generation-firewall.md#route-traffic-through-ngfw-only) |Javasolja, hogy konfigurálja a hálózati biztonsági csoport (NSG) szabályok, amelyek a bejövő forgalom tooyour keresztül az NGFW VM kényszerítése. |
| [Endpoint Protection telepítése](security-center-install-endpoint-protection.md) |Javasolja, hogy mértékben kártevőirtó programok tooVMs (csak Windows virtuális gépek esetén). |
| [Endpoint Protection állapotriasztások feloldása](security-center-resolve-endpoint-protection-health-alerts.md) |Javasolja, hogy hárítsa el az Endpoint Protection használatával kapcsolatos hibákat. |
| [Hálózati biztonsági csoportok engedélyezi az alhálózatokat vagy a virtuális gépek](security-center-enable-network-security-groups.md) |Az alhálózatok és virtuális gépek NSG-k engedélyezését javasolja. |
| [Internetre irányuló végpont-en keresztüli hozzáférés korlátozása](security-center-restrict-access-through-internet-facing-endpoints.md) |Azt javasolja, hogy az NSG-ket konfigurálhat a bejövő forgalomra vonatkozó szabályokat. |
| [Naplózás és fenyegetésészlelés engedélyezése az SQL-kiszolgálókon](security-center-enable-auditing-on-sql-servers.md) |Javasolja, hogy kapcsolja be az Azure SQL-kiszolgálókra naplózás és a fenyegetések észlelésére. (Csak az azure SQL-szolgáltatás esetében. Nem tartalmazza a virtuális gépeken futó SQL.) |
| [Naplózás és fenyegetésészlelés engedélyezése az SQL-adatbázisokon](security-center-enable-auditing-on-sql-databases.md) |Javasolja, hogy kapcsolja be az Azure SQL-adatbázisok naplózásának és a fenyegetések észlelésére. (Csak az azure SQL-szolgáltatás esetében. Nem tartalmazza a virtuális gépeken futó SQL.) |
| [Az átlátható adattitkosítási engedélyezése az SQL-adatbázisok](security-center-enable-transparent-data-encryption.md) |Az SQL-adatbázisok titkosítási engedélyezését javasolja. (Az azure SQL-szolgáltatás csak.) |
| [Virtuálisgép-ügynök engedélyezése](security-center-enable-vm-agent.md) |Lehetővé teszi, hogy toosee virtuális gépek igénylő hello Virtuálisgép-ügynök. virtuális gépek tooprovision javítás vizsgálatát, Alapterv vizsgálatát, és a kártevőirtó-programok hello ügynököt telepíteni kell. Virtuálisgép-ügynök hello alapértelmezés szerint telepítve van a hello Azure Piactérről származó központilag telepített virtuális gépekhez. hello cikk [ügynök és Virtuálisgép-bővítmények – 2. rész](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) bemutatja, hogyan tooinstall hello Virtuálisgép-ügynök. |
| [Lemeztitkosítás alkalmazása](security-center-apply-disk-encryption.md) |Javasolja, hogy végezze el a virtuális gép titkosítását az Azure Disk Encryption használatával (Windows és Linux rendszerű virtuális gépek esetében). Titkosítási ajánlott hello az operációs rendszer, mind az adatkötetek a virtuális gépen. |
| [Biztonsági kapcsolattartói adatok megadása](security-center-provide-security-contact-details.md) |Javasolja, hogy megadja a biztonsági információk kérjen minden egyes előfizetésnél. Kapcsolattartási adatok egy e-mail címét és telefonszámát számát. hello információ használt toocontact, ha a biztonsági csoport megállapítja, hogy az erőforrások kerülnek veszélybe. |
| [Operációs rendszer verziójának frissítése](security-center-update-os-version.md) |Azt javasolja, hogy frissíteni hello operációs rendszer verziója a felhőalapú szolgáltatás toohello legfrissebb verzió érhető el az operációsrendszer-család esetében.  További információk a Cloud Services toolearn lásd: hello [felhőalapú szolgáltatások – áttekintés](../cloud-services/cloud-services-choose-me.md). |
| [A sebezhetőségi felmérés nincs telepítve](security-center-vulnerability-assessment-recommendations.md) |Javasolja, hogy telepítsen egy biztonsági rések felmérése szolgáló megoldást a virtuális gépére. |
| [Sebezhetőségek javítása](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Lehetővé teszi a toosee rendszer és az alkalmazás által észlelt biztonsági rések hello biztonsági rés értékelési megoldás a virtuális Gépre telepítve. |
| [Engedélyezze a titkosítást az Azure Storage-fiók](security-center-enable-encryption-for-storage-account.md) | Az inaktív adatok Azure Storage szolgáltatás titkosítási engedélyezését javasolja. Storage Service Encryption (SSE) működik tooAzure tárolási írása és beolvasása előtt visszafejti hello adatok titkosításával. SSE jelenleg csak hello Azure Blob szolgáltatáshoz érhető el, és nem használható blokkblobokat, lapblobokat, és hozzáfűző blobokat. több, lásd: toolearn [Storage szolgáltatás titkosítási az inaktív adatok](../storage/common/storage-service-encryption.md).</br>SSE csak erőforrás-kezelő tárfiókok esetén támogatott. |

Szűrés, és elvetésére vonatkozó javaslatok.

1. Válassza ki **szűrő** a hello **javaslatok** panelen. Hello **szűrő** panel megnyitása és toosee kívánja hello súlyossági és állapot értéket választja.

    ![Szűrő javaslatok][2]
2. Ha azt állapítja meg, hogy azt ajánljuk, nem alkalmazható, hagyja figyelmen kívül hello javaslat, és szűréssel a nézetben. Nincsenek két módon toodismiss ajánlás olyan környezetekben. Egyik módja van tooright kattintson egy elemet, és válassza **leállítási**. más hello toohover elemre, kattintson a három hello pontra, amely toohello jobb jelenik meg, majd válassza ki **leállítási**. Kattintva megtekintheti a elvetettként javaslatok **szűrő**, és jelölje be **Dismissed**.

    ![Hagyja figyelmen kívül a javaslat][3]

### <a name="apply-recommendations"></a>Javaslatok alkalmazása
Miután megtekintette a ajánlások, döntse el, amely egy alkalmazza először. Azt javasoljuk, hogy mely ajánlások először alkalmazni fő paraméter tooevaluate hello, hello besorolásával használja.

Hello tábla a fenti ajánlásokat, válassza ki azt ajánljuk, és végezze el az példa bemutatja, hogyan tooapply ajánlás olyan környezetekben.

## <a name="next-steps"></a>Következő lépések
A jelen dokumentum a Security Center javaslatait bevezetett toosecurity volt. További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – további hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

<!--Image references-->
[1]: ./media/security-center-recommendations/recommendations-tile.png
[2]: ./media/security-center-recommendations/filter-recommendations.png
[3]: ./media/security-center-recommendations/dismiss-recommendations.png
