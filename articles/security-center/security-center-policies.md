---
title: "aaaSet biztonsági házirendek az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum segít tooconfigure biztonsági házirendek az Azure Security Centerben."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: yurid
ms.openlocfilehash: 59226dd84a1c66a2d8367417060ab10a1ff73848
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-security-policies-in-azure-security-center"></a>Biztonsági szabályzatok beállítása az Azure Security Centerben
Ez a dokumentum segít tooconfigure biztonsági házirendek segítségével a Security Center végigvezeti hello szükséges lépéseket tooperform ezt a feladatot.

>[!NOTE] 
>Korai. június 2017 verziótól kezdve a Security Center hello Microsoft Monitoring Agent toocollect és a tároló adatait használja. Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn. a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>

## <a name="what-are-security-policies"></a>Mik azok a biztonsági szabályzatok?
A biztonsági házirend vezérlők, javasolt a megadott hello erőforrások hello csoportját határozza meg előfizetés. A Security Center, szabályzatok készítése az Azure-előfizetések tooyour vállalati biztonsági igényeket és hello szereplő alkalmazások típusának vagy az egyes előfizetések hello adatok érzékenységének megfelelően.

Különböző biztonsági követelmények vonatkozhatnak például a fejlesztésben vagy tesztelésben, illetve az éles környezetben használt erőforrásokra. A szabályozott adatokat, például személyazonosításra alkalmas adatokat használó alkalmazások is magasabb szintű biztonságot követelhetnek meg. Biztonsági házirendeket, amelyek engedélyezve vannak az Azure Security Center meghajtó biztonsági javaslatok és figyelési toohelp azonosítani a potenciális biztonsági réseket és elhárítani a fenyegetéseket. Olvasási [Azure Security Center tervezéséhez és az üzemeltetési útmutatóban](security-center-planning-and-operations-guide.md) hogyan toodetermine hello lehetőséget is kapcsolatos további információk az Ön megfelelő érték.

## <a name="set-security-policies"></a>Biztonsági házirendek beállítása
Az egyes előfizetésekhez külön-külön biztonsági szabályzatot állíthat be. a biztonsági házirend toomodify tulajdonosának vagy közreműködőjének előfizetésben kell lennie. Jelentkezzen be Azure-portálon toohello, és hajtsa végre a következő lépéseket a Security Center tooconfigure biztonsági szabályzatainak hello:

1. Kattintson a hello **házirend** hello Security Center irányítópultjának csempére.
2. Hello biztonsági szabályzat paneljén megjelenő válassza ki kívánja tooenable hello biztonsági házirend hello előfizetést.

    ![A szabályzat definiálása](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. Hello **biztonsági házirend** lehetőségekkel hello kijelölt előfizetés panel nyílik meg. Ezen a panelen hello lehetőségek a következők:

   * **Megakadályozási szabályzat**: Ez a beállítás tooconfigure házirendekkel előfizetésenként.  
   * **E-mailes értesítés**: használja ezt a beállítást tooconfigure küldött e-mailben értesítést hello első napi előfordulása riasztási és magas súlyosságú riasztások esetén. Az e-mail-beállítások kizárólag előfizetési szabályok esetében konfigurálhatók. Olvasási [adja meg az Azure Security Centerben a biztonsági kapcsolattartási adatait](security-center-provide-security-contact-details.md) további információt tooconfigure e-mailben értesítést.
   * **IP-címek**: használja ezt a beállítást tooupgrade hello tarifacsomag kiválasztása. Lásd: [Security Center árképzési](security-center-pricing.md) toolearn további vonatkozó beállítások.
4. A **Collect data from virtual machines** (Adatgyűjtés a virtuális gépekről) beállítás értéke legyen **On** (Bekapcsolva). Ez a beállítás lehetővé teszi, hogy a Microsoft Monitoring Agent hello használata meglévő és új erőforrásokra vonatkozó automatikus naplógyűjtést – Ez a hello hello Operations Management Suite és Naplóelemzés szolgáltatás által használt ugyanannak az ügynöknek. Ez az ügynök gyűjtött adatokat tárolja vagy egy meglévő Naplóelemzési munkaterületek az Azure-előfizetéshez társított, vagy egy új munkaterületek fiók hello földrajzi hely, a virtuális gép hello figyelembevételével.

5. A hello **biztonsági házirend** panelen kattintson a **megakadályozási szabályzat** toosee hello elérhető beállítások. Kattintson a **a** tooenable hello biztonsági javaslatok, amely kapcsolódik ehhez az előfizetéshez.

    ![Hello biztonsági szabályzatok kiválasztása](./media/security-center-policies/security-center-policies-fig7.png)

Használja a következő táblázat a referencia-toounderstand, hello minden beállítást:

| Szabályzat | Bekapcsolt állapotban |
| --- | --- |
| System updates (Rendszerfrissítések) |Lekéri az elérhető biztonsági és kritikus frissítések napi listáját a Windows Update vagy a Windows Server Update Services szolgáltatástól. hello lekért listában hello szolgáltatástól függ, amely a virtuális gép van konfigurálva, és azt javasolja, hogy hello hiányzó frissítések alkalmazását. A Linux rendszerek esetében a hello házirend hello distro által biztosított csomag felügyeleti rendszer toodetermine rendelkező csomagok építését rendelkezésre álló frissítéseket használja. Az [Azure Cloud Services](../cloud-services/cloud-services-how-to-configure.md) virtuális gépeitől származó biztonsági és kritikus frissítéseket is keres. |
| OS vulnerabilities (Operációs rendszerek sebezhetőségei) |Operációs rendszer konfigurációk napi toodetermine problémákat sikerült hello virtuális gép sebezhető tooattack elemzi. hello házirend is azt javasolja, hogy konfigurációs módosításokat tooaddress biztonsági rések. Lásd: hello [javasolt alapkonfigurációk listáját](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) figyelt hello konfigurációkkal kapcsolatos további információk. (A Windows Server 2016 jelenleg nem részesül teljes mértékű támogatásban.) |
| Endpoint protection (Végpontok védelme) |Az endpoint protection toobe az összes Windows virtuális gép toohelp azonosításához, és távolítsa el a vírusok, kémprogramok és más, kártevő szoftverek kiosztása javasolja. |
| Disk encryption (Lemeztitkosítás) |Az összes virtuális gépek tooenhance adatvédelem inaktív adatok titkosítása engedélyezése javasolja. |
| Network security groups (Hálózati biztonsági csoportok) |Azt javasolja, hogy [hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md) konfigurált toocontrol kell a bejövő és kimenő forgalom tooVMs rendelkező nyilvános végpontok. Az alhálózatra beállított hálózati biztonsági csoportokat az összes virtuális géphez tartozó hálózati adapter örökli, kivéve, ha Ön más beállítást ad meg. Továbbá a toochecking, hogy a hálózati biztonsági csoport van konfigurálva, a házirend értékelésére bejövő biztonsági szabályok tooidentify szabályok, amelyek engedélyezik a bejövő forgalmat. |
| Web application firewall (Webalkalmazási tűzfal) |Javasolja, hogy webalkalmazási tűzfal építhető virtuális gépeken, ha hello következő valamelyike teljesül: </br></br>[Példányszintű nyilvános IP-cím](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) használja, és hello hello társított hálózati biztonsági csoport bejövő biztonsági szabályait konfigurált tooallow hozzáférés tooport 80/443-as.</br></br>Elosztott terhelésű IP használt hello tartozó terheléselosztási és bejövő hálózati cím címfordítási (NAT) szabályok konfigurált tooallow hozzáférés tooport 80/443-as. (A további információkat az [Azure Resource Manager support for Load Balancer](../load-balancer/load-balancer-arm.md) (Az Azure Resource Manager támogatása a terheléselosztó számára)) című rész tartalmazza. |
| Next generation firewall (Új generációs tűzfal) |Az Azure-ba épített hálózati biztonsági csoportokon túlra is kiterjesztheti a hálózati védelmet. A Security Center központi telepítéseket, amelyek új generációs tűzfal ajánlott, és engedélyezze a virtuális készülék tooprovision deríti fel. |
| SQL-naplózás és fenyegetésészlelés |Azt javasolja, hogy hozzáférési tooAzure adatbázis naplózásának kell engedélyezve van a megfelelőség is speciális fenyegetések észlelése, a nyomozás érdekében. |
| SQL-titkosítás |Javasolja, hogy engedélyezze az inaktív adatok titkosítását az Azure SQL Database számára, valamint az azokhoz kapcsolódó biztonsági mentési és tranzakciós naplófájlokra vonatkozóan. Így hiába jutnak be illetéktelen személyek a rendszerbe, az adatokat nem fogják tudni olvasni. |
| Sebezhetőségi felmérés |Javasolja, hogy telepítsen egy biztonsági rések felmérése szolgáló megoldást a virtuális gépére. |
| Storage-titkosítás |Ez a szolgáltatás jelenleg Azure-blobok és -fájlok számára érhető el. A Storage szolgáltatás titkosításának engedélyezése után csak az új adatok lesznek titkosítva, és a tárfiók meglévő fájljai titkosítatlanok maradnak. |
| JIT hálózati hozzáférés |Ha JIT engedélyezve van, a Security Center zárolja bejövő forgalom tooyour Azure virtuális gépek egy NSG-szabály létrehozásával. Kiválaszthatja hello hello VM toowhich a bejövő forgalom zárolni kell. További információk: [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (A virtuális gépekhez való hozzáférés kezelése igény szerinti hozzáférés használata esetén). |

Minden beállítás megadása után kattintson az **OK** hello a **biztonsági házirend** panel, amelyen hello javaslatokat, és kattintson a **mentése** a hello **biztonsági Házirend** panel, amelyen hello kezdeti beállítása.

> [!NOTE]
> IP-címek hello esetén hello erőforráscsoport szintjén is alkalmazható. További információt a látogató hello [árazás](https://azure.microsoft.com/pricing/details/security-center/) lap.
>
>

## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan tooconfigure biztonsági házirendek az Azure Security Centerben. További információ az Azure Security Center toolearn hello következő lásd:

* [Útmutató az Azure Security Center tervezéséhez és működtetéséhez](security-center-planning-and-operations-guide.md). Megtudhatja, hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése.
* [Biztonsági állapotfigyelés az Azure Security Centerben](security-center-monitoring.md). Ismerje meg, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md). Megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Centerrel](security-center-partner-solutions.md). Ismerje meg, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center – gyakori kérdések](security-center-faq.md) Hello szolgáltatással kapcsolatos gyakran ismételt kérdések található.
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
