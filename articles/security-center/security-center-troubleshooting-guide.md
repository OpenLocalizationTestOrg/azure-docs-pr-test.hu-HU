---
title: "Biztonsági központ hibaelhárítási útmutató aaaAzure |} Microsoft Docs"
description: "A dokumentum segítséget nyújt az Azure Security Centerben tootroubleshoot problémákat."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 78b3c49eb66fe3a4f80efbba3a47a87b039c07ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-troubleshooting-guide"></a>Azure Security Center – Hibaelhárítási útmutató
Ez az útmutató informatikai (IT) szakemberek, adatbiztonsági elemzők és felhő rendszergazdák, amelynek szervezetek az Azure Security Center használ, és szükség, tootroubleshoot Security Center kapcsolatos hiba lépett fel.

>[!NOTE] 
>Korai. június 2017 verziótól kezdve a Security Center hello Microsoft Monitoring Agent toocollect és a tároló adatait használja. Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn. a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>

## <a name="troubleshooting-guide"></a>Hibaelhárítási útmutató
Ez az útmutató ismerteti, hogyan tootroubleshoot Security Center kapcsolatos hiba lépett fel. Hello hibaelhárítási végezheti el a Security Center legtöbb történik hello első megtekintésével [napló](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) hello rekordok összetevő nem sikerült. A naplókból a következők állapíthatók meg:

* A végrehajtott műveletek
* Hello művelet korábban kik kezdeményeztek
* Ha a hello művelet történt
* hello művelet hello állapotát
* hello értékek, amelyek segíthetnek tulajdonságokat kutatás hello művelet

hello napló tartalmazza az erőforrásokon végrehajtott minden írási műveletek (PUT, POST, Törlés), azonban nem tartalmazza az olvasási műveletek (GET).

## <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
A Security Center a Microsoft Monitoring Agent hello használ, – hello ugyanannak az ügynöknek használt hello Operations Management Suite és Naplóelemzési service – az Azure virtuális gépek toocollect biztonsági adatait. Miután adatgyűjtés engedélyezve van, és hello ügynök megfelelően van telepítve a célszámítógépen hello, hello folyamat az alábbi végrehajtása kell lennie:

* HealthService.exe

Ha hello services management console (services.msc) megnyitásához is láthat hello Microsoft Monitoring Agent szolgáltatás fut alább látható módon:

![Szolgáltatások](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

toosee hello ügynök melyik verzióját telepítette, nyissa meg a **Feladatkezelő**, a hello **folyamatok** lapon keresse meg a hello **a Microsoft figyelési ügynök szolgáltatás**, kattintson a jobb gombbal a és Kattintson a **tulajdonságok**. A hello **részletek** fülre, tekintse meg a hello fájlverzió alább látható módon:

![Fájl](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## <a name="microsoft-monitoring-agent-installation-scenarios"></a>A Microsoft Monitoring Agent telepítési forgatókönyvei
Nincsenek két telepítési forgatókönyvek hello Microsoft Monitoring Agent telepítése a számítógép eltérő eredményeket eredményezhetnek. hello támogatott forgatókönyvek a következők:

* **A Security Center által automatikusan telepített ügynök**: Ebben a forgatókönyvben képes tooview hello riasztásokat a helyek, a Security Center és a keresési napló lesz. E-mail értesítések toohello e-mail címet a hello előfizetés hello erőforrás tartozik hello biztonsági házirendben beállított fog kapni.
.
* **Az Azure-ban található egy virtuális Gépet manuálisan telepített ügynök**: Ebben a forgatókönyvben, ha használ ügynökök letöltése és telepítése manuálisan előzetes tooFebruary 2017, csak akkor, ha szűrheti a hello képes tooview hello riasztásokat a Security Center portál hello lesz előfizetés hello munkaterület tartozik. Abban az esetben szűrő hello előfizetés hello erőforráshoz tartozik, akkor nem fogja tudni toosee e riasztások. E-mail értesítések toohello e-mail címet a hello előfizetés hello munkaterület tartozik hello biztonsági házirendben beállított fog kapni.

>[!NOTE]
> tooavoid hello viselkedését, tekintse meg a hello második, ellenőrizze, hogy hello hello ügynök legújabb verziójának letöltése.
> 

## <a name="troubleshooting-monitoring-agent-network-requirements"></a>A figyelőügynök hibaelhárítása – hálózati követelmények
Az ügynökök tooconnect tooand regisztrálása a Security Center toonetwork erőforrások eléréséhez, beleértve hello portszámok és a tartomány URL-címek kell rendelkezniük.

- Proxykiszolgálók van szüksége, amely megfelelő proxykiszolgáló erőforrások vannak konfigurálva ügynökbeállítások hello tooensure. Olvassa el ebben a cikkben találhat további információt a [hogyan toochange hello proxybeállítások](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).
- A, amelyek korlátozzák a hozzáférést toohello Internet tűzfalak kell tooconfigure a tűzfal toopermit hozzáférés tooOMS. Az ügynök beállításait nem kell módosítania.

a következő táblázat hello látható kommunikációhoz szükséges erőforrásokat.

| Ügynök erőforrása | Portok | HTTPS-ellenőrzés kihagyása |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Igen |
| *.oms.opinsights.azure.com | 443 | Igen |
| *.blob.core.windows.net | 443 | Igen |
| *.azure-automation.net | 443 | Igen |

Ha hibát tapasztal bevezetési hello ügynökkel, győződjön meg arról, hogy tooread hello cikk [hogyan tootroubleshoot Operations Management Suite előkészítési problémák](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Az Endpoint Protection hibaelhárítása nem működik megfelelően

a vendégügynök hello hello szülő folyamat minden, a hello [Microsoft Antimalware](../security/azure-security-antimalware.md) bővítmény does. Hello Vendég ügynök folyamat sikertelen lesz, amikor a futtatását, egyik gyermekfolyamata hello Vendég ügynöke a Microsoft Antimalware hello is sikertelen lehet.  A helyzetekben, például, hogy van ajánlott tooverify hello a következő beállításokat:

- Ha hello cél virtuális gép egy egyéni lemezképet, és hello VM hello létrehozója nem telepítve vendégügynök.
- Ha hello célként megadott helyett egy Windows virtuális Gépet, majd a Windows-verzió hello hello kártevőirtó-bővítmény telepítése Linux virtuális gép Linux virtuális gép sikertelen lesz. Linux-vendégügynök hello virtualizálásra konkrét követelmények vonatkoznak, az operációs rendszer verziója és a szükséges csomagokat, és ezek nem teljesülnek hello Virtuálisgép-ügynök nem fog működni hiba vagy. 
- Ha hello virtuális gép vendégügynökének egy régi verziója lett létrehozva. Ha igen, vegye figyelembe, hogy az egyes régi ügynökök sikerült nem automatikus frissítés maga toohello újabb verzióra, és a toothis probléma vezethet kell lennie. Mindig használja a vendégügynök hello legújabb verzióját, ha a saját lemezképek létrehozásához.
- Egyes külső felügyeleti szoftverek a hello vendégügynök letiltása, vagy hozzáférési toocertain fájlhelyek letiltása. Ha külső a virtuális gép telepítve van, gondoskodjon arról, hogy az hello ügynök hello kizárási listához.
- Bizonyos tűzfal vagy a hálózati biztonsági csoport (NSG) blokkolhatják a hálózati forgalom tooand Vendég ügynöktől.
- Bizonyos hozzáférés-vezérlési listák (ACL) megakadályozhatják a lemezhez való hozzáférést.
- Kevés a szabad lemezterület akkor képes blokkolni a hello vendégügynök helyes működését. 

Alapértelmezett hello Microsoft Antimalware felhasználói felület le van tiltva, az olvasási [engedélyezése a Microsoft Antimalware felhasználói felület Azure Resource Manager virtuális gépek feladás egy vagy több központi telepítési](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) további információt a tooenable, ha van szüksége.

## <a name="troubleshooting-problems-loading-hello-dashboard"></a>Hello irányítópult betöltése hibáinak elhárítása

Ha problémák hello Security Center irányítópultjának betöltése, győződjön meg arról, hogy regisztrálja hello előfizetés tooSecurity Center (azaz hello első felhasználó, aki a Security Center megnyitott hello előfizetés) hello felhasználó és a tooturn szeretné hello felhasználó adatgyűjtés kell *tulajdonos* vagy *közreműködő* hello az előfizetésben. Adott pillanattól is rendelkező felhasználók *olvasó* hello az előfizetés hello-irányítópult és riasztások/ajánlás/házirend látható.

## <a name="contacting-microsoft-support"></a>Kapcsolatfelvétel a Microsoft támogatási szolgálatával
Bizonyos problémák hello útmutatásait, ebben a cikkben használatával azonosíthatók, mások is megkeresheti című cikk dokumentálja hello Security Center nyilvános [fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Ha további hibaelhárításra van szüksége, az alábbi képen látható módon nyithat meg új támogatási kérelmet az **Azure Portalon**: 

![Microsoft támogatási szolgálat](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Lásd még:
Ebben a dokumentumban, megtudta, hogyan tooconfigure biztonsági házirendek az Azure Security Centerben. További információ az Azure Security Center toolearn hello következő lásd:

* [Azure Security Center tervezéséhez és az üzemeltetési útmutatóban](security-center-planning-and-operations-guide.md) – további hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello állapotát az Azure-erőforrások
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztások
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.

