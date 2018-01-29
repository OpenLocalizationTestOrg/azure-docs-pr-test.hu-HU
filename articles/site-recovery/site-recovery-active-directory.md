---
title: "Active Directory és a DNS az Azure Site Recovery védelmét |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Site Recovery segítségével Active Directory egy vész-helyreállítási megoldás megvalósításához."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: 5db2d424c176428d31fb99fd8288120f18fcac7a
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/15/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Az Active Directory és az Azure Site Recovery DNS védelme
Vállalati alkalmazások, például a SharePoint, a Dynamics AX és az SAP attól függ, az Active Directory és a DNS-infrastruktúra a megfelelő működéshez. Amikor létrehoz egy vészhelyreállítási megoldás az alkalmazások, gyakran szüksége lesz az Active Directory és a DNS-helyreállítása előtt a más alkalmazás-összetevők megfelelő működésének biztosításához.

A Site Recovery segítségével, az Active Directory egy teljes automatizált vészhelyreállítási tervet hozhat létre. Megszakadása esetén kezdeményezzen feladatátvételt bárhonnan másodpercen belül, és az Active Directory, amelyekből megismerheti néhány perc múlva. Ha telepítése után az Active Directory egyszerre több alkalmazás, például a SharePoint és az SAP az elsődleges helyen, és szeretné feladatátvételi a webhely teljes, akkor a feladatátvételi Active Directory először a Site Recovery segítségével, és ezután átveheti a más alkalmazásokhoz alkalmazásspecifikus helyreállítási tervet.

Ez a cikk ismerteti, hogyan hozzon létre egy vész-helyreállítási megoldást az Active Directory, hogyan hajthat végre feladatátvételt egyetlen kattintással helyreállítási tervet, a támogatott konfigurációk és az előfeltételek.  Ismernie kell az Active Directory és az Azure Site Recovery megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek
* A Microsoft Azure-előfizetés a Recovery Services-tároló.
* Ha az Azure-bA replikál [előkészítése](tutorial-prepare-azure.md) Azure-erőforrások, például az Azure-előfizetéssel, Azure virtuális hálózat és a storage-fiók.
* Tekintse át a támogatási lévő valamennyi összetevőnél.

## <a name="replicating-domain-controller"></a>Tartományvezérlő replikálása

A telepítő kell [Site Recovery replikációs](#enable-protection-using-site-recovery) futtató tartományvezérlő és DNS-legalább egy virtuális gépen. Ha rendelkezik [több tartományvezérlő](#environment-with-multiple-domain-controllers) a környezetben, a tartomány tartományvezérlő virtuális gépnek a Site Recovery replikálása mellett is állíthat be egy [további tartományvezérlő](#protect-active-directory-with-active-directory-replication)a célként megadott helyen (Azure vagy egy másodlagos helyszíni adatközpontba).

### <a name="single-domain-controller-environment"></a>Egyetlen tartományvezérlő környezet
Ha néhány alkalmazást, és csak egyetlen tartományvezérlővel rendelkezik, és azt szeretné, hogy áthelyezze a feladatokat a teljes helyre együtt, majd azt javasoljuk, a Site Recovery a tartományvezérlő replikálja az adatokat a célhelyre (Azure vagy egy másodlagos helyszíni adatközpontba). Azonos replikált tartomány, tartományvezérlő/DNS-virtuális gép használható [feladatátvételi teszt](#test-failover-considerations) is.

### <a name="environment-with-multiple-domain-controllers"></a>Több tartományvezérlővel rendelkező környezetben
Ha sok alkalmazásokat, és csak egy tartományvezérlő van a környezetben, vagy ha azt tervezi, hogy áthelyezze a feladatokat néhány alkalmazások egyszerre, azt javasoljuk, hogy a tartomány tartományvezérlő virtuális gépnek a Site Recovery replikálása mellett, akkor is Állítson be egy [további tartományvezérlő](#protect-active-directory-with-active-directory-replication) a célként megadott helyen (Azure vagy egy másodlagos helyszíni adatközpontba). A [feladatátvételi teszt](#test-failover-considerations), használhatja a tartományvezérlő replikálja a Site Recovery által és a feladatátvétel, a további tartományvezérlő a célként megadott helyen.

## <a name="enable-protection-using-site-recovery"></a>A Site Recovery segítségével védelmének engedélyezése
### <a name="protect-the-virtual-machine"></a>A virtuális gép védelme
Engedélyezze a tartomány a tartományvezérlő/DNS-virtuális gép a Site Recovery általi védelmet. A tartományvezérlő replikálja a Site Recovery segítségével szolgál [feladatátvételi teszt](#test-failover-considerations). Győződjön meg arról, hogy megfelel-e az alábbi követelményeknek:

1. A tartományvezérlő globáliskatalógus-kiszolgáló nem
2. A tartományvezérlőnek kell lennie a műveleti Főkiszolgálói szerepkör tulajdonosával egy teszt feladatátvétele során szükséges szerepkörök (más ezeket a szerepköröket kell lennie [lefoglalt](http://aka.ms/ad_seize_fsmo) a feladatátvétel után)

### <a name="configure-virtual-machine-network-settings"></a>Virtuális gép hálózati beállításainak konfigurálása
A tartomány a tartományvezérlő/DNS-virtuális gép esetén konfigurálja a hálózati beállításai a számítási és hálózati beállítások, a replikált virtuális gép a Site Recovery szolgáltatásban. Ez azért szükséges, hogy a virtuális gép csatolva lesz a megfelelő hálózati feladatátvételt követően.

## <a name="protect-active-directory-with-active-directory-replication"></a>Az Active Directory védelmére az Active Directory-replikáció
### <a name="site-to-site-protection"></a>Pont-pont védelme
A tartományvezérlő létrehozása a másodlagos helyen. Amikor előlépteti egy tartomány tartományvezérlői szerepkör a kiszolgálót, adja meg annak a tartománynak az elsődleges helyen használt nevét. Használhatja a **Active Directory – helyek és szolgáltatások** beépülő modul segítségével a hely objektum, amelyhez a helyek ad hozzá a beállításokat. A helykapcsolatot beállítások konfigurálásával szabályozhatja, mikor közötti replikáció két vagy több, és milyen gyakran. További információkért lásd: [ütemezés helyek közötti replikáció](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Hely Azure védelem
Kövesse az utasításokat [egy tartományvezérlőt hozzon létre egy Azure virtuális hálózatra](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Amikor előlépteti egy tartomány tartományvezérlői szerepkör a kiszolgálót, adja meg az elsődleges helyen használt ugyanazon tartomány nevét.

Majd [konfigurálja újra a virtuális hálózat DNS-kiszolgáló](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), a DNS-kiszolgáló használatára az Azure-ban.

![Azure Network-hálózat](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Azure-Azure védelme
Kövesse az utasításokat [egy tartományvezérlőt hozzon létre egy Azure virtuális hálózatra](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Amikor előlépteti egy tartomány tartományvezérlői szerepkör a kiszolgálót, adja meg az elsődleges helyen használt ugyanazon tartomány nevét.

Majd [konfigurálja újra a virtuális hálózat DNS-kiszolgáló](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), a DNS-kiszolgáló használatára az Azure-ban.

## <a name="test-failover-considerations"></a>Feladatátvételi szempontokat részletező cikket
Feladatátvételi teszt következik be, hogy el van különítve az éles hálózati környezetben termelési számítási feladatokhoz gyakorolt hatás elkerülése érdekében a hálózaton.

A legtöbb alkalmazás is szükség lehet egy tartományvezérlő és DNS-kiszolgáló működésének jelenlétét. Ezért át nem adja az alkalmazást, mielőtt egy tartományvezérlő kell létrehozni a feladatátvételi teszt végrehajtásához használja a elkülönített hálózatot. Ennek legegyszerűbb módja replikálásához a Site Recovery tartomány a tartományvezérlő/DNS-virtuális gép. Az alkalmazás a helyreállítási terv feladatátvételi teszt futtatása előtt futtatja, a tartomány a tartományvezérlő virtuális gép feladatátvételi tesztet. Itt látható, hogyan teheti, hogy:

1. [Replikáció](site-recovery-replicate-vmware-to-azure.md) a tartomány a tartományvezérlő/DNS-virtuális gép Site Recovery segítségével.
2. Hozzon létre egy elkülönített hálózatot. Alapértelmezés szerint az Azure-ban létrehozott virtuális hálózatnak el különítve a többi hálózaton. Azt javasoljuk, hogy az IP-címtartományt ezen a hálózaton azonos legyen, mint az éles hálózattól. Hely-hely kapcsolatot, a hálózat nem engedélyezi.
3. Adjon meg egy DNS-IP-címet, a hálózat létrehozása az IP-címet a DNS-virtuális gép beolvasandó várt. Ha az Azure-bA replikál, majd adja meg az IP-cím a virtuális gép feladatátvétele használt a **cél IP-címet** beállítása az **számítás és hálózat** a replikált virtuális gép beállításai között.

    ![Azure tesztet hálózathoz](./media/site-recovery-active-directory/azure-test-network.png)

> [!TIP]
> A Site Recovery megpróbálja létrehozni a teszt virtuális gépek az azonos név és az azonos IP-cím használatával, mint a megadott alhálózat **számítás és hálózat** a virtuális gép beállításai között. Ha azonos nevű alhálózat az Azure virtuális hálózatban, a feladatátvételi teszthez megadott nem érhető el, majd a teszt virtuális gép jön létre az első alhálózat ábécésorrendben vannak. Ha a cél IP-címet a kiválasztott alhálózat egy része, majd a Site Recovery megkísérli létrehozni a teszt feladatátvételi virtuális gépet a cél IP-címet használ. Ha a figyelt IP cím nem része a kiválasztott alhálózatnak, majd teszt feladatátvételi virtuális gép jön létre a következő rendelkezésre álló IP használata a kiválasztott alhálózaton.
>
>


1. Ha a DHCP használata egy másik helyszíni helyre replikál, kövesse az utasításokat [DNS és DHCP-feladatátvételi teszt beállítása](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
2. Végezzen a tartomány a tartományvezérlő virtuális gép elkülönített hálózatban futtassa feladatátvételi tesztet. Használja legújabb elérhető **konzisztens alkalmazás** helyreállítási pont a tartomány a tartományvezérlő virtuális gép feladatátvételi teszt végrehajtásához.
3. A helyreállítási terv, amely tartalmazza az alkalmazás virtuális gépek feladatátvételi teszt futtatása.
4. A tesztelés után, **karbantartása a feladatátvételi teszt** a tartomány a tartományvezérlő virtuális gépen. Ezzel a lépéssel törli a teszt feladatátvételhez létrehozott tartományvezérlő.


### <a name="removing-reference-to-other-domain-controllers"></a>Más tartományvezérlők mutató hivatkozás eltávolítását
Feladatátvételi teszt során, akkor a tesztelési célú hálózat nem kapcsolja a minden tartományvezérlő. Az éles környezetben, más tartományvezérlők létező hivatkozásának eltávolítására, szükség lehet [FSMO Active Directory-szerepkörök zárolása](http://aka.ms/ad_seize_fsmo) és [metaadatait](https://technet.microsoft.com/library/cc816907.aspx) tartományvezérlők hiányzik.



> [!IMPORTANT]
> A konfigurációk a következő szakaszban leírt némelyike nem a normál/alapértelmezett tartományvezérlő-konfigurációkban. Ha nem kívánja ilyen jellegű változtatásokat éles-tartományvezérlővé, majd létrehozhat egy tartományvezérlő a Site Recovery a feladatátvételi teszthez használni, és a módosításokat, amelyek dedikált.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>A virtualizációvédelmi funkciók miatt problémák

Windows Server 2012 rendszertől kezdődően [további védelmet készített az Active Directory tartományi szolgáltatásaiba](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Ezek a funkciók védelme a virtualizált tartományvezérlők USN visszagörgetése ellen, amíg az alapul szolgáló hipervizor platform támogatja a virtuális gép Generációazonosítóját. Azure virtuális gép Generációazonosítóját, ami azt jelenti, hogy a Windows Server 2012 vagy újabb Azure virtuális gépek futtató tartományvezérlők kell-e a további funkciók támogatja.


Amikor a virtuális gép Generációazonosítóját, az invocationid-t az AD DS-adatbázis is alaphelyzetbe áll, a program elveti a RID-vermet, és SYSVOL nem mérvadó van megjelölve. További információkért lásd: [Bevezetés az Active Directory tartományi szolgáltatások virtualizálása](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) és [DFSR biztonságos virtualizálása](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

Az Azure-bA feladatátvételét okozhatnak, alaphelyzetbe állítása a virtuális gép Generációazonosítóját, és, amely lép működésbe, a kiegészítő védelmet az Azure-ban a tartomány a tartományvezérlő virtuális gép indításakor. Emiatt előfordulhat, hogy egy **jelentős késés** tudnak majd bejelentkezni a tartomány a tartományvezérlő virtuális gépet a felhasználó. Mivel a tartományvezérlő csak a feladatátvételi teszt használni, virtualizációvédelmi funkciók nem szükségesek. Annak érdekében, hogy nem módosítja a virtuális gép Generációazonosítóját, a tartomány a tartományvezérlő virtuális gépnek, majd a helyi tartományvezérlő 4 módosíthatja a következő DWORD értékét.


        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start


#### <a name="symptoms-of-virtualization-safeguards"></a>A virtualizációvédelmi funkciók tünetei

A virtualizációvédelmi funkciók rendelkezik kezdődött feladatátvételi teszt után, jelenhet meg alábbi jelenségek közül:  

Létrehozás ID módosítása

![Létrehozás ID módosítása](./media/site-recovery-active-directory/Event2170.png)

Meghívási azonosító módosításának

![Meghívási azonosító módosításának](./media/site-recovery-active-directory/Event1109.png)

Netlogon és a SYSVOL megosztás nem érhetők el.

![SYSVOL-megosztás](./media/site-recovery-active-directory/sysvolshare.png)

![NTFRS Sysvol](./media/site-recovery-active-directory/Event13565.png)

Bármely adatbázisainak törlődnek.

![Elosztott fájlrendszer-replikációs DB törlése](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> A konfigurációk a következő szakaszban leírt némelyike nem a normál/alapértelmezett tartományvezérlő-konfigurációkban. Ha nem kívánja ilyen jellegű változtatásokat éles-tartományvezérlővé, majd létrehozhat egy tartományvezérlő a Site Recovery a feladatátvételi teszthez használni, és a módosításokat, amelyek dedikált.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Tartomány a tartományvezérlő problémamegoldást feladatátvételi tesztje közben


Egy parancssorból futtassa a következő parancs futtatásával ellenőrizze, hogy SYSVOL-és a NETLOGON megosztott:

    NET SHARE

A parancssorban futtassa a következő parancs futtatásával győződjön meg arról, hogy a tartományvezérlő megfelelően működik-e.

    dcdiag /v > dcdiag.txt

A kimeneti naplót, a keresés következő annak ellenőrzéséhez, hogy a tartományvezérlő működik jól.

* "test átadott kapcsolat"
* "átadott teszt hirdetési"
* "test átadott MachineAccount"

Ha az előző feltételek teljesülnek, valószínű, hogy a tartományvezérlő működik jól. Ha nem, végezze el az alábbi lépéseket.


* Hajtsa végre a tartományvezérlő mérvadó visszaállítást.
    * Bár ez [fájlreplikációs használata nem ajánlott](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), de ha továbbra is használja, akkor kövesse az itt ismertetett lépéseket [Itt](https://support.microsoft.com/kb/290762) egy mérvadó helyreállításával. El tudja olvasni a Burflags bővebben arról volt szó korábbi hivatkozás [Itt](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Ha az elosztott fájlrendszer-replikációs replikációt használ, majd kövesse elérhető [Itt](https://support.microsoft.com/kb/2218556) egy mérvadó helyreállításával. Használhatja a Powershell elérhető funkciók a [hivatkozás](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) erre a célra.

* A kezdeti szinkronizálási követelményről kihagyása a következő beállításkulcsot a helyi tartományvezérlő 0 értékre állításával. Ha a DWORD nem létezik, majd létrehozhat azt csomópont alatt "Parameters". További információk [Itt](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Tiltsa le a követelmény, hogy felhasználói bejelentkezés érvényesítése úgy, hogy a helyi tartományvezérlő 1 rendszerleíró kulcsot következő rendelkezésére áll-e globáliskatalógus-kiszolgáló. Ha a DWORD nem létezik, majd létrehozhat az "Lsa" csomópont alatt. További információk [Itt](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>A különböző gépeken DNS- és tartományvezérlő
Ha a DNS nem szerepel az azonos virtuális gépen, ahol a tartományvezérlő, a teszt feladatátvételhez DNS virtuális gép létrehozása szeretné. Ha az azonos virtuális gépen, ez a szakasz kihagyhatja.

Egy új DNS-kiszolgálót használjon, és hozzon létre a szükséges zónákat. Például ha az Active Directory-tartomány pedig contoso.com, létrehozhat egy DNS-zónát a contoso.com tartománynevet a. Az Active Directory megfelelő bejegyzések meg kell adni a DNS-ben, az alábbiak szerint:

1. Győződjön meg arról, ezek a beállítások legyenek érvényben, mielőtt másik virtuális géphez a helyreállítási tervben felmerül:

   * A zóna nevet kell kapniuk az erdő gyökér neve után.
   * A zóna fájl alapú elemnek kell lennie.
   * Biztonságos, és nem biztonságos frissítések engedélyezni kell a zónát.
   * A tartomány a tartományvezérlő virtuális gép a feloldó a DNS-virtuális gép IP-címére kell mutatnia.
2. A következő parancsot a tartomány a tartományvezérlő virtuális gépen:

    `nltest /dsregdns`
3. Vegyen fel egy zónát a DNS-kiszolgálón, nem biztonságos frissítések engedélyezése, és adjon hozzá egy bejegyzést ki azt a DNS-ben:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Következő lépések
[További](site-recovery-workload.md) az Azure Site Recovery vállalati munkaterhelések védelme.
