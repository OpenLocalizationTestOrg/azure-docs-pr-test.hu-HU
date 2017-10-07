---
title: "aaaUse hello CustomScript bővítménnyel a Linux virtuális gép |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello CustomScript bővítmény toodeploy alkalmazások a Linux virtuális gépek Azure-ban hello klasszikus telepítési modellel készült-e."
editor: tysonn
manager: timlt
documentationcenter: 
services: virtual-machines-linux
author: gbowerman
tags: azure-service-management
ms.assetid: e535241d-feca-4412-b07a-67c936ba88a0
ms.service: virtual-machines-linux
ms.workload: multiple
ms.tgt_pltfrm: linux
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: guybo
ms.openlocfilehash: 864a586e70093eefbabc065a3c05e1cf9e315704
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-lamp-app-using-hello-azure-customscript-extension-for-linux"></a>Hello Azure CustomScript bővítménnyel használata Linux LÁMPA alkalmazás telepítése
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Egy LÁMPA verem hello Resource Manager modellt használó központi telepítésével kapcsolatos információkért lásd: [Itt](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

a Microsoft Azure CustomScript bővítménnyel Linux hello a virtuális gépek (VM) biztosít egy módon toocustomize bármilyen hello (például Python és Bash) virtuális gép által támogatott programozási nyelven írt tetszőleges kód futtatásával. Így lehetővé teszi a rendkívül rugalmasan tooautomate alkalmazás központi telepítési toomultiple gépek.

Hello CustomScript bővítménnyel telepítése Azure-portálon, a Windows PowerShell vagy a hello Azure parancssori felület (CLI) használatával hello.

Ebben a cikkben fogjuk használni a hello Azure CLI toodeploy egy egyszerű LÁMPA alkalmazás tooan Ubuntu virtuális gép hello klasszikus telepítési modellel készült.

## <a name="prerequisites"></a>Előfeltételek
Ehhez a példához először létre kell hoznia két Azure virtuális gépeken futó Ubuntu 14.04 vagy újabb. hello virtuális gépek nevezzük *parancsfájl-vm* és *lámpa-vm*. Használjon egyedi nevek hello virtuális gépek létrehozásakor. Egyik használt toorun hello parancssori felület parancsait, egy használt toodeploy hello LÁMPA alkalmazást.

Egy Azure Storage-fiókot és egy kulcs tooaccess is szüksége informatikai (letölthető ez hello Azure-portál).

Ha segítségre van szüksége az Azure-on Linux virtuális gépek létrehozása tekintse meg a túl[hozzon létre egy virtuális gép futó Linux](createportal.md).

hello telepítési parancsok használata Ubuntu, de minden támogatott Linux distro hello telepítési módosíthatja.

hello parancsfájl-virtuális gép virtuális gép van szüksége az Azure parancssori felület telepítve, és egy működő kapcsolat tooAzure toohave. A túl tekintse meg a Súgó[telepítése és konfigurálása az Azure parancssori felület hello](../../../cli-install-nodejs.md).

## <a name="upload-a-script"></a>A parancsfájl feltöltése
Azt lesz hello CustomScript bővítménnyel toorun parancsfájl használata a távoli virtuális gép tooinstall hello LÁMPA veremben, és hozzon létre egy PHP lap. Rendelés tooaccess hello parancsfájlban bárhonnan azt fogja feltölteni az, az Azure blob.

### <a name="script-overview"></a>Parancsfájl áttekintése
hello mintaparancsfájl egy LÁMPA verem tooUbuntu (beleértve a MySQL csendes telepítéshez beállítása) telepíti, egy egyszerű PHP-fájlt ír, és Apache elindul.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install hello LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Töltse fel a parancsfájl
Hello parancsfájl elmentse egy szövegfájlt, például *install_lamp.sh*, és a tárolási tooAzure feltöltése. Ehhez egyszerűen az Azure parancssori felület. hello alábbi példa fájlfeltöltések hello fájl tooa tároló "parancsfájlok" nevű. Ha hello tároló nem létezik toocreate lesz szüksége az első.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

A JSON-fájl, amely leírja, hogyan toodownload hello parancsfájl Azure Storage-ból is létrehozhat. Mentse ezt *public_config.json* (cseréje a "mystorage" hello nevet, a tárfiók):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-hello-extension"></a>Hello bővítmény telepítése
Mostantól a hello tovább parancs toodeploy hello Linux CustomScript bővítménnyel toohello távoli VM használatával hello Azure parancssori felület.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

hello előző parancs letölti és futtatja a hello *install_lamp.sh* parancsfájl a virtuális gép nevű hello *lámpa-vm*.

Mivel hello alkalmazást tartalmaz egy webkiszolgálón, ne feledje tooopen HTTP figyelőportja hello a távoli virtuális gép a következő paranccsal hello.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Figyelés és hibaelhárítás
Akkor is ellenőrizhesse mennyire hello egyéni parancsfájl futtatása hello naplófájl felmérésével hello távoli virtuális gép. SSH túl*lámpa-vm* és kiegészítő hello naplófájlt a következő paranccsal hello.

    cd /var/log/azure/customscript
    tail -f handler.log

Miután lefuttatta a hello CustomScript bővítménnyel, tallózással toohello PHP lap információt létrehozott. hello PHP lapon, az ebben a cikkben hello például *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>További források
Használhatja ugyanazon alapvető lépéseket toodeploy hello összetettebb alkalmazásokat. Ebben a példában a hello telepítési parancsfájl az Azure Storage nyilvános blob néven lett mentve. A biztonságosabb beállítás toostore hello telepítési parancsfájl lenne, a biztonságos blob egy [biztonságos hozzáférési aláírás](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).

További erőforrások az Azure parancssori felület, a Linux és a hello CustomScript bővítménnyel a következő találhatók.

[CustomScript bővítménnyel Linux virtuális gép testreszabása feladatok automatizálásához](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Az Azure Linux Extensions (GitHub)](https://github.com/Azure/azure-linux-extensions)