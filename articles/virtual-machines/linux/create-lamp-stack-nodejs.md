---
title: "Az Azure CLI 1.0 rendelkező Linux virtuális gép üzembe helyezéshez LÁMPA |} Microsoft Docs"
description: "Útmutató a LÁMPA verem telepítése egy Linux virtuális gépre az Azure-ban"
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
ms.openlocfilehash: feba2fb20d1831e92358ff5d1b4c9589d63d28dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-lamp-stack-with-the-azure-cli-10"></a>LÁMPA verem az Azure CLI 1.0 és központi telepítése
Ez a cikk végigvezeti egy Apache webkiszolgálón, a MySQL és a PHP (a LÁMPA stack) Azure központi telepítése. Egy Azure-fiókra lesz szüksége ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és a [Azure CLI](../../cli-install-nodejs.md) , amely [az Azure-fiókjával kapcsolódik](../../xplat-cli-connect.md).

## <a name="cli-versions-to-complete-the-task"></a>A feladat befejezéséhez használható CLI-verziók
A következő CLI-verziók egyikével elvégezheti a feladatot:

- [Az azure CLI 1.0] – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* A meglévő virtuális gép LÁMPA telepítése

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Az új virtuális gép forgatókönyv LÁMPA telepítése
Hozzon létre elindíthatja egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md) , amely az új virtuális Gépet fogja tartalmazni:

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

Hozzon létre a virtuális gépért, használhatja az Azure Resource Manager sablon már megírt található [ide a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Néhány további bemenetek értesítése választ kell megjelennie:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
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

Most létrehozott egy Linux virtuális Gépet a LÁMPA már telepítve van-e. Ha kívánja, ellenőrizheti a telepítést le Ugrás [ellenőrizze LÁMPA sikeresen telepítve](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>LÁMPA telepített meglévő virtuális gép forgatókönyv
Ha a Linux virtuális gép létrehozása segítségre van szüksége, látogasson [Itt megtudhatja, hogyan hozzon létre egy Linux virtuális Gépet a](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ezt követően kell SSH a Linux virtuális gép be. Ha SSH-kulcs létrehozása segítségre van szüksége, látogasson [itt SSH-kulcs létrehozása Linux/Mac hogyan](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Ha már rendelkezik egy SSH-kulccsal, kísérhet előre és SSH a parancssorból a Linux virtuális Gépet a `ssh exampleUsername@exampleDNS`.

Most, hogy a Linux virtuális Gépen belül dolgozik, akkor azt is végezze el a LÁMPA verem telepítését Debian-alapú terjesztéseket. A pontos parancsok eltérőek lehetnek a más Linux disztribúciókkal.

#### <a name="installing-on-debianubuntu"></a>Debian/Ubuntu telepítése
A következő csomagok telepítve van szüksége: `apache2`, `mysql-server`, `php5`, és `php5-mysql`. Közvetlenül grabbing ezeket a csomagokat, vagy használja a Tasksel telepítheti ezeket a csomagokat. Mindkét lehetőség utasításokat alább láthatók.
Telepítése előtt kell letölteni, és frissítse a csomag listája.

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a>Az egyes csomagok
Apt get használatával:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel használatával
Másik megoldásként letöltheti a Tasksel, a Debian/Ubuntu eszköz, amely több kapcsolódó csomagot feladatként koordinált"", a rendszer telepíti.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

Miután az előző beállítások valamelyikét, kérni fogja ezeket a csomagokat és a különböző más függőségek telepítése. Nyomja meg az "y", majd az "Enter" továbbra is, és állítsa be a rendszergazdai jelszó a MySQL más lépéseket követve. Ez telepíti a minimálisan szükséges PHP-bővítmények a MySQL PHP használatához szükséges. 

![][1]

Futtassa a következő parancsot, hogy más PHP-bővítményeket, amelyek csomagként érhető el:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php dokumentum létrehozása
Meg kell tudni ellenőrizni a Apache, a MySQL és a PHP verziójának rendelkezik a parancssor használatával történő beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.

Ha azt szeretné, további teszteléséhez, létrehozhat egy gyors PHP adatai lap használatával jeleníthetők meg a böngészőben. Hozzon létre egy fájl Nano szövegszerkesztőben ezzel a paranccsal:

    user@ubuntu$ sudo nano /var/www/html/info.php

GNU Nano szövegszerkesztőben belül adja hozzá a következő sorokat:

    <?php
    phpinfo();
    ?>

Ezután mentse, és zárja be a szövegszerkesztőben.

Ezért minden új telepítések életbe léptetéséhez indítsa újra a Apache ezzel a paranccsal.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>Ellenőrizze a LÁMPA sikeresen telepítve
Most már ellenőrizheti, a PHP adatok lapján létrehozott nyisson meg egy böngészőt, és http://youruniqueDNS/info.php címen. Ez a rendszerkép hasonlóan kell kinéznie.

![][2]

Az Apache telepítése akkor http://youruniqueDNS/ címen a Apache2 Ubuntu alapértelmezett oldal megtekintésével ellenőrizheti. Ez a rendszerkép hasonlót meg kell jelennie.

![][3]

Gratulálunk, rendelkezik a telepítő csak egy LÁMPA verem az Azure virtuális gép!

## <a name="next-steps"></a>Következő lépések
Tekintse meg a LÁMPA veremben az Ubuntu dokumentáció:

* [https://help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png