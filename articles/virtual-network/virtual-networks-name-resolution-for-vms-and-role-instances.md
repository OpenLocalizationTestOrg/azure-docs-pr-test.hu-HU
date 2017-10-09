---
title: "a virtuális gépek és a Szerepkörpéldányok aaaResolution"
description: "Megoldási forgatókönyvek nevet Azure IaaS, hibrid megoldások között különböző felhőalapú szolgáltatások, az Active Directory és a saját DNS-kiszolgáló használatával "
services: virtual-network
documentationcenter: na
author: GarethBradshawMSFT
manager: carmonm
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2016
ms.author: telmos
ms.openlocfilehash: 0ec7903cf200c1d04d75601a5b0cefe4268f3dcf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="name-resolution-for-vms-and-role-instances"></a>A virtuális gépek és szerepkörpéldányok névfeloldása
Attól függően, hogyan használhat hibrid megoldások, Azure toohost IaaS és PaaS szükség lehet a virtuális gépek tooallow hello és szerepkörpéldányt beállítani, hogy hozzon létre toocommunicate egymással. Bár ez a kommunikáció IP-címek használatával teheti meg, továbbra is jóval egyszerűbb toouse nevét, amely jól megjegyezhető, és ne módosítsa. 

Ha a szerepkörpéldányok és a virtuális gépek Azure-ban üzemeltetett kell tooresolve tartomány nevek toointernal IP-címek, használhatnak módszerek egyikét:

* [Azure által biztosított névfeloldás](#azure-provided-name-resolution)
* [Névfeloldás a saját DNS-kiszolgáló segítségével](#name-resolution-using-your-own-dns-server) (ami továbbít lekérdezések toohello Azure által biztosított DNS-kiszolgálók) 

névfeloldás használata hello típusa attól függ, hogy a virtuális gépek és a szerepkörpéldányok kell toocommunicate egymással.

**hello alábbi táblázat szemlélteti forgatókönyvek és a megfelelő név feloldása megoldások:**

| **A forgatókönyv** | **Megoldás** | **Utótag** |
| --- | --- | --- |
| Névfeloldás szerepkörpéldányokat vagy a virtuális gépek között található hello ugyanaz a felhőalapú szolgáltatás, vagy a virtuális hálózat |[Azure által biztosított névfeloldás](#azure-provided-name-resolution) |állomásnév vagy teljes Tartományneve |
| Névfeloldás szerepkörpéldányokat vagy különböző virtuális hálózatokon lévő virtuális gépek között |Ügyfél által felügyelt DNS-kiszolgálók között a névfeloldás az Azure-ban (DNS-proxy) vnetek lekérdezések továbbítása.  Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-using-your-own-dns-server) |Kizárólag az FQDN esetében |
| A helyi számítógép és a szolgáltatás neve a szerepkörpéldányok vagy az Azure virtuális gépek feloldását. |Ügyfél által felügyelt DNS-kiszolgálók (például a helyi tartományvezérlő, helyi írásvédett tartományvezérlő vagy másodlagos DNS zónaletöltés használatával szinkronizálva).  Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-using-your-own-dns-server) |Kizárólag az FQDN esetében |
| A helyszíni számítógépek Azure állomásnevének feloldása |Továbbítási kérelmek tooa ügyfél által felügyelt DNS-proxy kiszolgáló hello megfelelő virtuális, hello proxykiszolgáló továbbítja lekérdezések tooAzure a feloldásához. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-using-your-own-dns-server) |Kizárólag az FQDN esetében |
| Névkeresési DNS, a belső IP-címek |[Névfeloldás a saját DNS-kiszolgáló használatával](#name-resolution-using-your-own-dns-server) |n/a |
| Névfeloldás virtuális gépekre vagy szerepkörpéldányokra található különböző felhőszolgáltatások, nem a virtuális hálózat között |Nem alkalmazható. Virtuális gépek és a szerepkörpéldányok különböző felhőszolgáltatások közötti kapcsolat nem támogatja a virtuális hálózatokon kívül. |n/a |

## <a name="azure-provided-name-resolution"></a>Azure által biztosított névfeloldás
Nyilvános DNS-nevek feloldása, valamint Azure belső névfeloldást biztosít a virtuális gépek és a szerepkörpéldányok belüli hello ugyanazon virtuális hálózat vagy a felhőalapú szolgáltatás.  Virtuális gépek/példány egy felhőalapú szolgáltatás hello azonos DNS utótag (így hello állomásnév önmagában is elegendő), de más DNS-utótag a klasszikus virtuális hálózatok különböző felhőszolgáltatások rendelkezik, így hello FQDN szükséges tooresolve nevek különböző felhőszolgáltatások közötti megosztása.  A virtuális hálózatokon, hello Resource Manager üzembe helyezési modellel hello DNS-utótag konzisztens hello virtuális hálózaton (így nincs szükség a hello FQDN), és DNS-nevek tooboth hálózati adapterek és virtuális gépek rendelhető. Bár az Azure által biztosított névfeloldás kell konfigurálni, nincs hello megfelelő választás az összes központi telepítési forgatókönyv, mint a fenti hello táblán.

> [!NOTE]
> Webes és feldolgozói szerepkörök hello esetben hello belső IP-címek a szerepkörpéldányok alapuló hello szerepkör nevét és példányszámát hello Azure szolgáltatásfelügyelet REST API használatával is elérhető. További információkért lásd: [Service Management REST API-referencia](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features-and-considerations"></a>Szolgáltatások és szempontok
**Szolgáltatások:**

* Könnyű használat: nincs szükség konfigurációra az Azure által biztosított névfeloldás rendelés toouse.
* hello Azure által biztosított névfeloldási szolgáltatás magas rendelkezésre állású, akkor hello mentése toocreate kell és fürtöket a saját DNS-kiszolgálók kezelése.
* A saját DNS-kiszolgálók tooresolve együtt is használható a helyszíni, mind az Azure állomásnevek.
* Névfeloldás kerül a szerepkör példányok/virtuális gépek közötti hello belül ugyanaz a felhőalapú szolgáltatás teljes tartománynévhez szükség nélkül.
* Virtuális gépek hello Resource Manager üzembe helyezési modelljével hello FQDN nélkül használó virtuális hálózatok közötti névfeloldás valósul meg. Virtuális hálózatok hello klasszikus üzembe helyezési modellel hello teljes Tartománynevet igényelnek, különböző felhőszolgáltatások a nevek feloldásakor. 
* Használhatja a központi telepítések jellemző állomásnevek ahelyett, hogy működik-e automatikusan létrehozott névvel.

**Szempontok:**

* nem lehet módosítani a hello Azure által létrehozott DNS-utótagot.
* Nem lehet manuálisan regisztrálni a saját rögzíti.
* WINS- és NetBIOS nem támogatottak. (A Windows Intézőben a virtuális gépek nem látható.)
* Állomásnevek kell lennie a DNS-kompatibilis (kell használniuk, csak 0-9, a – z és a "-", és nem kezdődhet vagy végződhet egy "-". Című rész RFC 3696 2.)
* DNS-lekérdezés forgalom az egyes virtuális gépek folyamatban van. Ez nem befolyásolja a legtöbb alkalmazást.  Ha kérelem szabályozása, győződjön meg arról, hogy engedélyezve van-e az ügyféloldali gyorsítótárazás.  További részletekért lásd: [hello Azure által biztosított névfeloldás a legtöbb kezdeti](#Getting-the-most-from-Azure-provided-name-resolution).
* Csak virtuális gépek hello első 180 felhőszolgáltatások minden virtuális hálózathoz, a klasszikus üzembe helyezési modellben van regisztrálva. Ez nem vonatkozik a Resource Manager üzembe helyezési modellel toovirtual hálózatok.

### <a name="getting-hello-most-from-azure-provided-name-resolution"></a>Hello Azure által biztosított névfeloldás a legtöbb beolvasása
**Ügyféloldali gyorsítótárazás:**

Nem minden DNS-lekérdezés hello hálózaton keresztül küldött toobe kell.  Ügyféloldali gyorsítótárazás segít a késés csökkentésére, és növelheti rugalmasság toonetwork blips feloldása ismétlődő DNS-lekérdezések a helyi gyorsítótárból.  DNS-rekordokat tartalmazza a Time-Live (TTL) hello toostore hello gyorsítótárrekord, amíg nélkül érintő rekord frissesség, így a legtöbb esetben a megfelelő ügyféloldali gyorsítótárazás így.

hello alapértelmezett Windows DNS-ügyfél a DNS-gyorsítótár beépített rendelkezik.  Egyes Linux disztribúciókkal kihagyása alapértelmezés szerint gyorsítótárazás, javasoljuk, egy fel tooeach Linux virtuális gép (miután ellenőrizte, hogy a helyi gyorsítótárba még nem).

Számos különböző DNS gyorsítótárazási csomagok érhető el, például dnsmasq, az alábbiakban hello lépéseket tooinstall dnsmasq a leggyakrabban használt disztribúciókkal hello:

* **Ubuntu (használ resolvconf)**:
  * csak hello dnsmasq telepítőcsomag ("sudo apt-get-telepítés dnsmasq").
* **SUSE (használ netconf)**:
  * hello dnsmasq csomag ("sudo zypper telepítés dnsmasq") telepítése 
  * Engedélyezze az hello dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service") 
  * Indítsa el hello dnsmasq szolgáltatást ("systemctl start dnsmasq.service") 
  * Szerkesztés "/ etc/sysconfig/hálózati/config", és módosítsa a NETCONFIG_DNS_FORWARDER = "" túl "dnsmasq"
  * resolv.conf ("netconfig update") tooset hello gyorsítótár, a helyi DNS-feloldási hello frissítése
* **OpenLogic (használja a NetworkManager)**:
  * hello dnsmasq csomag ("sudo yum telepítés dnsmasq") telepítése
  * Engedélyezze az hello dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service")
  * Indítsa el hello dnsmasq szolgáltatást ("systemctl start dnsmasq.service")
  * Adja hozzá a "név-tartománykiszolgálók 127.0.0.1; illesztenie" too"/etc/dhclient-eth0.conf"
  * Indítsa újra a hello hálózati szolgáltatás ("szolgáltatás hálózati restart") tooset hello gyorsítótár, a helyi DNS-feloldási hello

> [!NOTE]
> hello "dnsmasq" csomag Ez csak egy hello Linux számos DNS gyorsítótárak érhető el.  Annak használata előtt ellenőrizze az adott igényeknek megfelelően való alkalmasságát, és nincs más gyorsítótár telepítve van.
> 
> 

**Ügyféloldali újrapróbálkozások:**

DNS elsősorban UDP protokoll.  Mivel hello UDP protokoll üzenetkézbesítést nem garantálja, újrapróbálkozási logika hello DNS protokoll kezelése.  Minden DNS-ügyfél (operációs rendszer) hello creators preferencia függően különböző újrapróbálkozási logika is lehetnek:

* Windows operációs rendszerek újrapróbálkozás után 1 második és majd újra egy másik 2, 4, és egy másik 4 másodperc. 
* hello alapértelmezett Linux telepítő ismételt próbálkozás után 5 másodperc.  Az 5-ször időközönként 1 második ez tooretry toochange ajánlott.  

toocheck jelenlegi beállítások a Linux virtuális gép, "cat /etc/resolv.conf" hello, és tekintse meg hello "beállítások" sor, pl.:

    options timeout:1 attempts:5

hello resolv.conf fájl van általában automatikusan létrehozott, és nem szerkeszthető.  distro változhat hello hello "beállítások" sor hozzáadásának lépéseit:

* **Ubuntu** (használja a resolvconf):
  * Adja hozzá a hello beállítások sor too'/etc/resolveconf/resolv.conf.d/head " 
  * Futtassa a 'resolvconf -u' tooupdate
* **SUSE** (használja a netconf):
  * Adja hozzá a 'timeout:1 kísérletek: 5' toohello NETCONFIG_DNS_RESOLVER_OPTIONS = "" a "/ etc/sysconfig/hálózati/config" paraméter 
  * Futtassa a "netconfig frissítés" tooupdate
* **OpenLogic** (használja a NetworkManager):
  * Adja hozzá a too'/etc/NetworkManager/dispatcher.d/11-dhclient "echo" lehetőségek timeout:1 kísérletek: 5"" " 
  * Futtassa a "service hálózati restart" tooupdate

## <a name="name-resolution-using-your-own-dns-server"></a>Névfeloldás a saját DNS-kiszolgáló használatával
Számos helyzetben, ahol a névfeloldási szükségleteit lehet, hogy lépjen túl hello által nyújtott szolgáltatásokat Azure, például ha használja az Active Directory-tartományok, vagy ha DNS-feloldás virtuális hálózatokról (vnetekről) között.  toocover forgatókönyvekben Azure hello lehetőséget nyújt az Ön toouse saját DNS-kiszolgálókat.  

A virtuális hálózaton belül DNS-kiszolgálók DNS lekérdezések tooAzure rekurzív feloldókat továbbíthatja a tooresolve állomásnevek adott virtuális hálózaton belül.  Például egy tartományvezérlőn (DC) Azure-beli válaszolni a tartományok tooDNS lekérdezések és minden más lekérdezések tooAzure továbbítja.  Ez lehetővé teszi, hogy a virtuális gépek toosee a helyszíni erőforrások (keresztül hello DC) és az Azure által biztosított állomásnevek (keresztül hello továbbító).  Hozzáférés tooAzure rekurzív feloldókat hello virtuális IP-cím 168.63.129.16 keresztül valósul meg.

DNS-továbbítást is lehetővé teszi virtuális közötti virtuális hálózat DNS-feloldás és a helyszíni gépeket tooresolve Azure által biztosított állomásnevek.  A rendezés tooresolve egy virtuális gép állomásnevét, hello DNS-kiszolgáló virtuális gép kell lennie, hello azonos virtuális hálózati és a beállított tooforward állomásnév lekérdezések tooAzure lehet.  Módon hello DNS-utótagja eltér minden egyes virtuális, használhatja a feltételes továbbítás toosend DNS-lekérdezések toohello szabályok javítsa ki a virtuális hálózat a feloldásához.  a következő kép hello két virtuális hálózatokat és egy virtuális közötti virtuális hálózat DNS-feloldás ezzel a módszerrel történt a helyi hálózati jeleníti meg.  Egy példa DNS-továbbító érhető el hello [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) és [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Régión vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Ha Azure által biztosított névfeloldás, olyan belső DNS-utótagot használ (*. internal.cloudapp.net) van megadott tooeach VM DHCP használatával.  Ez lehetővé teszi, hogy állomásnév-feloldási rekordok olyan hello internal.cloudapp.net zónában hello állomásnév szerint.  A saját nevet névfeloldási megoldás használata esetén hello IDN utótag nincs megadott tooVMs, mert azt más DNS-architektúrák (például a tartományhoz csatlakoztatott forgatókönyvek) ütköző.  Ehelyett azt adja meg a nem működő helyőrző (reddog.microsoft.com).  

Szükség esetén – hello belső DNS-utótag meghatározható PowerShell vagy hello API használatával:

* Virtuális hálózatok a Resource Manager üzembe helyezési modellel, hello utótag megtalálható hello [hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx) erőforrás vagy hello segítségével [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) parancsmag.    
* A klasszikus üzembe helyezési modellek hello utótag megtalálható hello [telepítési API beszerzése](https://msdn.microsoft.com/library/azure/ee460804.aspx) hívás vagy hello segítségével [Get-AzureVM-Debug](https://msdn.microsoft.com/library/azure/dn495236.aspx) parancsmag.

Ha tooAzure nem felelnek meg az igényeinek lekérdezések továbbítása, akkor a saját DNS-megoldást tooprovide.  A DNS-megoldást kell:

* Segítségével például adja meg a megfelelő állomásnév-feloldási [DDNS](virtual-networks-name-resolution-ddns.md).  Ne feledje, ha túl korán DDNS esetleg toodisable DNS rekord kitakarítási nagyon hosszú Azure DHCP-bérletek és kitakarítási távolíthatja el a DNS használatával rögzíti. 
* Adja meg a megfelelő feloldás tooallow rekurziót külső tartománynevek.
* Kell érhető el (TCP és UDP 53-as porton) hello ügyfelek szolgál, és képes tooaccess kell hello internet.
* Szemben hello hozzáférést biztosítani kell az internet, külső ügynökök által jelentett toomitigate fenyegetések.

> [!NOTE]
> A legjobb teljesítmény érdekében Azure virtuális gépek használata a DNS-kiszolgálóként, IPv6 le kell tiltani és egy [példányszintű nyilvános IP-cím](virtual-networks-instance-level-public-ip.md) tooeach DNS-kiszolgáló virtuális gép hozzá kell rendelni.  Ha úgy dönt, a Windows Server toouse a DNS-kiszolgálóként [Ez a cikk](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) biztosít további Teljesítményelemzés és optimalizálási lehetőségeit.
> 
> 

### <a name="specifying-dns-servers"></a>DNS-kiszolgálók megadása
A saját DNS-kiszolgálók, Azure biztosít hello képességét toospecify virtuális hálózatonként több DNS-kiszolgáló vagy egy hálózati csatoló (Resource Manager), vagy a felhőalapú szolgáltatás (klasszikus).  Egy felhőalapú szolgáltatás/hálózati illesztőhöz megadott DNS-kiszolgálók elsőbbséget élveznek hello virtuális hálózathoz megadott beolvasása.

> [!NOTE]
> Hálózati kapcsolat tulajdonságai, például a DNS-kiszolgáló IP-címek, nem lehet szerkeszteni közvetlenül a Windows virtuális gépeken belül, akkor előfordulhat, hogy első újramegfeleltetés során szolgáltatás javítandó hello virtuális hálózati adapterek lekérdezi cseréje. 
> 
> 

Hello Resource Manager üzembe helyezési modellben használatakor DNS-kiszolgálók adhatók hello portálon API/sablonok ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) vagy a PowerShell használatával ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Amikor hello klasszikus üzembe helyezési modellel, DNS-kiszolgálók a virtuális hálózati hello adható hello Portal vagy [hello *hálózati konfiguráció* fájl](https://msdn.microsoft.com/library/azure/jj157100).  A felhőszolgáltatások hello DNS-kiszolgálókon keresztül megadott [hello *szolgáltatáskonfiguráció* fájl](https://msdn.microsoft.com/library/azure/ee758710) vagy a PowerShellben ([New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [!NOTE]
> Ha már telepítve van egy virtuális hálózat vagy virtuális gép hello DNS-beállítások módosításához szüksége toorestart minden érintett VM hello módosítások tootake hatást.
> 
> 

## <a name="next-steps"></a>Következő lépések
Erőforrás-kezelő telepítési modell:

* [A virtuális hálózat létrehozása vagy módosítása](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Létrehozni vagy frissíteni a hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [Új-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
* [Új AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

Klasszikus telepítési modell:

* [Az Azure szolgáltatás konfigurációs séma](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtuális hálózati konfiguráció séma](https://msdn.microsoft.com/library/azure/jj157100)
* [Virtuális hálózat konfigurálása a hálózati konfigurációs fájl segítségével](virtual-networks-using-network-configuration-file.md) 

