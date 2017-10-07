---
title: "Linux aaaEndorsed disztribúciók |} Microsoft Docs"
description: "További információk a Linux az Azure-támogatott disztribúciókkal, többek között irányelvek, Ubuntu, CentOS, Oracle és SUSE."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: f006972d4611434c62b72a1d8df60caf753e15f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Az Azure által támogatott disztribúciók Linux
Partnerek hello Azure piactér Linux képek adja meg. Már dolgozunk a különböző Linux Közösségek tooadd még több változatban is elkészíti toohello által támogatott terjesztési lista. Hello addig, az azokat a terjesztéseket, amelyek nem érhetők el az hello piactér, mindig helyezheti saját Linux: hello útmutatást követve [létrehozása és feltöltése hello Linux operációs rendszertartalmazóvirtuálismerevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="supported-distributions-and-versions"></a>Támogatott disztribúcióiról, valamint a verziók
hello következő táblázatban hello Linux terjesztéseket, a támogatott Azure-on. Tekintse meg a túl[Linux támogatása a Microsoft Azure-ban lemezképek](https://support.microsoft.com/en-us/kb/2941892) részletes információkat.

hello Linux integrációs szolgáltatások (LIS) illesztőprogramok a Hyper-V és Azure győződjön meg arról, hogy a Microsoft közvetlenül toohello fölérendelt Linux kernel hozzájárul kernel modulok érhetők el.  Egyes LIS-illesztőprogramok beépített hello terjesztési kernel alapértelmezés szerint. Régebbi azokat a terjesztéseket, amelyek Red Hat Enterprise (RHEL) – CentOS, külön letölthető / [Linux integrációs szolgáltatások verzió 4.1 a Hyper-V](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). Lásd: [Linux kernel követelmények](create-upload-generic.md#linux-kernel-requirements) hello LIS illesztőprogramok további információt.

hello Azure Linux ügynök már előre telepítve van a hello Azure piactéren elérhető rendszerkép és általában adattárból hello telepítési csomag. Forráskód található [GitHub](https://github.com/azure/walinuxagent).

| Disztribúció | Verzió | Illesztőprogramok | Ügynök |
| --- | --- | --- | --- |
| CentOS |A centOS 6.3 + 7.0 + |CentOS 6.3: [LIS letöltése](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: kernel a |Csomag: A [tárház](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) "WALinuxAgent" alatt <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |A kernel |Forráskód: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7.9 +, 8.2 + |A kernel |Csomag: a "waagent" adattárban lévő <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |A kernel |Csomag: a "WALinuxAgent" adattárban lévő <br/>Forráskód: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 + 7.1 + |A kernel |Csomag: a "WALinuxAgent" adattárban lévő <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES SAP esetén<br>11 SP4<br>12 SP1 +|A kernel |Csomag:<p> a 11 [felhő: eszközök](https://build.opensuse.org/project/show/Cloud:Tools) tárház<br>a "python-azure-ügynök" a "Nyilvános felhő" modulban 12<br/>Forráskód: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |Termékek 42.1 + openSUSE |A kernel |Csomag: A [felhő: eszközök](https://build.opensuse.org/project/show/Cloud:Tools) tárház "python-azure-ügynök" alatt <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04, 14.04, 16.04, 16.10 |A kernel |Csomag: a "walinuxagent" adattárban lévő <br/>Forráskód: [GitHub](https://github.com/Azure/WALinuxAgent) |

## <a name="partners"></a>Partnerek

### <a name="coreos"></a>CoreOS
[https://CoreOS.com/docs/running-CoreOS/cloud-Providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Hello CoreOS webhelyéről:

*CoreOS készült biztonsági, konzisztencia- és megbízhatóságát. Helyett yum vagy apt csomagok telepítése, használja a Linux-tárolók toomanage CoreOS a szolgáltatások magasabb absztrakciós szinten. Egyetlen szolgáltatás kódot és az összes függősége olyan, amelyek egy vagy több CoreOS gépeken futtathatók tárolóban vannak csomagolva.*

### <a name="credativ"></a>Credativ
[http://www.credativ.Co.uk/credativ-blog/debian-Images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ egy független tanácsadás és a szolgáltatások munkahelyi, amely hello fejlesztése és megvalósítása szakmai megoldások szabad szoftvernek a használatával. Vezető nyílt forráskódú szakemberei, mert a Credativ számos számítástechnikai osztály számára támogatási használó a nemzetközi felismerés rendelkezik. A Microsoft együtt Credativ jelenleg előkészítését végzi, megfelelő Debian képek Debian 8 (Jessie) és a Debian 7 (Wheezy) előtt. Mindkét képek Azure kifejezetten toorun és könnyen kezelhető hello platformon keresztül. Credativ hello hosszú távú karbantartási és az Azure-ba, a nyílt forrás támogatási erőforrások segítségével hello Debian lemezképek frissítését is támogatja.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle stratégia toooffer megoldások a nyilvános és magánfelhőkhöz széleskörű portfóliót. hello stratégia biztosít az ügyfelek hogyan Oracle-felhőkben Oracle szoftvereket és a többi felhőből telepíthet-e azokat a rugalmasságának és. Oracle partneri kapcsolat áll fenn a Microsoft lehetővé teszi, hogy az ügyfelek toodeploy Oracle szoftver a Microsoft nyilvános és privát felhők hello megbízhatósággal certification és Oracle-támogatást.  Oracle kötelezettségvállalás és Oracle nyilvános és magánfelhő-megoldások használatára irányuló befektetéséből nem változott.

### <a name="red-hat"></a>Red Hat
[http://www.redhat.com/en/partners/strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

hello világ vezető szolgáltató nyílt forráskódú megoldások, több mint 90 % Fortune 500 vállalatok üzleti kihívások megoldásában, informatikai igazítása segít a Red Hat és üzleti stratégiát, és készítse elő a hello jövőbeli technológia. Red Hat ennek érdekében biztonságos megoldások egy megnyitott üzleti modelljéhez és egy elérhető, előre jelezhető előfizetés modellt biztosít.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server az Azure-on bevált platformot biztosít az felső szintű megbízhatóság és a felhőalapú informatika. SUSE tartozó sokoldalú Linux platformon zökkenőmentesen integrálható az Azure cloud services toodeliver egy könnyen kezelhető felhőalapú környezetben. Több mint 9,200 hiteles alkalmazások SUSE Linux Enterprise Server több mint 1,800 független szoftverszállítóktól származó SUSE biztosítja, hogy hello adatközpont futó támogatott munkaterhelések magabiztosan telepíthető az Azure-on.

### <a name="canonical"></a>Canonical
[http://www.ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Canonical termékgondozó csoportja és a közösségi Megnyitás irányítás meghajtó Ubuntu a sikeres ügyfél, kiszolgáló és a felhőalapú megoldások, mely tartalmazza a fogyasztók személyes felhőbeli szolgáltatások. Egy egységes, szabad platform az Ubuntu, a telefonszám toocloud kanonikus tartozó stratégiai hello telefon, táblagép, televízió és asztali családba tartozó összefüggő felületek biztosít. A stratégiai köszönhetően az Ubuntu hello első választási fogyasztói Electronics nyilvánosfelhő-szolgáltatók toohello döntéshozók különböző intézmények kedvenc egyes technologists között.

A fejlesztők és hello világ mérnöki adatközpontok Canonical hardver döntéshozók, tartalomszolgáltatók és szoftver fejlesztők toobring Ubuntu megoldások toomarket számítógépek, kiszolgálók és a kézi eszközök egyedi módon pozícionált toopartner.
