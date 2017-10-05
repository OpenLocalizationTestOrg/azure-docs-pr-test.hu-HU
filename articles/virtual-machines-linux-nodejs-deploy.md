---
title: "Egy Linux virtuális gépek Azure-ban Node.js-alkalmazás központi telepítése"
description: "Linux virtuális gépek Azure-ban egy Node.js-alkalmazás telepítésének megismerése."
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
ms.openlocfilehash: d3d9fecfafb9ba422420230716b9c985481d1e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-nodejs-application-to-linux-virtual-machines-in-azure"></a><span data-ttu-id="4a7b0-103">Egy Linux virtuális gépek Azure-ban Node.js-alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="4a7b0-103">Deploy a Node.js application to Linux Virtual Machines in Azure</span></span>
<span data-ttu-id="4a7b0-104">Ez az oktatóanyag bemutatja, hogyan igénybe vehet egy Node.js-alkalmazást, és telepítse azt az Azure-ban futó Linux virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-104">This tutorial shows how to take a Node.js application and deploy it to Linux virtual machines running in Azure.</span></span> <span data-ttu-id="4a7b0-105">Az oktatóanyag utasításai követhetők bármely olyan operációs rendszeren, amely alkalmas a Node.js futtatására.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-105">The instructions in this tutorial can be followed on any operating system that is capable of running Node.js.</span></span>

<span data-ttu-id="4a7b0-106">Megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-106">You'll learn how to:</span></span>

* <span data-ttu-id="4a7b0-107">Elágazás és a klón egy egyszerű teendőlistát Megvalósító alkalmazás; tartalmazó GitHub-tárházat</span><span class="sxs-lookup"><span data-stu-id="4a7b0-107">Fork and clone a GitHub repository containing a simple TODO application;</span></span>
* <span data-ttu-id="4a7b0-108">Hozzon létre, és két Linux virtuális gép konfigurálása az Azure-futtatni az alkalmazást;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-108">Create and configure two Linux virtual machines in Azure to run the application;</span></span>
* <span data-ttu-id="4a7b0-109">Az alkalmazás többször a webes előtér virtuális gép frissítések küldésével.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-109">Iterate on the application by pushing updates to the web frontend virtual machine.</span></span>

> [!NOTE]
> <span data-ttu-id="4a7b0-110">Az oktatóanyag teljesítéséhez szüksége van GitHub-fiók és a Microsoft Azure-fiók és a Git fejlesztési gépről használhatja.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-110">To complete this tutorial, you need a GitHub account and a Microsoft Azure account, and the ability to use Git from a development machine.</span></span>
> 
> <span data-ttu-id="4a7b0-111">Ha a GitHub-fiók nem rendelkezik, iratkozzon fel a [Itt](https://github.com/join).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-111">If you don't have a GitHub account, you can sign up [here](https://github.com/join).</span></span>
> 
> <span data-ttu-id="4a7b0-112">Ha nem rendelkezik egy [Microsoft Azure](https://azure.microsoft.com/) fiókja, regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-112">If you don't have a [Microsoft Azure](https://azure.microsoft.com/) account, you can sign up for a FREE trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="4a7b0-113">Ez is végigvezeti a regisztrációs folyamat az egy [Microsoft Account](http://account.microsoft.com) Ha még nem rendelkezik egy.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-113">This will also lead you through the sign up process for a [Microsoft Account](http://account.microsoft.com) if you do not already have one.</span></span> <span data-ttu-id="4a7b0-114">Másik lehetőségként, ha a Visual Studio előfizetői, is [aktiválhatja MSDN előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-114">Alternatively, if you are a Visual Studio subscriber, you can [activate your MSDN benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
> 
> <span data-ttu-id="4a7b0-115">Ha még nem git a fejlesztői számítógépén, használata Macintosh- vagy Windows-számítógépen, telepítse a git [Itt](http://www.git-scm.com).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-115">If you do not have git on your development machine, then if you are using a Macintosh or Windows machine, install git from [here](http://www.git-scm.com).</span></span> <span data-ttu-id="4a7b0-116">Ha Linux használ, telepítse a git, legmegfelelőbb mechanizmus használatával, mint `sudo apt-get install git`.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-116">If you are using Linux, install git using the mechanism most appropriate for you, such as `sudo apt-get install git`.</span></span>
> 
> 

## <a name="forking-and-cloning-the-todo-application"></a><span data-ttu-id="4a7b0-117">Elágazó és a Klónozás a Teendőlista alkalmazás</span><span class="sxs-lookup"><span data-stu-id="4a7b0-117">Forking and Cloning the TODO Application</span></span>
<span data-ttu-id="4a7b0-118">Az oktatóprogram során használt Teendőlista alkalmazás megvalósítja a egy egyszerű webes előtér, amely a teendőlista nyomon követi a MongoDB-példány felett.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-118">The TODO application used by this tutorial implements a simple web frontend over a MongoDB instance that keeps track of a TODO list.</span></span> <span data-ttu-id="4a7b0-119">Nyissa meg a Githubon történő bejelentkezés után [Itt](https://github.com/stepro/node-todo) forkolhatja, a kapcsolat használatával jobb felső és az alkalmazás megkeresése.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-119">After signing in to GitHub, go [here](https://github.com/stepro/node-todo) to find the application and fork it using the link in the top right.</span></span> <span data-ttu-id="4a7b0-120">Ez hozzon létre egy tárház nevű fiókjában *accountname*/node-todo.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-120">This should create a repository in your account named *accountname*/node-todo.</span></span>

<span data-ttu-id="4a7b0-121">Most a fejlesztői gépen klónozza a tárházat:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-121">Now on your development machine, clone this repository:</span></span>

    git clone https://github.com/accountname/node-todo.git

<span data-ttu-id="4a7b0-122">Fogjuk használni a helyi klónozott tárház egy kicsit később a Forráskód módosítása során.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-122">We'll use this local clone of the repository a little later when making changes to the source code.</span></span>

## <a name="creating-and-configuring-the-linux-virtual-machines"></a><span data-ttu-id="4a7b0-123">Létrehozása és konfigurálása a Linux virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="4a7b0-123">Creating and Configuring the Linux Virtual Machines</span></span>
<span data-ttu-id="4a7b0-124">Azure Linux virtuális gépek használatával nyers számítás kiváló visszajelzési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-124">Azure has great support for raw compute using Linux virtual machines.</span></span> <span data-ttu-id="4a7b0-125">Ez az oktatóanyag azt mutatja be hogyan könnyen léptetési két Linux virtuális gépek és a Teendőlista alkalmazás, központi telepítése egyet, majd a MongoDB-példány fut a webes előtér részét.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-125">This part of the tutorial shows how you can easily spin up two Linux virtual machines and deploy the TODO application to them, running the web frontend on one and the MongoDB instance on the other.</span></span>

### <a name="creating-virtual-machines"></a><span data-ttu-id="4a7b0-126">Virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-126">Creating Virtual Machines</span></span>
<span data-ttu-id="4a7b0-127">A legegyszerűbben úgy, hogy egy új virtuális gép létrehozása az Azure-ban az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-127">The easiest way to create a new virtual machine in Azure is to use the Azure Portal.</span></span> <span data-ttu-id="4a7b0-128">Kattintson a [Itt](https://portal.azure.com) jelentkezzen be, majd indítsa el az Azure-portálon a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-128">Click [here](https://portal.azure.com) to sign in and launch the Azure Portal in your web browser.</span></span> <span data-ttu-id="4a7b0-129">Miután az Azure portál be van töltve, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-129">Once the Azure Portal has loaded, complete the following steps:</span></span>

* <span data-ttu-id="4a7b0-130">A "+ új" hivatkozásra;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-130">Click the "+ New" link;</span></span>
* <span data-ttu-id="4a7b0-131">Válassza ki a "Számítási" kategória, és válassza a "Ubuntu Server 14.04 LTS";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-131">Pick the "Compute" category and choose "Ubuntu Server 14.04 LTS";</span></span>
* <span data-ttu-id="4a7b0-132">Válassza ki a "erőforrás-kezelő" üzembe helyezési modellel, és kattintson a "Létrehozás";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-132">Select the "Resource Manager" deployment model and click "Create";</span></span>
* <span data-ttu-id="4a7b0-133">Töltse ki az alábbi utasításokat követve alapvető:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-133">Fill in the basics following these guidelines:</span></span>
  * <span data-ttu-id="4a7b0-134">Adjon meg egy újabb; könnyen azonosítható nevet</span><span class="sxs-lookup"><span data-stu-id="4a7b0-134">Specify a name you can easily identify later;</span></span>
  * <span data-ttu-id="4a7b0-135">A jelen oktatóanyag esetében válassza a jelszó-hitelesítés;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-135">For this tutorial, choose Password authentication;</span></span>
  * <span data-ttu-id="4a7b0-136">Hozzon létre egy új erőforráscsoportot azonosítható névvel.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-136">Create a new resource group with an identifiable name.</span></span>
* <span data-ttu-id="4a7b0-137">A virtuálisgép-méret "A1 szabványos" esetén ésszerű választás ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-137">For the Virtual Machine size, "A1 Standard" is a reasonable choice for this tutorial.</span></span>
* <span data-ttu-id="4a7b0-138">A további beállításokhoz ellenőrizze a lemez típusát "Standard", és fogadja el a többi alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-138">For additional settings, ensure the disk type is "Standard" and accept all the remaining defaults.</span></span>
* <span data-ttu-id="4a7b0-139">Az összefoglalás lapon létrehozása indítsa.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-139">Kick off the creation on the summary page.</span></span>

<span data-ttu-id="4a7b0-140">Hajtsa végre a fenti kétszer létrehozásának folyamata két Linux virtuális gépek, egyet a webes előtér és egyet a MongoDB-példány.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-140">Perform the above process twice to create two Linux virtual machines, one for the web frontend and one for the MongoDB instance.</span></span> <span data-ttu-id="4a7b0-141">A virtuális gépek létrehozását körülbelül 5 – 10 percig tart.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-141">Creation of the virtual machines will take about 5-10 minutes.</span></span>

### <a name="assigning-a-dns-entry-for-virtual-machines"></a><span data-ttu-id="4a7b0-142">A DNS-bejegyzés hozzárendelése a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="4a7b0-142">Assigning a DNS entry for Virtual Machines</span></span>
<span data-ttu-id="4a7b0-143">Az Azure-ban létrehozott virtuális gépek csak egy nyilvános IP-címet, például 1.2.3.4 keresztül érhető el alapértelmezés szerint is.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-143">Virtual machines created in Azure are by default only accessible through a public IP address like 1.2.3.4.</span></span> <span data-ttu-id="4a7b0-144">Ellenőrizze a gép könnyebben azonosítható DNS-bejegyzések hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-144">Let's make the machines more easily identifiable by assigning them DNS entries.</span></span>

<span data-ttu-id="4a7b0-145">Miután a portál azt jelzi, hogy létrejöttek-e a virtuális gépek, a bal oldali navigációs sávja a "Virtual machines" hivatkozásra, és keresse meg a gépek.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-145">Once the portal indicates that the virtual machines have been created, click on the "Virtual machines" link in the left navbar and locate your machines.</span></span> <span data-ttu-id="4a7b0-146">Az egyes gépek:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-146">For each machine:</span></span>

* <span data-ttu-id="4a7b0-147">Keresse meg az alapvető erőforrások lapon, majd kattintson a nyilvános IP-címét;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-147">Locate the Essentials tab and click on the Public IP Address;</span></span>
* <span data-ttu-id="4a7b0-148">A nyilvános IP-címének konfigurációja a DNS-névcímke meg és mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-148">In the public IP address configuration, assign a DNS name label and save.</span></span>

<span data-ttu-id="4a7b0-149">A portál biztosítja, hogy rendelkezésre áll-e a megadott név.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-149">The portal will ensure that the name you specify is available.</span></span> <span data-ttu-id="4a7b0-150">A konfiguráció mentése, után a virtuális gépek állomás neve hasonlít kell `machinename.region.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-150">After saving the configuration, your virtual machines will have host names similar to `machinename.region.cloudapp.azure.com`.</span></span>

### <a name="connecting-to-the-virtual-machines"></a><span data-ttu-id="4a7b0-151">A virtuális gépekhez való csatlakozás</span><span class="sxs-lookup"><span data-stu-id="4a7b0-151">Connecting to the Virtual Machines</span></span>
<span data-ttu-id="4a7b0-152">Ha a virtuális gépek kiépített, amilyenek korábban voltak előre konfigurált SSH-n keresztül a távoli kapcsolatok lehetővé tételéhez.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-152">When your virtual machines were provisioned, they were pre-configured to allow remote connections over SSH.</span></span> <span data-ttu-id="4a7b0-153">Ez az a mechanizmus segítségével a virtuális gépet állíthat be.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-153">This is the mechanism we will use to configure the virtual machines.</span></span> <span data-ttu-id="4a7b0-154">Használatakor a Windows a fejlesztési, szüksége lesz egy SSH-ügyfél segítségével, ha még nem rendelkezik egy.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-154">If you are using Windows for your development, you will need to get an SSH client if you do not already have one.</span></span> <span data-ttu-id="4a7b0-155">Egy közös választott, amely letölthető a PuTTY [Itt](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-155">A common choice here is PuTTY, which can be downloaded from [here](http://www.chiark.greenend.org.uk/~sgtatham/putty/).</span></span> <span data-ttu-id="4a7b0-156">A Macintosh- és Linux operációs rendszer származnak SSH előre telepített verziója.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-156">Macintosh and Linux OSes come with a version of SSH pre-installed.</span></span>

### <a name="configuring-the-web-frontend-virtual-machine"></a><span data-ttu-id="4a7b0-157">A webes előtér virtuális gép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-157">Configuring the Web Frontend Virtual Machine</span></span>
<span data-ttu-id="4a7b0-158">A webes előtér géphez létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz SSH.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-158">SSH to the web frontend machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="4a7b0-159">Egy parancs parancssori futtatásával követ üdvözlő üzenet megjelenik.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-159">You should see a welcome message followed by a command prompt.</span></span>

<span data-ttu-id="4a7b0-160">Első lépésként most Meggyőződünk arról, hogy a git szoftver, csomópont is telepítve vannak:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-160">First, let's make sure that git and node are both installed:</span></span>

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

<span data-ttu-id="4a7b0-161">Az alkalmazás webes előtér néhány natív Node.js modulok támaszkodik, mivel azt is telepítenie kell a build tools alapvető készlete:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-161">Since the application's web frontend relies on some native Node.js modules, we also need to install the essential set of build tools:</span></span>

    sudo apt-get install -y build-essential

<span data-ttu-id="4a7b0-162">Végezetül most telepíteni a Node.js-alkalmazás neve *végtelen*, ami segít Node.js server-alkalmazások futtatására:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-162">Finally, let's install a Node.js application called *forever*, which helps to run Node.js server applications:</span></span>

    sudo npm install -g forever

<span data-ttu-id="4a7b0-163">Ezek a minden a függőségeket, hogy az alkalmazás webes előtér futtatni, ezért folytassuk, hogy fut a virtuális gépen szükséges.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-163">These are all the dependencies needed on this virtual machine to be able to run the application's web frontend, so let's get that running.</span></span> <span data-ttu-id="4a7b0-164">Ehhez először létrehozunk egy operációs rendszer klónja korábban meg ágazik el, így könnyen frissítések közzétételéhez a virtuális gép (oktatóanyag a következőket ismerteti a frissítési forgatókönyv később), és adja meg a tárház ténylegesen végrehajtható verzióját az operációs rendszer klónt majd klónozza a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-164">To do this, we will first create a bare clone of the GitHub repository you previously forked so that you can easily publish updates to the virtual machine (we'll cover this update scenario later), and then clone the bare clone to provide a version of the repository that can actually be executed.</span></span>

<span data-ttu-id="4a7b0-165">A (~) kezdőkönyvtára, kezdve a következő parancsokat (cseréje *accountname* a GitHub felhasználói fiók nevével):</span><span class="sxs-lookup"><span data-stu-id="4a7b0-165">Starting from the home (~) directory, run the following commands (replacing *accountname* with your GitHub user account name):</span></span>

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

<span data-ttu-id="4a7b0-166">Most adja meg a csomópont-todo könyvtárat, és futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-166">Now enter the node-todo directory and run these commands:</span></span>

    npm install
    forever start server.js

<span data-ttu-id="4a7b0-167">Az alkalmazás webes előtér innentől kezdve fut, azonban nincs egy további lépést egy webböngészőből próbál hozzáférni az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-167">The application's web frontend is now running, however there is one more step before you can access the application from a web browser.</span></span> <span data-ttu-id="4a7b0-168">A létrehozott virtuális gépet egy Azure-erőforrás neve védi egy *hálózati biztonsági csoport*, amely jött létre az Ön, amikor a virtuális gép létesített.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-168">The virtual machine you created is protected by an Azure resource called a *network security group*, which was created for you when you provisioned the virtual machine.</span></span> <span data-ttu-id="4a7b0-169">Jelenleg ez az erőforrás csak lehetővé teszi a külső kérelmek 22-es portot irányíthatja át a virtuális géphez, amely lehetővé teszi, hogy a gép, de semmi mást SSH-kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-169">Currently, this resource only allows external requests to port 22 to be routed to the virtual machine, which enables SSH communication with the machine but nothing else.</span></span> <span data-ttu-id="4a7b0-170">Így megtekintheti a Teendőlista alkalmazás, a 8080-as porton futtatásra van beállítva, amely ezt a portot is kell megnyitását.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-170">So in order to view the TODO application, which is configured to run on port 8080, this port also needs to be opened up.</span></span>

<span data-ttu-id="4a7b0-171">Térjen vissza az Azure portálra, és kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-171">Return to the Azure Portal and complete the following steps:</span></span>

* <span data-ttu-id="4a7b0-172">Kattintson a "Erőforráscsoportok" a bal oldali navigációs sávja;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-172">Click on "Resource groups" in the left navbar;</span></span>
* <span data-ttu-id="4a7b0-173">Válassza ki az erőforrás-csoportot, amely tartalmazza a virtuális gép;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-173">Select the resource group that contains your virtual machine;</span></span>
* <span data-ttu-id="4a7b0-174">Az eredményül kapott erőforrások, jelölje ki a hálózati biztonsági csoport (egy pajzs ikon egy);</span><span class="sxs-lookup"><span data-stu-id="4a7b0-174">In the resulting list of resources, select the network security group (the one with a shield icon);</span></span>
* <span data-ttu-id="4a7b0-175">Válassza a Tulajdonságok "bejövő biztonsági szabályok";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-175">In the properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="4a7b0-176">Az eszköztáron kattintson a "Hozzáadás";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-176">In the toolbar, click "Add";</span></span>
* <span data-ttu-id="4a7b0-177">Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-teendőlista";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-177">Provide a name like "default-allow-todo";</span></span>
* <span data-ttu-id="4a7b0-178">A "TCP"; a protokoll beállítása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-178">Set the protocol to "TCP";</span></span>
* <span data-ttu-id="4a7b0-179">Állítsa be a Célporttartomány "8080";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-179">Set the destination port range to "8080";</span></span>
* <span data-ttu-id="4a7b0-180">Kattintson az OK gombra, és várja meg a létrehozandó szabály.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-180">Click OK and wait for the security rule to be created.</span></span>

<span data-ttu-id="4a7b0-181">A biztonsági szabály létrehozása után a Teendőlista alkalmazás nyilvánosan látható az interneten, és tallózással is kikeresheti azt, például, mint az egy URL-cím segítségével:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-181">After creating this security rule, the TODO application is publically visible on the internet and you can browse to it, for instance using a URL such as:</span></span>

    http://machinename.region.cloudapp.azure.com:8080

<span data-ttu-id="4a7b0-182">Megfigyelheti, hogy annak ellenére, hogy még nincs konfigurálva a MongoDB virtuális gépet, a Teendőlista alkalmazás úgy tűnik, hogy teljesen működőképes.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-182">You will notice that even though we have not yet configured the MongoDB virtual machine, the TODO application appears to be quite functional.</span></span> <span data-ttu-id="4a7b0-183">Ez azért, mert a tárházban szoftveresen kötött előre telepített MongoDB-példány használata.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-183">This is because the source repository is hardcoded to use a pre-deployed MongoDB instance.</span></span> <span data-ttu-id="4a7b0-184">Miután a MongoDB-virtuális gép rendelkezik konfigurált, rendszer lépjen vissza és a titkos MongoDB-példány helyett magukat, hogy a Forráskód módosítása.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-184">Once we have configured the MongoDB virtual machine, we will go back and change the source code to utilize our private MongoDB instance instead.</span></span>

### <a name="configuring-the-mongodb-virtual-machine"></a><span data-ttu-id="4a7b0-185">A MongoDB-virtuális gép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-185">Configuring the MongoDB Virtual Machine</span></span>
<span data-ttu-id="4a7b0-186">A második géphez létrehozott, a PuTTY ssh parancssorból vagy a kedvenc SSH eszköz SSH.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-186">SSH to the second machine you created using PuTTY, ssh command line or your other favorite SSH tool.</span></span> <span data-ttu-id="4a7b0-187">A MongoDB telepítése után megtekintheti az üdvözlő üzenet és a parancssort, (ezek az utasítások vették [Itt](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span><span class="sxs-lookup"><span data-stu-id="4a7b0-187">After seeing the welcome message and command prompt, install MongoDB (these instructions were taken from [here](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):</span></span>

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

<span data-ttu-id="4a7b0-188">Alapértelmezés szerint MongoDB van beállítva, ezért csak elérhető helyileg.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-188">By default, MongoDB is configured so it can only be accessed locally.</span></span> <span data-ttu-id="4a7b0-189">Ebben az oktatóanyagban azt konfigurálja MongoDB, a rendszer az elérhető legyen a az alkalmazás virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-189">For this tutorial, we will configure MongoDB so it can be accessed from the application's virtual machine.</span></span> <span data-ttu-id="4a7b0-190">A sudo a környezetben, nyissa meg a /etc/mongod.conf fájlt, és keresse meg a `# network interfaces` szakasz.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-190">In a sudo context, open the /etc/mongod.conf file and locate the `# network interfaces` section.</span></span> <span data-ttu-id="4a7b0-191">Módosítsa a `net.bindIp` konfigurációs érték `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-191">Change the `net.bindIp` configuration value to `0.0.0.0`.</span></span>

> [!NOTE]
> <span data-ttu-id="4a7b0-192">Ez a konfiguráció akkor csak ez az oktatóanyag céljából.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-192">This configuration is for the purposes of this tutorial only.</span></span> <span data-ttu-id="4a7b0-193">Az **nem** egy ajánlott biztonsági eljárás, és nem használható üzemi környezetben.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-193">It is **NOT** a recommended security practice and should not be used in production environments.</span></span>
> 
> 

<span data-ttu-id="4a7b0-194">Most ellenőrizze, hogy a szolgáltatás el van indítva MongoDB:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-194">Now ensure the MongoDB service has been started:</span></span>

    sudo service mongod restart

<span data-ttu-id="4a7b0-195">MongoDB porton 27017 alapértelmezés szerint működik.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-195">MongoDB operates over port 27017 by default.</span></span> <span data-ttu-id="4a7b0-196">Igen azonos módon, hogy változtatnunk kell, nyissa meg a webes előtér virtuális gépen a 8080-as porton, igazolnia kell nyissa meg a portot 27017 a MongoDB virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-196">So, in the same way that we needed to open port 8080 on the web frontend virtual machine, we need to open port 27017 on the MongoDB virtual machine.</span></span>

<span data-ttu-id="4a7b0-197">Térjen vissza az Azure portálra, és kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-197">Return to the Azure Portal and complete the following steps:</span></span>

* <span data-ttu-id="4a7b0-198">Kattintson a "Erőforráscsoportok" a bal oldali navigációs sávja;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-198">Click on "Resource groups" in the left navbar;</span></span>
* <span data-ttu-id="4a7b0-199">Válassza ki az erőforráscsoportot, amely tartalmazza a MongoDB-virtuális gép;</span><span class="sxs-lookup"><span data-stu-id="4a7b0-199">Select the resource group that contains the MongoDB virtual machine;</span></span>
* <span data-ttu-id="4a7b0-200">Az eredményül kapott erőforrások, jelölje ki a hálózati biztonsági csoport (egy a pajzs ikon) ugyanazzal a névvel, amelyek a MongoDB-virtuális gép; adott</span><span class="sxs-lookup"><span data-stu-id="4a7b0-200">In the resulting list of resources, select the network security group (the one with a shield icon) with the same name that you gave to the MongoDB virtual machine;</span></span>
* <span data-ttu-id="4a7b0-201">Válassza a Tulajdonságok "bejövő biztonsági szabályok";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-201">In the properties, choose "Inbound security rules";</span></span>
* <span data-ttu-id="4a7b0-202">Az eszköztáron kattintson a "Hozzáadás";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-202">In the toolbar, click "Add";</span></span>
* <span data-ttu-id="4a7b0-203">Adjon meg egy nevet, például a "alapértelmezett-engedélyezése-mongo";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-203">Provide a name like "default-allow-mongo";</span></span>
* <span data-ttu-id="4a7b0-204">A "TCP"; a protokoll beállítása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-204">Set the protocol to "TCP";</span></span>
* <span data-ttu-id="4a7b0-205">Állítsa be a Célporttartomány "27017";</span><span class="sxs-lookup"><span data-stu-id="4a7b0-205">Set the destination port range to "27017";</span></span>
* <span data-ttu-id="4a7b0-206">Kattintson az OK gombra, és várja meg a létrehozandó szabály.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-206">Click OK and wait for the security rule to be created.</span></span>

## <a name="iterating-on-the-todo-application"></a><span data-ttu-id="4a7b0-207">A Teendőlista alkalmazás a léptetés</span><span class="sxs-lookup"><span data-stu-id="4a7b0-207">Iterating on the TODO application</span></span>
<span data-ttu-id="4a7b0-208">Az eddigi jelenleg két Linux virtuális gépek kiépítése: egyet, hogy fut az alkalmazás webes előtér- és egy, a MongoDB-példány fut.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-208">So far, we have provisioned two Linux virtual machines: one that is running the application's web frontend and one that is running a MongoDB instance.</span></span> <span data-ttu-id="4a7b0-209">De a probléma merül fel, mert a webes előtér nem használja a kiosztott MongoDB-példány még.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-209">But there is a problem - the web frontend isn't actually using the provisioned MongoDB instance yet.</span></span> <span data-ttu-id="4a7b0-210">Most javíthatja ki, hogy a webes előtér-kód kódolt példány helyett egy környezeti változó használatával frissítése.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-210">Let's fix that by updating the web frontend code to use an environment variable instead of a hard-coded instance.</span></span>

### <a name="changing-the-todo-application"></a><span data-ttu-id="4a7b0-211">A Teendőlista alkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-211">Changing the TODO application</span></span>
<span data-ttu-id="4a7b0-212">A fejlesztői számítógépén, ha először klónozása a csomópont-todo tárház, nyissa meg a `node-todo/config/database.js` a kedvenc szerkesztő fájlt, és módosítsa az URL-cím a kódolt érték például `mongodb://...` való `process.env.MONGODB`.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-212">On your development machine where you first cloned the node-todo repository, open the `node-todo/config/database.js` file in your favorite editor and change the url value from the hard-coded value like `mongodb://...` to `process.env.MONGODB`.</span></span>

<span data-ttu-id="4a7b0-213">A változtatások véglegesítése a határidő, és küldje le a GitHub-főkiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-213">Commit your changes and push to the GitHub master:</span></span>

    git commit -am "Get MongoDB instance from env"
    git push origin master

<span data-ttu-id="4a7b0-214">Sajnos ez nem közzéteheti a módosítást a webes előtér virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-214">Unfortunately, this doesn't publish the change to the web frontend virtual machine.</span></span> <span data-ttu-id="4a7b0-215">Most Meggyőződünk néhány további módosításokat egy egyszerű, de hatékony mechanizmus a frissítések közzétételéhez, így gyorsan figyelheti a változásokat az éles környezetben engedélyezése virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-215">Let's make a few more changes to that virtual machine to enable a simple but effective mechanism for publishing updates so you can quickly observe the effect of the changes in the live environment.</span></span>

### <a name="configuring-the-web-frontend-virtual-machine"></a><span data-ttu-id="4a7b0-216">A webes előtér virtuális gép konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-216">Configuring the Web Frontend Virtual Machine</span></span>
<span data-ttu-id="4a7b0-217">Előhívja a korábban létrehozott egy operációs rendszer klón a csomópont-todo tárház a webes előtér virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-217">Recall that we previously created a bare clone of the node-todo repository on the web frontend virtual machine.</span></span> <span data-ttu-id="4a7b0-218">Az elemről kiderül, hogy ez a művelet létrehozott egy új távoli Git, amelyhez a módosítások továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-218">It turns out that this action created a new Git remote to which changes can be pushed.</span></span> <span data-ttu-id="4a7b0-219">Azonban egyszerűen küldését a ebben a távoli nem elég biztosítják a gyors iterációs modellt, amely a fejlesztők olyan eszközökre vonatkozóan feldolgozása során a rendszer a kódot.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-219">However, simply pushing to this remote doesn't quite give the rapid iteration model that developers are looking for when working on their code.</span></span>

<span data-ttu-id="4a7b0-220">Mi szeretnénk végezhető el, győződjön meg arról, hogy a virtuális gépen a távoli tárházba leküldéses akkor fordul elő, amikor a futó Teendőlista alkalmazás automatikusan frissül.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-220">What we would like to be able to do is ensure that when a push to the remote repository on the virtual machine occurs, the running TODO application is automatically updated.</span></span> <span data-ttu-id="4a7b0-221">Szerencsére Ez az egyszerű git eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-221">Fortunately, this is easy to achieve with git.</span></span>

<span data-ttu-id="4a7b0-222">Git egy hurkok, melynek neve adott időpontokban reagálni a tárház végzett műveletek számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-222">Git exposes a number of hooks that are called at particular times to react to actions taken on the repository.</span></span> <span data-ttu-id="4a7b0-223">Ezek megjelennek a rendszerhéj-parancsfájlok használata a tárházban `hooks` mappát.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-223">These are specified using shell scripts in the repository's `hooks` folder.</span></span> <span data-ttu-id="4a7b0-224">A leginkább megfelelő az automatikus frissítési forgatókönyv hook van a `post-update` esemény.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-224">The hook that is most applicable for the auto-update scenario is the `post-update` event.</span></span>

<span data-ttu-id="4a7b0-225">Az SSH-munkamenetet a webes előtér virtuális géphez, így a `~/node-todo.git/hooks` könyvtár, és adja hozzá a tartalmat a következő nevű fájlba `post-update` (cseréje `machinename` és `region` a MongoDB-virtuális gép adatait):</span><span class="sxs-lookup"><span data-stu-id="4a7b0-225">In a SSH session to the web frontend virtual machine, change to the `~/node-todo.git/hooks` directory and add the following content to a file named `post-update` (replacing `machinename` and `region` with your MongoDB virtual machine information):</span></span>

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

<span data-ttu-id="4a7b0-226">Győződjön meg arról, ez a fájl végrehajtható, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-226">Ensure this file is executable by running the following command:</span></span>

    chmod 755 post-update

<span data-ttu-id="4a7b0-227">Ez a parancsfájl biztosítja, hogy az aktuális kiszolgáló alkalmazás le van állítva, a klónozott tárház a kód frissítése, hogy a legújabb, frissített függőségek teljesülnek, és a kiszolgáló újraindítása.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-227">This script ensures that the current server application is stopped, the code in the cloned repository is updated to the latest, any updated dependencies are satisfied, and the server is restarted.</span></span> <span data-ttu-id="4a7b0-228">Emellett biztosítja, hogy a környezet előkészítése a fogadásához az első alkalmazás frissítése a MongoDB-példány lekérése egy környezeti változó lett konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-228">It also ensures that the environment has been configured in preparation for receiving our first application update to get the MongoDB instance from an environment variable.</span></span>

### <a name="configuring-your-development-machine"></a><span data-ttu-id="4a7b0-229">A fejlesztői számítógépén konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4a7b0-229">Configuring your Development Machine</span></span>
<span data-ttu-id="4a7b0-230">Most folytassuk a fejlesztési számítógépén csatlakoztatnia kell a webes előtér virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-230">Now let's get your development machine hooked up to the web frontend virtual machine.</span></span> <span data-ttu-id="4a7b0-231">Ez a más dolga, mint az operációs rendszer tárház hozzáadása a virtuális gépen, mint egy távoli.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-231">This is as simple as adding the bare repository on the virtual machine as a remote.</span></span> <span data-ttu-id="4a7b0-232">Ehhez a következő parancsot (cseréje *felhasználói* a webes előtér virtuális gép bejelentkezési névvel és *machinename* és *régió* szükség szerint):</span><span class="sxs-lookup"><span data-stu-id="4a7b0-232">Run the following command to do this (replacing *user* with your web frontend virtual machine login name and *machinename* and *region* as appropriate):</span></span>

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

<span data-ttu-id="4a7b0-233">Mást nem kérdez le engedélyezéséhez szükséges, vagy érvényben közzététel, megváltozik a webes előtér virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-233">This is all that is needed to enable pushing, or in effect publishing, changes to the web frontend virtual machine.</span></span>

### <a name="publishing-updates"></a><span data-ttu-id="4a7b0-234">Közzétételi frissítések</span><span class="sxs-lookup"><span data-stu-id="4a7b0-234">Publishing Updates</span></span>
<span data-ttu-id="4a7b0-235">Most közzététele a egy módosítás tett eddig, hogy az alkalmazás saját MongoDB-példány használja:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-235">Let's publish the one change that has been made so far so that the application will use our own MongoDB instance:</span></span>

    git push azure master

<span data-ttu-id="4a7b0-236">Ez hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-236">You should see output similar to this:</span></span>

    Counting objects: 4, done.
    Delta compression using up to 4 threads.
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
    To username@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

<span data-ttu-id="4a7b0-237">Ez a parancs után frissítse az alkalmazást egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-237">After this command completes, try refreshing the application in a web browser.</span></span> <span data-ttu-id="4a7b0-238">Tudja meg, hogy az itt bemutatott teendőlista üres és a megosztott telepített MongoDB-példány már nem kötött kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-238">You should be able to see that the TODO list presented here is empty and no longer tied to the shared deployed MongoDB instance.</span></span>

<span data-ttu-id="4a7b0-239">Az oktatóanyag elvégzéséhez most Meggyőződünk egy másik, több látható módosítása.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-239">To complete the tutorial, let's make another, more visible change.</span></span> <span data-ttu-id="4a7b0-240">A fejlesztői számítógépén nyissa meg node-todo/public/index.html a kedvenc szerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-240">On your development machine, open the node-todo/public/index.html file using your favorite editor.</span></span> <span data-ttu-id="4a7b0-241">Keresse meg a jumbotron fejlécet, és módosítsa a címet a "Vagyok a teendőlista aholic" a "Vagyok a teendőlista-aholic Azure!".</span><span class="sxs-lookup"><span data-stu-id="4a7b0-241">Locate the jumbotron header and change  the title from "I'm a Todo-aholic" to "I'm a Todo-aholic on Azure!".</span></span>

<span data-ttu-id="4a7b0-242">Most tegyük véglegesítése:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-242">Now let's commit:</span></span>

    git commit -am "Azurify the title"

<span data-ttu-id="4a7b0-243">Ez alkalommal, most közzéteheti a módosítást az Azure-bA előtt, hogy a GitHub-tárház biztonsági:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-243">This time, let's publish the change to Azure before pushing it to back to the GitHub repo:</span></span>

    git push azure master

<span data-ttu-id="4a7b0-244">Ez a parancs után frissítse a weblapot, és látni fogja a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-244">Once this command completes, refresh the web page and you will see the changes.</span></span> <span data-ttu-id="4a7b0-245">Mivel azok szépen, küldje le a módosítás vissza és a forrás távoli:</span><span class="sxs-lookup"><span data-stu-id="4a7b0-245">Since they look good, push the change back to the origin remote:</span></span> 

    git push origin master

## <a name="next-steps"></a><span data-ttu-id="4a7b0-246">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a7b0-246">Next Steps</span></span>
<span data-ttu-id="4a7b0-247">Ez a cikk bemutatta, hogyan számára a Node.js-alkalmazás és a központilag telepítenie kell az Azure-ban futó Linux virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="4a7b0-247">This article showed how to take a Node.js application and deploy it to Linux virtual machines running in Azure.</span></span> <span data-ttu-id="4a7b0-248">Linux virtuális gépek Azure-ban kapcsolatos további információkért lásd: [Bevezetés az Azure-on Linux](/documentation/articles/virtual-machines-linux-introduction/).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-248">To learn more about Linux virtual machines in Azure, see [Introduction to Linux on Azure](/documentation/articles/virtual-machines-linux-introduction/).</span></span>

<span data-ttu-id="4a7b0-249">A Node.js-alkalmazásoknak az Azure-on történő fejlesztéséről további információkat a következő témakörben talál: [Node.js fejlesztői központ](/develop/nodejs/).</span><span class="sxs-lookup"><span data-stu-id="4a7b0-249">For more information about how to develop Node.js applications on Azure, see the [Node.js Developer Center](/develop/nodejs/).</span></span>

