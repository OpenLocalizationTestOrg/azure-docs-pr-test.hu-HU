---
title: "aaaAzure Linux virtuális gép ügynök áttekintése |} Microsoft Docs"
description: "Megtudhatja, hogyan tooinstall és a Linux-ügynök (waagent) toomanage konfigurálja a virtuális gép Azure Fabric Controller interakció."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: szark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4e08c84d9205f4db7aae6fd1568ec1f15fba395c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-and-using-hello-azure-linux-agent"></a>Megismeréséhez és használatához hello Azure Linux ügynök
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Bevezetés
hello Microsoft Azure Linux ügynök (waagent) a Linux és freebsd rendszerű kiépítés és hello Azure Fabric Controller interakcióba VM kezeli. A következő funkció a Linux és freebsd rendszerű infrastruktúra-szolgáltatási központi telepítések hello tartalmazza:

> [!NOTE]
> Lásd: hello Azure Linux ügynök [információs](https://github.com/Azure/WALinuxAgent/blob/master/README.md) további részleteket.
> 
> 

* **Kép kiépítése**
  
  * A felhasználói fiók létrehozása
  * SSH hitelesítési típus konfigurálása
  * Az SSH nyilvános kulcsok és kulcspárok központi telepítését
  * Beállítás hello gazdagép neve
  * Közzétételi hello állomás neve toohello platform DNS
  * Jelentéskészítési SSH állomás kulcs ujjlenyomat toohello platform
  * Erőforrás Lemezkezelés
  * Formázás és hello erőforrás lemez csatlakoztatása
  * Lapozófájl konfigurálása
* **Hálózat**
  
  * Kezeli az útvonalak tooimprove kompatibilitási platform DHCP-kiszolgálókkal
  * Biztosítja a hello stabilitásának hello a hálózati adapter neve
* **Kernel**
  
  * Konfigurálja a virtuális NUMA (letiltja a kernel < 2.6.37)
  * Hyper-V entrópia /dev/random a felhasználva
  * Konfigurálja az SCSI-időtúllépések hello legfelső szintű eszköz (amely lehet távoli)
* **Diagnosztika**
  
  * Konzol átirányítási toohello soros port
* **Az SCVMM központi telepítések**
  
  * Észleli és hello VMM-ügynök Linux betöltéséhez, ha a System Center Virtual Machine Manager 2012 R2 környezetben futó
* **Virtuálisgép-bővítmény**
  
  * A Linux virtuális gép (IaaS) tooenable szoftver- és konfigurációs automation Microsoft és a partnerei által készített összetevő beszúrása
  * Virtuálisgép-bővítmény hivatkozási megvalósítása a [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions)

## <a name="communication"></a>Kommunikáció
hello információáramlás hello platform toohello ügynöktől két csatornákon keresztül történnek:

* A rendszerindítás DVD csatolt IaaS telepítésekhez. A DVD-t egy OVF-kompatibilis hello tényleges SSH keypairs kivételével az összes kiépítési információkat tartalmazó konfigurációs fájlt tartalmazza.
* A TCP-végpont teszi ki a REST API használt tooobtain központi telepítése és topológia konfigurálása.

## <a name="requirements"></a>Követelmények
hello következő rendszerek lettek tesztelve, és a hello Azure Linux ügynök toowork ismert:

> [!NOTE]
> Ez a lista eltérhet a hello hivatalos a Microsoft Azure Platform hello támogatott rendszerek listáját itt leírt módon: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)
> 
> 

* CoreOS
* CentOS 6.3 +
* Red Hat Enterprise Linux 6.7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Egyéb támogatott rendszerek:

* Freebsd rendszerű 10 + (Azure Linux ügynök v2.0.10 +)

hello Linux-ügynök megfelelően egyes rendszer csomagok rendelés toofunction függ:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Fájlrendszer segédprogramok: sfdisk, fdisk, mkfs, válogatottak
* Jelszó-eszközök: chpasswd, sudo
* Eszközök feldolgozása szöveg: csökkentésének, grep
* A hálózati eszközök: ip-útvonal
* Kernel támogatása UDF fájlrendszerek csatlakoztatni.

## <a name="installation"></a>Telepítés
A telepítési csomag tárházból egy RPM vagy DEB-csomag telepítését telepítése és hello Azure Linux ügynök frissítése hello előnyben részesített módja. Minden hello [terjesztési szolgáltatók által támogatott](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hello Azure Linux ügynök csomag integrálja a képeket és tárházak találhatók.

Tekintse meg a hello toohello dokumentációját [Azure Linux ügynök-tárház a Githubon](https://github.com/Azure/WALinuxAgent) a speciális telepítési lehetőségeket, például a forrás- vagy toocustom helyekről vagy az előtagok telepítése.

## <a name="command-line-options"></a>Parancssori kapcsolók
### <a name="flags"></a>Jelzők
* részletes: növelése a megadott parancs
* kényszerített: hagyja ki az egyes parancsok interaktív megerősítése

### <a name="commands"></a>Parancsok
* Súgó: hello támogatott parancsok és jelzők sorolja fel.
* deprovision: tooclean hello rendszer kísérletet, és teszi megfelelő ismételt üzembe helyezéséhez. Ez a művelet törli a hello következő:
  
  * Az összes SSH állomáskulcsai (ha Provisioning.RegenerateSshHostKeyPair "y" hello konfigurációs fájlban)
  * A /etc/resolv.conf névkiszolgáló-konfiguráció
  * Gyökér szintű jelszavát a /etc/shadow (ha Provisioning.DeleteRootPassword "y" hello konfigurációs fájlban)
  * Gyorsítótárazott DHCP-ügyfél bérletek
  * Alaphelyzetbe állítását gazdagép neve toolocalhost.localdomain

> [!WARNING]
> Megszüntetés nem garantálja hello lemezkép nincs bejelölve, az összes bizalmas adatokat, és a megfelelő terjesztési.
> 
> 

* deprovision + felhasználói: hajt végre, minden elemet - deprovision (fent) is törli a hello (/var/lib/waagent nyert) utolsó kiépített felhasználói fiókot, és az adatok. Ez a paraméter esetén a megszüntetést egy olyanra, amely korábban az Azure-on kiépítés volt, előfordulhat, hogy lehet rögzíteni és újra felhasználni.
* verzió: waagent hello verzióját jeleníti meg
* serialconsole: konfigurálja a LÁRVAJÁRAT toomark ttyS0 (hello első soros port) hello rendszerindító konzollal. Ez biztosítja, hogy a kernel rendszerindítási naplóit toothe soros port küldött és elérhetővé tenni a hibakereséshez.
* démon: waagent futtató hello platform démon toomanage interakció. Az argumentum értéke a megadott toowaagent hello waagent init parancsfájlban.
* Start: háttérfolyamatként waagent futtatása

## <a name="configuration"></a>Konfiguráció
Egy konfigurációs fájl (/ etc/waagent.conf) vezérlők hello waagent műveleteket. Alább látható egy példa konfigurációs fájlt:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

hello részletesen ismerteti a különböző konfigurációs beállításait. Beállítási lehetőségek állnak a három típusa létezik; Logikai érték, String vagy Integer. "y" vagy "n" Hello logikai konfigurációs beállításokat adhat meg. hello különleges kulcsszó "None" néhány karakterlánc típusú konfigurációs elemet részletes, az alábbi használható.

**Provisioning.Enabled:**  
Típus: logikai  
Alapértelmezett: y

Ez lehetővé teszi, hogy a felhasználó tooenable hello, vagy tiltsa le a hello kiépítés hello ügynök funkcióit. Érvényes értékek: "y" vagy "n". Kiépítés le van tiltva, ha hello kép SSH-állomás és a felhasználói kulcsok megmaradnak, és a hello Azure létesítési API megadott minden beállítás figyelmen kívül hagyja.

> [!NOTE]
> Hello `Provisioning.Enabled` túl "n" Ubuntu felhő lemezképeket inicializálás felhőben történő üzembe helyezéséhez használjon a paraméter alapértelmezett értéke.
> 
> 

**Provisioning.DeleteRootPassword:**  
Típus: logikai  
Alapértelmezett: n

Ha be van állítva, hello gyökér szintű jelszavát a hello/etc/árnyékmásolat fájl törlése során hello a telepítési folyamatot.

**Provisioning.RegenerateSshHostKeyPair:**  
Típus: logikai  
Alapértelmezett: y

Ha be van állítva, az összes SSH állomás kulcspárok (ecdsa, dsa és rsa) során hello létesítésének folyamatát kell használnia az/etc/ssh/törlés. És egy egyetlen új kulcspár.

hello titkosítási hello friss kulcspár alaptípusa hello Provisioning.SshHostKeyPairType bejegyzés által konfigurálható. Vegye figyelembe, hogy néhány terjesztési hozza létre a hiányzó titkosítási típusok SSH-kulcspár, hello SSH démon (például biztonsági újraindításkor) újraindításakor.

**Provisioning.SshHostKeyPairType:**  
Típus: Karakterlánc  
Alapértelmezett: rsa

E beállítás tooan titkosítási algoritmus típus hello SSH-démon hello virtuális gép által támogatott. hello általában a támogatott értékek a következők: "rsa", "dsa" és "ecdsa". Vegye figyelembe, hogy a "putty.exe" a Windows nem támogatja "ecdsa". Igen ha azt tervezi, a Windows tooconnect tooa Linux telepítési toouse putty.exe, használja az "rsa" vagy "dsa".

**Provisioning.MonitorHostName:**  
Típus: logikai  
Alapértelmezett: y

Ha beállításához waagent ellenőrzi hello Linux virtuális gép állomásnevét módosítások (a hello "állomásnév" parancs által visszaadott) és automatikusan frissíti a hálózati konfiguráció hello hello kép tooreflect hello módosítása. A sorrend toopush hello név toohello DNS-kiszolgálók módosításához hálózatkezelés hello virtuális gép újraindul. Ekkor röviden a Internet kapcsolat megszakadása.

**Provisioning.DecodeCustomData**  
Típus: logikai  
Alapértelmezett: n

Ha beállításához waagent meg fog dekódolni a Base64 kódolású anyag CustomData.

**Provisioning.ExecuteCustomData**  
Típus: logikai  
Alapértelmezett: n

Ha beállításához waagent végrehajtja a CustomData kiépítése után.

**Provisioning.PasswordCryptId**  
Típus: karakterlánc  
Alapértelmezett: 6

Jelszókivonat létrehozásakor használt titkosítási algoritmus.  
 1 - MD5  
 2a – Blowfish  
 5 - SHA-256  
 6 - SHA-512  

**Provisioning.PasswordCryptSaltLength**  
Típus: karakterlánc  
Alapértelmezett: 10

Jelszókivonat létrehozásához használt véletlenszerű védőérték hosszát.

**ResourceDisk.Format:**  
Típus: logikai  
Alapértelmezett: y

Ha beállításához hello erőforrás lemez hello platform által biztosított fog formázható és hello filesystem "ResourceDisk.Filesystem" hello felhasználó által kért típus "ntfs" csakis által waagent csatlakoztatva. Egy olyan partíciót, Linux (83) típusú hello lemezen rendelkezésre álló fog történni. Vegye figyelembe, hogy ez a partíció nem lesz formázva Ha sikeresen csatlakoztatva.

**ResourceDisk.Filesystem:**  
Típus: Karakterlánc  
Alapértelmezett: ext4

Azt határozza meg a hello erőforrás lemez hello fájlrendszer típusát. A Linux-disztribúció által támogatott értékek eltérők lehetnek. Ha X, majd mkfs hello karakterlánc. X hello Linux kép jelen kell lennie. SLES 11 lemezképeket általában használjon "ext3". Freebsd rendszerű lemezképek itt "ufs2" kell használni.

**ResourceDisk.MountPoint:**  
Típus: Karakterlánc  
Alapértelmezett: / mnt és az erőforrások 

Azt határozza meg a hello elérési utat, amelyen hello erőforrás lemez csatlakoztatva van. Vegye figyelembe, hogy hello erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.

**ResourceDisk.MountOptions**  
Típus: Karakterlánc  
Alapértelmezett: nincs

Lemez csatlakoztatási lehetőségek átadott toobe toohello csatlakoztatási -o parancsot határozza meg. Például ez az értékek, vesszővel elválasztott listája. "nodev, nosuid". Tekintse meg a részletes mount(8).

**ResourceDisk.EnableSwap:**  
Típus: logikai  
Alapértelmezett: n

Ha beállításához lapozófájl (/ swapfile) hello erőforrás lemezen jön létre, és toohello rendszer lapozóterület hozzá.

**ResourceDisk.SwapSizeMB:**  
Típus: egész szám  
Alapértelmezett: 0

hello mérete hello lapozófájl mérete (MB).

**Logs.Verbose:**  
Típus: logikai  
Alapértelmezett: n

Ha be van állítva, napló részletességi súlyozott van. Waagent too/var/log/waagent.log naplózza, és lehetővé teszi a hello logrotate funkció toorotate rendszernaplókat.

**AZ OPERÁCIÓS RENDSZER. EnableRDMA**  
Típus: logikai  
Alapértelmezett: n

Amennyiben beállításához hello ügynök fog próbálja tooinstall és töltsön be egy RDMA kernel-illesztőprogram hello belső vezérlőprogramját az alapul szolgáló hardver hello hello verziójának megfelelő.

**AZ OPERÁCIÓS RENDSZER. RootDeviceScsiTimeout:**  
Típus: egész szám  
Alapértelmezett: 300

Ez konfigurálja az hello SCSI időtúllépés másodpercben hello OS lemez- és meghajtókon. Ha nincs beállítva, hello rendszer alapértelmezett értékeket használják.

**AZ OPERÁCIÓS RENDSZER. OpensslPath:**  
Típus: Karakterlánc  
Alapértelmezett: nincs

Ez lehet használt toospecify egy alternatív elérési utat a hello openssl bináris toouse titkosítási műveletek.

**HttpProxy.Host, HttpProxy.Port**  
Típus: Karakterlánc  
Alapértelmezett: nincs

Ha beállításához hello ügynök használni fog a proxy server tooaccess hello internet. 

## <a name="ubuntu-cloud-images"></a>Ubuntu felhő lemezképek
Vegye figyelembe, hogy Ubuntu felhő lemezképek használata [felhő inicializálás](https://launchpad.net/ubuntu/+source/cloud-init) tooperform számos konfigurációs feladatok, akkor más módon kell kezelnie hello Azure Linux ügynök.  Vegye figyelembe a következő különbségek hello:

* **Provisioning.Enabled** túl "n" Ubuntu felhő képek feladatok kiépítés felhő inicializálás tooperform használó alapértelmezett értékeket.
* a következő konfigurációs paraméterek hello nincs hatással a felhő inicializálás toomanage hello erőforrás lemez és a lapozófájl-kapacitás helyet használó Ubuntu felhő képek rendelkezik:
  
  * **ResourceDisk.Format**
  * **ResourceDisk.Filesystem**
  * **ResourceDisk.MountPoint**
  * **ResourceDisk.EnableSwap**
  * **ResourceDisk.SwapSizeMB**
* Tekintse meg a következő erőforrások tooconfigure hello erőforrás lemez csatlakoztatási pont hello, és a kiépítés során Ubuntu felhő képek a lapozófájl:
  
  * [Ubuntu Wiki: Lapozófájl-kapacitás-partíciók konfigurálása](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Egyéni adatok hogy Azure virtuális géphez](../windows/classic/inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

