---
title: "a Linux virtuális gépen a hello Azure CLI 1.0 LÁMPA aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello LÁMPA verem a Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jluk
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: NA
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: e78a82d388ce68710933b9b673aa1b2460bdbb14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-with-hello-azure-cli-10"></a>LÁMPA verem hello Azure CLI 1.0 és központi telepítése
Ez a cikk bemutatja, hogyan hogyan toodeploy Apache webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LÁMPA stack) az Azure-on. Egy Azure-fiókra lesz szüksége ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és hello [Azure CLI](../../cli-install-nodejs.md) , amely [tooyour Azure-fiók csatlakoztatva](../../xplat-cli-connect.md).

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az azure CLI 1.0] – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

```
# One command toocreate a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* A meglévő virtuális gép LÁMPA telepítése

```
# Two commands: one updates packages, hello other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Az új virtuális gép forgatókönyv LÁMPA telepítése
Elindíthatja, hozzon létre egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md) fogja tartalmazni, amely új virtuális gép hello:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

toocreate hello virtuális gépért, használja az Azure Resource Manager sablon már megírt található [ide a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Néhány további bemenetek értesítése választ kell megjelennie:

    info:    Executing command group deployment create
    info:    Supply values for hello following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment toocomplete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Most létrehozott egy Linux virtuális Gépet a LÁMPA már telepítve van-e. Ha kívánja, ellenőrizheti átugorja túl hello telepítés[ellenőrizze LÁMPA sikeresen telepítve](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>LÁMPA telepített meglévő virtuális gép forgatókönyv
Ha a Linux virtuális gép létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate Linux virtuális gép](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). A következő lépésben tooSSH hello Linux virtuális gép be. Ha SSH-kulcs létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate egy SSH-kulcsot a Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Ha már rendelkezik egy SSH-kulccsal, kísérhet előre és SSH a parancssorból a Linux virtuális Gépet a `ssh exampleUsername@exampleDNS`.

Most, hogy a Linux virtuális Gépen belül dolgozik, akkor azt is bízná hello LÁMPA verem telepítését Debian-alapú terjesztéseket. az egyéb Linux disztribúciókkal hello pontos parancsok eltérőek lehetnek.

#### <a name="installing-on-debianubuntu"></a>Debian/Ubuntu telepítése
A következő telepített csomagok hello van szüksége: `apache2`, `mysql-server`, `php5`, és `php5-mysql`. Közvetlenül grabbing ezeket a csomagokat, vagy használja a Tasksel telepítheti ezeket a csomagokat. Mindkét lehetőség utasításokat alább láthatók.
Telepítése előtt kell toodownload, és frissítse a csomag listája.

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a>Az egyes csomagok
Apt get használatával:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel használatával
Másik megoldásként letöltheti a Tasksel, a Debian/Ubuntu eszköz, amely több kapcsolódó csomagot feladatként koordinált"", a rendszer telepíti.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Miután hello előző beállítások valamelyikét, fog felszólító tooinstall kell ezeket a csomagokat és a különböző más függőségek. Nyomja meg az "y", majd az "Enter" toocontinue, és hajtsa végre az összes többi kér tooset rendszergazdai jelszó MySQL. A MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP Ezzel telepíti. 

![][1]

Futtassa a következő parancs toosee hello más PHP-bővítményeket, amelyek csomagként érhető el:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php dokumentum létrehozása
Kell tudni toocheck Apache, a MySQL és a PHP verziójának rendelkezik hello parancssor használatával történő beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.

Ha szeretné tootest például további, gyors PHP-információ lapon tooview hozhat létre a böngészőben. Hozzon létre egy fájl Nano szövegszerkesztőben ezzel a paranccsal:

    user@ubuntu$ sudo nano /var/www/html/info.php

Hello GNU Nano szövegszerkesztőben belül adja hozzá az alábbi hello:

    <?php
    phpinfo();
    ?>

Ezután mentse, és zárja be a hello szövegszerkesztőben.

Ezért minden új telepítések életbe léptetéséhez indítsa újra a Apache ezzel a paranccsal.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Ellenőrizze a LÁMPA sikeresen telepítve
Most nyisson meg egy böngészőt, és toohttp://youruniqueDNS/info.php fog létrehozott hello PHP adatok lapján ellenőrizheti. Az alábbihoz hasonló toothis kép.

![][2]

Az Apache telepítése tooyou http://youruniqueDNS/ címen hello Apache2 Ubuntu alapértelmezett oldal megtekintésével ellenőrizheti. Ez a rendszerkép hasonlót meg kell jelennie.

![][3]

Gratulálunk, rendelkezik a telepítő csak egy LÁMPA verem az Azure virtuális gép!

## <a name="next-steps"></a>Következő lépések
Tekintse meg a hello hello LÁMPA veremben Ubuntu dokumentáció:

* [https://help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png