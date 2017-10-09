---
title: "aaaUsing hello Azure Linux Docker Virtuálisgép-bővítménnyel"
description: "Docker és hello Azure virtuálisgép-bővítmények ismerteti és bemutatja, hogyan tooprogrammatically létre, amelyek a docker-gazdagépekből hello Azure parancssori felület használatával hello parancssorból Azure virtuális gépek."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: eaff75e3-d929-4931-a4a1-8c377a8e7302
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/29/2016
ms.author: rasquill
ms.openlocfilehash: 1e192ad7c273aa9c997ea7bfa53b7de0b41a43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-from-hello-azure-command-line-interface-azure-cli"></a>Hello Docker Virtuálisgép-bővítmény a hello Azure parancssori felület (CLI) használatával
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Hello Resource Manager modellt hello Docker Virtuálisgép-bővítmény használatával kapcsolatos információkért lásd: [Itt](../dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ez a témakör ismerteti, hogyan toocreate virtuális gép és a hello Docker Virtuálisgép-bővítmény hello szolgáltatás bármilyen platformon az Azure parancssori felület kezelési (asm) módban. [Docker](https://www.docker.com/) az hello egyik legnépszerűbb virtualizálási megközelítések használó [Linux tárolók](http://en.wikipedia.org/wiki/LXC) ahelyett, hogy a virtuális gépek adatforgalom elkülönítése, és a megosztott erőforrások számítástechnikai módja. Hello Docker Virtuálisgép-bővítmény és hello [Azure Linux ügynök](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate egy tároló korlátlan számú tárolót tárolhat az Azure-alkalmazások Docker virtuális Gépet. toosee magas szintű információt a tárolók és azok előnyeit, lásd: hello [Docker magas szintű Faliújság](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="how-toouse-hello-docker-vm-extension-with-azure"></a>Hogyan toouse hello Docker Virtuálisgép-bővítmény az Azure-ral
toouse hello Docker Virtuálisgép-bővítmény az Azure-ral, telepítenie kell egy hello verziója [Azure parancssori felület](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) nagyobb, mint 0.8.6 (mivel az írás hello a jelenlegi verzió: 0.10.0-s). Telepítheti a hello Azure parancssori felület Mac, Linux és Windows rendszeren.

hello teljes folyamat toouse Azure Docker felettébb egyszerű:

* Hello Azure CLI és annak függőségeit, amelyből el kívánja toocontrol Azure hello számítógépre telepítse (Windows, ez lesz egy Linux-disztribúció virtuális gépként fut)
* Hello Azure CLI Docker parancsok toocreate egy virtuális gép Docker állomás használja az Azure-ban
* Hello helyi Docker parancsok toomanage a Docker-tárolók használatára a Docker virtuális Gépet az Azure-ban.

### <a name="install-hello-azure-command-line-interface-azure-cli"></a>Hello Azure parancssori felület (CLI) telepítése
tooinstall és az Azure parancssori felület hello konfigurálása, lásd: [hogyan tooinstall hello Azure parancssori felület](../../../cli-install-nodejs.md). tooconfirm hello telepítési típus `azure` hello parancssorban és rövid rövid időn belül kell megjelennie hello Azure CLI ASCII ábrákat, amely alapszintű hello sorolja fel a rendelkezésre álló tooyou parancsokat. Hello telepítési megfelelően működött, ha el tudja tootype `azure help vm` és felsorolt hello parancsok van-e "docker".

> [!NOTE]
> Docker vannak a Windows-eszközök [Docker gép](https://docs.docker.com/installation/windows/), mely tooautomate hello létrehozása egy docker-ügyfél használható toowork Azure virtuális gépeken futó, docker-gazdagépekből is használható.
> 
> 

### <a name="connect-hello-azure-cli-tootooyour-azure-account"></a>Csatlakozás hello Azure CLI tootooyour Azure-fiók
Hello Azure CLI használata előtt össze kell kapcsolnia az Azure fiókhitelesítő adatok hello Azure parancssori felület a platformon. szakasz hello [hogyan tooconnect tooyour Azure-előfizetés](../../../xplat-cli-connect.md) ismerteti, hogyan tooeither töltse le, és importálja a **.publishsettings** fájlt, vagy rendelje hozzá az Azure CLI szervezeti azonosítóval.

> [!NOTE]
> Néhány különbségek vannak viselkedés egy használatakor vagy hello más módszerekkel hitelesítési, így meg arról, hogy tooread hello dokumentum fenti toounderstand hello másképpen fog működni.
> 
> 

### <a name="install-docker-and-use-hello-docker-vm-extension-for-azure"></a>Docker telepítéséhez és hello Docker Virtuálisgép-bővítmény használatához az Azure-bA
Hajtsa végre a hello [Docker telepítési utasításokat](https://docs.docker.com/installation/#installation) tooinstall Docker helyileg a számítógépen.

toouse Docker az Azure virtuális gépeket, a virtuális gép hello használt hello Linux kép rendelkeznie kell hello [Azure Linux Virtuálisgép-ügynök](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) telepítve. Jelenleg nincsenek lemezképekről Ez csak két típusa:

* Az Ubuntu lemezképének hello Azure kép gyűjtemény vagy
* Egy egyéni Linux lemezképnek, amely a korábban létrehozott hello Azure Linux Virtuálisgép-ügynök telepítve és konfigurálva. Lásd: [Azure Linux Virtuálisgép-ügynök](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt toobuild egyéni Linux virtuális gép és hello Azure Virtuálisgép-ügynök.

### <a name="using-hello-azure-image-gallery"></a>Hello Azure lemezkép-katalógus használatával
Bash vagy terminál-munkamenetet használja az Azure CLI parancs toolocate hello VM gyűjtemény toouse legutóbbi Ubuntu lemezképet hello írja be a következő hello

`azure vm image list | grep Ubuntu-14_04`

Válassza ki valamelyik hello képek nevének, például a `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, és használjon hello következő parancsot a toocreate egy új virtuális Gépet, hogy a lemezkép használatával.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Ahol:

* *&lt;VM-cloudservice neve&gt;*  hello hello lesz hello Docker tároló állomására Azure virtuális gép neve
* *&lt;felhasználónév&gt;*  hello alapértelmezett legfelső szintű felhasználó a virtuális gép hello hello felhasználónév
* *&lt;jelszó&gt;*  hello jelszó a hello *felhasználónév* szabványoknak hello összetettségi az Azure-fiók

> [!NOTE]
> Jelenleg a jelszót kell legalább 8 karakterből álljon, egy kisbetű és egy nagybetű, szám és egy, a következő karaktereket hello egy speciális karaktert tartalmaz: `!@#$%^&+=`. Nem, hello időszaka mondat megelőző hello hello végén nincs egy speciális karaktert.
> 
> 

Ha hello parancs sikeres volt, akkor kell kinéznie: hello következő, attól függően, hogy hello pontos argumentumok és használt beállítások

![](media/cli-use-docker/dockercreateresults.png)

> [!NOTE]
> A virtuális gépek létrehozására is igénybe vehet néhány percet, de után van kiépítve (hello állapot értéke `ReadyRole`) hello Docker démon (hello Docker szolgáltatás) kezdődik, és csatlakozhat a Docker-tároló toohello-gazdagépen.
> 
> 

tootest hello típusa, az Azure-ban létrehozott Docker VM

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

Ha  *&lt;vm-neve--használt&gt;*  hello hello virtuális gép nevét, a hívás túl használt van`azure vm docker create`. Valami hasonló toohello következő, az azt jelzi, hogy a Docker állomás virtuális gép mentése Azure-ban futó és a parancsok Várakozás kell megjelennie. 

Most megpróbálhatja tooconnect a docker ügyfél tooobtain információk segítségével (az egyes Docker ügyfél beállítások, például a Mac, előfordulhat, hogy toouse `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Csak toobe, hogy szerepel-e az összes működő, a hello Docker-bővítményt a virtuális gép hello láthatja:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker gazdagép VM hitelesítés
Ezenkívül toocreating hello Docker VM hello `azure vm docker create` parancs automatikusan is hoz létre hello szükséges tanúsítványok tooallow a Docker ügyfél számítógép tooconnect toohello Azure tároló-gazdagépen HTTPS-kapcsolaton keresztül, és hello tanúsítványokat is tárolja hello ügyfél és a gazdagép gépnek, szükség szerint. Az ezt követő kísérletek hello meglévő újra felhasználható és tanúsítványokat hello új állomás megosztva.

Alapértelmezés szerint tanúsítványok kerülnek `~/.docker`, és a Docker lesz portra beállított toorun **2376**. Ha szeretné toouse egy másik portot vagy a könyvtárat, majd segítségével hello következő `azure vm docker create` parancssori beállítások tooconfigure a Docker tároló gazdagép VM toouse egy másik portot vagy különböző tanúsítványok csatlakozó ügyfeleken:

```
-dp, --docker-port [port]              Port toouse for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

hello hello gazdagépen Docker démon a konfigurált toolisten és hello kapcsolatok megadott port hello tanúsítványok használatával hello által generált ügyfél hitelesítésére `azure vm docker create` parancsot. hello ügyfélszámítógép ezen tanúsítványok toogain hozzáférés toohello Docker állomással kell rendelkeznie.

> [!NOTE]
> Hálózati futtató gazdagépet, amelyen ezek a tanúsítványok nélkül lesz téve tooanyone tooconnect toohello számítógép által. Hello alapértelmezett konfiguráció módosítása előtt győződjön meg arról, hogy tudomásul veszi hello kockázatok tooyour számítógépeknek és alkalmazásoknak.
> 
> 

## <a name="next-steps"></a>Következő lépések
* Készen áll a toogo toohello áll [Docker felhasználói útmutató] és a Docker virtuális gép használja. egy Docker-kompatibilis virtuális Gépet az új portálon hello toocreate lásd [hogyan toouse hello-Docker Virtuálisgép-bővítmény hello Portal].
* hello Azure Docker Virtuálisgép-bővítmény is támogatja a Docker Compose, teljes bármely környezet egy deklaratív YAM-fájl tootake egy fejlesztői modellezve alkalmazást használó, és egy egységes központi telepítés létrehozása. Lásd: [Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a].  

<!--Anchors-->
[Subheading 1]:#subheading-1
[Subheading 2]:#subheading-2
[Subheading 3]:#subheading-3
[Next steps]:#next-steps

[How toouse hello Docker VM Extension with Azure]:#How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]:#Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]:#Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 tooanother azure.microsoft.com documentation topic]:../../virtual-machines-windows-hero-tutorial.md
[Link 2 tooanother azure.microsoft.com documentation topic]:../../../app-service-web/web-sites-custom-domain-name.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md
[hogyan toouse hello-Docker Virtuálisgép-bővítmény hello Portal]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker felhasználói útmutató]:https://docs.docker.com/userguide/

[Ismerkedés a Docker és toodefine összeállítása, és futtassa a több tároló alkalmazást egy Azure virtuális gépen a]:../docker-compose-quickstart.md
