---
title: "aaaDNS névfeloldási lehetőségek a Linux virtuális gépek Azure-ban"
description: "Név feloldása forgatókönyvek a Linux virtuális gépek Azure IaaS, beleértve a megadott DNS-szolgáltatásokra, hibrid külső DNS- és Bring Your Own DNS-kiszolgáló."
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: timlt
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: 7df2320b6f6b42b479bf4070ea60fe5770a78a41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>A DNS-névfeloldás beállítások a Linux virtuális gépek Azure-ban
Azure alapértelmezés szerint a DNS-névfeloldás biztosít egy virtuális hálózaton lévő összes virtuális gépekhez. Valósíthatja meg a saját DNS-név feloldása megoldást a saját DNS-szolgáltatások konfigurálása a virtuális gépeken, amely az Azure futtatja. hello következő esetekben kell segítségével eldöntheti, hello eszközre, amely a megfelelőt.

* [Azure által biztosított névfeloldás](#azure-provided-name-resolution)
* [Névfeloldás a saját DNS-kiszolgáló használatával](#name-resolution-using-your-own-dns-server)

névfeloldás, amelyekkel hello típusa attól függ, hogy a virtuális gépek és a szerepkörpéldányok kell toocommunicate egymással.

hello alábbi táblázat szemlélteti forgatókönyvek és a megfelelő név feloldása megoldások:

| **A forgatókönyv** | **Megoldás** | **Utótag** |
| --- | --- | --- |
| Névfeloldás a szerepkörpéldányok vagy hello lévő virtuális gépek közötti azonos virtuális hálózatban |[Azure által biztosított névfeloldás](#azure-provided-name-resolution) |állomásnév vagy teljesen minősített tartománynevét (FQDN) |
| Névfeloldás szerepkörpéldányokat vagy különböző virtuális hálózatokon lévő virtuális gépek között |Ügyfél által felügyelt DNS-kiszolgálók, amelyek az Azure-ban (DNS-proxy) megoldás virtuális hálózatok közötti lekérdezéseket. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-using-your-own-dns-server). |Kizárólag az FQDN esetében |
| A helyszíni számítógépek és a szerepkörpéldányok vagy Azure virtuális gépek szolgáltatásnevek feloldását. |Ügyfél által felügyelt DNS-kiszolgálók (például a helyi tartományvezérlő, helyi írásvédett tartományvezérlő vagy másodlagos DNS zónaletöltés használatával szinkronizálva). Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-using-your-own-dns-server). |Kizárólag az FQDN esetében |
| A helyszíni számítógépek Azure állomásnevének feloldása |Lekérdezések tooa ügyfél által felügyelt DNS-proxy kiszolgáló hello megfelelő virtuális hálózat továbbítja. hello proxykiszolgáló lekérdezések tooAzure a feloldásához továbbítja. Lásd: [névfeloldáshoz a saját DNS-kiszolgáló](#name-resolution-using-your-own-dns-server). |Kizárólag az FQDN esetében |
| Névkeresési DNS, a belső IP-címek |[Névfeloldás a saját DNS-kiszolgáló használatával](#name-resolution-using-your-own-dns-server) |n/a |

## <a name="name-resolution-that-azure-provides"></a>Azure által biztosított névfeloldás
Nyilvános DNS-nevek feloldása, valamint Azure belső névfeloldást biztosít a virtuális gépek és a szerepkör-példányok hello ugyanazt a virtuális hálózatot. A virtuális hálózatok az Azure Resource Manageren alapuló hello DNS-utótag összhangban a virtuális hálózaton hello; nincs szükség a hello teljes Tartománynevét. DNS-nevek tooboth hálózati adapterek (NIC) és a virtuális gépek hozzárendelhetők legyenek. Bár az Azure által biztosított hello névfeloldás kell konfigurálni, nincs hello megfelelő választás az összes központi telepítési forgatókönyv hello előző táblázatban látható módon.

### <a name="features-and-considerations"></a>Szolgáltatások és szempontok
**Szolgáltatások:**

* Beállításokra nincs is szükség toouse névfeloldás Azure által biztosított.
* Azure által biztosított hello névfeloldási szolgáltatás magas rendelkezésre állású. Nem kell toocreate, és kezelheti a fürtöket a saját DNS-kiszolgálók.
* Azure által biztosított hello névfeloldási szolgáltatás használható együtt a saját DNS-kiszolgálók tooresolve helyszíni, mind az Azure állomásnevek.
* Virtuális gépek anélkül hello FQDN virtuális hálózatok közötti névfeloldás valósul meg.
* Használhatja a központi telepítések jellemző állomásnevek ahelyett, hogy működik-e automatikusan létrehozott névvel.

**Szempontok:**

* nem lehet módosítani a hello DNS-utótagot, amely Azure-hoz.
* Nem lehet manuálisan regisztrálni a saját rögzíti.
* WINS- és NetBIOS nem támogatottak.
* Állomásnevek kell lennie a DNS-kompatibilis.
    Neveket kell használnia csak 0-9, a – z, és a "-", és nem kezdődhet vagy végződhet egy "-". Című rész RFC 3696 2.
* DNS-lekérdezés forgalom az egyes virtuális gépek folyamatban van. Sávszélesség-szabályozás nem befolyásolja a legtöbb alkalmazást.  Ha kérelem szabályozása, győződjön meg arról, hogy engedélyezve van-e az ügyféloldali gyorsítótárazás.  További információkért lásd: [hello Azure által biztosított névfeloldás a legtöbb kezdeti](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-hello-most-from-name-resolution-that-azure-provides"></a>Hello Azure által biztosított névfeloldás a legtöbb beolvasása
**Ügyféloldali gyorsítótárazás:**

Bizonyos DNS-lekérdezéseket a rendszer nem küldi hello hálózaton keresztül. Ügyféloldali gyorsítótárazás segít a késés csökkentésére, és növelheti rugalmasság toonetwork inkonzisztenciák feloldása ismétlődő DNS-lekérdezések a helyi gyorsítótárból. DNS-rekordokat tartalmaz egy Time-Live (TTL), amely lehetővé teszi hello toostore hello gyorsítótárrekord, amíg a rekord frissesség befolyásolása nélkül. Ennek eredményeképpen ügyféloldali gyorsítótárazás alkalmas a legtöbb esetben.

Egyes Linux terjesztésekről gyorsítótárazás alapértelmezés szerint nem tartalmaznak. Azt javasoljuk, hogy a gyorsítótár tooeach Linux virtuális gép hozzáadása után ellenőrizheti, hogy a helyi gyorsítótárba még nem.

Számos különböző DNS-, csomagok, például a dnsmasq, gyorsítótárazás érhetők el. A leggyakoribb terjesztéseket hello az alábbiakban hello lépéseket tooinstall dnsmasq:

**Ubuntu (használ resolvconf)**
  * Telepítse a hello dnsmasq csomagot ("sudo apt-get-telepítés dnsmasq").

**SUSE (használ netconf)**:
1. Telepítse a hello dnsmasq csomagot ("sudo zypper telepítés dnsmasq").
2. Engedélyezze a hello dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service").
3. Indítsa el a hello dnsmasq szolgáltatást ("systemctl start dnsmasq.service").
4. Szerkesztés "/ etc/sysconfig/hálózati/config", és módosítsa a NETCONFIG_DNS_FORWARDER = "" túl "dnsmasq".
5. Frissíti, a helyi DNS-feloldási hello resolv.conf ("netconfig update") tooset hello gyorsítótárát.

**Rosszindulatú Wave szoftver (korábbi nevén OpenLogic; NetworkManager használja) – centOS**
1. Telepítse a hello dnsmasq csomagot ("sudo yum telepítés dnsmasq").
2. Engedélyezze a hello dnsmasq szolgáltatást ("systemctl engedélyezése dnsmasq.service").
3. Indítsa el a hello dnsmasq szolgáltatást ("systemctl start dnsmasq.service").
4. Adja hozzá a "név-tartománykiszolgálók 127.0.0.1; illesztenie" too"/etc/dhclient-eth0.conf".
5. Indítsa újra a hello hálózati szolgáltatás ("szolgáltatás hálózati restart") tooset hello gyorsítótár, a helyi DNS-feloldási hello

> [!NOTE]
> : hello "dnsmasq" csomag Ez csak egy hello számos DNS gyorsítótárazza a Linux elérhető. Használatba vétel előtt ellenőrizze az igényeinek alkalmas, és nincs más gyorsítótár telepített.
>
>

**Ügyféloldali újrapróbálkozások**

DNS elsősorban UDP protokoll. Hello UDP protokoll nem biztosítására üzenetkézbesítést, mert a DNS protokoll hello újrapróbálkozási logika kezeli. Minden DNS-ügyfél (operációs rendszer) hello létrehozásához használt beállítások függően különböző újrapróbálkozási logika is lehetnek:

* Windows operációs rendszerek újrapróbálkozás után egy második és majd újra egy másik kettő, négy, és egy másik négy másodperc.
* hello alapértelmezett Linux telepítő újrapróbálkozások öt másodperc múlva.  Meg kell változtatni a tooretry ötször egy másodperces időközönként.  

toocheck hello Linux virtuális gépen, "/etc/resolv.conf cat", aktuális beállításokat, és tekintse meg hello "beállítások" sor, például:

    options timeout:1 attempts:5

hello resolv.conf fájl jön létre automatikusan, és nem szerkeszthető. hello hello "beállítások" sor eltérő terjesztési által hozzáadott lépéseit:

**Ubuntu** (resolvconf használ)
1. Adja hozzá a hello beállítások sor too'/etc/resolveconf/resolv.conf.d/head ".
2. Futtassa a 'resolvconf -u' tooupdate.

**SUSE** (netconf használ)
1. Adja hozzá a 'timeout:1 kísérletek: 5' toohello NETCONFIG_DNS_RESOLVER_OPTIONS = "" a "/ etc/sysconfig/hálózati/config" paraméter.
2. Futtassa a "netconfig frissítés" tooupdate.

**Az engedélyezetlen Wave szoftver (korábbi nevén OpenLogic) – centOS** (NetworkManager használ)
1. Adja hozzá a too'/etc/NetworkManager/dispatcher.d/11-dhclient "echo" lehetőségek timeout:1 kísérletek: 5"" ".
2. Futtassa a "service hálózati restart" tooupdate.

## <a name="name-resolution-using-your-own-dns-server"></a>Névfeloldás a saját DNS-kiszolgáló használatával
A névfeloldási szükségleteit hello szolgáltatások, Azure által biztosított, előfordulhat, hogy túlmutató. Például a DNS-feloldás virtuális hálózatok közötti lehet szükség. toocover ebben az esetben a saját DNS-kiszolgálókat is használhat.  

Egy virtuális hálózat is tud előrehaladó DNS-lekérdezések toorecursive feloldókat Azure tooresolve állomásnevének lévő DNS-kiszolgálók hello ugyanazt a virtuális hálózatot. Például az Azure-ban futó DNS-kiszolgáló válaszolhassanak tooDNS lekérdezések a saját DNS-zónát fájlokat, és minden más lekérdezések tooAzure továbbítja. Ez a funkció lehetővé teszi a virtuális gépek toosee a zóna fájlok és az Azure által biztosított (keresztül hello továbbító) állomásnevek mindkét a bejegyzést. Hozzáférés toohello rekurzív feloldókat Azure hello virtuális IP-cím 168.63.129.16 keresztül valósul meg.

DNS-továbbítást is lehetővé teszi, hogy a DNS-feloldás virtuális hálózatok között, és lehetővé teszi, hogy a helyszíni gépeket tooresolve állomásnevek Azure által biztosított. tooresolve hello DNS-kiszolgáló virtuális gép egy virtuális gép állomásnevét, kell lennie a hello azonos virtuális hálózatban, és a beállított tooforward állomásnév lekérdezések tooAzure kell. Mivel a hello DNS-utótagja eltér az egyes virtuális hálózati, feltételes továbbítás segítségével toosend DNS-lekérdezések toohello szabályok javítsa ki a virtuális hálózat a feloldásához. hello alábbi ábrán két virtuális hálózatot és egy DNS-feloldás ezt a módszert a virtuális hálózatok közötti történt a helyi hálózaton:

![Virtuális hálózatok közötti DNS-feloldás](./media/azure-dns/inter-vnet-dns.png)

Azure által biztosított névfeloldás használatakor hello belső DNS-utótag tooeach virtuális gép által biztosított DHCP használatával. A saját nevet névfeloldási megoldás használata esetén ez utótag nincs megadott toovirtual gépek mert más DNS-architektúrák ütköző hello utótag. toorefer toomachines által a virtuális gépek teljes Tartománynevét vagy tooconfigure hello utótagot, a PowerShell vagy hello API toodetermine hello utótag használhatja:

* Az Azure Resource Manager által kezelt virtuális hálózatok hello utótag megtalálható hello [hálózati kártya](https://msdn.microsoft.com/library/azure/mt163668.aspx) erőforrás. Hello is futtathatja `azure network public-ip show <resource group> <pip name>` parancs a nyilvános IP-cím, amely tartalmazza a toodisplay hello részleteit hello FQDN-jét hello hálózati adaptert.

Ha tooAzure nem felelnek meg az igényeinek lekérdezések továbbítása, akkor tooprovide a saját DNS-megoldást.  A DNS-megoldást kell megfelelnie:

* Segítségével például adja meg a megfelelő állomásnév-feloldási [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). Ha DDNS használ, szükség lehet toodisable DNS-rekord kitakarítási. DHCP-bérletek Azure nagyon hosszú, és kitakarítási távolíthatja el a DNS-rekordok idő előtt.
* Adja meg a megfelelő feloldás tooallow rekurziót külső tartománynevek.
* Elérhető (TCP és UDP 53-as porton) hello ügyfelek működik és képes tooaccess hello Internet.
* A külső ügynökök által jelentett hello Internet toomitigate fenyegetésekkel szembeni hatékony hozzáféréssel szemben védve legyenek.

> [!NOTE]
> A legjobb teljesítmény érdekében a virtuális gépek Azure DNS-kiszolgálókat használ, amikor ki IPv6 letiltása, és rendelje hozzá egy [példányszintű nyilvános IP-cím](../../virtual-network/virtual-networks-instance-level-public-ip.md) tooeach DNS-kiszolgáló virtuális gép.  
>
>
