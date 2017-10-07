---
title: "aaaModify hello StorSimple eszköz konfigurációs |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello StorSimple Manager szolgáltatás tooreconfigure a StorSimple eszköz, amely már telepítve van."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: bb679264-8d46-429c-9ef7-630dc3b4a423
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/19/2017
ms.author: v-sharos
ms.openlocfilehash: 10a54c191260bf1baba58d28cdbfa0ed72217f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomodify-your-storsimple-device-configuration"></a>Hello StorSimple Manager szolgáltatás toomodify a StorSimple eszköz konfigurációs használata
## <a name="overview"></a>Áttekintés
a klasszikus Azure portálon hello **konfigurálása** lap tartalmaz olyan módon konfigurálhatja újra a StorSimple eszközön a StorSimple Manager szolgáltatás által kezelt összes hello eszköz paramétert. Ez az oktatóanyag azt ismerteti, hogyan használhatja a hello **konfigurálása** lap tooperform hello eszközszintű feladatok a következő:

* Eszköz beállításainak módosítása 
* Idő beállításainak módosítása 
* DNS-beállításainak módosítása 
* Hálózati illesztők módosítása
* A felcserélendő vagy IP-cím ismételt hozzárendelése

## <a name="modify-device-settings"></a>Eszköz beállításainak módosítása
hello eszközbeállítások hello eszköz és hello eszköz leírása hello rövid nevét tartalmazza.

> [!NOTE] 
> Hello eszköz neve a klasszikus Azure portálon hello nem módosítható. Az eszköz átnevezésekor hello nem támogatott.

A StorSimple eszköz, amely csatlakoztatott toohello StorSimple Manager szolgáltatás hozzá van rendelve egy alapértelmezett nevet. hello alapértelmezett neve általában tükrözi hello hello eszköz sorozatszámát. Például egy alapértelmezett eszköz neve 15 karakternél hosszabb, mint 8600-SHX0991003G44HT, például azt jelzi, hello következő:

* **8600** – annak jelzése hello eszközmodell.
* **SHX** – annak jelzése hello gyártási helyét.
* **0991003** -azt jelzi, a termék.
* **G44HT**– hello utolsó 5 számjegy sorozatszámai növekménye toocreate egyedi. Az egymást követő beállítása nem lehet.

Egy eszköz leírást is megadhat. Egy eszköz leírása általában azonosítja hello tulajdonosa és hello eszköz hello fizikai helye. hello leírási mezője 256-nál kevesebb karaktert kell tartalmaznia.

## <a name="modify-time-settings"></a>Idő beállításainak módosítása
Az eszköz szinkronizálnia kell ahhoz tooauthenticate idő tároló felhőszolgáltatást. Válassza ki az időzónát hello legördülő listából, és adja meg tootwo Network Time Protocol (NTP) kiszolgáló. hello elsődleges NTP-kiszolgáló szükséges, és ha használja a Windows PowerShell StorSimple tooconfigure az eszköz van megadva. Megadhatja a Windows Server hello alapértelmezett **time.windows.com** az NTP-kiszolgálóként. Megtekintheti a hello elsődleges NTP kiszolgálókonfiguráció hello a klasszikus Azure portálon keresztül, de hello Windows PowerShell felületet toochange kell használni azt.

hello másodlagos NTP-kiszolgáló konfigurálása nem kötelező. Hello klasszikus portál tooconfigure másodlagos NTP-kiszolgáló is használhatja. 

Hello NTP-kiszolgáló beállításakor győződjön meg arról, hogy a hálózat engedélyezi hello NTP forgalom toopass a datacenter toohello Internet a. Egy nyilvános NTP-kiszolgáló megadása esetén győződjön meg arról, hogy a hálózati tűzfalak és egyéb biztonsági eszközök-e a konfigurált tooallow NTP forgalom tootravel tooand a hálózaton kívül hello. Ha nem engedélyezett a kétirányú NTP-adatforgalmat, egy belső NTP-kiszolgáló (egy Windows rendszerbeli tartományvezérlőnek biztosítja ezt a funkciót) kell használnia. Ha az eszköz nem próbálta szinkronizálni az időt, nem lehet képes toocommunicate a felhő tárolási szolgáltatóval.

a nyilvános NTP-kiszolgálóhoz, nyissa meg toohello listáját toosee [NTP-kiszolgáló Web](http://support.ntp.org/bin/view/Servers/WebHome). 

### <a name="what-happens-if-hello-device-is-deployed-in-a-different-time-zone"></a>Mi történik, ha egy másik időzónába hello eszköz telepítése?
Ha egy másik időzónába hello eszközön van telepítve, hello eszköz időzóna módosíthatja. Fényében, hogy az összes hello biztonsági mentési házirendek hello eszköz időzóna használatához hello biztonsági mentési házirendek automatikusan módosítsa a hello új időzónájának megfelelően. Meg kell adni a felhasználói beavatkozás nélkül.

## <a name="modify-dns-settings"></a>DNS-beállításainak módosítása
A DNS-kiszolgáló használatos, amikor az eszköz kísérli meg a felhő tárolási szolgáltató toocommunicate. A magas rendelkezésre álláshoz szükséges tooconfigure mindkét hello elsődleges és másodlagos DNS-kiszolgálók hello hello kezdeti eszköz üzembe helyezése során. tooreconfigure hello elsődleges DNS-kiszolgáló, szüksége lesz toouse hello Windows PowerShell felületet a StorSimple eszköz.

toomodify hello másodlagos DNS-kiszolgáló, a klasszikus Azure portálon hello is használhatja.

## <a name="modify-network-interfaces"></a>Hálózati illesztők módosítása
Az eszköz hat eszköz hálózati adapterrel rendelkezik, négyet 1 gbe-s, illetve amelyek két 10 GbE. Ezek a kapcsolatok Data 0 – 5 adatok tartalma. 0, az 1, adatok 4 és adatok 5 esetén 1 gbe-s, mivel a DATA 2 és a DATA 3 10 GbE hálózati adapterrel.

Konfigurálása **hálózati kapcsolati beállítások** az egyes hello felületek toobe használt. tooensure magas rendelkezésre állás érdekében azt javasoljuk, hogy legalább két iSCSI és a felhőalapú kapcsolatain két az eszközön. Azt javasoljuk, de nem szükséges, hogy a nem használt csatolók le kell tiltani.

Amikor hello hálózati illesztők bármelyikét konfigurál, konfigurálnia kell egy virtuális IP-cím (VIP).

DATA 0 alapértelmezés szerint felhő engedélyezve. DATA 0 konfigurálásakor áll is szükséges tooconfigure két rögzített IP-cím, egy, az egyes vezérlők. Ezek állandó IP-címet lehet használt tooaccess hello eszközvezérlők közvetlenül, és akkor hasznos, ha a frissítések telepítése hello eszközön vagy hibaelhárítási céllal hello hello vezérlők elérésekor.

A StorSimple 8000 Series Update 1 DATA 0 útválasztási metrikája hello beállítása toohello legalacsonyabb; Ezért ha az eszközt a StorSimple 8000 Series Update 1 fut, akkor minden hello felhőalapú forgalom az DATA 0 keresztül történik. Jegyezze fel a ez Ha egynél több felhő-kompatibilis hálózati adaptert a StorSimple eszköz.

> [!NOTE]
> IP-címek hello vezérlő rögzített hello hello frissítések toohello eszköz karbantartásához használatosak. Emiatt rögzített IP-címei hello irányíthatók, és képes tooconnect toohello Internet kell lennie.
> 
> 

Minden egyes hálózati adapter a következő paraméterek hello jelennek meg:

* **Sebesség** – nem a felhasználó által konfigurálható paraméter. 0, az 1, adatok 4 és adatok 5 esetén mindig 1 gbe-s, mivel a DATA 2 és a DATA 3 10 GbE felületek.
  
  > [!NOTE]
  > Sebesség és a kétirányú mindig automatikus-egyeztetését. Jumbo-keretek támogatása nem támogatottak.
  > 
  > 
* **Csatoló állapota** – illesztőfelület engedélyezhetők és letilthatók. Ha engedélyezve van, a hello eszköz toouse hello felület kísérli meg. Azt javasoljuk, hogy csak a csatlakoztatott toohello hálózati és használt csatolók engedélyezve legyen. Minden nem használt csatolók letiltása.
* **Illesztőfelület-típuson** – Ez a paraméter lehetővé teszi a felhő tárolási forgalom tooisolate iSCSI adatforgalmat. Ez a paraméter hello a következők egyike lehet:
  
  * **Engedélyezett felhő** – engedélyezésekor hello eszköz hello felhő használja a kapcsolat toocommunicate.
  * **az iSCSI engedélyezve** – Ha engedélyezve van, hello eszköz a felület toocommunicate használandó hello iSCSI-gazdagép.
    
    Azt javasoljuk, hogy az iSCSI forgalom és a felhő tárolási forgalom elkülönítésére. Továbbá ne feledje, ha a gazdagépen belül hello ugyanazon az alhálózaton, az eszköz nem kell tooassign átjáró; azonban ha a gazdagép egy másik alhálózat, mint az eszköz, szüksége lesz egy átjáró tooassign.
* **IP-cím** – Ez lehet IPv4 vagy IPv6-alapú vagy mindkettőt. Hello eszköz hálózati illesztők hello IPv4 és IPv6-alapú címcsaládra is támogatottak. IPv4-alapú használata esetén adja meg egy 32-bit-es IP-cím (*xxx.xxx.xxx.xxx*) pont decimális jelöléssel. IPv6 használatakor egyszerűen adjon meg egy 4 számjegyből álló előtagot, és a 128 bites cím jön létre automatikusan az eszköz hálózati illesztőt ezt az előtagot.
* **Alhálózati** – ez toohello alhálózati maszk hivatkozik, és hello Windows PowerShell felületén keresztül van konfigurálva.
* **Átjáró** – Ez a hello alapértelmezett átjáró, amelyek nincsenek hello csomópontokkal toocommunicate kísérletek során ez a felület használandó azonos IP-címtér (alhálózat). hello alapértelmezett átjárót kell lennie a hello azonos címtere (alhálózat) hello illesztő IP-címet, hello alhálózati maszk alapján.
* **Statikus IP-cím** – Ez a mező akkor válik elérhetővé, csak a DATA 0 hello konfigurálása során felületet. A műveletek, például a frissítések vagy hibaelhárítási hello eszközre, szükség lehet a tooconnect közvetlenül toohello eszköz vezérlő. statikus IP-cím hello lehet használt tooaccess hello aktív és a passzív vezérlő hello az eszközön.

A vezérlő 0 és a vezérlő 1 újrakonfigurálhatja az hello a klasszikus Azure portálon keresztül.

> [!NOTE]
> * tooensure megfelelő műveletet, ellenőrizze a hello illesztőjének sebessége és a kétirányú eszköz csatolóhoz csatlakoztatott hello kapcsolón. Kapcsoló felületek vagy egyeztetni kell, vagy Gigabit Ethernet konfigurálni (1000 MB/s) és a kétirányú lehet. Alacsony sebességen vagy fél duplex működő felületek teljesítményproblémák eredményez.
> * toominimize megszakításait és állásidő, azt javasoljuk, hogy engedélyezi-e portfast egyes hello kapcsoló, amely az eszköz iSCSI hálózati illesztő hello portok csatlakoznak. Ezzel biztosíthatja, hogy hálózati kapcsolatot is hozhatók létre gyorsan egy feladatátvételi hello eseményben.
> 
> 

## <a name="swap-or-reassign-ips"></a>A felcserélendő vagy IP-cím ismételt hozzárendelése
Jelenleg, ha bármelyik hálózati adapter hello tartományvezérlő van hozzárendelt virtuális IP-címhez, amely használatban van (hello által ugyanazon vagy egy másik eszköz hello hálózat), majd hello vezérlő hajt végre feladatátvételt. Ezért ezért rendelkezik toofollow hello megfelelő eljárás Ha hello eszköz hálózati adapter, amelyek csere a virtuális IP-címmel létrehozhat egy ismétlődő IP-helyzetben.

Hajtsa végre a következő lépéseket tooswap hello, vagy ismételt hozzárendelése hello virtuális IP-címek a hello hálózati illesztők bármelyikét:

#### <a name="tooreassign-ips"></a>tooreassign IP-címek
1. Törölje a jelet hello IP-címet a két felülethez.
2. Hello IP-címek törlődik, miután hello új IP-cím hozzárendelése toohello megfelelő felületek.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[az MPIO konfigurálása a StorSimple eszköz](storsimple-configure-mpio-windows-server.md).
* Ismerje meg, hogyan túl[használata hello StorSimple Manager szolgáltatás tooadminister a StorSimple eszköz](storsimple-manager-service-administration.md).

