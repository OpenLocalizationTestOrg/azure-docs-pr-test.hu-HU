---
title: "Az Azure alárendelt beépülő modul használata Hudson folyamatos integrációt |} Microsoft Docs"
description: "Ismerteti, hogyan használható az Azure alárendelt beépülő modul Hudson folyamatos integrációt."
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: c11b59f8ea432075b147a391de4b7bd3331e639e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-azure-slave-plug-in-with-hudson-continuous-integration"></a><span data-ttu-id="f2e76-103">Az Azure alárendelt beépülő modul használata Hudson folyamatos integrációt</span><span class="sxs-lookup"><span data-stu-id="f2e76-103">How to use the Azure slave plug-in with Hudson Continuous Integration</span></span>
<span data-ttu-id="f2e76-104">Az Azure alárendelt Hudson beépülő modul lehetővé teszi, hogy hozzon létre az alárendelt csomópontok az Azure-on, amikor fut elosztott alkot.</span><span class="sxs-lookup"><span data-stu-id="f2e76-104">The Azure slave plug-in for Hudson enables you to provision slave nodes on Azure when running distributed builds.</span></span>

## <a name="install-the-azure-slave-plug-in"></a><span data-ttu-id="f2e76-105">Az Azure alárendelt beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="f2e76-105">Install the Azure Slave plug-in</span></span>
1. <span data-ttu-id="f2e76-106">Hudson irányítópultján kattintson **kezelése Hudson**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-106">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="f2e76-107">Az a **kezelése Hudson** lapján kattintson a **kezelése beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-107">In the **Manage Hudson** page, click on **Manage Plugins**.</span></span>
3. <span data-ttu-id="f2e76-108">Kattintson a **elérhető** fülre.</span><span class="sxs-lookup"><span data-stu-id="f2e76-108">Click the **Available** tab.</span></span>
4. <span data-ttu-id="f2e76-109">Kattintson a **keresési** és típus **Azure** vonatkozó beépülő modulokhoz-lista korlátozását.</span><span class="sxs-lookup"><span data-stu-id="f2e76-109">Click **Search** and type **Azure** to limit the list to relevant plug-ins.</span></span>
   
    <span data-ttu-id="f2e76-110">Ha úgy dönt, ha az elérhető beépülő modulok listáját megtalálja az Azure alárendelt a beépülő modul a **kiszolgálófürt-felügyelet és az elosztott Build** szakasz a **mások** lapon.</span><span class="sxs-lookup"><span data-stu-id="f2e76-110">If you opt to scroll through the list of available plug-ins, you will find the Azure slave plug-in under the **Cluster Management and Distributed Build** section in the **Others** tab.</span></span>
5. <span data-ttu-id="f2e76-111">Jelölje be a **Azure alárendelt beépülő modul**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-111">Select the checkbox for **Azure Slave Plugin**.</span></span>
6. <span data-ttu-id="f2e76-112">Kattintson az **Install** (Telepítés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f2e76-112">Click **Install**.</span></span>
7. <span data-ttu-id="f2e76-113">Indítsa újra a Hudson.</span><span class="sxs-lookup"><span data-stu-id="f2e76-113">Restart Hudson.</span></span>

<span data-ttu-id="f2e76-114">Most, hogy a beépülő modul telepítve van, a következő lépések lenne, az Azure-előfizetés profillal beállíthatja a beépülő modult, és az alárendelt csomópont a virtuális gép létrehozásához használt sablon létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f2e76-114">Now that the plug-in is installed, the next steps would be to configure the plug-in with your Azure subscription profile and to create a template that will be used in creating the VM for the slave node.</span></span>

## <a name="configure-the-azure-slave-plug-in-with-your-subscription-profile"></a><span data-ttu-id="f2e76-115">Az előfizetés profillal az Azure alárendelt beépülő modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f2e76-115">Configure the Azure Slave plug-in with your subscription profile</span></span>
<span data-ttu-id="f2e76-116">Egy előfizetés profilt is hívják közzétételi beállítások, a biztonságos hitelesítő adatok, és további információkra szüksége lesz a fejlesztési környezetet az Azure-ral működni tartalmazó XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="f2e76-116">A subscription profile, also referred to as publish settings, is an XML file that contains secure credentials and some additional information you'll need to work with Azure in your development environment.</span></span> <span data-ttu-id="f2e76-117">Az Azure alárendelt beépülő modul konfigurálásához lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f2e76-117">To configure the Azure slave plug-in, you need:</span></span>

* <span data-ttu-id="f2e76-118">Az előfizetés-azonosítóval</span><span class="sxs-lookup"><span data-stu-id="f2e76-118">Your subscription id</span></span>
* <span data-ttu-id="f2e76-119">Az előfizetéshez tartozó felügyeleti tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="f2e76-119">A management certificate for your subscription</span></span>

<span data-ttu-id="f2e76-120">Ezek itt található: a [előfizetés profil].</span><span class="sxs-lookup"><span data-stu-id="f2e76-120">These can be found in your [subscription profile].</span></span> <span data-ttu-id="f2e76-121">Az alábbiakban az előfizetés profil egy példája.</span><span class="sxs-lookup"><span data-stu-id="f2e76-121">Below is an example of a subscription profile.</span></span>

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

<span data-ttu-id="f2e76-122">Miután az előfizetés profil, kövesse az alábbi lépéseket az Azure alárendelt beépülő modul konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f2e76-122">Once you have your subscription profile, follow these steps to configure the Azure slave plug-in.</span></span>

1. <span data-ttu-id="f2e76-123">Hudson irányítópultján kattintson **kezelése Hudson**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-123">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="f2e76-124">Kattintson a **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-124">Click **Configure System**.</span></span>
3. <span data-ttu-id="f2e76-125">Görgessen lefelé a lapon található a **felhő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f2e76-125">Scroll down the page to find the **Cloud** section.</span></span>
4. <span data-ttu-id="f2e76-126">Kattintson a **adja hozzá az új felhő > Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-126">Click **Add new cloud > Microsoft Azure**.</span></span>
   
    ![új felhőalapú hozzáadása][add new cloud]
   
    <span data-ttu-id="f2e76-128">Ez azt mutatja majd a mezők ahol meg kell adnia az előfizetés részletei.</span><span class="sxs-lookup"><span data-stu-id="f2e76-128">This will show the fields where you need to enter your subscription details.</span></span>
   
    ![profil konfigurálása][configure profile]
5. <span data-ttu-id="f2e76-130">Az előfizetési azonosító és felügyeleti tanúsítvány a előfizetés profilból másolással illessze be őket a megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="f2e76-130">Copy the subscription id and management certificate from your subscription profile and paste them in the appropriate fields.</span></span>
   
    <span data-ttu-id="f2e76-131">Az előfizetési azonosító és felügyeleti tanúsítvány másolásakor **nem** értékek tegye idézőjelek közé tartozik.</span><span class="sxs-lookup"><span data-stu-id="f2e76-131">When copying the subscription id and management certificate, **do not** include the quotes that enclose the values.</span></span>
6. <span data-ttu-id="f2e76-132">Kattintson a **ellenőrizze konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-132">Click on **Verify configuration**.</span></span>
7. <span data-ttu-id="f2e76-133">Ha a konfiguráció ellenőrzése sikeresen befejeződött, kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-133">When the configuration is verified successfully, click **Save**.</span></span>

## <a name="set-up-a-virtual-machine-template-for-the-azure-slave-plug-in"></a><span data-ttu-id="f2e76-134">Beállítása virtuálisgép-sablont az Azure alárendelt beépülő modul</span><span class="sxs-lookup"><span data-stu-id="f2e76-134">Set up a virtual machine template for the Azure Slave plug-in</span></span>
<span data-ttu-id="f2e76-135">Virtuálisgép-sablon a paraméterek, a beépülő modul segítségével létrehozhat egy alárendelt csomópont az Azure-határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f2e76-135">A virtual machine template defines the parameters the plug-in will use to create a slave node on Azure.</span></span> <span data-ttu-id="f2e76-136">A következő lépések azt fogja kell sablon létrehozása egy Ubuntu virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="f2e76-136">In the following steps we'll be creating template for an Ubuntu VM.</span></span>

1. <span data-ttu-id="f2e76-137">Hudson irányítópultján kattintson **kezelése Hudson**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-137">In the Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="f2e76-138">Kattintson a **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-138">Click on **Configure System**.</span></span>
3. <span data-ttu-id="f2e76-139">Görgessen lefelé a lapon található a **felhő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="f2e76-139">Scroll down the page to find the **Cloud** section.</span></span>
4. <span data-ttu-id="f2e76-140">Belül a **felhő** területen található **Azure virtuálisgép-sablon hozzáadása** , és kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2e76-140">Within the **Cloud** section, find **Add Azure Virtual Machine Template** and click the **Add** button.</span></span>
   
    ![Virtuálisgép-sablon hozzáadása][add vm template]
5. <span data-ttu-id="f2e76-142">Adja meg a felhőalapú szolgáltatás nevét a **neve** mező.</span><span class="sxs-lookup"><span data-stu-id="f2e76-142">Specify a cloud service name in the **Name** field.</span></span> <span data-ttu-id="f2e76-143">Ha a megadott név egy meglévő felhőszolgáltatáshoz hivatkozik, a virtuális gép lesz üzembe helyezve, hogy a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f2e76-143">If the name you specify refers to an existing cloud service, the VM will be provisioned in that service.</span></span> <span data-ttu-id="f2e76-144">Ellenkező esetben az Azure létrehoz egy új.</span><span class="sxs-lookup"><span data-stu-id="f2e76-144">Otherwise, Azure will create a new one.</span></span>
6. <span data-ttu-id="f2e76-145">Az a **leírás** mezőben adja meg a létrehozandó sablon leírását.</span><span class="sxs-lookup"><span data-stu-id="f2e76-145">In the **Description** field, enter text that describes the template you are creating.</span></span> <span data-ttu-id="f2e76-146">Ezek az információk csak dokumentációs célokat szolgál, és nem szerepel a virtuális gép kiépítése.</span><span class="sxs-lookup"><span data-stu-id="f2e76-146">This information is only for documentary purposes and is not used in provisioning a VM.</span></span>
7. <span data-ttu-id="f2e76-147">Az a **címkék** mezőbe írja be **linux**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-147">In the **Labels** field, enter **linux**.</span></span> <span data-ttu-id="f2e76-148">Ez a címke alapján határozza meg a sablon létrehozásakor és való hivatkozáshoz a sablon, egy Hudson feladat létrehozásakor ezt követően használható.</span><span class="sxs-lookup"><span data-stu-id="f2e76-148">This label is used to identify the template you are creating and is subsequently used to reference the template when creating a Hudson job.</span></span>
8. <span data-ttu-id="f2e76-149">Válasszon ki egy régiót, ahol a virtuális gép létrejön.</span><span class="sxs-lookup"><span data-stu-id="f2e76-149">Select a region where the VM will be created.</span></span>
9. <span data-ttu-id="f2e76-150">Válassza ki a megfelelő Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="f2e76-150">Select the appropriate VM size.</span></span>
10. <span data-ttu-id="f2e76-151">Adjon meg egy tárfiókot, ahol a virtuális gép létrejön.</span><span class="sxs-lookup"><span data-stu-id="f2e76-151">Specify a storage account where the VM will be created.</span></span> <span data-ttu-id="f2e76-152">Győződjön meg arról, hogy a felhőszolgáltatás fogja használni és ugyanabban a régióban van.</span><span class="sxs-lookup"><span data-stu-id="f2e76-152">Make sure that it is in the same region as the cloud service you'll be using.</span></span> <span data-ttu-id="f2e76-153">Ha a létrehozandó új tárhelyre, akkor ezt a mezőt üresen hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="f2e76-153">If you want new storage to be created, you can leave this field blank.</span></span>
11. <span data-ttu-id="f2e76-154">Megőrzési időtartama percben, mielőtt Hudson törli az inaktív alárendelt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f2e76-154">Retention time specifies the number of minutes before Hudson deletes an idle slave.</span></span> <span data-ttu-id="f2e76-155">Adja meg ezt az alapértelmezett értéket 60.</span><span class="sxs-lookup"><span data-stu-id="f2e76-155">Leave this at the default value of 60.</span></span>
12. <span data-ttu-id="f2e76-156">A **használati**, válassza ki azt a megfelelő állapotot, ha ez az alárendelt csomópont fogja használni.</span><span class="sxs-lookup"><span data-stu-id="f2e76-156">In **Usage**, select the appropriate condition when this slave node will be used.</span></span> <span data-ttu-id="f2e76-157">Most, válassza ki a **használata a lehető legnagyobb mértékben csomópont**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-157">For now, select **Utilize this node as much as possible**.</span></span>
    
     <span data-ttu-id="f2e76-158">Ezen a ponton az űrlap ez némileg hasonló lenne:</span><span class="sxs-lookup"><span data-stu-id="f2e76-158">At this point, your form would look somewhat similar to this:</span></span>
    
     ![sablon config][template config]
13. <span data-ttu-id="f2e76-160">A **kép termékcsalád vagy azonosító** meg kell adnia, hogy milyen rendszerkép lesz telepítve a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="f2e76-160">In **Image Family or Id** you have to specify what system image will be installed on your VM.</span></span> <span data-ttu-id="f2e76-161">Válassza ki a lemezkép termékcsaládok listáját, vagy adjon meg egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="f2e76-161">You can either select from a list of image families or specify a custom image.</span></span>
    
     <span data-ttu-id="f2e76-162">Ha azt szeretné kiválasztani a lemezkép-családok listája, adja meg a lemezkép csomagcsalád nevének (kis-és nagybetűket) karaktert.</span><span class="sxs-lookup"><span data-stu-id="f2e76-162">If you want to select from a list of image families, enter the first character (case-sensitive) of the image family name.</span></span> <span data-ttu-id="f2e76-163">Például írja be **U** Ubuntu Server családok listája megjelenik.</span><span class="sxs-lookup"><span data-stu-id="f2e76-163">For instance, typing **U** will bring up a list of Ubuntu Server families.</span></span> <span data-ttu-id="f2e76-164">Után válassza ki a listából, a Jenkins fogja használni, hogy az operációs rendszer lemezkép legújabb verzióját, ha a virtuális gép kiépítésétől.</span><span class="sxs-lookup"><span data-stu-id="f2e76-164">Once you select from the list, Jenkins will use the latest version of that system image from that family when provisioning your VM.</span></span>
    
     ![Az operációs rendszer termékcsalád listája][OS family list]
    
     <span data-ttu-id="f2e76-166">Ha a használni kívánt egyéni lemezképet, adja meg az egyéni lemezkép nevét.</span><span class="sxs-lookup"><span data-stu-id="f2e76-166">If you have a custom image that you want to use instead, enter the name of that custom image.</span></span> <span data-ttu-id="f2e76-167">Egyéni rendszerkép neve nem jelennek meg listáját, így kell győződjön meg arról, hogy a neve helyesen van-e megadva.</span><span class="sxs-lookup"><span data-stu-id="f2e76-167">Custom image names are not shown in a list so you have to ensure that the name is entered correctly.</span></span>    
    
     <span data-ttu-id="f2e76-168">Ebben az oktatóanyagban írja be a következőt **U** Ubuntu lemezképek listáját és kiválasztása **Ubuntu Server 14.04 LTS**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-168">For this tutorial, type **U** to bring up a list of Ubuntu images and select **Ubuntu Server 14.04 LTS**.</span></span>
14. <span data-ttu-id="f2e76-169">A **indítsa el a metódus**, jelölje be **SSH**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-169">For **Launch method**, select **SSH**.</span></span>
15. <span data-ttu-id="f2e76-170">Az alábbi parancsfájl másolja és illessze be a **Init parancsfájl** mező.</span><span class="sxs-lookup"><span data-stu-id="f2e76-170">Copy the script below and paste in the **Init script** field.</span></span>
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     <span data-ttu-id="f2e76-171">A **Init parancsfájl** végrehajtja a virtuális gép létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="f2e76-171">The **Init script** will be executed after the VM is created.</span></span> <span data-ttu-id="f2e76-172">Ebben a példában a parancsfájl telepíti, Java, a git és az telepítsenek.</span><span class="sxs-lookup"><span data-stu-id="f2e76-172">In this example, the script installs Java, git, and ant.</span></span>
16. <span data-ttu-id="f2e76-173">Az a **felhasználónév** és **jelszó** mezők, adja meg az előnyben részesített értékeket rendel a virtuális Gépen lévő rendszergazdai fiók.</span><span class="sxs-lookup"><span data-stu-id="f2e76-173">In the **Username** and **Password** fields, enter your preferred values for the administrator account that will be created on your VM.</span></span>
17. <span data-ttu-id="f2e76-174">Kattintson a **sablon ellenőrzése** ellenőrzése, ha a megadott paraméterek érvényesek.</span><span class="sxs-lookup"><span data-stu-id="f2e76-174">Click on **Verify Template** to check if the parameters you specified are valid.</span></span>
18. <span data-ttu-id="f2e76-175">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2e76-175">Click on **Save**.</span></span>

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a><span data-ttu-id="f2e76-176">Egy alárendelt csomópont az Azure-on futó Hudson feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f2e76-176">Create a Hudson job that runs on a slave node on Azure</span></span>
<span data-ttu-id="f2e76-177">Ebben a szakaszban egy alárendelt csomópont az Azure-on futó Hudson feladat csak létrehozunk.</span><span class="sxs-lookup"><span data-stu-id="f2e76-177">In this section, you'll be creating a Hudson task that will run on a slave node on Azure.</span></span>

1. <span data-ttu-id="f2e76-178">Hudson irányítópultján kattintson **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-178">In the Hudson dashboard, click **New Job**.</span></span>
2. <span data-ttu-id="f2e76-179">Adja meg a létrehozandó feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="f2e76-179">Enter a name for the job you are creating.</span></span>
3. <span data-ttu-id="f2e76-180">Válassza ki a feladattípus **egy ingyenes stílusú szoftver feladat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-180">For the job type, select **Build a free-style software job**.</span></span>
4. <span data-ttu-id="f2e76-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2e76-181">Click **OK**.</span></span>
5. <span data-ttu-id="f2e76-182">A feladat konfigurálása lapon válassza ki a **korlátozása, amelyben ez a projekt futtathatók**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-182">In the job configuration page, select **Restrict where this project can be run**.</span></span>
6. <span data-ttu-id="f2e76-183">Válassza ki **csomópont és a felirat menü** válassza **linux** (azt meg ezt a címkét a virtuális gép sablon létrehozásakor az előző szakaszban).</span><span class="sxs-lookup"><span data-stu-id="f2e76-183">Select **Node and label menu** and select **linux** (we specified this label when creating the virtual machine template in the previous section).</span></span>
7. <span data-ttu-id="f2e76-184">A a **Build** területen kattintson **Hozzáadás összeállítása lépés** válassza **hajtható végre a rendszerhéj**.</span><span class="sxs-lookup"><span data-stu-id="f2e76-184">In the **Build** section, click **Add build step** and select **Execute shell**.</span></span>
8. <span data-ttu-id="f2e76-185">Szerkessze a következő parancsfájl cseréje **{a githubon fióknevet}**, **{a projekt neve}**, és **{projektkönyvtárban}** a megfelelő értékeket, és illessze be a szerkesztett a szöveg területen megjelenő parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="f2e76-185">Edit the following script, replacing **{your github account name}**, **{your project name}**, and **{your project directory}** with appropriate values, and paste the edited script in the text area that appears.</span></span>
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory to project
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. <span data-ttu-id="f2e76-186">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="f2e76-186">Click on **Save**.</span></span>
10. <span data-ttu-id="f2e76-187">A Hudson irányítópult, keresse meg az imént létrehozott feladat, majd kattintson a a **build ütemezése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f2e76-187">In the Hudson dashboard, find the job you just created and click on the **Schedule a build** icon.</span></span>

<span data-ttu-id="f2e76-188">Hudson majd hozható létre az előző szakaszban létrehozott sablon használatával az alárendelt csomópont, és futtassa a parancsfájlt a feladathoz build lépésben megadott.</span><span class="sxs-lookup"><span data-stu-id="f2e76-188">Hudson will then create a slave node using the template created in the previous section and execute the script you specified in the build step for this task.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f2e76-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f2e76-189">Next Steps</span></span>
<span data-ttu-id="f2e76-190">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="f2e76-190">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="f2e76-191">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="f2e76-191">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="f2e76-192">[előfizetés profil]: http://go.microsoft.com/fwlink/?LinkID=396395</span><span class="sxs-lookup"><span data-stu-id="f2e76-192">[subscription profile]: http://go.microsoft.com/fwlink/?LinkID=396395</span></span>

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

