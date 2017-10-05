---
title: "Jenkins-kiszolgáló létrehozása az Azure-on"
description: "Jenkins-kiszolgáló telepítése egy Azure-beli linuxos virtuális gépen a Jenkins-megoldássablonból és egy Java-mintaalkalmazás létrehozása."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 7bb74f297d52fb25171817175cce64187b397c38
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-the-azure-portal"></a><span data-ttu-id="0700e-103">Jenkins-kiszolgáló létrehozása Azure-beli linuxos virtuális gépen az Azure Portalról</span><span class="sxs-lookup"><span data-stu-id="0700e-103">Create a Jenkins server on an Azure Linux VM from the Azure portal</span></span>

<span data-ttu-id="0700e-104">Ez a rövid útmutató bemutatja, hogyan telepítheti a [Jenkins](https://jenkins.io)-kiszolgálót Ubuntu Linux rendszerű virtuális gépre az Azure-ral való együttműködésre konfigurált eszközökkel és beépülő modulokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="0700e-104">This quickstart shows how to install [Jenkins](https://jenkins.io) on an Ubuntu Linux VM with the tools and plug-ins configured to work with Azure.</span></span> <span data-ttu-id="0700e-105">Ha elkészült, rendelkezni fog egy Azure-ban futó Jenkins-kiszolgálóval, amely egy Java-mintaalkalmazást hoz létre a [GitHubról](https://github.com).</span><span class="sxs-lookup"><span data-stu-id="0700e-105">When you're finished, you have a Jenkins server running in Azure building a sample Java app from [GitHub](https://github.com).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0700e-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0700e-106">Prerequisites</span></span>

* <span data-ttu-id="0700e-107">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="0700e-107">An Azure subscription</span></span>
* <span data-ttu-id="0700e-108">SSH-hozzáférés a számítógép parancssorából (például Bash felület vagy [PuTTY](http://www.putty.org/))</span><span class="sxs-lookup"><span data-stu-id="0700e-108">Access to SSH on your computer's command line (such as the Bash shell or [PuTTY](http://www.putty.org/))</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-jenkins-vm-from-the-solution-template"></a><span data-ttu-id="0700e-109">Jenkins virtuális gép létrehozása a megoldássablonból</span><span class="sxs-lookup"><span data-stu-id="0700e-109">Create the Jenkins VM from the solution template</span></span>

<span data-ttu-id="0700e-110">Nyissa meg a [Jenkins áruházbeli rendszerképét](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) a webböngészőben, és az oldal bal oldalán válassza a **LETÖLTÉS MOST** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0700e-110">Open the [marketplace image for Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) in your web browser and select  **GET IT NOW** from the left-hand side of the page.</span></span> <span data-ttu-id="0700e-111">Tekintse át a díjszabás részleteit, és válassza a **Folytatás**, majd a **Létrehozás** lehetőséget a Jenkins-kiszolgáló Azure Portalon történő konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="0700e-111">Review the pricing details and select **Continue**, then select **Create** to configure the Jenkins server in the Azure portal.</span></span> 
   
![Azure Portal párbeszédpanel](./media/install-jenkins-solution-template/ap-create.png)

<span data-ttu-id="0700e-113">**Az alapszintű beállítások konfigurálása** lapon töltse ki a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="0700e-113">In the **Configure basic settings** tab, fill in the following fields:</span></span>

![Az alapvető beállítások konfigurálása](./media/install-jenkins-solution-template/ap-basic.png)

* <span data-ttu-id="0700e-115">Írja be a **Jenkins** értéket a **Név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0700e-115">Use **Jenkins** for **Name**.</span></span>
* <span data-ttu-id="0700e-116">Írjon be egy **felhasználónevet**.</span><span class="sxs-lookup"><span data-stu-id="0700e-116">Enter a **User name**.</span></span> <span data-ttu-id="0700e-117">A felhasználónévnek [egyedi követelményeknek](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) kell megfelelnie.</span><span class="sxs-lookup"><span data-stu-id="0700e-117">The user name must meet [specific requirements](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span></span>
* <span data-ttu-id="0700e-118">A **Hitelesítés típusa** mezőben válassza a **Jelszó** lehetőséget, majd adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="0700e-118">Select **Password** as the **Authentication type** and enter a password.</span></span> <span data-ttu-id="0700e-119">A jelszónak legalább egy nagybetűs karaktert, egy számot és egy speciális karaktert is tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="0700e-119">The password must have an upper case character, a number, and one special character.</span></span>
* <span data-ttu-id="0700e-120">Az **Erőforráscsoport** mezőben adja meg a **myJenkinsResourceGroup** kifejezést.</span><span class="sxs-lookup"><span data-stu-id="0700e-120">Use **myJenkinsResourceGroup** for the **Resource Group**.</span></span>
* <span data-ttu-id="0700e-121">A **Hely** legördülő menüből válassza az **USA keleti régiója** [Azure-régiót](https://azure.microsoft.com/regions/).</span><span class="sxs-lookup"><span data-stu-id="0700e-121">Choose the **East US** [Azure region](https://azure.microsoft.com/regions/) from the **Location** drop-down.</span></span>

<span data-ttu-id="0700e-122">Az **OK** lehetőség kiválasztásával lépjen tovább a **További beállítások konfigurálása** lapra. Adjon meg egy egyedi tartománynevet a Jenkins-kiszolgáló azonosításához, majd válassza az **OK** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0700e-122">Select **OK** to proceed to the **Configure additional options** tab. Enter a unique domain name to identify the Jenkins server and select **OK**.</span></span>

![További beállítások megadása](./media/install-jenkins-solution-template/ap-addtional.png)  

 <span data-ttu-id="0700e-124">Az ellenőrzés után válassza ismét az **OK** lehetőséget az **Összefoglalás** lapon. Végül válassza a **Vásárlás** lehetőséget a Jenkins virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0700e-124">Once validation passes, select **OK** again from the **Summary** tab. Finally, select **Purchase** to create the Jenkins VM.</span></span> <span data-ttu-id="0700e-125">Ha a kiszolgáló készen áll, értesítést fog kapni az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="0700e-125">When your server is ready, you get a notification in the Azure portal:</span></span>   

![„A Jenkins készen áll” értesítés](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-to-jenkins"></a><span data-ttu-id="0700e-127">Kapcsolódás a Jenkinshez</span><span class="sxs-lookup"><span data-stu-id="0700e-127">Connect to Jenkins</span></span>

<span data-ttu-id="0700e-128">A webböngészőjében nyissa meg a virtuális gépet (például: http://jenkins2517454.eastus.cloudapp.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0700e-128">Navigate to your virtual machine (for example, http://jenkins2517454.eastus.cloudapp.azure.com/) in  your web browser.</span></span> <span data-ttu-id="0700e-129">A Jenkins-konzol nem érhető el nem biztonságos HTTP-n keresztül. Az oldalon szereplő utasítások szerint, SSH-alagút használatával biztonságosan nyithatja meg a Jenkins-konzolt.</span><span class="sxs-lookup"><span data-stu-id="0700e-129">The Jenkins console is inaccessible through unsecured HTTP so instructions are provided on the page to access the Jenkins console securely from your computer using an SSH tunnel.</span></span>

![Jenkins zárolásának feloldása](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

<span data-ttu-id="0700e-131">Állítsa be az alagutat az `ssh` paranccsal az oldal parancssorából. A `username` helyére a virtuális gép korábban, a virtuális gép megoldássablonból történő beállításakor kiválasztott rendszergazdáját írja be.</span><span class="sxs-lookup"><span data-stu-id="0700e-131">Set up the tunnel using the `ssh` command on the page from the command line, replacing `username` with the name of the virtual machine admin user chosen earlier when setting up the virtual machine from the solution template.</span></span>

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

<span data-ttu-id="0700e-132">Az alagút elindítása után lépjen a http://localhost:8080/ címre a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="0700e-132">After you have started the tunnel, navigate to http://localhost:8080/ on your local machine.</span></span> 

<span data-ttu-id="0700e-133">Kérje le a kezdeti jelszót az alábbi parancs parancssorbeli futtatásával, miközben SSH-n keresztül kapcsolódik a Jenkinst futtató virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="0700e-133">Get the initial password by running the following command in the command line while connected through SSH to the Jenkins VM.</span></span>

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

<span data-ttu-id="0700e-134">Először a kezdeti jelszóval oldja fel a Jenkins-irányítópult zárolását.</span><span class="sxs-lookup"><span data-stu-id="0700e-134">Unlock the Jenkins dashboard for the first time using this initial password.</span></span>

![Jenkins zárolásának feloldása](./media/install-jenkins-solution-template/jenkins-unlock.png)

<span data-ttu-id="0700e-136">A következő oldalon válassza az **Install suggested plugins** (Javasolt beépülő modulok telepítése) lehetőséget, majd hozzon létre egy Jenkins-rendszergazdát a Jenkins-irányítópult eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0700e-136">Select **Install suggested plugins** on the next page and then create a Jenkins admin user used to access the Jenkins dashboard.</span></span>

![Jenkins készen áll!](./media/install-jenkins-solution-template/jenkins-welcome.png)

<span data-ttu-id="0700e-138">A Jenkins-kiszolgáló mostantól készen áll a kódok előállítására.</span><span class="sxs-lookup"><span data-stu-id="0700e-138">The Jenkins server is now ready to build code.</span></span>

## <a name="create-your-first-job"></a><span data-ttu-id="0700e-139">Az első feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0700e-139">Create your first job</span></span>

<span data-ttu-id="0700e-140">A Jenkins-konzolon válassza a **Create new jobs** (Új feladatok létrehozása) lehetőséget, majd adja neki a **mySampleApp** nevet, és válassza a **Freestyle project** (Szabad stílusú projekt), majd az **OK** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0700e-140">Select **Create new jobs** from the Jenkins console, then name it **mySampleApp** and select **Freestyle project**, then select **OK**.</span></span>

![Új feladat létrehozása](./media/install-jenkins-solution-template/jenkins-new-job.png) 

<span data-ttu-id="0700e-142">A **Source Code Management** (Forráskódkezelés) lapon engedélyezze a **Git** elemet, majd írja be a következő URL-címet a **Repository URL** Adattár URL-címe mezőbe: `https://github.com/spring-guides/gs-spring-boot.git`</span><span class="sxs-lookup"><span data-stu-id="0700e-142">Select the **Source Code Management** tab, enable **Git**, and enter the following URL in **Repository URL**  field: `https://github.com/spring-guides/gs-spring-boot.git`</span></span>

![A Git-tárház meghatározása](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

<span data-ttu-id="0700e-144">A **Build** (Felépítés) lapon válassza az **Add build step** (Felépítési lépés hozzáadása), majd az **Invoke Gradle Script** (Gradle-szkript meghívása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0700e-144">Select the **Build** tab, then select **Add build step**, **Invoke Gradle script**.</span></span> <span data-ttu-id="0700e-145">Válassza a **Use Gradle Wrapper** (Gradle-burkoló használata) lehetőséget, majd a **Wrapper location** (Burkoló helye) mezőbe írja be a `complete`, a **Tasks** (Feladatok) mezőbe pedig a `build` értéket.</span><span class="sxs-lookup"><span data-stu-id="0700e-145">Select **Use Gradle Wrapper**, then enter `complete` in **Wrapper location** and `build` for **Tasks**.</span></span>

![Gradle-burkoló használata a létrehozáshoz](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

<span data-ttu-id="0700e-147">Válassza az **Advanced...** (Speciális) lehetőséget,</span><span class="sxs-lookup"><span data-stu-id="0700e-147">Select **Advanced..**</span></span> <span data-ttu-id="0700e-148">majd adja meg a `complete` értéket a **Root Build script** (Fő felépítési szkript) mezőben.</span><span class="sxs-lookup"><span data-stu-id="0700e-148">and then enter `complete` in the **Root Build script** field.</span></span> <span data-ttu-id="0700e-149">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0700e-149">Select **Save**.</span></span>

![Speciális beállítások megadása a Gradle-burkoló létrehozása lépésben](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-the-code"></a><span data-ttu-id="0700e-151">A kód létrehozása</span><span class="sxs-lookup"><span data-stu-id="0700e-151">Build the code</span></span>

<span data-ttu-id="0700e-152">Válassza a **Build Now** (Létrehozás most) lehetőséget a kód fordításához és a mintaalkalmazás becsomagolásához.</span><span class="sxs-lookup"><span data-stu-id="0700e-152">Select **Build Now** to compile the code and package the sample app.</span></span> <span data-ttu-id="0700e-153">A build létrehozása után válassza a projekt **Workspace** (Munkaterület) hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="0700e-153">When your build completes, select the **Workspace** link for the project.</span></span>

![A munkaterület megkeresése a JAR-fájl buildből történő lekérdezéséhez](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

<span data-ttu-id="0700e-155">Navigáljon a `complete/build/libs` könyvtárhoz, és a létrehozás sikerességének ellenőrzéséhez győződjön meg arról, hogy a `gs-spring-boot-0.1.0.jar` fájl megtalálható benne.</span><span class="sxs-lookup"><span data-stu-id="0700e-155">Navigate to `complete/build/libs` and ensure the `gs-spring-boot-0.1.0.jar` is there to verify that your build was successful.</span></span> <span data-ttu-id="0700e-156">A Jenkins-kiszolgáló mostantól készen áll saját Azure-projektek létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0700e-156">Your Jenkins server is now ready to build your own projects in Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0700e-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0700e-157">Next Steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0700e-158">Azure-beli virtuális gépek hozzáadása Jenkins-ügynökökként</span><span class="sxs-lookup"><span data-stu-id="0700e-158">Add Azure VMs as Jenkins agents</span></span>](jenkins-azure-vm-agents.md)
