---
title: "aaaHow toouse hello Azure alárendelt Hudson folyamatos integrációt beépülő modul |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello Azure alárendelt Hudson folyamatos integrációt beépülő modul."
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
ms.openlocfilehash: cd6e67ad71c208aa56746aa8b70ba507da20bee9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-slave-plug-in-with-hudson-continuous-integration"></a><span data-ttu-id="3e2dd-103">Hogyan toouse hello Azure alárendelt Hudson folyamatos integrációt beépülő modul</span><span class="sxs-lookup"><span data-stu-id="3e2dd-103">How toouse hello Azure slave plug-in with Hudson Continuous Integration</span></span>
<span data-ttu-id="3e2dd-104">hello Azure alárendelt Hudson beépülő modul lehetővé teszi tooprovision alárendelt csomópontok Azure amikor fut elosztott alkot.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-104">hello Azure slave plug-in for Hudson enables you tooprovision slave nodes on Azure when running distributed builds.</span></span>

## <a name="install-hello-azure-slave-plug-in"></a><span data-ttu-id="3e2dd-105">Hello Azure alárendelt beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="3e2dd-105">Install hello Azure Slave plug-in</span></span>
1. <span data-ttu-id="3e2dd-106">Hello Hudson irányítópultot, kattintson **kezelése Hudson**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-106">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="3e2dd-107">A hello **kezelése Hudson** lapján kattintson a **kezelése beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-107">In hello **Manage Hudson** page, click on **Manage Plugins**.</span></span>
3. <span data-ttu-id="3e2dd-108">Kattintson a hello **elérhető** fülre.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-108">Click hello **Available** tab.</span></span>
4. <span data-ttu-id="3e2dd-109">Kattintson a **keresési** és típus **Azure** toolimit hello lista toorelevant beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-109">Click **Search** and type **Azure** toolimit hello list toorelevant plug-ins.</span></span>
   
    <span data-ttu-id="3e2dd-110">Ha úgy dönt, hogy tooscroll keresztül elérhető beépülő modulok hello listája, megtalálja hello Azure alárendelt hello a beépülő modul **kiszolgálófürt-felügyelet és az elosztott Build** hello című **mások** lapon.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-110">If you opt tooscroll through hello list of available plug-ins, you will find hello Azure slave plug-in under hello **Cluster Management and Distributed Build** section in hello **Others** tab.</span></span>
5. <span data-ttu-id="3e2dd-111">Válassza ki a hello jelölőnégyzetét **Azure alárendelt beépülő modul**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-111">Select hello checkbox for **Azure Slave Plugin**.</span></span>
6. <span data-ttu-id="3e2dd-112">Kattintson az **Install** (Telepítés) gombra.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-112">Click **Install**.</span></span>
7. <span data-ttu-id="3e2dd-113">Indítsa újra a Hudson.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-113">Restart Hudson.</span></span>

<span data-ttu-id="3e2dd-114">Most, hogy hello beépülő modul telepítve van, akkor hello lépések tooconfigure hello beépülő modul, az Azure-előfizetés profil és a toocreate hello VM hello az alárendelt csomópont létrehozásakor használt sablon lenne.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-114">Now that hello plug-in is installed, hello next steps would be tooconfigure hello plug-in with your Azure subscription profile and toocreate a template that will be used in creating hello VM for hello slave node.</span></span>

## <a name="configure-hello-azure-slave-plug-in-with-your-subscription-profile"></a><span data-ttu-id="3e2dd-115">Az előfizetés profillal hello Azure alárendelt beépülő modul konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3e2dd-115">Configure hello Azure Slave plug-in with your subscription profile</span></span>
<span data-ttu-id="3e2dd-116">Egy előfizetés profilt is hivatkozott tooas közzétételi beállítások, egy XML-fájl, amely tartalmazza a biztonságos hitelesítő adatok, és néhány további információ a fejlesztési környezetet az Azure-ral toowork lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-116">A subscription profile, also referred tooas publish settings, is an XML file that contains secure credentials and some additional information you'll need toowork with Azure in your development environment.</span></span> <span data-ttu-id="3e2dd-117">tooconfigure hello Azure alárendelt beépülő modult, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3e2dd-117">tooconfigure hello Azure slave plug-in, you need:</span></span>

* <span data-ttu-id="3e2dd-118">Az előfizetés-azonosítóval</span><span class="sxs-lookup"><span data-stu-id="3e2dd-118">Your subscription id</span></span>
* <span data-ttu-id="3e2dd-119">Az előfizetéshez tartozó felügyeleti tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="3e2dd-119">A management certificate for your subscription</span></span>

<span data-ttu-id="3e2dd-120">Ezek itt található: a [előfizetés profil].</span><span class="sxs-lookup"><span data-stu-id="3e2dd-120">These can be found in your [subscription profile].</span></span> <span data-ttu-id="3e2dd-121">Az alábbiakban az előfizetés profil egy példája.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-121">Below is an example of a subscription profile.</span></span>

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

<span data-ttu-id="3e2dd-122">Miután az előfizetés profil, hajtsa végre az ezen lépések tooconfigure hello Azure alárendelt beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-122">Once you have your subscription profile, follow these steps tooconfigure hello Azure slave plug-in.</span></span>

1. <span data-ttu-id="3e2dd-123">Hello Hudson irányítópultot, kattintson **kezelése Hudson**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-123">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="3e2dd-124">Kattintson a **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-124">Click **Configure System**.</span></span>
3. <span data-ttu-id="3e2dd-125">Görgessen lefelé hello lap toofind hello **felhő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-125">Scroll down hello page toofind hello **Cloud** section.</span></span>
4. <span data-ttu-id="3e2dd-126">Kattintson a **adja hozzá az új felhő > Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-126">Click **Add new cloud > Microsoft Azure**.</span></span>
   
    ![új felhőalapú hozzáadása][add new cloud]
   
    <span data-ttu-id="3e2dd-128">Ez azt mutatja majd hello mezők tooenter kell az előfizetés részletei.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-128">This will show hello fields where you need tooenter your subscription details.</span></span>
   
    ![profil konfigurálása][configure profile]
5. <span data-ttu-id="3e2dd-130">Hello előfizetési azonosító és felügyeleti tanúsítvány a előfizetés profilból másolással illessze be őket a hello megfelelő mezőket.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-130">Copy hello subscription id and management certificate from your subscription profile and paste them in hello appropriate fields.</span></span>
   
    <span data-ttu-id="3e2dd-131">Hello előfizetési azonosító és felügyeleti tanúsítvány, másolásakor **nem** hello idézőjelek között, tegye a hello értékeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-131">When copying hello subscription id and management certificate, **do not** include hello quotes that enclose hello values.</span></span>
6. <span data-ttu-id="3e2dd-132">Kattintson a **ellenőrizze konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-132">Click on **Verify configuration**.</span></span>
7. <span data-ttu-id="3e2dd-133">Hello konfiguráció sikeres ellenőrzését követően kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-133">When hello configuration is verified successfully, click **Save**.</span></span>

## <a name="set-up-a-virtual-machine-template-for-hello-azure-slave-plug-in"></a><span data-ttu-id="3e2dd-134">Beépülő modul hello Azure alárendelt virtuálisgép-sablon beállítása</span><span class="sxs-lookup"><span data-stu-id="3e2dd-134">Set up a virtual machine template for hello Azure Slave plug-in</span></span>
<span data-ttu-id="3e2dd-135">A virtuális gép sablon meghatározza hello hello beépülő modul egy alárendelt csomópont toocreate fogja használni az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-135">A virtual machine template defines hello parameters hello plug-in will use toocreate a slave node on Azure.</span></span> <span data-ttu-id="3e2dd-136">Az alábbi lépésekkel hello azt fogja kell sablon létrehozása egy Ubuntu virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-136">In hello following steps we'll be creating template for an Ubuntu VM.</span></span>

1. <span data-ttu-id="3e2dd-137">Hello Hudson irányítópultot, kattintson **kezelése Hudson**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-137">In hello Hudson dashboard, click **Manage Hudson**.</span></span>
2. <span data-ttu-id="3e2dd-138">Kattintson a **rendszer konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-138">Click on **Configure System**.</span></span>
3. <span data-ttu-id="3e2dd-139">Görgessen lefelé hello lap toofind hello **felhő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-139">Scroll down hello page toofind hello **Cloud** section.</span></span>
4. <span data-ttu-id="3e2dd-140">Hello belül **felhő** területen található **Azure virtuálisgép-sablon hozzáadása** hello kattintson **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-140">Within hello **Cloud** section, find **Add Azure Virtual Machine Template** and click hello **Add** button.</span></span>
   
    ![Virtuálisgép-sablon hozzáadása][add vm template]
5. <span data-ttu-id="3e2dd-142">Adja meg a felhőszolgáltatás neve hello **neve** mező.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-142">Specify a cloud service name in hello **Name** field.</span></span> <span data-ttu-id="3e2dd-143">Ha hello nevét adja meg, hogy a meglévő felhőalapú szolgáltatás tooan utal, hello VM lesznek üzembe helyezve, hogy a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-143">If hello name you specify refers tooan existing cloud service, hello VM will be provisioned in that service.</span></span> <span data-ttu-id="3e2dd-144">Ellenkező esetben az Azure létrehoz egy új.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-144">Otherwise, Azure will create a new one.</span></span>
6. <span data-ttu-id="3e2dd-145">A hello **leírás** mezőbe írja be a létrehozandó hello sablon leírását.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-145">In hello **Description** field, enter text that describes hello template you are creating.</span></span> <span data-ttu-id="3e2dd-146">Ezek az információk csak dokumentációs célokat szolgál, és nem szerepel a virtuális gép kiépítése.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-146">This information is only for documentary purposes and is not used in provisioning a VM.</span></span>
7. <span data-ttu-id="3e2dd-147">A hello **címkék** mezőbe írja be **linux**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-147">In hello **Labels** field, enter **linux**.</span></span> <span data-ttu-id="3e2dd-148">Ez a címke használt tooidentify hello sablon létrehozásakor és ezt követően használt tooreference hello sablon Hudson feladat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-148">This label is used tooidentify hello template you are creating and is subsequently used tooreference hello template when creating a Hudson job.</span></span>
8. <span data-ttu-id="3e2dd-149">Válasszon ki egy régiót, ahol a virtuális gép hello létrejön.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-149">Select a region where hello VM will be created.</span></span>
9. <span data-ttu-id="3e2dd-150">Válassza ki a megfelelő hello Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-150">Select hello appropriate VM size.</span></span>
10. <span data-ttu-id="3e2dd-151">Adjon meg egy tárfiókot, ahol a virtuális gép hello létrejön.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-151">Specify a storage account where hello VM will be created.</span></span> <span data-ttu-id="3e2dd-152">Győződjön meg arról, hogy hello ugyanabban a régióban hello felhőalapú szolgáltatást fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-152">Make sure that it is in hello same region as hello cloud service you'll be using.</span></span> <span data-ttu-id="3e2dd-153">Ha azt szeretné, hogy a létrehozott új tárolási toobe, akkor ezt a mezőt üresen hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-153">If you want new storage toobe created, you can leave this field blank.</span></span>
11. <span data-ttu-id="3e2dd-154">Megőrzési idő megadja perc elteltével Hudson törli az inaktív alárendelt hello számát.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-154">Retention time specifies hello number of minutes before Hudson deletes an idle slave.</span></span> <span data-ttu-id="3e2dd-155">Adja meg ezt az alapértelmezett értéket hello 60.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-155">Leave this at hello default value of 60.</span></span>
12. <span data-ttu-id="3e2dd-156">A **használati**, válassza ki hello megfelelő feltétel, ha ez az alárendelt csomópont fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-156">In **Usage**, select hello appropriate condition when this slave node will be used.</span></span> <span data-ttu-id="3e2dd-157">Most, válassza ki a **használata a lehető legnagyobb mértékben csomópont**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-157">For now, select **Utilize this node as much as possible**.</span></span>
    
     <span data-ttu-id="3e2dd-158">Ezen a ponton az űrlap némileg hasonló toothis jelenne meg:</span><span class="sxs-lookup"><span data-stu-id="3e2dd-158">At this point, your form would look somewhat similar toothis:</span></span>
    
     ![sablon config][template config]
13. <span data-ttu-id="3e2dd-160">A **kép termékcsalád vagy azonosító** rendelkezik toospecify milyen rendszerkép lesz telepítve a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-160">In **Image Family or Id** you have toospecify what system image will be installed on your VM.</span></span> <span data-ttu-id="3e2dd-161">Válassza ki a lemezkép termékcsaládok listáját, vagy adjon meg egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-161">You can either select from a list of image families or specify a custom image.</span></span>
    
     <span data-ttu-id="3e2dd-162">Ha azt szeretné, hogy a lemezkép-családok listája tooselect, adja meg a hello első karakterének (kis-és nagybetűket) hello kép csomagcsalád nevét.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-162">If you want tooselect from a list of image families, enter hello first character (case-sensitive) of hello image family name.</span></span> <span data-ttu-id="3e2dd-163">Például írja be **U** Ubuntu Server családok listája megjelenik.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-163">For instance, typing **U** will bring up a list of Ubuntu Server families.</span></span> <span data-ttu-id="3e2dd-164">Miután hello listából válassza ki a Jenkins fogja használni hello legújabb verzióját, hogy az operációs rendszer lemezkép, amikor a virtuális gép kiépítésétől.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-164">Once you select from hello list, Jenkins will use hello latest version of that system image from that family when provisioning your VM.</span></span>
    
     ![Az operációs rendszer termékcsalád listája][OS family list]
    
     <span data-ttu-id="3e2dd-166">Ha rendelkezik, amelyet toouse helyette egyéni kép, adja meg az egyéni lemezkép hello nevét.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-166">If you have a custom image that you want toouse instead, enter hello name of that custom image.</span></span> <span data-ttu-id="3e2dd-167">Egyéni rendszerkép neve nem láthatók listáját, hogy rendelkezik, amelyek neve hello tooensure megfelelően van-e megadva.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-167">Custom image names are not shown in a list so you have tooensure that hello name is entered correctly.</span></span>    
    
     <span data-ttu-id="3e2dd-168">Ebben az oktatóanyagban írja be a következőt **U** Ubuntu lemezképeihez, és válassza ki listája toobring **Ubuntu Server 14.04 LTS**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-168">For this tutorial, type **U** toobring up a list of Ubuntu images and select **Ubuntu Server 14.04 LTS**.</span></span>
14. <span data-ttu-id="3e2dd-169">A **indítsa el a metódus**, jelölje be **SSH**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-169">For **Launch method**, select **SSH**.</span></span>
15. <span data-ttu-id="3e2dd-170">Az alábbi parancsfájl hello másolja és illessze be a hello **Init parancsfájl** mező.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-170">Copy hello script below and paste in hello **Init script** field.</span></span>
    
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
    
     <span data-ttu-id="3e2dd-171">Hello **Init parancsfájl** hello virtuális gép létrehozása után a rendszer hajtja majd végre.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-171">hello **Init script** will be executed after hello VM is created.</span></span> <span data-ttu-id="3e2dd-172">Ebben a példában a hello parancsfájl telepíti a Java, a git és az telepítsenek.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-172">In this example, hello script installs Java, git, and ant.</span></span>
16. <span data-ttu-id="3e2dd-173">A hello **felhasználónév** és **jelszó** mezőkben adja meg a kívánt értékeket hello rendszergazdai fiók, amely a virtuális gép létrejön.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-173">In hello **Username** and **Password** fields, enter your preferred values for hello administrator account that will be created on your VM.</span></span>
17. <span data-ttu-id="3e2dd-174">Kattintson a **sablon ellenőrzése** toocheck, ha a megadott hello paraméterek érvényesek.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-174">Click on **Verify Template** toocheck if hello parameters you specified are valid.</span></span>
18. <span data-ttu-id="3e2dd-175">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-175">Click on **Save**.</span></span>

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a><span data-ttu-id="3e2dd-176">Egy alárendelt csomópont az Azure-on futó Hudson feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e2dd-176">Create a Hudson job that runs on a slave node on Azure</span></span>
<span data-ttu-id="3e2dd-177">Ebben a szakaszban egy alárendelt csomópont az Azure-on futó Hudson feladat csak létrehozunk.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-177">In this section, you'll be creating a Hudson task that will run on a slave node on Azure.</span></span>

1. <span data-ttu-id="3e2dd-178">Hello Hudson irányítópultot, kattintson **új feladat**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-178">In hello Hudson dashboard, click **New Job**.</span></span>
2. <span data-ttu-id="3e2dd-179">Adja meg a létrehozandó hello feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-179">Enter a name for hello job you are creating.</span></span>
3. <span data-ttu-id="3e2dd-180">Hello feladattípus, válassza a **egy ingyenes stílusú szoftver feladat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-180">For hello job type, select **Build a free-style software job**.</span></span>
4. <span data-ttu-id="3e2dd-181">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-181">Click **OK**.</span></span>
5. <span data-ttu-id="3e2dd-182">Hello feladat konfigurálása lapon válassza ki a **korlátozása, amelyben ez a projekt futtathatók**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-182">In hello job configuration page, select **Restrict where this project can be run**.</span></span>
6. <span data-ttu-id="3e2dd-183">Válassza ki **csomópont és a felirat menü** válassza **linux** (azt meg ezt a címkét az előző szakaszban hello hello virtuálisgép-sablon létrehozásakor).</span><span class="sxs-lookup"><span data-stu-id="3e2dd-183">Select **Node and label menu** and select **linux** (we specified this label when creating hello virtual machine template in hello previous section).</span></span>
7. <span data-ttu-id="3e2dd-184">A hello **Build** területen kattintson **Hozzáadás összeállítása lépés** válassza **hajtható végre a rendszerhéj**.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-184">In hello **Build** section, click **Add build step** and select **Execute shell**.</span></span>
8. <span data-ttu-id="3e2dd-185">A következő parancsfájlt, hogy hello szerkesztése **{a githubon fióknevet}**, **{a projekt neve}**, és **{projektkönyvtárban}** a megfelelő értékeket, és illessze be a hello parancsfájl hello szöveg területen megjelenő szerkeszteni.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-185">Edit hello following script, replacing **{your github account name}**, **{your project name}**, and **{your project directory}** with appropriate values, and paste hello edited script in hello text area that appears.</span></span>
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory tooproject
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. <span data-ttu-id="3e2dd-186">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-186">Click on **Save**.</span></span>
10. <span data-ttu-id="3e2dd-187">A hello Hudson irányítópult, újonnan létrehozott hello feladat keresése, majd kattintson a hello **build ütemezése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-187">In hello Hudson dashboard, find hello job you just created and click on hello **Schedule a build** icon.</span></span>

<span data-ttu-id="3e2dd-188">Hudson majd egy alárendelt csomópont hello előző szakaszban létrehozott hello sablonnal létrehozása, és ehhez a feladathoz hello build lépésben megadott hello parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="3e2dd-188">Hudson will then create a slave node using hello template created in hello previous section and execute hello script you specified in hello build step for this task.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e2dd-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3e2dd-189">Next Steps</span></span>
<span data-ttu-id="3e2dd-190">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].</span><span class="sxs-lookup"><span data-stu-id="3e2dd-190">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<!-- URL List -->

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[előfizetés profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

