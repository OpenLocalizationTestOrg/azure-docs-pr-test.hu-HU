---
title: "Linux virtuális gépre az Azure-ban LÁMPA aaaDeploy |} Microsoft Docs"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 2/21/2017
ms.author: juluk
ms.openlocfilehash: 42d887bb9f78becc02505e336be25fdaaf78df70
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-lamp-stack-on-azure"></a>Az Azure-on LÁMPA verem telepítése
Ez a cikk bemutatja, hogyan hogyan toodeploy Apache webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LÁMPA stack) az Azure-on. Egy Azure-fiókra van szüksége ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2). Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-command-summary"></a>Gyors parancs összefoglaló

1. Mentse és hello szerkesztése [azuredeploy.parameters.json fájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour beállítás a helyi számítógépen.
2. Futtassa a következő két parancsok toocreate hello egy erőforráscsoport, és majd a sablon telepítéséhez:

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a>A meglévő virtuális gép LÁMPA telepítése
hello következő frissítések csomagok parancsokat, majd Apache, a MySQL és a PHP telepítése:

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Az új virtuális gép forgatókönyv LÁMPA telepítése

1. Hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create) toocontain hello új virtuális Gépet:

```azurecli
az group create -l westus -n myResourceGroup
```
toocreate hello virtuális gépért, használja az Azure Resource Manager sablon már megírt található [ide a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).

2. Mentse a hello [azuredeploy.parameters.json fájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour helyi számítógép.
3. Hello szerkesztése **azuredeploy.parameters.json** fájl tooyour előnyben részesített bemeneti adatok.
4. Hello sablon a telepítéséhez [az csoport központi telepítésének létrehozása] hello hivatkozó letöltött json-fájlt:

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

hello hasonló toohello a következő példa a kimenetre:

```json
{
"id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Resources/deployments/azuredeploy",
"name": "azuredeploy",
"properties": {
    "correlationId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "debugSetting": null,
}
...
"provisioningState": "Succeeded",
"template": null,
"templateLink": {
    "contentVersion": "1.0.0.0",
    "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json"
    },
    "timestamp": "2017-02-22T00:05:51.860411+00:00"
},
"resourceGroup": "myResourceGroup"
}
```

Most létrehozott egy Linux virtuális Gépet a LÁMPA már telepítve van-e. Ha kívánja, ellenőrizheti átugorja túl hello telepítés[ellenőrizze LÁMPA sikeresen telepítve](#verify-lamp-successfully-installed).

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>LÁMPA telepített meglévő virtuális gép forgatókönyv
Ha a Linux virtuális gép létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate Linux virtuális gép](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli). A következő lépésben tooSSH hello Linux virtuális gép be. Ha SSH-kulcs létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate egy SSH-kulcsot a Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Ha már rendelkezik egy SSH-kulccsal, kísérhet előre és SSH a parancssorból a Linux virtuális Gépet a `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.

Most, hogy a Linux virtuális Gépen belül dolgozik, akkor azt is bízná hello LÁMPA verem telepítését Debian-alapú terjesztéseket. az egyéb Linux disztribúciókkal hello pontos parancsok eltérőek lehetnek.

#### <a name="installing-on-debianubuntu"></a>Debian/Ubuntu telepítése
A következő telepített csomagok hello van szüksége: `apache2`, `mysql-server`, `php5`, és `php5-mysql`. Közvetlenül grabbing ezeket a csomagokat, vagy használja a Tasksel telepítheti ezeket a csomagokat.
Telepítése előtt kell toodownload, és frissítse a csomag listája.

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a>Az egyes csomagok
Apt get használatával:

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a>Tasksel használatával
Másik megoldásként letöltheti a Tasksel, a Debian/Ubuntu eszköz, amely több kapcsolódó csomagot feladatként koordinált"", a rendszer telepíti.

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

Miután hello előző beállítások valamelyikét, fog felszólító tooinstall kell ezeket a csomagokat és a különböző más függőségek. tooset MySQL, a rendszergazdai jelszó nyomja meg az "y", majd az "Enter" toocontinue, és bármely más lépéseket követve. Ez a folyamat a MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP telepíti. 

![][1]

Futtassa a következő parancs toosee hello más PHP-bővítményeket, amelyek csomagként érhető el:

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a>Info.php dokumentum létrehozása
Kell tudni toocheck Apache, a MySQL és a PHP verziójának rendelkezik hello parancssor használatával történő beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.

Ha szeretné tootest például további, gyors PHP-információ lapon tooview hozhat létre a böngészőben. Hozzon létre egy fájl Nano szövegszerkesztőben ezzel a paranccsal:

```bash
sudo nano /var/www/html/info.php
```

Hello GNU Nano szövegszerkesztőben belül adja hozzá az alábbi hello:

```php
<?php
phpinfo();
?>
```

Ezután mentse, és zárja be a hello szövegszerkesztőben.

Ezért minden új telepítések életbe léptetéséhez indítsa újra a Apache ezzel a paranccsal.

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a>Ellenőrizze a LÁMPA sikeresen telepítve
Most nyisson meg egy böngészőt, és toohttp://youruniqueDNS/info.php fog létrehozott hello PHP adatok lapján ellenőrizheti. Az alábbihoz hasonló toothis kép.

![][2]

Az Apache telepítése tooyou http://youruniqueDNS/ címen hello Apache2 Ubuntu alapértelmezett oldal megtekintésével ellenőrizheti. hello hasonló toohello a következő példa a kimenetre:

![][3]

Gratulálunk, rendelkezik a telepítő csak egy LÁMPA verem az Azure virtuális gép!

## <a name="next-steps"></a>Következő lépések
Tekintse meg a hello hello LÁMPA veremben Ubuntu dokumentáció:

* [https://help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
