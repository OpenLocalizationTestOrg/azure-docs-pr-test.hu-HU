---
title: "Linux felhasználónevek aaaSelecting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooselect felhasználó nevét, a Linux virtuális gép az Azure-ban."
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
ms.openlocfilehash: c65e2ac46f40bb8c9d74cccbaf248a070c0fa6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a>Felhasználónevek kiválasztása Azure-ban futtatott Linux esetén
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Amikor egy Linux virtuális gépet az Azure később használható toolog hello VM a nem rendszergazda felhasználó hello nevét kell megadnia. Úgy is dönthet, hello hello új felhasználó nevét, vagy ha a beállítás a klasszikus Azure portálon hello elfogadhatja hello alapértelmezett neve "azureuser".

A legtöbb esetben ez a felhasználó nem található a hello alapjául szolgáló lemezképhez, és hello kiépítési folyamat során jön létre. Ha alapszintű hello Virtuálisgép-lemezkép hello létezik, akkor hello Azure Linux ügynök egyszerűen konfigurál hello jelszó vagy SSH-kulcs az adott felhasználó hello hello virtuális gép létrehozásakor a megadott adatok alapján.

**Azonban**, Linux, amely nem használható felhasználónevek álló készletet határoz meg. kiépítési folyamat lesz hello **sikertelen** Ha tooprovision Linux virtuális gép egy meglévő rendszer felhasználó, amelyet a 0 – 99 UID rendelkező felhasználóként. Tipikus példája hello `root` felhasználó, amelynek UID 0.

* További információ: [Linux Standard Base - felhasználói azonosító tartományok](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

hello CentOS és Ubuntu, hogy azt ne használja az Azure-on Linux virtuális gépek kiépítése közös beépített rendszer felhasználók listája látható. A lista létrehozási csak egy példa, toohello dokumentáció a részletek a telepítési tooensure úgy dönt, nem ütközik egy meglévő felhasználóval rendszer hello felhasználónévhez.

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

