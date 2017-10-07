---
title: "Linux Virtuálisgép-bővítmény Docker aaaUsing |} Microsoft Docs"
description: "Docker és hello Azure virtuálisgép-bővítmények és hogyan toocreate Azure virtuális gépek, amelyek segítségével docker-gazdagépekből hello klasszikus üzembe helyezési modellel Azure CLI ismerteti."
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 19cf64e8-f92c-43ad-a120-8976cd9102ac
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/27/2016
ms.author: rasquill
ms.openlocfilehash: 9d455b63c48b0c1b6f14862e072f899a73b46153
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-docker-vm-extension-with-hello-azure-classic-portal"></a>Hello Docker Virtuálisgép-bővítmény hello klasszikus Azure portál használatával
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

[Docker](https://www.docker.com/) az hello egyik legnépszerűbb virtualizálási megközelítések használó [Linux tárolók](http://en.wikipedia.org/wiki/LXC) ahelyett, hogy a virtuális gépek adatforgalom elkülönítése, és a megosztott erőforrások számítástechnikai módja. Használhatja a hello Docker Virtuálisgép-bővítmény kezeli [Azure Linux ügynök] toocreate egy tároló korlátlan számú tárolót tárolhat az Azure-alkalmazások Docker virtuális Gépet.

> [!NOTE]
> Ez a témakör ismerteti, hogyan toocreate a virtuális gép Docker hello a klasszikus Azure portálon. Hogyan toocreate egy Docker-VM hello parancssorból: toosee [hogyan toouse hello hello Azure parancssori felület (CLI) a Virtuálisgép-bővítmény Docker]. toosee magas szintű információt a tárolók és azok előnyeit, lásd: hello [Docker magas szintű Faliújság](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).
> 
> 

## <a name="create-a-new-vm-from-hello-image-gallery"></a>Új virtuális gép létrehozása a hello kép gyűjteménye
hello első lépése az Azure virtuális gép, amely támogatja a Docker Virtuálisgép-bővítmény, az Ubuntu 14.04 LTS lemezképének kép gyűjtemény hello használata egy példa kiszolgálói lemezképet és az Ubuntu 14.04 asztali ügyfélként hello Linux lemezképéről igényel. Hello portálon kattintson **+ új** hello az alsó bal sarok toocreate egy új Virtuálisgép-példány, és jelöljön ki egy Ubuntu 14.04 LTS képet hello választható vagy hello befejezéséhez kép gyűjteménye, alább látható módon.

> [!NOTE]
> Jelenleg csak Ubuntu 14.04 LTS lemezképek újabb a 2014. július támogatja a Virtuálisgép-bővítmény Docker hello.
> 
> 

![Hozzon létre egy új Ubuntu kép](./media/portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Docker tanúsítványok létrehozása
Virtuális gép létrehozása után hello, győződjön meg arról, hogy Docker telepítve van-e az ügyfélszámítógépen. (További információkért lásd: [Docker telepítési utasításokat](https://docs.docker.com/installation/#installation).)

Hozzon létre hello szerint túl Docker kommunikációs tanúsítványt és kulcsot fájlok[Docker fut, a https] és helyezze el őket hello  **`~/.docker`**  könyvtárhoz, az ügyfélszámítógépen.

> [!NOTE]
> hello Docker Virtuálisgép-bővítmény hello portálon jelenleg szükséges hitelesítő adatokat, amelyek base64-kódolású.
> 
> 

Hello parancssorból használható  **`base64`**  vagy egy másik kedvenc eszköz toocreate base64-kódolású témakörök kódolást. Ezzel a tanúsítvány és kulcs fájlok egyszerű számú hasonló toothis nézhet ki:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-hello-docker-vm-extension"></a>Hello Docker Virtuálisgép-bővítmény hozzáadása
tooadd hello Docker Virtuálisgép-bővítmény, keresse meg a létrehozott hello Virtuálisgép-példány, és görgessen lefelé, túl**bővítmények** , és kattintson rá a Virtuálisgép-bővítmények mentése toobring alább látható módon.

> [!NOTE]
> Ez a funkció csak a hello betekintő portálon támogatott: https://portal.azure.com/
> 
> 

![](media/portal-use-docker/ClickExtensions.png)

### <a name="add-an-extension"></a>Egy bővítmény hozzáadása
Kattintson a hello **+ Hozzáadás** toodisplay hello lehetséges Virtuálisgép-bővítmények toothis VM is hozzáadhat.

![](media/portal-use-docker/ClickAdd.png)

### <a name="select-hello-docker-vm-extension"></a>Válassza ki a hello Docker Virtuálisgép-bővítmény
Válassza ki a hello Docker Virtuálisgép-bővítmény, amely hello Docker leírás és fontos hivatkozásokat, és kattintson a **létrehozása** : hello alsó toobegin hello telepítési eljárást.

![](media/portal-use-docker/ChooseDockerExtension.png)

![](media/portal-use-docker/CreateButtonFocus.png)

### <a name="add-your-certificate-and-key-files"></a>A tanúsítvány és kulcs fájlok hozzáadása:
A hello mezőket adja meg hello base64-kódolású verziói a Hitelesítésszolgáltatói tanúsítvány, a kiszolgálói tanúsítvány és a kiszolgáló kulcs, ahogy az ábra a következő hello.

![](media/portal-use-docker/AddExtensionFormFilled.png)

> [!NOTE]
> Vegye figyelembe, hogy (ahogy kép megelőző hello) hello 2376 ki van töltve alapértelmezés szerint. Bármely végpont Itt adhat meg, de hello tovább tooopen megfelelő végpont hello fel. Hello alapértelmezett módosítása esetén győződjön meg arról, hogy tooopen megfelelő végpont a következő lépésben hello hello fel.
> 
> 

## <a name="add-hello-docker-communication-endpoint"></a>Hello Docker kommunikációs végpont hozzáadása
Korábban létrehozott erőforráscsoportot hello megtekintésekor válassza ki a virtuális Géphez társított hálózati biztonsági csoport hello, majd kattintson **bejövő biztonsági szabályok** tooview hello szabályok itt látható módon.

![](media/portal-use-docker/AddingEndpoint.png)

Kattintson a **+ Hozzáadás** tooadd egy másik szabály, és hello alapértelmezett esetben adja meg a hello végpont nevét (ebben a példában **Docker**), és 2376 "Célporttartomány". Állítsa be a hello protokoll értéket megjelenítő **TCP**, és kattintson a **OK** toocreate hello szabály.

![](media/portal-use-docker/AddEndpointFormFilledOut.png)

## <a name="test-your-docker-client-and-azure-docker-host"></a>A Docker-ügyfél és az Azure Docker állomás tesztelése
Keresse meg és másolja a virtuális gép tartomány, valamint a parancssorból hello az ügyfélszámítógép típus hello neve `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (ahol *dockerextension* hello helyébe altartomány a virtuális gép).

hello eredmény hasonló toothis kell megjelennie:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Ha befejezte a fenti lépéseket hello, most már rendelkezik egy Azure virtuális gépen, konfigurált futtató teljesen működőképes Docker állomás toobe tooremotely csatlakoztatott egyéb ügyfelekről.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
Készen áll a toogo toohello áll [Docker felhasználói útmutató] és a Docker virtuális gép használja. Ha azt szeretné, hogy a Docker-gazdagépekből létrehozása az Azure virtuális gépeken futó parancssori felületen keresztül tooautomate, [hogyan toouse hello hello Azure parancssori felület (CLI) a Virtuálisgép-bővítmény Docker]

<!--Anchors-->
[Create a new VM from hello Image Gallery]:#createvm
[Create Docker Certificates]:#dockercerts
[Add hello Docker VM Extension]:#adddockerextension
[Test Docker Client and Azure Docker Host]:#testclientandserver
[Next steps]:#next-steps

<!--Image references-->
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[StartingPoint]:./media/StartingPoint.png
[6]:./media/markdown-template-for-new-articles/pretty49.png
[7]:./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[hogyan toouse hello hello Azure parancssori felület (CLI) a Virtuálisgép-bővítmény Docker]:http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux ügynök]:../agent-user-guide.md
[Link 3 tooanother azure.microsoft.com documentation topic]:../storage-whatis-account.md

[Docker fut, a https]:http://docs.docker.com/articles/https/
[Docker felhasználói útmutató]:https://docs.docker.com/userguide/
