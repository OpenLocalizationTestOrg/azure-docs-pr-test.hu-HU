---
title: "egy Azure-Jenkins kiszolgáló aaaCreate"
description: "Jenkins telepíthető egy Azure Linux virtuális gép hello Jenkins megoldás sablonból, és egy minta Java-alkalmazás létrehozása."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 82ab2ac52594acba131414b449b608978591d4b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-hello-azure-portal"></a><span data-ttu-id="e00c8-103">Hozzon létre egy Jenkins kiszolgálót egy Azure Linux virtuális gépen a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e00c8-103">Create a Jenkins server on an Azure Linux VM from hello Azure portal</span></span>

<span data-ttu-id="e00c8-104">A gyors üzembe helyezés bemutatja, hogyan tooinstall [Jenkins](https://jenkins.io) az Ubuntu Linux virtuális gép hello eszközök és beépülő modulok konfigurált toowork az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="e00c8-104">This quickstart shows how tooinstall [Jenkins](https://jenkins.io) on an Ubuntu Linux VM with hello tools and plug-ins configured toowork with Azure.</span></span> <span data-ttu-id="e00c8-105">Ha elkészült, rendelkezni fog egy Azure-ban futó Jenkins-kiszolgálóval, amely egy Java-mintaalkalmazást hoz létre a [GitHubról](https://github.com).</span><span class="sxs-lookup"><span data-stu-id="e00c8-105">When you're finished, you have a Jenkins server running in Azure building a sample Java app from [GitHub](https://github.com).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e00c8-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e00c8-106">Prerequisites</span></span>

* <span data-ttu-id="e00c8-107">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="e00c8-107">An Azure subscription</span></span>
* <span data-ttu-id="e00c8-108">A számítógép parancssorban hozzáférési tooSSH (például hello Bash rendszerhéjat vagy [PuTTY](http://www.putty.org/))</span><span class="sxs-lookup"><span data-stu-id="e00c8-108">Access tooSSH on your computer's command line (such as hello Bash shell or [PuTTY](http://www.putty.org/))</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-jenkins-vm-from-hello-solution-template"></a><span data-ttu-id="e00c8-109">Hello megoldás sablonból hello Jenkins virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="e00c8-109">Create hello Jenkins VM from hello solution template</span></span>

<span data-ttu-id="e00c8-110">Nyissa meg hello [Jenkins a Piactéri lemezképhez](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) a webböngészőt, és válasszon **most az beszerzése informatikai** hello hello lap bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="e00c8-110">Open hello [marketplace image for Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) in your web browser and select  **GET IT NOW** from hello left-hand side of hello page.</span></span> <span data-ttu-id="e00c8-111">Felülvizsgálati hello díjszabás részleteit, és válassza **Folytatás**, majd jelölje be **létrehozása** tooconfigure hello Jenkins kiszolgáló hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e00c8-111">Review hello pricing details and select **Continue**, then select **Create** tooconfigure hello Jenkins server in hello Azure portal.</span></span> 
   
![Azure Portal párbeszédpanel](./media/install-jenkins-solution-template/ap-create.png)

<span data-ttu-id="e00c8-113">A hello **konfigurálja az alapbeállításokat** lapra, adja meg a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="e00c8-113">In hello **Configure basic settings** tab, fill in hello following fields:</span></span>

![Az alapvető beállítások konfigurálása](./media/install-jenkins-solution-template/ap-basic.png)

* <span data-ttu-id="e00c8-115">Írja be a **Jenkins** értéket a **Név** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e00c8-115">Use **Jenkins** for **Name**.</span></span>
* <span data-ttu-id="e00c8-116">Írjon be egy **felhasználónevet**.</span><span class="sxs-lookup"><span data-stu-id="e00c8-116">Enter a **User name**.</span></span> <span data-ttu-id="e00c8-117">hello felhasználónevet meg kell felelnie [kapcsolatos követelmények](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span><span class="sxs-lookup"><span data-stu-id="e00c8-117">hello user name must meet [specific requirements](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).</span></span>
* <span data-ttu-id="e00c8-118">Válassza ki **jelszó** , hello **hitelesítési típus** és adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="e00c8-118">Select **Password** as hello **Authentication type** and enter a password.</span></span> <span data-ttu-id="e00c8-119">hello ügyeljen arra, hogy egy nagybetűt, számot és egy speciális karaktert.</span><span class="sxs-lookup"><span data-stu-id="e00c8-119">hello password must have an upper case character, a number, and one special character.</span></span>
* <span data-ttu-id="e00c8-120">Használjon **myJenkinsResourceGroup** a hello **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="e00c8-120">Use **myJenkinsResourceGroup** for hello **Resource Group**.</span></span>
* <span data-ttu-id="e00c8-121">Válassza ki a hello **USA keleti régiója** [az Azure-régió](https://azure.microsoft.com/regions/) a hello **hely** legördülő.</span><span class="sxs-lookup"><span data-stu-id="e00c8-121">Choose hello **East US** [Azure region](https://azure.microsoft.com/regions/) from hello **Location** drop-down.</span></span>

<span data-ttu-id="e00c8-122">Válassza ki **OK** tooproceed toohello **további beállításokat** fülre. Adjon meg egy egyedi tartomány neve tooidentify hello Jenkins kiszolgálót, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="e00c8-122">Select **OK** tooproceed toohello **Configure additional options** tab. Enter a unique domain name tooidentify hello Jenkins server and select **OK**.</span></span>

![További beállítások megadása](./media/install-jenkins-solution-template/ap-addtional.png)  

 <span data-ttu-id="e00c8-124">Után az ellenőrzés eredménye akkor megfelelő, válassza a **OK** újra a hello **összegzés** fülre. Végül válassza ki **beszerzési** toocreate hello Jenkins virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="e00c8-124">Once validation passes, select **OK** again from hello **Summary** tab. Finally, select **Purchase** toocreate hello Jenkins VM.</span></span> <span data-ttu-id="e00c8-125">Ha a kiszolgáló készen áll, értesítést kap a hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="e00c8-125">When your server is ready, you get a notification in hello Azure portal:</span></span>   

![„A Jenkins készen áll” értesítés](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-toojenkins"></a><span data-ttu-id="e00c8-127">Csatlakozás tooJenkins</span><span class="sxs-lookup"><span data-stu-id="e00c8-127">Connect tooJenkins</span></span>

<span data-ttu-id="e00c8-128">A böngészőben nyissa meg a tooyour virtuális gép (például http://jenkins2517454.eastus.cloudapp.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e00c8-128">Navigate tooyour virtual machine (for example, http://jenkins2517454.eastus.cloudapp.azure.com/) in  your web browser.</span></span> <span data-ttu-id="e00c8-129">hello Jenkins konzol nem érhető el, nem biztonságos HTTP Protokollon keresztül, így útmutatást a hello lap tooaccess hello Jenkins konzolon biztonságosan a számítógépről SSH-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e00c8-129">hello Jenkins console is inaccessible through unsecured HTTP so instructions are provided on hello page tooaccess hello Jenkins console securely from your computer using an SSH tunnel.</span></span>

![Jenkins zárolásának feloldása](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

<span data-ttu-id="e00c8-131">Hello alagút használatával hello beállítása `ssh` parancs hello lapon hello parancssorból cseréje `username` hello virtuális gép rendszergazdai jogú felhasználó hello megoldás sablonból hello virtuális gép beállítása során a korábban kiválasztott hello nevére.</span><span class="sxs-lookup"><span data-stu-id="e00c8-131">Set up hello tunnel using hello `ssh` command on hello page from hello command line, replacing `username` with hello name of hello virtual machine admin user chosen earlier when setting up hello virtual machine from hello solution template.</span></span>

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

<span data-ttu-id="e00c8-132">Miután elindította hello alagút, keresse meg a toohttp://localhost:8080 / a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e00c8-132">After you have started hello tunnel, navigate toohttp://localhost:8080/ on your local machine.</span></span> 

<span data-ttu-id="e00c8-133">Kezdeti jelszó hello lekéréséhez futtassa a következő parancsot a parancssorban hello SSH toohello Jenkins VM keresztül csatlakozik hello.</span><span class="sxs-lookup"><span data-stu-id="e00c8-133">Get hello initial password by running hello following command in hello command line while connected through SSH toohello Jenkins VM.</span></span>

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

<span data-ttu-id="e00c8-134">Feloldási hello Jenkins irányítópult hello az első alkalommal a kezdeti jelszó használatával.</span><span class="sxs-lookup"><span data-stu-id="e00c8-134">Unlock hello Jenkins dashboard for hello first time using this initial password.</span></span>

![Jenkins zárolásának feloldása](./media/install-jenkins-solution-template/jenkins-unlock.png)

<span data-ttu-id="e00c8-136">Válassza ki **javasolt beépülő modulok telepítése** hello a következő lap, és hozzon létre egy Jenkins admin használt felhasználói tooaccess hello Jenkins irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="e00c8-136">Select **Install suggested plugins** on hello next page and then create a Jenkins admin user used tooaccess hello Jenkins dashboard.</span></span>

![Jenkins készen áll!](./media/install-jenkins-solution-template/jenkins-welcome.png)

<span data-ttu-id="e00c8-138">hello Jenkins kiszolgáló mostantól készen áll a toobuild kódot.</span><span class="sxs-lookup"><span data-stu-id="e00c8-138">hello Jenkins server is now ready toobuild code.</span></span>

## <a name="create-your-first-job"></a><span data-ttu-id="e00c8-139">Az első feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="e00c8-139">Create your first job</span></span>

<span data-ttu-id="e00c8-140">Válassza ki **hozzon létre új feladatokat** hello Jenkins konzolon, majd nevezze el **mySampleApp** válassza **Freestyle projekt**, majd jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="e00c8-140">Select **Create new jobs** from hello Jenkins console, then name it **mySampleApp** and select **Freestyle project**, then select **OK**.</span></span>

![Új feladat létrehozása](./media/install-jenkins-solution-template/jenkins-new-job.png) 

<span data-ttu-id="e00c8-142">SELECT hello **forrás kód felügyeleti** lapján engedélyezése **Git**, és írja be a következő URL-címet hello **tárház URL-CÍMÉT** mező:`https://github.com/spring-guides/gs-spring-boot.git`</span><span class="sxs-lookup"><span data-stu-id="e00c8-142">Select hello **Source Code Management** tab, enable **Git**, and enter hello following URL in **Repository URL**  field: `https://github.com/spring-guides/gs-spring-boot.git`</span></span>

![Hello Git-tárház meghatározása](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

<span data-ttu-id="e00c8-144">Jelölje be hello **Build** lapra, majd válassza ki **Hozzáadás összeállítása lépés**, **meghívása Gradle parancsfájl**.</span><span class="sxs-lookup"><span data-stu-id="e00c8-144">Select hello **Build** tab, then select **Add build step**, **Invoke Gradle script**.</span></span> <span data-ttu-id="e00c8-145">Válassza a **Use Gradle Wrapper** (Gradle-burkoló használata) lehetőséget, majd a **Wrapper location** (Burkoló helye) mezőbe írja be a `complete`, a **Tasks** (Feladatok) mezőbe pedig a `build` értéket.</span><span class="sxs-lookup"><span data-stu-id="e00c8-145">Select **Use Gradle Wrapper**, then enter `complete` in **Wrapper location** and `build` for **Tasks**.</span></span>

![Hello Gradle burkoló toobuild használata](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

<span data-ttu-id="e00c8-147">Válassza az **Advanced...** (Speciális) lehetőséget,</span><span class="sxs-lookup"><span data-stu-id="e00c8-147">Select **Advanced..**</span></span> <span data-ttu-id="e00c8-148">majd adja meg `complete` a hello **legfelső szintű Build script** mező.</span><span class="sxs-lookup"><span data-stu-id="e00c8-148">and then enter `complete` in hello **Root Build script** field.</span></span> <span data-ttu-id="e00c8-149">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="e00c8-149">Select **Save**.</span></span>

![Hello Gradle burkoló build lépésben speciális beállításainak megadása](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-hello-code"></a><span data-ttu-id="e00c8-151">Hello kód létrehozása</span><span class="sxs-lookup"><span data-stu-id="e00c8-151">Build hello code</span></span>

<span data-ttu-id="e00c8-152">Válassza ki **Build most** toocompile hello kód és a csomag hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e00c8-152">Select **Build Now** toocompile hello code and package hello sample app.</span></span> <span data-ttu-id="e00c8-153">A build befejezése után válassza ki a hello **munkaterület** hello projekt hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="e00c8-153">When your build completes, select hello **Workspace** link for hello project.</span></span>

![Keresse meg a toohello munkaterület tooget hello JAR-fájlra a hello build](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

<span data-ttu-id="e00c8-155">Keresse meg a túl`complete/build/libs` , és ellenőrizze a hello `gs-spring-boot-0.1.0.jar` van-e, hogy sikeres volt-e a build tooverify.</span><span class="sxs-lookup"><span data-stu-id="e00c8-155">Navigate too`complete/build/libs` and ensure hello `gs-spring-boot-0.1.0.jar` is there tooverify that your build was successful.</span></span> <span data-ttu-id="e00c8-156">A kiszolgáló jelenleg Jenkins kész toobuild saját projektek az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e00c8-156">Your Jenkins server is now ready toobuild your own projects in Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e00c8-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e00c8-157">Next Steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e00c8-158">Azure-beli virtuális gépek hozzáadása Jenkins-ügynökökként</span><span class="sxs-lookup"><span data-stu-id="e00c8-158">Add Azure VMs as Jenkins agents</span></span>](jenkins-azure-vm-agents.md)
