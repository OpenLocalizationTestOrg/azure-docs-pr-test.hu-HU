---
title: "a Microsoft Azure aaaOracle megoldások |} Microsoft Docs"
description: "További tudnivalók a támogatott konfigurációk és korlátozások vonatkoznak a Microsoft Azure Oracle-megoldások."
services: virtual-machines-linux
documentationcenter: 
manager: timlt
author: rickstercdn
tags: azure-resource-management
ms.assetid: 5d71886b-463a-43ae-b61f-35c6fc9bae25
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/15/2017
ms.author: rclaus
ms.openlocfilehash: 54dc62e76129535da7fb6f131af90c83adfec6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="oracle-solutions-and-their-deployment-on-microsoft-azure"></a>Oracle-megoldások és a központi telepítést, a Microsoft Azure
Ez a cikk ismerteti szükséges toosuccesfully központi telepítése a Microsoft Azure különböző Oracle-megoldások. Ezek a megoldások hello Azure piactér Oracle által közzétett virtuálisgép-rendszerképek alapulnak. Futtassa a következő parancs hello jelenleg elérhető rendszerkép listájának tooget:
```azurecli-interactive
az vm image list --publisher oracle -o table --all
```
Képek 6 – 16-2017 hello listájának frissítésétől hello következők tartoznak:
```bash
Offer                   Publisher    Sku                     Urn                                                          Version
----------------------  -----------  ----------------------  -----------------------------------------------------------  -------------
Oracle-Database-Ee      Oracle       12.1.0.2                Oracle:Oracle-Database-Ee:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Database-Se      Oracle       12.1.0.2                Oracle:Oracle-Database-Se:12.1.0.2:12.1.20170202             12.1.20170202
Oracle-Linux            Oracle       6.4                     Oracle:Oracle-Linux:6.4:6.4.20141206                         6.4.20141206
Oracle-Linux            Oracle       6.7                     Oracle:Oracle-Linux:6.7:6.7.20161007                         6.7.20161007
Oracle-Linux            Oracle       6.8                     Oracle:Oracle-Linux:6.8:6.8.20161020                         6.8.20161020
Oracle-Linux            Oracle       6.9                     Oracle:Oracle-Linux:6.9:6.9.20170406                         6.9.20170406
Oracle-Linux            Oracle       7.0                     Oracle:Oracle-Linux:7.0:7.0.20141217                         7.0.20141217
Oracle-Linux            Oracle       7.2                     Oracle:Oracle-Linux:7.2:7.2.20161020                         7.2.20161020
Oracle-Linux            Oracle       7.3                     Oracle:Oracle-Linux:7.3:7.3.20170320                         7.3.20170320
Oracle-WebLogic-Server  Oracle       Oracle-WebLogic-Server  Oracle:Oracle-WebLogic-Server:Oracle-WebLogic-Server:12.1.2  12.1.2
```

Ezek a lemezképek minősülnek "A saját licenc", és így csak kell fizetnie a számítási, tárolási és hálózati költségeit egy virtuális Gépet futtató.  Feltételezzük, hogy megfelelően licencelt toouse Oracle szoftver és, hogy rendelkezik-e egy jelenlegi támogatási megállapodás helyen Oracle-adatbázissal. Oracle a helyszíni tooAzure licenchordozhatósági garanciát. Tekintse meg a közzétett hello [Oracle és a Microsoft](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html) Megjegyzés licenchordozhatósági leírását. 

Egyéni felhasználók számára is választhatja, toobase azok is, az egyéni lemezképek létrehozása az Azure-ban, vagy egy egyéni képek azok a helyszíni környezetből feltöltése.

## <a name="support-for-jd-edwards"></a>JD Edwards támogatása
TooOracle támogatási megjegyzés szerint [Doc azonosító 2178595.1](https://support.oracle.com/epmos/faces/DocumentDisplay?_afrLoop=573435677515785&id=2178595.1&_afrWindowMode=0&_adf.ctrl-state=o852dw7d_4) , JD Edwards EnterpriseOne verions 9.2 és újabb verziók támogatottak **bármely nyilvánosfelhő-ajánlat** , amely megfelel saját konkrét `Minimum Technical Requirements` (MTR).  Szüksége lesz, amelyek megfelelnek az operációs rendszer és a szoftver alkalmazás kompatibilitási MTR specifikációk toocreate egyéni lemezképek. 

## <a name="oracle-database-virtual-machine-images"></a>Oracle Database virtuálisgép-rendszerképek
Oracle az Azure-ban futó Oracle DB 12.1 Standard és Enterprise kiadás támogatja az Oracle Linux-alapú virtuálisgép-lemezképeket.  Hello legjobb teljesítmény érdekében az Azure-on Oracle-adatbázis a termelési számítási feladatokhoz lehet, hogy tooproperly mérete hello Virtuálisgép-lemezkép, és a prémium szintű Storage üzemelnek felügyelt lemezeket használni. Kapcsolatban, hogyan a tooquickly le egy Oracle DB működik, és az Azure-ban hello Oracle közzétett Virtuálisgép-lemezkép [hello Oracle-adatbázis gyors üzembe helyezési forgatókönyv próbálja](oracle-database-quick-create.md).

### <a name="attached-disk-configuration-options"></a>Csatlakoztatott lemez konfigurációs beállítások

Csatlakoztatott lemezek hello Azure Blob storage szolgáltatás támaszkodnak. Minden egyes szabványos lemez képes elméleti legfeljebb körülbelül 500 bemeneti/kimeneti műveletek száma másodpercenként (IOPS). A prémium szintű lemez ajánlat részesíti előnyben a nagy teljesítményű adatbázis számítási feladatait, és be too5000 iops-érték lemezenként érhető el. Használhatja egyetlen lemezre, amely megfelel a teljesítmény Ha kell, mert a javíthatja hello hatékony IOPS teljesítményét, ha több csatlakoztatott lemez használata, adatbázis-adatok elosztva, és Oracle automatikus Storage kezelési (ASM) használja. Lásd: [Oracle automatikus tárolás – áttekintés](http://www.oracle.com/technetwork/database/index-100339.html) Oracle ASM adott olvashat. A példa bemutatja, hogyan tooinstall és Oracle ASM konfigurálása a Linux Azure virtuális gép – hello próbálkozás [telepítése és konfigurálása Oracle automatikus tárhely-kezelés](configure-oracle-asm.md) oktatóanyag.

### <a name="oracle-realtime-application-cluster-rac"></a>Oracle valós idejű alkalmazás fürt (RAC)
Oracle jogosultságifiók-tanúsítványok tervezett toomitigate hello sikertelen volt-e egy helyszíni több csomópontos fürt konfigurációjába egyetlen csomópont.  Két helyszíni technológiákat, amelyek nem natív toohyper méretű nyilvános felhő környezeteiben támaszkodnak: hálózati csoportos küldésre és a megosztott lemez. Nincsenek a vállalatok által létrehozott külső megoldások [FlashGrid például](https://www.flashgrid.io/oracle-rac-in-azure/) , hogy ezek a technológiák emulációjához, ha toodeploy Oracle RAC az Azure-ban van szüksége. 

### <a name="high-availability-and-disaster-recovery-considerations"></a>Magas rendelkezésre állási és vészhelyreállítási kapcsolatos szempontok
Oracle-adatbázis használata az Azure-ban, elkülönítésével egy magas rendelkezésre állás és a vész helyreállítási megoldást tooavoid leállási végrehajtásáért felelős. 

Oracle Database Enterprise Edition (RAC) nélkül Azure magas rendelkezésre állási és vészhelyreállítási helyreállítási használatával megvalósítható [Data Guard, aktív Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html), vagy [Oracle Golden kapu](http://www.oracle.com/technetwork/middleware/goldengate), két adatbázisok két különálló virtuális gép. Mindkét virtuális gép kell lennie a hello azonos [virtuális hálózati](https://azure.microsoft.com/documentation/services/virtual-network/) tooensure egymással hello magánhálózati állandó IP-cím keresztül hozzáférhessenek.  Ajánlott továbbá elhelyezéséhez hello virtuális gépek hello ugyanabban a rendelkezésre állási tooallow Azure tooplace állítson be különálló fault tartományok és frissítési tartományt.  Azt szeretné, hogy toohave-georedundancia - akkor is a két adatbázis két különböző régiók közötti replikáció és a csatlakozás VPN-átjáró hello két példányt.

Oktatóanyag tudunk "[megvalósítása Oracle DataGuard Azure](configure-oracle-dataguard.md)" amely bemutatja, hogyan hello alapbeállítások eljárás tootrial Ez az Azure-on.  

Az Oracle Data Guard, magas rendelkezésre állású érhető el egy virtuális gép, egy másik virtuális gép, egy másodlagos (készenléti) adatbázis elsődleges adatbázis és a közöttük beállítása egyirányú replikációs. hello eredménye hello adatbázis toohello példánya olvasási hozzáférést. Az Oracle GoldenGate beállíthatja a kétirányú replikációs hello két adatbázis között. Hogyan fel ezeket az eszközöket, az adatbázis egy magas rendelkezésre állású megoldás tooset: toolearn [aktív Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) és [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) hello Oracle webhelyen dokumentációját. Ha meg kell olvasási és írási hozzáférés toohello példányát hello adatbázis, [Oracle aktív Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

Oktatóanyag tudunk "[megvalósítása Oracle GoldenGate Azure](configure-oracle-golden-gate.md)" amely bemutatja, hogyan hello alapvető seup eljárás tootrial Ez az Azure-on.

Annak ellenére, hogy egy magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási megoldás, az Azure-ban tervezett, érdemes tooensure hely toorestore az adatbázis rendelkezik egy biztonsági mentési stratégiának.  Oktatóanyag tudunk [biztonsági mentés és helyreállítás, Oracle-adatbázishoz](oracle-backup-recovery.md) amely bemutatja, hogyan hello alapvető eljárás consistant biztonsági másolat létrehozásához.

## <a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server virtuálisgép-rendszerképek
* **Fürtszolgáltatás támogatott Enterprise Edition csak.** Rendelkezik licenccel az WebLogic Server Enterprise kiadása toouse WebLogic fürtözés használata esetén csak hello. Ne használjon WebLogic Server Standard Edition fürtszolgáltatásra.
* **UDP csoportos küldés nem támogatott.** Azure támogatja az UDP történő, de nem csoportos vagy szórásos küldéshez. WebLogic-kiszolgáló nem tud toorely be Azure UDP egyedi küldéses képességeket. A legjobb eredmények támaszkodva az UDP egyedi küldést, azt javasoljuk, hogy a hello WebLogic fürtméret megtartott statikus, vagy hello fürt része, legfeljebb 10 felügyelt kiszolgálókkal kell tartani.
* **WebLogic Server vár nyilvános vagy privát portokkal toobe hello T3 esetén érhető el, (például, amikor a vállalati JavaBeans használatával).** Fontolja meg egy többrétegű forgatókönyv egy szolgáltatási réteg (EJB) alkalmazás futtató két vagy több virtuális gép, egy nevű Vnetet álló WebLogic kiszolgálófürtre **SLWLS**. hello ügyfél réteg van egy másik alhálózat hello az ugyanazon virtuális toocall EJB hello szolgáltatás rétegben próbált egyszerű Java programot futtat. Mivel szükséges tooload egyenleg hello szolgáltatás réteg, egy nyilvános elosztott terhelésű végponthoz toobe hello WebLogic Server-fürt a virtuális gépek hello készült. Hello magánhálózati port, melyet nem hello nyilvános portot (például 7006:7008), hiba például hello következő esetén:

       [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

       Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

   Ennek az az oka az T3 távelérési WebLogic Server vár hello load balancer portot, és hello felügyelt WebLogic server port toobe hello azonos. A fenti eset hello hello ügyfél port (hello load balancer port) 7006 fér hozzá, és hello felügyelt kiszolgáló figyel a következőn 7008 (hello magánhálózati port). Ez a korlátozás esetén csak T3 hozzáférés, HTTP-nem alkalmazható.

   tooavoid ezt a problémát, használja a következő kerülő megoldások hello:

  * Használja hello azonos titkos és nyilvános portszámok terhelést, elosztott terhelésű dedikált végpontok tooT3 hozzáférés.
  * Hello alábbi JVM paraméter WebLogic Server indításakor a következők:

         -Dweblogic.rjvm.enableprotocolswitch=true

Kapcsolódó tudnivalókért lásd a Tudásbázis következő cikkét **860340.1** : <http://support.oracle.com>.

* **Dinamikus fürtszolgáltatás és terheléselosztás korlátozások.** Tegyük fel, hogy szeretné, hogy a dinamikus fürt WebLogic Server toouse, és azt egy egyetlen, nyilvános elosztott terhelésű végpont az Azure-ban elérhetővé. Ehhez mindaddig, amíg az egyes hello kezelt kiszolgálók (egy tartomány nem dinamikusan hozzárendelt), és nem indulnak el, mint a gép hello rendszergazda nyomon követéséhez több kezelt kiszolgálók rögzített portszámot használja (Ez azt jelenti, hogy legfeljebb egy felügyelt adatb kiszolgálónként az ual gép). Ha a konfiguráció további WebLogic kiszolgálók indítását, mint a virtuális gép (Ez azt jelenti, amelyben több WebLogic kiszolgálómegosztás példányok hello ugyanaz a virtuális gép), akkor nincs lehetőség több WebLogic Server logikailemez kiszolgálók toobind tooa megadott portszám – hello mások a virtuális gépen sikertelen lesz.

   A hello ugyanakkor, ha konfigurál hello felügyeleti kiszolgáló tooautomatically rendelje hozzá a tooits kezelt kiszolgálók, egyedi portszámot, majd az Azure nem támogatja a leképezés egy nyilvános port toomultiple magánhálózati portot, lenne, terheléselosztási nincs lehetőség Ez a konfiguráció szükséges.
* **A virtuális gépen Weblogic Server több példánya.** Attól függően, hogy a telepítési követelményeket, amelyeket érdemes figyelembe venni hello lehetőségét WebLogic Server több példánya futó hello ugyanahhoz a virtuális géphez, ha hello virtuális gép elég nagy. Például egy közepes méretű virtuális gépen, két magok tartalmazó jelenítette meg toorun WebLogic Server két példánya. Ne feledje azonban, hogy mégis azt javasoljuk, hogy hogy kerülje a hibaérzékeny pontokat kíván bevezetni a architektúra, amely hello eset lenne, ha csak egy virtuális gép, amelyen WebLogic Server több példánya fut.. Legalább két virtuális gépeknek jobb megközelítés lehet, és azon virtuális gépek mindegyikének futtatására WebLogic Server több példánya. Minden esetben WebLogic kiszolgáló továbbra is lehet hello részét ugyanabban a fürtben. Vegye figyelembe, azonban jelenleg nem lehetséges toouse Azure tooload-egyenleg végpontok által ilyen WebLogic Server környezetekben belül hello ugyanahhoz a virtuális géphez, mert az Azure terheléselosztó hello elosztott terhelésű kiszolgálók toobe csoportosulnak szükséges egyedi virtuális gépeket.

## <a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK virtuálisgép-rendszerképek
* **JDK 6 és 7 legújabb frissítéseit.** Azt javasoljuk, hello legújabb nyilvános, támogatott Java verzióját (jelenleg Java 8), amíg Azure is elérhetővé teszi JDK 6 és 7 képek. Ennek célja, amelyek még nem kész toobe frissítése tooJDK 8 örökölt alkalmazások számára. Bár a frissítések tooprevious JDK képek előfordulhat, hogy már nem elérhető toohello nyilvános, megadott hello partneri kapcsolat áll fenn, Oracle, hello JDK 6 és 7 képek Azure által biztosított vannak Microsoft szánt toocontain egy újabb nem nyilvános frissítés általában címtára által nyújtott Oracle tooonly egy csoportjának Oracle által támogatott ügyfelek. A hello JDK lemezképek új verzióit lesz elérhető az JDK 6 és 7 frissített kiadásainak idővel.

   hello JDK érhető el a JDK 6 és 7 képek és hello virtuális gépek és lemezképek származik, csak használható Azure-ban.
* **64 bites JDK.** hello Oracle WebLogic Server virtuálisgép-lemezképeket és hello virtuálisgép-rendszerképek Oracle JDK Azure által biztosított tartalmazhat hello 64 bites Windows Server és a hello JDK.

## <a name="next-steps"></a>Következő lépések
Most már rendelkezik aktuális Oracle megoldások áttekintése a Microsoft Azure-on. A következő lépés toogo, telepítse a Azure első Oracle-adatbázishoz.
- Próbálja meg hello [az Oracle-adatbázis létrehozása az Azure](oracle-database-quick-create.md) oktatóanyag tooget elindult.
