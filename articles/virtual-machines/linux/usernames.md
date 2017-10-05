---
title: "Kiválasztja a felhasználónevet Linux |} Microsoft Docs"
description: "Megtudhatja, hogyan válassza ki a felhasználói neveket Linux virtuális gép az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 1874d72e5f88816036667932371ff28704d186c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a>Felhasználónevek kiválasztása Azure-ban futtatott Linux esetén
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Amikor egy Linux virtuális gépet az Azure később használatával jelentkezzen be a virtuális gép nem legfelső szintű felhasználó nevét kell megadnia. Úgy is dönthet, az új felhasználó nevét, vagy ha a beállítás a klasszikus Azure portálon elfogadhatja az alapértelmezett neve "azureuser".

A legtöbb esetben ez a felhasználó nem létezik az alapjául szolgáló lemezképhez, és létrehozza a telepítési folyamat során. Ha a felhasználó létezik a virtuális gép alapjául szolgáló lemezképhez, majd az Azure Linux ügynök egyszerűen állítja be a jelszót és/vagy SSH-kulcs az adott felhasználó, amikor a virtuális gép létrehozása a megadott adatok alapján.

**Azonban**, Linux, amely nem használható felhasználónevek álló készletet határoz meg. Az üzembe helyezési folyamat fog **sikertelen** Ha kiépítéséhez Linux virtuális gép egy meglévő rendszer felhasználó, amelyet a 0 – 99 UID rendelkező felhasználóként. Tipikus példája a `root` felhasználó, amelynek UID 0.

* További információ: [Linux Standard Base - felhasználói azonosító tartományok](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

A CentOS és Ubuntu, hogy azt ne használja az Azure-on Linux virtuális gépek kiépítése közös beépített rendszer felhasználók listáját a következő: A lista létrehozási csak egy példa, olvassa el a dokumentációt a annak érdekében, hogy úgy dönt, a felhasználónév nem ütközik egy meglévő rendszer felhasználó.

## <a name="centos"></a>CentOS
* abrt
* ADM
* hang
* bin
* CD-ROM
* cgred
* démon
* dbus
* kitárcsázáshoz
* DIP
* lemez
* hajlékonylemez
* FTP
* FTP
* játékok
* Gopher
* haldaemon
* halt
* kmem
* zárolás
* LP
* mail
* Man
* Hiba
* nfsnobody
* senki sem
* NTP
* Operátor
* oprofile
* postdrop
* utótag
* qpidd
* legfelső szintű
* RPC
* rpcuser
* saslauth
* Leállítás
* slocate
* sshd
* stapdev
* stapusr
* Szinkronizálás
* sys
* szalag
* Teszt
* tcpdump parancsot
* TTY
* felhasználók
* utempter
* utmp
* uucp
* vcsa
* Videó
* kerék

## <a name="ubuntu"></a>Ubuntu
* ADM
* Rendszergazda
* hang
* biztonsági mentés
* bin
* CD-ROM
* crontab
* démon
* kitárcsázáshoz
* DIP
* lemez
* Faxkiszolgáló
* hajlékonylemez
* biztosító
* játékok
* gnats
* IRC
* kmem
* fekvő
* libuuid
* lista
* LP
* mail
* Man
* MessageBus
* mlocate
* netdev
* Hírek
* senki sem
* nogroup
* Operátor
* plugdev
* Proxy
* legfelső szintű
* SASL
* árnyékmásolat
* src
* ssh
* sshd
* személyzet
* sudo
* Szinkronizálás
* sys
* syslog
* szalag
* TTY
* felhasználók
* utmp
* uucp
* Videó
* hangalámondás
* whoopsie
* www-adatok

