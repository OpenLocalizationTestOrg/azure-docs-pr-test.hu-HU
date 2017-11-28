---
title: "Natív üzemmódú jelentéskészítő kiszolgálón hozzon létre egy virtuális Gépet a PowerShell használatával |} Microsoft Docs"
description: "Ez a témakör ismerteti, és bemutatja, hogyan telepítését és konfigurálását az SQL Server Reporting Services natív üzemmódú jelentéskészítő kiszolgáló egy Azure virtuális gépen. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: 5e5c11251cd316e8161dbe362b300be76927ac01
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-powershell-to-create-an-azure-vm-with-a-native-mode-report-server"></a><span data-ttu-id="44244-103">Natív üzemmódú jelentéskészítő kiszolgálót futtató Azure-beli virtuális gép létrehozása a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="44244-103">Use PowerShell to Create an Azure VM With a Native Mode Report Server</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="44244-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="44244-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="44244-105">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="44244-105">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="44244-106">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="44244-106">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

<span data-ttu-id="44244-107">Ez a témakör ismerteti, és bemutatja, hogyan telepítését és konfigurálását az SQL Server Reporting Services natív üzemmódú jelentéskészítő kiszolgáló egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="44244-107">This topic describes and walks you through the deployment and configuration of a SQL Server Reporting Services native mode report server in an Azure Virtual Machine.</span></span> <span data-ttu-id="44244-108">A jelen dokumentumban leírt lépések manuális lépések kombinálhatja a virtuális gép és a Reporting Services konfigurálásához a virtuális Gépet a Windows PowerShell-parancsfájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="44244-108">The steps in this document use a combination of manual steps to create the virtual machine and a Windows PowerShell script to configure Reporting Services on the VM.</span></span> <span data-ttu-id="44244-109">A konfigurációs parancsfájl tartalmazza a HTTP és HTTPs a tűzfal port megnyitása.</span><span class="sxs-lookup"><span data-stu-id="44244-109">The configuration script includes opening a firewall port for HTTP or HTTPs.</span></span>

> [!NOTE]
> <span data-ttu-id="44244-110">Ha nincs szüksége **HTTPS** a jelentéskészítő kiszolgálón **a 2**.</span><span class="sxs-lookup"><span data-stu-id="44244-110">If you do not require **HTTPS** on the report server, **skip step 2**.</span></span>
> 
> <span data-ttu-id="44244-111">Miután létrehozta a virtuális gép az 1. lépésben, keresse a parancsfájl használata a jelentéskészítő kiszolgáló és a HTTP konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="44244-111">After creating the VM in step 1, go to the section Use script to configure the report server and HTTP.</span></span> <span data-ttu-id="44244-112">A parancsfájl futtatása után a jelentéskészítő kiszolgáló használatra készen áll.</span><span class="sxs-lookup"><span data-stu-id="44244-112">After you run the script, the report server is ready to use.</span></span>

## <a name="prerequisites-and-assumptions"></a><span data-ttu-id="44244-113">Előfeltételek és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="44244-113">Prerequisites and Assumptions</span></span>
* <span data-ttu-id="44244-114">**Azure-előfizetés**: Ellenőrizze az elérhető az Azure-előfizetésében magok száma.</span><span class="sxs-lookup"><span data-stu-id="44244-114">**Azure Subscription**: Verify the number of cores available in your Azure Subscription.</span></span> <span data-ttu-id="44244-115">Ha létrehozta az ajánlott Virtuálisgép-méretet **A3**, kell **4** rendelkezésre álló magot.</span><span class="sxs-lookup"><span data-stu-id="44244-115">If you create the recommended VM size of **A3**, you need **4** available cores.</span></span> <span data-ttu-id="44244-116">Ha egy Virtuálisgép-méretet **A2**, kell **2** rendelkezésre álló magot.</span><span class="sxs-lookup"><span data-stu-id="44244-116">If you use a VM size of **A2**, you need **2** available cores.</span></span>
  
  * <span data-ttu-id="44244-117">Ellenőrizze a core korlátot az előfizetéséhez, a klasszikus Azure portálon, kattintson a beállítások gombra a bal oldali ablaktáblán, majd kattintson a használati a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="44244-117">To verify the core limit of your subscription, in the Azure classic portal, click SETTINGS in the left pane and then Click USAGE in the top menu.</span></span>
  * <span data-ttu-id="44244-118">A core kvóta növeléséhez forduljon [Azure támogatási](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="44244-118">To increase the core quota, contact [Azure Support](https://azure.microsoft.com/support/options/).</span></span> <span data-ttu-id="44244-119">Virtuálisgép-méret információkért lásd: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="44244-119">For VM size information, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="44244-120">**A Windows PowerShell-Parancsprogramokról**: A témakör feltételezi, hogy rendelkezik-e a Windows PowerShell alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="44244-120">**Windows PowerShell Scripting**: The topic assumes that you have a basic working knowledge of Windows PowerShell.</span></span> <span data-ttu-id="44244-121">Windows PowerShell használatával kapcsolatos további információkért tekintse meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="44244-121">For more information about using Windows PowerShell, see the following:</span></span>
  
  * [<span data-ttu-id="44244-122">A Windows PowerShell indítása a Windows Server</span><span class="sxs-lookup"><span data-stu-id="44244-122">Starting Windows PowerShell on Windows Server</span></span>](https://technet.microsoft.com/library/hh847814.aspx)
  * [<span data-ttu-id="44244-123">Bevezetés a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="44244-123">Getting Started with Windows PowerShell</span></span>](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a><span data-ttu-id="44244-124">1. lépés: Az Azure virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="44244-124">Step 1: Provision an Azure Virtual Machine</span></span>
1. <span data-ttu-id="44244-125">Tallózással keresse meg a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="44244-125">Browse to the Azure classic portal.</span></span>
2. <span data-ttu-id="44244-126">Kattintson a **virtuális gépek** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="44244-126">Click **Virtual Machines** in the left pane.</span></span>
   
    ![a Microsoft azure virtuális gépek](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. <span data-ttu-id="44244-128">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="44244-128">Click **New**.</span></span>
   
    ![Új gomb](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. <span data-ttu-id="44244-130">Kattintson a **gyűjteményből**.</span><span class="sxs-lookup"><span data-stu-id="44244-130">Click **From Gallery**.</span></span>
   
    ![új virtuális gép gyűjteményből](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. <span data-ttu-id="44244-132">Kattintson a **SQL Server 2014 RTM Standard – Windows Server 2012 R2** és kattintson a nyílra, a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="44244-132">Click **SQL Server 2014 RTM Standard – Windows Server 2012 R2** and then click the arrow to continue.</span></span>
   
    ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    <span data-ttu-id="44244-134">Ha a Reporting Services adatvezérelt előfizetések funkció van szüksége, válassza a **SQL Server 2014 RTM vállalati – Windows Server 2012 R2**.</span><span class="sxs-lookup"><span data-stu-id="44244-134">If you need the Reporting Services data driven subscriptions feature, choose **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**.</span></span> <span data-ttu-id="44244-135">SQL Server kiadása és által nyújtott szolgáltatások támogatásáról további információkért lásd: [SQL Server 2012 kiadásai által támogatott funkciók](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span><span class="sxs-lookup"><span data-stu-id="44244-135">For more information on SQL Server editions and feature support, see [Features Supported by the Editions of SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span></span>
6. <span data-ttu-id="44244-136">Az a **virtuálisgép-konfiguráció** lapon, módosítsa a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="44244-136">On the **Virtual machine configuration** page, edit the following fields:</span></span>
   
   * <span data-ttu-id="44244-137">Ha egynél több **VERZIÓJÚ kiadási dátum**, válassza ki az alkalmazás legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="44244-137">If there is more than one **VERSION RELEASE DATE**, select the most recent version.</span></span>
   * <span data-ttu-id="44244-138">**Virtuális gép neve**: A számítógép nevét is szolgál a következő konfigurációs lapon alapértelmezett felhőalapú szolgáltatás DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="44244-138">**Virtual Machine Name**: The machine name is also used on the next configuration page as the default Cloud Service DNS name.</span></span> <span data-ttu-id="44244-139">Az Azure szolgáltatásban, egyedinek kell lennie a DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="44244-139">The DNS name must be unique across the Azure service.</span></span> <span data-ttu-id="44244-140">Fontolja meg, hogy a virtuális Gépet, amely leírja, hogy a virtuális gép használt számítógépnévvel.</span><span class="sxs-lookup"><span data-stu-id="44244-140">Consider configuring the VM with a computer name that describes what the VM is used for.</span></span> <span data-ttu-id="44244-141">Például ssrsnativecloud.</span><span class="sxs-lookup"><span data-stu-id="44244-141">For example ssrsnativecloud.</span></span>
   * <span data-ttu-id="44244-142">**Réteg**: Standard</span><span class="sxs-lookup"><span data-stu-id="44244-142">**Tier**: Standard</span></span>
   * <span data-ttu-id="44244-143">**Méret: A3** van az SQL Server munkaterhelésekhez ajánlott Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="44244-143">**Size:A3** is the recommended VM size for SQL Server workloads.</span></span> <span data-ttu-id="44244-144">Ha egy virtuális gép csak egy jelentéskészítő kiszolgálón, a virtuális gép méretét A2 is használhatók, kivéve, ha a jelentéskészítő kiszolgáló során a nagy terhelés lép fel.</span><span class="sxs-lookup"><span data-stu-id="44244-144">If a VM is only used as a report server, a VM size of A2 is sufficient unless the report server experiences a large workload.</span></span> <span data-ttu-id="44244-145">A virtuális gép a díjszabásról, lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="44244-145">For VM pricing information, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>
   * <span data-ttu-id="44244-146">**Új felhasználónevet**: a megadott jön létre a virtuális gép rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="44244-146">**New User Name**: the name you provide is created as an administrator on the VM.</span></span>
   * <span data-ttu-id="44244-147">**Új jelszó** és **megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="44244-147">**New Password** and **confirm**.</span></span> <span data-ttu-id="44244-148">Ezt a jelszót az új rendszergazdafiókhoz szolgál, és erős jelszó használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="44244-148">This password is used for the new administrator account and it is recommended you use a strong password.</span></span>
   * <span data-ttu-id="44244-149">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-149">Click **Next**.</span></span> <span data-ttu-id="44244-150">![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span><span class="sxs-lookup"><span data-stu-id="44244-150">![next](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span></span>
7. <span data-ttu-id="44244-151">A következő lapon szerkessze a következő mezőket:</span><span class="sxs-lookup"><span data-stu-id="44244-151">On the next page, edit the following fields:</span></span>
   
   * <span data-ttu-id="44244-152">**A felhőalapú szolgáltatás**: válasszon **hozzon létre egy új felhőalapú szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="44244-152">**Cloud Service**: select **Create a new Cloud Service**.</span></span>
   * <span data-ttu-id="44244-153">**A felhőalapú szolgáltatás DNS-név**: a felhőalapú szolgáltatás, a virtuális Géphez társított nyilvános DNS-neve.</span><span class="sxs-lookup"><span data-stu-id="44244-153">**Cloud Service DNS Name**: This is the public DNS name of the Cloud Service that is associated with the VM.</span></span> <span data-ttu-id="44244-154">Alapértelmezés szerint ez a virtuális gép nevét a megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="44244-154">The default name is the name you typed in for the VM name.</span></span> <span data-ttu-id="44244-155">Ha a témakör a későbbi lépésekben hoz létre egy megbízható az SSL-tanúsítványt, és majd a DNS-név értékének használható a "**kiadott**" a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="44244-155">If in later steps of the topic, you create a trusted SSL certificate and then the DNS name is used for the value of the “**Issued to**” of the certificate.</span></span>
   * <span data-ttu-id="44244-156">**Régió/affinitás csoport/virtuális hálózati**: válassza ki a végfelhasználók legközelebb eső régiót.</span><span class="sxs-lookup"><span data-stu-id="44244-156">**Region/Affinity Group/Virtual Network**: Choose the region closest to your end users.</span></span>
   * <span data-ttu-id="44244-157">**A Tárfiók**: egy automatikusan létrehozott tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="44244-157">**Storage Account**: Use an automatically generated storage account.</span></span>
   * <span data-ttu-id="44244-158">**A rendelkezésre állási csoport**: nincs.</span><span class="sxs-lookup"><span data-stu-id="44244-158">**Availability Set**: None.</span></span>
   * <span data-ttu-id="44244-159">**VÉGPONTOK** tartsa a **távoli asztal** és **PowerShell** végpontok majd adja hozzá a HTTP vagy HTTPS-végpont a környezettől függően.</span><span class="sxs-lookup"><span data-stu-id="44244-159">**ENDPOINTS** Keep the **Remote Desktop** and **PowerShell** endpoints and then add either an HTTP or HTTPS endpoint, depending on your environment.</span></span>
     
     * <span data-ttu-id="44244-160">**HTTP**: az alapértelmezett nyilvános és titkos portok: **80**.</span><span class="sxs-lookup"><span data-stu-id="44244-160">**HTTP**: The default public and private ports are **80**.</span></span> <span data-ttu-id="44244-161">Vegye figyelembe, hogy ha egy magánhálózati port a 80-as, nem módosíthatja **$HTTPport = 80** a http-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="44244-161">Note that if you use a private port other than 80, modify **$HTTPport = 80** in the http script.</span></span>
     * <span data-ttu-id="44244-162">**HTTPS**: az alapértelmezett nyilvános és titkos portok: **443-as**.</span><span class="sxs-lookup"><span data-stu-id="44244-162">**HTTPS**: The default public and private ports are **443**.</span></span> <span data-ttu-id="44244-163">Biztonsági szempontból ajánlott, hogy módosítsa a magánhálózati port és a tűzfal és a jelentéskészítő kiszolgáló a magánhálózati port használatára.</span><span class="sxs-lookup"><span data-stu-id="44244-163">A security best practice is to change the private port and configure your firewall and the report server to use the private port.</span></span> <span data-ttu-id="44244-164">A végpontok további információkért lásd: [hogyan állítsa be kommunikáció a virtuális gép](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="44244-164">For more information on endpoints, see [How to Set Up Communication with a Virtual Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="44244-165">Vegye figyelembe, hogy egy 443,-astól eltérő port használata esetén módosítsa a paraméter **$HTTPsport = 443-as** a HTTPS-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="44244-165">Note that if you use a port other than 443, change the parameter **$HTTPsport = 443** in the HTTPS script.</span></span>
   * <span data-ttu-id="44244-166">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-166">Click next .</span></span> ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. <span data-ttu-id="44244-168">A varázsló utolsó oldalán, tartsa meg az alapértelmezett **a Virtuálisgép-ügynök telepítése** kijelölt.</span><span class="sxs-lookup"><span data-stu-id="44244-168">On the last page of the wizard, keep the default **Install the VM agent** selected.</span></span> <span data-ttu-id="44244-169">A témakörben ismertetett lépések nem használja a Virtuálisgép-ügynök, de ha le szeretné tartani a virtuális Gépet, a Virtuálisgép-ügynök és a bővítmények lehetővé teszi javítása érdekében, hogy CM.</span><span class="sxs-lookup"><span data-stu-id="44244-169">The steps in this topic do not utilize the VM agent but if you plan to keep this VM, the VM agent and extensions will allow you to enhance he CM.</span></span>  <span data-ttu-id="44244-170">A Virtuálisgép-ügynök további információkért lásd: [ügynök és Virtuálisgép-bővítmények – 1. rész](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span><span class="sxs-lookup"><span data-stu-id="44244-170">For more information on the VM agent, see [VM Agent and Extensions – Part 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span></span> <span data-ttu-id="44244-171">Az alapértelmezett telepített kiterjesztéseket ad futó egyik a "BGINFO" bővítményt, amely a virtuális gép asztali, a rendszer-információkat, például a belső IP-cím és a szabad lemezterületet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="44244-171">One of the default extensions installed ad running is the “BGINFO” extension that displays on the VM desktop, system information such as internal IP and free drive space.</span></span>
9. <span data-ttu-id="44244-172">Kattintson a Kész gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-172">Click complete .</span></span> ![oké](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. <span data-ttu-id="44244-174">A **állapota** látható a virtuális gép **indítása (kiépítés)** a kiépítési folyamat során értékként jelenik majd meg **futtató** a virtuális gép esetén kiosztott és készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="44244-174">The **Status** of the VM displays as **Starting (Provisioning)** during the provision process and then displays as **Running** when the VM is provisioned and ready to use.</span></span>

## <a name="step-2-create-a-server-certificate"></a><span data-ttu-id="44244-175">2. lépés: A kiszolgálói tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="44244-175">Step 2: Create a Server Certificate</span></span>
> [!NOTE]
> <span data-ttu-id="44244-176">Ha nincs szüksége HTTPS a jelentéskészítő kiszolgálón, akkor **a 2** , és keresse meg a **parancsfájl segítségével konfigurálja a jelentéskészítő kiszolgáló és a HTTP**.</span><span class="sxs-lookup"><span data-stu-id="44244-176">If you do not require HTTPS on the report server, you can **skip step 2** and go to the section **Use script to configure the report server and HTTP**.</span></span> <span data-ttu-id="44244-177">A HTTP-parancsfájl segítségével gyorsan konfigurálása a jelentéskészítő kiszolgáló, és a jelentéskészítő kiszolgáló készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="44244-177">Use the HTTP script to quickly configure the report server and the report server will be ready to use.</span></span>

<span data-ttu-id="44244-178">A virtuális Gépen a HTTPS protokoll használatához megbízható az SSL-tanúsítvány van szüksége.</span><span class="sxs-lookup"><span data-stu-id="44244-178">In order to use HTTPS on the VM, you need a trusted SSL certificate.</span></span> <span data-ttu-id="44244-179">A forgatókönyvtől függően az alábbi két módszer egyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="44244-179">Depending on your scenario, you can use one of the following two methods:</span></span>

* <span data-ttu-id="44244-180">Érvényes SSL-tanúsítvány kiadott által hitelesítésszolgáltató (CA), és a Microsoft megbízhatónak.</span><span class="sxs-lookup"><span data-stu-id="44244-180">A valid SSL certificate issued by a Certification Authority (CA) and trusted by Microsoft.</span></span> <span data-ttu-id="44244-181">A Microsoft Root Certificate programon keresztül terjeszteni a hitelesítésszolgáltató legfelső szintű tanúsítványok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="44244-181">The CA root certificates are required to be distributed via the Microsoft Root Certificate Program.</span></span> <span data-ttu-id="44244-182">A programra vonatkozó további információkért lásd: [Windows és Windows Phone 8 SSL Root Certificate Program (tag CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) és [bemutatása a Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-182">For more information about this program, see [Windows and Windows Phone 8 SSL Root Certificate Program (Member CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) and [Introduction to The Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span></span>
* <span data-ttu-id="44244-183">Egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="44244-183">A self-signed certificate.</span></span> <span data-ttu-id="44244-184">Önaláírt tanúsítványok éles környezetekben nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="44244-184">Self-signed certificates are not recommended for production environments.</span></span>

### <a name="to-use-a-certificate-created-by-a-trusted-certificate-authority-ca"></a><span data-ttu-id="44244-185">Létrehozott egy megbízható tanúsítvány hitelesítésszolgáltatói (CA) tanúsítvány használatára</span><span class="sxs-lookup"><span data-stu-id="44244-185">To use a certificate created by a trusted Certificate Authority (CA)</span></span>
1. <span data-ttu-id="44244-186">**A webhely kiszolgálói tanúsítványt kérhet egy hitelesítésszolgáltatótól**.</span><span class="sxs-lookup"><span data-stu-id="44244-186">**Request a server certificate for the website from a certification authority**.</span></span> 
   
    <span data-ttu-id="44244-187">A kiszolgálói tanúsítvány varázsló használhatja, vagy egy kérelem Tanúsítványfájl (Certreq.txt) hitelesítésszolgáltató küldendő létrehozásához, vagy egy online hitelesítésszolgáltatót kérelmet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="44244-187">You can use the Web Server Certificate Wizard either to generate a certificate request file (Certreq.txt) that you send to a certification authority, or to generate a request for an online certification authority.</span></span> <span data-ttu-id="44244-188">Például a Microsoft tanúsítványszolgáltatások újdonságai a Windows Server 2012-ben.</span><span class="sxs-lookup"><span data-stu-id="44244-188">For example, Microsoft Certificate Services in Windows Server 2012.</span></span> <span data-ttu-id="44244-189">Attól függően, hogy a kiszolgálói tanúsítvány által kínált azonosító megbízhatósági szintjét akkor a hitelesítésszolgáltatót a kérelem jóváhagyása és a tanúsítvány fájl küldése néhány hónapig néhány nap.</span><span class="sxs-lookup"><span data-stu-id="44244-189">Depending on the level of identification assurance offered by your server certificate, it is several days to several months for the certification authority to approve your request and send you a certificate file.</span></span> 
   
    <span data-ttu-id="44244-190">A kiszolgálói tanúsítványok kérésével kapcsolatos további információkért tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="44244-190">For more information about requesting a server certificates, see the following:</span></span> 
   
   * <span data-ttu-id="44244-191">Használjon [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-191">Use [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span></span>
   * <span data-ttu-id="44244-192">Biztonsági eszközök a Windows Server 2012 rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="44244-192">Security Tools to Administer Windows Server 2012.</span></span>
     
     [<span data-ttu-id="44244-193">Biztonsági eszközök a Windows Server 2012 rendszerhez</span><span class="sxs-lookup"><span data-stu-id="44244-193">Security Tools to Administer Windows Server 2012</span></span>](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > <span data-ttu-id="44244-194">A **kiadott** mező a megbízható az SSL-tanúsítvány egyeznie kell a **felhőalapú szolgáltatás DNS-név** az új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="44244-194">The **issued to** field of the trusted SSL certificate should be the same as the **Cloud Service DNS NAME** you used for the new VM.</span></span>

2. <span data-ttu-id="44244-195">**A kiszolgálótanúsítvány telepítésének a webkiszolgálón**.</span><span class="sxs-lookup"><span data-stu-id="44244-195">**Install the server certificate on the Web server**.</span></span> <span data-ttu-id="44244-196">A webkiszolgáló ebben az esetben a virtuális gép, amelyen a jelentéskészítő kiszolgáló és a webhely a későbbi lépésekben jön létre, amikor a Reporting Services konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="44244-196">The Web server in this case is the VM that hosts the report server and the website is created in later steps when you configure Reporting Services.</span></span> <span data-ttu-id="44244-197">A webkiszolgáló a kiszolgáló-tanúsítvány telepítése a tanúsítvány MMC beépülő modul használatával kapcsolatos további információkért lásd: [a kiszolgálótanúsítvány telepítésének](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="44244-197">For more information about installing the server certificate on the Web server by using the Certificate MMC snap-in, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
    <span data-ttu-id="44244-198">Ha a jelentéskészítő kiszolgáló, az érték a tanúsítványok konfigurálása az ebben a témakörben-parancsprogramja használni kívánt **ujjlenyomat** szükséges, a parancsfájl-paraméterként.</span><span class="sxs-lookup"><span data-stu-id="44244-198">If you want to use the script included with this topic, to configure the report server, the value of the certificates **Thumbprint** is required as a parameter of the script.</span></span> <span data-ttu-id="44244-199">Tekintse át a következő című szakaszban talál információt szerezni a tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="44244-199">See the next section for details on how to obtain the thumbprint of the certificate.</span></span>
3. <span data-ttu-id="44244-200">A kiszolgáló-tanúsítványt hozzárendeli a jelentéskészítő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="44244-200">Assign the server certificate to the report server.</span></span> <span data-ttu-id="44244-201">A hozzárendelés befejeződött a következő szakaszban a jelentéskészítő kiszolgáló konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="44244-201">The assignment is completed in the next section when you configure the report server.</span></span>

### <a name="to-use-the-virtual-machines-self-signed-certificate"></a><span data-ttu-id="44244-202">A virtuális gépek önaláírt tanúsítvány használatára</span><span class="sxs-lookup"><span data-stu-id="44244-202">To use the Virtual Machines Self-signed Certificate</span></span>
<span data-ttu-id="44244-203">Önaláírt tanúsítvány hozta létre a virtuális Gépre, a virtuális gép lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="44244-203">A self-signed certificate was created on the VM when the VM was provisioned.</span></span> <span data-ttu-id="44244-204">A tanúsítvány a a virtuális gép DNS-névvel megegyező névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44244-204">The certificate has the same name as the VM DNS name.</span></span> <span data-ttu-id="44244-205">Hitelesítési hibák elkerülése érdekében szükség, hogy megbízható, maga a virtuális gépen és a hely összes felhasználó-e a tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="44244-205">In order to avoid certificate errors, it is required that the certificate is trusted on the VM itself, and also by all users of the site.</span></span>

1. <span data-ttu-id="44244-206">Megbízható a legfelső szintű hitelesítésszolgáltató a tanúsítvány a helyi virtuális Gépen, vegye fel a tanúsítványt a **megbízható legfelső szintű hitelesítésszolgáltatók**.</span><span class="sxs-lookup"><span data-stu-id="44244-206">To trust the root CA of the certificate on the Local VM, add the certificate to the **Trusted Root Certification Authorities**.</span></span> <span data-ttu-id="44244-207">A következő szükséges lépések összefoglalása látható.</span><span class="sxs-lookup"><span data-stu-id="44244-207">The following is a summary of the steps required.</span></span> <span data-ttu-id="44244-208">Ismerteti, hogy bízzon meg a hitelesítésszolgáltató, lásd a [a kiszolgálótanúsítvány telepítésének](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="44244-208">For detailed steps on how to trust the CA, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
   1. <span data-ttu-id="44244-209">A klasszikus Azure portálon válassza ki a virtuális Gépet, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-209">From the Azure classic portal, select the VM and click connect.</span></span> <span data-ttu-id="44244-210">A böngésző konfigurációtól függően előfordulhat, hogy kérni fogja menteni egy .rdp fájlt, a virtuális Géphez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="44244-210">Depending on your browser configuration, you may be prompted to save an .rdp file for connecting to the VM.</span></span>
      
       ![Csatlakozás az azure virtuális géphez](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="44244-212">A felhasználói virtuális gép nevét, a felhasználónév és a virtuális gép létrehozásakor beállított jelszót használja.</span><span class="sxs-lookup"><span data-stu-id="44244-212">Use the user VM name, user name and password you configured when you created the VM.</span></span> 
      
       <span data-ttu-id="44244-213">Például az alábbi ábrán a virtuális gép neve van **ssrsnativecloud** és a felhasználónév **tesztfelhasználó néven**.</span><span class="sxs-lookup"><span data-stu-id="44244-213">For example, in the following image, the VM name is **ssrsnativecloud** and the user name is **testuser**.</span></span>
      
       ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. <span data-ttu-id="44244-215">Futtassa a mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="44244-215">Run mmc.exe.</span></span> <span data-ttu-id="44244-216">További információkért lásd: [hogyan: tanúsítványok megtekintése az MMC beépülő modullal rendelkező](https://msdn.microsoft.com/library/ms788967.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-216">For more information, see [How to: View Certificates with the MMC Snap-in](https://msdn.microsoft.com/library/ms788967.aspx).</span></span>
   3. <span data-ttu-id="44244-217">A Konzolalkalmazás **fájl** menüben adja hozzá a **tanúsítványok** beépülő modulban válassza **számítógépfiók** kéri, és kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="44244-217">In the console application **File** menu, add the **Certificates** snap-in, select **Computer Account** when prompted, and then click **Next**.</span></span>
   4. <span data-ttu-id="44244-218">Válassza ki **helyi számítógép** kezelése, majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="44244-218">Select **Local Computer** to manage and then click **Finish**.</span></span>
   5. <span data-ttu-id="44244-219">Kattintson a **Ok** majd bontsa ki a **tanúsítványok - személyes** csomópont, és kattintson **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="44244-219">Click **Ok** and then expand the **Certificates -Personal** nodes and then click **Certificates**.</span></span> <span data-ttu-id="44244-220">A tanúsítvány neve után a virtuális Gépet DNS-nevét, és végződik **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="44244-220">The certificate is named after the DNS name of the VM and ends with **cloudapp.net**.</span></span> <span data-ttu-id="44244-221">Kattintson a jobb gombbal a tanúsítványnevet, és kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="44244-221">Right-click the certificate name and click **Copy**.</span></span>
   6. <span data-ttu-id="44244-222">Bontsa ki a **megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, és kattintson rá jobb gombbal **tanúsítványok** majd **Beillesztés**.</span><span class="sxs-lookup"><span data-stu-id="44244-222">Expand the **Trusted Root Certification Authorities** node and then right-click **Certificates** and then click **Paste**.</span></span>
   7. <span data-ttu-id="44244-223">Érvényesíteni, kattintson duplán arra a tanúsítvány neve alatt **megbízható legfelső szintű hitelesítésszolgáltatók** , és ellenőrizze, hogy nincsenek hibák, és a tanúsítvány látható.</span><span class="sxs-lookup"><span data-stu-id="44244-223">To validate, double click on the certificate name under **Trusted Root Certification Authorities** and verify that there are no errors and you see your certificate.</span></span> <span data-ttu-id="44244-224">Ha szeretné-e a HTTPS-parancsprogramja Ez a témakör segítségével konfigurálja a jelentéskészítő kiszolgáló, a tanúsítványok értékének **ujjlenyomat** szükséges, a parancsfájl-paraméterként.</span><span class="sxs-lookup"><span data-stu-id="44244-224">If you want to use the HTTPS script included with this topic, to configure the report server, the value of the certificates **Thumbprint** is required as a parameter of the script.</span></span> <span data-ttu-id="44244-225">**Az ujjlenyomat értékének eléréséhez**, adja meg a következőket.</span><span class="sxs-lookup"><span data-stu-id="44244-225">**To get the thumbprint value**, complete the following.</span></span> <span data-ttu-id="44244-226">Szerepel továbbá egy PowerShell-példa szakaszában az ujjlenyomat beolvasása [parancsfájl segítségével konfigurálja a jelentéskészítő kiszolgáló és a HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span><span class="sxs-lookup"><span data-stu-id="44244-226">There is also a PowerShell sample to retrieve the thumbprint in section [Use script to configure the report server and HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span></span>
      
      1. <span data-ttu-id="44244-227">Kattintson duplán a tanúsítvány, például ssrsnativecloud.cloudapp.net neve.</span><span class="sxs-lookup"><span data-stu-id="44244-227">Double-click the name of the certificate, for example ssrsnativecloud.cloudapp.net.</span></span>
      2. <span data-ttu-id="44244-228">Kattintson a **részletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="44244-228">Click the **Details** tab.</span></span>
      3. <span data-ttu-id="44244-229">Kattintson a **ujjlenyomat**.</span><span class="sxs-lookup"><span data-stu-id="44244-229">Click **Thumbprint**.</span></span> <span data-ttu-id="44244-230">Az ujjlenyomat értékének jelenik meg a részleteket tartalmazó mező, például a6 08 3 c. df f9 0b f7 e3 7c 25 kell adnia végrehajtási adatokat a4 kell adnia végrehajtási adatokat 7e ac 91 9c 2c fb 2f.</span><span class="sxs-lookup"><span data-stu-id="44244-230">The value of the thumbprint is displayed in the details field, for example ‎a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.</span></span>
      4. <span data-ttu-id="44244-231">Másolja le az ujjlenyomatot, és mentse a értéket későbbi használatra, vagy a parancsfájl most szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="44244-231">Copy the thumbprint and save the value for later or edit the script now.</span></span>
      5. <span data-ttu-id="44244-232">(*) A parancsfájl futtatása előtt távolítsa el a szóközöket a értékpárok Between.</span><span class="sxs-lookup"><span data-stu-id="44244-232">(*) Before you run the script, remove the spaces in between the pairs of values.</span></span> <span data-ttu-id="44244-233">Például az ujjlenyomat előtt feljegyzett most lenne a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span><span class="sxs-lookup"><span data-stu-id="44244-233">For example the thumbprint noted before would now be ‎a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span></span>
      6. <span data-ttu-id="44244-234">A kiszolgáló-tanúsítványt hozzárendeli a jelentéskészítő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="44244-234">Assign the server certificate to the report server.</span></span> <span data-ttu-id="44244-235">A hozzárendelés befejeződött a következő szakaszban a jelentéskészítő kiszolgáló konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="44244-235">The assignment is completed in the next section when you configure the report server.</span></span>

<span data-ttu-id="44244-236">Ha egy önaláírt SSL-tanúsítványt használ, a tanúsítvány neve már megfelel a virtuális gép állomásnevét.</span><span class="sxs-lookup"><span data-stu-id="44244-236">If you are using a self-signed SSL certificate, the name on the certificate already matches the hostname of the VM.</span></span> <span data-ttu-id="44244-237">Ezért a DNS, a gép már regisztrálva van globálisan, és bármely ügyfél érhető el.</span><span class="sxs-lookup"><span data-stu-id="44244-237">Therefore, the DNS of the machine is already registered globally and can be accessed from any client.</span></span>

## <a name="step-3-configure-the-report-server"></a><span data-ttu-id="44244-238">3. lépés: A jelentéskészítő kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44244-238">Step 3: Configure the Report Server</span></span>
<span data-ttu-id="44244-239">Ez a szakasz végigvezeti a virtuális gép konfigurálása natív üzemmódú Reporting Services jelentéskészítő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="44244-239">This section walks you through configuring the VM as a Reporting Services native mode report server.</span></span> <span data-ttu-id="44244-240">A jelentéskészítő kiszolgáló konfigurálása a következő módszerek egyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="44244-240">You can use one of the following methods to configure the report server:</span></span>

* <span data-ttu-id="44244-241">A parancsfájl használata a jelentéskészítő kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44244-241">Use the script to configure the report server</span></span>
* <span data-ttu-id="44244-242">A jelentéskészítő kiszolgáló konfigurálása a Configuration Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="44244-242">Use Configuration Manager to Configure the Report Server.</span></span>

<span data-ttu-id="44244-243">Részletes lépéseket, tekintse meg a szakasz [csatlakozzon a virtuális géphez, és indítsa el a Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="44244-243">For more detailed steps, see the section [Connect to the Virtual Machine and Start the Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span></span>

<span data-ttu-id="44244-244">**Hitelesítési Megjegyzés:** Windows-hitelesítés a javasolt hitelesítési módszert, valamint az alapértelmezett Reporting Services hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="44244-244">**Authentication Note:** Windows authentication is the recommended authentication method and it is the default Reporting Services authentication.</span></span> <span data-ttu-id="44244-245">Csak a virtuális Gépen konfigurált felhasználók férhetnek a Reporting Services és a Reporting Services szerepkörökhöz hozzárendelt.</span><span class="sxs-lookup"><span data-stu-id="44244-245">Only users that are configured on the VM can access Reporting Services and assigned to Reporting Services roles.</span></span>

### <a name="use-script-to-configure-the-report-server-and-http"></a><span data-ttu-id="44244-246">Parancsfájl használata a jelentéskészítő kiszolgáló és a HTTP konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44244-246">Use script to configure the report server and HTTP</span></span>
<span data-ttu-id="44244-247">A jelentéskészítő kiszolgáló konfigurálása a Windows PowerShell-parancsfájl használatához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="44244-247">To use the Windows PowerShell script to configure the report server, complete the following steps.</span></span> <span data-ttu-id="44244-248">A konfiguráció HTTP, HTTPS-nem tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="44244-248">The configuration includes HTTP, not HTTPS:</span></span>

1. <span data-ttu-id="44244-249">A klasszikus Azure portálon válassza ki a virtuális Gépet, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-249">From the Azure classic portal, select the VM and click connect.</span></span> <span data-ttu-id="44244-250">A böngésző konfigurációtól függően előfordulhat, hogy kérni fogja menteni egy .rdp fájlt, a virtuális Géphez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="44244-250">Depending on your browser configuration, you may be prompted to save an .rdp file for connecting to the VM.</span></span>
   
    ![Csatlakozás az azure virtuális géphez](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="44244-252">A felhasználói virtuális gép nevét, a felhasználónév és a virtuális gép létrehozásakor beállított jelszót használja.</span><span class="sxs-lookup"><span data-stu-id="44244-252">Use the user VM name, user name and password you configured when you created the VM.</span></span> 
   
    <span data-ttu-id="44244-253">Például az alábbi ábrán a virtuális gép neve van **ssrsnativecloud** és a felhasználónév **tesztfelhasználó néven**.</span><span class="sxs-lookup"><span data-stu-id="44244-253">For example, in the following image, the VM name is **ssrsnativecloud** and the user name is **testuser**.</span></span>
   
    ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="44244-255">Nyissa meg a virtuális gép **Windows PowerShell ISE** rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="44244-255">On the VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="44244-256">A PowerShell ISE a Windows server 2012 rendszeren alapértelmezés szerint telepítve van.</span><span class="sxs-lookup"><span data-stu-id="44244-256">The PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="44244-257">Javasoljuk, akkor az ISE helyett a szabványos Windows PowerShell-ablakot, hogy a parancsfájl illessze be az ISE, módosítsa a parancsfájlt, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="44244-257">It is recommended you use the ISE instead of a standard Windows PowerShell window so that you can paste the script into the ISE, modify the script, and then run the script.</span></span>
3. <span data-ttu-id="44244-258">A Windows PowerShell ISE, kattintson a **nézet** menüre, majd majd **parancsfájl ablaktábla megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="44244-258">In Windows PowerShell ISE, click the **View** menu and then click **Show Script Pane**.</span></span>
4. <span data-ttu-id="44244-259">Másolja a következő parancsfájlt, és illessze be a parancsfájlt a Windows PowerShell ISE parancsfájl ablaktáblára.</span><span class="sxs-lookup"><span data-stu-id="44244-259">Copy the following script, and paste the script into the Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
   
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. <span data-ttu-id="44244-260">Ha a virtuális Gépet egy eltérő 80-as HTTP-port hozta létre, módosítsa a paramétert $HTTPport = 80-as.</span><span class="sxs-lookup"><span data-stu-id="44244-260">If you created the VM with an HTTP port other than 80, modify the parameter $HTTPport = 80.</span></span>
6. <span data-ttu-id="44244-261">A parancsfájl a Reporting Services van beállítva.</span><span class="sxs-lookup"><span data-stu-id="44244-261">The script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="44244-262">Ha azt szeretné, a parancsfájl futtatásához a Reporting Services, a "v11", a Get-WmiObject fényében a névtér elérési verzió részét módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="44244-262">If you want to run the script for  Reporting Services, modify the version portion of the path to the namespace to “v11”, on the Get-WmiObject statement.</span></span>
7. <span data-ttu-id="44244-263">Futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="44244-263">Run the script.</span></span>

<span data-ttu-id="44244-264">**Érvényesítési**: az alapszintű jelentéskészítő kiszolgálói funkciók működésének ellenőrzéséhez tekintse meg a [a konfiguráció ellenőrzése](#verify-the-configuration) későbbi szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="44244-264">**Validation**: To verify that the basic report server functionality is working, see the [Verify the configuration](#verify-the-configuration) section later in this topic.</span></span>

### <a name="use-script-to-configure-the-report-server-and-https"></a><span data-ttu-id="44244-265">Parancsfájl használata a jelentéskészítő kiszolgáló és a HTTPS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44244-265">Use script to configure the report server and HTTPS</span></span>
<span data-ttu-id="44244-266">A jelentéskészítő kiszolgáló konfigurálása a Windows PowerShell használatához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="44244-266">To use Windows PowerShell to configure the report server, complete the following steps.</span></span> <span data-ttu-id="44244-267">A konfiguráció tartalmazza a HTTPS-en nem HTTP.</span><span class="sxs-lookup"><span data-stu-id="44244-267">The configuration includes HTTPS, not HTTP.</span></span>

1. <span data-ttu-id="44244-268">A klasszikus Azure portálon válassza ki a virtuális Gépet, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-268">From the Azure classic portal, select the VM and click connect.</span></span> <span data-ttu-id="44244-269">A böngésző konfigurációtól függően előfordulhat, hogy kérni fogja menteni egy .rdp fájlt, a virtuális Géphez való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="44244-269">Depending on your browser configuration, you may be prompted to save an .rdp file for connecting to the VM.</span></span>
   
    ![Csatlakozás az azure virtuális géphez](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="44244-271">A felhasználói virtuális gép nevét, a felhasználónév és a virtuális gép létrehozásakor beállított jelszót használja.</span><span class="sxs-lookup"><span data-stu-id="44244-271">Use the user VM name, user name and password you configured when you created the VM.</span></span> 
   
    <span data-ttu-id="44244-272">Például az alábbi ábrán a virtuális gép neve van **ssrsnativecloud** és a felhasználónév **tesztfelhasználó néven**.</span><span class="sxs-lookup"><span data-stu-id="44244-272">For example, in the following image, the VM name is **ssrsnativecloud** and the user name is **testuser**.</span></span>
   
    ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="44244-274">Nyissa meg a virtuális gép **Windows PowerShell ISE** rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="44244-274">On the VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="44244-275">A PowerShell ISE a Windows server 2012 rendszeren alapértelmezés szerint telepítve van.</span><span class="sxs-lookup"><span data-stu-id="44244-275">The PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="44244-276">Javasoljuk, akkor az ISE helyett a szabványos Windows PowerShell-ablakot, hogy a parancsfájl illessze be az ISE, módosítsa a parancsfájlt, és futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="44244-276">It is recommended you use the ISE instead of a standard Windows PowerShell window so that you can paste the script into the ISE, modify the script, and then run the script.</span></span>
3. <span data-ttu-id="44244-277">Ahhoz, hogy a parancsfájlok futtatását, futtassa a következő Windows PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="44244-277">To enable running scripts, run the following Windows PowerShell command:</span></span>
   
        Set-ExecutionPolicy RemoteSigned
   
    <span data-ttu-id="44244-278">Ezt követően futtathatja a házirend ellenőrzése a következőt:</span><span class="sxs-lookup"><span data-stu-id="44244-278">You can then run the following to verify the policy:</span></span>
   
        Get-ExecutionPolicy
4. <span data-ttu-id="44244-279">A **Windows PowerShell ISE**, kattintson a **nézet** menüre, majd majd **parancsfájl ablaktábla megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="44244-279">In **Windows PowerShell ISE**, click the **View** menu and then click **Show Script Pane**.</span></span>
5. <span data-ttu-id="44244-280">Másolja a következő parancsfájlt, és illessze be a Windows PowerShell ISE parancsfájl ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="44244-280">Copy the following script and paste it into the Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
   
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting the Report Manager URL ##
   
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. <span data-ttu-id="44244-281">Módosítsa a **$certificatehash** paraméter a parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="44244-281">Modify the **$certificatehash** parameter in the script:</span></span>
   
   * <span data-ttu-id="44244-282">Ez egy **szükséges** paraméter.</span><span class="sxs-lookup"><span data-stu-id="44244-282">This is a **required** parameter.</span></span> <span data-ttu-id="44244-283">Ha a tanúsítvány érték az előző lépésekben nem mentette, az a következő módszerek egyikét másolhatja a tanúsítvány kivonatoló értékét a tanúsítványok ujjlenyomatát.:</span><span class="sxs-lookup"><span data-stu-id="44244-283">If you did not save the certificate value from the previous steps, use one of the following methods to copy the certificate hash value from the certificates thumbprint.:</span></span>
     
       <span data-ttu-id="44244-284">A virtuális Gépre nyissa meg a Windows PowerShell ISE, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="44244-284">On the VM, open Windows PowerShell ISE and run the following command:</span></span>
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       <span data-ttu-id="44244-285">A kimenet az alábbihoz hasonló fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="44244-285">The output will look similar to the following.</span></span> <span data-ttu-id="44244-286">A parancsfájl egy üres sort ad vissza, ha a virtuális gép nem rendelkezik konfigurált például tanúsítvány, című részében [a virtuális gépek önaláírt tanúsítvány használatára](#to-use-the-virtual-machines-self-signed-certificate).</span><span class="sxs-lookup"><span data-stu-id="44244-286">If the script returns a blank line, the VM does not have a certificate configured for example, see the section [To use the Virtual Machines Self-signed Certificate](#to-use-the-virtual-machines-self-signed-certificate).</span></span>
     
     <span data-ttu-id="44244-287">VAGY</span><span class="sxs-lookup"><span data-stu-id="44244-287">OR</span></span>
   * <span data-ttu-id="44244-288">A virtuális gép fut mmc.exe, majd adja hozzá a **tanúsítványok** beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="44244-288">On the VM Run mmc.exe and then add the **Certificates** snap-in.</span></span>
   * <span data-ttu-id="44244-289">Az a **megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, kattintson duplán a tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="44244-289">Under the **Trusted Root Certificate Authorities** node double-click your certificate name.</span></span> <span data-ttu-id="44244-290">A virtuális gép önaláírt tanúsítványt használ, ha a tanúsítvány neve után a virtuális Gépet DNS-nevét, és végződik **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="44244-290">If you are using the self-signed certificate of the VM, the certificate is named after the DNS name of the VM and ends with **cloudapp.net**.</span></span>
   * <span data-ttu-id="44244-291">Kattintson a **részletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="44244-291">Click the **Details** tab.</span></span>
   * <span data-ttu-id="44244-292">Kattintson a **ujjlenyomat**.</span><span class="sxs-lookup"><span data-stu-id="44244-292">Click **Thumbprint**.</span></span> <span data-ttu-id="44244-293">Az ujjlenyomat értékének jelenik meg a részleteket tartalmazó mező, például af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span><span class="sxs-lookup"><span data-stu-id="44244-293">The value of the thumbprint is displayed in the details field, for example af 11 60 b6 4b 28 8d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span></span>
   * <span data-ttu-id="44244-294">**A parancsfájl futtatása előtt**, távolítsa el a szóközöket érték párok között.</span><span class="sxs-lookup"><span data-stu-id="44244-294">**Before you run the script**, remove the spaces in between the pairs of values.</span></span> <span data-ttu-id="44244-295">Például af1160b64b288d890a8212ff6ba9c3664f319048</span><span class="sxs-lookup"><span data-stu-id="44244-295">For example af1160b64b288d890a8212ff6ba9c3664f319048</span></span>
7. <span data-ttu-id="44244-296">Módosítsa a **$httpsport** paraméter:</span><span class="sxs-lookup"><span data-stu-id="44244-296">Modify the **$httpsport** parameter:</span></span> 
   
   * <span data-ttu-id="44244-297">Ha a 443-as portot használja a HTTPS-végponton, majd nem módosítania ezt a paramétert a parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="44244-297">If you used port 443 for the HTTPS endpoint, then you do not need to update this parameter in the script.</span></span> <span data-ttu-id="44244-298">Ellenkező esetben használja a port értékét a titkos HTTPS-végpont a virtuális Gépre konfigurálásakor választotta.</span><span class="sxs-lookup"><span data-stu-id="44244-298">Otherwise use the port value you selected when you configured the HTTPS private endpoint on the VM.</span></span>
8. <span data-ttu-id="44244-299">Módosítsa a **$DNSName** paraméter:</span><span class="sxs-lookup"><span data-stu-id="44244-299">Modify the **$DNSName** parameter:</span></span> 
   
   * <span data-ttu-id="44244-300">A parancsfájl beállítása helyettesítő tanúsítvány $DNSName = "+".</span><span class="sxs-lookup"><span data-stu-id="44244-300">The script is configured for a wild card certificate $DNSName="+".</span></span> <span data-ttu-id="44244-301">Ha így tesz, nem kívánt segítségével konfigurálhatja a helyettesítő tanúsítvány kötés $DNSName megjegyzéssé ="+"és a következő sorban, a teljes $DNSNAme hivatkozás, ## $DNSName="$server.cloudapp.net engedélyezése".</span><span class="sxs-lookup"><span data-stu-id="44244-301">If you do no want to configure for a wildcard certificate binding, comment out $DNSName="+" and enable the following line, the full $DNSNAme reference, ##$DNSName="$server.cloudapp.net".</span></span>
     
       <span data-ttu-id="44244-302">Módosítsa a $DNSName értékét, ha nem szeretné használni a Reporting Services a virtuális gép DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="44244-302">Change the $DNSName value if you do not want to use the virtual machine’s DNS name for Reporting Services.</span></span> <span data-ttu-id="44244-303">Ha a paraméter használata esetén a tanúsítványt kell használnia az ezt a nevet, és regisztrálnia globálisan a DNS-kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="44244-303">If you use the parameter, the certificate must also use this name and you register the name globally on a DNS server.</span></span>
9. <span data-ttu-id="44244-304">A parancsfájl a Reporting Services van beállítva.</span><span class="sxs-lookup"><span data-stu-id="44244-304">The script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="44244-305">Ha azt szeretné, a parancsfájl futtatásához a Reporting Services, a "v11", a Get-WmiObject fényében a névtér elérési verzió részét módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="44244-305">If you want to run the script for  Reporting Services, modify the version portion of the path to the namespace to “v11”, on the Get-WmiObject statement.</span></span>
10. <span data-ttu-id="44244-306">Futtassa a parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="44244-306">Run the script.</span></span>

<span data-ttu-id="44244-307">**Érvényesítési**: az alapszintű jelentéskészítő kiszolgálói funkciók működésének ellenőrzéséhez tekintse meg a [a konfiguráció ellenőrzése](#verify-the-connection) későbbi szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="44244-307">**Validation**: To verify that the basic report server functionality is working, see the [Verify the configuration](#verify-the-connection) section later in this topic.</span></span> <span data-ttu-id="44244-308">Lévő tanúsítvány ellenőrzéséhez kötési nyisson meg egy parancssort rendszergazdai jogosultságokkal, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="44244-308">To verify the certificate binding open a command prompt with administrative privileges, and then run the following command:</span></span>

    netsh http show sslcert

<span data-ttu-id="44244-309">Az eredmény a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="44244-309">The result will include the following:</span></span>

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-to-configure-the-report-server"></a><span data-ttu-id="44244-310">A jelentéskészítő kiszolgáló konfigurálása a Configuration Manager használatával</span><span class="sxs-lookup"><span data-stu-id="44244-310">Use Configuration Manager to Configure the Report Server</span></span>
<span data-ttu-id="44244-311">Ha nem szeretné, hogy a jelentéskészítő kiszolgáló konfigurálása a PowerShell parancsfájl futtatásához, kövesse az ebben a szakaszban a jelentéskészítő kiszolgáló konfigurálása a configuration manager jelentéskészítési szolgáltatások natív mód segítségével.</span><span class="sxs-lookup"><span data-stu-id="44244-311">If you do not want to run the PowerShell script to configure the report server, follow the steps in this section to use the Reporting Services native mode configuration manager to configure the report server.</span></span>

1. <span data-ttu-id="44244-312">A klasszikus Azure portálon válassza ki a virtuális Gépet, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-312">From the Azure classic portal, select the VM and click connect.</span></span> <span data-ttu-id="44244-313">A felhasználónév és a virtuális gép létrehozásakor beállított jelszó használata.</span><span class="sxs-lookup"><span data-stu-id="44244-313">Use the user name and password you configured when you created the VM.</span></span>
   
    ![Csatlakozás az azure virtuális géphez](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. <span data-ttu-id="44244-315">Futtassa a Windows update, és telepítse a frissítéseket a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="44244-315">Run Windows update and install updates to the VM.</span></span> <span data-ttu-id="44244-316">Ha a virtuális gép újraindítására szükség, indítsa újra a virtuális gép, és csatlakozzon újra a virtuális Gépet a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="44244-316">If a restart of the VM is required, restart the VM and reconnect to the VM from the Azure classic portal.</span></span>
3. <span data-ttu-id="44244-317">A virtuális Gépen a Start menüben írja be a **Reporting Services** , és nyissa meg **Reporting Services Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="44244-317">From the Start menu on the VM, type **Reporting Services** and open **Reporting Services Configuration Manager**.</span></span>
4. <span data-ttu-id="44244-318">Hagyja meg az alapértelmezett értékeit **kiszolgálónév** és **jelentéskészítő kiszolgáló példánya**.</span><span class="sxs-lookup"><span data-stu-id="44244-318">Leave the default values for **Server Name** and **Report Server Instance**.</span></span> <span data-ttu-id="44244-319">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-319">Click **Connect**.</span></span>
5. <span data-ttu-id="44244-320">Kattintson a bal oldali ablaktáblában **webes szolgáltatás URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="44244-320">In the left pane, click **Web Service URL**.</span></span>
6. <span data-ttu-id="44244-321">Alapértelmezés szerint RS "Az összes hozzárendelt" IP-80-as HTTP-port van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="44244-321">By default, RS is configured for HTTP port 80 with IP “All Assigned”.</span></span> <span data-ttu-id="44244-322">HTTPS hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="44244-322">To add HTTPS:</span></span>
   
   1. <span data-ttu-id="44244-323">A **SSL-tanúsítvány**: válassza ki a [virtuális gép neve] például használni kívánt tanúsítványt. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="44244-323">In **SSL Certificate**: select the certificate you want to use, for example [VM name].cloudapp.net.</span></span> <span data-ttu-id="44244-324">Ha nincsenek felsorolva tanúsítványok, tekintse meg a szakasz **2. lépés: hozzon létre egy kiszolgálói tanúsítványt** telepítéséről és a virtuális Gépen a tanúsítvány megbízható olvashat.</span><span class="sxs-lookup"><span data-stu-id="44244-324">If no certificates are listed, see the section **Step 2: Create a Server Certificate** for information on how to install and trust the certificate on the VM.</span></span>
   2. <span data-ttu-id="44244-325">A **SSL-Port**: válassza ki a 443-as.</span><span class="sxs-lookup"><span data-stu-id="44244-325">Under **SSL Port**: choose 443.</span></span> <span data-ttu-id="44244-326">Ha konfigurálta a titkos HTTPS-végpont a virtuális gép más magánhálózati port, használni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="44244-326">If you configured the HTTPS private endpoint in the VM with a different private port, use that value here.</span></span>
   3. <span data-ttu-id="44244-327">Kattintson a **alkalmaz** és várjon, amíg a művelet elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="44244-327">Click **Apply** and wait for the operation to complete.</span></span>
7. <span data-ttu-id="44244-328">Kattintson a bal oldali ablaktáblában **adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="44244-328">In the left pane, click **Database**.</span></span>
   
   1. <span data-ttu-id="44244-329">Kattintson a **Databas módosítása**e.</span><span class="sxs-lookup"><span data-stu-id="44244-329">Click **Change Databas**e.</span></span>
   2. <span data-ttu-id="44244-330">Kattintson a **hozzon létre egy új jelentéskészítő kiszolgáló adatbázisában** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="44244-330">Click **Create a new report server database** and then click **Next**.</span></span>
   3. <span data-ttu-id="44244-331">Hagyja meg az alapértelmezett **kiszolgálónév**: neve legyen a virtuális Gépet, és hagyja meg az alapértelmezett **hitelesítési típus** , **aktuális felhasználó** – **integrált biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="44244-331">Leave the default **Server Name**: as the VM name and leave the default **Authentication Type** as **Current User** – **Integrated Security**.</span></span> <span data-ttu-id="44244-332">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="44244-332">Click **Next**.</span></span>
   4. <span data-ttu-id="44244-333">Hagyja meg az alapértelmezett **adatbázisnév** , **ReportServer** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="44244-333">Leave the default **Database Name** as **ReportServer** and click **Next**.</span></span>
   5. <span data-ttu-id="44244-334">Hagyja meg az alapértelmezett **hitelesítési típus** , **szolgáltatás hitelesítő adatai** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="44244-334">Leave the default **Authentication Type** as **Service Credentials** and click **Next**.</span></span>
   6. <span data-ttu-id="44244-335">Kattintson a **következő** a a **összegzés** lap.</span><span class="sxs-lookup"><span data-stu-id="44244-335">Click **Next** on the **Summary** page.</span></span>
   7. <span data-ttu-id="44244-336">A konfiguráció befejeztével kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="44244-336">When the configuration is complete, click **Finish**.</span></span>
8. <span data-ttu-id="44244-337">Kattintson a bal oldali ablaktáblában **Jelentéskezelő URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="44244-337">In the left pane, click **Report Manager URL**.</span></span> <span data-ttu-id="44244-338">Hagyja meg az alapértelmezett **virtuális könyvtár** , **jelentések** kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="44244-338">Leave the default **Virtual Directory** as **Reports** and click **Apply**.</span></span>
9. <span data-ttu-id="44244-339">Kattintson a **kilépési** a Reporting Services Configuration Manager bezárásához.</span><span class="sxs-lookup"><span data-stu-id="44244-339">Click **Exit** to close the Reporting Services Configuration Manager.</span></span>

## <a name="step-4-open-windows-firewall-port"></a><span data-ttu-id="44244-340">4. lépés: A Windows tűzfal Port megnyitása</span><span class="sxs-lookup"><span data-stu-id="44244-340">Step 4: Open Windows Firewall Port</span></span>
> [!NOTE]
> <span data-ttu-id="44244-341">Ha a parancsfájlok a jelentéskészítő kiszolgáló konfigurálása, ez a szakasz kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="44244-341">If you used one of the scripts to configure the report server, you can skip this section.</span></span> <span data-ttu-id="44244-342">A parancsfájl tartalmazza a lépés a tűzfal-port megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="44244-342">The script included a step to open the firewall port.</span></span> <span data-ttu-id="44244-343">Az alapértelmezett port a 80-as HTTP és HTTPS – 443-as port volt.</span><span class="sxs-lookup"><span data-stu-id="44244-343">The default was port 80 for HTTP and port 443 for HTTPS.</span></span>
> 
> 

<span data-ttu-id="44244-344">Távolról kapcsolódni a Jelentéskezelő vagy a jelentéskészítő kiszolgáló, a virtuális gépen, a TCP-végpontot a virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="44244-344">To connect remotely to Report Manager or the Report Server on the virtual machine, a TCP Endpoint is required on the VM.</span></span> <span data-ttu-id="44244-345">Szükség van rá ugyanazt a portot a virtuális gép megnyitása.</span><span class="sxs-lookup"><span data-stu-id="44244-345">It is required to open the same port in the VM’s firewall.</span></span> <span data-ttu-id="44244-346">A végpont jött létre, amikor a virtuális gép lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="44244-346">The endpoint was created when the VM was provisioned.</span></span>

<span data-ttu-id="44244-347">Ez a szakasz a tűzfal-port megnyitásának módjáról alapvető információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="44244-347">This section provides basic information on how to open the firewall port.</span></span> <span data-ttu-id="44244-348">További információkért lásd: [beállítani a tűzfalat a jelentéskészítő kiszolgáló elérése](https://technet.microsoft.com/library/bb934283.aspx)</span><span class="sxs-lookup"><span data-stu-id="44244-348">For more information, see [Configure a Firewall for Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)</span></span>

> [!NOTE]
> <span data-ttu-id="44244-349">Ha a parancsfájl a jelentéskészítő kiszolgáló konfigurálására szolgáló, ez a szakasz kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="44244-349">If you used the script to configure the report server, you can skip this section.</span></span> <span data-ttu-id="44244-350">A parancsfájl tartalmazza a lépés a tűzfal-port megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="44244-350">The script included a step to open the firewall port.</span></span>
> 
> 

<span data-ttu-id="44244-351">Ha a magánhálózati port a HTTPS használatára konfigurált 443-astól eltérő, annak megfelelően módosítsa a következő parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="44244-351">If you configured a private port for HTTPS other than 443, modify the following script appropriately.</span></span> <span data-ttu-id="44244-352">Port megnyitásához **443-as** a Windows tűzfalon, adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="44244-352">To open port **443** on the Windows Firewall, complete the following:</span></span>

1. <span data-ttu-id="44244-353">Nyissa meg a Windows PowerShell ablakot rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="44244-353">Open a Windows PowerShell window with administrative privileges.</span></span>
2. <span data-ttu-id="44244-354">Ha a 443-astól eltérő portot a HTTPS-végpont a virtuális Gépre való konfigurálásakor, frissíteni a következő parancsot a portot, és futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="44244-354">If you used a port other than 443 when you configured the HTTPS endpoint on the VM, update the port in the following command and then run the command:</span></span>
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. <span data-ttu-id="44244-355">A parancs befejeződésekor **Ok** jelenik meg a parancssort.</span><span class="sxs-lookup"><span data-stu-id="44244-355">When the command completes, **Ok** is displayed in the command prompt.</span></span>

<span data-ttu-id="44244-356">Győződjön meg arról, hogy a port van megnyitva, nyisson meg egy Windows PowerShell-ablakot, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="44244-356">To verify that the port is opened, open a Windows PowerShell window and run the following command:</span></span>

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-the-configuration"></a><span data-ttu-id="44244-357">A konfiguráció ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="44244-357">Verify the configuration</span></span>
<span data-ttu-id="44244-358">Győződjön meg arról, hogy a jelentés alapvető funkcióihoz most működik, nyissa meg a böngésző rendszergazdai jogosultságokkal, és tallózással keressen meg a következő jelentéskészítő kiszolgáló ad Jelentéskezelő URL-CÍMEK:</span><span class="sxs-lookup"><span data-stu-id="44244-358">To verify that the basic report server functionality is now working, open your browser with administrative privileges and then browse to the following report server ad report manager URLS:</span></span>

* <span data-ttu-id="44244-359">A virtuális Gépre keresse meg a jelentéskészítő kiszolgáló URL-címe:</span><span class="sxs-lookup"><span data-stu-id="44244-359">On the VM, browse to the report server URL:</span></span>
  
        http://localhost/reportserver
* <span data-ttu-id="44244-360">A virtuális Gépre keresse meg a jelentés Manager URL-címe:</span><span class="sxs-lookup"><span data-stu-id="44244-360">On the VM, browse to the report manger URL:</span></span>
  
        http://localhost/Reports
* <span data-ttu-id="44244-361">A helyi számítógépről, keresse meg a **távoli** Manager jelentést a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="44244-361">From your local computer, browse to the **remote** report Manager on the VM.</span></span> <span data-ttu-id="44244-362">Szükség szerint az alábbi példában a DNS-név frissítése.</span><span class="sxs-lookup"><span data-stu-id="44244-362">Update the DNS name in the following example as appropriate.</span></span> <span data-ttu-id="44244-363">Amikor a rendszer kéri a jelszót, akkor jön létre, amikor a virtuális gép lett kiépítve rendszergazdai hitelesítő adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="44244-363">When prompted for a password, use the administrator credentials you created when the VM was provisioned.</span></span> <span data-ttu-id="44244-364">A felhasználó nevét kell a [tartomány]\[felhasználónév] formátumban, ahol a tartomány pedig a virtuális gép számítógép nevét, például ssrsnativecloud\testuser.</span><span class="sxs-lookup"><span data-stu-id="44244-364">The user name is in the [Domain]\[user name] format, where the domain is the VM computer name, for example ssrsnativecloud\testuser.</span></span> <span data-ttu-id="44244-365">Ha nem használja a HTTP**S**, távolítsa el a **s** az URL-címben.</span><span class="sxs-lookup"><span data-stu-id="44244-365">If you are not using HTTP**S**, remove the **s** in the URL.</span></span> <span data-ttu-id="44244-366">Tekintse meg a következő szakaszban talál további felhasználók létrehozásával a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="44244-366">See the next section for information on creating additional users on VM.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/Reports
* <span data-ttu-id="44244-367">A helyi számítógépről keresse meg azt a távoli jelentéskészítő kiszolgáló URL-címe.</span><span class="sxs-lookup"><span data-stu-id="44244-367">From your local computer, browse to the remote report server URL.</span></span> <span data-ttu-id="44244-368">Szükség szerint az alábbi példában a DNS-név frissítése.</span><span class="sxs-lookup"><span data-stu-id="44244-368">Update the DNS name in the following example as appropriate.</span></span> <span data-ttu-id="44244-369">Ha HTTPS nem használ, távolítsa el az s az URL-címben.</span><span class="sxs-lookup"><span data-stu-id="44244-369">If you are not using HTTPS, remove the s in the URL.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a><span data-ttu-id="44244-370">Felhasználók létrehozása és szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="44244-370">Create Users and Assign Roles</span></span>
<span data-ttu-id="44244-371">Konfigurálása, és a jelentéskészítő kiszolgáló ellenőrzése után a gyakori felügyeleti feladatokat, hogy hozzon létre egy vagy több felhasználó és a Reporting Services szerepkörökhöz rendeljen hozzá felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="44244-371">After configuring and verifying the report server, a common administrative task is to create one or more users and assign users to Reporting Services roles.</span></span> <span data-ttu-id="44244-372">További információkért tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="44244-372">For more information, see the following:</span></span>

* [<span data-ttu-id="44244-373">Helyi felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="44244-373">Create a local user account</span></span>](https://technet.microsoft.com/library/cc770642.aspx)
* <span data-ttu-id="44244-374">[A jelentéskészítő kiszolgáló (Jelentéskezelő) GRANT felhasználói hozzáférése](https://msdn.microsoft.com/library/ms156034.aspx))</span><span class="sxs-lookup"><span data-stu-id="44244-374">[Grant User Access to a Report Server (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))</span></span>
* [<span data-ttu-id="44244-375">Létrehozása és kezelése a szerepkör-hozzárendelések</span><span class="sxs-lookup"><span data-stu-id="44244-375">Create and Manage Role Assignments</span></span>](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a><span data-ttu-id="44244-376">Hozzon létre, és a jelentések közzététele az Azure virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="44244-376">To Create and Publish Reports to the Azure Virtual Machine</span></span>
<span data-ttu-id="44244-377">A következő táblázat összefoglalja az egyes lehetőségekről a helyi számítógépről a jelentéskészítő kiszolgálón található meg a Microsoft Azure virtuális gép meglévő jelentések közzététele érdekében:</span><span class="sxs-lookup"><span data-stu-id="44244-377">The following table summarizes some of the options available to publish existing reports from an on-premises computer to the report server hosted on the Microsoft Azure Virtual Machine:</span></span>

* <span data-ttu-id="44244-378">**RS.exe parancsfájl**: használata RS.exe parancsfájl jelentés elemeit és a meglévő jelentéskészítő kiszolgáló számára a Microsoft Azure virtuális gép másolása.</span><span class="sxs-lookup"><span data-stu-id="44244-378">**RS.exe script**: Use RS.exe script to copy report items from and existing report server to your Microsoft Azure Virtual Machine.</span></span> <span data-ttu-id="44244-379">További információkért lásd: a "Natív mód natív üzemmódra – a Microsoft Azure virtuális gép" szakaszban a [minta Reporting Services rs.exe tartalom áttelepítése jelentés kiszolgálók közötti parancsfájl](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-379">For more information, see the section “Native mode to Native Mode – Microsoft Azure Virtual Machine” in [Sample Reporting Services rs.exe Script to Migrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>
* <span data-ttu-id="44244-380">**Jelentéskészítő**: A virtuális gépet tartalmaz, kattintson a-Microsoft SQL Server jelentéskészítő egyszer verzióját.</span><span class="sxs-lookup"><span data-stu-id="44244-380">**Report Builder**: The virtual machine includes the click-once version of Microsoft SQL Server Report Builder.</span></span> <span data-ttu-id="44244-381">Elindítja a jelentés jelentéskészítő első alkalommal a virtuális gépet:</span><span class="sxs-lookup"><span data-stu-id="44244-381">To start Report builder the first time on the virtual machine:</span></span>
  
  1. <span data-ttu-id="44244-382">A böngészőben rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="44244-382">Start your browser with administrative privileges.</span></span>
  2. <span data-ttu-id="44244-383">Keresse meg a Jelentéskezelő virtuális gépen, és kattintson a **jelentéskészítő** a szalagon.</span><span class="sxs-lookup"><span data-stu-id="44244-383">Browse to report manager on the virtual machine and click **Report Builder**  in the ribbon.</span></span>
     
     <span data-ttu-id="44244-384">További információkért lásd: [telepítése, eltávolítása és a jelentéskészítő támogató](https://technet.microsoft.com/library/dd207038.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-384">For more information, see [Installing, Uninstalling, and Supporting Report Builder](https://technet.microsoft.com/library/dd207038.aspx).</span></span>
* <span data-ttu-id="44244-385">**SQL Server Data Tools: Virtuális gép**: Ha a virtuális gép az SQL Server 2012-ben létrehozott, akkor az SQL Server Data Tools a virtuális gépre van telepítve, és segítségével hozzon létre **jelentés Server projektek** és a jelentések a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="44244-385">**SQL Server Data Tools: VM**:  If you created the VM with SQL Server 2012, then SQL Server Data Tools is installed on the virtual machine and can be used to create **Report Server Projects** and reports on the virtual machine.</span></span> <span data-ttu-id="44244-386">SQL Server Data Tools közzéteheti a jelentéseket a jelentéskészítő kiszolgáló, a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="44244-386">SQL Server Data Tools can publish the reports to the report server on the virtual machine.</span></span>
  
    <span data-ttu-id="44244-387">Ha a virtuális gép az SQL server 2014 hozta létre, telepítheti az SQL Server Data Tools - BI visual Studio.</span><span class="sxs-lookup"><span data-stu-id="44244-387">If you created the VM with SQL server 2014, you can install SQL Server Data Tools- BI for visual Studio.</span></span> <span data-ttu-id="44244-388">További információkért tekintse át a következőket:</span><span class="sxs-lookup"><span data-stu-id="44244-388">For more information, see the following:</span></span>
  
  * [<span data-ttu-id="44244-389">Microsoft SQL Server Data Tools összetevővel - üzleti intelligencia, a Visual Studio 2013-hoz</span><span class="sxs-lookup"><span data-stu-id="44244-389">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013</span></span>](https://www.microsoft.com/download/details.aspx?id=42313)
  * [<span data-ttu-id="44244-390">Microsoft SQL Server Data Tools összetevővel - üzleti intelligencia, a Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="44244-390">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=36843)
  * [<span data-ttu-id="44244-391">SQL Server Data Tools összetevővel és az SQL Server Business Intelligence (SSDT-bA)</span><span class="sxs-lookup"><span data-stu-id="44244-391">SQL Server Data Tools and SQL Server Business Intelligence (SSDT-BI)</span></span>](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* <span data-ttu-id="44244-392">**SQL Server Data Tools: Távoli**: A helyi számítógépen, a Reporting Services-projekt létrehozása az SQL Server Data Tools összetevővel, amely tartalmazza a Reporting Services-jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="44244-392">**SQL Server Data Tools: Remote**:  On your local computer, create a Reporting Services project in SQL Server Data Tools that contains Reporting Services reports.</span></span> <span data-ttu-id="44244-393">A webes szolgáltatás URL-Címéhez való kapcsolódáshoz a projekt konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="44244-393">Configure the project to connect to the web service URL.</span></span>
  
    ![SSRS-projekt SSDT projektjének tulajdonságai](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* <span data-ttu-id="44244-395">**Parancsfájl használata**: parancsfájl segítségével másolja a jelentéskészítő kiszolgáló tartalmat.</span><span class="sxs-lookup"><span data-stu-id="44244-395">**Use script**: Use script to copy report server content.</span></span> <span data-ttu-id="44244-396">További információkért lásd: [minta Reporting Services rs.exe tartalom áttelepítése jelentés kiszolgálók közötti parancsfájl](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-396">For more information, see [Sample Reporting Services rs.exe Script to Migrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>

## <a name="minimize-cost-if-you-are-not-using-the-vm"></a><span data-ttu-id="44244-397">Ha nem használ a virtuális gép minimalizálása érdekében a költség</span><span class="sxs-lookup"><span data-stu-id="44244-397">Minimize cost if you are not using the VM</span></span>
> [!NOTE]
> <span data-ttu-id="44244-398">Költségek minimalizálása érdekében az az Azure virtuális gépek Ha nincsenek használatban, állítsa le a virtuális Gépet a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="44244-398">To minimize charges for your Azure Virtual Machines when not in use, shut down the VM from the Azure classic portal.</span></span> <span data-ttu-id="44244-399">A Windows energiagazdálkodási beállításait használja a virtuális Gépen belül a virtuális gép leállítása, ha még mindig van szó akkora a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="44244-399">If you use the Windows power options inside a VM to shut down the VM, you are still charged the same amount for the VM.</span></span> <span data-ttu-id="44244-400">Költségek csökkentése érdekében le a virtuális Gépet, a klasszikus Azure portálon kell.</span><span class="sxs-lookup"><span data-stu-id="44244-400">To reduce charges, you need to shut down the VM in the Azure classic portal.</span></span> <span data-ttu-id="44244-401">Ha már nincs szüksége a virtuális Gépet, ne felejtse el a virtuális gép és a hozzárendelt .vhd fájlokat tároló díjak elkerülése érdekében törölje. További információkért lásd: a következő gyakran feltett [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="44244-401">If you no longer need the VM, remember to delete the VM and the associated .vhd files to avoid storage charges.For more information, see the FAQ section at [Virtual Machines Pricing Details](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>

## <a name="more-information"></a><span data-ttu-id="44244-402">További információ</span><span class="sxs-lookup"><span data-stu-id="44244-402">More Information</span></span>
### <a name="resources"></a><span data-ttu-id="44244-403">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="44244-403">Resources</span></span>
* <span data-ttu-id="44244-404">Az SQL Server Business Intelligence és a SharePoint 2013 egyetlen kiszolgáló telepítéséhez kapcsolódó hasonló tartalomhoz, lásd: [a Windows PowerShell szolgáltatás használatával hozzon létre egy Azure virtuális gép az SQL Server BI és a SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-404">For similar content related to a single server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Use Windows PowerShell to Create an Azure VM With SQL Server BI and SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span></span>
* <span data-ttu-id="44244-405">A hasonló tartalomhoz kapcsolódó SQL Server Business Intelligence és a SharePoint 2013 többkiszolgálós telepítésben, lásd: [központi telepítése az SQL Server Business Intelligence Azure virtuális gépek](https://msdn.microsoft.com/library/dn321998.aspx).</span><span class="sxs-lookup"><span data-stu-id="44244-405">For similar content related to a multi-server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Deploy SQL Server Business Intelligence in Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).</span></span>
* <span data-ttu-id="44244-406">A SQL Server Business Intelligence Azure virtuális gépek központi telepítéséhez kapcsolódó általános információkért lásd: [SQL Server Business Intelligence Azure virtuális gépek](virtual-machines-windows-classic-ps-sql-bi.md).</span><span class="sxs-lookup"><span data-stu-id="44244-406">For General information related to deployments of SQL Server Business Intelligence in Azure Virtual Machines, see [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).</span></span>
* <span data-ttu-id="44244-407">Az Azure számítási díjakat költsége kapcsolatos további információkért lásd: a virtuális gépek lapján [Azure árképzési Számológép](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="44244-407">For more information about the cost of Azure compute charges, see the Virtual Machines tab of [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span></span>

### <a name="community-content"></a><span data-ttu-id="44244-408">Közösségi tartalom</span><span class="sxs-lookup"><span data-stu-id="44244-408">Community Content</span></span>
* <span data-ttu-id="44244-409">A jelentéskészítési szolgáltatások natív mód jelentéskészítő kiszolgáló létrehozásával parancsfájl használata nélkül részletes utasításokért lásd: [üzemeltető SQL jelentési szolgáltatás az Azure virtuális gép](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span><span class="sxs-lookup"><span data-stu-id="44244-409">For step by step instructions on how to create a Reporting Services Native mode report server without using script, see [Hosting SQL Reporting Service on Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span></span>

### <a name="links-to-other-resources-for-sql-server-in-azure-vms"></a><span data-ttu-id="44244-410">Az SQL Server Azure virtuális gépeken egyéb forrásokra mutató hivatkozások</span><span class="sxs-lookup"><span data-stu-id="44244-410">Links to other resources for SQL Server in Azure VMs</span></span>
[<span data-ttu-id="44244-411">SQL Server Azure virtuális gépek – áttekintés</span><span class="sxs-lookup"><span data-stu-id="44244-411">SQL Server on Azure Virtual Machines Overview</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

