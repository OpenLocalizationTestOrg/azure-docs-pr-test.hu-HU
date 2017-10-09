---
title: "aaaProtect Active Directory és a DNS az Azure Site Recovery |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooimplement egy vész-helyreállítási megoldást az Azure Site Recovery segítségével Active Directory."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 7/20/2017
ms.author: pratshar
ms.openlocfilehash: 49903e54f6d6e1839b0571b7a852c6d7517f0aa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Az Active Directory és az Azure Site Recovery DNS védelme
Vállalati alkalmazások, például a SharePoint, a Dynamics AX és az SAP függ az Active Directory és a DNS-infrastruktúra toofunction megfelelően. Amikor létrehoz egy vészhelyreállítási megoldás az alkalmazások, fontos tooremember tooprotect kell, és az Active Directory és a DNS-helyreállítása előtt hello más alkalmazás-összetevők, tooensure, amely dolgot megfelelően működik, ha katasztrófa történik.

Webhely-helyreállítás egy Azure-szolgáltatás, amely katasztrófa utáni helyreállítás replikáció, feladatátvétel és helyreállítási virtuális gépek megvalósításában. Több tooconsistently védelme a replikáció eseteire, és problémamentesen feladatátvételi virtuális gépek és az alkalmazások tooprivate, nyilvános vagy szolgáltató felhők a Site Recovery támogatja.

A Site Recovery segítségével, az Active Directory egy teljes automatizált vészhelyreállítási tervet hozhat létre. Megszakadása esetén kezdeményezzen feladatátvételt bárhonnan másodpercen belül, és az Active Directory, amelyekből megismerheti néhány perc múlva. Ha telepítése után az Active Directory egyszerre több alkalmazás, például a SharePoint és az SAP az elsődleges helyen, és a webhely teljes hello kívánt toofail, átveheti az Active Directory használatával helyreállítás, és ezután átkapcsolás hello más alkalmazások az alkalmazás-specifikus helyreállítási tervek segítségével.

Ez a cikk azt ismerteti, hogyan toocreate egy vész-helyreállítási megoldást az Active Directory, hogyan tooperform tervezett és nem tervezett és a feladatátvételi tesztet használ egy kattintással helyreállítási tervet, hello támogatott konfigurációk és előfeltételek.  Ismernie kell az Active Directory és az Azure Site Recovery megkezdése előtt.

## <a name="replicating-domain-controller"></a>Tartományvezérlő replikálása

Toosetup kell [Site Recovery replikációs](#enable-protection-using-site-recovery) futtató tartományvezérlő és DNS-legalább egy virtuális gépen. Ha rendelkezik [több tartományvezérlő](#environment-with-multiple-domain-controllers) a környezetben, továbbá tooreplicating tartomány a tartományvezérlő virtuális gép tooset másolatot is lenne, a Site Recovery szolgáltatással hello egy [további tartományvezérlő](#protect-active-directory-with-active-directory-replication) hello cél helyének (Azure vagy egy másodlagos helyszíni adatközpontba). 

### <a name="single-domain-controller-environment"></a>Egyetlen tartományvezérlő környezet
Ha rendelkezik néhány alkalmazást, és csak egyetlen tartományvezérlővel rendelkezik, és kívánt toofail hello teljes webhelyet együtt, akkor azt javasoljuk, a Site Recovery tooreplicate hello tartományvezérlő toohello másodlagos hely (hogy feladat-visszavétele keresztül tooAzure vagy tooa másodlagos helyhez). hello azonos replikált tartomány, tartományvezérlő/DNS-virtuális gép használható [feladatátvételi teszt](#test-failover-considerations) is.

### <a name="environment-with-multiple-domain-controllers"></a>Több tartományvezérlővel rendelkező környezetben
Ha több alkalmazás és hello környezetben csak egy tartományvezérlő van, vagy ha azt tervezi, toofail néhány alkalmazások keresztül egyszerre, azt javasoljuk, hogy ezenkívül tooreplicating hello tartományvezérlő virtuális gép Site Recovery szolgáltatással akkor is Állítson be egy [további tartományvezérlő](#protect-active-directory-with-active-directory-replication) hello cél helyének (Azure vagy egy másodlagos helyszíni adatközpontba). A [feladatátvételi teszt](#test-failover-considerations), replikálni a Site Recovery által és a feladatátvétel, hello további tartományvezérlő hello cél helyének tartományvezérlőt használhat. 


hello alábbi szakaszok azt ismertetik, hogyan tooenable védelmét egy tartományvezérlő, a Site Recovery szolgáltatásban, és hogyan tooset mentése az Azure-ban tartományvezérlő.

## <a name="prerequisites"></a>Előfeltételek
* Egy a helyszíni Active Directory és a DNS-kiszolgáló központi telepítése.
* Egy Azure Site Recovery Services-tároló egy Microsoft Azure-előfizetésben.
* Ha tooAzure, futtassa hello Azure virtuális gép Readiness Assessment eszközt a virtuális gépek tooensure replikál fontosságúak kompatibilis Azure virtuális gépek és az Azure Site Recovery Services.

## <a name="enable-protection-using-site-recovery"></a>A Site Recovery segítségével védelmének engedélyezése
### <a name="protect-hello-virtual-machine"></a>Hello virtuális gép védelme
Engedélyezze hello tartomány a tartományvezérlő/DNS-virtuális gép a Site Recovery általi védelmet. Adja meg a Site Recovery beállításait a hello virtuális gép típusát (Hyper-V vagy VMware) alapján. hello replikálni a Site Recovery segítségével használja a rendszer a [feladatátvételi teszt](#test-failover-considerations). Győződjön meg arról, hogy az megfelel a követelményeknek hello:

1. hello tartományvezérlő egy globáliskatalógus-kiszolgáló
2. hello tartományvezérlőnek kell lennie a hello műveleti Főkiszolgálói szerepkör tulajdonosával egy teszt feladatátvétele során szükséges szerepkörök (más ezeket a szerepköröket kell toobe [lefoglalt](http://aka.ms/ad_seize_fsmo) hello feladatátvételt követően)

### <a name="configure-virtual-machine-network-settings"></a>Virtuális gép hálózati beállításainak konfigurálása
Hello tartomány a tartományvezérlő/DNS-virtuális gép hálózati beállítások konfigurálása a Site Recovery, hogy hello virtuális géphez csatolt toohello megfelelő hálózati feladatátvételt követően. 

![Virtuális gép beállításait](./media/site-recovery-active-directory/DNS-Target-IP.png)

## <a name="protect-active-directory-with-active-directory-replication"></a>Az Active Directory védelmére az Active Directory-replikáció
### <a name="site-to-site-protection"></a>Pont-pont védelme
Hozzon létre egy tartományvezérlő hello másodlagos helyen. Hello server tooa tartományvezérlői szerepkör lépteti elő, amikor meg kell adnia a hello hello nevet ugyanabban a tartományban, amely hello elsődleges helyen használatban van. Használhatja a hello **Active Directory – helyek és szolgáltatások** beépülő modul hello helykapcsolatot tooconfigure beállítások objektum toowhich hello hely. A helykapcsolatot beállítások konfigurálásával szabályozhatja, mikor közötti replikáció két vagy több, és milyen gyakran. További információkért lásd: [ütemezés helyek közötti replikáció](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Hely Azure védelem
Útmutatás alapján hello túl[egy tartományvezérlőt hozzon létre egy Azure virtuális hálózatra](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Amikor előlépteti hello server tooa tartományvezérlői szerepkör, adja meg a hello hello elsődleges helyen használt ugyanazon tartomány nevét.

Majd [hello virtuális hálózat DNS-kiszolgáló hello](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), toouse hello DNS-kiszolgáló az Azure-ban.

![Azure Network-hálózat](./media/site-recovery-active-directory/azure-network.png)

**Az éles Azure DNS**

## <a name="test-failover-considerations"></a>Feladatátvételi szempontokat részletező cikket
A feladatátvételi teszt következik be, hogy nincs hatással a termelési számítási feladatokhoz az éles hálózattól elkülönített hálózatban.

A legtöbb alkalmazás is szükség lehet egy tartományvezérlőt és egy DNS-kiszolgáló toofunction hello jelenlétét. Ezért előtt hello alkalmazás feladatátvétel miatt megváltozott, a tartományvezérlő hello elkülönített hálózati toobe feladatátvételi teszt végrehajtásához használt létrehozott toobe szükséges. Ennek legegyszerűbb módja toodo hello Ez az tooreplicate tartomány tartományvezérlő/DNS-virtuális gép a Site Recovery szolgáltatással. Futtassa a hello tartomány a tartományvezérlő virtuális gép feladatátvételi tesztje hello alkalmazás hello helyreállítási terv feladatátvételi teszt futtatása előtt. Itt látható, hogyan teheti, hogy:

1. [Replikáció](site-recovery-replicate-vmware-to-azure.md) hello tartomány a tartományvezérlő/DNS-virtuális gép a Site Recovery használatával.
1. Hozzon létre egy elkülönített hálózatot. Alapértelmezés szerint az Azure-ban létrehozott virtuális hálózatnak el különítve a többi hálózaton. Azt javasoljuk, hogy hello IP-címtartomány ehhez a hálózathoz legyen, mint az az éles hálózattól. Hely-hely kapcsolatot, a hálózat nem engedélyezi.
1. Adjon meg egy DNS-IP-címet, hello hálózat hozott létre, hogy hello DNS virtuális gép tooget várt hello IP-címként. TooAzure replikál, majd adja meg hello IP-címet a virtuális gép feladatátvétele használt hello **cél IP-címet** beállítása az **számítás és hálózat** beállításait. 

    ![Cél IP](./media/site-recovery-active-directory/DNS-Target-IP.png) **céloz IP**

    ![Azure tesztet hálózathoz](./media/site-recovery-active-directory/azure-test-network.png)

    **DNS-ét Azure tesztet hálózathoz**

> [!TIP]
> A Site Recovery kísérletet tett toocreate teszt virtuális gépek azonos nevű alhálózat, és azonos IP mint, amelyek a használatával hello **számítás és hálózat** hello virtuális gép beállításai között. Ha azonos nevű alhálózat nem érhető el a hello feladatátvételi teszthez megadott Azure virtuális hálózat teszt virtuális gép létrehozása hello első alhálózat ábécésorrendben vannak, majd. Ha hello cél IP-címet a kiválasztott alhálózat hello része, a Site Recovery megpróbálja toocreate hello teszt feladatátvételi virtuális gép hello cél IP-címet használ. Ha nem választott alhálózati hello része hello cél IP-címet, majd teszt feladatátvételi virtuális gép jön létre bármilyen elérhető IP-alhálózatot a kiválasztott hello használata. 
>
>


1. Ha a DHCP használata tooanother helyszíni helyre replikál, utasítások hello túl[DNS és DHCP-feladatátvételi teszt beállítása](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
1. Végezzen hello tartomány a tartományvezérlő virtuális gép futtatása hello elkülönített hálózatban feladatátvételi tesztet. Használja legújabb elérhető **konzisztens alkalmazás** helyreállítási pont hello tartomány a tartományvezérlő virtuális gép toodo hello a feladatátvételi tesztet. 
1. Virtuális gépek hello alkalmazás tartalmazó hello helyreállítási terv feladatátvételi teszt futtatása. 
1. A tesztelés után, **karbantartása a feladatátvételi teszt** hello tartomány a tartományvezérlő virtuális gépen. Ez a lépés törli a teszt feladatátvételhez létrehozott hello tartományvezérlő.


### <a name="removing-reference-tooother-domain-controllers"></a>Hivatkozás tooother tartományvezérlők eltávolítása
Feladatátvételi teszt során, a hello tesztet hálózathoz nem kapcsolja a hello minden tartományvezérlő. hello hivatkozását tooremove más olyan tartományvezérlőn, amely a termelési környezetben található, akkor módosítania kell túl[FSMO Active Directory-szerepkörök zárolása](http://aka.ms/ad_seize_fsmo) és [metaadatait](https://technet.microsoft.com/library/cc816907.aspx) hiányzó tartomány tartományvezérlők. 



> [!IMPORTANT]
> Hello konfigurációk hello a következő szakaszban leírt némelyike nem hello standard/alapértelmezett tartomány a tartományvezérlő-konfigurációk. Ha nem szeretné, hogy e módosítások tooa éles tartományvezérlő, akkor létrehozhat egy tartomány tartományvezérlői dedikált toobe használt toomake a Site Recovery feladatátvételi teszt, és ellenőrizze a módosítások toothat.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>A virtualizációvédelmi funkciók miatt problémák 

Windows Server 2012 rendszertől kezdődően [további védelmet készített az Active Directory tartományi szolgáltatásaiba](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Ezek a funkciók védelme a virtualizált tartományvezérlők USN visszagörgetése ellen, amíg az alapul szolgáló platform hello a hipervizor támogatja a virtuális gép Generációazonosítóját. Azure virtuális gép Generációazonosítóját, ami azt jelenti, hogy a Windows Server 2012 vagy újabb Azure virtuális gépek futtató tartományvezérlők hello további védelmet támogatja. 


Amikor hello virtuális gép Generációazonosítóját, hello invocationID hello AD DS-adatbázis is alaphelyzetbe áll, hello RID-verem a program elveti és SYSVOL nem mérvadó van megjelölve. További információkért lásd: [bemutatása tooActive Directory tartományi szolgáltatások virtualizálása](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) és [DFSR biztonságos virtualizálása](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

TooAzure feladatátvételét okozhat a rendszer visszaállítja a virtuális gép Generációazonosítóját, és, hogy lép működésbe, hello további védelmet hello tartomány a tartományvezérlő virtuális gép indulásakor az Azure-ban. Emiatt előfordulhat, hogy egy **jelentős késés** a felhasználó képes toologin toohello tartomány a tartományvezérlő virtuális gép alatt. Mivel a tartományvezérlő csak a feladatátvételi teszt használni, virtualizációvédelmi funkciók nem szükségesek. az tooensure, amely a virtuális gép Generációazonosítóját hello tartomány a tartományvezérlő virtuális gép nem változik, akkor hello a helyi tartományvezérlő a következő DWORD too4 hello értéke megváltoztatható.

        
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
> Hello konfigurációk hello a következő szakaszban leírt némelyike nem hello standard/alapértelmezett tartomány a tartományvezérlő-konfigurációk. Ha nem szeretné, hogy e módosítások tooa éles tartományvezérlő, akkor létrehozhat egy tartomány tartományvezérlői dedikált toobe használt toomake a Site Recovery feladatátvételi teszt, és ellenőrizze a módosítások toothat.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Tartomány a tartományvezérlő problémamegoldást feladatátvételi tesztje közben


A egy parancssori ablakot futtassa a következő parancs toocheck, hogy a SYSVOL-és a NETLOGON megosztott hello:

    NET SHARE

Hello parancssorból futtassa a következő parancsot, hogy a tartományvezérlő hello tooensure megfelelően működik-e hello.

    dcdiag /v > dcdiag.txt

A hello kimeneti naplót tekintse meg a következő szöveg tooconfirm, hogy a tartományvezérlő hello működik jól. 

* "test átadott kapcsolat"
* "átadott teszt hirdetési"
* "test átadott MachineAccount"

Hello előző feltételek teljesülése esetén valószínű, hogy hello tartományvezérlő is működik. Ha nem, végezze el az alábbi lépéseket.


* Hajtsa végre a hello tartományvezérlő mérvadó visszaállítást.
    * Bár [nem ajánlott a fájlreplikációs szolgáltatás toouse replikációs](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), de ha továbbra is használja, akkor kövesse hello itt ismertetett lépéseket [Itt](https://support.microsoft.com/kb/290762) toodo mérvadó visszaállítást. El tudja olvasni a Burflags bővebben arról volt szó korábbi hivatkozásban hello [Itt](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Ha az elosztott fájlrendszer-replikációs replikációt használ, majd hajtsa végre hello elérhető [Itt](https://support.microsoft.com/kb/2218556) toodo mérvadó visszaállítást. Használhatja a Powershell elérhető funkciók a [hivatkozás](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) erre a célra. 
    
* A kezdeti szinkronizálási követelményről megkerülése úgy, hogy a következő beállításjegyzék-kulcs too0 hello a helyi tartományvezérlőn. Ha a DWORD nem létezik, majd létrehozhat azt csomópont alatt "Parameters". További információk [Itt](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Tiltsa le a hello követelmény, hogy-e a globális katalógus kiszolgálóján elérhető toovalidate felhasználói bejelentkezési úgy, hogy a következő beállításjegyzék-kulcs too1 hello a helyi tartományvezérlőn. Ha a DWORD nem létezik, majd létrehozhat az "Lsa" csomópont alatt. További információk [Itt](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>A különböző gépeken DNS- és tartományvezérlő
Ha a DNS nem a hello ugyanaz a virtuális gép hello tartományvezérlőként kell egy DNS-VM toocreate hello feladatátvételi teszthez. Ha ugyanazon VM hello, kihagyhatja ezt a részt.

Egy új DNS-kiszolgálót használjon, és minden szükséges hello zóna létrehozása. Például ha az Active Directory-tartomány pedig contoso.com, létrehozhat egy DNS-zónát a hello neve contoso.com. hello bejegyzéseket tooActive Directory megfelelő frissíteni kell a DNS-ben, az alábbiak szerint:

1. Győződjön meg arról, ezek a beállítások legyenek érvényben, mielőtt bármely más virtuális gép hello helyreállítási tervben felmerül:
   
   * hello zóna hello erdő gyökér neve után névvel kell ellátni.
   * hello zóna fájl alapú elemnek kell lennie.
   * hello zóna biztonságos, és nem biztonságos frissítések engedélyezni kell.
   * hello feloldó hello tartomány a tartományvezérlő virtuális gép toohello IP-cím hello DNS virtuális gép kell mutatnia.
2. Futtassa a következő parancsot a tartomány a tartományvezérlő virtuális gépen hello:
   
    `nltest /dsregdns`
3. Vegyen fel egy zónát hello DNS-kiszolgálón, nem biztonságos frissítések engedélyezése, és adjon hozzá egy bejegyzést az tooDNS:
   
        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Következő lépések
Olvasási [milyen számítási feladatokat tud védeni?](site-recovery-workload.md) toolearn további információk az Azure Site Recovery vállalati munkaterhelések védelme.

