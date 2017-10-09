---
title: "Dinamikus DNS tooregister állomásnevek aaaUsing"
description: "Ez a lap részleteket nyújt hogyan tooset be dinamikus DNS tooregister állomásnevek a saját DNS-kiszolgálókat."
services: dns
documentationcenter: na
author: GarethBradshawMSFT
manager: timlt
editor: 
ms.assetid: c315961a-fa33-45cf-82b9-4551e70d32dd
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2017
ms.author: garbrad
ms.openlocfilehash: 8d4b44265714e6976f26bfb3446e8101aa70996a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-dynamic-dns-tooregister-hostnames-in-your-own-dns-server"></a>Dinamikus DNS tooregister állomásnevek a saját DNS-kiszolgáló használatával
[Azure biztosít névfeloldás](virtual-networks-name-resolution-for-vms-and-role-instances.md) a virtuális gépek (VM) és a szerepkörpéldányok. Amikor a névfeloldás kell túlmutató Azure által biztosított naplókon, megadhatja a saját DNS-kiszolgálók. Ez biztosítja az energiagazdálkodási tootailor a DNS-megoldás toosuit hello saját igényeinek megfelelően. Például szükség lehet tooaccess a helyszíni erőforrások az Active Directory tartományvezérlőjéhez keresztül.

Az egyéni DNS-kiszolgálók működtetnek továbbíthatja Azure virtuális gépeken, ha az állomásnév lekérdezi hello azonos virtuális hálózaton tooAzure tooresolve hostnames. Ha nem szeretné toouse Ez az útvonal, a virtuális gép állomásnevek regisztrálhatja a dinamikus DNS használatával DNS-kiszolgáló.  Azure nincs hello lehetőséget (pl. a hitelesítő adatok) toodirectly rekordokat létrehozni a DNS-kiszolgálók, így alternatív megoldások gyakran van szükség. Az alábbiakban néhány gyakori helyzetek lehetőségeket.

## <a name="windows-clients"></a>Windows-ügyfelek
A nem tartományhoz csatlakoztatott Windows-ügyfelek nem biztonságos dinamikus DNS (DDNS) frissítések történt kísérlet, ha indulnak, vagy ha megváltoztatja az IP-címet. hello DNS-neve: a hello állomásnév plus hello elsődleges DNS-utótagot. Azure hello elsődleges DNS-utótag üresen hagyja, de ez a virtuális gép, hello hello segítségével beállíthatja [felhasználói felület](https://technet.microsoft.com/library/cc794784.aspx) vagy [automatizálás segítségével](https://social.technet.microsoft.com/forums/windowsserver/3720415a-6a9a-4bca-aa2a-6df58a1a47d7/change-primary-dns-suffix).

A tartományhoz csatlakoztatott Windows-ügyfelek regisztrálja az IP-címek hello tartományvezérlő biztonságos dinamikus DNS használatával. hello tartományhoz való csatlakozást folyamat hello ügyfél hello elsődleges DNS-utótag be és hoz létre, és hello megbízhatósági kapcsolatot tart fenn.

## <a name="linux-clients"></a>Linux-ügyfelek
Linux-ügyfelek általában nincs regisztrálása maguk hello DNS-kiszolgáló indításakor, akkor azt feltételezik, hogy hello DHCP-kiszolgáló minderre. Azure DHCP-kiszolgálók nem rendelkeznek hello lehetősége vagy a hitelesítő adatok tooregister rekordokat a DNS-kiszolgáló.  Használhatja a nevű eszköz *nsupdate*, amely hello Bind csomagban található, toosend dinamikus DNS-frissítések. Mivel a dinamikus DNS protokoll hello szabványosított, használhatja *nsupdate* még ha nem a Bind hello DNS-kiszolgálón.

DHCP-ügyfél toocreate hello által biztosított hello hurkok használja, és hello gazdagépnév-bejegyzés a hello DNS-kiszolgáló karbantartása. Hello DHCP ciklusban hello ügyfél hello parancsfájlok végrehajtása */etc/dhcp/dhclient-exit-hooks.d/*. Ez lehet tooregister hello új IP-címet használt *nsupdate*. Példa:

        #!/bin/sh
        requireddomain=mydomain.local

        # only execute on hello primary nic
        if [ "$interface" != "eth0" ]
        then
            return
        fi

        # when we have a new IP, perform nsupdate
        if [ "$reason" = BOUND ] || [ "$reason" = RENEW ] ||
           [ "$reason" = REBIND ] || [ "$reason" = REBOOT ]
        then
            host=`hostname`
            nsupdatecmds=/var/tmp/nsupdatecmds
              echo "update delete $host.$requireddomain a" > $nsupdatecmds
              echo "update add $host.$requireddomain 3600 a $new_ip_address" >> $nsupdatecmds
              echo "send" >> $nsupdatecmds

              nsupdate $nsupdatecmds
        fi

        
        

Is használhatja a hello *nsupdate* parancs tooperform biztonságos dinamikus DNS-frissítések. Például egy kötés DNS-kiszolgálót használ, egy nyilvános-titkos kulcsból álló kulcspárt esetén [generált](http://linux.yyz.us/nsupdate/).  hello a DNS-kiszolgáló [konfigurált](http://linux.yyz.us/dns/ddns-server.html) hello úgy, hogy ellenőrizni tudja a hello aláírás hello kérésre hello kulcs nyilvános része. Hello kell használnia *-k* beállítás tooprovide hello-kulcspár túl*nsupdate* ahhoz, hogy hello dinamikus DNS-frissítési kérelem toobe aláírva.

A Windows DNS-kiszolgáló használata, ha használható a Kerberos-hitelesítés hello *-g* paraméterének *nsupdate* (nem érhető el a hello Windows-verzión *nsupdate* ). toodo ehhez, *kinit* tooload hello hitelesítő adatait (pl. egy [keytab fájl](http://www.itadmintools.com/2011/07/creating-kerberos-keytab-files.html)). Majd *nsupdate -g* felveszi hello gyorsítótár hello hitelesítő adatokat.

Ha szükséges, a DNS keresési utótag tooyour virtuális gépek is hozzáadhat. hello DNS-utótag megadott hello */etc/resolv.conf* fájlt. A legtöbb Linux disztribúciókkal automatikusan kezeli a hello tartalmát a fájl, ezért általában nem szerkeszthető. Azonban a beállítás felülbírálható hello utótag hello DHCP-ügyfél használatával *felülír* parancsot. toodo Ez a */etc/dhcp/dhclient.conf*, adja hozzá:

        supersede domain-name <required-dns-suffix>;

