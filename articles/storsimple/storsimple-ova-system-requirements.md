---
title: "aaaMicrosoft Azure StorSimple virtuális tömb rendszerkövetelmények |} Microsoft Docs"
description: "A StorSimple virtuális tömb hello szoftver- és hálózati követelményeinek megismerése"
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: ea1d3bca-e71b-453d-aa82-440d2638f5e3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.openlocfilehash: 7a124873fdd806d409c7279851456e6347e7ec0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-system-requirements"></a>A StorSimple virtuális tömb rendszerkövetelményei
## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti a Microsoft Azure StorSimple virtuális tömb és hello storage ügyfelei hello tömb hello fontos rendszerre vonatkozó követelmények. Azt javasoljuk, hogy Ön hello információk előtt célszerű gondosan felülvizsgálni a StorSimple rendszer központi telepítését, és ezután körkörösen tooit szükség szerint üzembe helyezési és a következő művelet során.

hello rendszerre vonatkozó követelmények a következők:

* **Tárolási ügyfelek szoftverkövetelményei** -hello támogatott virtualizációs platformmal, webböngészők, iSCSI-kezdeményezők, SMB ügyfelek, virtuális eszköz minimális követelményeknek, és ismerteti az operációs rendszereket vonatkozó esetleges további követelményeket.
* **Hello StorSimple eszköz hálózatkezelési követelményei** -iSCSI, a felhő vagy a felügyeleti forgalom, hogy a tűzfal tooallow nyitva kell toobe hello portokkal kapcsolatos információkat nyújt.

Ez a cikk az közzétett adatok hello StorSimple rendszerkövetelmények virtuális tömbök tooStorSimple vonatkozik.

* 8000 sorozat eszközeire, a go túl[rendszerkövetelményei a StorSimple 8000 series eszköz](storsimple-system-requirements.md).
* 7000-es sorozathoz lépjen túl[az 5000-7000-es adatsorozat eszközét rendszerkövetelményei](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).

## <a name="software-requirements"></a>Szoftverkövetelmények
hello szoftverkövetelmények hello információt tartalmazniuk hello támogatott webböngészők, SMB verziók, virtualizálási platformokkal és hello virtuális eszköz minimális követelményeknek.

### <a name="supported-virtualization-platforms"></a>Támogatott virtualizációs platformmal
| **Hipervizor** | **Verzió** |
| --- | --- |
| Hyper-V |Windows Server 2008 R2 SP1 és újabb verziók |
| VMware ESXi |5.5-ös vagy újabb |

### <a name="virtual-device-requirements"></a>Virtuális eszköz követelményei
| **Összetevő** | **Követelmény** |
| --- | --- |
| Minimális számú virtuális processzort (mag) |4 |
| Minimális memória (RAM) |8 GB <br> Fájlkiszolgáló, 8 GB-ot legalább 2 millió fájlok és a 2 – 4 millió fájlok 16 GB|
| Szabad lemezterület<sup>1</sup> |Az operációsrendszer-lemez - 80 GB <br></br>Adatlemez - 500 GB too8 TB |
| Hálózati adaptert minimális száma |1 |
| Minimális internetes sávszélességet<sup>2</sup> |5 MB/s |

<sup>1</sup> - dinamikus kiosztása

<sup>2</sup> -hálózati követelmények eltérhetnek attól függően, hogy hello napi adatváltozási sebesség. Például ha egy eszköz tooback 10 GB-os vagy a további módosításokat egy nap alatt, majd hello a napi biztonsági mentéshez egy 5 MB/s-kapcsolaton keresztül is beletelhet too4.25 órában (ha hello adatokat nem lehet tömörített vagy nem deduplikált).

### <a name="supported-web-browsers"></a>Támogatott webböngészők
| **Összetevő** | **Verzió** | **További követelmények/megjegyzések** |
| --- | --- | --- |
| A Microsoft Edge |legújabb verzió | |
| Az Internet Explorer |legújabb verzió |Az Internet Explorer 11 tesztelése |
| Google Chrome |legújabb verzió |A Chrome 46 tesztelése |

### <a name="supported-storage-clients"></a>Támogatott tárolási ügyfelek
a következő szoftverkövetelmények hello vannak hello iSCSI-kezdeményezők, amelyek a StorSimple virtuális tömb (iSCSI-kiszolgálóként konfigurált) eléréséhez.

| **Támogatott operációs rendszerek** | **Szükséges verziója** | **További követelmények/megjegyzések** |
| --- | --- | --- |
| Windows Server |2008R2 SP1, 2012-BEN 2012R2 |StorSimple hozhat létre a dinamikusan kiosztott és teljesen kiosztott köteteket. Azt nem hozhat létre részben kiosztott köteteket. StorSimple-köteteket iSCSI csak a támogatottak: <ul><li>Egyszerű kötetekkel Windows alaplemezen.</li><li>Windows NTFS kötet formázásához.</li> |

a következő szoftverkövetelmények hello hello vonatkoznak a StorSimple virtuális tömb (fájlkiszolgálóként konfigurált) elérő SMB-ügyfelekről.

| **SMB-verzió** |
| --- |
| SMB 2.x |
| SMB 3.0 |
| SMB 3.02 |

> [!IMPORTANT]
> Ne másolja vagy Windows titkosított fájlrendszer (EFS) toohello StorSimple virtuális tömb fájlkiszolgáló; által védett fájlok tárolásához Ez egy nem támogatott konfigurációt eredményez. 
> 

### <a name="supported-storage-format"></a>Tárolási formátum támogatott.
Csak az Azure blob blokktárolást hello támogatott. Nem támogatja a lapblobokat. További információ [blokk blobokat és lapblobokat](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).

## <a name="networking-requirements"></a>Hálózati követelmények
hello következő táblázatban hello portra van szüksége a tűzfal tooallow iSCSI, az SMB, a felhő vagy a felügyeleti forgalom megnyitott toobe. Ebben a táblázatban *a* vagy *bejövő* toohello irányát, amelyről a bejövő ügyfélkérelmek kiszolgálásában az eszközhöz való hozzáféréshez hivatkozik. *Kimenő* vagy *kimenő* hivatkozik, amelyben a StorSimple eszköz kívülről, adatokat küldi hello telepítési túl toohello irány: például kimenő toohello Internet.

| **Port száma<sup>1</sup>** | **Bejövő vagy kimenő** | **Port hatókör** | **Szükséges** | **Megjegyzések** |
| --- | --- | --- | --- | --- |
| TCP 80-AS (HTTP) |Kimenő |WAN |Nem |Kimenő port használatban van az Internet access tooretrieve frissítések. <br></br>hello kimenő webalkalmazás-proxy egy felhasználó által konfigurálható. |
| A TCP 443-AS (HTTPS) |Kimenő |WAN |Igen |Kimenő port használja a hello felhőben található adatok eléréséhez. <br></br>hello kimenő webalkalmazás-proxy egy felhasználó által konfigurálható. |
| UDP 53 (DNS) |Kimenő |WAN |Bizonyos esetekben; Tekintse meg a megjegyzéseket. |Ez a port nem kötelező, csak akkor, ha egy internetes DNS-kiszolgálót használ. <br></br> Vegye figyelembe, hogy ha egy fájlkiszolgáló telepítése, azt javasoljuk, helyi DNS-kiszolgáló. |
| UDP 123 (NTP) |Kimenő |WAN |Bizonyos esetekben; Tekintse meg a megjegyzéseket. |Ez a port nem kötelező, csak akkor, ha az Internet alapú NTP-kiszolgáló használ.<br></br> Vegye figyelembe, hogy ha egy fájlkiszolgáló telepítése, ajánlott az Active Directory-tartományvezérlők és az idő szinkronizálása. |
| TCP 80-AS (HTTP) |A |LAN |Igen |Ez az hello bejövő portot helyi felhasználói felülete a helyi felügyeleti hello StorSimple eszközön. <br></br> Vegye figyelembe, hogy hello elérése HTTP Protokollon keresztül helyi felhasználói felület automatikusan átirányítja a tooHTTPS. |
| A TCP 443-AS (HTTPS) |A |LAN |Igen |Ez az hello bejövő portot helyi felhasználói felülete a helyi felügyeleti hello StorSimple eszközön. |
| TCP 3260-as (iSCSI) |A |LAN |Nem |Ez a port nem használt tooaccess adatok iSCSI keresztül. |

<sup>1</sup> nincs bejövő portokat kell megnyitnia azon toobe hello a nyilvános internethez.

> [!IMPORTANT]
> Győződjön meg arról, hogy hello a tűzfal nem módosíthatók, hello StorSimple eszköz és az Azure közötti SSL adatforgalmat visszafejtéséhez.
> 
> 

### <a name="url-patterns-for-firewall-rules"></a>A tűzfalszabályok URL-mintával
A hálózati rendszergazdák gyakran konfigurálhatja a speciális tűzfal hello URL-cím minták toofilter hello alapján bejövő és kimenő forgalom hello. A virtuális tömb és a StorSimple eszköz Manager szolgáltatás hello függ a más Microsoft-alkalmazások, például az Azure Service Bus, az Azure Active Directory hozzáférés-vezérlés, a storage-fiókok és a Microsoft Update-kiszolgálókon. Ezek az alkalmazások társított hello URL-mintával használt tooconfigure tűzfalszabályok lehet. Fontos, hogy ezek az alkalmazások társított hello URL-mintával módosíthatja toounderstand. Ezzel viszont hello hálózati rendszergazda toomonitor igényelnek, és a StorSimple, és szükség esetén tűzfalszabályainak frissítése. 

Azt javasoljuk, hogy állítsa a tűzfalszabályok a kimenő forgalom liberally rögzített IP-címek, a legtöbb esetben a StorSimple alapján. Azonban az alábbi tűzfalszabályokat, amelyek biztonságos környezetben szükséges toocreate speciális tooset hello információt is használhatja.

> [!NOTE]
> 
> * hello eszköz (forrás) IP-címek mindig meg kell tooall hello felhő-kompatibilis hálózati adapterek. 
> * hello cél IP-címet kell megadni túl[Azure datacenter IP-címtartományok](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).
> 
> 

| Az URL-minta | Összetevő/funkció |
| --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*` |A StorSimple eszköz kezelő szolgáltatás<br>Access Control Service<br>Azure Service Bus |
| `http://*.backup.windowsazure.com` |Eszközregisztráció |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Tanúsítvány-visszavonás |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Az Azure storage-fiókok és figyelése |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |A Microsoft Update-kiszolgálókon<br> |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*` |Támogatási csomag |
| `http://*.data.microsoft.com ` |Telemetria szolgáltatás a Windows rendszerben lásd: hello [a felhasználói élmény és diagnosztikai telemetriai adatok frissítése](https://support.microsoft.com/en-us/kb/3068708) |

## <a name="next-step"></a>Következő lépés
* [Készítse elő a hello portál toodeploy a StorSimple virtuális tömb](storsimple-virtual-array-deploy1-portal-prep.md)

