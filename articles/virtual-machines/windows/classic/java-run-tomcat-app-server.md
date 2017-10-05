---
title: "A klasszikus Azure virtuális gép futtatásához a Java-alkalmazáskiszolgáló |} Microsoft Docs"
description: "Ez az oktatóanyag a klasszikus üzembe helyezési modellel létrehozott erőforrást használ, és bemutatja, hogyan hozzon létre egy Windows virtuális gépet, és konfigurálja úgy, hogy az Apache Tomcat alkalmazáskiszolgáló futtatásához."
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
ms.openlocfilehash: 6e02f42613808bcb13c0057e9f8fcc1c02273e77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="04d98-103">Javás alkalmazáskiszolgáló futtatása hagyományos módon üzembe helyezett virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="04d98-103">How to run a Java application server on a virtual machine created with the classic deployment model</span></span>
> [!IMPORTANT]
> <span data-ttu-id="04d98-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="04d98-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="04d98-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="04d98-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="04d98-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="04d98-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="04d98-107">A Resource Manager-sablon egy Java 8 és Tomcat webalkalmazás telepítése, lásd: [Itt](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span><span class="sxs-lookup"><span data-stu-id="04d98-107">For a Resource Manager template to deploy a webapp with Java 8 and Tomcat, see [here](https://azure.microsoft.com/documentation/templates/201-web-app-java-tomcat/).</span></span>

<span data-ttu-id="04d98-108">Az Azure-egy virtuális gép segítségével adja meg a kiszolgálói szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="04d98-108">With Azure, you can use a virtual machine to provide server capabilities.</span></span> <span data-ttu-id="04d98-109">Például egy Azure-on futó virtuális gép beállítható úgy, hogy egy Java-alkalmazáskiszolgáló, például az Apache Tomcat üzemeltetésére.</span><span class="sxs-lookup"><span data-stu-id="04d98-109">As an example, a virtual machine running on Azure can be configured to host a Java application server, such as Apache Tomcat.</span></span>

<span data-ttu-id="04d98-110">Ez az útmutató befejezése után fog Azure-on futó virtuális gép létrehozása, és konfigurálja úgy, hogy futtassa egy Java-alkalmazáskiszolgáló ismeretét.</span><span class="sxs-lookup"><span data-stu-id="04d98-110">After completing this guide, you will have an understanding of how to create a virtual machine running on Azure and configure it to run a Java application server.</span></span> <span data-ttu-id="04d98-111">Először ismerje meg, a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="04d98-111">You will learn and perform the following tasks:</span></span>

* <span data-ttu-id="04d98-112">Megtudhatja, hogyan hozzon létre egy virtuális gépet, amely egy Java fejlesztői készlet (JDK) már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="04d98-112">How to create a virtual machine that has a Java Development Kit (JDK) already installed.</span></span>
* <span data-ttu-id="04d98-113">Távolról jelentkeznek be a virtuális géphez tudnivalók.</span><span class="sxs-lookup"><span data-stu-id="04d98-113">How to remotely sign in to your virtual machine.</span></span>
* <span data-ttu-id="04d98-114">Hogyan telepíthet egy Java-alkalmazáskiszolgáló--Apache Tomcat – a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="04d98-114">How to install a Java application server--Apache Tomcat--on your virtual machine.</span></span>
* <span data-ttu-id="04d98-115">Hozzon létre egy végpontot a virtuális géphez tudnivalók.</span><span class="sxs-lookup"><span data-stu-id="04d98-115">How to create an endpoint for your virtual machine.</span></span>
* <span data-ttu-id="04d98-116">Nyisson meg egy portot a tűzfalon az alkalmazáskiszolgáló módjáról.</span><span class="sxs-lookup"><span data-stu-id="04d98-116">How to open a port in the firewall for your application server.</span></span>

<span data-ttu-id="04d98-117">A virtuális gépen futó Tomcat telepítése befejeződött eredményez.</span><span class="sxs-lookup"><span data-stu-id="04d98-117">The completed installation results in Tomcat running on a virtual machine.</span></span>

![Apache Tomcat rendszerű virtuális gép][virtual_machine_tomcat]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a><span data-ttu-id="04d98-119">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="04d98-119">To create a virtual machine</span></span>
1. <span data-ttu-id="04d98-120">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="04d98-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="04d98-121">Kattintson a **új**, kattintson a **számítási**, majd kattintson a **láthatja az összes** a a **a kiemelt alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="04d98-121">Click **New**, click **Compute**, then click **See all** in the **Featured apps**.</span></span>
3. <span data-ttu-id="04d98-122">Kattintson a **JDK**, kattintson a **JDK 8** a a **JDK** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="04d98-122">Click **JDK**, click **JDK 8** in the **JDK** pane.</span></span>  
   <span data-ttu-id="04d98-123">Virtuálisgép-lemezképek támogató **JDK 6** és **JDK 7** érhető el, ha örökölt alkalmazásokat, amelyek nem kész a JDK 8-ban.</span><span class="sxs-lookup"><span data-stu-id="04d98-123">Virtual machine images that support **JDK 6** and **JDK 7** are available if you have legacy applications that are not ready to run in JDK 8.</span></span>
4. <span data-ttu-id="04d98-124">A JDK 8 ablaktábla, válassza a **klasszikus**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="04d98-124">In the JDK 8 pane, select **Classic**, then click **Create**.</span></span>
5. <span data-ttu-id="04d98-125">Az a **alapjai** panel:</span><span class="sxs-lookup"><span data-stu-id="04d98-125">In the **Basics** blade:</span></span>
   1. <span data-ttu-id="04d98-126">Adja meg a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="04d98-126">Specify a name for the virtual machine.</span></span>
   2. <span data-ttu-id="04d98-127">Adja meg a rendszergazdát a nevét a **felhasználónév** mező.</span><span class="sxs-lookup"><span data-stu-id="04d98-127">Enter a name for the administrator in the **User Name** field.</span></span> <span data-ttu-id="04d98-128">Ne felejtse el ezt a nevet és egy ahhoz tartozó jelszót, amely a következő mezőben a következő.</span><span class="sxs-lookup"><span data-stu-id="04d98-128">Remember this name and the associated password that follows in the next field.</span></span> <span data-ttu-id="04d98-129">Már szükség Amikor távolról jelentkeznek be a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="04d98-129">You need them when you remotely sign in to the virtual machine.</span></span>
   3. <span data-ttu-id="04d98-130">Adja meg egy jelszót a **új jelszó** mezőben, majd még egyszer a **jelszó megerősítése** mező.</span><span class="sxs-lookup"><span data-stu-id="04d98-130">Enter a password in the **New password** field, and reenter it in the **Confirm password** field.</span></span> <span data-ttu-id="04d98-131">Ezt a jelszót a rendszergazdai fiók van.</span><span class="sxs-lookup"><span data-stu-id="04d98-131">This password is for the Administrator account.</span></span>
   4. <span data-ttu-id="04d98-132">Válassza ki a megfelelő **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="04d98-132">Select the appropriate **Subscription**.</span></span>
   5. <span data-ttu-id="04d98-133">Az a **erőforráscsoport**, kattintson a **hozzon létre új** , és írja be az új erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="04d98-133">For the **Resource group**, click **Create new** and enter the name of a new resource group.</span></span> <span data-ttu-id="04d98-134">Vagy kattintson a **meglévő** , és válassza ki az elérhető erőforráscsoportok.</span><span class="sxs-lookup"><span data-stu-id="04d98-134">Or, click **Use existing** and select one of the available resource groups.</span></span>
   6. <span data-ttu-id="04d98-135">Jelöljön ki egy helyet, ahol a virtuális gép található, mint például **déli középső Régiójában**.</span><span class="sxs-lookup"><span data-stu-id="04d98-135">Select a location where the virtual machine resides, such as **South Central US**.</span></span>
6. <span data-ttu-id="04d98-136">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="04d98-136">Click **Next**.</span></span>
7. <span data-ttu-id="04d98-137">Az a **virtuálisgép-lemezkép mérete** panelen válassza **A1 szabványos** vagy egy másik megfelelő lemezképet.</span><span class="sxs-lookup"><span data-stu-id="04d98-137">In the **Virtual machine image size** blade, select **A1 Standard** or another appropriate image.</span></span>
8. <span data-ttu-id="04d98-138">Kattintson a **Kiválasztás** gombra.</span><span class="sxs-lookup"><span data-stu-id="04d98-138">Click **Select**.</span></span>

9. <span data-ttu-id="04d98-139">Az a **beállítások** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="04d98-139">In the **Settings** blade, click **OK**.</span></span> <span data-ttu-id="04d98-140">Az Azure által biztosított alapértelmezett értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="04d98-140">You can use the default values provided by Azure.</span></span>  
10. <span data-ttu-id="04d98-141">Az a **összegzés** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="04d98-141">In the **Summary** blade, click **OK**.</span></span>

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a><span data-ttu-id="04d98-142">A távolról jelentkeznek be a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="04d98-142">To remotely sign in to your virtual machine</span></span>
1. <span data-ttu-id="04d98-143">Jelentkezzen be az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="04d98-143">Log on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="04d98-144">Kattintson a **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="04d98-144">Click **Virtual machines (classic)**.</span></span> <span data-ttu-id="04d98-145">Ha szükséges, kattintson a **további szolgáltatások** , a szolgáltatás kategóriák bal alsó sarokban.</span><span class="sxs-lookup"><span data-stu-id="04d98-145">If needed, click **More services** at the bottom left corner under the service categories.</span></span> <span data-ttu-id="04d98-146">A **virtuális gépek (klasszikus)** bejegyzés szerepel-e a **számítási** csoport.</span><span class="sxs-lookup"><span data-stu-id="04d98-146">The **Virtual machines (classic)** entry is listed in the **Compute** group.</span></span>
3. <span data-ttu-id="04d98-147">Kattintson a nevére, a virtuális gép, amelyet szeretne bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="04d98-147">Click the name of the virtual machine that you want to sign in to.</span></span>
4. <span data-ttu-id="04d98-148">Miután a virtuális gép elindult, a menü, ha a panel tetején engedélyezi a csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="04d98-148">After the virtual machine has started, a menu at the top of the pane allows connections.</span></span>
5. <span data-ttu-id="04d98-149">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="04d98-149">Click **Connect**.</span></span>
6. <span data-ttu-id="04d98-150">Szükség esetén a virtuális géphez történő csatlakozáshoz, kövesse a megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="04d98-150">Respond to the prompts as needed to connect to the virtual machine.</span></span> <span data-ttu-id="04d98-151">Általában mentse, vagy nyissa meg a .rdp fájlt, amely tartalmazza a kapcsolat adatai.</span><span class="sxs-lookup"><span data-stu-id="04d98-151">Typically, you save or open the .rdp file that contains the connection details.</span></span> <span data-ttu-id="04d98-152">Lehetséges, hogy az URL-cím: port másolása az .rdp fájl első sorának utolsó részeként, és illessze be a távoli bejelentkezési alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="04d98-152">You might have to copy the url:port as the last part of the first line of the .rdp file and paste it in a remote sign-in application.</span></span>

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a><span data-ttu-id="04d98-153">A Java-alkalmazáskiszolgáló telepítése a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="04d98-153">To install a Java application server on your virtual machine</span></span>
<span data-ttu-id="04d98-154">Egy Java-alkalmazáskiszolgáló másolhatja a virtuális gép, vagy telepítheti egy Java-alkalmazáskiszolgáló telepítő keresztül.</span><span class="sxs-lookup"><span data-stu-id="04d98-154">You can copy a Java application server to your virtual machine, or you can install a Java application server through an installer.</span></span>

<span data-ttu-id="04d98-155">Ez az oktatóanyag Tomcat használja, a Java-alkalmazáskiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="04d98-155">This tutorial uses Tomcat as the Java application server to install.</span></span>

1. <span data-ttu-id="04d98-156">Ha be van jelentkezve a virtuális gép, nyissa meg a böngésző-munkamenet az [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span><span class="sxs-lookup"><span data-stu-id="04d98-156">When you are signed in to your virtual machine, open a browser session to [Apache Tomcat](http://tomcat.apache.org/download-80.cgi).</span></span>
2. <span data-ttu-id="04d98-157">Kattintson a hivatkozásra duplán **32-bites vagy 64 bites Windows Service telepítőjét**.</span><span class="sxs-lookup"><span data-stu-id="04d98-157">Double-click the link for **32-bit/64-bit Windows Service Installer**.</span></span> <span data-ttu-id="04d98-158">Ez a módszer használatával Tomcat telepíti egy Windows-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="04d98-158">By using this technique, Tomcat installs as a Windows service.</span></span>
3. <span data-ttu-id="04d98-159">Amikor a rendszer kéri, kattintson a telepítő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="04d98-159">When prompted, choose to run the installer.</span></span>
4. <span data-ttu-id="04d98-160">Belül a **Apache Tomcat telepítő** varázsló, kövesse a megjelenő utasításokat Tomcat telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="04d98-160">Within the **Apache Tomcat Setup** wizard, follow the prompts to install Tomcat.</span></span> <span data-ttu-id="04d98-161">Ez az oktatóanyag céljából az alapértelmezett értékek elfogadásával rendben.</span><span class="sxs-lookup"><span data-stu-id="04d98-161">For the purposes of this tutorial, accepting the defaults is fine.</span></span> <span data-ttu-id="04d98-162">Amikor eléri a **Apache Tomcat telepítővarázslójának befejezése** párbeszédpanelen opcionálisan ellenőrizheti a **futtassa: Apache Tomcat** Tomcat most rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="04d98-162">When you reach the **Completing the Apache Tomcat Setup Wizard** dialog box, you can optionally check **Run Apache Tomcat** to have Tomcat start now.</span></span> <span data-ttu-id="04d98-163">Kattintson a **Befejezés** a Tomcat telepítési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="04d98-163">Click **Finish** to complete the Tomcat setup process.</span></span>

## <a name="to-start-tomcat"></a><span data-ttu-id="04d98-164">Tomcat elejéig</span><span class="sxs-lookup"><span data-stu-id="04d98-164">To start Tomcat</span></span>

<span data-ttu-id="04d98-165">Nyisson meg egy parancssort, a virtuális gépen, és futtassa a parancsot manuálisan is kezdeményezhető Tomcat **nettó&nbsp;start&nbsp;Tomcat8**.</span><span class="sxs-lookup"><span data-stu-id="04d98-165">You can manually start Tomcat by opening a command prompt on your virtual machine and running the command **net&nbsp;start&nbsp;Tomcat8**.</span></span>

<span data-ttu-id="04d98-166">Ha Tomcat fut, hozzáférhet az URL-cím megadásával Tomcat <8080> a virtuális gép böngészőben.</span><span class="sxs-lookup"><span data-stu-id="04d98-166">Once Tomcat is running, you can access Tomcat by entering the URL <http://localhost:8080> in the virtual machine's browser.</span></span>

<span data-ttu-id="04d98-167">A külső gépekről futtató Tomcat megtekintéséhez szükség hozzon létre egy végpontot, és nyisson meg egy portot.</span><span class="sxs-lookup"><span data-stu-id="04d98-167">To see Tomcat running from external machines, you need to create an endpoint and open a port.</span></span>

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a><span data-ttu-id="04d98-168">A végpont a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="04d98-168">To create an endpoint for your virtual machine</span></span>
1. <span data-ttu-id="04d98-169">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="04d98-169">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="04d98-170">Kattintson a **virtuális gépek (klasszikus)**.</span><span class="sxs-lookup"><span data-stu-id="04d98-170">Click **Virtual machines (classic)**.</span></span>
3. <span data-ttu-id="04d98-171">Kattintson a virtuális gépet, hogy fut a Java-alkalmazáskiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="04d98-171">Click the name of the virtual machine that is running your Java application server.</span></span>
4. <span data-ttu-id="04d98-172">Kattintson a **Végpontok** elemre.</span><span class="sxs-lookup"><span data-stu-id="04d98-172">Click **Endpoints**.</span></span>
5. <span data-ttu-id="04d98-173">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="04d98-173">Click **Add**.</span></span>
6. <span data-ttu-id="04d98-174">Az a **végpont hozzáadása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="04d98-174">In the **Add endpoint** dialog box:</span></span>
   1. <span data-ttu-id="04d98-175">Adjon meg egy nevet a végpont; például **HttpIn**.</span><span class="sxs-lookup"><span data-stu-id="04d98-175">Specify a name for the endpoint; for example, **HttpIn**.</span></span>
   2. <span data-ttu-id="04d98-176">Válassza ki **TCP** a protokoll.</span><span class="sxs-lookup"><span data-stu-id="04d98-176">Select **TCP** for the protocol.</span></span>
   3. <span data-ttu-id="04d98-177">Adja meg **80** a nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="04d98-177">Specify **80** for the public port.</span></span>
   4. <span data-ttu-id="04d98-178">Adja meg **8080** a magánhálózati port.</span><span class="sxs-lookup"><span data-stu-id="04d98-178">Specify **8080** for the private port.</span></span>
   5. <span data-ttu-id="04d98-179">Válassza ki **letiltott** a fix IP-cím.</span><span class="sxs-lookup"><span data-stu-id="04d98-179">Select **Disabled** for the floating IP address.</span></span>
   6. <span data-ttu-id="04d98-180">A hozzáférés-vezérlési lista, hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="04d98-180">Leave the access control list as is.</span></span>
   7. <span data-ttu-id="04d98-181">Kattintson a **OK** gombra kattintva zárja be a párbeszédpanelt, és a végpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="04d98-181">Click the **OK** button to close the dialog box and create the endpoint.</span></span>

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a><span data-ttu-id="04d98-182">Nyisson meg egy portot a tűzfalon a virtuális gép számára</span><span class="sxs-lookup"><span data-stu-id="04d98-182">To open a port in the firewall for your virtual machine</span></span>
1. <span data-ttu-id="04d98-183">Jelentkezzen be a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="04d98-183">Sign in to your virtual machine.</span></span>
2. <span data-ttu-id="04d98-184">Kattintson a **Windows Start**.</span><span class="sxs-lookup"><span data-stu-id="04d98-184">Click **Windows Start**.</span></span>
3. <span data-ttu-id="04d98-185">Kattintson a **vezérlőpultot**.</span><span class="sxs-lookup"><span data-stu-id="04d98-185">Click **Control Panel**.</span></span>
4. <span data-ttu-id="04d98-186">Kattintson a **rendszer és biztonság**, kattintson a **Windows tűzfal**, és kattintson a **speciális beállítások**.</span><span class="sxs-lookup"><span data-stu-id="04d98-186">Click **System and Security**, click **Windows Firewall**, and then click **Advanced Settings**.</span></span>
5. <span data-ttu-id="04d98-187">Kattintson a **bejövő szabályok**, és kattintson a **új szabály**.</span><span class="sxs-lookup"><span data-stu-id="04d98-187">Click **Inbound Rules**, and then click **New Rule**.</span></span>  
   <span data-ttu-id="04d98-188">![Új bejövő szabály][NewIBRule]</span><span class="sxs-lookup"><span data-stu-id="04d98-188">![New inbound rule][NewIBRule]</span></span>
6. <span data-ttu-id="04d98-189">Az a **szabálytípus**, jelölje be **Port**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="04d98-189">For the **Rule Type**, select **Port**, and then click **Next**.</span></span>  
   <span data-ttu-id="04d98-190">![Új bejövő szabály port][NewRulePort]</span><span class="sxs-lookup"><span data-stu-id="04d98-190">![New inbound rule port][NewRulePort]</span></span>
7. <span data-ttu-id="04d98-191">Az a **protokoll és portok** képernyőn válassza ki **TCP**, adja meg **8080** , a **adott helyi port**, és kattintson a  **Következő**.</span><span class="sxs-lookup"><span data-stu-id="04d98-191">On the **Protocol and Ports** screen, select **TCP**, specify **8080** as the **Specific local port**, and then click **Next**.</span></span>  
  <span data-ttu-id="04d98-192">![Új bejövő szabály][NewRuleProtocol]</span><span class="sxs-lookup"><span data-stu-id="04d98-192">![New inbound rule ][NewRuleProtocol]</span></span>
8. <span data-ttu-id="04d98-193">Az a **művelet** képernyőn válassza ki **a kapcsolat engedélyezéséhez**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="04d98-193">On the **Action** screen, select **Allow the connection**, and then click **Next**.</span></span>
   <span data-ttu-id="04d98-194">![Új bejövő szabály művelet][NewRuleAction]</span><span class="sxs-lookup"><span data-stu-id="04d98-194">![New inbound rule action][NewRuleAction]</span></span>
9. <span data-ttu-id="04d98-195">Az a **profil** képernyőn **tartomány**, **titkos**, és **nyilvános** van kiválasztva, és kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="04d98-195">On the **Profile** screen, ensure that **Domain**, **Private**, and **Public** are selected, and then click **Next**.</span></span>
   <span data-ttu-id="04d98-196">![Új bejövő szabály profil][NewRuleProfile]</span><span class="sxs-lookup"><span data-stu-id="04d98-196">![New inbound rule profile][NewRuleProfile]</span></span>
10. <span data-ttu-id="04d98-197">Az a **neve** képernyőn, adja meg a szabály nevét, például a **HttpIn** (a szabály neve nem szükséges azonban felel meg a végpont neve), és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="04d98-197">On the **Name** screen, specify a name for the rule, such as **HttpIn** (the rule name is not required to match the endpoint name, however), and then click **Finish**.</span></span>  
    <span data-ttu-id="04d98-198">![Új bejövő szabály neve][NewRuleName]</span><span class="sxs-lookup"><span data-stu-id="04d98-198">![New inbound rule name][NewRuleName]</span></span>

<span data-ttu-id="04d98-199">Ezen a ponton a Tomcat webhely egy külső böngészőből megtekinthető legyen.</span><span class="sxs-lookup"><span data-stu-id="04d98-199">At this point, your Tomcat website should be viewable from an external browser.</span></span> <span data-ttu-id="04d98-200">A webböngésző cím ablakban írja be az URL-cím a  **http://*a\_DNS\_neve*. cloudapp.net**, ahol ***a\_DNS\_neve*** a virtuális gép létrehozásakor adott meg DNS-neve.</span><span class="sxs-lookup"><span data-stu-id="04d98-200">In the browser's address window, type a URL of the form **http://*your\_DNS\_name*.cloudapp.net**, where ***your\_DNS\_name*** is the DNS name you specified when you created the virtual machine.</span></span>

## <a name="application-lifecycle-considerations"></a><span data-ttu-id="04d98-201">Alkalmazás életciklusa kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="04d98-201">Application lifecycle considerations</span></span>
* <span data-ttu-id="04d98-202">Nem sikerült létrehozni a saját webalkalmazások archívumából (WAR), és hozzá kell adnia a **webapps** mappa.</span><span class="sxs-lookup"><span data-stu-id="04d98-202">You could create your own web application archive (WAR) and add it to the **webapps** folder.</span></span> <span data-ttu-id="04d98-203">Például egy alapszintű Java szolgáltatás lap (JSP) dinamikus webes projekt létrehozása és exportálni kell a WAR-fájlt.</span><span class="sxs-lookup"><span data-stu-id="04d98-203">For example, create a basic Java Service Page (JSP) dynamic web project and export it as a WAR file.</span></span> <span data-ttu-id="04d98-204">Ezután másolja a WAR az Apache Tomcat **webapps** mappa a virtuális gépen, majd futtassa a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="04d98-204">Next, copy the WAR to the Apache Tomcat **webapps** folder on the virtual machine, then run it in a browser.</span></span>
* <span data-ttu-id="04d98-205">Alapértelmezés szerint a Tomcat-szolgáltatás telepítve van, ha van állítva, akkor kézi elindításához.</span><span class="sxs-lookup"><span data-stu-id="04d98-205">By default when the Tomcat service is installed, it is set to start manually.</span></span> <span data-ttu-id="04d98-206">A szolgáltatások beépülő modul használatával automatikus indításra válthat.</span><span class="sxs-lookup"><span data-stu-id="04d98-206">You can switch it to start automatically by using the Services snap-in.</span></span> <span data-ttu-id="04d98-207">A szolgáltatások beépülő modul indításához kattintson **Windows Start**, **felügyeleti eszközök**, majd **szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="04d98-207">Start the Services snap-in by clicking **Windows Start**, **Administrative Tools**, and then **Services**.</span></span> <span data-ttu-id="04d98-208">Kattintson duplán a **Apache Tomcat** szolgáltatást, és állítsa **indítási típus** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="04d98-208">Double-click the **Apache Tomcat** service and set **Startup type** to **Automatic**.</span></span>

    ![A szolgáltatás automatikus indításra beállítása][service_automatic_startup]

    <span data-ttu-id="04d98-210">Az, hogy automatikusan elinduljon Tomcat előnye, hogy kezdődik futtatásakor, amikor a virtuális gép újraindítása után (például a számítógép újraindítása szükséges szoftverfrissítések telepítése után).</span><span class="sxs-lookup"><span data-stu-id="04d98-210">The benefit of having Tomcat start automatically is that it starts running when the virtual machine is rebooted (for example, after software updates that require a reboot are installed).</span></span>

## <a name="next-steps"></a><span data-ttu-id="04d98-211">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="04d98-211">Next steps</span></span>
<span data-ttu-id="04d98-212">Érdemes lehet felvenni a Java-alkalmazások és egyéb szolgáltatások (például az Azure Storage, service bus és SQL-adatbázis) olvashat.</span><span class="sxs-lookup"><span data-stu-id="04d98-212">You can learn about other services (such as Azure Storage, service bus, and SQL Database) that you may want to include with your Java applications.</span></span> <span data-ttu-id="04d98-213">A rendelkezésre álló információk megtekintése a [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="04d98-213">View the information available at the [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[virtual_machine_tomcat]:media/java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]:media/java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]:media/java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]:media/java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]:media/java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]:media/java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]:media/java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]:media/java-run-tomcat-app-server/NewRuleProfile.png


<!-- Deleted from the "To create an ednpoint for your virtual mache" 3/17/2017,
     to use the new portal.
6. In the **Add endpoint** dialog box, ensure **Add standalone endpoint** is selected, and then click **Next**.
7. In the **New endpoint details** dialog box:
-->
