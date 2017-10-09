---
title: "a Linux virtuális gép sínek webhelyen fonetikus aaaHost |} Microsoft Docs"
description: "Állítsa be, és egy Ruby webhelyen sínek-alapú Azure-ban a Linux virtuális gépek üzemeltetéséhez."
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: c545c24fc6c89497854bbe55a6d0d1d0b072c386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>A Ruby on Rails webalkalmazás használata Azure-beli virtuális gépen
Ez az oktatóanyag bemutatja, hogyan toohost egy Ruby sínek webhelyen Azure Linux virtuális gépek használata.  

Ez az oktatóanyag Ubuntu Server 14.04 LTS segítségével lett érvényesítve. Ha egy másik Linux-terjesztés használatához szükség lehet a toomodify hello tooinstall sínek lépések.

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.
>
>

## <a name="create-an-azure-vm"></a>Egy Azure virtuális gép létrehozása
Először hozzon létre egy Azure virtuális gép egy Linux-lemezképhez.

toocreate hello VM, hello Azure-portált használja, vagy hello Azure parancssori felület (CLI).

### <a name="azure-portal"></a>Azure Portal
1. Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com)
2. Kattintson a **új**, hello keresőmezőbe írja be a "Ubuntu Server 14.04". Kattintson a hello keresés által visszaadott hello bejegyzésre. Hello telepítési modell kiválasztása **klasszikus**, majd kattintson a "Létrehozás" gombra.
3. Hello alapvető beállítások panelen adja meg a szükséges hello mezők értékét: nevet (a virtuális gép hello), felhasználónév, hitelesítés típusa és hello megfelelő hitelesítő adatok, Azure-előfizetés, erőforráscsoportot és helyet.

   ![Hozzon létre egy új Ubuntu kép](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. Hello virtuális gép üzembe helyezése után kattintson a hello virtuális gép nevét, majd kattintson **végpontok** a hello **beállítások** kategóriát. Található hello SSH végpont tartozó **önálló**.

   ![Alapértelmezett végpont](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure CLI
Hello kövesse [hozzon létre egy virtuális gép futó Linux][vm-instructions].

Hello virtuális gép üzembe helyezése után is ki lehet hello SSH végpont hello a következő parancs futtatásával:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Ruby sínen telepítése
1. SSH tooconnect toohello virtuális gép használja.
2. Hello SSH-munkamenetet, a parancsok tooinstall Ruby követően a virtuális gép hello hello használata:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > hello brightbox repository contains hello current Ruby distribution.

    hello telepítés néhány percet vehet igénybe. Ha elkészült, használja a következő parancs tooverify, hogy telepítve van-e a Ruby hello:

        ruby -v

3. Használjon hello következő tooinstall sínek parancsot:

        sudo gem install rails --no-rdoc --no-ri -V

    Hello--nem-rdoc és – hello dokumentációját, amely gyorsabb telepítése nem-ri jelzők tooskip használja.
    Ez a parancs valószínűleg tooexecute, így hozzáadása hello -V hello telepítési folyamat kapcsolatos információkat jeleníti meg hosszabb időt vesz igénybe.

## <a name="create-and-run-an-app"></a>Hozzon létre, és az alkalmazás futtatása
Miközben továbbra is jelentkezik SSH-n keresztül, futtassa a következő parancsok hello:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Hello [új](http://guides.rubyonrails.org/command_line.html#rails-new) parancs létrehoz egy új sínek alkalmazást. Hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) indítása hello WEBrick webkiszolgáló sínek a parancsot. (Az éles környezetben való használathoz, akkor érdemes toouse egy másik kiszolgálót, például Unicorn vagy utas.)

Kimeneti hasonló toohello következő kell megjelennie.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C tooshutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Végpont hozzáadása
1. Nyissa meg toohello [Azure portálra] [https://portal.azure.com] válassza ki a virtuális Gépet.

2. Válassza ki **VÉGPONTOK** a hello **beállítások** hello bal oldali szélének hello lap mentén.

3. Kattintson a **hozzáadása** hello oldal hello tetején.

4. A hello **végpont hozzáadása** párbeszédpanel lapján adja meg a következő információ hello:

   * **Név**: HTTP
   * **Protokoll**: TCP
   * **Nyilvános port**: 80
   * **Magánhálózati port**: 3000
   * **Lebegőpontos PI cím**: letiltva
   * **Hozzáférés-vezérlési lista - rendelés**: 1001, vagy egy másik érték, amely beállítja a hozzáférési szabály hello prioritását.
   * **Hozzáférés-vezérlési lista - név**: allowHTTP
   * **Hozzáférés-vezérlési lista - művelet**: engedélyezése
   * **Hozzáférés-vezérlési lista - távoli alhálózati**: 1.0.0.0/16

     Ezt a végpontot, amely átirányítja a forgalmat toohello magánhálózati port az 3000, ahol hello sínek a kiszolgáló figyel a 80-as nyilvános port van. hello ACL-szabályhoz lehetővé teszi, hogy a nyilvános forgalom 80-as porton.

     ![új végpont](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Kattintson az OK toosave hello végpontra.

6. Egy üzenet megjelenjen-e arról, hogy **mentése a virtuális gép végpontjának**. Ez az üzenet megjelenik, amint hello-végpont esetében aktív. Navigáljon a virtuális gép toohello DNS-neve most is tesztelheti az alkalmazást. hello webhely hasonló toohello következő kell megjelennie:

    ![alapértelmezett sínek lap][default-rails-cloud]

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban hasonló hello lépéseket a legtöbb manuálisan. Éles környezetben kívánja írni az alkalmazást a fejlesztési számítógépen, majd központilag telepíti az Azure virtuális gép toohello. A legtöbb éles környezetekben is, hello sínek alkalmazás például az Apache vagy NginX, mely leírókat kérelem hello sínek alkalmazás és a kiszolgáló statikus erőforrások útválasztási toomultiple példánya egy másik kiszolgáló folyamat együtt üzemeltetéséhez. További információkért lásd: http://rubyonrails.org/deploy/.

toolearn Ruby, sínen kapcsolatos további információkért látogasson el a hello [sínek útmutatók a Ruby][rails-guides].

Azure-szolgáltatások toouse Ruby alkalmazás, lásd:

* [Blobok strukturálatlan adatok tárolásához][blobs]
* [Tároló kulcs/érték párok táblák használata][tables]
* [Nagy sávszélesség tartalomszolgáltatás a tartalom Delivery Network hello][cdn-howto]

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
