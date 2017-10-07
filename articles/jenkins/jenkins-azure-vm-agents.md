---
title: "Azure virtuális gép ügynökök aaaUse Jenkins folyamatos integrációját."
description: "Azure virtuálisgép-ügynökök használata Jenkins alárendelt csomópontokként."
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 2388e6919d0280372166fbd325d80dafb00d7550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a><span data-ttu-id="b5f2c-103">Azure virtuálisgép-ügynökök használata a Jenkins szolgáltatással fenntartott folyamatos integrációhoz.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-103">Use Azure VM agents for continuous integration with Jenkins.</span></span>

<span data-ttu-id="b5f2c-104">A gyors üzembe helyezés bemutatja, hogyan toouse hello Jenkins Azure VM ügynökök beépülő modul toocreate az igény szerinti Linux (Ubuntu) ügynök az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-104">This quickstart shows how toouse hello Jenkins Azure VM Agents plugin toocreate an on-demand Linux (Ubuntu) agent in Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5f2c-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b5f2c-105">Prerequisites</span></span>

<span data-ttu-id="b5f2c-106">toocomplete a gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="b5f2c-106">toocomplete this quickstart:</span></span>

* <span data-ttu-id="b5f2c-107">Ha még nem rendelkezik egy Jenkins master, Kezdésként használhatja az hello [megoldás sablon](install-jenkins-solution-template.md)</span><span class="sxs-lookup"><span data-stu-id="b5f2c-107">If you do not already have a Jenkins master, you can start with hello [Solution Template](install-jenkins-solution-template.md)</span></span> 
* <span data-ttu-id="b5f2c-108">Tekintse meg a túl[hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) Ha még nem rendelkezik egy egyszerű Azure szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-108">Refer too[Create an Azure Service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) if you do not already have an Azure service principal.</span></span>

## <a name="install-azure-vm-agents-plugin"></a><span data-ttu-id="b5f2c-109">Azure-beli virtuálisgép-ügynökhöz tartozó beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="b5f2c-109">Install Azure VM Agents plugin</span></span>

<span data-ttu-id="b5f2c-110">Ha később elindítják a hello [Megoldássablonban](install-jenkins-solution-template.md), hello Azure Virtuálisgép-ügynök beépülő modul telepítve van-e hello Jenkins fő.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-110">If you start from hello [Solution Template](install-jenkins-solution-template.md), hello Azure VM Agent plugin is installed in hello Jenkins master.</span></span>

<span data-ttu-id="b5f2c-111">Ellenkező esetben a hello telepítése **Azure VM ügynökök** hello Jenkins irányítópult belül a beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-111">Otherwise, install hello **Azure VM Agents** plugin from within hello Jenkins dashboard.</span></span>

## <a name="configure-hello-plugin"></a><span data-ttu-id="b5f2c-112">Hello beépülő modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5f2c-112">Configure hello plugin</span></span>

* <span data-ttu-id="b5f2c-113">Belül hello Jenkins irányítópultján kattintson **kezelése Jenkins -> rendszer beállítása ->**.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-113">Within hello Jenkins dashboard, click **Manage Jenkins -> Configure System ->**.</span></span> <span data-ttu-id="b5f2c-114">Toohello a hello lap alján görgessen és hello legördülő hello szakasz található **adja hozzá az új felhőalapú**.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-114">Scroll toohello bottom of hello page and find hello section with hello dropdown **Add new cloud**.</span></span> <span data-ttu-id="b5f2c-115">Hello menüben válassza ki a **Microsoft Azure virtuális gép ügynökök**</span><span class="sxs-lookup"><span data-stu-id="b5f2c-115">From hello menu, select **Microsoft Azure VM Agents**</span></span>
* <span data-ttu-id="b5f2c-116">Kiválaszthat egy meglévő fiókot hello Azure hitelesítő adatok legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-116">Select an existing account from hello Azure Credentials dropdown.</span></span>  <span data-ttu-id="b5f2c-117">új tooadd **Microsoft Azure szolgáltatás egyszerű** adja meg a következő értékek hello: előfizetés-azonosító, az ügyfél-azonosító, a titkos Ügyfélkulcs és a OAuth 2.0 Token-végpont.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-117">tooadd a new **Microsoft Azure Service Principal,** enter hello following values: Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span>

![Azure-beli Hitelesítő adatok](./media/jenkins-azure-vm-agents/service-principal.png)

* <span data-ttu-id="b5f2c-119">Kattintson a **ellenőrizze konfigurációs** toomake meg arról, hogy hello-profil konfigurációjának helyességéről.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-119">Click **Verify configuration** toomake sure that hello profile configuration is correct.</span></span>
* <span data-ttu-id="b5f2c-120">Hello konfiguráció mentéséhez, és folytassa a következő lépés toohello.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-120">Save hello configuration, and continue toohello next step.</span></span>

## <a name="template-configuration"></a><span data-ttu-id="b5f2c-121">Sablon konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5f2c-121">Template configuration</span></span>

### <a name="general-configuration"></a><span data-ttu-id="b5f2c-122">Általános konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b5f2c-122">General configuration</span></span>
<span data-ttu-id="b5f2c-123">Ezután konfigurálja a sablont használja toodefine egy Azure Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-123">Next, configure a template for use toodefine an Azure VM agent.</span></span> 

* <span data-ttu-id="b5f2c-124">Kattintson a **Hozzáadás** tooadd egy sablont.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-124">Click **Add** tooadd a template.</span></span> 
* <span data-ttu-id="b5f2c-125">Adja meg az új sablon nevét.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-125">Provide a name for your new template.</span></span> 
* <span data-ttu-id="b5f2c-126">Hello címkére írja be a "ubuntu."</span><span class="sxs-lookup"><span data-stu-id="b5f2c-126">For hello label, enter  "ubuntu."</span></span> <span data-ttu-id="b5f2c-127">Ez a címke hello feladat konfigurálása során használatos.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-127">This label is used during hello job configuration.</span></span>
* <span data-ttu-id="b5f2c-128">Válassza ki a kívánt régiót hello a hello kombinált lista.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-128">Select hello desired region from hello combo box.</span></span>
* <span data-ttu-id="b5f2c-129">Jelölje be hello szükséges Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-129">Select hello desired VM size.</span></span>
* <span data-ttu-id="b5f2c-130">Adja meg hello Azure Storage-fiók nevét, vagy hagyja üresen toouse hello alapértelmezett neve "jenkinsarmst."</span><span class="sxs-lookup"><span data-stu-id="b5f2c-130">Specify hello Azure Storage account name or leave it blank toouse hello default name "jenkinsarmst."</span></span>
* <span data-ttu-id="b5f2c-131">Adja meg a hello megőrzési időtartamot percben.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-131">Specify hello retention time in minutes.</span></span> <span data-ttu-id="b5f2c-132">Ez a beállítás határozza meg a hello hány perc Jenkins várja meg, amíg az üresjárati ügynök automatikusan törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-132">This setting defines hello number of minutes Jenkins can wait before automatically deleting an idle agent.</span></span> <span data-ttu-id="b5f2c-133">Adjon meg 0, ha nem szeretné, hogy üresjárati ügynökök toobe automatikusan törli.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-133">Specify 0 if you do not want idle agents toobe deleted automatically.</span></span>

![Általános konfiguráció](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a><span data-ttu-id="b5f2c-135">Rendszerkép-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b5f2c-135">Image configuration</span></span>

<span data-ttu-id="b5f2c-136">toocreate Linux (Ubuntu) ügynök, válassza ki **rendszerképet a referencia** és hello használja a következő konfigurációs példaként.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-136">toocreate a Linux (Ubuntu) agent, select **Image reference** and use hello following configuration as an example.</span></span> <span data-ttu-id="b5f2c-137">Tekintse meg a túl[Azure piactér](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) hello legújabb Azure támogatott képek.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-137">Refer too[Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) for hello latest Azure supported images.</span></span>

* <span data-ttu-id="b5f2c-138">Lemezkép kiadója: Canonical</span><span class="sxs-lookup"><span data-stu-id="b5f2c-138">Image Publisher: Canonical</span></span>
* <span data-ttu-id="b5f2c-139">Lemezkép tartalma: UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="b5f2c-139">Image Offer: UbuntuServer</span></span>
* <span data-ttu-id="b5f2c-140">Lemezkép termékváltozat: 14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="b5f2c-140">Image Sku: 14.04.5-LTS</span></span>
* <span data-ttu-id="b5f2c-141">Lemezkép verzió: legfrissebb</span><span class="sxs-lookup"><span data-stu-id="b5f2c-141">Image version: latest</span></span>
* <span data-ttu-id="b5f2c-142">Operációs rendszer típusa: Linux</span><span class="sxs-lookup"><span data-stu-id="b5f2c-142">OS Type: Linux</span></span>
* <span data-ttu-id="b5f2c-143">Indítási metódus: SSH</span><span class="sxs-lookup"><span data-stu-id="b5f2c-143">Launch method: SSH</span></span>
* <span data-ttu-id="b5f2c-144">Rendszergazdai hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="b5f2c-144">Provide an admin credentials</span></span>
* <span data-ttu-id="b5f2c-145">A virtuálisgép-inicializáló parancsprogram indításához írja be:</span><span class="sxs-lookup"><span data-stu-id="b5f2c-145">For VM initialization script, enter:</span></span>
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Rendszerkép-konfiguráció](./media/jenkins-azure-vm-agents/image-config.png)

* <span data-ttu-id="b5f2c-147">Kattintson a **sablon ellenőrzése** tooverify hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-147">Click **Verify Template** tooverify hello configuration.</span></span>
* <span data-ttu-id="b5f2c-148">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-148">Click **Save**.</span></span>

## <a name="create-a-job-in-jenkins"></a><span data-ttu-id="b5f2c-149">Feladat létrehozása a Jenkinsben</span><span class="sxs-lookup"><span data-stu-id="b5f2c-149">Create a job in Jenkins</span></span>

* <span data-ttu-id="b5f2c-150">Belül hello Jenkins irányítópultján kattintson **új elem**.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-150">Within hello Jenkins dashboard, click **New Item**.</span></span> 
* <span data-ttu-id="b5f2c-151">Adjon meg egy nevet, válassza ki a **Freestyle projekt** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-151">Enter a name and select **Freestyle project** and click **OK**.</span></span>
* <span data-ttu-id="b5f2c-152">A hello **általános** lapon, jelölje be "Korlátozása, amelyben futtathatók a projekt" és "ubuntu" címke kifejezés típusa.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-152">In hello **General** tab, select "Restrict where project can be run" and type "ubuntu" in Label Expression.</span></span> <span data-ttu-id="b5f2c-153">Ekkor megjelenik a "ubuntu" hello legördülő.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-153">You now see "ubuntu" in hello dropdown.</span></span>
* <span data-ttu-id="b5f2c-154">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-154">Click **Save**.</span></span>

![Feladat beállítása](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a><span data-ttu-id="b5f2c-156">Új projekt kiépítése</span><span class="sxs-lookup"><span data-stu-id="b5f2c-156">Build your new project</span></span>

* <span data-ttu-id="b5f2c-157">Lépjen vissza a toohello Jenkins irányítópult.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-157">Go back toohello Jenkins dashboard.</span></span>
* <span data-ttu-id="b5f2c-158">Kattintson a jobb gombbal hello új feladatot létrehozni, majd kattintson a **Létrehozás most**.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-158">Right-click hello new job you created, then click **Build now**.</span></span> <span data-ttu-id="b5f2c-159">Megkezdődik a fordítási folyamat.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-159">A build is kicked off.</span></span> 
* <span data-ttu-id="b5f2c-160">Hello build befejeződése után nyissa meg túl**a konzol kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-160">Once hello build is complete, go too**Console output**.</span></span> <span data-ttu-id="b5f2c-161">Láthatja, hogy hello build távolról lett végrehajtva Azure-on.</span><span class="sxs-lookup"><span data-stu-id="b5f2c-161">You see that hello build was performed remotely on Azure.</span></span>

![Konzolkimenet](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a><span data-ttu-id="b5f2c-163">Referencia</span><span class="sxs-lookup"><span data-stu-id="b5f2c-163">Reference</span></span>

* <span data-ttu-id="b5f2c-164">Azure Friday videó: [Folyamatos integráció a Jenkins szolgáltatással Azure-beli virtuálisgép-ügynökök használatával](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span><span class="sxs-lookup"><span data-stu-id="b5f2c-164">Azure Friday video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span></span>
* <span data-ttu-id="b5f2c-165">Támogatási információ és konfigurációs lehetőségek: [Azure-beli virtuálisgép-ügynök Jenkins beépülő modul Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span><span class="sxs-lookup"><span data-stu-id="b5f2c-165">Support information and configuration options:  [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span></span> 

