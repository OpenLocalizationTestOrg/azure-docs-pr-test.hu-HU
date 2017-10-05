---
title: "LÁMPA telepítése egy Linux virtuális gép az Azure-ban |} Microsoft Docs"
description: "Az oktatóanyag - telepítés a LÁMPA verem a Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: 9148ac9646e4e1cfeff8f20c096e390499437e78
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a><span data-ttu-id="fb75c-103">Egy Azure virtuális gépen LÁMPA webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="fb75c-103">Install a LAMP web server on an Azure VM</span></span>
<span data-ttu-id="fb75c-104">Ez a cikk végigvezeti egy Apache webkiszolgálón, a MySQL és a PHP (a LÁMPA stack) az Azure-ban Ubuntu virtuális gép telepítése.</span><span class="sxs-lookup"><span data-stu-id="fb75c-104">This article walks you through how to deploy an Apache web server, MySQL, and PHP (the LAMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="fb75c-105">Ha inkább a NGINX webkiszolgáló, tekintse meg a [LEMP verem](tutorial-lemp-stack.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fb75c-105">If you prefer the NGINX web server, see the [LEMP stack](tutorial-lemp-stack.md) tutorial.</span></span> <span data-ttu-id="fb75c-106">A művelet a LÁMPA kiszolgáló megtekintéséhez opcionálisan is telepítése és konfigurálása egy WordPress-webhely.</span><span class="sxs-lookup"><span data-stu-id="fb75c-106">To see the LAMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="fb75c-107">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="fb75c-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fb75c-108">Hozzon létre egy Ubuntu virtuális gép (a "L" a LÁMPA verem)</span><span class="sxs-lookup"><span data-stu-id="fb75c-108">Create an Ubuntu VM (the 'L' in the LAMP stack)</span></span>
> * <span data-ttu-id="fb75c-109">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="fb75c-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="fb75c-110">Apache, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="fb75c-110">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="fb75c-111">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="fb75c-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="fb75c-112">WordPress telepítése a LÁMPA kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="fb75c-112">Install WordPress on the LAMP server</span></span>


<span data-ttu-id="fb75c-113">A LÁMPA verem, beleértve az éles környezetbe, ajánlásokat bővebben lásd: a [Ubuntu dokumentáció](https://help.ubuntu.com/community/ApacheMySQLPHP).</span><span class="sxs-lookup"><span data-stu-id="fb75c-113">For more on the LAMP stack, including recommendations for a production environment, see the [Ubuntu documentation](https://help.ubuntu.com/community/ApacheMySQLPHP).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="fb75c-114">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fb75c-114">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="fb75c-115">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="fb75c-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="fb75c-116">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="fb75c-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a><span data-ttu-id="fb75c-117">Apache, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="fb75c-117">Install Apache, MySQL, and PHP</span></span>

<span data-ttu-id="fb75c-118">A következő parancsot Ubuntu csomag adatforrások frissítése, és Apache, a MySQL és a PHP telepítése.</span><span class="sxs-lookup"><span data-stu-id="fb75c-118">Run the following command to update Ubuntu package sources and install Apache, MySQL, and PHP.</span></span> <span data-ttu-id="fb75c-119">Megjegyzés: a parancs végén a beszúrási jel (^).</span><span class="sxs-lookup"><span data-stu-id="fb75c-119">Note the caret (^) at the end of the command.</span></span>


```bash
sudo apt update && sudo apt install lamp-server^
```



<span data-ttu-id="fb75c-120">A csomagok és más függőségek telepítése kéri.</span><span class="sxs-lookup"><span data-stu-id="fb75c-120">You are prompted to install the packages and other dependencies.</span></span> <span data-ttu-id="fb75c-121">Amikor a rendszer kéri, egy gyökérszintű jelszót beállítása MySQL, majd [Enter] folytatja.</span><span class="sxs-lookup"><span data-stu-id="fb75c-121">When prompted, set a root password for MySQL, and then [Enter] to continue.</span></span> <span data-ttu-id="fb75c-122">Kövesse a hátralévő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="fb75c-122">Follow the remaining prompts.</span></span> <span data-ttu-id="fb75c-123">Ez a folyamat telepíti a minimálisan szükséges PHP-bővítmények a MySQL PHP használatához szükséges.</span><span class="sxs-lookup"><span data-stu-id="fb75c-123">This process installs the minimum required PHP extensions needed to use PHP with MySQL.</span></span> 

![MySQL legfelső szintű jelszófrissítési lap][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="fb75c-125">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="fb75c-125">Verify installation and configuration</span></span>


### <a name="apache"></a><span data-ttu-id="fb75c-126">Apache</span><span class="sxs-lookup"><span data-stu-id="fb75c-126">Apache</span></span>

<span data-ttu-id="fb75c-127">A következő paranccsal Apache verziójának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="fb75c-127">Check the version of Apache with the following command:</span></span>
```bash
apache2 -v
```

<span data-ttu-id="fb75c-128">Az Apache telepítve, és nyissa meg a virtuális géphez a 80-as porton a webkiszolgáló most már elérhető az internetről.</span><span class="sxs-lookup"><span data-stu-id="fb75c-128">With Apache installed, and port 80 open to your VM, the web server can now be accessed from the internet.</span></span> <span data-ttu-id="fb75c-129">A Apache2 Ubuntu alapértelmezett lapnak a megtekintésére, nyisson meg egy webböngészőt, és adja meg a virtuális gép nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="fb75c-129">To view the Apache2 Ubuntu Default Page, open a web browser, and enter the public IP address of the VM.</span></span> <span data-ttu-id="fb75c-130">Az SSH-kapcsolatot a virtuális gép használt nyilvános IP-cím használata:</span><span class="sxs-lookup"><span data-stu-id="fb75c-130">Use the public IP address you used to SSH to the VM:</span></span>

![Apache alapértelmezett oldal][3]


### <a name="mysql"></a><span data-ttu-id="fb75c-132">MySQL</span><span class="sxs-lookup"><span data-stu-id="fb75c-132">MySQL</span></span>

<span data-ttu-id="fb75c-133">A következő paranccsal MySQL verziójának ellenőrzése (vegye figyelembe a beruházási `V` paraméter):</span><span class="sxs-lookup"><span data-stu-id="fb75c-133">Check the version of MySQL with the following command (note the capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="fb75c-134">Azt javasoljuk, hogy biztonságossá MySQL telepítése a következő parancsprogram futtatása:</span><span class="sxs-lookup"><span data-stu-id="fb75c-134">We recommend running the following script to help secure the installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="fb75c-135">Adja meg a MySQL gyökér szintű jelszavát, majd adja meg a környezet biztonsági beállításait.</span><span class="sxs-lookup"><span data-stu-id="fb75c-135">Enter your MySQL root password, and configure the security settings for your environment.</span></span>

<span data-ttu-id="fb75c-136">Ha azt szeretné, MySQL-adatbázis létrehozásához, adja hozzá a felhasználókat, vagy módosítsa a konfigurációs beállításokat, a MySQL-bejelentkezési:</span><span class="sxs-lookup"><span data-stu-id="fb75c-136">If you want to create a MySQL database, add users, or change configuration settings, login to MySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="fb75c-137">Ha befejezte, lépjen ki a mysql-parancssorba írja be a `\q`.</span><span class="sxs-lookup"><span data-stu-id="fb75c-137">When done, exit the mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="fb75c-138">PHP</span><span class="sxs-lookup"><span data-stu-id="fb75c-138">PHP</span></span>

<span data-ttu-id="fb75c-139">Ellenőrizze a PHP verzióját a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="fb75c-139">Check the version of PHP with the following command:</span></span>

```bash
php -v
```

<span data-ttu-id="fb75c-140">Ha azt szeretné, további teszteléséhez, hozzon létre egy gyors PHP adatai lap használatával jeleníthetők meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="fb75c-140">If you want to test further, create a quick PHP info page to view in a browser.</span></span> <span data-ttu-id="fb75c-141">A következő parancs létrehozza a PHP-adatok lapján:</span><span class="sxs-lookup"><span data-stu-id="fb75c-141">The following command creates the PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

<span data-ttu-id="fb75c-142">Most már a létrehozott PHP adatok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="fb75c-142">Now you can check the PHP info page you created.</span></span> <span data-ttu-id="fb75c-143">Nyisson meg egy böngészőt, és navigáljon `http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="fb75c-143">Open a browser and go to `http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="fb75c-144">Helyettesítse be a virtuális gép nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="fb75c-144">Substitute the public IP address of your VM.</span></span> <span data-ttu-id="fb75c-145">Ez a rendszerkép hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="fb75c-145">It should look similar to this image.</span></span>

![A PHP adatai lap][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a><span data-ttu-id="fb75c-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb75c-147">Next steps</span></span>

<span data-ttu-id="fb75c-148">Ebben az oktatóanyagban az Azure-ban LÁMPA kiszolgáló telepítette.</span><span class="sxs-lookup"><span data-stu-id="fb75c-148">In this tutorial, you deployed a LAMP server in Azure.</span></span> <span data-ttu-id="fb75c-149">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="fb75c-149">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fb75c-150">Ubuntu virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb75c-150">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="fb75c-151">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="fb75c-151">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="fb75c-152">Apache, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="fb75c-152">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="fb75c-153">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="fb75c-153">Verify installation and configuration</span></span>
> * <span data-ttu-id="fb75c-154">WordPress telepítése a LÁMPA kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="fb75c-154">Install WordPress on the LAMP server</span></span>

<span data-ttu-id="fb75c-155">A következő oktatóanyag megtudhatja, mennyire biztonságos SSL-tanúsítványokkal webkiszolgálók továbblépés.</span><span class="sxs-lookup"><span data-stu-id="fb75c-155">Advance to the next tutorial to learn how to secure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="fb75c-156">Biztonságos webkiszolgáló SSL</span><span class="sxs-lookup"><span data-stu-id="fb75c-156">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png