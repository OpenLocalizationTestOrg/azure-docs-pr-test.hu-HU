---
title: "Linux virtuális gépre az Azure-ban LEMP aaaDeploy |} Microsoft Docs"
description: "Az oktatóanyag - telepítés hello LEMP verem a Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: d8f9d84c5e9c0df4e9e985c10fe10f63a2f88214
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a><span data-ttu-id="a6b44-103">Egy Azure virtuális gépen LEMP webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="a6b44-103">Install a LEMP web server on an Azure VM</span></span>
<span data-ttu-id="a6b44-104">Ez a cikk bemutatja, hogyan hogyan toodeploy egy NGINX webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LEMP stack) az Ubuntu virtuális gép az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a6b44-104">This article walks you through how toodeploy an NGINX web server, MySQL, and PHP (hello LEMP stack) on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="a6b44-105">hello LEMP verem egy alternatív toohello népszerű [LÁMPA verem](tutorial-lamp-stack.md), amely az Azure-ban is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="a6b44-105">hello LEMP stack is an alternative toohello popular [LAMP stack](tutorial-lamp-stack.md), which you can also install in Azure.</span></span> <span data-ttu-id="a6b44-106">toosee hello LEMP server műveletben, igény szerint telepítheti és egy WordPress-webhely konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a6b44-106">toosee hello LEMP server in action, you can optionally install and configure a WordPress site.</span></span> <span data-ttu-id="a6b44-107">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="a6b44-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6b44-108">Hozzon létre egy Ubuntu virtuális gép (hello "L" hello LEMP verem)</span><span class="sxs-lookup"><span data-stu-id="a6b44-108">Create an Ubuntu VM (hello 'L' in hello LEMP stack)</span></span>
> * <span data-ttu-id="a6b44-109">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="a6b44-109">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="a6b44-110">NGINX, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="a6b44-110">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="a6b44-111">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="a6b44-111">Verify installation and configuration</span></span>
> * <span data-ttu-id="a6b44-112">WordPress telepítése hello LEMP kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="a6b44-112">Install WordPress on hello LEMP server</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a6b44-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="a6b44-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="a6b44-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="a6b44-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a6b44-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a6b44-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a><span data-ttu-id="a6b44-116">NGINX, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="a6b44-116">Install NGINX, MySQL, and PHP</span></span>

<span data-ttu-id="a6b44-117">Futtassa a következő parancs tooupdate Ubuntu csomag források hello és NGINX, a MySQL és a PHP telepítése.</span><span class="sxs-lookup"><span data-stu-id="a6b44-117">Run hello following command tooupdate Ubuntu package sources and install NGINX, MySQL, and PHP.</span></span> 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

<span data-ttu-id="a6b44-118">Biztos, hogy felszólító tooinstall hello csomagok és más függőségek.</span><span class="sxs-lookup"><span data-stu-id="a6b44-118">You are prompted tooinstall hello packages and other dependencies.</span></span> <span data-ttu-id="a6b44-119">Amikor a rendszer kéri, egy gyökérszintű jelszót beállítani a MySQL, majd [Enter] toocontinue.</span><span class="sxs-lookup"><span data-stu-id="a6b44-119">When prompted, set a root password for MySQL, and then [Enter] toocontinue.</span></span> <span data-ttu-id="a6b44-120">Kövesse a hátralévő utasításokat hello.</span><span class="sxs-lookup"><span data-stu-id="a6b44-120">Follow hello remaining prompts.</span></span> <span data-ttu-id="a6b44-121">Ez a folyamat a MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP telepíti.</span><span class="sxs-lookup"><span data-stu-id="a6b44-121">This process installs hello minimum required PHP extensions needed toouse PHP with MySQL.</span></span> 

![MySQL legfelső szintű jelszófrissítési lap][1]

## <a name="verify-installation-and-configuration"></a><span data-ttu-id="a6b44-123">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="a6b44-123">Verify installation and configuration</span></span>


### <a name="nginx"></a><span data-ttu-id="a6b44-124">NGINX</span><span class="sxs-lookup"><span data-stu-id="a6b44-124">NGINX</span></span>

<span data-ttu-id="a6b44-125">Hello verziójának NGINX egyeztetni hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a6b44-125">Check hello version of NGINX with hello following command:</span></span>
```bash
nginx -v
```

<span data-ttu-id="a6b44-126">Az NGINX telepített, és a port 80 nyissa meg a tooyour VM, hello webkiszolgáló most elérhető hello az interneten.</span><span class="sxs-lookup"><span data-stu-id="a6b44-126">With NGINX installed, and port 80 open tooyour VM, hello web server can now be accessed from hello internet.</span></span> <span data-ttu-id="a6b44-127">tooview hello NGINX kezdőlapján nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="a6b44-127">tooview hello NGINX welcome page, open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="a6b44-128">Hello tooSSH toohello VM használt nyilvános IP-cím használata:</span><span class="sxs-lookup"><span data-stu-id="a6b44-128">Use hello public IP address you used tooSSH toohello VM:</span></span>

![NGINX alapértelmezett oldal][3]


### <a name="mysql"></a><span data-ttu-id="a6b44-130">MySQL</span><span class="sxs-lookup"><span data-stu-id="a6b44-130">MySQL</span></span>

<span data-ttu-id="a6b44-131">MySQL hello verziója kérje meg a következő parancs hello (vegye figyelembe a hello beruházási `V` paraméter):</span><span class="sxs-lookup"><span data-stu-id="a6b44-131">Check hello version of MySQL with hello following command (note hello capital `V` parameter):</span></span>

```bash
msql -V
```

<span data-ttu-id="a6b44-132">Azt javasoljuk, hogy fut a következő parancsfájl toohelp biztonságos hello telepítésének MySQL hello:</span><span class="sxs-lookup"><span data-stu-id="a6b44-132">We recommend running hello following script toohelp secure hello installation of MySQL:</span></span>

```bash
mysql_secure_installation
```

<span data-ttu-id="a6b44-133">Adja meg a MySQL gyökér szintű jelszavát, és a környezet hello biztonsági beállításainak konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a6b44-133">Enter your MySQL root password, and configure hello security settings for your environment.</span></span>

<span data-ttu-id="a6b44-134">Ha azt szeretné, hogy a MySQL-adatbázis toocreate, adja hozzá a felhasználókat, vagy módosítsa a konfigurációs beállításokat, bejelentkezési tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="a6b44-134">If you want toocreate a MySQL database, add users, or change configuration settings, login tooMySQL:</span></span>

```bash
mysql -u root -p
```

<span data-ttu-id="a6b44-135">Ha befejezte, lépjen ki hello mysql parancssorba írja be a `\q`.</span><span class="sxs-lookup"><span data-stu-id="a6b44-135">When done, exit hello mysql prompt by typing `\q`.</span></span>

### <a name="php"></a><span data-ttu-id="a6b44-136">PHP</span><span class="sxs-lookup"><span data-stu-id="a6b44-136">PHP</span></span>

<span data-ttu-id="a6b44-137">Hello verzió a PHP egyeztetni hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a6b44-137">Check hello version of PHP with hello following command:</span></span>

```bash
php -v
```

<span data-ttu-id="a6b44-138">NGINX toouse hello PHP FastCGI folyamat-kezelő (PHP-FPM) konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a6b44-138">Configure NGINX toouse hello PHP FastCGI Process Manager (PHP-FPM).</span></span> <span data-ttu-id="a6b44-139">Futtassa a következő parancsok tooback hello eredeti NGINX kiszolgáló konfigurációs fájlt, és szerkessze a hello eredeti fájlt valamelyik szerkesztőben az Ön által választott hello:</span><span class="sxs-lookup"><span data-stu-id="a6b44-139">Run hello following commands tooback up hello original NGINX server block config file and then edit hello original file in an editor of your choice:</span></span>

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

<span data-ttu-id="a6b44-140">Hello szerkesztő, cserélje le a hello tartalmát `/etc/nginx/sites-available/default` hello következőre.</span><span class="sxs-lookup"><span data-stu-id="a6b44-140">In hello editor, replace hello contents of `/etc/nginx/sites-available/default` with hello following.</span></span> <span data-ttu-id="a6b44-141">Lásd: hello megjegyzések hello beállításainak ismertetése.</span><span class="sxs-lookup"><span data-stu-id="a6b44-141">See hello comments for explanation of hello settings.</span></span> <span data-ttu-id="a6b44-142">Helyettesítse be a virtuális Gépet a hello nyilvános IP-címe *yourPublicIPAddress*, és a fennmaradó beállításokat hello hagyja.</span><span class="sxs-lookup"><span data-stu-id="a6b44-142">Substitute hello public IP address of your VM for *yourPublicIPAddress*, and leave hello remaining settings.</span></span> <span data-ttu-id="a6b44-143">Mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="a6b44-143">Then save hello file.</span></span>

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

<span data-ttu-id="a6b44-144">Ellenőrizze a hello NGINX konfigurációját szintaktikai hibák:</span><span class="sxs-lookup"><span data-stu-id="a6b44-144">Check hello NGINX configuration for syntax errors:</span></span>

```bash
sudo nginx -t
```

<span data-ttu-id="a6b44-145">Ha hello szintaxisának helyességét, indítsa újra a NGINX a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="a6b44-145">If hello syntax is correct, restart NGINX with hello following command:</span></span>

```bash
sudo service nginx restart
```

<span data-ttu-id="a6b44-146">Ha azt szeretné, hogy további tootest, hozzon létre egy gyors PHP adatai lap tooview a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="a6b44-146">If you want tootest further, create a quick PHP info page tooview in a browser.</span></span> <span data-ttu-id="a6b44-147">a következő parancs hello hello PHP adatok lapján hoz létre:</span><span class="sxs-lookup"><span data-stu-id="a6b44-147">hello following command creates hello PHP info page:</span></span>

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



<span data-ttu-id="a6b44-148">Most létrehozott hello PHP adatok lapján ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="a6b44-148">Now you can check hello PHP info page you created.</span></span> <span data-ttu-id="a6b44-149">Nyisson meg egy böngészőt, és nyissa meg túl`http://yourPublicIPAddress/info.php`.</span><span class="sxs-lookup"><span data-stu-id="a6b44-149">Open a browser and go too`http://yourPublicIPAddress/info.php`.</span></span> <span data-ttu-id="a6b44-150">Helyettesítse be a virtuális gép hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="a6b44-150">Substitute hello public IP address of your VM.</span></span> <span data-ttu-id="a6b44-151">Az alábbihoz hasonló toothis kép.</span><span class="sxs-lookup"><span data-stu-id="a6b44-151">It should look similar toothis image.</span></span>

![A PHP adatai lap][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a><span data-ttu-id="a6b44-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6b44-153">Next steps</span></span>

<span data-ttu-id="a6b44-154">Ebben az oktatóanyagban az Azure-ban LEMP kiszolgáló telepítette.</span><span class="sxs-lookup"><span data-stu-id="a6b44-154">In this tutorial, you deployed a LEMP server in Azure.</span></span> <span data-ttu-id="a6b44-155">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="a6b44-155">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6b44-156">Ubuntu virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6b44-156">Create an Ubuntu VM</span></span>
> * <span data-ttu-id="a6b44-157">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="a6b44-157">Open port 80 for web traffic</span></span>
> * <span data-ttu-id="a6b44-158">NGINX, a MySQL és a PHP telepítése</span><span class="sxs-lookup"><span data-stu-id="a6b44-158">Install NGINX, MySQL, and PHP</span></span>
> * <span data-ttu-id="a6b44-159">Ellenőrizze a telepítés és konfigurálás</span><span class="sxs-lookup"><span data-stu-id="a6b44-159">Verify installation and configuration</span></span>
> * <span data-ttu-id="a6b44-160">Hello LEMP veremben WordPress telepítése</span><span class="sxs-lookup"><span data-stu-id="a6b44-160">Install WordPress on hello LEMP stack</span></span>

<span data-ttu-id="a6b44-161">Hogyan előzetes toohello következő útmutató toolearn toosecure webkiszolgálók SSL-tanúsítványokkal.</span><span class="sxs-lookup"><span data-stu-id="a6b44-161">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a6b44-162">Biztonságos webkiszolgáló SSL</span><span class="sxs-lookup"><span data-stu-id="a6b44-162">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
