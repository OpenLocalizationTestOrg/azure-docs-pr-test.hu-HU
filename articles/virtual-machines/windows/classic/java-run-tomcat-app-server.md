---
title: "aaaRun Java-alkalmazáskiszolgáló a klasszikus Azure virtuális gép |} Microsoft Docs"
description: "Ebben az oktatóanyagban létre hello klasszikus üzembe helyezési modellel, és bemutatja hogyan erőforrást használ, egy Windows virtuális toocreate számítógéphez, és konfigurálja azt toorun Apache Tomcat alkalmazáskiszolgáló."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: d627aa09-f7d6-4239-8110-f8fc5111b939
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 03/16/2017
ms.author: robmcm
ms.openlocfilehash: 2d9f586c9f628d3738522b320996b95b078d7454
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-java-application-server-on-a-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="36950-103">Hogyan toorun egy Java-alkalmazáskiszolgáló virtuális gépen létre hello klasszikus telepítési modell</span><span class="sxs-lookup"><span data-stu-id="36950-103">How toorun a Java application server on a virtual machine created with hello classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="36950-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="36950-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="36950-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="36950-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="36950-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="36950-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="36950-107">A Resource Manager sablon toodeploy Java 8 és Tomcat egy webalkalmazást, lásd: [Itt](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span><span class="sxs-lookup"><span data-stu-id="36950-107">For a Resource Manager template toodeploy a webapp with Java 8 and Tomcat, see [here](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span></span>

<span data-ttu-id="36950-108">A virtuális gép tooprovide server funkcióinak használata az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="36950-108">With Azure, you can use a virtual machine tooprovide server capabilities.</span></span> <span data-ttu-id="36950-109">Például egy Azure-on futó virtuális gép konfigurált toohost egy Java-alkalmazáskiszolgáló, például az Apache Tomcat is lehet.</span><span class="sxs-lookup"><span data-stu-id="36950-109">As an example, a virtual machine running on Azure can be configured toohost a Java application server, such as Apache Tomcat.</span></span>

<span data-ttu-id="36950-110">Ez az útmutató befejezése után fog megértéséhez toocreate egy virtuális gépet az Azure-on fut, és konfigurálja a Java-alkalmazáskiszolgáló toorun.</span><span class="sxs-lookup"><span data-stu-id="36950-110">After completing this guide, you will have an understanding of how toocreate a virtual machine running on Azure and configure it toorun a Java application server.</span></span> <span data-ttu-id="36950-111">Ismerje meg lesz, és hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="36950-111">You will learn and perform hello following tasks:</span></span>

* <span data-ttu-id="36950-112">Hogyan toocreate egy virtuális gépet, amely egy Java fejlesztői készlet (JDK) már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="36950-112">How toocreate a virtual machine that has a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="36950-113">Tooremotely hogyan tooyour virtuálisgép jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="36950-113">How tooremotely sign in tooyour virtual machine.</span></span>
* <span data-ttu-id="36950-114">Hogyan tooinstall egy Java-alkalmazáskiszolgáló--Apache Tomcat – a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="36950-114">How tooinstall a Java application server--Apache Tomcat--on your virtual machine.</span></span>
* <span data-ttu-id="36950-115">Hogyan toocreate egy végpontot a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="36950-115">How toocreate an endpoint for your virtual machine.</span></span>
* <span data-ttu-id="36950-116">Hogyan tooopen hello egy portot az alkalmazás-kiszolgáló tűzfal.</span><span class="sxs-lookup"><span data-stu-id="36950-116">How tooopen a port in hello firewall for your application server.</span></span>

<span data-ttu-id="36950-117">hello befejeződött a virtuális gépen futó Tomcat telepítési eredményez.</span><span class="sxs-lookup"><span data-stu-id="36950-117">hello completed installation results in Tomcat running on a virtual machine.</span></span>

![Apache Tomcat rendszerű virtuális gép][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a><span data-ttu-id="36950-119">a virtuális gépek toocreate</span><span class="sxs-lookup"><span data-stu-id="36950-119">toocreate a virtual machine</span></span>
1. <span data-ttu-id="36950-120">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36950-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="36950-121">Kattintson a **új**, kattintson a **számítási**, majd kattintson a **láthatja az összes** hello a **a kiemelt alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="36950-121">Click **New**, click **Compute**, then click **See all** in hello **Featured apps**.</span></span>
3. <span data-ttu-id="36950-122">Kattintson a **JDK**, kattintson a **JDK 8** a hello **JDK** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="36950-122">Click **JDK**, click **JDK 8** in hello **JDK** pane.</span></span>  
   <span data-ttu-id="36950-123">Virtuálisgép-lemezképek támogató **JDK 6** és **JDK 7** érhető el, ha örökölt alkalmazásokat, amelyek nem kész toorun JDK 8-ban.</span><span class="sxs-lookup"><span data-stu-id="36950-123">Virtual machine images that support **JDK 6** and **JDK 7** are available if you have legacy applications that are not ready toorun in JDK 8.</span></span>
4. <span data-ttu-id="36950-124">Hello JDK 8 ablaktáblában jelölje ki **klasszikus**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="36950-124">In hello JDK 8 pane, select **Classic**, then click **Create**.</span></span>
5. <span data-ttu-id="36950-125">A hello **alapjai** panel:</span><span class="sxs-lookup"><span data-stu-id="36950-125">In hello **Basics** blade:</span></span>
   1. <span data-ttu-id="36950-126">Adja meg a hello virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="36950-126">Specify a name for hello virtual machine.</span></span>
   2. <span data-ttu-id="36950-127">Adja meg egy nevet a hello rendszergazda hello **felhasználónév** mező.</span><span class="sxs-lookup"><span data-stu-id="36950-127">Enter a name for hello administrator in hello **User Name** field.</span></span> <span data-ttu-id="36950-128">Ne feledje ezt a nevet, és ahhoz tartozó jelszót, a következő mező hello követő hello.</span><span class="sxs-lookup"><span data-stu-id="36950-128">Remember this name and hello associated password that follows in hello next field.</span></span> <span data-ttu-id="36950-129">Már szükség Amikor távolról jelentkeznek toohello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="36950-129">You need them when you remotely sign in toohello virtual machine.</span></span>
   3. <span data-ttu-id="36950-130">Adjon meg egy jelszót hello **új jelszó** mezőben, majd adja meg újból hello **jelszó megerősítése** mező.</span><span class="sxs-lookup"><span data-stu-id="36950-130">Enter a password in hello **New password** field, and reenter it in hello **Confirm password** field.</span></span> <span data-ttu-id="36950-131">Ezt a jelszót a rendszergazdai fiók hello szolgál.</span><span class="sxs-lookup"><span data-stu-id="36950-131">This password is for hello Administrator account.</span></span>
   4. <span data-ttu-id="36950-132">Jelölje be hello megfelelő **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="36950-132">Select hello appropriate **Subscription**.</span></span>
   5. <span data-ttu-id="36950-133">A hello **erőforráscsoport**, kattintson a **hozzon létre új** és adja meg egy új erőforráscsoportot hello nevét.</span><span class="sxs-lookup"><span data-stu-id="36950-133">For hello **Resource group**, click **Create new** and enter hello name of a new resource group.</span></span> <span data-ttu-id="36950-134">Vagy kattintson a **meglévő** , és válassza ki az elérhető erőforráscsoportok hello.</span><span class="sxs-lookup"><span data-stu-id="36950-134">Or, click **Use existing** and select one of hello available resource groups.</span></span>
   6. <span data-ttu-id="36950-135">Jelöljön ki egy helyet, ahol hello virtuális gép található, mint például **déli középső Régiójában**.</span><span class="sxs-lookup"><span data-stu-id="36950-135">Select a location where hello virtual machine resides, such as **South Central US**.</span></span>
6. <span data-ttu-id="36950-136">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="36950-136">Click **Next**.</span></span>
7. <span data-ttu-id="36950-137">A hello **virtuálisgép-lemezkép mérete** panelen válassza **A1 szabványos** vagy egy másik megfelelő lemezképet.</span><span class="sxs-lookup"><span data-stu-id="36950-137">In hello **Virtual machine image size** blade, select **A1 Standard** or another appropriate image.</span></span>
8. <span data-ttu-id="36950-138">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="36950-138">Click **Select**.</span></span>

9. <span data-ttu-id="36950-139">A hello **beállítások** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="36950-139">In hello **Settings** blade, click **OK**.</span></span> <span data-ttu-id="36950-140">Azure által biztosított hello alapértelmezett értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="36950-140">You can use hello default values provided by Azure.</span></span>  
10. <span data-ttu-id="36950-141">A hello **összegzés** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="36950-141">In hello **Summary** blade, click **OK**.</span></span>

## <a name="tooremotely-sign-in-tooyour-virtual-machine"></a><span data-ttu-id="36950-142">virtuális gép tooyour tooremotely bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36950-142">tooremotely sign in tooyour virtual machine</span></span>
1. <span data-ttu-id="36950-143">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36950-143">Log on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="36950-144">Kattintson a **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="36950-144">Click **Virtual machines (classic)**.</span></span> <span data-ttu-id="36950-145">Ha szükséges, kattintson a **további szolgáltatások** : hello bal alsó sarokban a hello szolgáltatás kategóriák.</span><span class="sxs-lookup"><span data-stu-id="36950-145">If needed, click **More services** at hello bottom left corner under hello service categories.</span></span> <span data-ttu-id="36950-146">Hello **virtuális gépek (klasszikus)** bejegyzés szerepel hello **számítási** csoport.</span><span class="sxs-lookup"><span data-stu-id="36950-146">hello **Virtual machines (classic)** entry is listed in hello **Compute** group.</span></span>
3. <span data-ttu-id="36950-147">Kattintson a kívánt toosign a hello virtuális gép hello nevére.</span><span class="sxs-lookup"><span data-stu-id="36950-147">Click hello name of hello virtual machine that you want toosign in to.</span></span>
4. <span data-ttu-id="36950-148">Miután elindult hello virtuális gép, hello hello ablaktábla tetején a menüben kapcsolatait fogadja.</span><span class="sxs-lookup"><span data-stu-id="36950-148">After hello virtual machine has started, a menu at hello top of hello pane allows connections.</span></span>
5. <span data-ttu-id="36950-149">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="36950-149">Click **Connect**.</span></span>
6. <span data-ttu-id="36950-150">Válaszoljon toohello kérni fogja a szükséges tooconnect toohello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="36950-150">Respond toohello prompts as needed tooconnect toohello virtual machine.</span></span> <span data-ttu-id="36950-151">Általában megnyitásakor vagy mentésekor hello .rdp fájlt, amely hello kapcsolódási adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="36950-151">Typically, you save or open hello .rdp file that contains hello connection details.</span></span> <span data-ttu-id="36950-152">Előfordulhat, hogy rendelkezik toocopy hello URL-cím: port hello hello .rdp fájl első sorának hello utolsó része, és illessze be a távoli bejelentkezési alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="36950-152">You might have toocopy hello url:port as hello last part of hello first line of hello .rdp file and paste it in a remote sign-in application.</span></span>

## <a name="tooinstall-a-java-application-server-on-your-virtual-machine"></a><span data-ttu-id="36950-153">a virtuális gép Java-alkalmazáskiszolgáló tooinstall</span><span class="sxs-lookup"><span data-stu-id="36950-153">tooinstall a Java application server on your virtual machine</span></span>
<span data-ttu-id="36950-154">Másolhatja Java application server tooyour virtuális gépet, vagy a telepítő keresztül Java-alkalmazáskiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="36950-154">You can copy a Java application server tooyour virtual machine, or you can install a Java application server through an installer.</span></span>

<span data-ttu-id="36950-155">Ez az oktatóanyag hello Java application server tooinstall Tomcat használja.</span><span class="sxs-lookup"><span data-stu-id="36950-155">This tutorial uses Tomcat as hello Java application server tooinstall.</span></span>

1. <span data-ttu-id="36950-156">Ha be van jelentkezve tooyour virtuális gép, nyissa meg a böngésző-munkamenet túl[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span><span class="sxs-lookup"><span data-stu-id="36950-156">When you are signed in tooyour virtual machine, open a browser session too[Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span></span>
2. <span data-ttu-id="36950-157">Kattintson duplán az hello hivatkozásának **32-bites vagy 64 bites Windows Service telepítőjét**.</span><span class="sxs-lookup"><span data-stu-id="36950-157">Double-click hello link for **32-bit/64-bit Windows Service Installer**.</span></span> <span data-ttu-id="36950-158">Ez a módszer használatával Tomcat telepíti egy Windows-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="36950-158">By using this technique, Tomcat installs as a Windows service.</span></span>
3. <span data-ttu-id="36950-159">Amikor a rendszer kéri, válassza ki a toorun hello telepítő.</span><span class="sxs-lookup"><span data-stu-id="36950-159">When prompted, choose toorun hello installer.</span></span>
4. <span data-ttu-id="36950-160">Hello belül **Apache Tomcat telepítő** varázslóban kövesse hello tooinstall Tomcat kéri.</span><span class="sxs-lookup"><span data-stu-id="36950-160">Within hello **Apache Tomcat Setup** wizard, follow hello prompts tooinstall Tomcat.</span></span> <span data-ttu-id="36950-161">Ez az oktatóanyag hello céljából hello Alapértelmezések elfogadása rendben.</span><span class="sxs-lookup"><span data-stu-id="36950-161">For hello purposes of this tutorial, accepting hello defaults is fine.</span></span> <span data-ttu-id="36950-162">Amikor eléri a hello **Apache Tomcat telepítővarázsló befejezése hello** párbeszédpanelen opcionálisan ellenőrizheti a **futtassa: Apache Tomcat** toohave Tomcat start most.</span><span class="sxs-lookup"><span data-stu-id="36950-162">When you reach hello **Completing hello Apache Tomcat Setup Wizard** dialog box, you can optionally check **Run Apache Tomcat** toohave Tomcat start now.</span></span> <span data-ttu-id="36950-163">Kattintson a **Befejezés** toocomplete hello Tomcat telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="36950-163">Click **Finish** toocomplete hello Tomcat setup process.</span></span>

## <a name="toostart-tomcat"></a><span data-ttu-id="36950-164">toostart Tomcat</span><span class="sxs-lookup"><span data-stu-id="36950-164">toostart Tomcat</span></span>

<span data-ttu-id="36950-165">Nyissa meg a virtuális gép és a futó hello parancsot a parancssorba manuálisan is kezdeményezhető Tomcat **nettó&nbsp;start&nbsp;Tomcat8**.</span><span class="sxs-lookup"><span data-stu-id="36950-165">You can manually start Tomcat by opening a command prompt on your virtual machine and running hello command **net&nbsp;start&nbsp;Tomcat8**.</span></span>

<span data-ttu-id="36950-166">Miután Tomcat fut, elérheti hello URL-cím megadásával Tomcat <8080> hello virtuális gép böngészőben.</span><span class="sxs-lookup"><span data-stu-id="36950-166">Once Tomcat is running, you can access Tomcat by entering hello URL <http://localhost:8080> in hello virtual machine's browser.</span></span>

<span data-ttu-id="36950-167">toosee Tomcat külső gépeken futtatja, akkor toocreate egy olyan végpont szükséges, és nyissa meg egy portot.</span><span class="sxs-lookup"><span data-stu-id="36950-167">toosee Tomcat running from external machines, you need toocreate an endpoint and open a port.</span></span>

## <a name="toocreate-an-endpoint-for-your-virtual-machine"></a><span data-ttu-id="36950-168">a virtuális gép végpont toocreate</span><span class="sxs-lookup"><span data-stu-id="36950-168">toocreate an endpoint for your virtual machine</span></span>
1. <span data-ttu-id="36950-169">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="36950-169">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="36950-170">Kattintson a **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="36950-170">Click **Virtual machines (classic)**.</span></span>
3. <span data-ttu-id="36950-171">Kattintson a hello hello virtuális gép nevét, a Java-alkalmazáskiszolgáló futtató.</span><span class="sxs-lookup"><span data-stu-id="36950-171">Click hello name of hello virtual machine that is running your Java application server.</span></span>
4. <span data-ttu-id="36950-172">Kattintson a **Végpontok** elemre.</span><span class="sxs-lookup"><span data-stu-id="36950-172">Click **Endpoints**.</span></span>
5. <span data-ttu-id="36950-173">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="36950-173">Click **Add**.</span></span>
6. <span data-ttu-id="36950-174">A hello **végpont hozzáadása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="36950-174">In hello **Add endpoint** dialog box:</span></span>
   1. <span data-ttu-id="36950-175">Adjon meg egy nevet a hello végpont; például **HttpIn**.</span><span class="sxs-lookup"><span data-stu-id="36950-175">Specify a name for hello endpoint; for example, **HttpIn**.</span></span>
   2. <span data-ttu-id="36950-176">Válassza ki **TCP** hello protokollhoz.</span><span class="sxs-lookup"><span data-stu-id="36950-176">Select **TCP** for hello protocol.</span></span>
   3. <span data-ttu-id="36950-177">Adja meg **80** hello nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="36950-177">Specify **80** for hello public port.</span></span>
   4. <span data-ttu-id="36950-178">Adja meg **8080** hello magánhálózati port.</span><span class="sxs-lookup"><span data-stu-id="36950-178">Specify **8080** for hello private port.</span></span>
   5. <span data-ttu-id="36950-179">Válassza ki **letiltott** hello fix IP-cím számára.</span><span class="sxs-lookup"><span data-stu-id="36950-179">Select **Disabled** for hello floating IP address.</span></span>
   6. <span data-ttu-id="36950-180">Hello hozzáférés-vezérlési lista, hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="36950-180">Leave hello access control list as is.</span></span>
   7. <span data-ttu-id="36950-181">Kattintson a hello **OK** tooclose hello párbeszédpanel gombra, és hello végpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="36950-181">Click hello **OK** button tooclose hello dialog box and create hello endpoint.</span></span>

## <a name="tooopen-a-port-in-hello-firewall-for-your-virtual-machine"></a><span data-ttu-id="36950-182">a virtuális gép hello tűzfal port tooopen</span><span class="sxs-lookup"><span data-stu-id="36950-182">tooopen a port in hello firewall for your virtual machine</span></span>
1. <span data-ttu-id="36950-183">Tooyour virtuálisgép jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="36950-183">Sign in tooyour virtual machine.</span></span>
2. <span data-ttu-id="36950-184">Kattintson a **Windows Start**.</span><span class="sxs-lookup"><span data-stu-id="36950-184">Click **Windows Start**.</span></span>
3. <span data-ttu-id="36950-185">Kattintson a **vezérlőpultot**.</span><span class="sxs-lookup"><span data-stu-id="36950-185">Click **Control Panel**.</span></span>
4. <span data-ttu-id="36950-186">Kattintson a **rendszer és biztonság**, kattintson a **Windows tűzfal**, és kattintson a **speciális beállítások**.</span><span class="sxs-lookup"><span data-stu-id="36950-186">Click **System and Security**, click **Windows Firewall**, and then click **Advanced Settings**.</span></span>
5. <span data-ttu-id="36950-187">Kattintson a **bejövő szabályok**, és kattintson a **új szabály**.</span><span class="sxs-lookup"><span data-stu-id="36950-187">Click **Inbound Rules**, and then click **New Rule**.</span></span>  
   <span data-ttu-id="36950-188">![Új bejövő szabály][NewIBRule]</span><span class="sxs-lookup"><span data-stu-id="36950-188">![New inbound rule][NewIBRule]</span></span>
6. <span data-ttu-id="36950-189">A hello **szabálytípus**, jelölje be **Port**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="36950-189">For hello **Rule Type**, select **Port**, and then click **Next**.</span></span>  
   <span data-ttu-id="36950-190">![Új bejövő szabály port][NewRulePort]</span><span class="sxs-lookup"><span data-stu-id="36950-190">![New inbound rule port][NewRulePort]</span></span>
7. <span data-ttu-id="36950-191">A hello **protokoll és portok** képernyőn válassza ki **TCP**, adja meg **8080-as** hello, **adott helyi port**, majd kattintson az **Következő**.</span><span class="sxs-lookup"><span data-stu-id="36950-191">On hello **Protocol and Ports** screen, select **TCP**, specify **8080** as hello **Specific local port**, and then click **Next**.</span></span>  
  <span data-ttu-id="36950-192">![Új bejövő szabály][NewRuleProtocol]</span><span class="sxs-lookup"><span data-stu-id="36950-192">![New inbound rule ][NewRuleProtocol]</span></span>
8. <span data-ttu-id="36950-193">A hello **művelet** képernyőn válassza ki **hello csatlakozás engedélyezése**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="36950-193">On hello **Action** screen, select **Allow hello connection**, and then click **Next**.</span></span>
   <span data-ttu-id="36950-194">![Új bejövő szabály művelet][NewRuleAction]</span><span class="sxs-lookup"><span data-stu-id="36950-194">![New inbound rule action][NewRuleAction]</span></span>
9. <span data-ttu-id="36950-195">A hello **profil** képernyőn **tartomány**, **titkos**, és **nyilvános** van kiválasztva, és kattintson **tovább** .</span><span class="sxs-lookup"><span data-stu-id="36950-195">On hello **Profile** screen, ensure that **Domain**, **Private**, and **Public** are selected, and then click **Next**.</span></span>
   <span data-ttu-id="36950-196">![Új bejövő szabály profil][NewRuleProfile]</span><span class="sxs-lookup"><span data-stu-id="36950-196">![New inbound rule profile][NewRuleProfile]</span></span>
10. <span data-ttu-id="36950-197">A hello **neve** képernyőn, adjon meg egy nevet hello szabályhoz, például **HttpIn** (hello szabály neve nincs szükség toomatch hello végpont nevét, azonban), és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="36950-197">On hello **Name** screen, specify a name for hello rule, such as **HttpIn** (hello rule name is not required toomatch hello endpoint name, however), and then click **Finish**.</span></span>  
    <span data-ttu-id="36950-198">![Új bejövő szabály neve][NewRuleName]</span><span class="sxs-lookup"><span data-stu-id="36950-198">![New inbound rule name][NewRuleName]</span></span>

<span data-ttu-id="36950-199">Ezen a ponton a Tomcat webhely egy külső böngészőből megtekinthető legyen.</span><span class="sxs-lookup"><span data-stu-id="36950-199">At this point, your Tomcat website should be viewable from an external browser.</span></span> <span data-ttu-id="36950-200">A böngésző hello cím ablakban írja be a hello URL-cím  **http://*a\_DNS\_neve*. cloudapp.net**, ahol ***a\_DNS\_neve*** hello virtuális gép létrehozásakor megadott hello DNS-neve.</span><span class="sxs-lookup"><span data-stu-id="36950-200">In hello browser's address window, type a URL of hello form **http://*your\_DNS\_name*.cloudapp.net**, where ***your\_DNS\_name*** is hello DNS name you specified when you created hello virtual machine.</span></span>

## <a name="application-lifecycle-considerations"></a><span data-ttu-id="36950-201">Alkalmazás életciklusa kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="36950-201">Application lifecycle considerations</span></span>
* <span data-ttu-id="36950-202">Nem sikerült létrehozni a saját webalkalmazások archívumából (WAR) és toohello adja hozzá **webapps** mappa.</span><span class="sxs-lookup"><span data-stu-id="36950-202">You could create your own web application archive (WAR) and add it toohello **webapps** folder.</span></span> <span data-ttu-id="36950-203">Például egy alapszintű Java szolgáltatás lap (JSP) dinamikus webes projekt létrehozása és exportálni kell a WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="36950-203">For example, create a basic Java Service Page (JSP) dynamic web project and export it as a WAR file.</span></span> <span data-ttu-id="36950-204">Ezután másolja hello WAR toohello Apache Tomcat **webapps** mappa hello virtuális gépen, majd futtassa a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="36950-204">Next, copy hello WAR toohello Apache Tomcat **webapps** folder on hello virtual machine, then run it in a browser.</span></span>
* <span data-ttu-id="36950-205">Alapértelmezés szerint hello Tomcat-szolgáltatás telepítve van, ha van állítva, akkor toostart manuálisan.</span><span class="sxs-lookup"><span data-stu-id="36950-205">By default when hello Tomcat service is installed, it is set toostart manually.</span></span> <span data-ttu-id="36950-206">Megváltoztathatja azt toostart automatikusan hello szolgáltatások beépülő modul használatával.</span><span class="sxs-lookup"><span data-stu-id="36950-206">You can switch it toostart automatically by using hello Services snap-in.</span></span> <span data-ttu-id="36950-207">Hello szolgáltatások beépülő modul indításához kattintson **Windows Start**, **felügyeleti eszközök**, majd **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="36950-207">Start hello Services snap-in by clicking **Windows Start**, **Administrative Tools**, and then **Services**.</span></span> <span data-ttu-id="36950-208">Kattintson duplán a hello **Apache Tomcat** szolgáltatást, és állítsa **indítási típus** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="36950-208">Double-click hello **Apache Tomcat** service and set **Startup type** too**Automatic**.</span></span>

    ![A szolgáltatás toostart beállítása automatikusan][service_automatic_startup]

    <span data-ttu-id="36950-210">Hello, hogy automatikusan elinduljon Tomcat előnye, hogy futásának indításakor hello virtuális gép újraindításakor, (például a számítógép újraindítása szükséges szoftverfrissítések telepítése után).</span><span class="sxs-lookup"><span data-stu-id="36950-210">hello benefit of having Tomcat start automatically is that it starts running when hello virtual machine is rebooted (for example, after software updates that require a reboot are installed).</span></span>

## <a name="next-steps"></a><span data-ttu-id="36950-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36950-211">Next steps</span></span>
<span data-ttu-id="36950-212">Érdemes lehet egyéb szolgáltatások (például az Azure Storage, service bus és SQL-adatbázis) megismerheti a Java-alkalmazások tooinclude.</span><span class="sxs-lookup"><span data-stu-id="36950-212">You can learn about other services (such as Azure Storage, service bus, and SQL Database) that you may want tooinclude with your Java applications.</span></span> <span data-ttu-id="36950-213">Rendelkezésre álló hello információk megtekintéséhez hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="36950-213">View hello information available at hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from hello "toocreate an ednpoint for your virtual mache" 3/17/2017,
     toouse hello new portal.
6. In hello **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In hello **New endpoint details** dialog box:
-->
