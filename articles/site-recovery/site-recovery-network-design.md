---
title: "aaaDesigning a hálózati infrastruktúra vész-helyreállítási |} Microsoft Docs"
description: "Ez a cikk ismerteti az Azure Site Recovery hálózati kialakítási szempontjai"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pratshar
ms.openlocfilehash: 18bafa1b8b334192bfaf2f4aeba9f090011041ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>A vész-helyreállítási hálózat tervezése

Ez a cikk az irányított tooIT szakemberek számára, akik újratervezni, végrehajtási és támogatási üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási infrastruktúra felelős, és ki szeretné, hogy a Microsoft Azure Site helyreállítás (ASR) toosupport tooleverage és a BCDR-szolgáltatások javítása érdekében. Ez a cikk ismerteti, amelyek gyakorlati szempontok vész-helyreállítási helyen hálózati szerkezetének kialakítása, az Azure vagy más helyszíni hely. 

## <a name="overview"></a>Áttekintés
[Az Azure webhely-helyreállítás (ASR)](https://azure.microsoft.com/services/site-recovery/) koordinálja a hello védelmi és helyreállítási üzleti folytonossági (BCDR) vészhelyreállításra a virtualizált alkalmazások a Microsoft Azure-szolgáltatás. Ez a dokumentum tervezett tooguide hello olvasó hello folyamat tervezése során a hello hálózatok összpontosító újratervezni IP-címtartományok és -alhálózatok hello vész-helyreállítási helyen, a Site Recovery használata virtuális gépek (VM) replikálása esetén.

Ezenkívül ez a cikk bemutatja, hogyan lehetővé teszi a Site Recovery a újratervezni, és egy többhelyes virtuális datacenter toosupport BCDR szolgáltatások megvalósítása a teszt- vagy katasztrófa időpontjában.

Ha mindenki vár 24/7 kapcsolat világában, fontosabb, mint minden eddiginél tookeep az infrastruktúra és az alkalmazások lépéseivel. hello üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási feladata sikertelen toorestore összetevők, hello szervezete gyorsan folytathatja a normál működést. Fejlesztését vész helyreállítási stratégiák toodeal nem valószínű, pusztító eseményekkel nagy kihívást. Ez toohello rejlő nehézsége hello jövőbeli előrejelzésére miatt, különösen személyekbe tooimprobable eseményeket, és magas hello költség tooprovide kiterjedt katasztrófa elleni védelem megfelelő intézkedéseket.

Kritikus fontosságú a BCDR tervezési, helyreállítási idő célkitűzés (RTO) és a helyreállítási pont célkitűzés (RPO) definiálni kell egy vész-helyreállítási terv részeként. Ha egy olyan vészhelyzet esetén éri hello ügyfél adatközpont, Azure Site Recovery, az ügyfelek is gyorsan segítségével (alacsony RTO) kapcsolása a replikált virtuális gép vagy a másodlagos adatközpont hello, vagy a Microsoft Azure minimális adatvesztéssel (kis RPO) található.

Feladatátvételi ASR, amely kezdetben másolja a kijelölt virtuális gépek hello elsődleges data center toohello másodlagos adatközpont vagy tooAzure (attól függően, hogy hello esetén), és rendszeresen frissülnek hello replikák tette lehetővé. Infrastruktúrák tervezése, során hálózati terv, amely megakadályozhatja, hogy Ön értekezlet vállalat RTO és Helyreállításipont-célkitűzések lehetséges szűk kell tekinteni.  

Rendszergazdák tervez toodeploy vész-helyreállítási megoldást, ha saját készítésű hello hasonló fontos kérdések egyike hogyan hello virtuális gép lenne elérhető hello feladatátvétele után. Automatikus rendszer-Helyreállítás lehetővé teszi, hogy hello rendszergazda toochoose hello hálózati toowhich egy virtuális gép csatlakoztatott tooafter feladatátvételi lenne. Ha az elsődleges hely hello Azure, vagy egy helyszíni hely a VMM-kiszolgáló felügyeli, majd a Hálózatleképezés használatával érhető el. További információ [hálózati leképezés az Azure tooAzure vész-Helyreállítási](site-recovery-network-mapping-azure-to-azure.md) és [Hálózatleképezés a VMM-ből](site-recovery-network-mapping.md)


Hello hálózati hello helyreállítási hely tervezésekor hello rendszergazda két lehetősége van:

* Használjon egy másik IP-címtartomány hello hálózat helyreállítási helyen. Ebben a forgatókönyvben hello virtuális gép a feladatátvételt követően kap egy új IP-címet, és hello rendszergazda kellene toodo DNS-frissítés. 
* Használja az ugyanazon IP-címtartomány hello hálózat hello helyreállítási helyen. Bizonyos esetekben rendszergazdák jobban szeretik, tooretain hello IP-címek, amelyek rendelkeznek az elsődleges helyen hello hello feladatátvétel után is. Egy normál esetben a rendszergazda kellene tooupdate hello útvonalak tooindicate hello új helyre hello IP-címek. De hello IP-címek a virtuális gépek hello megőrzése hello forgatókönyv, amelyen üzembe van helyezve a felhőbe archivált alhálózati elsődleges hello és hello helyreállítási helyek között, vonzó lehetőség lesz. Kiterjesztené vele az alhálózat egy a helyszíni hálózati tooan Azure-hálózatot, vagy két Azure-hálózatok között nincs lehetőség.  

Rendszergazdák tervez toodeploy vész-helyreállítási megoldást, ha fontos kérdések hello a szem előtt egyik hogyan hello alkalmazások lesz elérhető hello feladatátvétele után. A modern alkalmazások függenek szinte mindig toosome fok, hálózati, fizikailag hálózati kihívást szolgáltatás áthelyezése egy hely tooanother jelöli. Két módon fő, hogy a probléma a vész-helyreállítási megoldások foglalkozik. hello első módszer statikus IP-címek toomaintain. Annak ellenére, hogy hello szolgáltatások áthelyezése és hello üzemeltető kiszolgálók, különböző fizikai helyen találhatóak alatt alkalmazások tegye meg a hello IP-címének konfigurációja, toohello új helyre. hello második megközelítés hello IP-cím teljesen megváltoztatása közben hello áttűnés hello helyreállított helyre. Mindkét megközelítés tartozik megvalósítási változat alábbi összegzését.

Hello hálózati hello helyreállítási hely tervezésekor hello rendszergazda két lehetősége van:

## <a name="option-1-retain-ip-addresses"></a>1. lehetőség: Tartsa meg az IP-címek
A vész helyreállítási folyamat szempontjából a toobe hello legegyszerűbb módszer tooimplement rögzített IP-címek használatával megjelenik, de lehetséges számos rendelkezik kihívás, amelyek a gyakorlatban teszik hello legalább népszerű megközelítést. Az Azure Site Recovery lehetővé teszi hello tooretain hello IP-címek minden forgatókönyvben. Mielőtt tooretain IP úgy dönt, egy megfelelő gondolat hello feladatátvételi lehetőségeket a kifejezhető toohello megkötések kell megadni. Ossza meg velünk nézze meg, melyek segíthetnek a döntési tooretain IP-címek, vagy nem toomake hello tényezőket. Ez megvalósítható a két módon felhőbe archivált alhálózat vagy a teljes alhálózat feladatátvétel.

### <a name="stretched-subnet"></a>Felhőbe archivált alhálózati
Itt hello alhálózati szeretné elérhetővé tenni egyidejűleg az elsődleges és a vész-Helyreállítási helyeken. Egyszerű Ez azt jelenti, hogy áthelyezheti egy kiszolgálót, és annak IP (3. rétegbeli) konfigurációs toohello második hely és a hello hálózati fogja átirányítani a hello forgalom toohello új helyre automatikusan. Ez a kiszolgáló szempontjából trivial toodeal, de számos kihívást rendelkezik:

* A 2. rétegbeli (adatrétege hivatkozás) szempontjából hálózatkezeléshez szükséges eszközök, melyekkel kezelhető a felhőbe archivált VLAN-OK ehhez, de ez lett kevesebb probléma, mivel most széles körben elérhető. hello második és nehezebb probléma az, hogy hello nyújtsák VLAN hello lehetséges tartalék tartomány ki van bővítve tooboth helyek lényegében váljon a hibaérzékeny pontok kialakulását. Ez nem egy valószínűtlen esetnek a bekövetkezése, azt, hogy egy szórási storm elindult, de nem sikerült elkülönített került sor. Azt utolsó problémáról vegyes vélemények talált, és amint láthatta számos sikeres megvalósítását, valamint "azt fogja soha nem a technológia itt".
* Felhőbe archivált alhálózati nincs lehetőség, ha használja a Microsoft Azure, a vész-Helyreállítási hely hello.

### <a name="subnet-failover"></a>Alhálózati feladatátvétel
Lehetséges tooimplement alhálózati feladatátvételi tooobtain hello előnyei felhőbe hello alhálózati megoldás a fent leírt nélkül hello alhálózati Nyújtás több helyre is. Itt bármely adott alhálózaton lenne a jelen webhely 1 vagy 2. hely, de soha nem mindkét helyen egyszerre. Rendelés toomaintain hello IP-címterének hello esemény egy feladatátvételi, hogy a rendszer lehetséges tooprogrammatically hello útválasztó infrastruktúra toomove hello alhálózatok az egyik hely tooanother elrendezése. A feladatátvételi forgatókönyv hello alhálózatok volna a hello együtt mozognak társított védett virtuális gépek. hello fő visszatérítési toothis megközelítés hello esetre, ha nem szerepel a toomove hello teljes alhálózat, amely lehet OK rendelkezik, de ez hatással lehet a hello feladatátvételi granularitási szempontok.

Vizsgáljuk meg tudja tooreplicate egy kitalált, Contoso nevű vállalati Mitől a virtuális gépek tooa helyreállítási hely hello teljes alhálózattal megfelelő működése közben. Követően először áttekintjük Contoso Mitől képes toomanage alhálózataikat során két között a virtuális gépek replikálása a helyszíni helyeken, majd ismertetjük, hogyan alhálózati feladatátvételi működik, ha [hello vész-helyreállítási helyen használt Azure](#failover-to-azure).

#### <a name="failover-from-on-premises-tooazure"></a>Feladatátvétel a helyszíni tooAzure az 
Azure webhely-helyreállítás (ASR) lehetővé teszi, hogy a virtuális gépek számára egy vész-helyreállítási helyként használt Azure toobe.  

Vizsgáljuk meg az olyan forgatókönyvekben, ahol a Woodgrove Bank nevű fiktív vállalat rendelkezik-e az üzletági alkalmazásokat futtató helyszíni infrastruktúrát, és azokat az alkalmazásokat az Azure-on üzemeltető. A helyek (közötti S2S) virtuális magánhálózati (VPN) vagy ExpressRoute biztosít a Woodgrove Bank virtuális gépeket az Azure és a helyszíni kiszolgálók közötti kapcsolatot. S2S VPN-je lehetővé teszi a Woodgrove Bank virtuális hálózati, a Woodgrove Bank kiterjesztése a helyszíni hálózat látható Azure toobe. Ez a kommunikáció S2S VPN engedélyezve van a Woodgrove Bank széle és az Azure-beli virtuális hálózat között. Most Woodgrove szeretne toouse ASR tooreplicate az Azure-régió, az Azure-régió tooanother futó számítási feladatok. Ez a beállítás Woodgrove, amely egy gazdaságos vész-Helyreállítási lehetőséget szeretne, és a nyilvános felhő környezeteiben képes toostore adatok hello igényeinek megfelel. Woodgrove az alkalmazások és konfigurációk IP-címek kódolt függő toodeal rendelkezik, ezért rendelkeznek egy követelmény tooretain IP-címet az alkalmazások futtatása sikertelen után keresztül tooanother régió az Azure-ban.

Woodgrove határozott tooassign IP-címek IP-cím (172.16.1.0/24, 172.16.2.0/24) tartomány tooits erőforrások Azure-beli.

A Woodgrove toobe képes tooreplicate hello IP-címek, megtartja a virtuális gépek tooAzure egy Azure virtuális hálózatot kell létrehozott toobe. Hello a helyszíni hálózat kiterjesztése, hogy az alkalmazások tudnak hello helyszíni hely tooAzure zökkenőmentesen kell. Azure lehetővé teszi a tooadd pont-pont, valamint a pont-pont VPN kapcsolatot toohello létrehozott virtuális hálózatokat az Azure-ban. A pont-pont kapcsolat beállításához, Azure-hálózat lehetővé teszi tooroute forgalom toohello helyszíni hely (Azure meghívja az helyi hálózati) csak akkor, ha hello IP-címtartomány különbözik hello a helyi IP-címtartomány, mert nem Azure a támogatási kiterjesztené vele az alhálózatokat.  Ez azt jelenti, hogy ha egy alhálózat 192.168.1.0/24 a helyszíni, nem adja hozzá a helyi hálózati 192.168.1.0/24 hello Azure-hálózatot. Ez elvárható, hiszen az Azure nem tudja, hogy nincs aktív virtuális gépek szerepelnek hello alhálózati és hello alhálózaton kerül létrehozásra csak a vész-Helyreállítási célokra. toobe képes toocorrectly hálózati forgalom hello és hello helyi-hálózat az Azure-hálózat hello alhálózatai kívül nem ütközhet.

![Alhálózati feladatátvétel előtt](./media/site-recovery-network-design/network-design7.png)

Feladatátvétel előtt

toohelp Woodgrove az üzleti követelmények teljesítésére, igazolnia kell a következő munkafolyamatok tooimplement hello:

* Hozzon létre egy további hálózati, ossza meg velünk neki helyreállítási hálózati, ahol hello átvevő virtuális gépek jönnek létre.
* tooensure, hogy a virtuális gépek IP-Címek hello őrizzen meg egy feladatátvétel után nyissa meg a virtuális gép tulajdonságok toohello konfigurálása lapon, adja meg a hello azonos IP-cím hello VM rendelkezik a helyszínen, és majd kattintson a Mentés gombra. Hello VM feladatátvételt, ha az Azure Site Recovery rendeli IP toohello virtuális géphez megadott hello.

![Hálózat tulajdonságai](./media/site-recovery-network-design/network-design8.png)

Miután hello feladatátvételi elindul, és hello virtuális gépek jönnek létre a hello helyreállítási hálózati hello kívánt IP-, kapcsolat toothis hálózati hozhatók létre a [Vnet tooVnet kapcsolat](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Ha szükséges, ez a művelet parancsfájlalapú lehet.  Azt című szakaszban leírtaknak megfelelően az előző szakaszban hello alhálózati feladatátvétellel, még akkor is, hello kis-és feladatátvételi tooAzure kapcsolatos útvonalak volna toobe megfelelően módosított tooreflect adott 192.168.1.0/24 most áthelyezte tooAzure.

![Alhálózati feladatátvételt követően](./media/site-recovery-network-design/network-design9.png)

A feladatátvételt követően

Ha egy Azure hálózati nincs a fenti hello képen látható módon. Egy hely toosite VPN-kapcsolat a "Helyreállítási hálózat" között "Elsődleges helyhez" hello feladatátvételt követően is létrehozhat.  


#### <a name="failover-tooa-secondary-on-premises-site"></a>Feladatátvételi tooa másodlagos helyszíni hely
Ossza meg velünk tekintse meg a forgatókönyvhöz ahol szeretnénk hello IP hello virtuális gépek mindegyikének megőrzése, és feladatátvételi hello befejezéséhez alhálózati együtt. hello elsődleges hely rendelkezik alhálózati 192.168.1.0/24 futó alkalmazások. Ha hello feladatátvétel történik, ez az alhálózat részét képező összes hello virtuális gépek toohello helyreállítási hely átvétele fog megtörténni, és tartsa meg az IP-címét. Útvonalak lesz toobe megfelelően módosítani tooreflect hello tényt, hogy toosubnet 192.168.1.0/24 tartozó összes hello virtuális gépek most áthelyezte toohello helyreállítási helyet.

Hello a következő ábra hello továbbítja az elsődleges hely és hely helyreállítási, harmadik és elsődleges hely között, és harmadik és a helyreállítási hely toobe megfelelően módosítani kell.

a következő képek hello hello alhálózatok hello feladatátvétel előtt jeleníti meg. Alhálózati 192.168.0.1/24 aktív az elsődleges hely hello hello feladatátvétel előtt, és hello feladatátvételt követően a helyreállítási hely hello aktiválódásakor

![Feladatátvétel előtt](./media/site-recovery-network-design/network-design2.png)

Feladatátvétel előtt

hello az alábbi képen látható a hálózatok és alhálózatok feladatátvételt követően.

![A feladatátvételt követően](./media/site-recovery-network-design/network-design3.png)

A feladatátvételt követően

A másodlagos hely van a helyszíni és a VMM-kiszolgáló toomanage használ majd engedélyezni a védelmet egy adott virtuális gépén, az automatikus rendszer-Helyreállítás amikor foglal le a következő munkafolyamat toohello szerint hálózati erőforrások:

* Az ASR foglal le, hello statikus IP-címkészletből hello releváns hálózati minden System Center VMM-példány a megadott IP-cím mindegyik hálózati interfész hello virtuális gépen.
* Ha hello rendszergazda határozza meg az azonos IP-címkészlet hello hálózat hello helyreállítási hely, hogy hello IP-címkészlet a hello hello elsődleges helyen, hello IP cím toohello replika virtuális gép automatikus rendszer-Helyreállítás lefoglalása közben hálózati szeretné kiosztani hello hello azonos IP-cím legyen, mint az elsődleges virtuális gép hello.  hello IP-cím foglalt a VMM-ben, de nincs beállítva az feladatátvételi IP hello Hyper-v gazdagépen. A Hyper-v gazdagép feladatátvételi IP hello feladatátvétel előtt van beállítva.


Hello azonos IP-cím nem érhető el, ha az automatikus rendszer-Helyreállítás néhány elérhető IP-cím volna hello meghatározott IP-címkészletből lefoglalni.

VM engedélyezve van a védelemre hello után is használhatja a következő minta parancsprogram tooverify hello IP-cím van rendelve, toohello virtuális gép. hello azonos IP ehhez állítja a feladatátvételi IP és hozzárendelt toohello VM feladatátvevő hello időpontjában:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> Hello forgatókönyvben, ahol a virtuális gépek DHCP használatára IP-címek kezelését hello teljesen kívül esik az ASR hello irányítását. A rendszergazda rendelkezik, amely hello DHCP kiszolgáló szolgál hello IP-címek hello helyreállítási helyen képes kiszolgálni az hello tooensure hello elsődleges hely, amely ugyanazon tartomány.
>
>



## <a name="option-2-changing-ip-addresses"></a>2. lehetőség: IP-címek módosítása
Ez a megközelítés toobe hello legjellemzőbb mi úgy találtuk, alapján úgy tűnik. Minden virtuális gép, amely részt vesz hello feladatátvételi hello IP-cím megváltoztatása hello formája vesz igénybe. Ez a megközelítés visszatérítési szükséges hello bejövő hálózati too'learn "használó hello alkalmazást, amely korábban IPx jelenleg IPy. Akkor is, ha IPX-et és IPy logikai nevek, DNS-bejegyzések általában rendelkeznek toobe új és módosított kiürített hello hálózaton, és hálózati táblák gyorsítótárazott tételeit rendelkezik toobe frissítése vagy a kiürített, ezért a leállás volt látható a attól függően, hogyan hello DNS-infrastruktúra rendelkezik a telepítő lett. Ezek a problémák enyhíthetők hello esetében az intranetes alkalmazások alacsony TTL értékek használata és használata [Azure Traffic Manager az automatikus rendszer-Helyreállítás](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) az internet-alapú alkalmazások

### <a name="changing-hello-ip-addresses---illustration"></a>Hello IP-címek - illusztráció módosítása
Ossza meg velünk meg hello forgatókönyv Ha azt tervezi, toouse különböző IP-címek elsődleges hello és hello helyreállítási helyen. Az alábbi példa hello is tudunk egy harmadik webhely, ahol elsődleges vagy tartalék hely üzemeltetett hello alkalmazások érhető el.

![Különböző IP - feladatátvétel előtt](./media/site-recovery-network-design/network-design10.png)


A fenti hello képen néhány alhálózati 192.168.1.0/24 alhálózati hello elsődleges helyen üzemeltetett alkalmazások, és voltak konfigurált toocome fel az alhálózati 172.16.1.0/24 hello helyreállítási helyen egy feladatátvétel után. VPN-kapcsolatok és a hálózati útvonalak konfigurálva megfelelően, hogy az összes három helyen képesek elérni egymást.

Az alábbi, egy vagy több alkalmazást, miután a hello képként azok visszaállnak hello helyreállítási alhálózaton. Ebben az esetben felhőmodellből nem korlátozott toofailover alhálózati hello-teljes hello ugyanannyi időt vesz igénybe. Nincs módosításokra szükség tooreconfigure VPN- vagy hálózati útvonalak. Feladatátvétel és bizonyos DNS-frissítések győződjön meg arról, hogy alkalmazások továbbra is elérhető lesz. Ha hello DNS dinamikus frissítések konfigurált tooallow hello virtuális gépek volna regisztrálják magukat hello új IP használatával, ha elindítják a feladatátvétel után.

![Különböző IP - feladatátvételt követően](./media/site-recovery-network-design/network-design11.png)


Miután sikertelen átvevő hello replika virtuális gép IP-címet, amely nem lehet hello azonos hello elsődleges virtuális gép hello IP-címként. Virtuális gépek frissíteni fogja a hello DNS-kiszolgálót használják indítása után. DNS-bejegyzések általában toobe hello hálózaton kiürített új és módosított, pedig a gyorsítótárazott bejegyzés hálózati táblák toobe frissítése vagy kiürített, így nem szembesül állásidő, amíg ezek kerül sor ritka toobe. A probléma enyhíthetők:

* Intranetes alkalmazások alacsony TTL értéket használja.
* Az automatikus rendszer-Helyreállítás Azure Traffic Manager használ az internet-alapú alkalmazások.
* Hello alábbi parancsfájl belül a helyreállítási terv tooupdate hello DNS-kiszolgáló tooensure egy időben történő frissítést (hello parancsfájl esetén nem szükséges hello dinamikus DNS-regisztráció van beállítva)

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-hello-ip-addresses--dr-tooazure"></a>Változó hello IP-címek – vész-Helyreállítási tooAzure
Hello [hálózati infrastruktúra telepítése a Microsoft Azure vész-helyreállítási helyként](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) blogbejegyzés ismerteti, hogyan toosetup hello szükséges, az Azure hálózati infrastruktúra IP-címek megőrzésekor nem előfeltétele. Hello alkalmazást és hogyan toosetup hálózatkezelés a helyszínen, és keresse meg az Azure és a majd áthaladva addig tart, hogyan leíró kezdődik toodo feladatátvételi tesztet, és egy tervezett feladatátvételt.

## <a name="next-steps"></a>Következő lépések
[Ismerje meg,](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) Site Recovery hogyan képezi le a forrás és cél hálózatok a Ha a VMM-kiszolgálóhoz használt toomanage hello elsődleges hely alatt.
