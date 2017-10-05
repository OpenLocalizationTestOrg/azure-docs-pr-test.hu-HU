---
title: "A fejlesztési folyamat létrehozása az Azure-ban Jenkins |} Microsoft Docs"
description: "Megtudhatja, hogyan Jenkins virtuális gép létrehozása, amely kéri le a Githubról. az egyes kód véglegesítési és összeállít egy új Docker-tároló az alkalmazás futtatásához Azure-ban"
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
ms.openlocfilehash: d9849b5e061dd7f2ae0744a3522dc2eb1fb37035
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-a-development-infrastructure-on-a-linux-vm-in-azure-with-jenkins-github-and-docker"></a><span data-ttu-id="bc994-103">A Linux virtuális gép az Azure-ban Jenkins, a Githubon és a Docker a fejlesztési infrastruktúra létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-103">How to create a development infrastructure on a Linux VM in Azure with Jenkins, GitHub, and Docker</span></span>
<span data-ttu-id="bc994-104">Automatizálható a build és a tesztelési fázis alkalmazásának fejlesztését, használhatja a folyamatos integrációt és a központi telepítés (CI/CD) folyamat.</span><span class="sxs-lookup"><span data-stu-id="bc994-104">To automate the build and test phase of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="bc994-105">Ebben az oktatóanyagban létrehoz egy CI/CD folyamat egy Azure virtuális gépen történő is beleértve:</span><span class="sxs-lookup"><span data-stu-id="bc994-105">In this tutorial, you create a CI/CD pipeline on an Azure VM including how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bc994-106">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-106">Create a Jenkins VM</span></span>
> * <span data-ttu-id="bc994-107">Telepítse és konfigurálja a Jenkins</span><span class="sxs-lookup"><span data-stu-id="bc994-107">Install and configure Jenkins</span></span>
> * <span data-ttu-id="bc994-108">GitHub és Jenkins integrációjával webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-108">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="bc994-109">Hozzon létre és Jenkins feladatok létrehozása a Githubról eseményindító véglegesítése</span><span class="sxs-lookup"><span data-stu-id="bc994-109">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="bc994-110">Az alkalmazás Docker-lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-110">Create a Docker image for your app</span></span>
> * <span data-ttu-id="bc994-111">Ellenőrizze a Githubon véglegesíti és hozhat létre. új Docker-lemezkép alkalmazást futtató frissítések</span><span class="sxs-lookup"><span data-stu-id="bc994-111">Verify GitHub commits build new Docker image and updates running app</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bc994-112">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="bc994-112">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="bc994-113">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="bc994-113">Run `az --version` to find the version.</span></span> <span data-ttu-id="bc994-114">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bc994-114">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-jenkins-instance"></a><span data-ttu-id="bc994-115">Jenkins példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-115">Create Jenkins instance</span></span>
<span data-ttu-id="bc994-116">Az oktatóanyag előző [első indításakor Linux virtuális gépek testreszabása](tutorial-automate-vm-deployment.md), megtudta, hogyan automatizálható a felhő inicializálás a virtuális gép testreszabása.</span><span class="sxs-lookup"><span data-stu-id="bc994-116">In a previous tutorial on [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), you learned how to automate VM customization with cloud-init.</span></span> <span data-ttu-id="bc994-117">Ez az oktatóanyag egy felhő-init fájl Jenkins és Docker telepítése a virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="bc994-117">This tutorial uses a cloud-init file to install Jenkins and Docker on a VM.</span></span> 

<span data-ttu-id="bc994-118">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* , majd illessze be a következő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="bc994-118">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="bc994-119">A felhő rendszerhéj nem a helyi számítógépen hozzon létre például a fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc994-119">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="bc994-120">Adja meg `sensible-editor cloud-init-jenkins.txt` hozza létre a fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc994-120">Enter `sensible-editor cloud-init-jenkins.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="bc994-121">Győződjön meg arról, hogy az egész felhő inicializálás fájl megfelelően lett lemásolva különösen az első sor:</span><span class="sxs-lookup"><span data-stu-id="bc994-121">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

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

<span data-ttu-id="bc994-122">A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="bc994-122">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="bc994-123">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupJenkins* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="bc994-123">The following example creates a resource group named *myResourceGroupJenkins* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupJenkins --location eastus
```

<span data-ttu-id="bc994-124">Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="bc994-124">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="bc994-125">Használja a `--custom-data` paraméter felelt meg a felhő inicializálás konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="bc994-125">Use the `--custom-data` parameter to pass in your cloud-init config file.</span></span> <span data-ttu-id="bc994-126">Adja meg a teljes elérési útja *felhő-init-jenkins.txt* Ha mentette a fájlt a jelenlegi munkakönyvtár kívül.</span><span class="sxs-lookup"><span data-stu-id="bc994-126">Provide the full path to *cloud-init-jenkins.txt* if you saved the file outside of your present working directory.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroupJenkins \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-jenkins.txt
```

<span data-ttu-id="bc994-127">A virtuális gépek létrehozása és konfigurálása a néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bc994-127">It takes a few minutes for the VM to be created and configured.</span></span>

<span data-ttu-id="bc994-128">A virtuális gép elérni kívánt webes forgalom engedélyezéséhez használja [az vm-port megnyitása](/cli/azure/vm#open-port) port megnyitásához *8080-as* Jenkins forgalom és a port *1337* számára a Node.js-alkalmazás, amely egy mintaalkalmazást futtatására szolgál:</span><span class="sxs-lookup"><span data-stu-id="bc994-128">To allow web traffic to reach your VM, use [az vm open-port](/cli/azure/vm#open-port) to open port *8080* for Jenkins traffic and port *1337* for the Node.js app that is used to run a sample app:</span></span>

```azurecli-interactive 
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 8080 --priority 1001
az vm open-port --resource-group myResourceGroupJenkins --name myVM --port 1337 --priority 1002
```


## <a name="configure-jenkins"></a><span data-ttu-id="bc994-129">Jenkins konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc994-129">Configure Jenkins</span></span>
<span data-ttu-id="bc994-130">Fér hozzá a Jenkins példányát, szerezze be a virtuális gép nyilvános IP-címe:</span><span class="sxs-lookup"><span data-stu-id="bc994-130">To access your Jenkins instance, obtain the public IP address of your VM:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="bc994-131">Biztonsági okokból meg kell adnia a kezdeti rendszergazdai jelszavát, amelyet a virtuális gép Jenkins telepítés elindításához a fájlt tárolja.</span><span class="sxs-lookup"><span data-stu-id="bc994-131">For security purposes, you need to enter the initial admin password that is stored in a text file on your VM to start the Jenkins install.</span></span> <span data-ttu-id="bc994-132">Az SSH-kapcsolatot a virtuális gép számára az előző lépésben beszerzett nyilvános IP-cím használata:</span><span class="sxs-lookup"><span data-stu-id="bc994-132">Use the public IP address obtained in the previous step to SSH to your VM:</span></span>

```bash
ssh azureuser@<publicIps>
```

<span data-ttu-id="bc994-133">Nézet a `initialAdminPassword` a Jenkins telepítése, és másolja azt:</span><span class="sxs-lookup"><span data-stu-id="bc994-133">View the `initialAdminPassword` for your Jenkins install and copy it:</span></span>

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<span data-ttu-id="bc994-134">Ha a fájl még nem érhető el, várjon néhány percet a Jenkins és a Docker felhő inicializálás telepítse.</span><span class="sxs-lookup"><span data-stu-id="bc994-134">If the file isn't available yet, wait a couple more minutes for cloud-init to complete the Jenkins and Docker install.</span></span>

<span data-ttu-id="bc994-135">Most nyisson meg egy webböngészőt, és navigáljon a `http://<publicIps>:8080`.</span><span class="sxs-lookup"><span data-stu-id="bc994-135">Now open a web browser and go to `http://<publicIps>:8080`.</span></span> <span data-ttu-id="bc994-136">Végezze el a kezdeti Jenkins a telepítő az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="bc994-136">Complete the initial Jenkins setup as follows:</span></span>

- <span data-ttu-id="bc994-137">Adja meg a *initialAdminPassword* a virtuális gép az előző lépésben beszerzett.</span><span class="sxs-lookup"><span data-stu-id="bc994-137">Enter the *initialAdminPassword* obtained from the VM in the previous step.</span></span>
- <span data-ttu-id="bc994-138">Kattintson a **jelölje be a beépülő modulok telepítése**</span><span class="sxs-lookup"><span data-stu-id="bc994-138">Click **Select plugins to install**</span></span>
- <span data-ttu-id="bc994-139">Keresse meg *GitHub* a szövegmezőben látható, válassza ki a *GitHub beépülő modul*, majd kattintson a **telepítése**</span><span class="sxs-lookup"><span data-stu-id="bc994-139">Search for *GitHub* in the text box across the top, select the *GitHub plugin*, then click **Install**</span></span>
- <span data-ttu-id="bc994-140">Jenkins felhasználói fiók létrehozása, töltse ki a kívánt módon működjenek az űrlapot.</span><span class="sxs-lookup"><span data-stu-id="bc994-140">To create a Jenkins user account, fill out the form as desired.</span></span> <span data-ttu-id="bc994-141">Biztonsági szempontból a Folytatás, az alapértelmezett rendszergazdai fiók helyett az első Jenkins felhasználó kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="bc994-141">From a security perspective, you should create this first Jenkins user rather than continuing as the default admin account.</span></span>
- <span data-ttu-id="bc994-142">Ha befejezte, kattintson a **Jenkins használatának megkezdése**</span><span class="sxs-lookup"><span data-stu-id="bc994-142">When finished, click **Start using Jenkins**</span></span>


## <a name="create-github-webhook"></a><span data-ttu-id="bc994-143">GitHub webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-143">Create GitHub webhook</span></span>
<span data-ttu-id="bc994-144">A rendszerrel történő integráció konfigurálása a GitHub, nyissa meg a [Node.js Hello World mintaalkalmazás](https://github.com/Azure-Samples/nodejs-docs-hello-world) az Azure-minták tárházból.</span><span class="sxs-lookup"><span data-stu-id="bc994-144">To configure the integration with GitHub, open the [Node.js Hello World sample app](https://github.com/Azure-Samples/nodejs-docs-hello-world) from the Azure samples repo.</span></span> <span data-ttu-id="bc994-145">A tárház a saját GitHub-fiók oszthatja ketté, kattintson a **elágazás** gombra a jobb felső sarkában.</span><span class="sxs-lookup"><span data-stu-id="bc994-145">To fork the repo to your own GitHub account, click the **Fork** button in the top right-hand corner.</span></span>

<span data-ttu-id="bc994-146">Hozzon létre egy webhook létrehozott elágazás belül:</span><span class="sxs-lookup"><span data-stu-id="bc994-146">Create a webhook inside the fork you created:</span></span>

- <span data-ttu-id="bc994-147">Kattintson a **beállítások**, majd jelölje be **integrációja és a szolgáltatások** a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="bc994-147">Click **Settings**, then select **Integrations & services** on the left-hand side.</span></span>
- <span data-ttu-id="bc994-148">Kattintson a **-szolgáltatás hozzáadása a**, majd adja meg *Jenkins* a Szűrő mezőbe.</span><span class="sxs-lookup"><span data-stu-id="bc994-148">Click **Add service**, then enter *Jenkins* in filter box.</span></span>
- <span data-ttu-id="bc994-149">Válassza ki *Jenkins (GitHub beépülő modul)*</span><span class="sxs-lookup"><span data-stu-id="bc994-149">Select *Jenkins (GitHub plugin)*</span></span>
- <span data-ttu-id="bc994-150">Az a **Jenkins hook URL-cím**, adja meg `http://<publicIps>:8080/github-webhook/`.</span><span class="sxs-lookup"><span data-stu-id="bc994-150">For the **Jenkins hook URL**, enter `http://<publicIps>:8080/github-webhook/`.</span></span> <span data-ttu-id="bc994-151">Győződjön meg arról, a záró /</span><span class="sxs-lookup"><span data-stu-id="bc994-151">Make sure you include the trailing /</span></span>
- <span data-ttu-id="bc994-152">Kattintson a **szolgáltatás hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="bc994-152">Click **Add service**</span></span>

![GitHub webhook hozzáadása a villás tárház](media/tutorial-jenkins-github-docker-cicd/github_webhook.png)


## <a name="create-jenkins-job"></a><span data-ttu-id="bc994-154">Jenkins feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-154">Create Jenkins job</span></span>
<span data-ttu-id="bc994-155">Ahhoz, hogy eseményre Jenkins válaszoljon a Githubon például kód végrehajtása, hozzon létre egy Jenkins feladatot.</span><span class="sxs-lookup"><span data-stu-id="bc994-155">To have Jenkins respond to an event in GitHub such as committing code, create a Jenkins job.</span></span> 

<span data-ttu-id="bc994-156">Kattintson a Jenkins webhely **hozzon létre új feladatokat** a kezdőlapról:</span><span class="sxs-lookup"><span data-stu-id="bc994-156">In your Jenkins website, click **Create new jobs** from the home page:</span></span>

- <span data-ttu-id="bc994-157">Adja meg *HelloWorld* feladat neve.</span><span class="sxs-lookup"><span data-stu-id="bc994-157">Enter *HelloWorld* as job name.</span></span> <span data-ttu-id="bc994-158">Válasszon **Freestyle projekt**, majd jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc994-158">Choose **Freestyle project**, then select **OK**.</span></span>
- <span data-ttu-id="bc994-159">Az a **általános** szakaszban jelölje be **GitHub** projektre, és adja meg a villás tárház URL-CÍMÉT, például *https://github.com/iainfoulds/nodejs-docs-hello-world*</span><span class="sxs-lookup"><span data-stu-id="bc994-159">Under the **General** section, select **GitHub** project and enter your forked repo URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world*</span></span>
- <span data-ttu-id="bc994-160">A a **kód felügyeleti forrás** szakaszban jelölje be **Git**, adja meg a villás tárház *.git* URL-CÍMÉT, például a *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span><span class="sxs-lookup"><span data-stu-id="bc994-160">Under the **Source code management** section, select **Git**, enter your forked repo *.git* URL, such as *https://github.com/iainfoulds/nodejs-docs-hello-world.git*</span></span>
- <span data-ttu-id="bc994-161">Az a **Build eseményindítók** szakaszban jelölje be **GitHub hook eseményindítója a következőnek: GITscm lekérdezési**.</span><span class="sxs-lookup"><span data-stu-id="bc994-161">Under the **Build Triggers** section, select **GitHub hook trigger for GITscm polling**.</span></span>
- <span data-ttu-id="bc994-162">Az a **Build** területen válassza a **Hozzáadás összeállítása lépés**.</span><span class="sxs-lookup"><span data-stu-id="bc994-162">Under the **Build** section, choose **Add build step**.</span></span> <span data-ttu-id="bc994-163">Válassza ki **hajtható végre a rendszerhéj**, majd adja meg `echo "Testing"` a a parancsablakban.</span><span class="sxs-lookup"><span data-stu-id="bc994-163">Select **Execute shell**, then enter `echo "Testing"` in to command window.</span></span>
- <span data-ttu-id="bc994-164">Kattintson a **mentése** a feladatok ablak alján.</span><span class="sxs-lookup"><span data-stu-id="bc994-164">Click **Save** at the bottom of the jobs window.</span></span>


## <a name="test-github-integration"></a><span data-ttu-id="bc994-165">GitHub-integráció tesztelése</span><span class="sxs-lookup"><span data-stu-id="bc994-165">Test GitHub integration</span></span>
<span data-ttu-id="bc994-166">Jenkins GitHub integrációja teszteléséhez véglegesítse az elágazáshoz változását.</span><span class="sxs-lookup"><span data-stu-id="bc994-166">To test the GitHub integration with Jenkins, commit a change in your fork.</span></span> 

<span data-ttu-id="bc994-167">Vissza a Githubon webes felhasználói felülete, válassza ki a villás tárház, majd kattintson a **index.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc994-167">Back in GitHub web UI, select your forked repo, and then click the **index.js** file.</span></span> <span data-ttu-id="bc994-168">Kattintson a ceruza ikonra a fájl szerkesztése, sor: 6 olvassa be:</span><span class="sxs-lookup"><span data-stu-id="bc994-168">Click the pencil icon to edit this file so line 6 reads:</span></span>

```nodejs
response.end("Hello World!");
```

<span data-ttu-id="bc994-169">A módosítások véglegesítéséhez, kattintson a **változtatások véglegesítése a határidő** panel alján.</span><span class="sxs-lookup"><span data-stu-id="bc994-169">To commit your changes, click the **Commit changes** button at the bottom.</span></span>

<span data-ttu-id="bc994-170">Jenkins, az új buildverziót elindul, a a **előzmények Build** szakasza a feladat lap bal alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="bc994-170">In Jenkins, a new build starts under the **Build history** section of the bottom left-hand corner of your job page.</span></span> <span data-ttu-id="bc994-171">A build számú hivatkozásra, és válassza ki **a konzol kimeneti** bal mérete.</span><span class="sxs-lookup"><span data-stu-id="bc994-171">Click the build number link and select **Console output** on the left-hand size.</span></span> <span data-ttu-id="bc994-172">Megtekintheti a Jenkins veszi, hogy a rendszer a kódot a Githubról hívja elő lépéseket, és a létrehozási művelet kiírja az üzenet `Testing` a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="bc994-172">You can view the steps Jenkins takes as your code is pulled from GitHub and the build action outputs the message `Testing` to the console.</span></span> <span data-ttu-id="bc994-173">Minden alkalommal, amikor egy véglegesítési a Githubon történik a webhook egészítse ki a Jenkins és indul el, így új buildverziót.</span><span class="sxs-lookup"><span data-stu-id="bc994-173">Each time a commit is made in GitHub, the webhook reaches out to Jenkins and trigger a new build in this way.</span></span>


## <a name="define-docker-build-image"></a><span data-ttu-id="bc994-174">Adja meg a Docker build kép</span><span class="sxs-lookup"><span data-stu-id="bc994-174">Define Docker build image</span></span>
<span data-ttu-id="bc994-175">Tekintse meg a GitHub véglegesítések alapján futó Node.js-alkalmazás lehetővé teszi az alkalmazás futtatásához Docker-lemezkép elkészítése.</span><span class="sxs-lookup"><span data-stu-id="bc994-175">To see the Node.js app running based on your GitHub commits, lets build a Docker image to run the app.</span></span> <span data-ttu-id="bc994-176">A kép össze egy Dockerfile, amely meghatározza a konfigurálása a tárolóhoz, amelybe futtatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc994-176">The image is built from a Dockerfile that defines how to configure the container that runs the app.</span></span> 

<span data-ttu-id="bc994-177">Az SSH-kapcsolat a virtuális géphez módosítsa az előző lépésben létrehozott feladat elnevezve Jenkins munkaterület könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="bc994-177">From the SSH connection to your VM, change to the Jenkins workspace directory named after the job you created in a previous step.</span></span> <span data-ttu-id="bc994-178">A fenti példában, amely nevű *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="bc994-178">In our example, that was named *HelloWorld*.</span></span>

```bash
cd /var/lib/jenkins/workspace/HelloWorld
```

<span data-ttu-id="bc994-179">Fájl létrehozása a könyvtár munkaterület `sudo sensible-editor Dockerfile` , majd illessze be az alábbiakat.</span><span class="sxs-lookup"><span data-stu-id="bc994-179">Create a file with in this workspace directory with `sudo sensible-editor Dockerfile` and paste the following contents.</span></span> <span data-ttu-id="bc994-180">Győződjön meg arról, hogy a teljes Dockerfile megfelelően lett lemásolva különösen az első sor:</span><span class="sxs-lookup"><span data-stu-id="bc994-180">Make sure that the whole Dockerfile is copied correctly, especially the first line:</span></span>

```yaml
FROM node:alpine

EXPOSE 1337

WORKDIR /var/www
COPY package.json /var/www/
RUN npm install
COPY index.js /var/www/
```

<span data-ttu-id="bc994-181">A Dockerfile használ, a Node.js alaplemezképet Alpine Linux használatával, tesz elérhetővé port 1337, amely a Hello World alkalmazás fut, majd másolja át az alkalmazás fájljai és inicializálja azt.</span><span class="sxs-lookup"><span data-stu-id="bc994-181">This Dockerfile uses the base Node.js image using Alpine Linux, exposes port 1337 that the Hello World app runs on, then copies the app files and initializes it.</span></span>


## <a name="create-jenkins-build-rules"></a><span data-ttu-id="bc994-182">Jenkins összeállítási szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-182">Create Jenkins build rules</span></span>
<span data-ttu-id="bc994-183">Az előző lépésben létrehozott egy alapszintű Jenkins build szabályt, amely egy üzenetet, amely a konzol kimeneti.</span><span class="sxs-lookup"><span data-stu-id="bc994-183">In a previous step, you created a basic Jenkins build rule that output a message to the console.</span></span> <span data-ttu-id="bc994-184">Lehetővé teszi, hogy létrehozása a build lépés a Dockerfile használatára, majd futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc994-184">Lets create the build step to use our Dockerfile and run the app.</span></span>

<span data-ttu-id="bc994-185">A Jenkins példánya válassza az előző lépésben létrehozott feladat.</span><span class="sxs-lookup"><span data-stu-id="bc994-185">Back in your Jenkins instance, select the job you created in a previous step.</span></span> <span data-ttu-id="bc994-186">Kattintson a **konfigurálása** a bal oldalon, és görgessen le a **Build** szakasz:</span><span class="sxs-lookup"><span data-stu-id="bc994-186">Click **Configure** on the left-hand side and scroll down to the **Build** section:</span></span>

- <span data-ttu-id="bc994-187">Távolítsa el a meglévő `echo "Test"` összeállítása lépés.</span><span class="sxs-lookup"><span data-stu-id="bc994-187">Remove your existing `echo "Test"` build step.</span></span> <span data-ttu-id="bc994-188">Kattintson a jobb felső sarkában a meglévő összeállítása lépés párbeszédpanel a piros kereszt.</span><span class="sxs-lookup"><span data-stu-id="bc994-188">Click the red cross on the top right-hand corner of the existing build step box.</span></span>
- <span data-ttu-id="bc994-189">Kattintson a **Hozzáadás összeállítása lépés**, majd jelölje be **rendszerhéj végrehajtása**</span><span class="sxs-lookup"><span data-stu-id="bc994-189">Click **Add build step**, then select **Execute shell**</span></span>
- <span data-ttu-id="bc994-190">Az a **parancs** mezőbe, írja be a következő Docker-parancsokat, majd válassza ki **mentése**:</span><span class="sxs-lookup"><span data-stu-id="bc994-190">In the **Command** box, enter the following Docker commands, then select **Save**:</span></span>

  ```bash
  docker build --tag helloworld:$BUILD_NUMBER .
  docker stop helloworld && docker rm helloworld
  docker run --name helloworld -p 1337:1337 helloworld:$BUILD_NUMBER node /var/www/index.js &
  ```

<span data-ttu-id="bc994-191">A Docker build lépéseket kép és a Jenkins a buildszám, akkor is fenntartható a képek előzményeit címke létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bc994-191">The Docker build steps create an image and tag it with the Jenkins build number so you can maintain a history of images.</span></span> <span data-ttu-id="bc994-192">Az alkalmazást futtató meglévő tárolókkal leáll, és eltávolítja majd.</span><span class="sxs-lookup"><span data-stu-id="bc994-192">Any existing containers running the app are stopped and then removed.</span></span> <span data-ttu-id="bc994-193">Új tároló majd a lemezkép használatával elindult, és a legújabb véglegesíti a Githubon alapján a Node.js-alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="bc994-193">A new container is then started using the image and runs your Node.js app based on the latest commits in GitHub.</span></span>


## <a name="test-your-pipeline"></a><span data-ttu-id="bc994-194">A folyamat tesztelése</span><span class="sxs-lookup"><span data-stu-id="bc994-194">Test your pipeline</span></span>
<span data-ttu-id="bc994-195">A művelet a teljes folyamat megtekintéséhez szerkesztése a *index.js* újra a villás GitHub-tárház fájlt, és kattintson a **módosítás véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="bc994-195">To see the whole pipeline in action, edit the *index.js* file in your forked GitHub repo again and click **Commit change**.</span></span> <span data-ttu-id="bc994-196">GitHub webhook meghatározásával Jenkins új feladat indítja el.</span><span class="sxs-lookup"><span data-stu-id="bc994-196">A new job starts in Jenkins based on the webhook for GitHub.</span></span> <span data-ttu-id="bc994-197">A Docker-lemezképet, és indítsa el az alkalmazást egy új tároló néhány másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bc994-197">It takes a few seconds to create the Docker image and start your app in a new container.</span></span>

<span data-ttu-id="bc994-198">Szükség esetén olvassa be újra a virtuális gép nyilvános IP-címe:</span><span class="sxs-lookup"><span data-stu-id="bc994-198">If needed, obtain the public IP address of your VM again:</span></span>

```azurecli-interactive 
az vm show --resource-group myResourceGroupJenkins --name myVM -d --query [publicIps] --o tsv
```

<span data-ttu-id="bc994-199">Nyisson meg egy webböngészőt, és írja be `http://<publicIps>:1337`.</span><span class="sxs-lookup"><span data-stu-id="bc994-199">Open a web browser and enter `http://<publicIps>:1337`.</span></span> <span data-ttu-id="bc994-200">A Node.js-alkalmazás jelenik meg, és a legújabb véglegesíti a Githubon elágazás a tükrözi az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="bc994-200">Your Node.js app is displayed and reflects the latest commits in your GitHub fork as follows:</span></span>

![Futó Node.js-alkalmazás](media/tutorial-jenkins-github-docker-cicd/running_nodejs_app.png)

<span data-ttu-id="bc994-202">Egy másik szerkesztése ellenőrizze a *index.js* fájlt a Githubon, és a módosítás véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="bc994-202">Now make another edit to the *index.js* file in GitHub and commit the change.</span></span> <span data-ttu-id="bc994-203">Várjon a feladat befejezése Jenkins néhány másodpercet, majd frissítse a webböngészőt a frissített verzió az alkalmazás fut egy új tároló az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="bc994-203">Wait a few seconds for the job to complete in Jenkins, then refresh your web browser to see the updated version of your app running in a new container as follows:</span></span>

![Node.js-alkalmazás futtatása után egy másik GitHub véglegesítési](media/tutorial-jenkins-github-docker-cicd/another_running_nodejs_app.png)


## <a name="next-steps"></a><span data-ttu-id="bc994-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc994-205">Next steps</span></span>
<span data-ttu-id="bc994-206">Ebben az oktatóanyagban egy Docker-tároló az alkalmazás tesztelése majd alkalmaznia kell egy Jenkins összeállítási feladat futtatása, minden kód véglegesítés a Githubon konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="bc994-206">In this tutorial, you configured GitHub to run a Jenkins build job on each code commit and then deploy a Docker container to test your app.</span></span> <span data-ttu-id="bc994-207">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="bc994-207">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="bc994-208">Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-208">Create a Jenkins VM</span></span>
> * <span data-ttu-id="bc994-209">Telepítse és konfigurálja a Jenkins</span><span class="sxs-lookup"><span data-stu-id="bc994-209">Install and configure Jenkins</span></span>
> * <span data-ttu-id="bc994-210">GitHub és Jenkins integrációjával webhook létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-210">Create webhook integration between GitHub and Jenkins</span></span>
> * <span data-ttu-id="bc994-211">Hozzon létre és Jenkins feladatok létrehozása a Githubról eseményindító véglegesítése</span><span class="sxs-lookup"><span data-stu-id="bc994-211">Create and trigger Jenkins build jobs from GitHub commits</span></span>
> * <span data-ttu-id="bc994-212">Az alkalmazás Docker-lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc994-212">Create a Docker image for your app</span></span>
> * <span data-ttu-id="bc994-213">Ellenőrizze a Githubon véglegesíti és hozhat létre. új Docker-lemezkép alkalmazást futtató frissítések</span><span class="sxs-lookup"><span data-stu-id="bc994-213">Verify GitHub commits build new Docker image and updates running app</span></span>

<span data-ttu-id="bc994-214">További információt a Visual Studio Team Services Jenkins integrálása a következő oktatóanyag továbblépés.</span><span class="sxs-lookup"><span data-stu-id="bc994-214">Advance to the next tutorial to learn more about how to integrate Jenkins with Visual Studio Team Services.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="bc994-215">Alkalmazások telepítése a Jenkins és Team Services</span><span class="sxs-lookup"><span data-stu-id="bc994-215">Deploy apps with Jenkins and Team Services</span></span>](tutorial-build-deploy-jenkins.md)