---
title: "Azure virtuálisgép-ügynökök használata a Jenkins szolgáltatással fenntartott folyamatos integrációhoz."
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
ms.openlocfilehash: 0b22a559fbc03158a6d4398603d1a7d2874d7b67
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a><span data-ttu-id="f824a-103">Azure virtuálisgép-ügynökök használata a Jenkins szolgáltatással fenntartott folyamatos integrációhoz.</span><span class="sxs-lookup"><span data-stu-id="f824a-103">Use Azure VM agents for continuous integration with Jenkins.</span></span>

<span data-ttu-id="f824a-104">Ez a gyors üzembe helyezési útmutató a Jenkins Azure virtuálisgép-ügynökhöz tartozó beépülő modul használatát ismerteti igény szerinti Linux (Ubuntu) ügynök létrehozásához az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f824a-104">This quickstart shows how to use the Jenkins Azure VM Agents plugin to create an on-demand Linux (Ubuntu) agent in Azure.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f824a-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f824a-105">Prerequisites</span></span>

<span data-ttu-id="f824a-106">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="f824a-106">To complete this quickstart:</span></span>

* <span data-ttu-id="f824a-107">Ha még nem rendelkezik Jenkins-főkiszolgálóval, kezdheti a [Megoldássablonnal](install-jenkins-solution-template.md)</span><span class="sxs-lookup"><span data-stu-id="f824a-107">If you do not already have a Jenkins master, you can start with the [Solution Template](install-jenkins-solution-template.md)</span></span> 
* <span data-ttu-id="f824a-108">Járjon el az [Azure-beli szolgáltatásnév létrehozása az Azure CLI 2.0-val](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) című cikk alapján, ha még nem rendelkezik Azure-beli szolgáltatásnévvel.</span><span class="sxs-lookup"><span data-stu-id="f824a-108">Refer to [Create an Azure Service principal with Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json) if you do not already have an Azure service principal.</span></span>

## <a name="install-azure-vm-agents-plugin"></a><span data-ttu-id="f824a-109">Azure-beli virtuálisgép-ügynökhöz tartozó beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="f824a-109">Install Azure VM Agents plugin</span></span>

<span data-ttu-id="f824a-110">Ha a [Megoldássablonból](install-jenkins-solution-template.md) indul ki, akkor az Azure-beli virtuálisgép-ügynökhöz tartozó beépülő modul a Jenkins-főkiszolgálón lesz telepítve.</span><span class="sxs-lookup"><span data-stu-id="f824a-110">If you start from the [Solution Template](install-jenkins-solution-template.md), the Azure VM Agent plugin is installed in the Jenkins master.</span></span>

<span data-ttu-id="f824a-111">Egyéb esetben telepítse az **Azure-beli virtuálisgép-ügynökhöz** tartozó beépülő modult a Jenkins irányítópultról.</span><span class="sxs-lookup"><span data-stu-id="f824a-111">Otherwise, install the **Azure VM Agents** plugin from within the Jenkins dashboard.</span></span>

## <a name="configure-the-plugin"></a><span data-ttu-id="f824a-112">A beépülő modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f824a-112">Configure the plugin</span></span>

* <span data-ttu-id="f824a-113">A Jenkins irányítópulton kattintson a **Jenkins kezelése-> Rendszer konfigurálása->** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f824a-113">Within the Jenkins dashboard, click **Manage Jenkins -> Configure System ->**.</span></span> <span data-ttu-id="f824a-114">Görgessen a lap aljáig és keresse meg az **Új felhő hozzáadása** legördülő menüt.</span><span class="sxs-lookup"><span data-stu-id="f824a-114">Scroll to the bottom of the page and find the section with the dropdown **Add new cloud**.</span></span> <span data-ttu-id="f824a-115">A menüből válassza a **Microsoft Azure-beli virtuálisgép-ügynökök** lehetőséget</span><span class="sxs-lookup"><span data-stu-id="f824a-115">From the menu, select **Microsoft Azure VM Agents**</span></span>
* <span data-ttu-id="f824a-116">Válasszon egy létező fiókot az Azure-beli hitelesítő adatok legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="f824a-116">Select an existing account from the Azure Credentials dropdown.</span></span>  <span data-ttu-id="f824a-117">Új **Microsoft Azure--beli egyszerű szolgáltatásnév** hozzáadásához adja meg a következő értékeket: előfizetés-azonosító, ügyfél-azonosító, titkos ügyfélkód és a OAuth 2.0 jogkivonat-végpont.</span><span class="sxs-lookup"><span data-stu-id="f824a-117">To add a new **Microsoft Azure Service Principal,** enter the following values: Subscription ID, Client ID, Client Secret, and OAuth 2.0 Token Endpoint.</span></span>

![Azure-beli Hitelesítő adatok](./media/jenkins-azure-vm-agents/service-principal.png)

* <span data-ttu-id="f824a-119">A **Konfiguráció ellenőrzése** lehetőségre kattintva győződjön meg a profil konfigurációjának helyességéről.</span><span class="sxs-lookup"><span data-stu-id="f824a-119">Click **Verify configuration** to make sure that the profile configuration is correct.</span></span>
* <span data-ttu-id="f824a-120">Mentse a konfigurációt és haladjon tovább a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="f824a-120">Save the configuration, and continue to the next step.</span></span>

## <a name="template-configuration"></a><span data-ttu-id="f824a-121">Sablon konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f824a-121">Template configuration</span></span>

### <a name="general-configuration"></a><span data-ttu-id="f824a-122">Általános konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f824a-122">General configuration</span></span>
<span data-ttu-id="f824a-123">Konfiguráljon egy sablont az Azure-beli virtuálisgép-ügynök definiálásához.</span><span class="sxs-lookup"><span data-stu-id="f824a-123">Next, configure a template for use to define an Azure VM agent.</span></span> 

* <span data-ttu-id="f824a-124">Adjon meg új sablont a **Hozzáadás** gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="f824a-124">Click **Add** to add a template.</span></span> 
* <span data-ttu-id="f824a-125">Adja meg az új sablon nevét.</span><span class="sxs-lookup"><span data-stu-id="f824a-125">Provide a name for your new template.</span></span> 
* <span data-ttu-id="f824a-126">Címkeként írja be: "ubuntu".</span><span class="sxs-lookup"><span data-stu-id="f824a-126">For the label, enter  "ubuntu."</span></span> <span data-ttu-id="f824a-127">Ez a címke a feladat konfigurálása során használatos.</span><span class="sxs-lookup"><span data-stu-id="f824a-127">This label is used during the job configuration.</span></span>
* <span data-ttu-id="f824a-128">A kombinált listából válassza ki a kívánt régiót.</span><span class="sxs-lookup"><span data-stu-id="f824a-128">Select the desired region from the combo box.</span></span>
* <span data-ttu-id="f824a-129">Válassza ki a virtuális gép kívánt méretét.</span><span class="sxs-lookup"><span data-stu-id="f824a-129">Select the desired VM size.</span></span>
* <span data-ttu-id="f824a-130">Adja meg az Azure-tárfiók nevét, vagy hagyja üresen a mezőt az alapértelmezett "jenkinsarmst" név használatához.</span><span class="sxs-lookup"><span data-stu-id="f824a-130">Specify the Azure Storage account name or leave it blank to use the default name "jenkinsarmst."</span></span>
* <span data-ttu-id="f824a-131">Adja meg a megőrzési időtartamot percekben mérve.</span><span class="sxs-lookup"><span data-stu-id="f824a-131">Specify the retention time in minutes.</span></span> <span data-ttu-id="f824a-132">Ez a beállítás határozza meg, hogy hány percet várhat a Jenkins egy tevékenység nélküli ügynök automatikus törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="f824a-132">This setting defines the number of minutes Jenkins can wait before automatically deleting an idle agent.</span></span> <span data-ttu-id="f824a-133">Adjon meg 0 értéket, ha nem kívánja automatikusan töröltetni a tevékenység nélküli ügynököket.</span><span class="sxs-lookup"><span data-stu-id="f824a-133">Specify 0 if you do not want idle agents to be deleted automatically.</span></span>

![Általános konfiguráció](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a><span data-ttu-id="f824a-135">Rendszerkép-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f824a-135">Image configuration</span></span>

<span data-ttu-id="f824a-136">Linux (Ubuntu) ügynök létrehozásához válassza a **Képhivatkozás** lehetőséget, majd használja példaként az alábbi konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="f824a-136">To create a Linux (Ubuntu) agent, select **Image reference** and use the following configuration as an example.</span></span> <span data-ttu-id="f824a-137">Az Azure által támogatott lemezképek legfrissebb listáját az [Azure Marketplace-en](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) találja meg.</span><span class="sxs-lookup"><span data-stu-id="f824a-137">Refer to [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) for the latest Azure supported images.</span></span>

* <span data-ttu-id="f824a-138">Lemezkép kiadója: Canonical</span><span class="sxs-lookup"><span data-stu-id="f824a-138">Image Publisher: Canonical</span></span>
* <span data-ttu-id="f824a-139">Lemezkép tartalma: UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="f824a-139">Image Offer: UbuntuServer</span></span>
* <span data-ttu-id="f824a-140">Lemezkép termékváltozat: 14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="f824a-140">Image Sku: 14.04.5-LTS</span></span>
* <span data-ttu-id="f824a-141">Lemezkép verzió: legfrissebb</span><span class="sxs-lookup"><span data-stu-id="f824a-141">Image version: latest</span></span>
* <span data-ttu-id="f824a-142">Operációs rendszer típusa: Linux</span><span class="sxs-lookup"><span data-stu-id="f824a-142">OS Type: Linux</span></span>
* <span data-ttu-id="f824a-143">Indítási metódus: SSH</span><span class="sxs-lookup"><span data-stu-id="f824a-143">Launch method: SSH</span></span>
* <span data-ttu-id="f824a-144">Rendszergazdai hitelesítő adatok megadása</span><span class="sxs-lookup"><span data-stu-id="f824a-144">Provide an admin credentials</span></span>
* <span data-ttu-id="f824a-145">A virtuálisgép-inicializáló parancsprogram indításához írja be:</span><span class="sxs-lookup"><span data-stu-id="f824a-145">For VM initialization script, enter:</span></span>
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Rendszerkép-konfiguráció](./media/jenkins-azure-vm-agents/image-config.png)

* <span data-ttu-id="f824a-147">A konfiguráció ellenőrzéséhez kattintson a **Sablon ellenőrzése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f824a-147">Click **Verify Template** to verify the configuration.</span></span>
* <span data-ttu-id="f824a-148">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f824a-148">Click **Save**.</span></span>

## <a name="create-a-job-in-jenkins"></a><span data-ttu-id="f824a-149">Feladat létrehozása a Jenkinsben</span><span class="sxs-lookup"><span data-stu-id="f824a-149">Create a job in Jenkins</span></span>

* <span data-ttu-id="f824a-150">A Jenkins irányítópulton kattintson az **Új elem** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f824a-150">Within the Jenkins dashboard, click **New Item**.</span></span> 
* <span data-ttu-id="f824a-151">Adjon meg egy nevet, válassza ki a **Freestyle projekt** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f824a-151">Enter a name and select **Freestyle project** and click **OK**.</span></span>
* <span data-ttu-id="f824a-152">Az **Általános** fülön válassza a "Projekt futási helyének korlátozása" lehetőséget, és a címke kifejezéshez írja be: "ubuntu".</span><span class="sxs-lookup"><span data-stu-id="f824a-152">In the **General** tab, select "Restrict where project can be run" and type "ubuntu" in Label Expression.</span></span> <span data-ttu-id="f824a-153">Az "ubuntu" ez után megjelenik a legördülő listában.</span><span class="sxs-lookup"><span data-stu-id="f824a-153">You now see "ubuntu" in the dropdown.</span></span>
* <span data-ttu-id="f824a-154">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f824a-154">Click **Save**.</span></span>

![Feladat beállítása](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a><span data-ttu-id="f824a-156">Új projekt kiépítése</span><span class="sxs-lookup"><span data-stu-id="f824a-156">Build your new project</span></span>

* <span data-ttu-id="f824a-157">Térjen vissza a Jenkins irányítópultra.</span><span class="sxs-lookup"><span data-stu-id="f824a-157">Go back to the Jenkins dashboard.</span></span>
* <span data-ttu-id="f824a-158">Kattintson jobb gombbal a létrehozott feladatra, majd kattintson a **kiépítés most** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="f824a-158">Right-click the new job you created, then click **Build now**.</span></span> <span data-ttu-id="f824a-159">Megkezdődik a fordítási folyamat.</span><span class="sxs-lookup"><span data-stu-id="f824a-159">A build is kicked off.</span></span> 
* <span data-ttu-id="f824a-160">A fordítás befejeződése után nyissa meg a **Konzolkimenetet**.</span><span class="sxs-lookup"><span data-stu-id="f824a-160">Once the build is complete, go to **Console output**.</span></span> <span data-ttu-id="f824a-161">Látható, hogy a fordítás távolról hajtódott végre az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="f824a-161">You see that the build was performed remotely on Azure.</span></span>

![Konzolkimenet](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a><span data-ttu-id="f824a-163">Referencia</span><span class="sxs-lookup"><span data-stu-id="f824a-163">Reference</span></span>

* <span data-ttu-id="f824a-164">Azure Friday videó: [Folyamatos integráció a Jenkins szolgáltatással Azure-beli virtuálisgép-ügynökök használatával](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span><span class="sxs-lookup"><span data-stu-id="f824a-164">Azure Friday video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents)</span></span>
* <span data-ttu-id="f824a-165">Támogatási információ és konfigurációs lehetőségek: [Azure-beli virtuálisgép-ügynök Jenkins beépülő modul Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span><span class="sxs-lookup"><span data-stu-id="f824a-165">Support information and configuration options:  [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin)</span></span> 

