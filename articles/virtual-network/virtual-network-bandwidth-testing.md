---
title: "aaaTesting Azure virtuális hálózat átbocsátóképességét |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest Azure virtuális gép hálózati átviteli sebesség."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: 2da85c27bc8d16a443b215891f4cd0460f41926f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a>Sávszélesség/átvitel (NTTTCP) tesztelése

Hálózat teljesítménye szempontjából tesztelése az Azure-ban, esetén ajánlott toouse olyan eszköz, amely hello hálózati teszteléséhez célozza, és minimálisra csökkenti a teljesítményt befolyásoló egyéb erőforrások hello használata. NTTTCP ajánlott.

Másolja a hello eszköz tootwo hello Azure virtuális gépeinek azonos méretűnek. Egy virtuális gép úgy működik, mint a KÜLDŐ és fogadó, hello.

#### <a name="deploying-vms-for-testing"></a>Virtuális gépek telepítése teszteléshez
Ebben a tesztben hello célokra két virtuális gépek kell hello vagy hello ugyanazt a felhőalapú szolgáltatás, vagy hello az azonos rendelkezésre állási csoportban, hogy azt használja a belső IP-címek és hello Terheléselosztók kizárása hello teszt. Hello VIP a lehetséges tootest, de az ilyen vizsgálat a dokumentum hello hatókör kívül esik.
 
Jegyezze fel a címzett hello IP-címe. Most hívható meg, hogy IP "a.b.c.r"

Jegyezze fel a hello magok száma a virtuális gép hello. Most hívása a "\#num\_magok"
 
Hello NTTTCP teszteléséhez 300 másodperc (azaz 5 perc) virtuális gép hello küldő és fogadó virtuális gép fut.

Tipp: Beállításakor a hello ellenőrzéséhez először, próbálja meg egy rövidebb tesztelési időszak tooget visszajelzés hamarabb. Miután hello eszköz a várt módon működik, kiterjesztése hello tesztelési időszak too300 másodperc hello legpontosabb eredmények.

> [!NOTE]
> hello küldő **és** adjon meg fogadó **hello azonos** időtartam paraméter tesztelése (-t).

10 másodperc az egyetlen TCP adatfolyam tootest:

Fogadó paraméterek: ntttcp - r -t 10 - P 1

Küldő paraméterek: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1

> [!NOTE]
> a minta megelőző hello csak akkor használt tooconfirm a konfigurációt. Érvényes példák a vizsgálat a dokumentum későbbi szakaszában tartoznak.

## <a name="testing-vms-running-windows"></a>WINDOWS rendszerű virtuális gépek ellenőrzési:

#### <a name="get-ntttcp-onto-hello-vms"></a>NTTTCP jussanak el a virtuális gépek hello.

Hello legújabb verzió letöltéséhez: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769>

Vagy keresse meg, ha át: <https://www.bing.com/search?q=ntttcp+download> \< --először találati kell lennie

Vegye figyelembe a külön mappába, például a c: NTTTCP üzembe\\eszközök

#### <a name="allow-ntttcp-through-hello-windows-firewall"></a>Hello Windows tűzfalon keresztül NTTTCP engedélyezése
Hello fogadó hozzon létre egy olyan engedélyezési szabály a Windows tűzfal tooallow hello a NTTTCP forgalom tooarrive. Legegyszerűbb tooallow hello teljes NTTTCP program neve helyett tooallow adott TCP-portot a bejövő.

A Windows tűzfal hello keresztül ntttcp engedélyezése:

netsh advfirewall tűzfal program szabály hozzáadása =\<ELÉRÉSI\>\\ntttcp.exe neve = "ntttcp" protokoll bármely dir = művelet = = enable engedélyezése = yes profil = ANY

Például, ha a másolt ntttcp.exe toohello "c:\\eszközök" mappa, hello parancs lenne: 

netsh advfirewall tűzfal hozzáadása szabály program = c:\\eszközök\\ntttcp.exe neve = "ntttcp" protokoll bármely dir = művelet = = enable engedélyezése = yes profil = bármely

#### <a name="running-ntttcp-tests"></a>Tesztek futtatása NTTTCP

Indítsa el a NTTTCP a fogadó hello (**CMD-ről futtatva**, nem a PowerShell):

ntttcp - r-m [2\*\#num\_magok],\*, a.b.c.r -t 300

Ha virtuális gép hello négy maggal és 10.0.0.4 IP-címét, azt néznek ki:

ntttcp - r – m 8,\*, 10.0.0.4 -t 300


Indítsa el a NTTTCP a KÜLDŐ hello (**CMD-ről futtatva**, nem a PowerShell):

ntttcp -s – m 8,\*, 10.0.0.4 -t 300 

Várjon, amíg a hello eredmények elérése érdekében.


## <a name="testing-vms-running-linux"></a>Linuxos virtuális gépek ellenőrzési:

Nttcp a linux használja. Az elérhető <https://github.com/Microsoft/ntttcp-for-linux>

A Linux virtuális gépek (KÜLDŐ és fogadó) hello futtassa az alábbi parancsokat ntttcp-az-linux a virtuális gépeken előkészítése:

CentOS - telepítés Git:
``` bash
  yum install gcc -y  
  yum install git -y
```
Ubuntu - telepítés Git:
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
Ellenőrizze, és mindkét telepítéséhez:
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

Ahogy hello Windows példában feltételezzük, hogy hello Linux fogadó IP-cím 10.0.0.4

Indítsa el a NTTTCP a Linux a fogadó hello:

``` bash
ntttcp -r -t 300
```

És hello KÜLDŐ, futtassa:

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
Ha nincs paraméter teszt hossza alapértelmezett too60 másodperc kap

## <a name="testing-between-vms-running-windows-and-linux"></a>Az ellenőrzési Windows és LINUX rendszerű virtuális gépek között:

Ez a forgatókönyv nem kell engedélyezni a hello nem szinkron módban hello vizsgálat futtatható, számára. Hello használata ehhez **-N jelző** Linux, és **-ns jelző** Windows.

#### <a name="from-linux-toowindows"></a>A Linux tooWindows:

Fogadó <Windows>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

Küldő <Linux> :

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-toolinux"></a>A Windows tooLinux:

Fogadó <Linux>:

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

Küldő <Windows>:

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a>Következő lépések
* Attól függően, hogy az eredmények lehet hely túl[optimalizálhatja a hálózati átviteli gépek](virtual-network-optimize-network-bandwidth.md) a forgatókönyvéhez.
* Ismerje meg [Azure Virtual Network gyakori kérdések (GYIK)](virtual-networks-faq.md)
