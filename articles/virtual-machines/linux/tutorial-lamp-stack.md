---
title: "Linux virtuális gépre az Azure-ban LÁMPA aaaDeploy |} Microsoft Docs"
description: "Az oktatóanyag - telepítés hello LÁMPA verem a Linux virtuális gép az Azure-ban"
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
ms.openlocfilehash: a3d0ecb3277f15bd0a2fdc0d85b738a760e68865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a><span data-ttu-id="0baca-103">Egy Azure virtuális gépen LÁMPA webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="0baca-103">Install a LAMP web server on an Azure VM</span></span>
<span data-ttu-id="0baca-104">Ez a cikk bemutatja, hogyan hogyan toodeploy Apache webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LÁMPA stack) az Ubuntu virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0baca-104">This article walks you through how toodeploy an Apache web server, MySQL, and PHP (hello LAMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="0baca-105">Ha jobban szeret hello NGINX webkiszolgáló, lásd: hello [LEMP verem](tutorial-lemp-stack.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="0baca-105">If you prefer hello NGINX web server, see hello [LEMP stack](tutorial-lemp-stack.md) tutorial.</span></span> <span data-ttu-id="0baca-106">toosee hello LÁMPA server műveletben, igény szerint telepítheti és egy WordPress-webhely konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0baca-106">toosee hello LAMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="0baca-107">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="0baca-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0baca-108">Hozzon létre egy Ubuntu virtuális gép (hello "L" hello LÁMPA verem)</span><span class="sxs-lookup"><span data-stu-id="0baca-108">Create an Ubuntu VM (hello 'L' in hello LAMP stack)</span></span>
> * <span data-ttu-id="0baca-109">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="0baca-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="0baca-110">Apache, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="0baca-110">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="0baca-111">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="0baca-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="0baca-112">WordPress telepítése hello LÁMPA kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="0baca-112">Install WordPress on hello LAMP server</span></span>


<span data-ttu-id="0baca-113">Hello LÁMPA verem, beleértve az éles környezetbe, ajánlásokat bővebben lásd: hello [Ubuntu dokumentáció](https://help.ubuntu.com/community/ApacheMySQLPHP).</span><span class="sxs-lookup"><span data-stu-id="0baca-113">For more on hello LAMP stack, including recommendations for a production environment, see hello [Ubuntu documentation](https://help.ubuntu.com/community/ApacheMySQLPHP).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="0baca-114">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="0baca-114">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="0baca-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="0baca-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="0baca-116">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0baca-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a><span data-ttu-id="0baca-117">Apache, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="0baca-117">Install Apache, MySQL, and PHP</span></span>

<span data-ttu-id="0baca-118">Futtassa a következő parancs tooupdate Ubuntu csomag források hello és Apache, a MySQL és a PHP telepítése.</span><span class="sxs-lookup"><span data-stu-id="0baca-118">Run hello following command tooupdate Ubuntu package sources and install Apache, MySQL, and PHP.</span></span> <span data-ttu-id="0baca-119">Vegye figyelembe a hello beszúrási jel (^) hello parancs hello végén.</span><span class="sxs-lookup"><span data-stu-id="0baca-119">Note hello caret (^) at hello end of hello command.</span></span>


```bash
sudo apt update && sudo apt install lamp-server^
```



<span data-ttu-id="0baca-120">Biztos, hogy felszólító tooinstall hello csomagok és más függőségek.</span><span class="sxs-lookup"><span data-stu-id="0baca-120">You are prompted tooinstall hello packages and other dependencies.</span></span> <span data-ttu-id="0baca-121">Amikor a rendszer kéri, egy gyökérszintű jelszót beállítani a MySQL, majd [Enter] toocontinue.</span><span class="sxs-lookup"><span data-stu-id="0baca-121">When prompted, set a root password for MySQL, and then [Enter] toocontinue.</span></span> <span data-ttu-id="0baca-122">Kövesse a hátralévő utasításokat hello.</span><span class="sxs-lookup"><span data-stu-id="0baca-122">Follow hello remaining prompts.</span></span> <span data-ttu-id="0baca-123">Ez a folyamat a MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP telepíti.</span><span class="sxs-lookup"><span data-stu-id="0baca-123">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![MySQL legfelső szintű jelszófrissítési lap][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="0baca-125">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="0baca-125">Verify installation and configuration</span></span>


### <a name="apache"></a><span data-ttu-id="0baca-126">Apache</span><span class="sxs-lookup"><span data-stu-id="0baca-126">Apache</span></span>

<span data-ttu-id="0baca-127">Apache hello verziója egyeztetni hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0baca-127">Check hello version of Apache with hello following command:</span></span>
```bash
apache2 -v
```

<span data-ttu-id="0baca-128">Az Apache telepített, és a port 80 nyissa meg a tooyour VM, hello webkiszolgáló most elérhető hello az interneten.</span><span class="sxs-lookup"><span data-stu-id="0baca-128">With Apache installed, and port 80 open tooyour VM, hello web server can now be accessed from hello internet.</span></span> <span data-ttu-id="0baca-129">tooview hello Apache2 Ubuntu alapértelmezett oldal, nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0baca-129">tooview hello Apache2 Ubuntu Default Page, open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="0baca-130">Hello tooSSH toohello VM használt nyilvános IP-cím használata:</span><span class="sxs-lookup"><span data-stu-id="0baca-130">Use hello public IP address you used tooSSH toohello VM:</span></span>

![Apache alapértelmezett oldal][3]


### <a name="mysql"></a><span data-ttu-id="0baca-132">MySQL</span><span class="sxs-lookup"><span data-stu-id="0baca-132">MySQL</span></span>

<span data-ttu-id="0baca-133">MySQL hello verziója kérje meg a következő parancs hello (vegye figyelembe a hello beruházási `V` paraméter):</span><span class="sxs-lookup"><span data-stu-id="0baca-133">Check hello version of MySQL with hello following command (note hello capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="0baca-134">Azt javasoljuk, hogy fut a következő parancsfájl toohelp biztonságos hello telepítésének MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="0baca-134">We recommend running hello following script toohelp secure hello installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="0baca-135">Adja meg a MySQL gyökér szintű jelszavát, és a környezet hello biztonsági beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0baca-135">Enter your MySQL root password, and configure hello security settings for your environment.</span></span>

<span data-ttu-id="0baca-136">Ha azt szeretné, hogy a MySQL-adatbázis toocreate, adja hozzá a felhasználókat, vagy módosítsa a konfigurációs beállításokat, bejelentkezési tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="0baca-136">If you want toocreate a MySQL database, add users, or change configuration settings, login tooMySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="0baca-137">Ha befejezte, lépjen ki hello mysql parancssorba írja be a `\q`.</span><span class="sxs-lookup"><span data-stu-id="0baca-137">When done, exit hello mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="0baca-138">PHP</span><span class="sxs-lookup"><span data-stu-id="0baca-138">PHP</span></span>

<span data-ttu-id="0baca-139">Hello verzió a PHP egyeztetni hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0baca-139">Check hello version of PHP with hello following command:</span></span>

```bash
php -v
```

<span data-ttu-id="0baca-140">Ha azt szeretné, hogy további tootest, hozzon létre egy gyors PHP adatai lap tooview a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="0baca-140">If you want tootest further, create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="0baca-141">a következő parancs hello hello PHP adatok lapján hoz létre:</span><span class="sxs-lookup"><span data-stu-id="0baca-141">hello following command creates hello PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

<span data-ttu-id="0baca-142">Most létrehozott hello PHP adatok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="0baca-142">Now you can check hello PHP info page you created.</span></span> <span data-ttu-id="0baca-143">Nyisson meg egy böngészőt, és nyissa meg túl`http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="0baca-143">Open a browser and go too`http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="0baca-144">Helyettesítse be a virtuális gép hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0baca-144">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="0baca-145">Az alábbihoz hasonló toothis kép.</span><span class="sxs-lookup"><span data-stu-id="0baca-145">It should look similar toothis image.</span></span>

![A PHP adatai lap][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a><span data-ttu-id="0baca-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0baca-147">Next steps</span></span>

<span data-ttu-id="0baca-148">Ebben az oktatóanyagban az Azure-ban LÁMPA kiszolgáló telepítette.</span><span class="sxs-lookup"><span data-stu-id="0baca-148">In this tutorial, you deployed a LAMP server in Azure.</span></span> <span data-ttu-id="0baca-149">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="0baca-149">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0baca-150">Ubuntu virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0baca-150">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="0baca-151">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="0baca-151">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="0baca-152">Apache, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="0baca-152">Install Apache, MySQL, and PHP</span></span>
> * <span data-ttu-id="0baca-153">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="0baca-153">Verify installation and configuration</span></span>
> * <span data-ttu-id="0baca-154">WordPress telepítése hello LÁMPA kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="0baca-154">Install WordPress on hello LAMP server</span></span>

<span data-ttu-id="0baca-155">Hogyan előzetes toohello következő útmutató toolearn toosecure webkiszolgálók SSL-tanúsítványokkal.</span><span class="sxs-lookup"><span data-stu-id="0baca-155">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0baca-156">Biztonságos webkiszolgáló SSL</span><span class="sxs-lookup"><span data-stu-id="0baca-156">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png