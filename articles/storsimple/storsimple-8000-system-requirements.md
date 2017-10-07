---
title: "aaaStorSimple 8000 sorozat rendszerkövetelmények |} Microsoft Docs"
description: "Szoftverek, hálózat, és magas rendelkezésre állás biztosításához és a Microsoft Azure StorSimple megoldáshoz ajánlott eljárásai ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: alkohli
ms.openlocfilehash: f13ccdb7cb317d72c60e9c2fe49937764d10b43e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-software-high-availability-and-networking-requirements"></a>A StorSimple 8000 series szoftver, a magas rendelkezésre állás és a hálózati követelmények

## <a name="overview"></a>Áttekintés

Üdvözli az Azure StorSimple tooMicrosoft. Ez a cikk ismerteti a fontos rendszerfájlokra követelmények és ajánlott eljárások a StorSimple eszköz és a hello storage ügyfelei hello eszköz. Azt javasoljuk, hogy Ön hello információk előtt célszerű gondosan felülvizsgálni a StorSimple rendszer központi telepítését, és ezután körkörösen tooit szükség szerint üzembe helyezési és a következő művelet során.

hello rendszerre vonatkozó követelmények a következők:

* **Tárolási ügyfelek szoftverkövetelményei** -hello támogatott operációs rendszerek és az operációs rendszereket vonatkozó esetleges további követelményeket ismerteti.
* **Hello StorSimple eszköz hálózatkezelési követelményei** -iSCSI, a felhő vagy a felügyeleti forgalom, hogy a tűzfal tooallow nyitva kell toobe hello portokkal kapcsolatos információkat nyújt.
* **A StorSimple követelményei magas rendelkezésre állású** - magas rendelkezésre állású írja le, és gyakorlati tanácsok a StorSimple eszköz és a fogadó számítógép.

## <a name="software-requirements-for-storage-clients"></a>Tárolási ügyfelek szoftverkövetelményei

a következő szoftverkövetelmények hello hello tárolási ügyfelek férnek hozzá a StorSimple eszköz vonatkoznak.

| Támogatott operációs rendszerek | Szükséges verziója | További követelmények/megjegyzések |
| --- | --- | --- |
| Windows Server |2008 R2 SP1, 2012, 2012 R2-BEN 2016 |StorSimple iSCSI-köteteket a csak a következő Windows lemeztípusok hello használatát támogatja:<ul><li>Egyszerű kötet alaplemezen</li><li>Egyszerű és tükrözött kötet dinamikus lemezen</li></ul>Csak hello szoftver iSCSI-kezdeményezők jelen hello operációs rendszeren natív módon támogatottak. Hardver iSCSI-kezdeményezők nem támogatottak.<br></br>Windows Server 2012 és a 2016 dinamikus kiosztás és az odx-et szolgáltatások iSCSI StorSimple-kötet használata támogatott.<br><br>StorSimple hozhat létre a dinamikusan kiosztott és teljesen kiosztott köteteket. Azt nem hozhat létre részben kiosztott köteteket.<br><br>A dinamikusan kiosztott kötet újraformázást hosszú időt is igénybe vehet. Javasoljuk hello kötetet, majd létrehoz egy új újraformázást helyett. Ha továbbra is inkább tooreformat kötet:<ul><li>Futtassa a következő parancs előtt hello újraformázza tooavoid hely visszanyerése késések hello: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Hello formázás befejezése után használja hello következő parancsot a terület-visszanyerést toore engedélyezése:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>A gyorsjavítás a Windows Server 2012 hello leírtak [KB 2878635](https://support.microsoft.com/kb/2870270) tooyour Windows Server rendszerű számítógépeket.</li></ul></li></ul></ul> StorSimple Snapshot Manager vagy a StorSimple adaptert konfigurál a SharePoint, ha túl lépjen[választható összetevők szoftverkövetelményei](#software-requirements-for-optional-components). |
| VMware ESX |5.5 és 6.0 |VMware vSphere iSCSI-ügyfélként támogatott. VAAI-blokk támogatja a VMware vSphere StorSimple eszközön. |
| Linux RHEL vagy CentOS |5, 6 és 7 |Nyissa meg az iSCSI kezdeményező verzióival 5, 6 és 7 Linux iSCSI ügyfelek támogatása. |
| Linux |SUSE Linux 11 | |

> [!NOTE]
> IBM AIX jelenleg nem támogatott a StorSimple.


## <a name="software-requirements-for-optional-components"></a>Választható összetevők szoftverkövetelményei

a következő szoftverkövetelmények hello hello StorSimple összetevők (StorSimple Snapshot Manager és a SharePoint StorSimple Adapter) vonatkoznak.

| Összetevő | Gazdagép platformja | További követelmények/megjegyzések |
| --- | --- | --- |
| StorSimple Snapshot Manager |Windows Server 2008 R2 SP1, 2012, 2012 R2 |A StorSimple Snapshot Manager Windows Server használata szükséges, a biztonsági mentés/visszaállítás tükrözött dinamikus lemez, és bármely alkalmazáskonzisztens biztonsági mentés.<br> StorSimple Snapshot Manager csak Windows Server 2008 R2 SP1 (64 bites), Windows Server 2012 R2 és Windows Server 2012 támogatott.<ul><li>Ha Windows Server 2012 rendszert StorSimple Snapshot Manager telepítése előtt telepítenie kell a .NET 3.5 – 4.5.</li><li>Ha a Windows Server 2008 R2 SP1 használata esetén telepítenie kell a Windows Management Framework 3.0 StorSimple Snapshot Manager telepítése előtt.</li></ul> |
| SharePointhoz készült StorSimple-adapter |Windows Server 2008 R2 SP1, 2012, 2012 R2 |<ul><li>StorSimple Adapter a SharePoint csak a SharePoint 2010 és SharePoint 2013 rendszeren támogatott.</li><li>RBS SQL Server Enterprise Edition, 2008 R2 vagy 2012 verzió szükséges.</li></ul> |

## <a name="networking-requirements-for-your-storsimple-device"></a>A StorSimple eszköz hálózatkezelési követelményei

A StorSimple eszköz egy zárolt eszközt. Azonban portokat kell megnyitni a tűzfalon tooallow iSCSI, a felhő és a felügyeleti forgalom toobe. hello következő táblázatban hello portra van szüksége a tűzfal megnyitott toobe. Ebben a táblázatban *a* vagy *bejövő* toohello irányát, amelyről a bejövő ügyfélkérelmek kiszolgálásában az eszközhöz való hozzáféréshez hivatkozik. *Kimenő* vagy *kimenő* hivatkozik, amelyben a StorSimple eszköz kívülről, adatokat küldi hello telepítési túl toohello irány: például kimenő toohello Internet.

| Port száma<sup>1,2</sup> | Bejövő vagy kimenő | Port hatókör | Szükséges | Megjegyzések |
| --- | --- | --- | --- | --- |
| TCP 80-AS (HTTP)<sup>3</sup> |Kimenő |WAN |Nem |<ul><li>Kimenő port használatban van az Internet access tooretrieve frissítések.</li><li>hello kimenő webalkalmazás-proxy egy felhasználó által konfigurálható.</li><li>tooallow Rendszerfrissítések, ezt a portot is nyitva kell lennie a hello vezérlő rögzített IP-címei.</li></ul> |
| A TCP 443-AS (HTTPS)<sup>3</sup> |Kimenő |WAN |Igen |<ul><li>Kimenő port használja a hello felhőben található adatok eléréséhez.</li><li>hello kimenő webalkalmazás-proxy egy felhasználó által konfigurálható.</li><li>tooallow Rendszerfrissítések, ezt a portot is nyitva kell lennie a hello vezérlő rögzített IP-címei.</li><li>Ezt a portot is használja a mindkét hello tartományvezérlőkön szemétgyűjtési.</li></ul> |
| UDP 53 (DNS) |Kimenő |WAN |Bizonyos esetekben; Tekintse meg a megjegyzéseket. |Ez a port nem kötelező, csak akkor, ha egy internetes DNS-kiszolgálót használ. |
| UDP 123 (NTP) |Kimenő |WAN |Bizonyos esetekben; Tekintse meg a megjegyzéseket. |Ez a port nem kötelező, csak akkor, ha az Internet alapú NTP-kiszolgáló használ. |
| 9354 TCP |Kimenő |WAN |Igen |hello StorSimple eszköz toocommunicate hello StorSimple Device Manager szolgáltatás a hello kimenő portot használja. |
| 3260-as (iSCSI) |A |LAN |Nem |Ez a port nem használt tooaccess adatok iSCSI keresztül. |
| 5985 |A |LAN |Nem |Hello StorSimple eszközt a StorSimple Snapshot Manager toocommunicate bejövő portot használja.<br>Ezt a portot is használja, amikor távolról csatlakoztat tooWindows PowerShell StorSimple HTTP Protokollon keresztül. |
| 5986 |A |LAN |Nem |Ezt a portot használja, amikor távolról csatlakoztat tooWindows PowerShell StorSimple HTTPS-KAPCSOLATON keresztül. |

<sup>1</sup> nincs bejövő portokat kell megnyitnia azon toobe hello a nyilvános internethez.

<sup>2</sup> több portot hajt egy átjáró konfigurálása, ha hello kimenő adatforgalomra sorrendje határozza meg hello port útválasztási rendelés leírtak alapján [Port útválasztási](#routing-metric), az alábbi.

<sup>3</sup> hello vezérlő rögzített IP-címei a StorSimple eszköz irányítható kell lennie, és képes tooconnect toohello Internet közvetlenül, sem hello segítségével konfigurált webalkalmazás-proxy. hello rögzített IP-címek hello frissítések toohello eszköz karbantartásához használatosak. Ha hello eszközvezérlők nem tud csatlakozni az interneten keresztül hello rögzített IP-címek toohello, csak akkor tudja tooupdate a StorSimple eszközt.

> [!IMPORTANT]
> Győződjön meg arról, hogy hello a tűzfal nem módosíthatók, hello StorSimple eszköz és az Azure közötti SSL adatforgalmat visszafejtéséhez.


### <a name="url-patterns-for-firewall-rules"></a>A tűzfalszabályok URL-mintával

A hálózati rendszergazdák gyakran konfigurálhatja a speciális tűzfal hello URL-cím minták toofilter hello alapján bejövő és kimenő forgalom hello. A StorSimple eszköz és a StorSimple Device Manager szolgáltatás hello függenek más Microsoft-alkalmazások, például az Azure Service Bus, az Azure Active Directory hozzáférés-vezérlés, a storage-fiókok és a Microsoft Update-kiszolgálókról. Ezek az alkalmazások társított hello URL-mintával használt tooconfigure tűzfalszabályok lehet. Fontos, hogy ezek az alkalmazások társított hello URL-mintával módosíthatja toounderstand. Ezzel viszont hello hálózati rendszergazda toomonitor igényelnek, és a StorSimple, és szükség esetén tűzfalszabályainak frissítése.

Azt javasoljuk, hogy állítsa a tűzfalszabályok a kimenő forgalom liberally rögzített IP-címek, a legtöbb esetben a StorSimple alapján. Azonban az alábbi tűzfalszabályokat, amelyek biztonságos környezetben szükséges toocreate speciális tooset hello információt is használhatja.

> [!NOTE]
> hello eszköz (forrás) IP-címek mindig meg kell tooall hello engedélyezett hálózati adapterrel. hello cél IP-címet kell megadni túl[Azure datacenter IP-címtartományok](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


#### <a name="url-patterns-for-azure-portal"></a>URL-mintával az Azure-portálon

| Az URL-minta | Összetevő/funkció | Eszköz IP-címek |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`<br>`https://login.windows.net` |A StorSimple eszköz kezelő szolgáltatás<br>Access Control Service<br>Azure Service Bus<br>Hitelesítési szolgáltatás |A felhőalapú hálózati illesztők |
| `https://*.backup.windowsazure.com` |Eszközregisztráció |DATA 0 csak |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Tanúsítvány-visszavonás |A felhőalapú hálózati illesztők |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Az Azure storage-fiókok és figyelése |A felhőalapú hálózati illesztők |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |A Microsoft Update-kiszolgálókon<br> |Vezérlő rögzített IP-címek csak |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Vezérlő rögzített IP-címek csak |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Támogatási csomag |A felhőalapú hálózati illesztők |

#### <a name="url-patterns-for-azure-government-portal"></a>Azure Government portál URL-mintával

| Az URL-minta | Összetevő/funkció | Eszköz IP-címek |
| --- | --- | --- |
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`<br>`https://login-us.microsoftonline.com` |A StorSimple eszköz kezelő szolgáltatás<br>Access Control Service<br>Azure Service Bus<br>Hitelesítési szolgáltatás |A felhőalapú hálózati illesztők |
| `https://*.backup.windowsazure.us` |Eszközregisztráció |DATA 0 csak |
| `http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*` |Tanúsítvány-visszavonás |A felhőalapú hálózati illesztők |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` |Az Azure storage-fiókok és figyelése |A felhőalapú hálózati illesztők |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com` |A Microsoft Update-kiszolgálókon<br> |Vezérlő rögzített IP-címek csak |
| `http://*.deploy.akamaitechnologies.com` |Akamai CDN |Vezérlő rögzített IP-címek csak |
| `https://*.partners.extranet.microsoft.com/*`<br>`https://dcupload.microsoft.com/`<br>`https://*.support.microsoft.com/` |Támogatási csomag |A felhőalapú hálózati illesztők |

### <a name="routing-metric"></a>Útválasztási metrika

Útválasztási metrika tartozik hello felületek és hello átjáró, amely hello adatok toohello megadott útvonal hálózatok. Útválasztási metrikát használja hello útválasztási protokoll toocalculate hello legjobb elérési tooa adott célra, ha Tanulja meg több útvonal létezik toohello azonos céllal. hello alacsonyabb hello útválasztási metrika, hello magasabb hello prioritása.

Hello környezet storsimple, ha több hálózati adapterrel és az átjárók konfigurálása toochannel forgalom, hello útválasztási metrika play toodetermine hello relatív rendelés mely hello a felületek használt első határozza meg. hello útválasztási metrika hello felhasználó által nem módosítható. Azonban használhat hello `Get-HcsRoutingTable` parancsmag tooprint hello útvonaltábla (és metrikákat) a StorSimple eszköz. További információ a Get-HcsRoutingTable parancsmag [hibaelhárítási StorSimple telepítési](storsimple-troubleshoot-deployment.md).

hello útválasztási metrika használt algoritmust az Update 2 és újabb verziókban is az alábbiak.

* Az előre meghatározott értékek toonetwork felületek van rendelve.
* Fontolja meg egy példa tábla alább látható a hozzárendelt értékek toohello különböző hálózati adapterek a felhőalapú vagy felhő-letiltva, de egy konfigurált átjáró. Megjegyzés: hello hozzárendelt értékei csak a példában szereplő értékeket.

    | Hálózati illesztő | A felhőalapú | Felhő-letiltva átjáróval |
    |-----|---------------|---------------------------|
    | Data 0  | 1            | -                        |
    | Adatok 1  | 2            | 20                       |
    | A Data 2  | 3            | 30                       |
    | A Data 3  | 4            | 40                       |
    | Data 4  | 5            | 50                       |
    | Data 5  | 6            | 60                       |


* hello amelyben hello felhőforgalom hello hálózati illesztők keresztül továbbítódik sorrendje:
  
    *Data 0 > adatok 1 > szerint megadott dátum, 2 > adatok 3 > adatok 4 > Data 5*
  
    Ez a következő példa hello által viszonylag.
  
    Vegye figyelembe a StorSimple eszköz, a két a felhőalapú hálózati felületek, a Data 0 és az 5. Adatok 1 – 4 adatok felhő-tiltva van, de konfigurált átjáró. ahol forgalmat továbbítja az eszköz hello sorrendje a következő lesz:
  
    *Data 0 (1) > adatok [6] 5 > adatok 1 (20) > adatok 2 (30) > adatok 3 (40) > adatok 4 (50)*
  
    *hello számok zárójelben hello megfelelő útválasztási metrika jelzi.*
  
    Data 0 nem sikerül, ha a irányítása tól Data 5 számozásig hello felhőforgalom beolvasása. Fényében, hogy az átjáró minden más hálózati van konfigurálva, ha a Data 0 és a Data 5 számozásig toofail, hello felhőalapú forgalom halad keresztül adatokat 1.
* Ha nem sikerül egy felhő-kompatibilis hálózati adaptert, akkor felülettel rendelkező 30 második késedelem tooconnect toohello 3 újrapróbálás. Ha összes hello újrapróbálkozás sikertelen lesz, hello akkor kimenő forgalomról irányított toohello következő érhető el a felhőalapú felület hello útválasztási táblázat alapján. Ha az összes hello a felhőalapú hálózati illesztők sikertelen lesz, majd hello eszköz hajt végre feladatátvételt toohello más vezérlő (újraindítás nem szükséges ebben az esetben).
* Ha egy iSCSI-kompatibilis hálózati adaptert egy virtuális IP-hiba, 2 másodperc késéssel 3 újrapróbálás lesz. Ez a viselkedés tartózkodott hello korábbi verzióiról hello azonos. Hello iSCSI hálózati adaptereken sikertelen lesz, ha a vezérlő feladatátvétel (újraindítás kísérik) történik.
* Is riasztás esetén a StorSimple eszköz VIP hiba esetén. További információ: túl[riasztás rövid összefoglaló](storsimple-8000-manage-alerts.md).
* Tekintetében, az újrapróbálkozásokat iSCSI elsőbbséget élvez felhő.
  
    Vegye figyelembe a következő példa hello: A StorSimple eszköz engedélyezett két hálózati adapterrel rendelkezik, a Data 0 és az adatok 1. Data 0 felhő-kompatibilis mivel Data 1 mindkét felhőben, és iSCSI-engedélyezve. A felhő vagy iSCSI engedélyezve vannak a más hálózati interfész nem ezen az eszközön.
  
    Ha adatok 1 sikertelen, mivel ez hello utolsó iSCSI hálózati illesztő, ennek eredményeképpen az egy tartományvezérlő feladatátvételi tooData 1 on hello más vezérlő.

### <a name="networking-best-practices"></a>Gyakorlati tanácsok hálózat

Továbbá fent hálózati követelményei hello optimális teljesítmény érdekében a StorSimple megoldás toohello adja a következő gyakorlati tanácsok toohello igazodik:

* Győződjön meg arról, hogy a StorSimple eszköz rendelkezik egy dedikált 40 MB/s sávszélesség (vagy több) mindenkor. A sávszélesség nem lehet megosztva (vagy foglalási biztosítani kell a QoS-házirendek hello használata) egyéb alkalmazásokkal.
* Győződjön meg arról, mindig hálózati kapcsolat toohello Internet áll rendelkezésre. Szórványos vagy nem megbízható internetes kapcsolatok toohello eszközöket, beleértve a internetkapcsolat nélküli minden, egy nem támogatott konfigurációt eredményez.
* Különítse el hello iSCSI és a felhőalapú forgalom dedikált hálózati adapterek iSCSI és a felhőalapú hozzáférés az eszközön. További információkért lásd: hogyan túl[módosítása a hálózati adapterek](storsimple-8000-modify-device-config.md#modify-network-interfaces) a StorSimple eszköz.
* Ne használjon egy hivatkozás összesítési vezérlő protokoll (LACP) konfigurációban a hálózati adapterek. Ez a beállítás egy nem támogatott.

## <a name="high-availability-requirements-for-storsimple"></a>A StorSimple követelményei magas rendelkezésre állás

hello hardver platform hello StorSimple megoldás részét képező alapot nyújt az adatközpontban található magas rendelkezésre állású, és hibatűrő tárolási infrastruktúra rendelkezésre állásának és megbízhatóságának funkciókra van. Azonban nincsenek követelmények, és be kell tartania toohelp bevált gyakorlatokat a StorSimple megoldásban hello rendelkezésre állásának biztosításához. StorSimple telepítése előtt gondosan tekintse át a következő követelmények és ajánlott eljárások hello StorSimple eszköz és a csatlakoztatott számítógépek hello.

Figyelés, és a StorSimple eszköz hello hardverösszetevők karbantartásával kapcsolatos további információkért lépjen túl[hello StorSimple Device Manager szolgáltatás toomonitor hardverösszetevők és állapot](storsimple-8000-monitor-hardware-status.md) és [ StorSimple hardver összetevő cseréje](storsimple-8000-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>A magas rendelkezésre állású követelményeket és eljárásokat a StorSimple eszköz

Következő információ gondosan tekintse át hello tooensure hello magas rendelkezésre állását a StorSimple eszközt.

#### <a name="pcms"></a>PCMs

A StorSimple eszközök közé tartozik a redundáns, közbeni-cserélhető energia- és hűtési modulok (PCMs). Minden egyes PCM elég kapacitás tooprovide szolgáltatás hello teljes ház rendelkezik. tooensure magas rendelkezésre állású, mindkét PCMs telepítve kell lennie.

* Kapcsolódás a PCMs toodifferent power források tooprovide rendelkezésre állási, abban az esetben, ha egy energiagazdálkodási sikertelen.
* Ha nem sikerül egy PCM, kérelem azonnal helyettesíti.
* Csak akkor, ha hello helyettesítő, és készen áll a tooinstall, távolítsa el a sikertelen PCM azt.
* Ne távolítsa el egyszerre mindkét PCMs. hello PCM modul hello biztonsági mentési akkumulátor modul tartalmazza. Mindkét eltávolítása a hello PCMs eredményezi egy leállítási akkumulátor védelem nélkül, és hello az eszköz állapotát a rendszer nem menti. Hello töltöttségű telepre vonatkozó további információért látogasson túl[karbantartása hello biztonsági mentési akkumulátor modul](storsimple-8000-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>A tartományvezérlő-modulok

A StorSimple eszközök közé tartozik a redundáns, közbeni-cserélhető vezérlő modulok. hello vezérlő modulok aktív/passzív módon fog működni. Egy adott időpontban egy tartományvezérlő modul aktív, és biztosít szolgáltatást, hello közben egyéb vezérlő modul passzív. hello passzív vezérlő modul be van kapcsolva, és akkor lép működésbe, ha hello aktív vezérlő modul sikertelen, vagy eltávolítani. Minden egyes tartományvezérlő modulnak hello teljes ház elég kapacitás tooprovide szolgáltatást. Mindkét vezérlő modulok telepített tooensure magas rendelkezésre állású kell lennie.

* Győződjön meg arról, hogy mindkét vezérlő modulok mindig telepítve vannak-e.
* Ha egy tartományvezérlő modul sikertelen, azonnal kérelmet helyettesíti.
* Távolítsa el egy hibás vezérlő modult csak akkor, ha hello helyettesítő, és készen áll a tooinstall azt. A modul eltávolítása a hosszabb ideig hello légmozgás hatással, és ezért a rendszer hello hűtési hello.
* Győződjön meg arról, hogy hello hálózati kapcsolatok tooboth vezérlő modulok azonosak, és hello csatlakoztatott hálózati adapterrel van egy azonos hálózati konfigurációja.
* Ha egy tartományvezérlő modul sikertelen lesz, vagy cserélni kell, győződjön meg arról, hogy hello más vezérlő modul hello sikertelen vezérlő modul cseréje előtt aktív állapotban van. túl nyissa meg, hogy a tartományvezérlő jelenleg aktív, tooverify[azonosítása hello aktív vezérlő az eszközön](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Ne távolítsa el az mindkét vezérlő modulok hello ugyanannyi időt vesz igénybe. Ha egy tartományvezérlő feladatátvétel van folyamatban, nem hello készenléti vezérlő modul leállítása vagy távolítsa el hello váz.
* A vezérlő feladatátvétel után várjon legalább öt perc vagy vezérlő modul eltávolítása előtt.

#### <a name="network-interfaces"></a>Hálózati illesztők

A StorSimple eszköz vezérlő modulok minden rendelkezik négy 1 gigabites és a két 10 gigabites Ethernet hálózati adapterek.

* Győződjön meg arról, hogy hello hálózati kapcsolatok tooboth vezérlő modulok azonosak, és hello hálózati adapterek, hogy hello, tartományvezérlői modul felületek-e a csatlakoztatott toohave egy azonos hálózati konfigurációt.
* Ha lehetséges, központi telepítése a hálózati kapcsolatok különböző kapcsolókhoz tooensure szolgáltatás rendelkezésre állása hello esemény a hálózati eszköz nem teljes.
* Ha csak hello vagy hello utolsó fennmaradó leválasztásával iSCSI-kapcsolatból (az IP-címek hozzárendelve), hello letiltásához először, és majd húzza a hello kábelek ki. Ha hello felület először nincs csatlakoztatva, majd az okoz hello aktív vezérlő toofail toohello passzív vezérlő fölé. Ha hello passzív vezérlő is rendelkezik a megfelelő adapterek választható le, majd mindkét hello tartományvezérlők újraindul többször egy tartományvezérlőn rendezése előtt.
* Legalább két adatok felületek toohello hálózati csatlakozás minden tartományvezérlő modulból.
* Ha engedélyezte a hello két 10 GbE felületek, telepítse különböző kapcsolókhoz jeggyel.
* Ha lehetséges, hogy hello kiszolgálók képesek legyenek tűrni egy hivatkozás, hálózati vagy adapterhibákat kiszolgálók tooensure a MPIO használata.

További információ a hálózat magas rendelkezésre állás és teljesítmény érdekében az eszköz lépjen túl[a StorSimple 8100 eszköz telepítése](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) vagy [a StorSimple 8600 eszköz telepítése](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD és HDD-k

A StorSimple eszközök közé tartozik a tartós állapotú meghajtón (SSD), és a (merevlemezes HDD) meghajtók használatával védett tükrözött tárolóhelyek. Tükrözött tárolóhelyek használata biztosítja, hogy hello eszköz képes tootolerate hello sikertelen volt-e az SSD és HDD legalább.

* Győződjön meg arról, hogy telepítve vannak-e az összes SSD és HDD-modulok.
* Ha az SSD és HDD sikertelen, azonnal kérelmet helyettesíti.
* Egy SSD és HDD meghibásodik, vagy helyettesítő van szükség, ha győződjön meg arról, hogy csak a SSD és HDD helyettesítő igénylő hello eltávolítása.
* Távolítsa el az egynél több SSD és HDD bármikor hello rendszerből időben.
  Egy bizonyos típusú (HDD SSD) 2 vagy több lemezt vagy egymást követő rövid időn belül a rendszer meghibásodása és a lehetséges adatvesztést eredményezhet. Ha ez történik, [forduljon a Microsoft Support](storsimple-8000-contact-microsoft-support.md) segítségért.
* Csere, közben figyelése hello **összetevők megosztott** a hello **hardver állapotának** hello paneljén hello SSD és HDD meghajtókon. Zöld pipa állapot azt jelzi, hogy hello lemezek kifogástalan vagy OK, mivel egy piros felkiáltójel jelzi egy sikertelen SSD és HDD.
* Azt javasoljuk, hogy konfigurálja-e az összes kötet, hogy kell-e a rendszer hiba esetén tooprotect felhőbeli pillanatképeket.

#### <a name="ebod-enclosure"></a>EBOD ház

Eszközmodell StorSimple 8600 egy bővített álló, lemezcsoport a lemezek (EBOD) ház hozzáadása toohello elsődleges ház tartalmazza. Egy EBOD EBOD vezérlőket tartalmaz, és a (merevlemezes HDD) meghajtók használatával védett tükrözött tárolóhelyek. Tükrözött tárolóhelyek használata azt biztosítja, hello eszköz egy vagy több HDD-k képesek tootolerate hello hiba. hello EBOD ház csatlakoztatott toohello elsődleges ház redundáns SAS-kábel segítségével.

* Győződjön meg arról, hogy mindkét EBOD ház vezérlő modulok, SAS-kábel és minden hello merevlemez-meghajtók mindig telepítve van.
* Ha egy EBOD ház vezérlő modul sikertelen, kérelem azonnal helyettesíti.
* Ha egy EBOD ház vezérlő modul sikertelen lesz, győződjön meg arról, hogy hello más vezérlő modul az aktív, mielőtt lecseréli a hello sikertelen modul. túl nyissa meg, hogy a tartományvezérlő jelenleg aktív, tooverify[azonosítása hello aktív vezérlő az eszközön](storsimple-8000-controller-replacement.md#identify-the-active-controller-on-your-device).
* Egy EBOD vezérlő modul csere közben folyamatosan hello állapotának figyelése hello összetevő hello StorSimple Device Manager szolgáltatás elérésével **figyelő** > **hardver állapotának** .
* Ha egy SAS-kábel nem sikerül, vagy helyettesítő (Microsoft Support kell érintett toomake megállapítása) megköveteli, ellenőrizze, hogy csak hello SAS kábel helyettesítő igénylő eltávolítani.
* Nem egyidejűleg eltávolít két SAS-kábel rendszerből hello bármikor időben.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Magas rendelkezésre állású javaslatokat a gazdagép számítógépek

Gondosan tekintse át a ajánlott eljárások tooensure hello magas rendelkezésre állású állomások csatlakoztatott tooyour StorSimple eszköz.

* Konfigurálja a StorSimple [két csomópontos fájlkiszolgáló fürt-konfigurációkkal][1]. Hiba és a redundancia hello fogadó oldalon a hibaérzékeny pontokat eltávolításával hello teljes megoldás magas rendelkezésre állású lesz.
* Használjon folyamatosan rendelkezésre álló (CA) megosztásokat érhető el a Windows Server 2012 (SMB 3.0 esetében) a magas rendelkezésre állású hello tárolóvezérlők feladatátvétele során. További információt a Windows Server 2012 fájlkiszolgáló fürtök és a folyamatosan elérhető fájlmegosztásokat konfigurálásához, tekintse meg a toothis [bemutató videó](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Következő lépések

* [További tudnivalók a StorSimple rendszer korlátok](storsimple-8000-limits.md).
* [Megtudhatja, hogyan toodeploy a StorSimple megoldásban](storsimple-8000-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
