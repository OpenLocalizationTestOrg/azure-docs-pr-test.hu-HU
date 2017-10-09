---
title: "mentése a Linux virtuális gép az Azure-ban MySQL aaaSet |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello MySQL verem Linux virtuális gépre (Ubuntu vagy RedHat termékcsalád operációs rendszer) az Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 153bae7c-897b-46b3-bd86-192a6efb94fa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/01/2016
ms.author: mingzhan
ms.openlocfilehash: e47d0de7f0eb5bb873ad20e4bc35f1b5f8d33bdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooinstall-mysql-on-azure"></a><span data-ttu-id="f81ce-103">Hogyan tooinstall MySQL az Azure-on</span><span class="sxs-lookup"><span data-stu-id="f81ce-103">How tooinstall MySQL on Azure</span></span>
<span data-ttu-id="f81ce-104">Ebből a cikkből megtudhatja, hogyan tooinstall és MySQL konfigurálása a Linux operációs rendszert futtató Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="f81ce-104">In this article, you will learn how tooinstall and configure MySQL on an Azure virtual machine running Linux.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="install-mysql-on-your-virtual-machine"></a><span data-ttu-id="f81ce-105">MySQL telepítése a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="f81ce-105">Install MySQL on your virtual machine</span></span>
> [!NOTE]
> <span data-ttu-id="f81ce-106">Már rendelkeznie kell egy Microsoft Azure virtuális gépen, Linuxot futtató rendelés toocomplete Ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="f81ce-106">You must already have a Microsoft Azure virtual machine running Linux in order toocomplete this tutorial.</span></span> <span data-ttu-id="f81ce-107">Tekintse meg a [Azure Linux virtuális gép oktatóanyag](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate és a Linux virtuális gépet `mysqlnode` hello virtuális gép nevét, és `azureuser` felhasználóként a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="f81ce-107">Please see the [Azure Linux VM tutorial](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) toocreate and set up a Linux VM with `mysqlnode` as hello VM name and `azureuser` as user before proceeding.</span></span>
> 
> 

<span data-ttu-id="f81ce-108">Ebben az esetben 3306 portot használni hello MySQL port.</span><span class="sxs-lookup"><span data-stu-id="f81ce-108">In this case, use 3306 port as hello MySQL port.</span></span>  

<span data-ttu-id="f81ce-109">Csatlakozás toohello putty segítségével létrehozott Linux virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="f81ce-109">Connect toohello Linux VM you created via putty.</span></span> <span data-ttu-id="f81ce-110">Ha ez hello Azure Linux virtuális gép első használatakor, lásd: hogyan kapcsolódnak az toouse putty a Linux virtuális gép tooa [Itt](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f81ce-110">If this is hello first time you use Azure Linux VM, see how toouse putty connect tooa Linux VM [here](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f81ce-111">Tárház csomag tooinstall MySQL5.6 használjuk példaként, ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="f81ce-111">We will use repository package tooinstall MySQL5.6 as an example in this article.</span></span> <span data-ttu-id="f81ce-112">MySQL5.6 rendelkezik-e további fokozása mint MySQL5.5 teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="f81ce-112">Actually, MySQL5.6 has more improvement in performance than MySQL5.5.</span></span>  <span data-ttu-id="f81ce-113">További információ [Itt](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span><span class="sxs-lookup"><span data-stu-id="f81ce-113">More information [here](http://www.mysqlperformanceblog.com/2013/02/18/is-mysql-5-6-slower-than-mysql-5-5/).</span></span>

### <a name="how-tooinstall-mysql56-on-ubuntu"></a><span data-ttu-id="f81ce-114">Hogyan tooinstall az Ubuntu MySQL5.6</span><span class="sxs-lookup"><span data-stu-id="f81ce-114">How tooinstall MySQL5.6 on Ubuntu</span></span>
<span data-ttu-id="f81ce-115">Használjuk Linux virtuális gép az Azure-ból Ubuntu itt.</span><span class="sxs-lookup"><span data-stu-id="f81ce-115">We will use Linux VM with Ubuntu from Azure here.</span></span>

* <span data-ttu-id="f81ce-116">1. lépés: Telepítse a MySQL Server 5.6 túl kapcsoló`root` felhasználó:</span><span class="sxs-lookup"><span data-stu-id="f81ce-116">Step 1: Install MySQL Server 5.6   Switch too`root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="f81ce-117">Mysql-kiszolgáló 5.6 telepítése:</span><span class="sxs-lookup"><span data-stu-id="f81ce-117">Install mysql-server 5.6:</span></span>
  
            #[root@mysqlnode ~]# apt-get update
            #[root@mysqlnode ~]# apt-get -y install mysql-server-5.6
  
    <span data-ttu-id="f81ce-118">A telepítés során megjelenik egy párbeszédpanel ablak poping tooask fel akkor tooset MySQL gyökér szintű jelszavát az alábbi, és kell itt beállította az hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f81ce-118">During installation, you will see a dialog window poping up tooask you tooset MySQL root password below, and you need set hello password here.</span></span>
  
    ![Kép](./media/mysql-install/virtual-machines-linux-install-mysql-p1.png)

    <span data-ttu-id="f81ce-120">Bemeneti hello jelszó újra tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="f81ce-120">Input hello password again tooconfirm.</span></span>

    ![Kép](./media/mysql-install/virtual-machines-linux-install-mysql-p2.png)

* <span data-ttu-id="f81ce-122">2. lépés: Bejelentkezés MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f81ce-122">Step 2: Login MySQL Server</span></span>
  
    <span data-ttu-id="f81ce-123">MySQL-kiszolgáló telepítésének befejeződése után MySQL-szolgáltatás automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="f81ce-123">When MySQL server installation finished, MySQL service will be started automatically.</span></span> <span data-ttu-id="f81ce-124">MySQL kiszolgáló jelentkezhet `root` felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f81ce-124">You can login MySQL Server with `root` user.</span></span>
    <span data-ttu-id="f81ce-125">Hello alatt parancs toologin és a bemeneti jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="f81ce-125">Use hello below command toologin and input password.</span></span>
  
             #[root@mysqlnode ~]# mysql -uroot -p
* <span data-ttu-id="f81ce-126">3. lépés: A MySQL-szolgáltatást futtató hello kezelése</span><span class="sxs-lookup"><span data-stu-id="f81ce-126">Step 3: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="f81ce-127">a MySQL-szolgáltatás állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="f81ce-127">(a) Get status of MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql status
  
    <span data-ttu-id="f81ce-128">(b) a MySQL-szolgáltatás elindítása</span><span class="sxs-lookup"><span data-stu-id="f81ce-128">(b) Start MySQL Service</span></span>
  
             #[root@mysqlnode ~]# service mysql start
  
    <span data-ttu-id="f81ce-129">(c) MySQL-szolgáltatás leállítása</span><span class="sxs-lookup"><span data-stu-id="f81ce-129">(c) Stop MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql stop
  
    <span data-ttu-id="f81ce-130">(d) hello MySQL-szolgáltatás újraindítása</span><span class="sxs-lookup"><span data-stu-id="f81ce-130">(d) Restart hello MySQL service</span></span>
  
             #[root@mysqlnode ~]# service mysql restart

### <a name="how-tooinstall-mysql-on-red-hat-os-family-like-centos-oracle-linux"></a><span data-ttu-id="f81ce-131">Hogyan tooinstall MySQL Red Hat operációsrendszer-család például a CentOS, Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="f81ce-131">How tooinstall MySQL on Red Hat OS family like CentOS, Oracle Linux</span></span>
<span data-ttu-id="f81ce-132">Használjuk Linux virtuális gép CentOS vagy Oracle Linux itt.</span><span class="sxs-lookup"><span data-stu-id="f81ce-132">We will use Linux VM with CentOS or Oracle Linux here.</span></span>

* <span data-ttu-id="f81ce-133">1. lépés: Hello MySQL Yum tárház kapcsoló túl hozzáadása`root` felhasználó:</span><span class="sxs-lookup"><span data-stu-id="f81ce-133">Step 1: Add hello MySQL Yum repository   Switch too`root` user:</span></span>
  
            #[azureuser@mysqlnode:~]sudo su -
  
    <span data-ttu-id="f81ce-134">Töltse le és telepítse hello MySQL kiadási csomag:</span><span class="sxs-lookup"><span data-stu-id="f81ce-134">Download and install hello MySQL release package:</span></span>
  
            #[root@mysqlnode ~]# wget http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm
            #[root@mysqlnode ~]# yum localinstall -y mysql-community-release-el6-5.noarch.rpm
* <span data-ttu-id="f81ce-135">2. lépés: Szerkesztés alatt fájl tooenable hello MySQL tárháza hello MySQL5.6 csomag letöltése.</span><span class="sxs-lookup"><span data-stu-id="f81ce-135">Step 2: Edit below file tooenable hello MySQL repository for downloading hello MySQL5.6 package.</span></span>
  
            #[root@mysqlnode ~]# vim /etc/yum.repos.d/mysql-community.repo
  
    <span data-ttu-id="f81ce-136">A fájl toobelow minden értékének frissítése:</span><span class="sxs-lookup"><span data-stu-id="f81ce-136">Update each value of this file toobelow:</span></span>
  
        \# *Enable toouse MySQL 5.6*
  
        [mysql56-community]
        name=MySQL 5.6 Community Server
  
        baseurl=http://repo.mysql.com/yum/mysql-5.6-community/el/6/$basearch/
  
        enabled=1
  
        gpgcheck=1
  
        gpgkey=file:/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
* <span data-ttu-id="f81ce-137">3. lépés: A telepítés MySQL MySQL tárházból MySQL telepítése:</span><span class="sxs-lookup"><span data-stu-id="f81ce-137">Step 3: Install MySQL from MySQL repository   Install MySQL:</span></span>
  
           #[root@mysqlnode ~]#yum install mysql-community-server
  
    <span data-ttu-id="f81ce-138">MySQL RPM-csomag és az összes kapcsolódó csomag lesz telepítve.</span><span class="sxs-lookup"><span data-stu-id="f81ce-138">MySQL RPM package and all related packages will be installed.</span></span>
* <span data-ttu-id="f81ce-139">4. lépés: A MySQL-szolgáltatást futtató hello kezelése</span><span class="sxs-lookup"><span data-stu-id="f81ce-139">Step 4: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="f81ce-140">(a) hello MySQL kiszolgáló hello szolgáltatás állapotának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="f81ce-140">(a) Check hello service status of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]#service mysqld status
  
    <span data-ttu-id="f81ce-141">(b) ellenőrizze, hogy fut-e hello alapértelmezett port a MySQL-kiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="f81ce-141">(b) Check whether hello default port of  MySQL server is running:</span></span>
  
           #[root@mysqlnode ~]#netstat  –tunlp|grep 3306

    <span data-ttu-id="f81ce-142">(c) hello MySQL kiszolgáló indítása:</span><span class="sxs-lookup"><span data-stu-id="f81ce-142">(c) Start hello MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld start

    <span data-ttu-id="f81ce-143">(d) hello MySQL kiszolgáló leállítása:</span><span class="sxs-lookup"><span data-stu-id="f81ce-143">(d) Stop hello MySQL server:</span></span>

           #[root@mysqlnode ~]#service mysqld stop

    <span data-ttu-id="f81ce-144">e MySQL toostart be, ha a rendszer hello állítja:</span><span class="sxs-lookup"><span data-stu-id="f81ce-144">(e) Set MySQL toostart when hello system boot-up:</span></span>

           #[root@mysqlnode ~]#chkconfig mysqld on


### <a name="how-tooinstall-mysql-on-suse-linux"></a><span data-ttu-id="f81ce-145">Hogyan tooinstall MySQL SUSE Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="f81ce-145">How tooinstall MySQL on SUSE Linux</span></span>
<span data-ttu-id="f81ce-146">Használjuk Linux virtuális gép, OpenSUSE itt.</span><span class="sxs-lookup"><span data-stu-id="f81ce-146">We will use Linux VM with OpenSUSE here.</span></span>

* <span data-ttu-id="f81ce-147">1. lépés: Töltse le és telepítse a MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="f81ce-147">Step 1: Download and install MySQL Server</span></span>
  
    <span data-ttu-id="f81ce-148">Váltás túl`root` felhasználó keresztül alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="f81ce-148">Switch too`root` user through below command:</span></span>  
  
           #sudo su -
  
    <span data-ttu-id="f81ce-149">Töltse le és telepítse a MySQL-csomagot:</span><span class="sxs-lookup"><span data-stu-id="f81ce-149">Download and install MySQL package:</span></span>
  
           #[root@mysqlnode ~]# zypper update
  
           #[root@mysqlnode ~]# zypper install mysql-server mysql-devel mysql
* <span data-ttu-id="f81ce-150">2. lépés: A MySQL-szolgáltatást futtató hello kezelése</span><span class="sxs-lookup"><span data-stu-id="f81ce-150">Step 2: Manage hello running MySQL service</span></span>
  
    <span data-ttu-id="f81ce-151">(a) hello MySQL server hello állapotának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="f81ce-151">(a) Check hello status of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]# rcmysql status
  
    <span data-ttu-id="f81ce-152">(b) ellenőrizze, hogy hello hello MySQL kiszolgáló alapértelmezett port:</span><span class="sxs-lookup"><span data-stu-id="f81ce-152">(b) Check whether hello default port of hello MySQL server:</span></span>
  
           #[root@mysqlnode ~]# netstat  –tunlp|grep 3306

    <span data-ttu-id="f81ce-153">(c) hello MySQL kiszolgáló indítása:</span><span class="sxs-lookup"><span data-stu-id="f81ce-153">(c) Start hello MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql start

    <span data-ttu-id="f81ce-154">(d) hello MySQL kiszolgáló leállítása:</span><span class="sxs-lookup"><span data-stu-id="f81ce-154">(d) Stop hello MySQL server:</span></span>

           #[root@mysqlnode ~]# rcmysql stop

    <span data-ttu-id="f81ce-155">e MySQL toostart be, ha a rendszer hello állítja:</span><span class="sxs-lookup"><span data-stu-id="f81ce-155">(e) Set MySQL toostart when hello system boot-up:</span></span>

           #[root@mysqlnode ~]# insserv mysql

### <a name="next-step"></a><span data-ttu-id="f81ce-156">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="f81ce-156">Next Step</span></span>
<span data-ttu-id="f81ce-157">További használati és MySQL kapcsolatos információk keresése [Itt](https://www.mysql.com/).</span><span class="sxs-lookup"><span data-stu-id="f81ce-157">Find more usage and information regarding MySQL [Here](https://www.mysql.com/).</span></span>

