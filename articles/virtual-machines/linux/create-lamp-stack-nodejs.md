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
# <a name="deploy-lamp-stack-with-hello-azure-cli-10"></a><span data-ttu-id="9f683-103">LÁMPA verem hello Azure CLI 1.0 és központi telepítése</span><span class="sxs-lookup"><span data-stu-id="9f683-103">Deploy LAMP stack with hello Azure CLI 1.0</span></span>
<span data-ttu-id="9f683-104">Ez a cikk bemutatja, hogyan hogyan toodeploy Apache webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LÁMPA stack) az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="9f683-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on Azure.</span></span> <span data-ttu-id="9f683-105">Egy Azure-fiókra lesz szüksége ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)) és hello [Azure CLI](../../cli-install-nodejs.md) , amely [tooyour Azure-fiók csatlakoztatva](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="9f683-105">You need an Azure Account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)) and hello [Azure CLI](../../cli-install-nodejs.md) that is [connected tooyour Azure account](../../xplat-cli-connect.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="9f683-106">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="9f683-106">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="9f683-107">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="9f683-107">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="9f683-108">[Az azure CLI 1.0] – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="9f683-108">[Azure CLI 1.0] – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="9f683-109">[Az Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="9f683-109">[Azure CLI 2.0](create-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

```
# One command toocreate a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

* <span data-ttu-id="9f683-110">A meglévő virtuális gép LÁMPA telepítése</span><span class="sxs-lookup"><span data-stu-id="9f683-110">Deploy LAMP on existing VM</span></span>

```
# Two commands: one updates packages, hello other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a><span data-ttu-id="9f683-111">Az új virtuális gép forgatókönyv LÁMPA telepítése</span><span class="sxs-lookup"><span data-stu-id="9f683-111">Deploy LAMP on new VM walkthrough</span></span>
<span data-ttu-id="9f683-112">Elindíthatja, hozzon létre egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md) fogja tartalmazni, amely új virtuális gép hello:</span><span class="sxs-lookup"><span data-stu-id="9f683-112">You can start by creating a [resource group](../../azure-resource-manager/resource-group-overview.md) that will contain hello new VM:</span></span>

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

<span data-ttu-id="9f683-113">toocreate hello virtuális gépért, használja az Azure Resource Manager sablon már megírt található [ide a Githubon](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span><span class="sxs-lookup"><span data-stu-id="9f683-113">toocreate hello VM itself, you can use an already written Azure Resource Manager template found [here on GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app).</span></span>

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

<span data-ttu-id="9f683-114">Néhány további bemenetek értesítése választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="9f683-114">You should see a response prompting some more inputs:</span></span>

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

<span data-ttu-id="9f683-115">Most létrehozott egy Linux virtuális Gépet a LÁMPA már telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="9f683-115">You have now created a Linux VM with LAMP already installed on it.</span></span> <span data-ttu-id="9f683-116">Ha kívánja, ellenőrizheti átugorja túl hello telepítés[ellenőrizze LÁMPA sikeresen telepítve](#verify-lamp-successfully-installed).</span><span class="sxs-lookup"><span data-stu-id="9f683-116">If you wish, you can verify hello install by jumping down too[Verify LAMP Successfully Installed](#verify-lamp-successfully-installed).</span></span>

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a><span data-ttu-id="9f683-117">LÁMPA telepített meglévő virtuális gép forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="9f683-117">Deploy LAMP on existing VM walkthrough</span></span>
<span data-ttu-id="9f683-118">Ha a Linux virtuális gép létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate Linux virtuális gép](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9f683-118">If you need help creating a Linux VM, you can head [here toolearn how toocreate a Linux VM](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="9f683-119">A következő lépésben tooSSH hello Linux virtuális gép be.</span><span class="sxs-lookup"><span data-stu-id="9f683-119">Next, you need tooSSH into hello Linux VM.</span></span> <span data-ttu-id="9f683-120">Ha SSH-kulcs létrehozása segítségre van szüksége, látogasson [ide toolearn hogyan toocreate egy SSH-kulcsot a Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9f683-120">If you need help with creating an SSH key, you can head [here toolearn how toocreate an SSH key on Linux/Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
<span data-ttu-id="9f683-121">Ha már rendelkezik egy SSH-kulccsal, kísérhet előre és SSH a parancssorból a Linux virtuális Gépet a `ssh exampleUsername@exampleDNS`.</span><span class="sxs-lookup"><span data-stu-id="9f683-121">If you have an SSH key already, go ahead and SSH from your command line into your Linux VM with `ssh exampleUsername@exampleDNS`.</span></span>

<span data-ttu-id="9f683-122">Most, hogy a Linux virtuális Gépen belül dolgozik, akkor azt is bízná hello LÁMPA verem telepítését Debian-alapú terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="9f683-122">Now that you are working within your Linux VM, we can walk through installing hello LAMP stack on Debian-based distributions.</span></span> <span data-ttu-id="9f683-123">az egyéb Linux disztribúciókkal hello pontos parancsok eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="9f683-123">hello exact commands might differ for other Linux distros.</span></span>

#### <a name="installing-on-debianubuntu"></a><span data-ttu-id="9f683-124">Debian/Ubuntu telepítése</span><span class="sxs-lookup"><span data-stu-id="9f683-124">Installing on Debian/Ubuntu</span></span>
<span data-ttu-id="9f683-125">A következő telepített csomagok hello van szüksége: `apache2`, `mysql-server`, `php5`, és `php5-mysql`.</span><span class="sxs-lookup"><span data-stu-id="9f683-125">You need hello following packages installed: `apache2`, `mysql-server`, `php5`, and `php5-mysql`.</span></span> <span data-ttu-id="9f683-126">Közvetlenül grabbing ezeket a csomagokat, vagy használja a Tasksel telepítheti ezeket a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="9f683-126">You can install these packages by directly grabbing these packages or using Tasksel.</span></span> <span data-ttu-id="9f683-127">Mindkét lehetőség utasításokat alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="9f683-127">Instructions for both options are listed below.</span></span>
<span data-ttu-id="9f683-128">Telepítése előtt kell toodownload, és frissítse a csomag listája.</span><span class="sxs-lookup"><span data-stu-id="9f683-128">Before installing you need toodownload and update package lists.</span></span>

    user@ubuntu$ sudo apt-get update

##### <a name="individual-packages"></a><span data-ttu-id="9f683-129">Az egyes csomagok</span><span class="sxs-lookup"><span data-stu-id="9f683-129">Individual packages</span></span>
<span data-ttu-id="9f683-130">Apt get használatával:</span><span class="sxs-lookup"><span data-stu-id="9f683-130">Using apt-get:</span></span>

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a><span data-ttu-id="9f683-131">Tasksel használatával</span><span class="sxs-lookup"><span data-stu-id="9f683-131">Using tasksel</span></span>
<span data-ttu-id="9f683-132">Másik megoldásként letöltheti a Tasksel, a Debian/Ubuntu eszköz, amely több kapcsolódó csomagot feladatként koordinált"", a rendszer telepíti.</span><span class="sxs-lookup"><span data-stu-id="9f683-132">Alternatively you can download Tasksel, a Debian/Ubuntu tool that installs multiple related packages as a coordinated "task" onto your system.</span></span>

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

<span data-ttu-id="9f683-133">Miután hello előző beállítások valamelyikét, fog felszólító tooinstall kell ezeket a csomagokat és a különböző más függőségek.</span><span class="sxs-lookup"><span data-stu-id="9f683-133">After running either of hello previous options, you will be prompted tooinstall these packages and various other dependencies.</span></span> <span data-ttu-id="9f683-134">Nyomja meg az "y", majd az "Enter" toocontinue, és hajtsa végre az összes többi kér tooset rendszergazdai jelszó MySQL.</span><span class="sxs-lookup"><span data-stu-id="9f683-134">Press 'y' and then 'Enter' toocontinue, and follow any other prompts tooset an administrative password for MySQL.</span></span> <span data-ttu-id="9f683-135">A MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP Ezzel telepíti.</span><span class="sxs-lookup"><span data-stu-id="9f683-135">This installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![][1]

<span data-ttu-id="9f683-136">Futtassa a következő parancs toosee hello más PHP-bővítményeket, amelyek csomagként érhető el:</span><span class="sxs-lookup"><span data-stu-id="9f683-136">Run hello following command toosee other PHP extensions that are available as packages:</span></span>

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a><span data-ttu-id="9f683-137">Info.php dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="9f683-137">Create info.php document</span></span>
<span data-ttu-id="9f683-138">Kell tudni toocheck Apache, a MySQL és a PHP verziójának rendelkezik hello parancssor használatával történő beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.</span><span class="sxs-lookup"><span data-stu-id="9f683-138">You should now be able toocheck what version of Apache, MySQL, and PHP you have through hello command line by typing `apache2 -v`, `mysql -v`, or `php -v`.</span></span>

<span data-ttu-id="9f683-139">Ha szeretné tootest például további, gyors PHP-információ lapon tooview hozhat létre a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="9f683-139">If you would like tootest further, you can create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="9f683-140">Hozzon létre egy fájl Nano szövegszerkesztőben ezzel a paranccsal:</span><span class="sxs-lookup"><span data-stu-id="9f683-140">Create a file with Nano text editor with this command:</span></span>

    user@ubuntu$ sudo nano /var/www/html/info.php

<span data-ttu-id="9f683-141">Hello GNU Nano szövegszerkesztőben belül adja hozzá az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="9f683-141">Within hello GNU Nano text editor, add hello following lines:</span></span>

    <?php
    phpinfo();
    ?>

<span data-ttu-id="9f683-142">Ezután mentse, és zárja be a hello szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="9f683-142">Then save and exit hello text editor.</span></span>

<span data-ttu-id="9f683-143">Ezért minden új telepítések életbe léptetéséhez indítsa újra a Apache ezzel a paranccsal.</span><span class="sxs-lookup"><span data-stu-id="9f683-143">Restart Apache with this command so all new installs take effect.</span></span>

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a><span data-ttu-id="9f683-144">Ellenőrizze a LÁMPA sikeresen telepítve</span><span class="sxs-lookup"><span data-stu-id="9f683-144">Verify LAMP successfully installed</span></span>
<span data-ttu-id="9f683-145">Most nyisson meg egy böngészőt, és toohttp://youruniqueDNS/info.php fog létrehozott hello PHP adatok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="9f683-145">Now you can check hello PHP info page you created by opening a browser and going toohttp://youruniqueDNS/info.php.</span></span> <span data-ttu-id="9f683-146">Az alábbihoz hasonló toothis kép.</span><span class="sxs-lookup"><span data-stu-id="9f683-146">It should look similar toothis image.</span></span>

![][2]

<span data-ttu-id="9f683-147">Az Apache telepítése tooyou http://youruniqueDNS/ címen hello Apache2 Ubuntu alapértelmezett oldal megtekintésével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="9f683-147">You can check your Apache installation by viewing hello Apache2 Ubuntu Default Page by going tooyou http://youruniqueDNS/.</span></span> <span data-ttu-id="9f683-148">Ez a rendszerkép hasonlót meg kell jelennie.</span><span class="sxs-lookup"><span data-stu-id="9f683-148">You should see something like this image.</span></span>

![][3]

<span data-ttu-id="9f683-149">Gratulálunk, rendelkezik a telepítő csak egy LÁMPA verem az Azure virtuális gép!</span><span class="sxs-lookup"><span data-stu-id="9f683-149">Congratulations, you have just setup a LAMP stack on your Azure VM!</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f683-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f683-150">Next steps</span></span>
<span data-ttu-id="9f683-151">Tekintse meg a hello hello LÁMPA veremben Ubuntu dokumentáció:</span><span class="sxs-lookup"><span data-stu-id="9f683-151">Check out hello Ubuntu documentation on hello LAMP stack:</span></span>

* [<span data-ttu-id="9f683-152">https://help.ubuntu.com/Community/ApacheMySQLPHP</span><span class="sxs-lookup"><span data-stu-id="9f683-152">https://help.ubuntu.com/community/ApacheMySQLPHP</span></span>](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/deploy-lamp-stack/apachesuccesspage.png