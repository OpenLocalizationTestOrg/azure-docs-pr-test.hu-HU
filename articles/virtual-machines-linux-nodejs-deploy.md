---
title: "aaaDeploy egy Node.js-alkalmazás tooLinux virtuális gépek Azure-ban"
description: "Megtudhatja, hogyan toodeploy a Node.js alkalmazás tooLinux virtuális gépek Azure-ban."
services: 
documentationcenter: nodejs
author: stepro
manager: dmitryr
editor: 
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: /azure
ms.assetid: 857a812d-c73e-4af7-a985-2d0baf8b6f71
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2016
ms.author: stephpr
ms.openlocfilehash: e876c8170a06f102f7f32ec7cb930b876647a707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-nodejs-application-toolinux-virtual-machines-in-azure"></a><span data-ttu-id="9d7e7-103">A Node.js-alkalmazás tooLinux az Azure virtuális gépek telepítése</span><span class="sxs-lookup"><span data-stu-id="9d7e7-103">Deploy a Node.js application tooLinux Virtual Machines in Azure</span></span>
<span data-ttu-id="9d7e7-104">Ez az oktatóanyag bemutatja, hogyan tootake egy Node.js-alkalmazást, és telepítse azt az Azure-ban futó tooLinux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-104">This tutorial shows how tootake a Node.js application and deploy it tooLinux virtual machines running in Azure.</span></span> <span data-ttu-id="9d7e7-105">hello az oktatóanyag utasításai követhetők bármely operációs rendszeren, amely alkalmas a Node.js futtatására.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-105">hello instructions in this tutorial can be followed on any operating system that is capable of running Node.js.</span></span>

<span data-ttu-id="9d7e7-106">Megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-106">You'll learn how to:</span></span>

* <span data-ttu-id="9d7e7-107">Elágazás és a klón egy egyszerű teendőlistát Megvalósító alkalmazás; tartalmazó GitHub-tárházat</span><span class="sxs-lookup"><span data-stu-id="9d7e7-107">Fork and clone a GitHub repository containing a simple TODO application;</span></span>
* <span data-ttu-id="9d7e7-108">Hozza létre és konfigurálja a két Linux virtuális gépek Azure toorun hello alkalmazásban;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-108">Create and configure two Linux virtual machines in Azure toorun hello application;</span></span>
* <span data-ttu-id="9d7e7-109">Hello alkalmazás többször frissítések toohello webes előtér virtuális gép küldésével.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-109">Iterate on hello application by pushing updates toohello web frontend virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="9d7e7-110">toocomplete ebben az oktatóanyagban GitHub-fiók és a Microsoft Azure-fiókra van szükség, és képes toouse Git fejlesztési gépről hello.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-110">toocomplete this tutorial, you need a GitHub account and a Microsoft Azure account, and hello ability toouse Git from a development machine.</span></span>
> 
> <span data-ttu-id="9d7e7-111">Ha a GitHub-fiók nem rendelkezik, iratkozzon fel a [Itt](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-111">If you don't have a GitHub account, you can sign up [here](https://github.com/join).</span></span>
> 
> <span data-ttu-id="9d7e7-112">Ha nem rendelkezik egy [Microsoft Azure](https://azure.microsoft.com/) fiókja, regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-112">If you don't have a [Microsoft Azure](https://azure.microsoft.com/) account, you can sign up for a FREE trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="9d7e7-113">Ez is végigvezeti hello regisztrációs folyamat az egy [Microsoft Account](http://account.microsoft.com) Ha még nem rendelkezik egy.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-113">This will also lead you through hello sign up process for a [Microsoft Account](http://account.microsoft.com) if you do not already have one.</span></span> <span data-ttu-id="9d7e7-114">Másik lehetőségként, ha a Visual Studio előfizetői, is [aktiválhatja MSDN előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-114">Alternatively, if you are a Visual Studio subscriber, you can [activate your MSDN benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="9d7e7-115">Ha még nem git a fejlesztői számítógépén, használata Macintosh- vagy Windows-számítógépen, telepítse a git [Itt](http://www.git-scm.com).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-115">If you do not have git on your development machine, then if you are using a Macintosh or Windows machine, install git from [here](http://www.git-scm.com).</span></span> <span data-ttu-id="9d7e7-116">Ha Linux használ, telepítse a git használatával, mint hello mechanizmus legmegfelelőbb értéket, `sudo apt-get install git`.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-116">If you are using Linux, install git using hello mechanism most appropriate for you, such as `sudo apt-get install git`.</span></span>
> 
> 

## <a name="forking-and-cloning-hello-todo-application"></a><span data-ttu-id="9d7e7-117">Elágazó és a Klónozás hello Teendőlista alkalmazás</span><span class="sxs-lookup"><span data-stu-id="9d7e7-117">Forking and Cloning hello TODO Application</span></span>
<span data-ttu-id="9d7e7-118">Az oktatóprogram során használt Teendőlista alkalmazás hello egy egyszerű webes előtér keresztül, amely a teendőlista nyomon követi a MongoDB-példány valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-118">hello TODO application used by this tutorial implements a simple web frontend over a MongoDB instance that keeps track of a TODO list.</span></span> <span data-ttu-id="9d7e7-119">Nyissa meg a bejelentkezést követően a tooGitHub [Itt](https://github.com/stepro/node-todo) toofind alkalmazás hello és oszthatja ketté, a jobb felső hello hello-kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-119">After signing in tooGitHub, go [here](https://github.com/stepro/node-todo) toofind hello application and fork it using hello link in hello top right.</span></span> <span data-ttu-id="9d7e7-120">Ez hozzon létre egy tárház nevű fiókjában *accountname*/node-todo.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-120">This should create a repository in your account named *accountname*/node-todo.</span></span>

<span data-ttu-id="9d7e7-121">Most a fejlesztői gépen klónozza a tárházat:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-121">Now on your development machine, clone this repository:</span></span>

    git clone https://github.com/accountname/node-todo.git

<span data-ttu-id="9d7e7-122">Fogjuk használni a helyi klónozott tárház hello egy kicsit később változtatások toohello forráskódot.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-122">We'll use this local clone of hello repository a little later when making changes toohello source code.</span></span>

## <a name="creating-and-configuring-hello-linux-virtual-machines"></a><span data-ttu-id="9d7e7-123">Létrehozásához és konfigurálásához hello Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="9d7e7-123">Creating and Configuring hello Linux Virtual Machines</span></span>
<span data-ttu-id="9d7e7-124">Azure Linux virtuális gépek használatával nyers számítás kiváló visszajelzési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-124">Azure has great support for raw compute using Linux virtual machines.</span></span> <span data-ttu-id="9d7e7-125">Ez a része hello oktatóanyag bemutatja, hogyan könnyen léptetési két Linux virtuális gépek és hello Teendőlista alkalmazás toothem, futó hello webes előtér egy és hello más hello a MongoDB-példány telepítése.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-125">This part of hello tutorial shows how you can easily spin up two Linux virtual machines and deploy hello TODO application toothem, running hello web frontend on one and hello MongoDB instance on hello other.</span></span>

### <a name="creating-virtual-machines"></a><span data-ttu-id="9d7e7-126">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-126">Creating Virtual Machines</span></span>
<span data-ttu-id="9d7e7-127">hello legegyszerűbb módja toocreate egy új virtuális gépet az Azure-ban toouse hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-127">hello easiest way toocreate a new virtual machine in Azure is toouse hello Azure Portal.</span></span> <span data-ttu-id="9d7e7-128">Kattintson a [Itt](https://portal.azure.com) a toosign, és indítsa el a webböngésző hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-128">Click [here](https://portal.azure.com) toosign in and launch hello Azure Portal in your web browser.</span></span> <span data-ttu-id="9d7e7-129">Azure portál be van töltve, hello befejezést hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-129">Once hello Azure Portal has loaded, complete hello following steps:</span></span>

* <span data-ttu-id="9d7e7-130">Kattintson a hello "+ új" hivatkozásra;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-130">Click hello "+ New" link;</span></span>
* <span data-ttu-id="9d7e7-131">Válassza ki a hello "Számítási" kategória, és válassza a "Ubuntu Server 14.04 LTS";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-131">Pick hello "Compute" category and choose "Ubuntu Server 14.04 LTS";</span></span>
* <span data-ttu-id="9d7e7-132">Hello "erőforrás-kezelő" telepítési modell kiválasztása, és kattintson a "Létrehozás";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-132">Select hello "Resource Manager" deployment model and click "Create";</span></span>
* <span data-ttu-id="9d7e7-133">Töltse ki az alábbi utasításokat követve hello alapjai:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-133">Fill in hello basics following these guidelines:</span></span>
  * <span data-ttu-id="9d7e7-134">Adjon meg egy újabb; könnyen azonosítható nevet</span><span class="sxs-lookup"><span data-stu-id="9d7e7-134">Specify a name you can easily identify later;</span></span>
  * <span data-ttu-id="9d7e7-135">A jelen oktatóanyag esetében válassza a jelszó-hitelesítés;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-135">For this tutorial, choose Password authentication;</span></span>
  * <span data-ttu-id="9d7e7-136">Hozzon létre egy új erőforráscsoportot azonosítható névvel.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-136">Create a new resource group with an identifiable name.</span></span>
* <span data-ttu-id="9d7e7-137">Hello virtuális gép méretét, a "A1 szabványos" akkor ésszerű választani, ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-137">For hello Virtual Machine size, "A1 Standard" is a reasonable choice for this tutorial.</span></span>
* <span data-ttu-id="9d7e7-138">A további beállításokhoz győződjön meg arról hello lemez típusa "Standard", és fogadja el az összes többi alapértelmezett hello.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-138">For additional settings, ensure hello disk type is "Standard" and accept all hello remaining defaults.</span></span>
* <span data-ttu-id="9d7e7-139">Indítsa hello létrehozása hello összefoglaló oldalon.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-139">Kick off hello creation on hello summary page.</span></span>

<span data-ttu-id="9d7e7-140">Hajtsa végre a fenti hello feldolgozása kétszer toocreate két Linux virtuális gépek, egy a hello webes előtér és egy hello MongoDB-példány.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-140">Perform hello above process twice toocreate two Linux virtual machines, one for hello web frontend and one for hello MongoDB instance.</span></span> <span data-ttu-id="9d7e7-141">Hello virtuális gépek létrehozását körülbelül 5 – 10 percig tart.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-141">Creation of hello virtual machines will take about 5-10 minutes.</span></span>

### <a name="assigning-a-dns-entry-for-virtual-machines"></a><span data-ttu-id="9d7e7-142">A DNS-bejegyzés hozzárendelése a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="9d7e7-142">Assigning a DNS entry for Virtual Machines</span></span>
<span data-ttu-id="9d7e7-143">Az Azure-ban létrehozott virtuális gépek csak egy nyilvános IP-címet, például 1.2.3.4 keresztül érhető el alapértelmezés szerint is.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-143">Virtual machines created in Azure are by default only accessible through a public IP address like 1.2.3.4.</span></span> <span data-ttu-id="9d7e7-144">Most Meggyőződünk hello gépek könnyebben azonosítható DNS-bejegyzések hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-144">Let's make hello machines more easily identifiable by assigning them DNS entries.</span></span>

<span data-ttu-id="9d7e7-145">Miután hello portal azt jelzi, hogy létrejöttek-e a hello virtuális gépek, kattintson a hello bal oldali navigációs sávja hello "Virtual machines" hivatkozásra, és keresse meg a gépek.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-145">Once hello portal indicates that hello virtual machines have been created, click on hello "Virtual machines" link in hello left navbar and locate your machines.</span></span> <span data-ttu-id="9d7e7-146">Az egyes gépek:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-146">For each machine:</span></span>

* <span data-ttu-id="9d7e7-147">Keresse meg a hello alapvető erőforrások lapon, majd kattintson a hello nyilvános IP-cím;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-147">Locate hello Essentials tab and click on hello Public IP Address;</span></span>
* <span data-ttu-id="9d7e7-148">A hello nyilvános IP-címének konfigurációja rendelje hozzá egy DNS-névcímke, és mentse.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-148">In hello public IP address configuration, assign a DNS name label and save.</span></span>

<span data-ttu-id="9d7e7-149">hello portal biztosítja az adott hello néven érhető el.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-149">hello portal will ensure that hello name you specify is available.</span></span> <span data-ttu-id="9d7e7-150">Hello konfigurációjának mentése után a virtuális gépek lesz hasonló állomásnevek túl`machinename.region.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-150">After saving hello configuration, your virtual machines will have host names similar too`machinename.region.cloudapp.azure.com`.</span></span>

### <a name="connecting-toohello-virtual-machines"></a><span data-ttu-id="9d7e7-151">Csatlakozás a virtuális gépek toohello</span><span class="sxs-lookup"><span data-stu-id="9d7e7-151">Connecting toohello Virtual Machines</span></span>
<span data-ttu-id="9d7e7-152">Ha a virtuális gépek kiépített, előre konfigurált tooallow távoli kapcsolatok voltak SSH-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-152">When your virtual machines were provisioned, they were pre-configured tooallow remote connections over SSH.</span></span> <span data-ttu-id="9d7e7-153">Ez a hello mechanizmus tooconfigure hello virtuális gépek használjuk.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-153">This is hello mechanism we will use tooconfigure hello virtual machines.</span></span> <span data-ttu-id="9d7e7-154">Ha Windows használ a fejlesztési, ha még nem rendelkezik egy szüksége lesz tooget egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-154">If you are using Windows for your development, you will need tooget an SSH client if you do not already have one.</span></span> <span data-ttu-id="9d7e7-155">Egy közös választott, amely letölthető a PuTTY [Itt](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-155">A common choice here is PuTTY, which can be downloaded from [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span></span> <span data-ttu-id="9d7e7-156">A Macintosh- és Linux operációs rendszer származnak SSH előre telepített verziója.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-156">Macintosh and Linux OSes come with a version of SSH pre-installed.</span></span>

### <a name="configuring-hello-web-frontend-virtual-machine"></a><span data-ttu-id="9d7e7-157">Hello webes előtér virtuális gép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-157">Configuring hello Web Frontend Virtual Machine</span></span>
<span data-ttu-id="9d7e7-158">SSH toohello webes előtér gép létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-158">SSH toohello web frontend machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="9d7e7-159">Egy parancs parancssori futtatásával követ üdvözlő üzenet megjelenik.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-159">You should see a welcome message followed by a command prompt.</span></span>

<span data-ttu-id="9d7e7-160">Első lépésként most Meggyőződünk arról, hogy a git szoftver, csomópont is telepítve vannak:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-160">First, let's make sure that git and node are both installed:</span></span>

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

<span data-ttu-id="9d7e7-161">Hello alkalmazás webes előtér néhány natív Node.js modulok támaszkodik, mivel tooinstall hello alapvető összeállítási eszközök gyűjteménye is kell:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-161">Since hello application's web frontend relies on some native Node.js modules, we also need tooinstall hello essential set of build tools:</span></span>

    sudo apt-get install -y build-essential

<span data-ttu-id="9d7e7-162">Végezetül most telepíteni a Node.js-alkalmazás neve *végtelen*, toorun Node.js kiszolgálóalkalmazások segítségével:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-162">Finally, let's install a Node.js application called *forever*, which helps toorun Node.js server applications:</span></span>

    sudo npm install -g forever

<span data-ttu-id="9d7e7-163">Ezek azok a virtuális gép toobe képes toorun hello alkalmazás webes előtér szükséges összes hello függőségek, így folytassuk, hogy futnak.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-163">These are all hello dependencies needed on this virtual machine toobe able toorun hello application's web frontend, so let's get that running.</span></span> <span data-ttu-id="9d7e7-164">toodo, először létrehozunk egy operációs rendszer klónja hello GitHub-tárházban korábban meg ágazik el, így könnyen közzéteheti a frissítéseket toohello virtuális gép (oktatóanyag a következőket ismerteti a frissítési forgatókönyv később), és majd klónozni hello operációs rendszer Klónozás tooprovide hello egy verziója az tárház ténylegesen hajt végre.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-164">toodo this, we will first create a bare clone of hello GitHub repository you previously forked so that you can easily publish updates toohello virtual machine (we'll cover this update scenario later), and then clone hello bare clone tooprovide a version of hello repository that can actually be executed.</span></span>

<span data-ttu-id="9d7e7-165">Hello kezdőkönyvtára (~) kiindulva, futtassa a következő parancsok hello (cseréje *accountname* a GitHub felhasználói fiók nevével):</span><span class="sxs-lookup"><span data-stu-id="9d7e7-165">Starting from hello home (~) directory, run hello following commands (replacing *accountname* with your GitHub user account name):</span></span>

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

<span data-ttu-id="9d7e7-166">Most adja meg hello csomópont-todo könyvtárat, és futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-166">Now enter hello node-todo directory and run these commands:</span></span>

    npm install
    forever start server.js

<span data-ttu-id="9d7e7-167">hello alkalmazás webes előtér most már fut, azonban, hogy egy további lépést egy webböngészőből hello alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-167">hello application's web frontend is now running, however there is one more step before you can access hello application from a web browser.</span></span> <span data-ttu-id="9d7e7-168">hello létrehozott virtuális gép által védett egy Azure-erőforrás neve egy *hálózati biztonsági csoport*, amely hozták létre, amikor a kiépített hello virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-168">hello virtual machine you created is protected by an Azure resource called a *network security group*, which was created for you when you provisioned hello virtual machine.</span></span> <span data-ttu-id="9d7e7-169">Ezt az erőforrást jelenleg csak külső kérelmek tooport irányított 22 toobe toohello virtuális gépet, mely lehetővé teszi az SSH-kommunikációhoz hello gép, de semmi mást teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-169">Currently, this resource only allows external requests tooport 22 toobe routed toohello virtual machine, which enables SSH communication with hello machine but nothing else.</span></span> <span data-ttu-id="9d7e7-170">Ezért a rendelés tooview hello Teendőlista alkalmazás, amely konfigurált toorun a 8080-as porton, ezt a portot is igények toobe megnyitotta a.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-170">So in order tooview hello TODO application, which is configured toorun on port 8080, this port also needs toobe opened up.</span></span>

<span data-ttu-id="9d7e7-171">Térjen vissza a toohello Azure portálon, és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-171">Return toohello Azure Portal and complete hello following steps:</span></span>

* <span data-ttu-id="9d7e7-172">Kattintson a "Erőforráscsoportok" hello bal oldali navigációs sávja;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-172">Click on "Resource groups" in hello left navbar;</span></span>
* <span data-ttu-id="9d7e7-173">A virtuális gépet; tartalmazó hello erőforráscsoport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-173">Select hello resource group that contains your virtual machine;</span></span>
* <span data-ttu-id="9d7e7-174">Hello az erőforrások listájában válassza ki a hello hálózati biztonsági csoportot (hello egy egy pajzs ikon);</span><span class="sxs-lookup"><span data-stu-id="9d7e7-174">In hello resulting list of resources, select hello network security group (hello one with a shield icon);</span></span>
* <span data-ttu-id="9d7e7-175">Hello tulajdonságait válassza a "bejövő biztonsági szabályok";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-175">In hello properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="9d7e7-176">A hello eszköztárában kattintson a "Hozzáadás";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-176">In hello toolbar, click "Add";</span></span>
* <span data-ttu-id="9d7e7-177">Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-teendőlista";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-177">Provide a name like "default-allow-todo";</span></span>
* <span data-ttu-id="9d7e7-178">Hello protokoll túl beállítása "TCP";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-178">Set hello protocol too"TCP";</span></span>
* <span data-ttu-id="9d7e7-179">Állítsa be a hello Célporttartomány túl "8080-as";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-179">Set hello destination port range too"8080";</span></span>
* <span data-ttu-id="9d7e7-180">Kattintson az OK gombra, majd várja meg a létrehozott hello biztonsági szabály toobe.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-180">Click OK and wait for hello security rule toobe created.</span></span>

<span data-ttu-id="9d7e7-181">A biztonsági szabály létrehozása után hello Teendőlista alkalmazás nyilvánosan látható hello internet, és tallózással is kikeresheti tooit, például, mint az egy URL-cím segítségével:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-181">After creating this security rule, hello TODO application is publically visible on hello internet and you can browse tooit, for instance using a URL such as:</span></span>

    http://machinename.region.cloudapp.azure.com:8080

<span data-ttu-id="9d7e7-182">Megfigyelheti, hogy annak ellenére, hogy még nincs konfigurálva hello MongoDB virtuális gép, hello Teendőlista alkalmazás teljesen működőképes toobe megjelenik.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-182">You will notice that even though we have not yet configured hello MongoDB virtual machine, hello TODO application appears toobe quite functional.</span></span> <span data-ttu-id="9d7e7-183">Ez azért, mert hello forrás tárház szoftveresen kötött toouse előre telepített MongoDB-példány.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-183">This is because hello source repository is hardcoded toouse a pre-deployed MongoDB instance.</span></span> <span data-ttu-id="9d7e7-184">Miután hello MongoDB virtuális gép rendelkezik konfigurált, rendszer lépjen vissza és módosítása hello forrás kód tooutilize a személyes MongoDB-példány helyett.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-184">Once we have configured hello MongoDB virtual machine, we will go back and change hello source code tooutilize our private MongoDB instance instead.</span></span>

### <a name="configuring-hello-mongodb-virtual-machine"></a><span data-ttu-id="9d7e7-185">Hello MongoDB virtuális gép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-185">Configuring hello MongoDB Virtual Machine</span></span>
<span data-ttu-id="9d7e7-186">SSH toohello második gép létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-186">SSH toohello second machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="9d7e7-187">A MongoDB telepítése után megtekintheti a hello üdvözlő üzenet és a parancssort, (ezek az utasítások vették [Itt](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span><span class="sxs-lookup"><span data-stu-id="9d7e7-187">After seeing hello welcome message and command prompt, install MongoDB (these instructions were taken from [here](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span></span>

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

<span data-ttu-id="9d7e7-188">Alapértelmezés szerint MongoDB van beállítva, ezért csak elérhető helyileg.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-188">By default, MongoDB is configured so it can only be accessed locally.</span></span> <span data-ttu-id="9d7e7-189">Ebben az oktatóanyagban azt konfigurálja MongoDB, a rendszer az elérhető legyen a hello alkalmazás virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-189">For this tutorial, we will configure MongoDB so it can be accessed from hello application's virtual machine.</span></span> <span data-ttu-id="9d7e7-190">A sudo a környezetben, nyissa meg hello /etc/mongod.conf fájlt, és keresse meg a hello `# network interfaces` szakasz.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-190">In a sudo context, open hello /etc/mongod.conf file and locate hello `# network interfaces` section.</span></span> <span data-ttu-id="9d7e7-191">Változás hello `net.bindIp` konfigurációs érték túl`0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-191">Change hello `net.bindIp` configuration value too`0.0.0.0`.</span></span>

> [!NOTE]
> <span data-ttu-id="9d7e7-192">Ez a konfiguráció akkor csak ez az oktatóanyag hello alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-192">This configuration is for hello purposes of this tutorial only.</span></span> <span data-ttu-id="9d7e7-193">Az **nem** egy ajánlott biztonsági eljárás, és nem használható üzemi környezetben.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-193">It is **NOT** a recommended security practice and should not be used in production environments.</span></span>
> 
> 

<span data-ttu-id="9d7e7-194">Most már biztosítja hello szolgáltatás el van indítva MongoDB:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-194">Now ensure hello MongoDB service has been started:</span></span>

    sudo service mongod restart

<span data-ttu-id="9d7e7-195">MongoDB porton 27017 alapértelmezés szerint működik.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-195">MongoDB operates over port 27017 by default.</span></span> <span data-ttu-id="9d7e7-196">Így hello a megszokott módon, hogy változtatnunk tooopen hello webes előtér virtuális gépen, a 8080-as port igazolnia kell, hogy tooopen port 27017 hello MongoDB virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-196">So, in hello same way that we needed tooopen port 8080 on hello web frontend virtual machine, we need tooopen port 27017 on hello MongoDB virtual machine.</span></span>

<span data-ttu-id="9d7e7-197">Térjen vissza a toohello Azure portálon, és végezze el az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-197">Return toohello Azure Portal and complete hello following steps:</span></span>

* <span data-ttu-id="9d7e7-198">Kattintson a "Erőforráscsoportok" hello bal oldali navigációs sávja;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-198">Click on "Resource groups" in hello left navbar;</span></span>
* <span data-ttu-id="9d7e7-199">Hello MongoDB virtuális gépet; tartalmazó hello erőforráscsoport kiválasztása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-199">Select hello resource group that contains hello MongoDB virtual machine;</span></span>
* <span data-ttu-id="9d7e7-200">Hello az erőforrások listájában válassza ki az hello hálózati biztonsági csoport (hello egy egy pajzs ikon) rendelkező hello azonos neve, mint toohello MongoDB virtuális gép;</span><span class="sxs-lookup"><span data-stu-id="9d7e7-200">In hello resulting list of resources, select hello network security group (hello one with a shield icon) with hello same name that you gave toohello MongoDB virtual machine;</span></span>
* <span data-ttu-id="9d7e7-201">Hello tulajdonságait válassza a "bejövő biztonsági szabályok";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-201">In hello properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="9d7e7-202">A hello eszköztárában kattintson a "Hozzáadás";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-202">In hello toolbar, click "Add";</span></span>
* <span data-ttu-id="9d7e7-203">Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-mongo";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-203">Provide a name like "default-allow-mongo";</span></span>
* <span data-ttu-id="9d7e7-204">Hello protokoll túl beállítása "TCP";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-204">Set hello protocol too"TCP";</span></span>
* <span data-ttu-id="9d7e7-205">Állítsa be a hello Célporttartomány túl "27017";</span><span class="sxs-lookup"><span data-stu-id="9d7e7-205">Set hello destination port range too"27017";</span></span>
* <span data-ttu-id="9d7e7-206">Kattintson az OK gombra, majd várja meg a létrehozott hello biztonsági szabály toobe.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-206">Click OK and wait for hello security rule toobe created.</span></span>

## <a name="iterating-on-hello-todo-application"></a><span data-ttu-id="9d7e7-207">A Teendőlista alkalmazás hello léptetés</span><span class="sxs-lookup"><span data-stu-id="9d7e7-207">Iterating on hello TODO application</span></span>
<span data-ttu-id="9d7e7-208">Az eddigi jelenleg két Linux virtuális gépek kiépítése: futtató hello alkalmazás webes előtér- és egy, a MongoDB-példány fut.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-208">So far, we have provisioned two Linux virtual machines: one that is running hello application's web frontend and one that is running a MongoDB instance.</span></span> <span data-ttu-id="9d7e7-209">De a probléma – hello webes előtér nem ténylegesen használó kiépítve hello MongoDB-példány még.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-209">But there is a problem - hello web frontend isn't actually using hello provisioned MongoDB instance yet.</span></span> <span data-ttu-id="9d7e7-210">Most javíthatja ki, hogy hello webes előtér kód toouse frissítése kódolt példány helyett egy környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-210">Let's fix that by updating hello web frontend code toouse an environment variable instead of a hard-coded instance.</span></span>

### <a name="changing-hello-todo-application"></a><span data-ttu-id="9d7e7-211">Hello Teendőlista alkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-211">Changing hello TODO application</span></span>
<span data-ttu-id="9d7e7-212">A fejlesztői számítógépén, ha először klónozott hello csomópont-todo tárház, nyissa meg a hello `node-todo/config/database.js` a kedvenc szerkesztő fájlt, és módosítsa hello URL-cím hello kódolt értékből például `mongodb://...` túl`process.env.MONGODB`.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-212">On your development machine where you first cloned hello node-todo repository, open hello `node-todo/config/database.js` file in your favorite editor and change hello url value from hello hard-coded value like `mongodb://...` too`process.env.MONGODB`.</span></span>

<span data-ttu-id="9d7e7-213">A változtatások véglegesítése a határidő, és leküldéses toohello GitHub-főkiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-213">Commit your changes and push toohello GitHub master:</span></span>

    git commit -am "Get MongoDB instance from env"
    git push origin master

<span data-ttu-id="9d7e7-214">Sajnos ez nem közzététele hello módosítása toohello webes előtér virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-214">Unfortunately, this doesn't publish hello change toohello web frontend virtual machine.</span></span> <span data-ttu-id="9d7e7-215">Most Meggyőződünk módosítások toothat néhány további virtuális gépek tooenable egy egyszerű, de hatékony mechanizmus a frissítések közzétételéhez, így gyorsan figyelheti hello hatását hello hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-215">Let's make a few more changes toothat virtual machine tooenable a simple but effective mechanism for publishing updates so you can quickly observe hello effect of hello changes in hello live environment.</span></span>

### <a name="configuring-hello-web-frontend-virtual-machine"></a><span data-ttu-id="9d7e7-216">Hello webes előtér virtuális gép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-216">Configuring hello Web Frontend Virtual Machine</span></span>
<span data-ttu-id="9d7e7-217">Előhívja a korábban létrehozott egy operációs rendszer Klónozás hello csomópont-todo tárház hello webes előtér virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-217">Recall that we previously created a bare clone of hello node-todo repository on hello web frontend virtual machine.</span></span> <span data-ttu-id="9d7e7-218">Az elemről kiderül, hogy ez a művelet létrehozott egy új Git távoli toowhich módosítások továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-218">It turns out that this action created a new Git remote toowhich changes can be pushed.</span></span> <span data-ttu-id="9d7e7-219">Azonban egyszerűen küldését toothis távoli nem elég adnak hello gyors iterációs modell, amely a fejlesztők olyan eszközökre vonatkozóan feldolgozása során a rendszer a kódot.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-219">However, simply pushing toothis remote doesn't quite give hello rapid iteration model that developers are looking for when working on their code.</span></span>

<span data-ttu-id="9d7e7-220">Mi azt kellene például toobe képes toodo van győződjön meg arról, hogy egy leküldéses toohello távoli tárházba hello a virtuális gép esetén hello futó Teendőlista alkalmazás automatikusan frissül.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-220">What we would like toobe able toodo is ensure that when a push toohello remote repository on hello virtual machine occurs, hello running TODO application is automatically updated.</span></span> <span data-ttu-id="9d7e7-221">Szerencsére Ez az egyszerű tooachieve git.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-221">Fortunately, this is easy tooachieve with git.</span></span>

<span data-ttu-id="9d7e7-222">Git hurkok, melynek neve adott alkalommal tooreact tooactions hello tárház végzett, a számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-222">Git exposes a number of hooks that are called at particular times tooreact tooactions taken on hello repository.</span></span> <span data-ttu-id="9d7e7-223">Ezek vannak megadva a rendszerhéj-parancsfájlok használatával hello tárházban `hooks` mappát.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-223">These are specified using shell scripts in hello repository's `hooks` folder.</span></span> <span data-ttu-id="9d7e7-224">hello hook leginkább megfelelő hello automatikus frissítési forgatókönyv hello `post-update` esemény.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-224">hello hook that is most applicable for hello auto-update scenario is hello `post-update` event.</span></span>

<span data-ttu-id="9d7e7-225">SSH munkamenet toohello webes előtér virtuális gépen, módosítsa a toohello `~/node-todo.git/hooks` könyvtár, és adja hozzá a következő nevű tartalom tooa fájl hello `post-update` (cseréje `machinename` és `region` a MongoDB-virtuális gép adatait):</span><span class="sxs-lookup"><span data-stu-id="9d7e7-225">In a SSH session toohello web frontend virtual machine, change toohello `~/node-todo.git/hooks` directory and add hello following content tooa file named `post-update` (replacing `machinename` and `region` with your MongoDB virtual machine information):</span></span>

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

<span data-ttu-id="9d7e7-226">Győződjön meg arról, ez a fájl végrehajtható hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-226">Ensure this file is executable by running hello following command:</span></span>

    chmod 755 post-update

<span data-ttu-id="9d7e7-227">Ez a parancsfájl ellenőrzi, hogy hello aktuális kiszolgálóalkalmazás le van állítva, hello klónozott tárház hello kód frissített toohello legújabb, frissített függőségek teljesülnek, és hello kiszolgáló újraindul.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-227">This script ensures that hello current server application is stopped, hello code in hello cloned repository is updated toohello latest, any updated dependencies are satisfied, and hello server is restarted.</span></span> <span data-ttu-id="9d7e7-228">Emellett biztosítja hello környezet előkészítése az első alkalmazás frissítés tooget hello MongoDB-példány fogad egy környezeti változó lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-228">It also ensures that hello environment has been configured in preparation for receiving our first application update tooget hello MongoDB instance from an environment variable.</span></span>

### <a name="configuring-your-development-machine"></a><span data-ttu-id="9d7e7-229">A fejlesztői számítógépén konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d7e7-229">Configuring your Development Machine</span></span>
<span data-ttu-id="9d7e7-230">Most folytassuk a fejlesztési számítógépén toohello webes előtér virtuális gép csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-230">Now let's get your development machine hooked up toohello web frontend virtual machine.</span></span> <span data-ttu-id="9d7e7-231">Ez a lehető legegyszerűbb hello operációs rendszer tárház hozzáadása távoli tárházként hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-231">This is as simple as adding hello bare repository on hello virtual machine as a remote.</span></span> <span data-ttu-id="9d7e7-232">Futtassa a következő parancs toodo hello ez (cseréje *felhasználói* a webes előtér virtuális gép bejelentkezési névvel és *machinename* és *régió* szükség szerint):</span><span class="sxs-lookup"><span data-stu-id="9d7e7-232">Run hello following command toodo this (replacing *user* with your web frontend virtual machine login name and *machinename* and *region* as appropriate):</span></span>

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

<span data-ttu-id="9d7e7-233">Ez az összes szükséges tooenable kérdez le, vagy a közzétételi érvényben, toohello webes előtér virtuális gép vált.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-233">This is all that is needed tooenable pushing, or in effect publishing, changes toohello web frontend virtual machine.</span></span>

### <a name="publishing-updates"></a><span data-ttu-id="9d7e7-234">Közzétételi frissítések</span><span class="sxs-lookup"><span data-stu-id="9d7e7-234">Publishing Updates</span></span>
<span data-ttu-id="9d7e7-235">Most közzététele hello egy módosítás tett eddig, hogy hello alkalmazás fogja használni a saját MongoDB-példány:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-235">Let's publish hello one change that has been made so far so that hello application will use our own MongoDB instance:</span></span>

    git push azure master

<span data-ttu-id="9d7e7-236">Kimeneti hasonló toothis kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-236">You should see output similar toothis:</span></span>

    Counting objects: 4, done.
    Delta compression using up too4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    toousername@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

<span data-ttu-id="9d7e7-237">Ez a parancs után frissítse hello alkalmazást egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-237">After this command completes, try refreshing hello application in a web browser.</span></span> <span data-ttu-id="9d7e7-238">Képes toosee itt bemutatott hello teendőlista üres, és már nem a megosztott kapcsolt toohello MongoDB-példány telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-238">You should be able toosee that hello TODO list presented here is empty and no longer tied toohello shared deployed MongoDB instance.</span></span>

<span data-ttu-id="9d7e7-239">az oktatóanyagban toocomplete hello most Meggyőződünk egy másik, több látható módosítása.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-239">toocomplete hello tutorial, let's make another, more visible change.</span></span> <span data-ttu-id="9d7e7-240">A fejlesztői számítógépén nyissa meg a kedvenc szerkesztőjével hello node-todo/public/index.html fájl.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-240">On your development machine, open hello node-todo/public/index.html file using your favorite editor.</span></span> <span data-ttu-id="9d7e7-241">Keresse meg a hello jumbotron fejlécet, és módosítsa a "Vagyok a teendőlista aholic" túl "vagyok a teendőlista-aholic Azure!" hello címet.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-241">Locate hello jumbotron header and change  hello title from "I'm a Todo-aholic" too"I'm a Todo-aholic on Azure!".</span></span>

<span data-ttu-id="9d7e7-242">Most tegyük véglegesítése:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-242">Now let's commit:</span></span>

    git commit -am "Azurify hello title"

<span data-ttu-id="9d7e7-243">Ez alkalommal, most közzététele előtt azt tooback toohello GitHub-tárház hello módosítása tooAzure:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-243">This time, let's publish hello change tooAzure before pushing it tooback toohello GitHub repo:</span></span>

    git push azure master

<span data-ttu-id="9d7e7-244">Ez a parancs után frissítési hello weblapot, és akkor jelenik meg hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-244">Once this command completes, refresh hello web page and you will see hello changes.</span></span> <span data-ttu-id="9d7e7-245">Mivel azok szépen, hello módosítás vissza toohello származási távoli leküldéses:</span><span class="sxs-lookup"><span data-stu-id="9d7e7-245">Since they look good, push hello change back toohello origin remote:</span></span> 

    git push origin master

## <a name="next-steps"></a><span data-ttu-id="9d7e7-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9d7e7-246">Next Steps</span></span>
<span data-ttu-id="9d7e7-247">Ez a cikk bemutatta, hogyan tootake egy Node.js-alkalmazást, és telepítse azt az Azure-ban futó tooLinux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="9d7e7-247">This article showed how tootake a Node.js application and deploy it tooLinux virtual machines running in Azure.</span></span> <span data-ttu-id="9d7e7-248">További információ a Linux virtuális gépek Azure-ban toolearn lásd [bemutatása tooLinux Azure](/documentation/articles/virtual-machines-linux-introduction/).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-248">toolearn more about Linux virtual machines in Azure, see [Introduction tooLinux on Azure](/documentation/articles/virtual-machines-linux-introduction/).</span></span>

<span data-ttu-id="9d7e7-249">További információ toodevelop Node.js-alkalmazások Azure, lásd: hello [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="9d7e7-249">For more information about how toodevelop Node.js applications on Azure, see hello [Node.js Developer Center](/develop/nodejs/).</span></span>

