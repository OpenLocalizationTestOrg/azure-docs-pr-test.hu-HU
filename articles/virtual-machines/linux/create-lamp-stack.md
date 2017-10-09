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
# <a name="deploy-lamp-stack-on-azure"></a><span data-ttu-id="0a8be-103">Az Azure-on LÁMPA verem telepítése</span><span class="sxs-lookup"><span data-stu-id="0a8be-103">Deploy LAMP stack on Azure</span></span>
<span data-ttu-id="0a8be-104">Ez a cikk bemutatja, hogyan hogyan toodeploy Apache webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LÁMPA stack) az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="0a8be-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on Azure.</span></span> <span data-ttu-id="0a8be-105">Egy Azure-fiókra van szüksége ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="0a8be-105">You need an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and hello [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).</span></span> <span data-ttu-id="0a8be-106">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0a8be-106">You can also perform these steps with hello [Azure CLI 1.0](create-lamp-stack-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="0a8be-107">Gyors parancs összefoglaló</span><span class="sxs-lookup"><span data-stu-id="0a8be-107">Quick command summary</span></span>

1. <span data-ttu-id="0a8be-108">Mentse és hello szerkesztése [azuredeploy.parameters.json fájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour beállítás a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0a8be-108">Save and edit hello [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour preference on your local machine.</span></span>
2. <span data-ttu-id="0a8be-109">Futtassa a következő két parancsok toocreate hello egy erőforráscsoport, és majd a sablon telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="0a8be-109">Run hello following two commands toocreate a resource group and then deploy your template:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

### <a name="deploy-lamp-on-existing-vm"></a><span data-ttu-id="0a8be-110">A meglévő virtuális gép LÁMPA telepítése</span><span class="sxs-lookup"><span data-stu-id="0a8be-110">Deploy LAMP on existing VM</span></span>
<span data-ttu-id="0a8be-111">hello következő frissítések csomagok parancsokat, majd Apache, a MySQL és a PHP telepítése:</span><span class="sxs-lookup"><span data-stu-id="0a8be-111">hello following commands updates packages, then installs Apache, MySQL, and PHP:</span></span>

```bash
sudo apt-get update
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="0a8be-112">Az új virtuális gép forgatókönyv LÁMPA telepítése</span><span class="sxs-lookup"><span data-stu-id="0a8be-112">Deploy LAMP on new VM walkthrough</span></span>

1. <span data-ttu-id="0a8be-113">Hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create) toocontain hello új virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="0a8be-113">Create a resource group with [az group create](/cli/azure/group#create) toocontain hello new VM:</span></span>

```azurecli
az group create -l westus -n myResourceGroup
```
<span data-ttu-id="0a8be-114">toocreate hello virtuális gépért, használja az Azure Resource Manager sablon már megírt található [ide a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="0a8be-114">toocreate hello VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

2. <span data-ttu-id="0a8be-115">Mentse a hello [azuredeploy.parameters.json fájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour helyi számítógép.</span><span class="sxs-lookup"><span data-stu-id="0a8be-115">Save hello [azuredeploy.parameters.json file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.parameters.json) tooyour local machine.</span></span>
3. <span data-ttu-id="0a8be-116">Hello szerkesztése **azuredeploy.parameters.json** fájl tooyour előnyben részesített bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="0a8be-116">Edit hello **azuredeploy.parameters.json** file tooyour preferred inputs.</span></span>
4. <span data-ttu-id="0a8be-117">Hello sablon a telepítéséhez [az csoport központi telepítésének létrehozása] hello hivatkozó letöltött json-fájlt:</span><span class="sxs-lookup"><span data-stu-id="0a8be-117">Deploy hello template with [az group deployment create] referencing hello downloaded json file:</span></span>

```azurecli
az group deployment create -g myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json \
    --parameters @filepathToParameters.json
```

<span data-ttu-id="0a8be-118">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="0a8be-118">hello output is similar toohello following example:</span></span>

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

<span data-ttu-id="0a8be-119">Most létrehozott egy Linux virtuális Gépet a LÁMPA már telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="0a8be-119">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="0a8be-120">Ha kívánja, ellenőrizheti átugorja túl hello telepítés[ellenőrizze LÁMPA sikeresen telepítve](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="0a8be-120">If you wish, you can verify hello install by jumping down too[Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="0a8be-121">LÁMPA telepített meglévő virtuális gép forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0a8be-121">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="0a8be-122">Ha a Linux virtuális gép létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate Linux virtuális gép](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span><span class="sxs-lookup"><span data-stu-id="0a8be-122">If you need help creating a Linux VM, you can head [here toolearn how toocreate a Linux VM](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-quick-create-cli).</span></span> <span data-ttu-id="0a8be-123">A következő lépésben tooSSH hello Linux virtuális gép be.</span><span class="sxs-lookup"><span data-stu-id="0a8be-123">Next, you need tooSSH into hello Linux VM.</span></span> <span data-ttu-id="0a8be-124">Ha SSH-kulcs létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate egy SSH-kulcsot a Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0a8be-124">If you need help with creating an SSH key, you can head [here toolearn how toocreate an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="0a8be-125">Ha már rendelkezik egy SSH-kulccsal, kísérhet előre és SSH a parancssorból a Linux virtuális Gépet a `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="0a8be-125">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh azureuser@mypublicdns.westus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="0a8be-126">Most, hogy a Linux virtuális Gépen belül dolgozik, akkor azt is bízná hello LÁMPA verem telepítését Debian-alapú terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="0a8be-126">Now that you are working within your Linux VM, we can walk through installing hello LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="0a8be-127">az egyéb Linux disztribúciókkal hello pontos parancsok eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="0a8be-127">hello exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="0a8be-128">Debian/Ubuntu telepítése</span><span class="sxs-lookup"><span data-stu-id="0a8be-128">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="0a8be-129">A következő telepített csomagok hello van szüksége: `apache2`, `mysql-server`, `php5`, és `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="0a8be-129">You need hello following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="0a8be-130">Közvetlenül grabbing ezeket a csomagokat, vagy használja a Tasksel telepítheti ezeket a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="0a8be-130">You can install these packages by directly grabbing these packages or using Tasksel.</span></span>
<span data-ttu-id="0a8be-131">Telepítése előtt kell toodownload, és frissítse a csomag listája.</span><span class="sxs-lookup"><span data-stu-id="0a8be-131">Before installing you need toodownload and update package lists.</span></span>

```bash
sudo apt-get update
```

##### <a name="individual-packages"></a><span data-ttu-id="0a8be-132">Az egyes csomagok</span><span class="sxs-lookup"><span data-stu-id="0a8be-132">Individual packages</span></span>
<span data-ttu-id="0a8be-133">Apt get használatával:</span><span class="sxs-lookup"><span data-stu-id="0a8be-133">Using apt-get:</span></span>

```bash
sudo apt-get install apache2 mysql-server php5 php5-mysql
```

##### <a name="using-tasksel"></a><span data-ttu-id="0a8be-134">Tasksel használatával</span><span class="sxs-lookup"><span data-stu-id="0a8be-134">Using tasksel</span></span>
<span data-ttu-id="0a8be-135">Másik megoldásként letöltheti a Tasksel, a Debian/Ubuntu eszköz, amely több kapcsolódó csomagot feladatként koordinált"", a rendszer telepíti.</span><span class="sxs-lookup"><span data-stu-id="0a8be-135">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

```bash
sudo apt-get install tasksel
sudo tasksel install lamp-server
```

<span data-ttu-id="0a8be-136">Miután hello előző beállítások valamelyikét, fog felszólító tooinstall kell ezeket a csomagokat és a különböző más függőségek.</span><span class="sxs-lookup"><span data-stu-id="0a8be-136">After running either of hello previous options, you will be prompted tooinstall these packages and various other dependencies.</span></span> <span data-ttu-id="0a8be-137">tooset MySQL, a rendszergazdai jelszó nyomja meg az "y", majd az "Enter" toocontinue, és bármely más lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="0a8be-137">tooset an administrative password for MySQL, press 'y' and then 'Enter' toocontinue, and follow any other prompts.</span></span> <span data-ttu-id="0a8be-138">Ez a folyamat a MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP telepíti.</span><span class="sxs-lookup"><span data-stu-id="0a8be-138">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="0a8be-139">Futtassa a következő parancs toosee hello más PHP-bővítményeket, amelyek csomagként érhető el:</span><span class="sxs-lookup"><span data-stu-id="0a8be-139">Run hello following command toosee other PHP extensions that are available as packages:</span></span>

```bash
apt-cache search php5
```

#### <a name="create-infophp-document"></a><span data-ttu-id="0a8be-140">Info.php dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a8be-140">Create info.php document</span></span>
<span data-ttu-id="0a8be-141">Kell tudni toocheck Apache, a MySQL és a PHP verziójának rendelkezik hello parancssor használatával történő beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.</span><span class="sxs-lookup"><span data-stu-id="0a8be-141">You should now be able toocheck what version of Apache, MySQL, and PHP you have through hello command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="0a8be-142">Ha szeretné tootest például további, gyors PHP-információ lapon tooview hozhat létre a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0a8be-142">If you would like tootest further, you can create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="0a8be-143">Hozzon létre egy fájl Nano szövegszerkesztőben ezzel a paranccsal:</span><span class="sxs-lookup"><span data-stu-id="0a8be-143">Create a file with Nano text editor with this command:</span></span>

```bash
sudo nano /var/www/html/info.php
```

<span data-ttu-id="0a8be-144">Hello GNU Nano szövegszerkesztőben belül adja hozzá az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="0a8be-144">Within hello GNU Nano text editor, add hello following lines:</span></span>

```php
<?php
phpinfo();
?>
```

<span data-ttu-id="0a8be-145">Ezután mentse, és zárja be a hello szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="0a8be-145">Then save and exit hello text editor.</span></span>

<span data-ttu-id="0a8be-146">Ezért minden új telepítések életbe léptetéséhez indítsa újra a Apache ezzel a paranccsal.</span><span class="sxs-lookup"><span data-stu-id="0a8be-146">Restart Apache with this command so all new installs take effect.</span></span>

```bash
sudo service apache2 restart
```

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="0a8be-147">Ellenőrizze a LÁMPA sikeresen telepítve</span><span class="sxs-lookup"><span data-stu-id="0a8be-147">Verify LAMP successfully installed</span></span>
<span data-ttu-id="0a8be-148">Most nyisson meg egy böngészőt, és toohttp://youruniqueDNS/info.php fog létrehozott hello PHP adatok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="0a8be-148">Now you can check hello PHP info page you created by opening a browser and going toohttp://youruniqueDNS/info.php.</span></span> <span data-ttu-id="0a8be-149">Az alábbihoz hasonló toothis kép.</span><span class="sxs-lookup"><span data-stu-id="0a8be-149">It should look similar toothis image.</span></span>

![][2]

<span data-ttu-id="0a8be-150">Az Apache telepítése tooyou http://youruniqueDNS/ címen hello Apache2 Ubuntu alapértelmezett oldal megtekintésével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="0a8be-150">You can check your Apache installation by viewing hello Apache2 Ubuntu Default Page by going tooyou http://youruniqueDNS/.</span></span> <span data-ttu-id="0a8be-151">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="0a8be-151">hello output is similar toohello following example:</span></span>

![][3]

<span data-ttu-id="0a8be-152">Gratulálunk, rendelkezik a telepítő csak egy LÁMPA verem az Azure virtuális gép!</span><span class="sxs-lookup"><span data-stu-id="0a8be-152">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a8be-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a8be-153">Next steps</span></span>
<span data-ttu-id="0a8be-154">Tekintse meg a hello hello LÁMPA veremben Ubuntu dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="0a8be-154">Check out hello Ubuntu documentation on hello LAMP stack:</span></span>

* [<span data-ttu-id="0a8be-155">https://help.ubuntu.com/Community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="0a8be-155">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png
