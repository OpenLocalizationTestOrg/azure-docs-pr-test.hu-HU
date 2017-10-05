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
# <a name="deploy-lamp-stack-with-the-azure-cli-10"></a><span data-ttu-id="225bb-103">LÁMPA verem az Azure CLI 1.0 és központi telepítése</span><span class="sxs-lookup"><span data-stu-id="225bb-103">Deploy LAMP stack with the Azure CLI 1.0</span></span>
<span data-ttu-id="225bb-104">Ez a cikk végigvezeti egy Apache webkiszolgálón, a MySQL és a PHP (a LÁMPA stack) Azure központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="225bb-104">This article walks you through how to deploy an Apache web server, MySQL, and PHP (the LAMP stack) on Azure.</span></span> <span data-ttu-id="225bb-105">Egy Azure-fiókra lesz szüksége ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és a [Azure CLI](../../cli-install-nodejs.md) , amely [az Azure-fiókjával kapcsolódik](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="225bb-105">You need an Azure Account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and the [Azure CLI](../../cli-install-nodejs.md) that is [connected to your Azure account](../../xplat-cli-connect.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="225bb-106">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="225bb-106">CLI versions to complete the task</span></span>
<span data-ttu-id="225bb-107">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="225bb-107">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="225bb-108">[Az azure CLI 1.0] – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="225bb-108">[Azure CLI 1.0] – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="225bb-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="225bb-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* <span data-ttu-id="225bb-110">A meglévő virtuális gép LÁMPA telepítése</span><span class="sxs-lookup"><span data-stu-id="225bb-110">Deploy LAMP on existing VM</span></span>

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="225bb-111">Az új virtuális gép forgatókönyv LÁMPA telepítése</span><span class="sxs-lookup"><span data-stu-id="225bb-111">Deploy LAMP on new VM walkthrough</span></span>
<span data-ttu-id="225bb-112">Hozzon létre elindíthatja egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md) , amely az új virtuális Gépet fogja tartalmazni:</span><span class="sxs-lookup"><span data-stu-id="225bb-112">You can start by creating a [resource group](../../azure-resource-manager/resource-group-overview.md) that will contain the new VM:</span></span>

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

<span data-ttu-id="225bb-113">Hozzon létre a virtuális gépért, használhatja az Azure Resource Manager sablon már megírt található [ide a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="225bb-113">To create the VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

<span data-ttu-id="225bb-114">Néhány további bemenetek értesítése választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="225bb-114">You should see a response prompting some more inputs:</span></span>

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

<span data-ttu-id="225bb-115">Most létrehozott egy Linux virtuális Gépet a LÁMPA már telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="225bb-115">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="225bb-116">Ha kívánja, ellenőrizheti a telepítést le Ugrás [ellenőrizze LÁMPA sikeresen telepítve](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="225bb-116">If you wish, you can verify the install by jumping down to [Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="225bb-117">LÁMPA telepített meglévő virtuális gép forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="225bb-117">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="225bb-118">Ha a Linux virtuális gép létrehozása segítségre van szüksége, látogasson [Itt megtudhatja, hogyan hozzon létre egy Linux virtuális Gépet a](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="225bb-118">If you need help creating a Linux VM, you can head [here to learn how to create a Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="225bb-119">Ezt követően kell SSH a Linux virtuális gép be.</span><span class="sxs-lookup"><span data-stu-id="225bb-119">Next, you need to SSH into the Linux VM.</span></span> <span data-ttu-id="225bb-120">Ha SSH-kulcs létrehozása segítségre van szüksége, látogasson [itt SSH-kulcs létrehozása Linux/Mac hogyan](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="225bb-120">If you need help with creating an SSH key, you can head [here to learn how to create an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="225bb-121">Ha már rendelkezik egy SSH-kulccsal, kísérhet előre és SSH a parancssorból a Linux virtuális Gépet a `ssh exampleUsername@exampleDNS`.</span><span class="sxs-lookup"><span data-stu-id="225bb-121">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh exampleUsername@exampleDNS`.</span></span>

<span data-ttu-id="225bb-122">Most, hogy a Linux virtuális Gépen belül dolgozik, akkor azt is végezze el a LÁMPA verem telepítését Debian-alapú terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="225bb-122">Now that you are working within your Linux VM, we can walk through installing the LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="225bb-123">A pontos parancsok eltérőek lehetnek a más Linux disztribúciókkal.</span><span class="sxs-lookup"><span data-stu-id="225bb-123">The exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="225bb-124">Debian/Ubuntu telepítése</span><span class="sxs-lookup"><span data-stu-id="225bb-124">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="225bb-125">A következő csomagok telepítve van szüksége: `apache2`, `mysql-server`, `php5`, és `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="225bb-125">You need the following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="225bb-126">Közvetlenül grabbing ezeket a csomagokat, vagy használja a Tasksel telepítheti ezeket a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="225bb-126">You can install these packages by directly grabbing these packages or using Tasksel.</span></span> <span data-ttu-id="225bb-127">Mindkét lehetőség utasításokat alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="225bb-127">Instructions for both options are listed below.</span></span>
<span data-ttu-id="225bb-128">Telepítése előtt kell letölteni, és frissítse a csomag listája.</span><span class="sxs-lookup"><span data-stu-id="225bb-128">Before installing you need to download and update package lists.</span></span>

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a><span data-ttu-id="225bb-129">Az egyes csomagok</span><span class="sxs-lookup"><span data-stu-id="225bb-129">Individual packages</span></span>
<span data-ttu-id="225bb-130">Apt get használatával:</span><span class="sxs-lookup"><span data-stu-id="225bb-130">Using apt-get:</span></span>

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a><span data-ttu-id="225bb-131">Tasksel használatával</span><span class="sxs-lookup"><span data-stu-id="225bb-131">Using tasksel</span></span>
<span data-ttu-id="225bb-132">Másik megoldásként letöltheti a Tasksel, a Debian/Ubuntu eszköz, amely több kapcsolódó csomagot feladatként koordinált"", a rendszer telepíti.</span><span class="sxs-lookup"><span data-stu-id="225bb-132">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

<span data-ttu-id="225bb-133">Miután az előző beállítások valamelyikét, kérni fogja ezeket a csomagokat és a különböző más függőségek telepítése.</span><span class="sxs-lookup"><span data-stu-id="225bb-133">After running either of the previous options, you will be prompted to install these packages and various other dependencies.</span></span> <span data-ttu-id="225bb-134">Nyomja meg az "y", majd az "Enter" továbbra is, és állítsa be a rendszergazdai jelszó a MySQL más lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="225bb-134">Press 'y' and then 'Enter' to continue, and follow any other prompts to set an administrative password for MySQL.</span></span> <span data-ttu-id="225bb-135">Ez telepíti a minimálisan szükséges PHP-bővítmények a MySQL PHP használatához szükséges.</span><span class="sxs-lookup"><span data-stu-id="225bb-135">This installs the minimum required PHP extensions needed to use PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="225bb-136">Futtassa a következő parancsot, hogy más PHP-bővítményeket, amelyek csomagként érhető el:</span><span class="sxs-lookup"><span data-stu-id="225bb-136">Run the following command to see other PHP extensions that are available as packages:</span></span>

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a><span data-ttu-id="225bb-137">Info.php dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="225bb-137">Create info.php document</span></span>
<span data-ttu-id="225bb-138">Meg kell tudni ellenőrizni a Apache, a MySQL és a PHP verziójának rendelkezik a parancssor használatával történő beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.</span><span class="sxs-lookup"><span data-stu-id="225bb-138">You should now be able to check what version of Apache, MySQL, and PHP you have through the command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="225bb-139">Ha azt szeretné, további teszteléséhez, létrehozhat egy gyors PHP adatai lap használatával jeleníthetők meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="225bb-139">If you would like to test further, you can create a quick PHP info page to view in a browser.</span></span> <span data-ttu-id="225bb-140">Hozzon létre egy fájl Nano szövegszerkesztőben ezzel a paranccsal:</span><span class="sxs-lookup"><span data-stu-id="225bb-140">Create a file with Nano text editor with this command:</span></span>

    user@ubuntu$ sudo nano /var/www/html/info.php

<span data-ttu-id="225bb-141">GNU Nano szövegszerkesztőben belül adja hozzá a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="225bb-141">Within the GNU Nano text editor, add the following lines:</span></span>

    <?php
    phpinfo();
    ?>

<span data-ttu-id="225bb-142">Ezután mentse, és zárja be a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="225bb-142">Then save and exit the text editor.</span></span>

<span data-ttu-id="225bb-143">Ezért minden új telepítések életbe léptetéséhez indítsa újra a Apache ezzel a paranccsal.</span><span class="sxs-lookup"><span data-stu-id="225bb-143">Restart Apache with this command so all new installs take effect.</span></span>

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="225bb-144">Ellenőrizze a LÁMPA sikeresen telepítve</span><span class="sxs-lookup"><span data-stu-id="225bb-144">Verify LAMP successfully installed</span></span>
<span data-ttu-id="225bb-145">Most már ellenőrizheti, a PHP adatok lapján létrehozott nyisson meg egy böngészőt, és http://youruniqueDNS/info.php címen.</span><span class="sxs-lookup"><span data-stu-id="225bb-145">Now you can check the PHP info page you created by opening a browser and going to http://youruniqueDNS/info.php.</span></span> <span data-ttu-id="225bb-146">Ez a rendszerkép hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="225bb-146">It should look similar to this image.</span></span>

![][2]

<span data-ttu-id="225bb-147">Az Apache telepítése akkor http://youruniqueDNS/ címen a Apache2 Ubuntu alapértelmezett oldal megtekintésével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="225bb-147">You can check your Apache installation by viewing the Apache2 Ubuntu Default Page by going to you http://youruniqueDNS/.</span></span> <span data-ttu-id="225bb-148">Ez a rendszerkép hasonlót meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="225bb-148">You should see something like this image.</span></span>

![][3]

<span data-ttu-id="225bb-149">Gratulálunk, rendelkezik a telepítő csak egy LÁMPA verem az Azure virtuális gép!</span><span class="sxs-lookup"><span data-stu-id="225bb-149">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="225bb-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="225bb-150">Next steps</span></span>
<span data-ttu-id="225bb-151">Tekintse meg a LÁMPA veremben az Ubuntu dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="225bb-151">Check out the Ubuntu documentation on the LAMP stack:</span></span>

* [<span data-ttu-id="225bb-152">https://help.ubuntu.com/Community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="225bb-152">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png