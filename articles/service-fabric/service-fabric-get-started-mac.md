---
title: "a Mac OS X toowork az Azure Service Fabric a fejlesztési környezet létrehozása aaaSet |} Microsoft Docs"
description: "Hello futtatókörnyezet, az SDK és az eszközök telepítése, és hozzon létre egy helyi fejlesztési fürtöt. A telepítés befejezése után készen áll a toobuild alkalmazások Mac OS x lesz."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/21/2017
ms.author: saysa
ms.openlocfilehash: 0b8a6c1fc1871fa76f3e21cefbc7f66f79072797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>A fejlesztési környezet beállítása Mac OS X-en
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

A Service Fabric-alkalmazások toorun Linux fürtökön Mac OS X használatával hozhat létre. Ez a cikk ismerteti hogyan tooset be a fejlesztési Mac.

## <a name="prerequisites"></a>Előfeltételek
A Service Fabric nem futtatható natív módon OS x rendszerekhez toorun egy helyi Service Fabric-fürt, előre konfigurált Ubuntu virtuális gép Vagrant és VirtualBox nyújtunk. A kezdés előtt a következőkre lesz szüksége:

* [Vagrant (1.8.4-es vagy újabb verzió)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Vagrant és VirtualBox toouse kölcsönösen támogatott verziója van szüksége. A Vagrant kiszámíthatatlan módon viselkedhet egy nem támogatott VirtualBox-verzión.
>

## <a name="create-hello-local-vm"></a>Hozzon létre helyi virtuális gép hello
toocreate hello 5-csomópont Service Fabric-fürt tartalmazó helyi virtuális gép, hajtsa végre az alábbi lépésekkel hello:

1. Klónozás hello `Vagrantfile` tárház

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Ez a lépés kapcsolása időszakosan megszakadó hello fájl `Vagrantfile` hello VM tartalmazó konfigurációs hello hely hello VM együtt letölti.

2. Keresse meg a helyi klónját toohello hello tárház

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Választható) Hello alapértelmezett virtuális gép beállításainak módosítása

    Alapértelmezés szerint hello helyi virtuális gép van konfigurálva az alábbiak szerint:

   * 3 GB lefoglalt memória
   * IP-címmel 192.168.50.50 konfigurált titkos állomás hálózati forgalom hello Mac gazdagépről csatlakoztatott engedélyezése

     Módosítsa az ezeket a beállításokat, vagy adja hozzá a többi konfigurációs toohello VM hello `Vagrantfile`. Lásd: hello [Vagrant dokumentáció](http://www.vagrantup.com/docs) hello a konfigurációs beállítások teljes listáját.
4. Hello virtuális gép létrehozása

    ```bash
    vagrant up
    ```

   Ez a lépés hello előre konfigurált Virtuálisgép-lemezkép, helyileg azt, majd állítsa be egy helyi Service Fabric-fürt azt rendszerindító tölti le. Várható azt tootake néhány perc múlva. Ha a telepítés sikeresen befejeződött, megjelenik egy üzenet hello kimenet jelző hello fürthöz indul el.

    ![A fürt beállításának megkezdése a virtuális gép kiépítése után][cluster-setup-script]

    >[!TIP]
    > Ha hello VM letöltési hosszú ideig tart, letöltheti a wget vagy a curl vagy toohello kapcsolat által meghatározott útvonalon böngészőn keresztül **config.vm.box_url** hello fájlban `Vagrantfile`. Helyi letöltése, után szerkeszteni `Vagrantfile` toopoint toohello helyi elérési út hello kép kezelőportálon. Például ha letöltötte a hello kép too/home/users/test/azureservicefabric.tp8.box, majd állítsa be a **config.vm.box_url** toothat elérési útja.
    >

5. Tesztelje, hogy hello fürt beállítása megfelelően http://192.168.50.50:19080/Explorer (feltéve, hogy hello alapértelmezett magánhálózati IP meghagyta) a Fabric Explorer tooService útvonalon.

    ![Service Fabric Explorer hello állomás Mac helyei][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Alkalmazás létrehozása Mac gépen a Yeoman használatával
A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyek segítségével Service Fabric-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával. Kérjük, kövesse alább hello Service Fabric yeoman sablon generátor működik-e a számítógépen van tooensure hello lépéseket.

1. Toohave Node.js és NPM, mac gépen telepítve van szükség. Ha nem a Node.js és NPM hello alábbi Homebrew használatával is telepíthet. a Node.js és a Mac gépen telepítve NPM toocheck hello verzióban használhatja hello ``-v`` lehetőséget.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből

  ```bash
  npm install -g yo
  ```
3. Hello Yeoman telepítése generátor kívánt toouse, első lépések hello hello lépései [dokumentáció](service-fabric-get-started-linux.md). toocreate Service Fabric-alkalmazások használatával Yeoman, lépésekkel hello-

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. Service Fabric Java-alkalmazások Mac toobuild, kellene - JDK 1.8 és a gradle-lel hello számítógépen telepítve.


## <a name="install-hello-service-fabric-plugin-for-eclipse-neon"></a>Az Eclipse Neonfény hello Service Fabric beépülő modul telepítése

A Service Fabric egy beépülő modul biztosít hello **Eclipse Neonfény Java ide** , egyszerűsítheti hello történő létrehozása, kiépítési és Java-szolgáltatások telepítése. Ez általában említett hello telepítési lépések végrehajtásával [dokumentáció](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) telepítéséről és a Service Fabric Eclipse beépülő modul frissítését.

>[!TIP]
> Alapértelmezés szerint támogatott hello alapértelmezett IP a hello ``Vagrantfile`` a hello ``Local.json`` hello létrehozott alkalmazás. Módosíthatja, hogy és központi telepítése egy másik IP-Vagrant, frissítse hello megfelelő IP-Címmel ``Local.json`` , valamint az alkalmazás.

## <a name="next-steps"></a>Következő lépések
<!-- Links -->
* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával](service-fabric-create-your-first-linux-application-with-java.md)
* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával](service-fabric-get-started-eclipse.md)
* [A Service Fabric-fürt létrehozása a hello Azure-portálon](service-fabric-cluster-creation-via-portal.md)
* [Azure Resource Manager hello segítségével a Service Fabric-fürt létrehozása](service-fabric-cluster-creation-via-arm.md)
* [A Service Fabric-alkalmazás modell hello ismertetése](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
