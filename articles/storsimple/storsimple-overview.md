---
title: "aaaStorSimple 8000 sorozat megoldás áttekintése |} Microsoft Docs"
description: "Ismerteti a StorSimple rétegezéséhez, hello eszköz, virtuális eszköz, szolgáltatások és tárolók kezelése, és bemutatja a legfontosabb kifejezések a StorSimple."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: v-sharos@microsoft.com
ms.openlocfilehash: 0891841186dcd4c46f48d13ed829b98fab7bdf67
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>A StorSimple 8000 series: hibrid felhő tárolási megoldás
## <a name="overview"></a>Áttekintés
Üdvözli az Azure StorSimple, egy integrált tárolási megoldás, amely kezeli a helyszíni eszközök és a Microsoft Azure felhőalapú tárolást között a tárolási feladatok elvégzéséről tooMicrosoft. StorSimple egy hatékony, költséghatékony és könnyen kezelhető tárolási terület tárolóhálózat (SAN) megoldás, amely eltávolít számos olyan hello problémák és a vállalati tárolás és az adatvédelem társított is. Azt hello jogvédett StorSimple 8000 series eszközt használ, integrálható a felhőszolgáltatásokkal, és felügyeleti eszközöket biztosít az összes vállalati tároló, beleértve a felhőalapú tárolást zökkenőmentes nézetét. (hello hello Microsoft Azure webhelyen közzétett StorSimple üzembehelyezési információk csak érvényes tooStorSimple 8000 sorozat eszközeire érvényesek. Ha egy StorSimple 5000/7000-es adatsorozat eszközt használ, lépjen az túl[StorSimple súgó](http://onlinehelp.storsimple.com/).)

StorSimple használ [tárolórétegzés](#automatic-storage-tiering) toomanage tárolt adatok között különböző tárolási adathordozókon. hello aktuális munkakészlete a helyszínen tárolt (SSD) a ritkábban használt adatok tárolása merevlemezes (HDD) meghajtók és archiválási adatok fejlesztőre toohello felhő. Ezenkívül StorSimple deduplikációs használ, és tömörítés tooreduce hello összege hello adatokat tároló fel. További információ: túl[a Deduplikáció és a tömörítést](#deduplication-and-compression). Más kulcs használt kifejezések és fogalmak hello StorSimple 8000 series dokumentációjának definícióját, nyissa meg túl[StorSimple terminológia](#storsimple-terminology) hello Ez a cikk végén.

Ezenkívül toostorage felügyeleti StorSimple adatbiztonsági funkciók lehetővé teszi a akkor toocreate igény szerinti és ütemezett biztonsági mentések, majd azokat helyben vagy hello felhőbeli tároló. Biztonsági másolatok formájában hello növekményes pillanatképet, ami azt jelenti, hogy azok hozható létre és gyorsan vissza kell venni. Felhőalapú pillanatfelvételek különösen fontos a vész-helyreállítási eljárással lehet, mert cserélje le a másodlagos tárterületre rendszerek (például a szalagos biztonsági mentés), és lehetővé teszik adatok tooyour datacenter vagy tooalternate helyek toorestore szükség esetén.

![Videó ikonja](./media/storsimple-overview/video_icon.png) Hello videóban számára egy Azure StorSimple gyors bevezetés tooMicrosoft.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>StorSimple miért érdemes használni?
hello alábbi táblázat néhány hello kulcsfontosságú előnyt biztosít a Microsoft Azure StorSimple.

| Szolgáltatás | Előny |
| --- | --- |
| Transzparens integráció |Hello iSCSI protokoll tooinvisibly hivatkozás tárolási létesítmények használja. Ez biztosítja, hogy hello adatközpontjában hello felhőben tárolt adatok, vagy a távoli kiszolgálókon jelenik meg a toobe egyetlen helyen tárolja. |
| Alacsonyabb tárolási költségek |Osztja ki a megfelelő helyi vagy felhőalapú tárolók toomeet aktuális terén, és csak szükség esetén felhőbeli tárhelyén bővíti. Ez tovább csökkenti a tárhellyel kapcsolatos követelmények és költségek hello redundáns verziói kiküszöbölése révén ugyanazokat az adatokat (a deduplikáció) és a tömörítést. |
| Egyszerűsített tárolók kezelése |Rendszer-felügyeleti eszközök tooconfigure biztosít, és kezelheti a tárolt adatok a helyszínen, a távoli kiszolgálón, és hello felhőben. Emellett kezelheti a biztonsági mentés és visszaállítás funkciókat egy Microsoft Management Console (MMC) beépülő.|
| Továbbfejlesztett vész-helyreállítási és megfelelőség |Nincs szükség a bővített helyreállítási idő. Ehelyett helyreállítja adatok igény szerint. Ez azt jelenti, hogy a normál működés minimális megszakítás folytatása. Emellett házirendek toospecify a mentési ütemezések és az adatmegőrzés is konfigurálhatja. |
| Adatok mobilitási |Adatok feltöltése tooMicrosoft Azure felhőalapú szolgáltatások elérhetők a helyreállítási és áttelepítési célokból más helyekre. Emellett a StorSimple tooconfigure StorSimple felhő készülékek használhatja a Microsoft Azure-ban futó virtuális gépek (VM). virtuális gépek hello vizsgálati vagy helyreállítási célokra használhatja virtuális eszközök tooaccess tárolt adatokat. |
| Az üzletmenet folytonossága |Lehetővé teszi, hogy a StorSimple 5000-7000-es adatsorozat felhasználók toomigrate adatok tooa a StorSimple 8000 series eszközüket. |
| Hello Azure Government portálon rendelkezésre állás érdekében |StorSimple hello Azure Government portál érhető el. További információkért lásd: [a hello kormányzati Portal a helyszíni StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-gov-u2.md). |
| Adatvédelem és rendelkezésre állás |a StorSimple 8000 series hello zóna redundáns tárolás (ZRS), továbbá tooLocally redundáns tárolás (LRS) a és a georedundáns tárolás (GRS) támogatja. Tekintse meg a túl[Ez a cikk az Azure Storage redundancia beállításai](https://azure.microsoft.com/documentation/articles/storage-redundancy/) zrs-t a részletekért. |
| Kritikus fontosságú alkalmazások támogatása |StorSimple lehetővé teszi a megfelelő kötetek azonosítása, mint a helyileg rögzített, ami pedig lehetővé teszi, hogy kritikus fontosságú alkalmazások által megkövetelt adatok nem rétegzett toohello felhő. Helyileg rögzített kötetekhez nincsenek tulajdonos toocloud késések vagy kapcsolódási problémákat. További információ a helyileg rögzített kötetekhez: [hello StorSimple Device Manager szolgáltatás toomanage köteteket](storsimple-8000-manage-volumes-u2.md). |
| Alacsony késéssel és nagy teljesítményű |Felhő készülékek előnyeit hello nagy teljesítményű, alacsony késéssel részeit, a prémium szintű Azure storage hozhat létre. További információ a StorSimple prémium felhő készülékek: [központi telepítése és kezelése az Azure-ban egy StorSimple felhő készülék](storsimple-8000-cloud-appliance-u2.md). |


## <a name="storsimple-components"></a>StorSimple-összetevők
a Microsoft Azure StorSimple megoldáshoz hello hello a következő összetevőket tartalmazza:

* **A Microsoft Azure StorSimple eszköz** – helyszíni hibrid tárolási tömb, amely tartalmazza az SSD és HDD, redundáns vezérlők és az automatikus feladatátvételi lehetőségeket. hello tartományvezérlők kezelése a tároló rétegezésével, helyezését jelenleg (vagy a használt kiemelt) adatokat (a hello eszköz vagy a helyszíni kiszolgálók) helyi tároló ritkábban használt adatok toohello felhő áthelyezés közben.
* **StorSimple felhő készülék** – más néven hello StorSimple virtuális készüléknek, ez az egy hello architektúra replikáló hello StorSimple eszköz szoftverének verziójával, és a legtöbb képességeit hello fizikai hibrid tárolóeszköz. StorSimple felhő készülék hello Azure virtuális gép egyetlen csomópontján fut. Prémium szintű virtuális eszközök, a prémium szintű Azure storage előnyeit, amelyek Update 2 és újabb verziói is.
* **StorSimple Device Manager szolgáltatás** – az Azure-portál, amely lehetővé teszi egy egyetlen webes felhasználói felületen keresztüli kezelését a StorSimple eszköz vagy a StorSimple felhő készülék hello kiterjesztését. Is hello StorSimple Device Manager szolgáltatás toocreate használja és szolgáltatások kezelése, megtekintése és eszközök kezeléséhez, megtekintheti a riasztásokat, kötetek, kezelése és megtekinthető és kezelhető biztonsági mentési házirendek és hello biztonságimásolat-katalógus.
* **A Windows PowerShell-lel** – használható toomanage parancssori felülettel hello StorSimple eszközt. A Windows PowerShell-lel, amelyek lehetővé teszik tooregister lehetőséggel rendelkezik a StorSimple eszköz, hálózati illesztő hello konfigurálása az eszközön, bizonyos típusú frissítések telepítése, az eszköz hibaelhárításához hello támogatási munkamenet elérésével és hello módosítása az eszköz állapotát. A StorSimple Windows PowerShell toohello csatlakozás soros konzolon vagy a Windows PowerShell távoli eljáráshívás segítségével érheti el.
* **Az Azure PowerShell StorSimple-parancsmagok** –, amelyek lehetővé teszik tooautomate szolgáltatásiszint- és áttelepítési feladatok hello parancssorból Windows PowerShell-parancsmagok egy gyűjteményét. A StorSimple hello Azure PowerShell-parancsmagokkal kapcsolatos további információkért nyissa meg a toohello [parancsmag-referencia](/powershell/module/azure/?view=azuresmps-3.7.0#azure).
* **StorSimple Snapshot Manager** – egy MMC beépülő modul által használt kötet csoportok és hello Windows a kötet árnyékmásolata szolgáltatás toogenerate alkalmazáskonzisztens biztonsági mentéseket. Ezenkívül használja a StorSimple Snapshot Manager toocreate biztonsági mentési ütemezés és a Klónozás vagy kötetek visszaállítása.
* **A SharePoint StorSimple Adapter** – olyan eszköz, amely transzparens módon kiterjeszti a Microsoft Azure StorSimple tárolási és az adatvédelem tooSharePoint kiszolgálófarmok, miközben StorSimple tárolási megtekinthető és kezelhető a SharePoint központi hello Felügyeleti portálján.

az alábbi ábrán hello hello Microsoft Azure StorSimple architektúra vázlatos elrendezése és összetevők biztosít.

![StorSimple-architektúra](./media/storsimple-overview/overview-big-picture.png)

hello alábbi szakaszok ismertetik ezeket az összetevőket nagyobb részletességgel és azt ismertetik, hogyan hello megoldás adatok elrendezése, foglal helyet, és elősegíti a tárolók kezelése és az adatok védelmére. hello utolsó szakasza biztosít jelentésdefiníciókat néhány hello fontos fogalmakat tartalmazza, és fogalmak kapcsolódó tooStorSimple összetevők és a felügyelet.

## <a name="storsimple-device"></a>StorSimple-eszköz
hello Microsoft Azure StorSimple-eszköz a helyszíni hibrid tárolási tömb, amely elsődleges tárolási és a rajta tárolt iSCSI hozzáférés toodata biztosít. A felhőalapú tárolással kommunikációt kezeli, valamint segít tooensure hello biztonsági és az adatok bizalmas mivoltát minden Microsoft Azure StorSimple megoldáshoz hello tárolt.

hello StorSimple eszközt magában foglalja az SSD és HDD merevlemez-meghajtók, valamint a Feladatátvételi fürtszolgáltatás és automatikus támogatása. Egy megosztott processzor, a megosztott tároló és a két tükrözött vezérlő tartalmaz. Minden egyes tartományvezérlő hello következőket biztosítja:

* Kapcsolat tooa gazdaszámítógépen
* Másolatot toosix hálózati portok tooconnect toohello helyi hálózat (LAN)
* Hardver ellenőrzése
* A nem felejtő közvetlen elérésű memória (NVRAM), amely mindaddig megőrzi az adatokat, akkor is, ha tápellátása megszakad
* A fürttámogató toomanage szoftverfrissítéseket egy feladatátvevő fürt kiszolgálóinak frissítése, hogy a hello frissítések minimális vagy nincs hatással a szolgáltatás rendelkezésre állása
* Fürtszolgáltatás, mely funkciók, például egy háttér-fürt, a magas rendelkezésre állás biztosítása és minimalizálja a állapotot okoz, amely akkor jelentkezhet, ha egy HDD vagy SSD meghibásodik vagy offline állapotba

Csak egy tartományvezérlő aktív bármikor időben. Hello aktív vezérlő meghibásodásakor hello második vezérlő automatikusan aktívvá válik.

További információ: túl[StorSimple hardverösszetevők és állapot](storsimple-8000-monitor-hardware-status.md).

## <a name="storsimple-cloud-appliance"></a>StorSimple Cloud Appliance
StorSimple toocreate egy felhőalapú készülék hello architektúra és hello fizikai hibrid tárolóeszköz képességeket sorolja is használhatja. hello StorSimple felhő készülék (más néven a StorSimple virtuális készülék hello) az Azure virtuális gép egyetlen csomópontján fut. (A felhő készülék csak hozhatók létre egy Azure virtuális gépen. Nem hozható létre a StorSimple eszköz vagy egy helyszíni kiszolgálón.)

hello felhő készülék rendelkezik a következő funkciók hello:

* Egy fizikai készülék viselkedik, és kínálhat iSCSI felület toovirtual gépek hello felhőben.
* Felhő készülékek korlátlan számú létrehozása hello felhőben, és kapcsolja be- és kikapcsolását szükség szerint.
* Segíthet a helyszíni környezetben vészhelyreállítás, fejlesztési és tesztelési forgatókönyvek szimulálása, és elősegíti az elemszintű lekérési biztonsági másolatból.

StorSimple felhő készülék hello érhető el a két modell: hello 8010-es eszköz (korábbi nevén hello 1100 modell) és a 8020-as modellt hello eszköz. hello 8010 eszközhöz tartozik egy 30 TB maximális kapacitását. hello 8020 eszköz, amely kihasználja a prémium szintű Azure storage, 64 TB-os maximális kapacitása nem. (A helyi rétegeken, a prémium szintű Azure storage eltárolja SSD meghajtókon mivel standard tárolási eltárolja a HDD.) Vegye figyelembe, hogy egy prémium szintű Azure storage fiók toouse prémium szintű storage kell rendelkeznie. Prémium szintű storage kapcsolatos további információkért lépjen túl[prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../storage/common/storage-premium-storage.md).

További információ a StorSimple felhő készülék hello lépjen túl[központi telepítése és kezelése az Azure-ban egy StorSimple felhő készülék](storsimple-8000-cloud-appliance-u2.md).

## <a name="storsimple-device-manager-service"></a>A StorSimple eszköz kezelő szolgáltatás
A Microsoft Azure StorSimple webes felhasználói felülete lehetőséget nyújt (hello StorSimple Device Manager szolgáltatás), amely lehetővé teszi toocentrally datacenter kezelése és felhőbeli tárhelyén. Használhatja a hello StorSimple Device Manager szolgáltatás tooperform hello a következő feladatokat:

* A StorSimple eszköz rendszerbeállításainak konfigurálására.
* Konfigurálja, és a StorSimple eszközök biztonsági beállításainak kezelése.
* Felhő hitelesítő adatokat és a tulajdonságok konfigurálása.
* Konfigurálása és kezelése a kiszolgálón köteteket.
* Kötet csoportok konfigurálása.
* Készítsen biztonsági másolatot, és állítsa vissza adatokat.
* Figyelemmel kísérni a teljesítményét.
* Tekintse át a rendszer beállításait, és lehetséges problémák azonosításához.

Használhat hello StorSimple Device Manager szolgáltatás tooperform kivéve a rendszer állásidő, például a kezdeti beállítás és a frissítések telepítésének igénylő összes felügyeleti feladatokat.

További információ: túl[használata hello StorSimple Device Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-8000-manager-service-administration.md).

## <a name="windows-powershell-for-storsimple"></a>A Windows PowerShell-lel
A Windows PowerShell-lel is használja toocreate és hello Microsoft Azure StorSimple szolgáltatás kezelése és beállítása és a StorSimple eszközök figyeléséhez parancssori felületet biztosít. Egy Windows PowerShell-alapú, parancssori felületet, amely tartalmazza a StorSimple eszköz kezelésére szolgáló dedikált parancsmagok. A Windows PowerShell-lel funkciókat, amelyek lehetővé teszik, hogy rendelkezik:

* Eszköz regisztrálása.
* Állítsa be az eszközön hello hálózati illesztő.
* Bizonyos típusú frissítések telepítéséhez.
* Az eszköz hibaelhárításához hello támogatási munkamenet elérésével.
* Módosítsa a hello az eszköz állapotát.

Akkor érhessék el a Windows PowerShell StorSimple soros konzolon (egy gazdagépen számítógép közvetlenül csatlakoztatott toohello eszköz) vagy a Windows PowerShell távoli eljáráshívás segítségével távolról. Vegye figyelembe, hogy egy Windows PowerShell StorSimple tevékenységek, például az eszközök kezdeti regisztrációja, csak a hello soros konzolon végezhető.

További információ: túl[a Windows Powershellt használja a StorSimple tooadminister az eszköz](storsimple-8000-windows-powershell-administration.md).

## <a name="azure-powershell-storsimple-cmdlets"></a>Az Azure PowerShell StorSimple-parancsmagok
hello Azure PowerShell StorSimple parancsmagok olyan Windows PowerShell-parancsmagokat, amelyek lehetővé teszik a szolgáltatásiszint-tooautomate és áttelepítési feladatok hello parancssorból gyűjteménye. A StorSimple hello Azure PowerShell-parancsmagokkal kapcsolatos további információkért nyissa meg a toohello [parancsmag-referencia](/powershell/module/azure/?view=azuresmps-3.7.0).

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager egy Microsoft Management Console (MMC) beépülő modul használható toocreate egységes, időpontban a biztonsági másolatok a helyi rendszer és a felhőalapú adatokat. hello beépülő modul a Windows Server-alapú gazdagépen futtatja. A StorSimple Snapshot Manager használhatja:

* Konfigurálhatja, készítsen biztonsági másolatot, és törölje köteteket.
* Konfigurálja a kötet, amely az adatok biztonsági másolatát csoportok tooensure alkalmazáskonzisztens.
* Biztonsági házirendek kezelése, hogy az adatok biztonsági mentése egy előre meghatározott ütemezés szerint, és kijelölt helyen tárolni (helyileg vagy hello felhőben).
* Állítsa vissza a köteteket, és a fájlokat.

Biztonsági mentések pillanatképként, amely csak hello változások rögzítéséhez hello utolsó pillanatkép készítése óta, és teljes biztonsági mentés mint sokkal kevesebb helyet igényelnek a rendszer rögzíti. Hozzon létre biztonsági mentési ütemezést, vagy azonnali biztonsági másolatok készítése, igény szerint. Emellett használhatja a StorSimple Snapshot Manager tooestablish adatmegőrzési, hogy a vezérlő hány pillanatképeket a rendszer menti. Ha ezt követően kell a biztonsági másolat, a StorSimple Snapshot Manager segítségével toorestore adatait hello katalógus a helyi vagy felhőalapú pillanatfelvételek közül választhat. 

Ha katasztrófa történik, vagy ha szüksége van egy másik okból toorestore adatok, a StorSimple Snapshot Manager visszaállítja azt Növekményesen igény szerint. Adatok visszaállítása nem szükséges, hogy leállítja hello teljes rendszer állítsa vissza a fájlt, cserélje le a készülék, vagy helyezze át a műveletek tooanother hely közben.

További információ: túl[Mi az StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>SharePointhoz készült StorSimple-adapter
A Microsoft Azure StorSimple hello StorSimple Adapter tartalmaz a SharePoint, transzparens módon terjeszti ki a StorSimple-tároló és az adatvédelem szolgáltatások tooSharePoint kiszolgálófarmok választható összetevőként. hello adapter működik, a Blob Storage (RBS) szolgáltató és a hello SQL Server RBS funkció, így Ön toomove Blobok tooa server biztonsági másolat hello Microsoft Azure StorSimple rendszer. A Microsoft Azure StorSimple majd eltárolja hello BLOB helyileg vagy hello felhőben-használata alapján.

a SharePoint StorSimple Adapter hello felügyelete hello SharePoint központi felügyeleti portálon találja meg. Következésképpen SharePoint felügyeleti központosított marad, és minden tároló megjelenik-e toobe hello SharePoint-farm.

További információ: túl[SharePoint StorSimple adaptert](storsimple-adapter-for-sharepoint.md). 

## <a name="storage-management-technologies"></a>Felügyeleti tárolótechnológiák
Ezenkívül toohello dedikált más összetevők, a StorSimple eszköz és a virtuális eszköz, a Microsoft Azure StorSimple használja a következő szoftver technológiák tooprovide gyorselérési toodata és tooreduce tárhelyhasználatot hello:

* [Automatikus tárolórétegzés](#automatic-storage-tiering) 
* [Dinamikus kiosztás](#thin-provisioning) 
* [A deduplikáció és a tömörítés](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Automatikus tárolórétegzés
A Microsoft Azure StorSimple automatikusan adatokat logikai rétegek aktuális használatát, kor, és a kapcsolat tooother adatok alapján rendezi. A legtöbb aktív helyileg van tárolva, kevésbé aktív és inaktív adatokat a rendszer automatikusan adatok toohello felhő át. a következő diagram hello ezt a tárolási módszert mutatja be.

![StorSimple tárolási rétegek](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

tooenable gyorsan elérheti őket, a StorSimple tárolja nagyon aktív (gyakran használt adatok) adatokat SSD meghajtókon hello StorSimple eszközt. Alkalmanként használt adatokat tárol (mozog adatok) a HDD hello eszköz vagy hello datacenter kiszolgálókon. Inaktív adatok, a biztonsági mentési adatok átvitel során, és adatok megőrizve archiválási vagy megfelelőségi jellegű toohello felhő. 

> [!NOTE]
> 2. frissítés vagy újabb verzióját a helyileg rögzített kötet is megadhat, ebben az esetben a hello adatok hello helyi eszközön marad, és nem rétegzett toohello felhő. 


StorSimple állítja be, és újrarendezi az adatokat, és a használati minták tárolási hozzárendelését módosítsa. Például bizonyos adatokat válhatnak kevésbé aktív adott idő alatt. Fokozatosan kevésbé aktív válik, mivel az SSD-k tooHDDs és toohello felhő telepítik át. Ha ugyanazokat az adatokat ismét aktívvá válik, áttelepített hátsó toohello tárolóeszköz áll.

hello tároló-rétegezési folyamat a következőképpen történik:

1. Egy rendszergazda állít be egy Microsoft Azure felhőalapú társzolgáltatás fiókja.
2. hello rendszergazda hello soros konzol és hello StorSimple Device Manager (futtató hello Azure-portál) szolgáltatás tooconfigure hello eszköz és a fájl kiszolgálót használ, kötetek és adatok védelme szabályzatokat hoznak létre. A helyszíni gépeket (például a fájlkiszolgálók) hello Internet Small Computer System Interface (iSCSI) tooaccess hello StorSimple eszközt használja.
3. StorSimple kezdetben hello gyors SSD-rétegen hello eszköz tárolja az adatokat.
4. Hello megközelítések SSD-réteg kapacitását, mint a StorSimple deduplicates és hello legrégebbi adatblokkok tömöríti, és helyezi azokat toohello HDD-réteget.
5. Hello HDD-réteg megközelítések kapacitását, mint StorSimple titkosítja hello legrégebbi, és elküldi azokat biztonságosan toohello Microsoft Azure storage-fiók HTTPS használatával.
6. A Microsoft Azure replikákat hoz létre több hello adatok az adatközpontban és a távoli adatközpontban, győződjön meg arról, hogy hello adatok helyreállíthatók, ha katasztrófa történik.
7. Amikor hello fájlkiszolgáló hello felhőben tárolt adatok, a StorSimple adja vissza zökkenőmentesen, és tárolja a másolatukat hello SSD-rétegen hello StorSimple eszköz.

#### <a name="how-storsimple-manages-cloud-data"></a>Hogyan kezeli a StorSimple a felhőbeli adatát

StorSimple ügyféladatok deduplicates összes hello pillanatképeket és hello elsődleges (állomások által írt) adatokat. Míg a deduplikáció nagy a tárhely hatékonyságát, "Mi az hello felhőben található" bonyolult hello kérdését teszi. hello rétegzett elsődleges és hello pillanatkép adatainak átfedésben vannak egymással. Egy egyetlen adattömb hello felhőben rétegzett elsődleges adatként használható, és több pillanatképek is hivatkozhatnak. Minden felhőalapú pillanatfelvétel biztosítja, hogy minden hello időpontban adatok másolatát zárolva hello felhőbe, amíg, hogy a pillanatkép törlését.

Adatok csak töröltek, amelyek hello felhő nem hivatkozik toothat adatok esetén. Például ha egy felhő-pillanatfelvételt az összes hello adat, amely hello StorSimple eszközön, és ezután törölje az egyes elsődleges adatok vesszük, azt jelenik meg hello _elsődleges adatok_ dobja el azonnal. Hello _felhőalapú adatokat_ tartalmaz, amelyek hello rétegzett adatok és a biztonsági mentések hello, marad hello azonos. Ez azért, mert még mindig hivatkozik hello felhőbeli adatát pillanatképet. Pillanatkép törlését követően hello felhő (és bármilyen egyéb pillanatkép hivatkozott hello ugyanazokat az adatokat), fogyasztás elhagyta a felhő. Mielőtt tudjuk eltávolítani a felhőbeli adatát, ellenőrizzük, hogy a pillanatképek nem továbbra is hivatkoznak-e el az adatok. Ez a folyamat _szemétgyűjtés_ és a háttérben szolgáltatásként hello eszközön futó. Felhő adatok eltávolítása nincs közvetlen módon hello szemétgyűjtési szolgáltatás egyéb toothat adatok hello törlés előtt ellenőrzi. hello szemétgyűjtés függ hello teljes számát a pillanatképek és hello összesített adatokat. Általában hello felhő adatok törlődnek a kisebb, mint hét.


### <a name="thin-provisioning"></a>Dinamikus kiosztás
Dinamikus kiosztás egy olyan virtualizációs technológia, rendelkezésre álló tár jelenik meg tooexceed fizikai erőforrásokat. Ahelyett, hogy elegendő tárhely előre foglalással, a StorSimple éppen elegendő toomeet aktuális lemezterület használ vékony létesítési tooallocate. felhőalapú tárolás rugalmas jellege hello elősegíti a ezt a módszert használja, mert StorSimple növelheti vagy csökkentheti a felhőalapú tárolás toomeet tartalomkérései változó igényeinek.

> [!NOTE]
> Helyileg rögzített kötetek nem vékonyan létesített. Csak helyi kötet tooa lefoglalt tárolót ki van építve egészében hello kötet létrehozásakor.


### <a name="deduplication-and-compression"></a>A deduplikáció és a tömörítés
A Microsoft Azure StorSimple által használt deduplikációs és az adatok tömörítése toofurther csökkenti a tárolási követelményeket.

A deduplikáció csökkenti hello tárolt hello adatkészlet redundanciájának kiküszöbölése révén tárolt adatok teljes mennyisége. Információk változásával StorSimple figyelmen kívül hagyja változatlanul hello adatok, és csak a hello változtatásokat rögzíti. Továbbá a StorSimple csökkenti tárolt adatok mennyisége hello azonosításával és a felesleges adatok eltávolítása. 

> [!NOTE]
> A helyileg rögzített kötetekhez adatait nem deduplikált vagy tömörített. Azonban a helyileg rögzített kötetek biztonsági másolatokat deduplikált, és tömöríti.


## <a name="storsimple-workload-summary"></a>StorSimple a számítási feladatok összefoglalása
Hello támogatott StorSimple munkaterhelések összefoglalását az alábbi táblázatban.

| Forgatókönyv | Számítási feladat | Támogatott | Korlátozások | Verzió |
| --- | --- | --- | --- | --- |
| Együttműködés |Fájlmegosztás |Igen | |Az összes verzió |
| Együttműködés |Elosztott fájlmegosztás |Igen | |Az összes verzió |
| Együttműködés |SharePoint |Igen* |Csak a helyileg rögzített kötetekhez támogatott |Update 2 és újabb verziók |
| Archiválási |Egyszerű fájl archiválása |Igen | |Az összes verzió |
| Virtualizáció |Virtual machines (Virtuális gépek) |Igen* |Csak a helyileg rögzített kötetekhez támogatott |Update 2 és újabb verziók |
| Adatbázis |SQL |Igen* |Csak a helyileg rögzített kötetekhez támogatott |Update 2 és újabb verziók |
| Figyelőrendszer |Figyelőrendszer |Igen* |Támogatott, ha a StorSimple eszköz dedikált csak toothis munkaterhelés |Update 2 és újabb verziók |
| Biztonsági mentés |Elsődleges cél biztonsági mentése |Igen* |Támogatott, ha a StorSimple eszköz dedikált csak toothis munkaterhelés |Update 3 és újabb verziók |
| Biztonsági mentés |Másodlagos cél biztonsági mentése |Igen* |Támogatott, ha a StorSimple eszköz dedikált csak toothis munkaterhelés |Update 3 és újabb verziók |

*Igen &#42; -Megoldás alapelveket és korlátozásokat kell alkalmazni.*

a következő munkaterhelések hello nem támogatja a StorSimple 8000 sorozat eszközeire. StorSimple telepíthetők, ha az ilyen terhelések jár beállításai nem támogatottak.

* Orvosi lemezképpel végrehajtott telepítéshez
* Exchange
* VDI
* Oracle
* SAP
* Big Data
* Tartalom terjesztése
* Rendszerindítás SCSI

Az alábbiakban olvashat egy listát hello támogatott StorSimple infrastruktúra összetevőinek.

| Forgatókönyv | Számítási feladat | Támogatott | Korlátozások | Verzió |
| --- | --- | --- | --- | --- |
| Általános kérdések |Express Route |Igen | |Az összes verzió |
| Általános kérdések |FC DataCore |Igen* |Támogatott, ha DataCore SANsymphony |Az összes verzió |
| Általános kérdések |ELOSZTOTT FÁJLRENDSZER REPLIKÁCIÓS SZOLGÁLTATÁSA |Igen* |Csak a helyileg rögzített kötetekhez támogatott |Az összes verzió |
| Általános kérdések |Indexelés |Igen* |A rétegzett kötetek csak metaadatok indexelése támogat (adatok nélkül).<br>A helyileg rögzített kötetekhez a teljes indexelő támogatott. |Az összes verzió |
| Általános kérdések |Víruskereső |Igen* |A rétegzett kötetek csak nyissa meg a vizsgálati és a záró támogatott.<br> A helyileg rögzített kötetekhez teljes ellenőrzés esetén támogatott. |Az összes verzió |

*Igen &#42; -Megoldás alapelveket és korlátozásokat kell alkalmazni.*

Az alábbiakban az egyéb StorSimple toobuild megoldások használt szoftverek listáját.

| Munkaterhelésének típusát | A StorSimple használt szoftverek | Támogatott verziók|Hivatkozás toosolution útmutató| 
| --- | --- | --- | --- |
| Biztonsági mentés célja |Veeam |Veeam v 9-es és újabb verziók |[Biztonsági mentési cél a Veaam StorSimple](storsimple-configure-backup-target-veeam.md)|
| Biztonsági mentés célja |Exec VERITAS biztonsági mentése |Biztonsági mentési Exec 16 és újabb verziók |[A biztonsági mentési Exec biztonsági mentési cél StorSimple](storsimple-configure-backup-target-using-backup-exec.md)|
| Biztonsági mentés célja |VERITAS NetBackup |NetBackup 7.7.x és újabb verziók  |[Biztonsági mentési cél a NetBackup StorSimple](storsimple-configure-backuptarget-netbackup.md)|
| Globális fájlmegosztás <br></br> Együttműködés |Talon  |[A Talon StorSimple](https://www.talonstorage.com/products/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>StorSimple-terminológia
A Microsoft Azure StorSimple megoldásban való telepítése előtt javasoljuk, hogy tekintse át a következő hello szakkifejezéseket és definíciójukat.

### <a name="key-terms-and-definitions"></a>Legfontosabb kifejezések és meghatározások
| Kifejezés (mozaikszó vagy rövidítése) | Leírás |
| --- | --- |
| hozzáférés-vezérlési rekordot (ACR) |A Microsoft Azure StorSimple eszköz, amely meghatározza, hogy mely gazdagépek kapcsolódhatnak tooit kötetein társított rekord. hello meghatározása alapul hello iSCSI minősített nevét (IQN) hello gazdagépet (a hello ACR) tooyour StorSimple eszköz csatlakozó. |
| AES-256-RA |A 256 bites Advanced Encryption Standard (AES) titkosításához használt algoritmus adatok tooand hello felhőből átvitel során. |
| foglalásiegység-méret (Ausztráliai) |hello legkisebb memóriamennyiség, amely lehet egy fájl hozzárendelt toohold a Windows rendszer. Ha egy fájl mérete nem egy páros számú többszöröse az hello fürtméret, területnek használt toohold hello fájlnak kell lennie (mentése toohello hello fürtméret következő többszöröse) elveszett terület és hello merevlemez töredezettsége. <br>hello ajánlott az Azure StorSimple-köteteket Ausztráliai nem 64 KB-os, mert azt jól működik hello deduplikációs algoritmusokkal. |
| automatikus tárolórétegzés |SSD-k tooHDDs és tooa rétegű hello felhő, és a központi felhasználói felületről megadott összes felügyeleti engedélyezésével automatikusan áthelyezést kevésbé aktív adatok. |
| Biztonságimásolat-katalógus |Biztonsági mentések, általában kapcsolódó által használt hello alkalmazástípus gyűjteménye. Ez a gyűjtemény hello StorSimple Device Manager szolgáltatás felhasználói felületi hello biztonságimásolat-katalógus panel jelenik meg. |
| biztonsági mentési katalógusfájl |A rendelkezésre álló hello a backup database a StorSimple Snapshot Manager jelenleg tárolt pillanatképek listáját tartalmazó fájl. |
| a biztonsági mentési házirend |A kijelölt kötetek, a biztonsági másolat típusa és a, amely lehetővé teszi egy előre meghatározott ütemezés szerint toocreate biztonsági mentések ütemezését. |
| nagyméretű bináris objektumok (BLOB) |Egy adatbázis-kezelő rendszer egyetlen egységként tárolt bináris adatok gyűjteménye. Blobok jellemzően képek, hang-, vagy más multimédiás objektumok, bár egyes esetekben bináris végrehajtható kód egy BLOB tárolja. |
| Challenge Hésshake Authentication Protocol (CHAP) |A protokoll egy kapcsolattal, a jelszó vagy titkos kulcs megosztása hello társ alapuló társa tooauthenticate hello használatban. A CHAP egyirányú vagy kölcsönös lehet. Az egyirányú CHAP hello cél hitelesíti az kezdeményező. Kölcsönös CHAP a hello cél hello kezdeményező hitelesítéséhez, és adott hello kezdeményező hitelesíti hello cél használatához. |
| Klónozás |A kötet másolatát. |
| Felhő, egyetlen rétegként (CaaT) |A felhő tárolási integrált hello tároló-architektúra belül egy réteget, hogy az összes tároló megjelenik-e egy vállalati tárolóhálózat toobe részét. |
| felhőszolgáltató (CSP) |A szolgáltató a felhőalapú informatika meghatározására szolgáltatások. |
| felhő-pillanatfelvételt |Kötet hello felhőben tárolt adatok pont időponthoz kötött másolata. Egy felhőalapú pillanatfelvétel értéke megegyezik egy másik, a telephelytől távoli tárolórendszer replikálva tooa pillanatkép. Felhőalapú pillanatfelvételek különösen hasznosak a vész-helyreállítási eljárással. |
| Felhőalapú tárolás titkosítási kulcsa |Jelszó vagy a StorSimple eszköz tooaccess Titkosított hello által küldött adatokat az eszköz toohello felhő által használt kulcs. |
| fürttámogató frissítés |A feladatátvevő fürt kiszolgálóinak a szoftverfrissítések kezelése, hogy a hello frissítések minimális, vagy nincs hatással a szolgáltatás rendelkezésre állása. |
| DataPath |Funkcionális egység összekapcsolt adatfeldolgozási műveleteket gyűjteménye. |
| inaktiválása |Egy állandó műveletet, hogy oldaltörések hello hello StorSimple-eszköz kapcsolatot meg hello társított felhőalapú szolgáltatás. Felhőalapú pillanatfelvételek hello eszköz ezen folyamat után továbbra is és klónozható vagy a katasztrófa utáni helyreállítás. |
| lemez tükrözés |A logikai lemezkötet külön, rögzített replikációs meghajtókat a valós idejű tooensure folyamatos rendelkezésre állás érdekében. |
| tükrözést |A dinamikus lemezek a logikai lemez kötetek replikációját. |
| a dinamikus lemezek |A kötet lemezformátumot, hogy által használt logikai lemezkezelő (LDM) toostore hello adatok kezelése és több fizikai lemezeken. A dinamikus lemezek lehet kibővített tooprovide szabadítson fel helyet. |
| Kiterjesztett lemezek álló, lemezcsoport (EBOD) ház |A Microsoft Azure StorSimple eszköz extra merevlemez-meghajtóról lemezeket a további tárhely tartalmazó másodlagos ház. |
| FAT-kiépítés |Egy hagyományos tárolólétesítés mely Storage legyen lefoglalva terület alapján kell várható (és általában túl hello szükséges). Lásd még: *dinamikus kiosztás*. |
| merevlemez-meghajtó (HDD) |Egyedi lemezeinek száma általában toostore forgó adatokat használó meghajtóra. |
| hibrid felhőalapú tárolásról |Egy tároló-architektúra, amely a helyi és távoli, beleértve a felhőalapú tárolást. |
| Internet Small Computer System Interface (iSCSI) |Az Internet Protocol IP-alapú tárolási hálózati szabvány az adatok tárolási berendezések vagy létesítményekben csatolásának. |
| iSCSI-kezdeményező |Lehetővé teszi, hogy a gazdagép futtató Windows tooconnect tooan külső iSCSI-alapú tárolási hálózati szoftverösszetevő. |
| az iSCSI minősített nevét (IQN) |Az iSCSI-tároló vagy kezdeményező azonosító egyedi név. |
| iSCSI-tároló |Biztosít a központosított iSCSI lemez-alrendszereket a tárolóhálózat szoftverösszetevő. |
| élő archiválási |Egy tárolási módszert alkalmaz, amelyben archiválási adatok az elérhető összes hello idő (ez nem tárolja a telephelytől távol szalag, például). Microsoft Azure StorSimple használ, az élő archiválása. |
| helyileg rögzített kötet |hello eszközön található, és soha ne kötetre rétegzett toohello felhő. |
| helyi pillanatfelvétel |Kötet hello Microsoft Azure StorSimple eszközön tárolt adatok pont időponthoz kötött másolata. |
| Microsoft Azure StorSimple |Egy hatékony megoldás, datacenter tárolóeszköz és szoftver, amely lehetővé teszi, hogy informatikai szervezetek tooleverage felhőbeli tárhelyén, mintha az Adatközpont tárolójában lenne. StorSimple költségek csökkentése során leegyszerűsíti az adatok védelme és kezelése. hello megoldás elsődleges tárolási, archiválás, biztonsági mentési és vész-helyreállítási keresztül való zökkenőmentes integrációt biztosítanak hello felhő összesíti. SAN tárolás és a felhőbeli adatkezelés egy vállalati szintű platformon kombinálásával a StorSimple eszközök sebességét, az egyszerűség és megbízhatóságának tárolással kapcsolatos összes kell engedélyezni. |
| Teljesítmény- és hűtési modul (PCM) |A StorSimple eszköz hello áramforrások és hűtési ventilátor, hello hardverösszetevők ezért hello Power és Cooling modul nevét. hello elsődleges ház hello eszköz két 764W PCMs rendelkezik, mivel a hello EBOD ház két 580W PCMs. |
| Elsődleges ház |Fő szolgáltatással a StorSimple eszköz, amely tartalmazza a hello alkalmazás platform tartományvezérlők. |
| A helyreállítási idő célkitűzése (RTO) |hello maximális időt, amely egy üzleti vagy rendszer előtt a rendszer arra kell fordított teljes visszaállítása után egy olyan vészhelyzet esetén. |
| soros csatlakozású SCSI (SAS) |Merevlemez-meghajtó (HDD) típusú. |
| szolgáltatásadat-titkosítási kulcs |A kulcs végrehajtott elérhető tooany új StorSimple eszközt, amely hello StorSimple Device Manager szolgáltatás regisztrálja. hello konfigurációs adatok hello StorSimple Device Manager szolgáltatás és az eszköz hello között továbbított nyilvános kulccsal titkosított, és csak a titkos kulcs segítségével hello eszközén tud majd visszafejteni. Szolgáltatásadat-titkosítási kulcs lehetővé teszi, hogy hello szolgáltatás tooobtain a titkos kulcs a visszafejtéshez. |
| Szolgáltatásregisztrációs kulcs |Egy kulcs, amely segít úgy, hogy a hello Azure-portál a további felügyeleti műveletek hello StorSimple Device Manager szolgáltatás regisztrálása hello StorSimple eszközt. |
| Small Computer System Interface (SCSI) |Fizikai számítógépek kapcsolódásához és a közöttük adatok átadására szabványok csoportja. |
| tartós állapotú meghajtót (SSD) |Nincs mozgó részek; tartalmazó lemez Ha például egy USB-meghajtóra. |
| Tárfiók |Hozzáférési hitelesítő adatokat egy adott felhőbeli szolgáltatás szolgáltatója tooyour tárfiók összekapcsolva. |
| SharePointhoz készült StorSimple-adapter |A Microsoft Azure StorSimple összetevője, amely transzparens módon tooSharePoint kiszolgálófarmok kiterjeszti a StorSimple-tároló és az adatvédelem. |
| A StorSimple eszköz kezelő szolgáltatás |Kiterjesztési hello Azure-portál, amely lehetővé teszi toomanage, a helyszíni Azure StorSimple és a virtuális eszközök. |
| StorSimple Snapshot Manager |A Microsoft Management Console (MMC) beépülő kezeléséhez a Microsoft Azure StorSimple biztonsági mentési és helyreállítási műveletek. |
| biztonsági másolatok készítéséhez |Ez a szolgáltatás lehetővé teszi, hogy a felhasználó tootake hello kötet egy interaktív biztonsági mentést. Manuális biztonsági mentés kötet alapul véve megakadályozását tootaking az automatikus biztonsági mentés meghatározott házirendjében más módja. |
| Dinamikus kiosztás |Optimalizálás hello hatékonyságát, mely hello a rendelkezésre álló szabad hely a tárolórendszerek használt módszer. A dinamikus kiosztás hello-tároló lefoglalása alapján hello minimális szükséges lemezterület minden felhasználó egyszerre több felhasználó használ. Lásd még: *fat kiépítés*. |
| rétegezéséhez |Az aktuális használatát, kor, és a kapcsolat tooother adatok alapuló logikai csoportok adatok elrendezése. StorSimple automatikusan elrendezi a rétegek adatokat. |
| Kötet |Logikai tárolási területek meghajtó hello formájában. StorSimple-köteteket hello állomás, beleértve a felderített hello használata az iSCSI- és a StorSimple eszköz által csatlakoztatott toohello kötetek felelnek meg. |
| kötettároló |Kötetek és toothem hello beállítások csoportja. A StorSimple eszköz minden kötet kötettárolók vannak csoportosítva. Kötet tároló beállítások közé tartoznak a storage-fiókok, titkosítás beállításai küldött toocloud kapcsolódó titkosítási kulcsokkal és hello felhő érintő műveletek felhasznált sávszélesség. |
| kötet csoport |A StorSimple Snapshot Manager, a kötet csoport gyűjteménye készleteiből konfigurált toofacilitate biztonsági mentési feldolgozása. |
| Kötet árnyékmásolata szolgáltatás (VSS) |Egy Windows Server operációs rendszer szolgáltatás, amely elősegíti a alkalmazás konzisztencia által növekményes pillanatképek Kötetárnyékmásolat-felismerésre képes alkalmazások toocoordinate hello létrehozásának kommunikál. VSS biztosítja, hogy hello alkalmazások ideiglenesen inaktív amikor pillanatfelvételeket készít. |
| A Windows PowerShell-lel |A Windows PowerShell-alapú parancssori felület toooperate használja, és a StorSimple eszköz kezelése. Megőrzésével néhány, a Windows PowerShell hello alapvető képességeit, a kapcsolatnak a StorSimple eszköz kezelése körétől további dedikált parancsmagokat. |

## <a name="next-steps"></a>Következő lépések
További tudnivalók [StorSimple biztonsági](storsimple-8000-security.md).

