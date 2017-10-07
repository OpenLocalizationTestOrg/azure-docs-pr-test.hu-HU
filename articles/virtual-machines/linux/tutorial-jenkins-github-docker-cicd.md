---
title: "az Azure-ban Jenkins fejlesztési folyamat aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Jenkins virtuális gépet, hogy minden egyes kódja a Githubon ponttá véglegesíteni és összeállít egy új Docker tároló toorun Azure-ban az alkalmazás"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: c079e3c9186c9da0a3e51e1823215779c565e0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a><span data-ttu-id="daad7-103">Hogyan toocreate egy Linux virtuális Gépet az Azure-ban Jenkins, a Githubon és a Docker-fejlesztési infrastruktúra</span><span class="sxs-lookup"><span data-stu-id="daad7-103">How toocreate a development infrastructure on a Linux VM in Azure with Jenkins, GitHub, and Docker</span></span>
<span data-ttu-id="daad7-104">tooautomate hello build, és tesztelési fázis alkalmazásának fejlesztését, használhatja a folyamatos integrációt és a központi telepítés (CI/CD) folyamat.</span><span class="sxs-lookup"><span data-stu-id="daad7-104">tooautomate hello build and test phase of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="daad7-105">Ebben az oktatóanyagban létrehoz egy CI/CD folyamat egy Azure virtuális gépen történő is beleértve:</span><span class="sxs-lookup"><span data-stu-id="daad7-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="daad7-106">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="daad7-107">Telepítse és konfigurálja a Jenkins</span><span class="sxs-lookup"><span data-stu-id="daad7-107">Install and configure Jenkins</span></span>
> * <span data-ttu-id="daad7-108">GitHub és Jenkins integrációjával webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-108">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="daad7-109">Hozzon létre és Jenkins feladatok létrehozása a Githubról eseményindító véglegesítése</span><span class="sxs-lookup"><span data-stu-id="daad7-109">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="daad7-110">Az alkalmazás Docker-lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-110">Create a Docker image for your app</span></span>
> * <span data-ttu-id="daad7-111">Ellenőrizze a Githubon véglegesíti és hozhat létre. új Docker-lemezkép alkalmazást futtató frissítések</span><span class="sxs-lookup"><span data-stu-id="daad7-111">Verify GitHub commits build new Docker image and updates running app</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="daad7-112">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="daad7-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="daad7-113">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="daad7-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="daad7-114">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="daad7-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-jenkins-instance"></a><span data-ttu-id="daad7-115">Jenkins példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-115">Create Jenkins instance</span></span>
<span data-ttu-id="daad7-116">Az oktatóanyag előző [hogyan toocustomize egy Linux virtuális gép első indításakor](tutorial-automate-vm-deployment.md), akkor megtanulta, hogyan tooautomate virtuális gép testreszabása a felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="daad7-116">In a previous tutorial on [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how tooautomate VM customization with cloud-init.</span></span> <span data-ttu-id="daad7-117">Ez az oktatóanyag a virtuális gép egy felhőben inicializálás fájl tooinstall Jenkins és a Docker használja.</span><span class="sxs-lookup"><span data-stu-id="daad7-117">This tutorial uses a cloud-init file tooinstall Jenkins and Docker on a VM.</span></span> 

<span data-ttu-id="daad7-118">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="daad7-118">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="daad7-119">A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="daad7-119">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="daad7-120">Adja meg `sensible-editor cloud-init-jenkins.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="daad7-120">Enter `sensible-editor cloud-init-jenkins.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="daad7-121">Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:</span><span class="sxs-lookup"><span data-stu-id="daad7-121">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
write_files:
  - path: /etc/systemd/system/docker.service.d/docker.conf
    content: |
      [Service]
        ExecStart=
        ExecStart=/usr/bin/dockerd
  - path: /etc/docker/daemon.json
    content: |
      {
        "hosts": ["fd://","tcp://127.0.0.1:2375"]
      }
runcmd:
  - wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | apt-key add -
  - sh -c 'echo deb http://pkg.jenkins-ci.org/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
  - apt-get update && apt-get install jenkins -y
  - curl -sSL https://get.docker.com/ | sh
  - usermod -aG docker azureuser
  - usermod -aG docker jenkins
  - service jenkins restart
```

<span data-ttu-id="daad7-122">A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="daad7-122">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="daad7-123">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupJenkins* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="daad7-123">hello following example creates a resource group named *myResourceGroupJenkins* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

<span data-ttu-id="daad7-124">Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="daad7-124">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="daad7-125">Használjon hello `--custom-data` paraméter toopass a felhő inicializálás konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="daad7-125">Use hello `--custom-data` parameter toopass in your cloud-init config file.</span></span> <span data-ttu-id="daad7-126">Adja meg a hello teljes elérési útja túl*felhő-init-jenkins.txt* ha kívül a jelen munkakönyvtár hello fájlt mentette.</span><span class="sxs-lookup"><span data-stu-id="daad7-126">Provide hello full path too*cloud-init-jenkins.txt* if you saved hello file outside of your present working directory.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

<span data-ttu-id="daad7-127">Hello VM toobe létrehozása és konfigurálása néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="daad7-127">It takes a few minutes for hello VM toobe created and configured.</span></span>

<span data-ttu-id="daad7-128">tooallow webes forgalom tooreach a virtuális gép használata [az vm-port megnyitása](/cli/azure/vm#open-port) tooopen port *8080* Jenkins forgalom és a port *1337* hello Node.js-alkalmazás, amely használt toorun egy mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="daad7-128">tooallow web traffic tooreach your VM, use [az vm open-port](/cli/azure/vm#open-port) tooopen port *8080* for Jenkins traffic and port *1337* for hello Node.js app that is used toorun a sample app:</span></span>

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a><span data-ttu-id="daad7-129">Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="daad7-129">Configure Jenkins</span></span>
<span data-ttu-id="daad7-130">tooaccess a Jenkins példány, szerezze be a virtuális gép hello nyilvános IP-címe:</span><span class="sxs-lookup"><span data-stu-id="daad7-130">tooaccess your Jenkins instance, obtain hello public IP address of your VM:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="daad7-131">Biztonsági okokból tooenter hello kezdeti rendszergazdai jelszó tárolt szövegfájlba a virtuális gép toostart hello Jenkins telepíteni kell.</span><span class="sxs-lookup"><span data-stu-id="daad7-131">For security purposes, you need tooenter hello initial admin password that is stored in a text file on your VM toostart hello Jenkins install.</span></span> <span data-ttu-id="daad7-132">Hello hello előző lépés tooSSH tooyour VM beszerzett nyilvános IP-cím használata:</span><span class="sxs-lookup"><span data-stu-id="daad7-132">Use hello public IP address obtained in hello previous step tooSSH tooyour VM:</span></span>

```bash
ssh azureuser@<publicIps>
```

<span data-ttu-id="daad7-133">Nézet hello `initialAdminPassword` a Jenkins telepítése, és másolja azt:</span><span class="sxs-lookup"><span data-stu-id="daad7-133">View hello `initialAdminPassword` for your Jenkins install and copy it:</span></span>

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<span data-ttu-id="daad7-134">Hello fájl még nem érhető el, ha Várjon néhány percet a cloud inicializálás toocomplete hello Jenkins és a Docker telepítésről.</span><span class="sxs-lookup"><span data-stu-id="daad7-134">If hello file isn't available yet, wait a couple more minutes for cloud-init toocomplete hello Jenkins and Docker install.</span></span>

<span data-ttu-id="daad7-135">Most nyisson meg egy webböngészőt, és nyissa meg túl`http://<publicIps>:8080`.</span><span class="sxs-lookup"><span data-stu-id="daad7-135">Now open a web browser and go too`http://<publicIps>:8080`.</span></span> <span data-ttu-id="daad7-136">Hajtsa végre az alábbiak szerint hello kezdeti Jenkins beállítás:</span><span class="sxs-lookup"><span data-stu-id="daad7-136">Complete hello initial Jenkins setup as follows:</span></span>

- <span data-ttu-id="daad7-137">Adja meg a hello *initialAdminPassword* hello VM hello előző lépésben beszerzett.</span><span class="sxs-lookup"><span data-stu-id="daad7-137">Enter hello *initialAdminPassword* obtained from hello VM in hello previous step.</span></span>
- <span data-ttu-id="daad7-138">Kattintson a **beépülő modulok tooinstall kiválasztása**</span><span class="sxs-lookup"><span data-stu-id="daad7-138">Click **Select plugins tooinstall**</span></span>
- <span data-ttu-id="daad7-139">Keresse meg *GitHub* hello szövegmezőben hello tetején, válassza ki a hello *GitHub beépülő modul*, majd kattintson a **telepítése**</span><span class="sxs-lookup"><span data-stu-id="daad7-139">Search for *GitHub* in hello text box across hello top, select hello *GitHub plugin*, then click **Install**</span></span>
- <span data-ttu-id="daad7-140">toocreate Jenkins felhasználói fiók, töltse ki a kívánt hello űrlap.</span><span class="sxs-lookup"><span data-stu-id="daad7-140">toocreate a Jenkins user account, fill out hello form as desired.</span></span> <span data-ttu-id="daad7-141">Biztonsági szempontból Folytatás hello alapértelmezett rendszergazdai fiók helyett az első Jenkins felhasználó kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="daad7-141">From a security perspective, you should create this first Jenkins user rather than continuing as hello default admin account.</span></span>
- <span data-ttu-id="daad7-142">Ha befejezte, kattintson a **Jenkins használatának megkezdése**</span><span class="sxs-lookup"><span data-stu-id="daad7-142">When finished, click **Start using Jenkins**</span></span>


## <a name="create-github-webhook"></a><span data-ttu-id="daad7-143">GitHub webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-143">Create GitHub webhook</span></span>
<span data-ttu-id="daad7-144">tooconfigure hello integráció a github webhelyen, nyissa meg hello [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) hello Azure-minták tárházból.</span><span class="sxs-lookup"><span data-stu-id="daad7-144">tooconfigure hello integration with GitHub, open hello [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) from hello Azure samples repo.</span></span> <span data-ttu-id="daad7-145">toofork hello tárház tooyour saját GitHub-fiók, kattintson a hello **elágazás** hello jobb felső sarkában található gombra.</span><span class="sxs-lookup"><span data-stu-id="daad7-145">toofork hello repo tooyour own GitHub account, click hello **Fork** button in hello top right-hand corner.</span></span>

<span data-ttu-id="daad7-146">Hozzon létre egy webhook létrehozott hello elágazás belül:</span><span class="sxs-lookup"><span data-stu-id="daad7-146">Create a webhook inside hello fork you created:</span></span>

- <span data-ttu-id="daad7-147">Kattintson a **beállítások**, majd jelölje be **integrációja és a szolgáltatások** hello bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="daad7-147">Click **Settings**, then select **Integrations & services** on hello left-hand side.</span></span>
- <span data-ttu-id="daad7-148">Kattintson a **-szolgáltatás hozzáadása a**, majd adja meg *Jenkins* a Szűrő mezőbe.</span><span class="sxs-lookup"><span data-stu-id="daad7-148">Click **Add service**, then enter *Jenkins* in filter box.</span></span>
- <span data-ttu-id="daad7-149">Válassza ki *Jenkins (GitHub beépülő modul)*</span><span class="sxs-lookup"><span data-stu-id="daad7-149">Select *Jenkins (GitHub plugin)*</span></span>
- <span data-ttu-id="daad7-150">A hello **Jenkins hook URL-cím**, adja meg `http://<publicIps>:8080/github-webhook/`.</span><span class="sxs-lookup"><span data-stu-id="daad7-150">For hello **Jenkins hook URL**, enter `http://<publicIps>:8080/github-webhook/`.</span></span> <span data-ttu-id="daad7-151">Meg kell hello záró /</span><span class="sxs-lookup"><span data-stu-id="daad7-151">Make sure you include hello trailing /</span></span>
- <span data-ttu-id="daad7-152">Kattintson a **szolgáltatás hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="daad7-152">Click **Add service**</span></span>

![GitHub webhook ágazik el tooyour-tárház hozzáadása](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a><span data-ttu-id="daad7-154">Jenkins feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-154">Create Jenkins job</span></span>
<span data-ttu-id="daad7-155">toohave Jenkins válaszoljon tooan esemény a Githubon véglegesítése kód, például hozzon létre egy Jenkins feladatot.</span><span class="sxs-lookup"><span data-stu-id="daad7-155">toohave Jenkins respond tooan event in GitHub such as committing code, create a Jenkins job.</span></span> 

<span data-ttu-id="daad7-156">Kattintson a Jenkins webhely **hozzon létre új feladatokat** hello kezdőlapról:</span><span class="sxs-lookup"><span data-stu-id="daad7-156">In your Jenkins website, click **Create new jobs** from hello home page:</span></span>

- <span data-ttu-id="daad7-157">Adja meg *HelloWorld* feladat neve.</span><span class="sxs-lookup"><span data-stu-id="daad7-157">Enter *HelloWorld* as job name.</span></span> <span data-ttu-id="daad7-158">Válasszon **Freestyle projekt**, majd jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="daad7-158">Choose **Freestyle project**, then select **OK**.</span></span>
- <span data-ttu-id="daad7-159">A hello **általános** szakaszban jelölje be **GitHub** projektre, és adja meg a villás tárház URL-CÍMÉT, például *https://github.com/iainfoulds/nodejs-docs-hello-world*</span><span class="sxs-lookup"><span data-stu-id="daad7-159">Under hello **General** section, select **GitHub** project and enter your forked repo URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world*</span></span>
- <span data-ttu-id="daad7-160">A hello **forrás kód felügyeleti** szakaszban jelölje be **Git**, adja meg a villás tárház *.git* URL-címet, például *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span><span class="sxs-lookup"><span data-stu-id="daad7-160">Under hello **Source code management** section, select **Git**, enter your forked repo *.git* URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span></span>
- <span data-ttu-id="daad7-161">A hello **Build eseményindítók** szakaszban jelölje be **GitHub hook eseményindítója a következőnek: GITscm lekérdezési**.</span><span class="sxs-lookup"><span data-stu-id="daad7-161">Under hello **Build Triggers** section, select **GitHub hook trigger for GITscm polling**.</span></span>
- <span data-ttu-id="daad7-162">A hello **Build** területen válasszon **Hozzáadás összeállítása lépés**.</span><span class="sxs-lookup"><span data-stu-id="daad7-162">Under hello **Build** section, choose **Add build step**.</span></span> <span data-ttu-id="daad7-163">Válassza ki **hajtható végre a rendszerhéj**, majd adja meg `echo "Testing"` toocommand ablakban.</span><span class="sxs-lookup"><span data-stu-id="daad7-163">Select **Execute shell**, then enter `echo "Testing"` in toocommand window.</span></span>
- <span data-ttu-id="daad7-164">Kattintson a **mentése** hello feladatok ablak hello alján.</span><span class="sxs-lookup"><span data-stu-id="daad7-164">Click **Save** at hello bottom of hello jobs window.</span></span>


## <a name="test-github-integration"></a><span data-ttu-id="daad7-165">GitHub-integráció tesztelése</span><span class="sxs-lookup"><span data-stu-id="daad7-165">Test GitHub integration</span></span>
<span data-ttu-id="daad7-166">tootest hello Jenkins, GitHub integrációja az elágazáshoz változása véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="daad7-166">tootest hello GitHub integration with Jenkins, commit a change in your fork.</span></span> 

<span data-ttu-id="daad7-167">Vissza a Githubon webes felhasználói felülete, válassza ki a villás tárház, és kattintson a hello **index.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="daad7-167">Back in GitHub web UI, select your forked repo, and then click hello **index.js** file.</span></span> <span data-ttu-id="daad7-168">Kattintson hello ceruza ikonra tooedit ezt a fájlt, sor: 6 olvassa be:</span><span class="sxs-lookup"><span data-stu-id="daad7-168">Click hello pencil icon tooedit this file so line 6 reads:</span></span>

```nodejs
response.end("Hello World!");
```

<span data-ttu-id="daad7-169">toocommit módosításait, kattintson a hello **változtatások véglegesítése a határidő** hello alsó gombra.</span><span class="sxs-lookup"><span data-stu-id="daad7-169">toocommit your changes, click hello **Commit changes** button at hello bottom.</span></span>

<span data-ttu-id="daad7-170">Jenkins, az új buildverziót elindul, a hello **előzmények Build** hello bal alsó sarkában a feladat lap szakasza.</span><span class="sxs-lookup"><span data-stu-id="daad7-170">In Jenkins, a new build starts under hello **Build history** section of hello bottom left-hand corner of your job page.</span></span> <span data-ttu-id="daad7-171">Kattintson a hello build számú hivatkozásra, és válassza ki **a konzol kimeneti** a hello bal mérete.</span><span class="sxs-lookup"><span data-stu-id="daad7-171">Click hello build number link and select **Console output** on hello left-hand size.</span></span> <span data-ttu-id="daad7-172">Megtekintheti a Jenkins tesz a kódban van lekért GitHub és hello build művelet kimenetének üdvözlőüzenetére hello lépéseket `Testing` toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="daad7-172">You can view hello steps Jenkins takes as your code is pulled from GitHub and hello build action outputs hello message `Testing` toohello console.</span></span> <span data-ttu-id="daad7-173">Minden alkalommal, amikor egy véglegesítési legyen a Githubon, hello webhook egészítse ki tooJenkins és indul el, így új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="daad7-173">Each time a commit is made in GitHub, hello webhook reaches out tooJenkins and trigger a new build in this way.</span></span>


## <a name="define-docker-build-image"></a><span data-ttu-id="daad7-174">Adja meg a Docker build kép</span><span class="sxs-lookup"><span data-stu-id="daad7-174">Define Docker build image</span></span>
<span data-ttu-id="daad7-175">a Githubon véglegesítések alapján futó toosee hello Node.js alkalmazás lehetővé teszi, hogy egy Docker-lemezkép toorun hello alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="daad7-175">toosee hello Node.js app running based on your GitHub commits, lets build a Docker image toorun hello app.</span></span> <span data-ttu-id="daad7-176">hello kép össze egy Dockerfile, amely meghatározza, hogyan tooconfigure hello tároló, amely hello alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="daad7-176">hello image is built from a Dockerfile that defines how tooconfigure hello container that runs hello app.</span></span> 

<span data-ttu-id="daad7-177">Hello SSH-kapcsolat tooyour VM módosítsa az előző lépésben létrehozott hello feladat után nevű toohello Jenkins munkaterület könyvtár.</span><span class="sxs-lookup"><span data-stu-id="daad7-177">From hello SSH connection tooyour VM, change toohello Jenkins workspace directory named after hello job you created in a previous step.</span></span> <span data-ttu-id="daad7-178">A fenti példában, amely nevű *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="daad7-178">In our example, that was named *HelloWorld*.</span></span>

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

<span data-ttu-id="daad7-179">Fájl létrehozása a könyvtár munkaterület `sudo sensible-editor Dockerfile` és a Beillesztés hello követő tartalmát.</span><span class="sxs-lookup"><span data-stu-id="daad7-179">Create a file with in this workspace directory with `sudo sensible-editor Dockerfile` and paste hello following contents.</span></span> <span data-ttu-id="daad7-180">Győződjön meg arról, hogy teljes Dockerfile van hello lemásolva megfelelően, különösen akkor hello első sor:</span><span class="sxs-lookup"><span data-stu-id="daad7-180">Make sure that hello whole Dockerfile is copied correctly, especially hello first line:</span></span>

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

<span data-ttu-id="daad7-181">A Dockerfile hello alapszintű Node.js lemezkép Alpine Linux használ, tesz elérhetővé port 1337 app Hello World hello futtatja, akkor hello app fájlokat másolja és inicializálja azt.</span><span class="sxs-lookup"><span data-stu-id="daad7-181">This Dockerfile uses hello base Node.js image using Alpine Linux, exposes port 1337 that hello Hello World app runs on, then copies hello app files and initializes it.</span></span>


## <a name="create-jenkins-build-rules"></a><span data-ttu-id="daad7-182">Jenkins összeállítási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-182">Create Jenkins build rules</span></span>
<span data-ttu-id="daad7-183">Az előző lépésben létrehozott, amelyek kimenete egy üzenet toohello konzol alapvető Jenkins build szabály.</span><span class="sxs-lookup"><span data-stu-id="daad7-183">In a previous step, you created a basic Jenkins build rule that output a message toohello console.</span></span> <span data-ttu-id="daad7-184">Lehetővé teszi, hogy hozzon létre hello összeállítása lépés toouse a Dockerfile és hello alkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="daad7-184">Lets create hello build step toouse our Dockerfile and run hello app.</span></span>

<span data-ttu-id="daad7-185">A Jenkins példánya válassza az előző lépésben létrehozott hello feladat.</span><span class="sxs-lookup"><span data-stu-id="daad7-185">Back in your Jenkins instance, select hello job you created in a previous step.</span></span> <span data-ttu-id="daad7-186">Kattintson a **konfigurálása** hello bal oldalán és toohello görgetve **Build** szakasz:</span><span class="sxs-lookup"><span data-stu-id="daad7-186">Click **Configure** on hello left-hand side and scroll down toohello **Build** section:</span></span>

- <span data-ttu-id="daad7-187">Távolítsa el a meglévő `echo "Test"` összeállítása lépés.</span><span class="sxs-lookup"><span data-stu-id="daad7-187">Remove your existing `echo "Test"` build step.</span></span> <span data-ttu-id="daad7-188">Kattintson a hello közötti piros hello jobb felső sarkában hello meglévő összeállítása lépés párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="daad7-188">Click hello red cross on hello top right-hand corner of hello existing build step box.</span></span>
- <span data-ttu-id="daad7-189">Kattintson a **Hozzáadás összeállítása lépés**, majd jelölje be **rendszerhéj végrehajtása**</span><span class="sxs-lookup"><span data-stu-id="daad7-189">Click **Add build step**, then select **Execute shell**</span></span>
- <span data-ttu-id="daad7-190">A hello **parancs** mezőbe, írja be a következő Docker parancsok hello, majd válassza ki **mentése**:</span><span class="sxs-lookup"><span data-stu-id="daad7-190">In hello **Command** box, enter hello following Docker commands, then select **Save**:</span></span>

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

<span data-ttu-id="daad7-191">hello Docker build lépéseket kép és a hello Jenkins buildszám, akkor is fenntartható a képek előzményeit címke létrehozása.</span><span class="sxs-lookup"><span data-stu-id="daad7-191">hello Docker build steps create an image and tag it with hello Jenkins build number so you can maintain a history of images.</span></span> <span data-ttu-id="daad7-192">Hello alkalmazást futtató meglévő tárolókkal leáll, és eltávolítja majd.</span><span class="sxs-lookup"><span data-stu-id="daad7-192">Any existing containers running hello app are stopped and then removed.</span></span> <span data-ttu-id="daad7-193">Új tároló van, akkor hello lemezkép használatával elindult és hello legújabb véglegesíti a Githubon alapján a Node.js-alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="daad7-193">A new container is then started using hello image and runs your Node.js app based on hello latest commits in GitHub.</span></span>


## <a name="test-your-pipeline"></a><span data-ttu-id="daad7-194">A folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="daad7-194">Test your pipeline</span></span>
<span data-ttu-id="daad7-195">toosee hello egész folyamat a művelet szerkesztése hello *index.js* újra a villás GitHub-tárház fájlt, és kattintson a **módosítás véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="daad7-195">toosee hello whole pipeline in action, edit hello *index.js* file in your forked GitHub repo again and click **Commit change**.</span></span> <span data-ttu-id="daad7-196">GitHub hello webhook meghatározásával Jenkins új feladat indítja el.</span><span class="sxs-lookup"><span data-stu-id="daad7-196">A new job starts in Jenkins based on hello webhook for GitHub.</span></span> <span data-ttu-id="daad7-197">Toocreate hello Docker kép néhány másodpercet vesz igénybe, és indítsa el az alkalmazást egy új tárolóba.</span><span class="sxs-lookup"><span data-stu-id="daad7-197">It takes a few seconds toocreate hello Docker image and start your app in a new container.</span></span>

<span data-ttu-id="daad7-198">Szükség esetén újra be hello nyilvános IP-címet a virtuális gép:</span><span class="sxs-lookup"><span data-stu-id="daad7-198">If needed, obtain hello public IP address of your VM again:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="daad7-199">Nyisson meg egy webböngészőt, és írja be `http://<publicIps>:1337`.</span><span class="sxs-lookup"><span data-stu-id="daad7-199">Open a web browser and enter `http://<publicIps>:1337`.</span></span> <span data-ttu-id="daad7-200">A Node.js-alkalmazás jelenik meg, és által adott jelentéseket tükrözik hello legújabb véglegesíti a Githubon elágazás a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="daad7-200">Your Node.js app is displayed and reflects hello latest commits in your GitHub fork as follows:</span></span>

![Futó Node.js-alkalmazás](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

<span data-ttu-id="daad7-202">Ellenőrizze egy másik Szerkesztés toohello *index.js* fájlt a Githubon és a véglegesítési hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="daad7-202">Now make another edit toohello *index.js* file in GitHub and commit hello change.</span></span> <span data-ttu-id="daad7-203">Várjon néhány másodpercet, amíg a Jenkins hello feladat toocomplete, majd frissítse a webes böngésző toosee hello frissített verziója az alkalmazás fut egy új tároló az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="daad7-203">Wait a few seconds for hello job toocomplete in Jenkins, then refresh your web browser toosee hello updated version of your app running in a new container as follows:</span></span>

![Node.js-alkalmazás futtatása után egy másik GitHub véglegesítési](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a><span data-ttu-id="daad7-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="daad7-205">Next steps</span></span>
<span data-ttu-id="daad7-206">Ebben az oktatóanyagban GitHub toorun Jenkins összeállítási feladat minden kód véglegesítés konfigurált, és majd egy Docker-tároló tootest az alkalmazás üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="daad7-206">In this tutorial, you configured GitHub toorun a Jenkins build job on each code commit and then deploy a Docker container tootest your app.</span></span> <span data-ttu-id="daad7-207">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="daad7-207">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="daad7-208">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-208">Create a Jenkins VM</span></span>
> * <span data-ttu-id="daad7-209">Telepítse és konfigurálja a Jenkins</span><span class="sxs-lookup"><span data-stu-id="daad7-209">Install and configure Jenkins</span></span>
> * <span data-ttu-id="daad7-210">GitHub és Jenkins integrációjával webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-210">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="daad7-211">Hozzon létre és Jenkins feladatok létrehozása a Githubról eseményindító véglegesítése</span><span class="sxs-lookup"><span data-stu-id="daad7-211">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="daad7-212">Az alkalmazás Docker-lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="daad7-212">Create a Docker image for your app</span></span>
> * <span data-ttu-id="daad7-213">Ellenőrizze a Githubon véglegesíti és hozhat létre. új Docker-lemezkép alkalmazást futtató frissítések</span><span class="sxs-lookup"><span data-stu-id="daad7-213">Verify GitHub commits build new Docker image and updates running app</span></span>

<span data-ttu-id="daad7-214">További információt következő útmutató toolearn toohello előzetes toointegrate a Visual Studio Team Services Jenkins.</span><span class="sxs-lookup"><span data-stu-id="daad7-214">Advance toohello next tutorial toolearn more about how toointegrate Jenkins with Visual Studio Team Services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="daad7-215">Alkalmazások telepítése a Jenkins és Team Services</span><span class="sxs-lookup"><span data-stu-id="daad7-215">Deploy apps with Jenkins and Team Services</span></span>](tutorial-build-deploy-jenkins.md)