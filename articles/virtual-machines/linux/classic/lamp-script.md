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
# <a name="deploy-a-lamp-app-using-hello-azure-customscript-extension-for-linux"></a><span data-ttu-id="92c46-103">Hello Azure CustomScript bővítménnyel használata Linux LÁMPA alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="92c46-103">Deploy a LAMP app using hello Azure CustomScript Extension for Linux</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="92c46-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="92c46-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="92c46-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="92c46-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="92c46-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="92c46-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="92c46-107">Egy LÁMPA verem hello Resource Manager modellt használó központi telepítésével kapcsolatos információkért lásd: [Itt](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="92c46-107">For information about deploying a LAMP stack using hello Resource Manager model, see [here](../tutorial-lamp-stack.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="92c46-108">a Microsoft Azure CustomScript bővítménnyel Linux hello a virtuális gépek (VM) biztosít egy módon toocustomize bármilyen hello (például Python és Bash) virtuális gép által támogatott programozási nyelven írt tetszőleges kód futtatásával.</span><span class="sxs-lookup"><span data-stu-id="92c46-108">hello Microsoft Azure CustomScript Extension for Linux provides a way toocustomize your virtual machines (VMs) by running arbitrary code written in any scripting language supported by hello VM (for example, Python, and Bash).</span></span> <span data-ttu-id="92c46-109">Így lehetővé teszi a rendkívül rugalmasan tooautomate alkalmazás központi telepítési toomultiple gépek.</span><span class="sxs-lookup"><span data-stu-id="92c46-109">This provides a very flexible way tooautomate application deployment toomultiple machines.</span></span>

<span data-ttu-id="92c46-110">Hello CustomScript bővítménnyel telepítése Azure-portálon, a Windows PowerShell vagy a hello Azure parancssori felület (CLI) használatával hello.</span><span class="sxs-lookup"><span data-stu-id="92c46-110">You can deploy hello CustomScript Extension using hello Azure portal, Windows PowerShell, or hello Azure Command-Line Interface (Azure CLI).</span></span>

<span data-ttu-id="92c46-111">Ebben a cikkben fogjuk használni a hello Azure CLI toodeploy egy egyszerű LÁMPA alkalmazás tooan Ubuntu virtuális gép hello klasszikus telepítési modellel készült.</span><span class="sxs-lookup"><span data-stu-id="92c46-111">In this article we'll use hello Azure CLI toodeploy a simple LAMP application tooan Ubuntu VM created using hello classic deployment model.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92c46-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="92c46-112">Prerequisites</span></span>
<span data-ttu-id="92c46-113">Ehhez a példához először létre kell hoznia két Azure virtuális gépeken futó Ubuntu 14.04 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="92c46-113">For this example, first create two Azure VMs running Ubuntu 14.04 or later.</span></span> <span data-ttu-id="92c46-114">hello virtuális gépek nevezzük *parancsfájl-vm* és *lámpa-vm*.</span><span class="sxs-lookup"><span data-stu-id="92c46-114">hello VMs are called *script-vm* and *lamp-vm*.</span></span> <span data-ttu-id="92c46-115">Használjon egyedi nevek hello virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="92c46-115">Use unique names when you create hello VMs.</span></span> <span data-ttu-id="92c46-116">Egyik használt toorun hello parancssori felület parancsait, egy használt toodeploy hello LÁMPA alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="92c46-116">One is used toorun hello CLI commands and one is used toodeploy hello LAMP app.</span></span>

<span data-ttu-id="92c46-117">Egy Azure Storage-fiókot és egy kulcs tooaccess is szüksége informatikai (letölthető ez hello Azure-portál).</span><span class="sxs-lookup"><span data-stu-id="92c46-117">You also need an Azure Storage account and a key tooaccess it (you can get this from hello Azure portal).</span></span>

<span data-ttu-id="92c46-118">Ha segítségre van szüksége az Azure-on Linux virtuális gépek létrehozása tekintse meg a túl[hozzon létre egy virtuális gép futó Linux](createportal.md).</span><span class="sxs-lookup"><span data-stu-id="92c46-118">If you need help creating Linux VMs on Azure refer too[Create a Virtual Machine Running Linux](createportal.md).</span></span>

<span data-ttu-id="92c46-119">hello telepítési parancsok használata Ubuntu, de minden támogatott Linux distro hello telepítési módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="92c46-119">hello install commands assume Ubuntu, but you can adapt hello installation for any supported Linux distro.</span></span>

<span data-ttu-id="92c46-120">hello parancsfájl-virtuális gép virtuális gép van szüksége az Azure parancssori felület telepítve, és egy működő kapcsolat tooAzure toohave.</span><span class="sxs-lookup"><span data-stu-id="92c46-120">hello script-vm VM needs toohave Azure CLI installed, with a working connection tooAzure.</span></span> <span data-ttu-id="92c46-121">A túl tekintse meg a Súgó[telepítése és konfigurálása az Azure parancssori felület hello](../../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="92c46-121">For help with this refer too[Install and Configure hello Azure Command-Line Interface](../../../cli-install-nodejs.md).</span></span>

## <a name="upload-a-script"></a><span data-ttu-id="92c46-122">A parancsfájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="92c46-122">Upload a script</span></span>
<span data-ttu-id="92c46-123">Azt lesz hello CustomScript bővítménnyel toorun parancsfájl használata a távoli virtuális gép tooinstall hello LÁMPA veremben, és hozzon létre egy PHP lap.</span><span class="sxs-lookup"><span data-stu-id="92c46-123">We'll use hello CustomScript Extension toorun a script on a remote VM tooinstall hello LAMP stack and create a PHP page.</span></span> <span data-ttu-id="92c46-124">Rendelés tooaccess hello parancsfájlban bárhonnan azt fogja feltölteni az, az Azure blob.</span><span class="sxs-lookup"><span data-stu-id="92c46-124">In order tooaccess hello script from anywhere we'll upload it as an Azure blob.</span></span>

### <a name="script-overview"></a><span data-ttu-id="92c46-125">Parancsfájl áttekintése</span><span class="sxs-lookup"><span data-stu-id="92c46-125">Script overview</span></span>
<span data-ttu-id="92c46-126">hello mintaparancsfájl egy LÁMPA verem tooUbuntu (beleértve a MySQL csendes telepítéshez beállítása) telepíti, egy egyszerű PHP-fájlt ír, és Apache elindul.</span><span class="sxs-lookup"><span data-stu-id="92c46-126">hello script example installs a LAMP stack tooUbuntu (including setting up a silent install of MySQL), writes a simple PHP file, and starts Apache.</span></span>

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

### <a name="upload-script"></a><span data-ttu-id="92c46-127">Töltse fel a parancsfájl</span><span class="sxs-lookup"><span data-stu-id="92c46-127">Upload script</span></span>
<span data-ttu-id="92c46-128">Hello parancsfájl elmentse egy szövegfájlt, például *install_lamp.sh*, és a tárolási tooAzure feltöltése.</span><span class="sxs-lookup"><span data-stu-id="92c46-128">Save hello script as a text file, for example *install_lamp.sh*, and then upload it tooAzure Storage.</span></span> <span data-ttu-id="92c46-129">Ehhez egyszerűen az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="92c46-129">You can do this easily with Azure CLI.</span></span> <span data-ttu-id="92c46-130">hello alábbi példa fájlfeltöltések hello fájl tooa tároló "parancsfájlok" nevű.</span><span class="sxs-lookup"><span data-stu-id="92c46-130">hello following example uploads hello file tooa storage container named "scripts".</span></span> <span data-ttu-id="92c46-131">Ha hello tároló nem létezik toocreate lesz szüksége az első.</span><span class="sxs-lookup"><span data-stu-id="92c46-131">If hello container doesn't exist you'll need toocreate it first.</span></span>

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

<span data-ttu-id="92c46-132">A JSON-fájl, amely leírja, hogyan toodownload hello parancsfájl Azure Storage-ból is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="92c46-132">Also create a JSON file that describes how toodownload hello script from Azure Storage.</span></span> <span data-ttu-id="92c46-133">Mentse ezt *public_config.json* (cseréje a "mystorage" hello nevet, a tárfiók):</span><span class="sxs-lookup"><span data-stu-id="92c46-133">Save this as *public_config.json* (replacing "mystorage" with hello name of your storage account):</span></span>

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-hello-extension"></a><span data-ttu-id="92c46-134">Hello bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="92c46-134">Deploy hello extension</span></span>
<span data-ttu-id="92c46-135">Mostantól a hello tovább parancs toodeploy hello Linux CustomScript bővítménnyel toohello távoli VM használatával hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="92c46-135">Now you can use hello next command toodeploy hello Linux CustomScript Extension toohello remote VM using hello Azure CLI.</span></span>

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

<span data-ttu-id="92c46-136">hello előző parancs letölti és futtatja a hello *install_lamp.sh* parancsfájl a virtuális gép nevű hello *lámpa-vm*.</span><span class="sxs-lookup"><span data-stu-id="92c46-136">hello previous command downloads and runs hello *install_lamp.sh* script on hello VM called *lamp-vm*.</span></span>

<span data-ttu-id="92c46-137">Mivel hello alkalmazást tartalmaz egy webkiszolgálón, ne feledje tooopen HTTP figyelőportja hello a távoli virtuális gép a következő paranccsal hello.</span><span class="sxs-lookup"><span data-stu-id="92c46-137">Since hello app includes a web server, remember tooopen an HTTP listening port on hello remote VM with hello next command.</span></span>

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a><span data-ttu-id="92c46-138">Figyelés és hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="92c46-138">Monitoring and troubleshooting</span></span>
<span data-ttu-id="92c46-139">Akkor is ellenőrizhesse mennyire hello egyéni parancsfájl futtatása hello naplófájl felmérésével hello távoli virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="92c46-139">You can check on how well hello custom script runs by looking at hello log file on hello remote VM.</span></span> <span data-ttu-id="92c46-140">SSH túl*lámpa-vm* és kiegészítő hello naplófájlt a következő paranccsal hello.</span><span class="sxs-lookup"><span data-stu-id="92c46-140">SSH too*lamp-vm* and tail hello log file with hello next command.</span></span>

    cd /var/log/azure/customscript
    tail -f handler.log

<span data-ttu-id="92c46-141">Miután lefuttatta a hello CustomScript bővítménnyel, tallózással toohello PHP lap információt létrehozott.</span><span class="sxs-lookup"><span data-stu-id="92c46-141">After you run hello CustomScript Extension, you can browse toohello PHP page you created for information.</span></span> <span data-ttu-id="92c46-142">hello PHP lapon, az ebben a cikkben hello például *http://lamp-vm.cloudapp.net/phpinfo.php*.</span><span class="sxs-lookup"><span data-stu-id="92c46-142">hello PHP page for hello example in this article is *http://lamp-vm.cloudapp.net/phpinfo.php*.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92c46-143">További források</span><span class="sxs-lookup"><span data-stu-id="92c46-143">Additional resources</span></span>
<span data-ttu-id="92c46-144">Használhatja ugyanazon alapvető lépéseket toodeploy hello összetettebb alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="92c46-144">You can use hello same basic steps toodeploy more complex apps.</span></span> <span data-ttu-id="92c46-145">Ebben a példában a hello telepítési parancsfájl az Azure Storage nyilvános blob néven lett mentve.</span><span class="sxs-lookup"><span data-stu-id="92c46-145">In this example hello install script was saved as a public blob in Azure Storage.</span></span> <span data-ttu-id="92c46-146">A biztonságosabb beállítás toostore hello telepítési parancsfájl lenne, a biztonságos blob egy [biztonságos hozzáférési aláírás](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span><span class="sxs-lookup"><span data-stu-id="92c46-146">A more secure option would be toostore hello install script as a secure blob with a [Secure Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx) (SAS).</span></span>

<span data-ttu-id="92c46-147">További erőforrások az Azure parancssori felület, a Linux és a hello CustomScript bővítménnyel a következő találhatók.</span><span class="sxs-lookup"><span data-stu-id="92c46-147">Additional resources for Azure CLI, Linux and hello CustomScript Extension are listed next.</span></span>

[<span data-ttu-id="92c46-148">CustomScript bővítménnyel Linux virtuális gép testreszabása feladatok automatizálásához</span><span class="sxs-lookup"><span data-stu-id="92c46-148">Automate Linux VM Customization Tasks Using CustomScript Extension</span></span>](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[<span data-ttu-id="92c46-149">Az Azure Linux Extensions (GitHub)</span><span class="sxs-lookup"><span data-stu-id="92c46-149">Azure Linux Extensions (GitHub)</span></span>](https://github.com/Azure/azure-linux-extensions)