---
title: "aaaUse PowerShell tooCreate VM rendelkező egy natív mód jelentéskiszolgáló |} Microsoft Docs"
description: "Ez a témakör ismerteti, és bemutatja, hogyan hello központi telepítése és konfigurálása az SQL Server Reporting Services natív üzemmódú jelentéskészítő kiszolgáló egy Azure virtuális gépen. "
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
ms.openlocfilehash: e7791199c87dff106132f1535da12de40a8dbc9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-vm-with-a-native-mode-report-server"></a><span data-ttu-id="c1108-103">Használjon PowerShell tooCreate egy Azure virtuális gép, egy natív mód jelentéskészítő kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="c1108-103">Use PowerShell tooCreate an Azure VM With a Native Mode Report Server</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="c1108-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c1108-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c1108-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="c1108-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="c1108-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="c1108-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="c1108-107">Ez a témakör ismerteti, és bemutatja, hogyan hello központi telepítése és konfigurálása az SQL Server Reporting Services natív üzemmódú jelentéskészítő kiszolgáló egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c1108-107">This topic describes and walks you through hello deployment and configuration of a SQL Server Reporting Services native mode report server in an Azure Virtual Machine.</span></span> <span data-ttu-id="c1108-108">hello vonatkozó lépéseit a dokumentum használatban manuális toocreate hello virtuális gép és a Windows PowerShell-parancsfájl tooconfigure Reporting Services hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c1108-108">hello steps in this document use a combination of manual steps toocreate hello virtual machine and a Windows PowerShell script tooconfigure Reporting Services on hello VM.</span></span> <span data-ttu-id="c1108-109">hello konfigurációs parancsfájl tartalmazza a HTTP és HTTPs a tűzfal port megnyitása.</span><span class="sxs-lookup"><span data-stu-id="c1108-109">hello configuration script includes opening a firewall port for HTTP or HTTPs.</span></span>

> [!NOTE]
> <span data-ttu-id="c1108-110">Ha nincs szüksége **HTTPS** hello jelentéskészítő kiszolgálón **a 2**.</span><span class="sxs-lookup"><span data-stu-id="c1108-110">If you do not require **HTTPS** on hello report server, **skip step 2**.</span></span>
> 
> <span data-ttu-id="c1108-111">Miután létrehozta a virtuális gép hello az 1. lépésben, lépjen a toohello szakasz használata parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1108-111">After creating hello VM in step 1, go toohello section Use script tooconfigure hello report server and HTTP.</span></span> <span data-ttu-id="c1108-112">Hello parancsfájl futtatása után a hello jelentéskészítő kiszolgáló készen áll a toouse.</span><span class="sxs-lookup"><span data-stu-id="c1108-112">After you run hello script, hello report server is ready toouse.</span></span>

## <a name="prerequisites-and-assumptions"></a><span data-ttu-id="c1108-113">Előfeltételek és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c1108-113">Prerequisites and Assumptions</span></span>
* <span data-ttu-id="c1108-114">**Azure-előfizetés**: Ellenőrizze az Azure-előfizetésben elérhető magok hello száma.</span><span class="sxs-lookup"><span data-stu-id="c1108-114">**Azure Subscription**: Verify hello number of cores available in your Azure Subscription.</span></span> <span data-ttu-id="c1108-115">Ha hoz létre a Virtuálisgép-méretet ajánlott hello **A3**, kell **4** rendelkezésre álló magot.</span><span class="sxs-lookup"><span data-stu-id="c1108-115">If you create hello recommended VM size of **A3**, you need **4** available cores.</span></span> <span data-ttu-id="c1108-116">Ha egy Virtuálisgép-méretet **A2**, kell **2** rendelkezésre álló magot.</span><span class="sxs-lookup"><span data-stu-id="c1108-116">If you use a VM size of **A2**, you need **2** available cores.</span></span>
  
  * <span data-ttu-id="c1108-117">tooverify hello core korlátot az előfizetéséhez, a klasszikus Azure-portál hello-beállítások gombra a hello bal oldali ablaktáblán, majd kattintson a használati hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="c1108-117">tooverify hello core limit of your subscription, in hello Azure classic portal, click SETTINGS in hello left pane and then Click USAGE in hello top menu.</span></span>
  * <span data-ttu-id="c1108-118">tooincrease hello core kvóta, kapcsolattartási [Azure támogatási](https://azure.microsoft.com/support/options/).</span><span class="sxs-lookup"><span data-stu-id="c1108-118">tooincrease hello core quota, contact [Azure Support](https://azure.microsoft.com/support/options/).</span></span> <span data-ttu-id="c1108-119">Virtuálisgép-méret információkért lásd: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1108-119">For VM size information, see [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="c1108-120">**A Windows PowerShell-Parancsprogramokról**: hello a témakör feltételezi, hogy rendelkezik-e a Windows PowerShell alapszintű ismeretét.</span><span class="sxs-lookup"><span data-stu-id="c1108-120">**Windows PowerShell Scripting**: hello topic assumes that you have a basic working knowledge of Windows PowerShell.</span></span> <span data-ttu-id="c1108-121">A Windows PowerShell használatával kapcsolatos további információkért tekintse meg a hello következőt:</span><span class="sxs-lookup"><span data-stu-id="c1108-121">For more information about using Windows PowerShell, see hello following:</span></span>
  
  * [<span data-ttu-id="c1108-122">A Windows PowerShell indítása a Windows Server</span><span class="sxs-lookup"><span data-stu-id="c1108-122">Starting Windows PowerShell on Windows Server</span></span>](https://technet.microsoft.com/library/hh847814.aspx)
  * [<span data-ttu-id="c1108-123">Bevezetés a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="c1108-123">Getting Started with Windows PowerShell</span></span>](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a><span data-ttu-id="c1108-124">1. lépés: Az Azure virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="c1108-124">Step 1: Provision an Azure Virtual Machine</span></span>
1. <span data-ttu-id="c1108-125">Keresse meg a klasszikus Azure portálon toohello.</span><span class="sxs-lookup"><span data-stu-id="c1108-125">Browse toohello Azure classic portal.</span></span>
2. <span data-ttu-id="c1108-126">Kattintson a **virtuális gépek** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="c1108-126">Click **Virtual Machines** in hello left pane.</span></span>
   
    ![a Microsoft azure virtuális gépek](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. <span data-ttu-id="c1108-128">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="c1108-128">Click **New**.</span></span>
   
    ![Új gomb](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. <span data-ttu-id="c1108-130">Kattintson a **gyűjteményből**.</span><span class="sxs-lookup"><span data-stu-id="c1108-130">Click **From Gallery**.</span></span>
   
    ![új virtuális gép gyűjteményből](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. <span data-ttu-id="c1108-132">Kattintson a **SQL Server 2014 RTM Standard – Windows Server 2012 R2** és kattintson a nyílra toocontinue hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-132">Click **SQL Server 2014 RTM Standard – Windows Server 2012 R2** and then click hello arrow toocontinue.</span></span>
   
    ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    <span data-ttu-id="c1108-134">Ha hello Reporting Services adatvezérelt előfizetések funkció van szüksége, válassza a **SQL Server 2014 RTM vállalati – Windows Server 2012 R2**.</span><span class="sxs-lookup"><span data-stu-id="c1108-134">If you need hello Reporting Services data driven subscriptions feature, choose **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**.</span></span> <span data-ttu-id="c1108-135">SQL Server kiadása és által nyújtott szolgáltatások támogatásáról további információkért lásd: [hello kiadás az SQL Server 2012 által támogatott funkciók](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span><span class="sxs-lookup"><span data-stu-id="c1108-135">For more information on SQL Server editions and feature support, see [Features Supported by hello Editions of SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).</span></span>
6. <span data-ttu-id="c1108-136">A hello **virtuálisgép-konfiguráció** lapon, a következő mezők hello szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="c1108-136">On hello **Virtual machine configuration** page, edit hello following fields:</span></span>
   
   * <span data-ttu-id="c1108-137">Ha egynél több **VERZIÓJÚ kiadási dátum**, válassza ki a legújabb verziót hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-137">If there is more than one **VERSION RELEASE DATE**, select hello most recent version.</span></span>
   * <span data-ttu-id="c1108-138">**Virtuális gép neve**: hello számítógép nevét is használatos hello tovább konfigurálása lapon hello alapértelmezett felhőalapú szolgáltatás DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="c1108-138">**Virtual Machine Name**: hello machine name is also used on hello next configuration page as hello default Cloud Service DNS name.</span></span> <span data-ttu-id="c1108-139">hello DNS nevének egyedinek kell lennie a hello Azure szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="c1108-139">hello DNS name must be unique across hello Azure service.</span></span> <span data-ttu-id="c1108-140">Fontolja meg, hogy hello VM nevű számítógép, amely leírja, milyen hello VM szolgál.</span><span class="sxs-lookup"><span data-stu-id="c1108-140">Consider configuring hello VM with a computer name that describes what hello VM is used for.</span></span> <span data-ttu-id="c1108-141">Például ssrsnativecloud.</span><span class="sxs-lookup"><span data-stu-id="c1108-141">For example ssrsnativecloud.</span></span>
   * <span data-ttu-id="c1108-142">**Réteg**: Standard</span><span class="sxs-lookup"><span data-stu-id="c1108-142">**Tier**: Standard</span></span>
   * <span data-ttu-id="c1108-143">**Méret: A3** hello javasolt az SQL Server számítási feladatait a Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="c1108-143">**Size:A3** is hello recommended VM size for SQL Server workloads.</span></span> <span data-ttu-id="c1108-144">Ha egy virtuális gép csak egy jelentéskészítő kiszolgálón, a virtuális gép méretét A2 esetén elegendő hello jelentéskészítő kiszolgáló észlel a nagy munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="c1108-144">If a VM is only used as a report server, a VM size of A2 is sufficient unless hello report server experiences a large workload.</span></span> <span data-ttu-id="c1108-145">A virtuális gép a díjszabásról, lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="c1108-145">For VM pricing information, see [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>
   * <span data-ttu-id="c1108-146">**Új felhasználónevet**: hello-nevet a virtuális gép hello rendszergazdaként jön létre.</span><span class="sxs-lookup"><span data-stu-id="c1108-146">**New User Name**: hello name you provide is created as an administrator on hello VM.</span></span>
   * <span data-ttu-id="c1108-147">**Új jelszó** és **megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="c1108-147">**New Password** and **confirm**.</span></span> <span data-ttu-id="c1108-148">Hello új rendszergazdai fiókhoz használja ezt a jelszót, és erős jelszó használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="c1108-148">This password is used for hello new administrator account and it is recommended you use a strong password.</span></span>
   * <span data-ttu-id="c1108-149">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-149">Click **Next**.</span></span> <span data-ttu-id="c1108-150">![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span><span class="sxs-lookup"><span data-stu-id="c1108-150">![next](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)</span></span>
7. <span data-ttu-id="c1108-151">Hello következő lapon szerkessze a következő mezők hello:</span><span class="sxs-lookup"><span data-stu-id="c1108-151">On hello next page, edit hello following fields:</span></span>
   
   * <span data-ttu-id="c1108-152">**A felhőalapú szolgáltatás**: válasszon **hozzon létre egy új felhőalapú szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="c1108-152">**Cloud Service**: select **Create a new Cloud Service**.</span></span>
   * <span data-ttu-id="c1108-153">**A felhőalapú szolgáltatás DNS-név**: Ez az hello Felhőszolgáltatás, amely kapcsolódik a virtuális gép hello hello nyilvános DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="c1108-153">**Cloud Service DNS Name**: This is hello public DNS name of hello Cloud Service that is associated with hello VM.</span></span> <span data-ttu-id="c1108-154">hello alapértelmezett értéke hello virtuális gép nevét a megadott hello névhez.</span><span class="sxs-lookup"><span data-stu-id="c1108-154">hello default name is hello name you typed in for hello VM name.</span></span> <span data-ttu-id="c1108-155">Ha hello témakört a későbbi lépésekben hoz létre egy megbízható az SSL-tanúsítványt, és majd hello DNS-név használható hello hello értéke "**kiadott**" hello tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="c1108-155">If in later steps of hello topic, you create a trusted SSL certificate and then hello DNS name is used for hello value of hello “**Issued to**” of hello certificate.</span></span>
   * <span data-ttu-id="c1108-156">**Régió/affinitás csoport/virtuális hálózati**: válassza ki a hello régió legközelebbi tooyour végfelhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="c1108-156">**Region/Affinity Group/Virtual Network**: Choose hello region closest tooyour end users.</span></span>
   * <span data-ttu-id="c1108-157">**A Tárfiók**: egy automatikusan létrehozott tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="c1108-157">**Storage Account**: Use an automatically generated storage account.</span></span>
   * <span data-ttu-id="c1108-158">**A rendelkezésre állási csoport**: nincs.</span><span class="sxs-lookup"><span data-stu-id="c1108-158">**Availability Set**: None.</span></span>
   * <span data-ttu-id="c1108-159">**VÉGPONTOK** megtartása hello **távoli asztal** és **PowerShell** végpontok majd adja hozzá a HTTP vagy HTTPS-végpont a környezettől függően.</span><span class="sxs-lookup"><span data-stu-id="c1108-159">**ENDPOINTS** Keep hello **Remote Desktop** and **PowerShell** endpoints and then add either an HTTP or HTTPS endpoint, depending on your environment.</span></span>
     
     * <span data-ttu-id="c1108-160">**HTTP**: nyilvános és titkos portok alapértelmezett hello **80**.</span><span class="sxs-lookup"><span data-stu-id="c1108-160">**HTTP**: hello default public and private ports are **80**.</span></span> <span data-ttu-id="c1108-161">Vegye figyelembe, hogy ha egy magánhálózati port a 80-as, nem módosíthatja **$HTTPport = 80** hello http parancsfájlban.</span><span class="sxs-lookup"><span data-stu-id="c1108-161">Note that if you use a private port other than 80, modify **$HTTPport = 80** in hello http script.</span></span>
     * <span data-ttu-id="c1108-162">**HTTPS**: nyilvános és titkos portok alapértelmezett hello **443-as**.</span><span class="sxs-lookup"><span data-stu-id="c1108-162">**HTTPS**: hello default public and private ports are **443**.</span></span> <span data-ttu-id="c1108-163">Biztonsági szempontból ajánlott toochange hello magánhálózati port, és konfigurálja a tűzfal és hello jelentéskészítő kiszolgáló toouse hello magánhálózati port.</span><span class="sxs-lookup"><span data-stu-id="c1108-163">A security best practice is toochange hello private port and configure your firewall and hello report server toouse hello private port.</span></span> <span data-ttu-id="c1108-164">A végpontok további információkért lásd: [hogyan tooSet be egy virtuális gép kommunikációs](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1108-164">For more information on endpoints, see [How tooSet Up Communication with a Virtual Machine](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span> <span data-ttu-id="c1108-165">Vegye figyelembe, hogy egy 443,-astól eltérő port használata esetén módosítsa a hello paraméter **$HTTPsport = 443-as** a hello HTTPS parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c1108-165">Note that if you use a port other than 443, change hello parameter **$HTTPsport = 443** in hello HTTPS script.</span></span>
   * <span data-ttu-id="c1108-166">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-166">Click next .</span></span> ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. <span data-ttu-id="c1108-168">Hello hello varázsló utolsó lapján, tartsa hello alapértelmezett **hello Virtuálisgép-ügynök telepítéséhez** kijelölt.</span><span class="sxs-lookup"><span data-stu-id="c1108-168">On hello last page of hello wizard, keep hello default **Install hello VM agent** selected.</span></span> <span data-ttu-id="c1108-169">hello témakörben leírt lépések hasznosíthatja hello Virtuálisgép-ügynök, de ha tookeep a virtuális gép, Virtuálisgép-ügynök hello és bővítmények lehetővé teszi a tooenhance helykiszolgálójához CM.</span><span class="sxs-lookup"><span data-stu-id="c1108-169">hello steps in this topic do not utilize hello VM agent but if you plan tookeep this VM, hello VM agent and extensions will allow you tooenhance he CM.</span></span>  <span data-ttu-id="c1108-170">A Virtuálisgép-ügynök hello további információkért lásd: [ügynök és Virtuálisgép-bővítmények – 1. rész](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span><span class="sxs-lookup"><span data-stu-id="c1108-170">For more information on hello VM agent, see [VM Agent and Extensions – Part 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/).</span></span> <span data-ttu-id="c1108-171">Hello alapértelmezett telepített kiterjesztéseket ad futó egyik hello "BGINFO" bővítményt, amely hello virtuális gép asztalán jeleníti meg, a rendszer-információkat, például a belső IP- és szabad terület meghajtó.</span><span class="sxs-lookup"><span data-stu-id="c1108-171">One of hello default extensions installed ad running is hello “BGINFO” extension that displays on hello VM desktop, system information such as internal IP and free drive space.</span></span>
9. <span data-ttu-id="c1108-172">Kattintson a Kész gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-172">Click complete .</span></span> ![oké](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. <span data-ttu-id="c1108-174">Hello **állapota** a virtuális gép részére hello **indítása (kiépítés)** során hello kiépítési folyamat és értékként jelenik majd meg **futtató** hello virtuális gép esetén kiosztott és készen áll toouse.</span><span class="sxs-lookup"><span data-stu-id="c1108-174">hello **Status** of hello VM displays as **Starting (Provisioning)** during hello provision process and then displays as **Running** when hello VM is provisioned and ready toouse.</span></span>

## <a name="step-2-create-a-server-certificate"></a><span data-ttu-id="c1108-175">2. lépés: A kiszolgálói tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1108-175">Step 2: Create a Server Certificate</span></span>
> [!NOTE]
> <span data-ttu-id="c1108-176">Ha nincs szüksége HTTPS hello jelentéskészítő kiszolgálón, akkor **a 2** toohello szakaszt, és **parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTP használata**.</span><span class="sxs-lookup"><span data-stu-id="c1108-176">If you do not require HTTPS on hello report server, you can **skip step 2** and go toohello section **Use script tooconfigure hello report server and HTTP**.</span></span> <span data-ttu-id="c1108-177">Használjon hello HTTP parancsfájl tooquickly hello jelentéskészítő kiszolgáló és a kiszolgáló készen áll a toouse lesz hello jelentés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c1108-177">Use hello HTTP script tooquickly configure hello report server and hello report server will be ready toouse.</span></span>

<span data-ttu-id="c1108-178">Rendelés toouse HTTPS hello VM kell egy megbízható az SSL-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c1108-178">In order toouse HTTPS on hello VM, you need a trusted SSL certificate.</span></span> <span data-ttu-id="c1108-179">A forgatókönyvtől függően hello a következő két módszer egyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="c1108-179">Depending on your scenario, you can use one of hello following two methods:</span></span>

* <span data-ttu-id="c1108-180">Érvényes SSL-tanúsítvány kiadott által hitelesítésszolgáltató (CA), és a Microsoft megbízhatónak.</span><span class="sxs-lookup"><span data-stu-id="c1108-180">A valid SSL certificate issued by a Certification Authority (CA) and trusted by Microsoft.</span></span> <span data-ttu-id="c1108-181">hello hitelesítésszolgáltató legfelső szintű tanúsítványok hello Microsoft Root Certificate programon keresztül terjesztett szükséges toobe.</span><span class="sxs-lookup"><span data-stu-id="c1108-181">hello CA root certificates are required toobe distributed via hello Microsoft Root Certificate Program.</span></span> <span data-ttu-id="c1108-182">A programra vonatkozó további információkért lásd: [Windows és Windows Phone 8 SSL Root Certificate Program (tag CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) és [bemutatása toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-182">For more information about this program, see [Windows and Windows Phone 8 SSL Root Certificate Program (Member CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) and [Introduction toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).</span></span>
* <span data-ttu-id="c1108-183">Egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c1108-183">A self-signed certificate.</span></span> <span data-ttu-id="c1108-184">Önaláírt tanúsítványok éles környezetekben nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="c1108-184">Self-signed certificates are not recommended for production environments.</span></span>

### <a name="toouse-a-certificate-created-by-a-trusted-certificate-authority-ca"></a><span data-ttu-id="c1108-185">toouse létrehozni egy tanúsítványt egy megbízható tanúsítvány hitelesítésszolgáltatói (CA)</span><span class="sxs-lookup"><span data-stu-id="c1108-185">toouse a certificate created by a trusted Certificate Authority (CA)</span></span>
1. <span data-ttu-id="c1108-186">**Hello webhely kiszolgálói tanúsítványt kérhet egy hitelesítésszolgáltatótól**.</span><span class="sxs-lookup"><span data-stu-id="c1108-186">**Request a server certificate for hello website from a certification authority**.</span></span> 
   
    <span data-ttu-id="c1108-187">Használhat kiszolgálói tanúsítvány varázsló hello vagy toogenerate kérelem Tanúsítványfájl (Certreq.txt), hogy küldjön tooa hitelesítésszolgáltató vagy toogenerate egy online hitelesítésszolgáltatót kérelmet.</span><span class="sxs-lookup"><span data-stu-id="c1108-187">You can use hello Web Server Certificate Wizard either toogenerate a certificate request file (Certreq.txt) that you send tooa certification authority, or toogenerate a request for an online certification authority.</span></span> <span data-ttu-id="c1108-188">Például a Microsoft tanúsítványszolgáltatások újdonságai a Windows Server 2012-ben.</span><span class="sxs-lookup"><span data-stu-id="c1108-188">For example, Microsoft Certificate Services in Windows Server 2012.</span></span> <span data-ttu-id="c1108-189">Attól függően, hogy a kiszolgálói tanúsítvány által nyújtott azonosítás biztonsági szint hello hello certification authority tooapprove néhány napon tooseveral hónapig a kérést, és küldjön egy tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="c1108-189">Depending on hello level of identification assurance offered by your server certificate, it is several days tooseveral months for hello certification authority tooapprove your request and send you a certificate file.</span></span> 
   
    <span data-ttu-id="c1108-190">A kiszolgálói tanúsítványok kérésével kapcsolatos további információkért tekintse meg a hello következőt:</span><span class="sxs-lookup"><span data-stu-id="c1108-190">For more information about requesting a server certificates, see hello following:</span></span> 
   
   * <span data-ttu-id="c1108-191">Használjon [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-191">Use [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).</span></span>
   * <span data-ttu-id="c1108-192">Biztonsági eszközök tooAdminister Windows Server 2012-ben.</span><span class="sxs-lookup"><span data-stu-id="c1108-192">Security Tools tooAdminister Windows Server 2012.</span></span>
     
     [<span data-ttu-id="c1108-193">Biztonsági eszközök tooAdminister Windows Server 2012-ben</span><span class="sxs-lookup"><span data-stu-id="c1108-193">Security Tools tooAdminister Windows Server 2012</span></span>](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > <span data-ttu-id="c1108-194">Hello **kiadott** hello mezője megbízható az SSL-tanúsítványt kell kell hello ugyanaz, mint hello **felhőalapú szolgáltatás DNS-név** meg használt hello új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c1108-194">hello **issued to** field of hello trusted SSL certificate should be hello same as hello **Cloud Service DNS NAME** you used for hello new VM.</span></span>

2. <span data-ttu-id="c1108-195">**Hello kiszolgálótanúsítvány telepítésének hello webkiszolgálón**.</span><span class="sxs-lookup"><span data-stu-id="c1108-195">**Install hello server certificate on hello Web server**.</span></span> <span data-ttu-id="c1108-196">hello webkiszolgáló a jelen esetben ez hello VM állomások hello jelentéskészítő kiszolgáló és a hello webhely a későbbi lépésekben jön létre, amikor a Reporting Services konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c1108-196">hello Web server in this case is hello VM that hosts hello report server and hello website is created in later steps when you configure Reporting Services.</span></span> <span data-ttu-id="c1108-197">Hello kiszolgálói tanúsítvány telepítésével hello webkiszolgálón hello tanúsítvány MMC beépülő modul használatával kapcsolatos további információkért lásd: [a kiszolgálótanúsítvány telepítésének](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="c1108-197">For more information about installing hello server certificate on hello Web server by using hello Certificate MMC snap-in, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
    <span data-ttu-id="c1108-198">Ha azt szeretné, hogy toouse hello parancsprogramja ebben a témakörben tooconfigure hello jelentéskészítő kiszolgáló hello hello tanúsítványok értékének **ujjlenyomat** szükséges hello parancsfájl paraméterként.</span><span class="sxs-lookup"><span data-stu-id="c1108-198">If you want toouse hello script included with this topic, tooconfigure hello report server, hello value of hello certificates **Thumbprint** is required as a parameter of hello script.</span></span> <span data-ttu-id="c1108-199">Hello részletei a következő szakaszban látható hogyan tooobtain hello hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="c1108-199">See hello next section for details on how tooobtain hello thumbprint of hello certificate.</span></span>
3. <span data-ttu-id="c1108-200">Rendelje hozzá a hello kiszolgálói tanúsítvány toohello jelentéskészítő kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c1108-200">Assign hello server certificate toohello report server.</span></span> <span data-ttu-id="c1108-201">hello hozzárendelés hello jelentéskészítő kiszolgáló konfigurálásakor hello a következő szakaszban befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c1108-201">hello assignment is completed in hello next section when you configure hello report server.</span></span>

### <a name="toouse-hello-virtual-machines-self-signed-certificate"></a><span data-ttu-id="c1108-202">Virtuális gépek önaláírt tanúsítvány toouse hello</span><span class="sxs-lookup"><span data-stu-id="c1108-202">toouse hello Virtual Machines Self-signed Certificate</span></span>
<span data-ttu-id="c1108-203">Egy önaláírt tanúsítványt a virtuális gép hello jött létre, amikor a virtuális gép hello lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="c1108-203">A self-signed certificate was created on hello VM when hello VM was provisioned.</span></span> <span data-ttu-id="c1108-204">a tanúsítványnak hello hello azonos nevet, amint hello VM DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="c1108-204">hello certificate has hello same name as hello VM DNS name.</span></span> <span data-ttu-id="c1108-205">Az order tooavoid tanúsítvány hibákat kell hello tanúsítvány megbízható-e a virtuális gépért hello és minden felhasználó hello hely.</span><span class="sxs-lookup"><span data-stu-id="c1108-205">In order tooavoid certificate errors, it is required that hello certificate is trusted on hello VM itself, and also by all users of hello site.</span></span>

1. <span data-ttu-id="c1108-206">tootrust hello legfelső szintű hitelesítésszolgáltató hello tanúsítvány a helyi virtuális gép, hello hozzáadása hello tanúsítvány toohello **megbízható legfelső szintű hitelesítésszolgáltatók**.</span><span class="sxs-lookup"><span data-stu-id="c1108-206">tootrust hello root CA of hello certificate on hello Local VM, add hello certificate toohello **Trusted Root Certification Authorities**.</span></span> <span data-ttu-id="c1108-207">hello hello szükséges lépések összefoglalása látható.</span><span class="sxs-lookup"><span data-stu-id="c1108-207">hello following is a summary of hello steps required.</span></span> <span data-ttu-id="c1108-208">Hogyan tootrust hello hitelesítésszolgáltató részletes lépéseiért lásd: [a kiszolgálótanúsítvány telepítésének](https://technet.microsoft.com/library/cc740068).</span><span class="sxs-lookup"><span data-stu-id="c1108-208">For detailed steps on how tootrust hello CA, see [Install a Server Certificate](https://technet.microsoft.com/library/cc740068).</span></span>
   
   1. <span data-ttu-id="c1108-209">Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-209">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="c1108-210">A böngésző konfigurációtól függően előfordulhat felszólító toosave csatlakozni a virtuális gép toohello .rdp fájlt.</span><span class="sxs-lookup"><span data-stu-id="c1108-210">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
      
       ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="c1108-212">Hello VM felhasználónév, a felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használja.</span><span class="sxs-lookup"><span data-stu-id="c1108-212">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
      
       <span data-ttu-id="c1108-213">Például a következő kép hello, hello virtuális gép neve: **ssrsnativecloud** és hello felhasználónév **tesztfelhasználó néven**.</span><span class="sxs-lookup"><span data-stu-id="c1108-213">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
      
       ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. <span data-ttu-id="c1108-215">Futtassa a mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="c1108-215">Run mmc.exe.</span></span> <span data-ttu-id="c1108-216">További információkért lásd: [hogyan: nézet tanúsítványok beépülő MMC-modulban hello](https://msdn.microsoft.com/library/ms788967.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-216">For more information, see [How to: View Certificates with hello MMC Snap-in](https://msdn.microsoft.com/library/ms788967.aspx).</span></span>
   3. <span data-ttu-id="c1108-217">Hello Konzolalkalmazás **fájl** menü hello hozzáadása **tanúsítványok** beépülő modulban válassza **számítógépfiók** kéri, majd **tovább**.</span><span class="sxs-lookup"><span data-stu-id="c1108-217">In hello console application **File** menu, add hello **Certificates** snap-in, select **Computer Account** when prompted, and then click **Next**.</span></span>
   4. <span data-ttu-id="c1108-218">Válassza ki **helyi számítógép** toomanage majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="c1108-218">Select **Local Computer** toomanage and then click **Finish**.</span></span>
   5. <span data-ttu-id="c1108-219">Kattintson a **Ok** majd bontsa ki a hello **tanúsítványok - személyes** csomópont, és kattintson **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="c1108-219">Click **Ok** and then expand hello **Certificates -Personal** nodes and then click **Certificates**.</span></span> <span data-ttu-id="c1108-220">hello tanúsítvány neve után hello VM hello DNS-nevét és végződik **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="c1108-220">hello certificate is named after hello DNS name of hello VM and ends with **cloudapp.net**.</span></span> <span data-ttu-id="c1108-221">Kattintson a jobb gombbal a hello tanúsítványnevet, és kattintson a **másolási**.</span><span class="sxs-lookup"><span data-stu-id="c1108-221">Right-click hello certificate name and click **Copy**.</span></span>
   6. <span data-ttu-id="c1108-222">Bontsa ki a hello **megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, és kattintson rá jobb gombbal **tanúsítványok** majd **Beillesztés**.</span><span class="sxs-lookup"><span data-stu-id="c1108-222">Expand hello **Trusted Root Certification Authorities** node and then right-click **Certificates** and then click **Paste**.</span></span>
   7. <span data-ttu-id="c1108-223">dupla toovalidate kattintson hello tanúsítvány neve alatt a **megbízható legfelső szintű hitelesítésszolgáltatók** , és ellenőrizze, hogy nincsenek hibák, és a tanúsítvány látható.</span><span class="sxs-lookup"><span data-stu-id="c1108-223">toovalidate, double click on hello certificate name under **Trusted Root Certification Authorities** and verify that there are no errors and you see your certificate.</span></span> <span data-ttu-id="c1108-224">Ha azt szeretné, hogy toouse hello HTTPS-parancsprogramja ebben a témakörben tooconfigure hello jelentéskészítő kiszolgáló hello hello tanúsítványok értékének **ujjlenyomat** szükséges hello parancsfájl paraméterként.</span><span class="sxs-lookup"><span data-stu-id="c1108-224">If you want toouse hello HTTPS script included with this topic, tooconfigure hello report server, hello value of hello certificates **Thumbprint** is required as a parameter of hello script.</span></span> <span data-ttu-id="c1108-225">**tooget hello ujjlenyomat értékének**, végezze el a következő hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-225">**tooget hello thumbprint value**, complete hello following.</span></span> <span data-ttu-id="c1108-226">Van még egy PowerShell minta tooretrieve hello ujjlenyomatot szakaszban [parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTPS használatára](#use-script-to-configure-the-report-server-and-HTTPS).</span><span class="sxs-lookup"><span data-stu-id="c1108-226">There is also a PowerShell sample tooretrieve hello thumbprint in section [Use script tooconfigure hello report server and HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).</span></span>
      
      1. <span data-ttu-id="c1108-227">Kattintson duplán a hello hello tanúsítvány nevével, például ssrsnativecloud.cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="c1108-227">Double-click hello name of hello certificate, for example ssrsnativecloud.cloudapp.net.</span></span>
      2. <span data-ttu-id="c1108-228">Kattintson a hello **részletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="c1108-228">Click hello **Details** tab.</span></span>
      3. <span data-ttu-id="c1108-229">Kattintson a **ujjlenyomat**.</span><span class="sxs-lookup"><span data-stu-id="c1108-229">Click **Thumbprint**.</span></span> <span data-ttu-id="c1108-230">hello ujjlenyomat hello érték jelenik meg hello részleteket tartalmazó mező, például a6 08 3 c. df f9 0b f7 e3 7c 25 kell adnia végrehajtási adatokat a4 kell adnia végrehajtási adatokat 7e ac 91 9c 2c fb 2f.</span><span class="sxs-lookup"><span data-stu-id="c1108-230">hello value of hello thumbprint is displayed in hello details field, for example ‎a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.</span></span>
      4. <span data-ttu-id="c1108-231">Hello ujjlenyomat másolása és mentése hello értéket későbbi használatra, vagy most hello parancsfájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="c1108-231">Copy hello thumbprint and save hello value for later or edit hello script now.</span></span>
      5. <span data-ttu-id="c1108-232">(*) Hello parancsfájl futtatása előtt távolítsa el a hello szóközöket Between hello értékpárok.</span><span class="sxs-lookup"><span data-stu-id="c1108-232">(*) Before you run hello script, remove hello spaces in between hello pairs of values.</span></span> <span data-ttu-id="c1108-233">Például előtt feljegyzett hello ujjlenyomat most lenne a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span><span class="sxs-lookup"><span data-stu-id="c1108-233">For example hello thumbprint noted before would now be ‎a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.</span></span>
      6. <span data-ttu-id="c1108-234">Rendelje hozzá a hello kiszolgálói tanúsítvány toohello jelentéskészítő kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c1108-234">Assign hello server certificate toohello report server.</span></span> <span data-ttu-id="c1108-235">hello hozzárendelés hello jelentéskészítő kiszolgáló konfigurálásakor hello a következő szakaszban befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c1108-235">hello assignment is completed in hello next section when you configure hello report server.</span></span>

<span data-ttu-id="c1108-236">Ha egy önaláírt SSL-tanúsítványt használ, a hello nevét hello tanúsítvány már megegyezik hello VM hello állomásnevével.</span><span class="sxs-lookup"><span data-stu-id="c1108-236">If you are using a self-signed SSL certificate, hello name on hello certificate already matches hello hostname of hello VM.</span></span> <span data-ttu-id="c1108-237">Ezért hello DNS hello gép már regisztrálva van globálisan, és elérhető az összes ügyfél.</span><span class="sxs-lookup"><span data-stu-id="c1108-237">Therefore, hello DNS of hello machine is already registered globally and can be accessed from any client.</span></span>

## <a name="step-3-configure-hello-report-server"></a><span data-ttu-id="c1108-238">3. lépés: Hello jelentéskészítő kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1108-238">Step 3: Configure hello Report Server</span></span>
<span data-ttu-id="c1108-239">Ez a szakasz bemutatja, hogyan hello VM konfigurálása natív üzemmódú Reporting Services jelentéskészítő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c1108-239">This section walks you through configuring hello VM as a Reporting Services native mode report server.</span></span> <span data-ttu-id="c1108-240">A következő módszerek tooconfigure hello jelentéskészítő kiszolgáló hello egyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="c1108-240">You can use one of hello following methods tooconfigure hello report server:</span></span>

* <span data-ttu-id="c1108-241">Hello parancsfájl tooconfigure hello jelentéskészítő kiszolgáló használatához</span><span class="sxs-lookup"><span data-stu-id="c1108-241">Use hello script tooconfigure hello report server</span></span>
* <span data-ttu-id="c1108-242">Használja a Configuration Manager tooConfigure hello jelentéskészítő kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c1108-242">Use Configuration Manager tooConfigure hello Report Server.</span></span>

<span data-ttu-id="c1108-243">További részletes lépéseket, tekintse meg a hello szakasz [Connect toohello virtuális gép és a Start hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span><span class="sxs-lookup"><span data-stu-id="c1108-243">For more detailed steps, see hello section [Connect toohello Virtual Machine and Start hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).</span></span>

<span data-ttu-id="c1108-244">**Hitelesítési Megjegyzés:** Windows-hitelesítés hello ajánlott hitelesítési módszert, valamint hello alapértelmezett Reporting Services hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="c1108-244">**Authentication Note:** Windows authentication is hello recommended authentication method and it is hello default Reporting Services authentication.</span></span> <span data-ttu-id="c1108-245">Csak a virtuális gép hello konfigurált felhasználók férhetnek a Reporting Services és tooReporting szolgáltatások szerepkört.</span><span class="sxs-lookup"><span data-stu-id="c1108-245">Only users that are configured on hello VM can access Reporting Services and assigned tooReporting Services roles.</span></span>

### <a name="use-script-tooconfigure-hello-report-server-and-http"></a><span data-ttu-id="c1108-246">Parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTP használata</span><span class="sxs-lookup"><span data-stu-id="c1108-246">Use script tooconfigure hello report server and HTTP</span></span>
<span data-ttu-id="c1108-247">toouse hello Windows PowerShell parancsfájl tooconfigure hello jelentéskészítő kiszolgálón, a következő lépéseket teljes hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-247">toouse hello Windows PowerShell script tooconfigure hello report server, complete hello following steps.</span></span> <span data-ttu-id="c1108-248">hello konfiguráció HTTP, HTTPS-nem tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c1108-248">hello configuration includes HTTP, not HTTPS:</span></span>

1. <span data-ttu-id="c1108-249">Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-249">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="c1108-250">A böngésző konfigurációtól függően előfordulhat felszólító toosave csatlakozni a virtuális gép toohello .rdp fájlt.</span><span class="sxs-lookup"><span data-stu-id="c1108-250">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
   
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="c1108-252">Hello VM felhasználónév, a felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használja.</span><span class="sxs-lookup"><span data-stu-id="c1108-252">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
   
    <span data-ttu-id="c1108-253">Például a következő kép hello, hello virtuális gép neve: **ssrsnativecloud** és hello felhasználónév **tesztfelhasználó néven**.</span><span class="sxs-lookup"><span data-stu-id="c1108-253">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
   
    ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="c1108-255">Nyissa meg a virtuális gép hello, **Windows PowerShell ISE** rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="c1108-255">On hello VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="c1108-256">a Windows server 2012 rendszerben alapértelmezés szerint telepítve van a PowerShell ISE hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-256">hello PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="c1108-257">Ajánlott használnia hello ISE helyett a szabványos Windows PowerShell-ablakot, hogy hello parancsfájl beillesztése hello ISE, módosítsa hello parancsfájlt, és futtassa a hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c1108-257">It is recommended you use hello ISE instead of a standard Windows PowerShell window so that you can paste hello script into hello ISE, modify hello script, and then run hello script.</span></span>
3. <span data-ttu-id="c1108-258">A Windows PowerShell ISE, kattintson a hello **nézet** menüre, majd majd **parancsfájl ablaktábla megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="c1108-258">In Windows PowerShell ISE, click hello **View** menu and then click **Show Script Pane**.</span></span>
4. <span data-ttu-id="c1108-259">A következő parancsfájl hello másolással illessze be a hello parancsfájl hello Windows PowerShell ISE parancsfájl ablaktáblára.</span><span class="sxs-lookup"><span data-stu-id="c1108-259">Copy hello following script, and paste hello script into hello Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change hello value if you used a different port for hello private HTTP endpoint when hello VM was created.
   
        ## Set PowerShell execution policy toobe able toorun scripts
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
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
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
   
        ## Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes toosqlserver provider
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
   
        ## Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
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
5. <span data-ttu-id="c1108-260">Egy HTTP-port a 80-as eltérő hello VM hozta létre, ha módosítani hello paraméter $HTTPport = 80-as.</span><span class="sxs-lookup"><span data-stu-id="c1108-260">If you created hello VM with an HTTP port other than 80, modify hello parameter $HTTPport = 80.</span></span>
6. <span data-ttu-id="c1108-261">Reporting Services jelenleg konfigurált hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c1108-261">hello script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="c1108-262">Ha a Reporting Services toorun hello parancsfájl, hello verzió részét módosíthatja hello elérési toohello névtér túl "v11", a Get-WmiObject hello utasítást.</span><span class="sxs-lookup"><span data-stu-id="c1108-262">If you want toorun hello script for  Reporting Services, modify hello version portion of hello path toohello namespace too“v11”, on hello Get-WmiObject statement.</span></span>
7. <span data-ttu-id="c1108-263">Hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="c1108-263">Run hello script.</span></span>

<span data-ttu-id="c1108-264">**Érvényesítési**: tooverify, amely hello alapvető jelentéskészítő kiszolgáló működőképes, lásd: hello [ellenőrizze hello konfigurációs](#verify-the-configuration) későbbi szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="c1108-264">**Validation**: tooverify that hello basic report server functionality is working, see hello [Verify hello configuration](#verify-the-configuration) section later in this topic.</span></span>

### <a name="use-script-tooconfigure-hello-report-server-and-https"></a><span data-ttu-id="c1108-265">Parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTPS használatára</span><span class="sxs-lookup"><span data-stu-id="c1108-265">Use script tooconfigure hello report server and HTTPS</span></span>
<span data-ttu-id="c1108-266">toouse Windows PowerShell tooconfigure hello jelentéskészítő kiszolgálón, a következő lépéseket teljes hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-266">toouse Windows PowerShell tooconfigure hello report server, complete hello following steps.</span></span> <span data-ttu-id="c1108-267">hello konfiguráció tartalmazza a HTTPS-en nem HTTP.</span><span class="sxs-lookup"><span data-stu-id="c1108-267">hello configuration includes HTTPS, not HTTP.</span></span>

1. <span data-ttu-id="c1108-268">Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-268">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="c1108-269">A böngésző konfigurációtól függően előfordulhat felszólító toosave csatlakozni a virtuális gép toohello .rdp fájlt.</span><span class="sxs-lookup"><span data-stu-id="c1108-269">Depending on your browser configuration, you may be prompted toosave an .rdp file for connecting toohello VM.</span></span>
   
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) <span data-ttu-id="c1108-271">Hello VM felhasználónév, a felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használja.</span><span class="sxs-lookup"><span data-stu-id="c1108-271">Use hello user VM name, user name and password you configured when you created hello VM.</span></span> 
   
    <span data-ttu-id="c1108-272">Például a következő kép hello, hello virtuális gép neve: **ssrsnativecloud** és hello felhasználónév **tesztfelhasználó néven**.</span><span class="sxs-lookup"><span data-stu-id="c1108-272">For example, in hello following image, hello VM name is **ssrsnativecloud** and hello user name is **testuser**.</span></span>
   
    ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. <span data-ttu-id="c1108-274">Nyissa meg a virtuális gép hello, **Windows PowerShell ISE** rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="c1108-274">On hello VM, open **Windows PowerShell ISE** with administrative privileges.</span></span> <span data-ttu-id="c1108-275">a Windows server 2012 rendszerben alapértelmezés szerint telepítve van a PowerShell ISE hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-275">hello PowerShell ISE is installed by default on Windows server 2012.</span></span> <span data-ttu-id="c1108-276">Ajánlott használnia hello ISE helyett a szabványos Windows PowerShell-ablakot, hogy hello parancsfájl beillesztése hello ISE, módosítsa hello parancsfájlt, és futtassa a hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c1108-276">It is recommended you use hello ISE instead of a standard Windows PowerShell window so that you can paste hello script into hello ISE, modify hello script, and then run hello script.</span></span>
3. <span data-ttu-id="c1108-277">parancsfájlok futtatása, futtassa a következő Windows PowerShell-paranccsal hello tooenable:</span><span class="sxs-lookup"><span data-stu-id="c1108-277">tooenable running scripts, run hello following Windows PowerShell command:</span></span>
   
        Set-ExecutionPolicy RemoteSigned
   
    <span data-ttu-id="c1108-278">Ezt követően futtathatja a következő tooverify hello házirend hello:</span><span class="sxs-lookup"><span data-stu-id="c1108-278">You can then run hello following tooverify hello policy:</span></span>
   
        Get-ExecutionPolicy
4. <span data-ttu-id="c1108-279">A **Windows PowerShell ISE**, kattintson a hello **nézet** menüre, és kattintson **parancsfájl ablaktábla megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="c1108-279">In **Windows PowerShell ISE**, click hello **View** menu and then click **Show Script Pane**.</span></span>
5. <span data-ttu-id="c1108-280">Másolja az alábbi parancsfájlt, és illessze be a Windows PowerShell ISE panelbe hello hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-280">Copy hello following script and paste it into hello Windows PowerShell ISE script pane.</span></span>
   
        ## This script configures hello report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when hello HTTPS endpoint was created.
   
        # You can run hello following command tooget (.cloudapp.net certificates) so you can copy hello thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # hello certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # hello certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If hello certificate is not a wildcard certificate, comment out hello following line, and enable hello full $DNSNAme reference.
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
   
        write-host "hello script will use $DNSNameAndPort as hello DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
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
   
        ## 2. Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes toosqlserver provider
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
   
        ## 3. Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
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
6. <span data-ttu-id="c1108-281">Módosítsa a hello **$certificatehash** paraméter hello parancsfájlban:</span><span class="sxs-lookup"><span data-stu-id="c1108-281">Modify hello **$certificatehash** parameter in hello script:</span></span>
   
   * <span data-ttu-id="c1108-282">Ez egy **szükséges** paraméter.</span><span class="sxs-lookup"><span data-stu-id="c1108-282">This is a **required** parameter.</span></span> <span data-ttu-id="c1108-283">Ha hello tanúsítvány érték nem mentette hello előző lépéseiből, valamelyikével módszerek toocopy hello tanúsítvány kivonatoló függvénnyel követően – hello tanúsítványok ujjlenyomat hello.:</span><span class="sxs-lookup"><span data-stu-id="c1108-283">If you did not save hello certificate value from hello previous steps, use one of hello following methods toocopy hello certificate hash value from hello certificates thumbprint.:</span></span>
     
       <span data-ttu-id="c1108-284">A virtuális gép hello nyissa meg a Windows PowerShell ISE és hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c1108-284">On hello VM, open Windows PowerShell ISE and run hello following command:</span></span>
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       <span data-ttu-id="c1108-285">hello kimeneti hasonló toohello következő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="c1108-285">hello output will look similar toohello following.</span></span> <span data-ttu-id="c1108-286">Hello parancsfájl egy üres sort ad vissza, ha hello virtuális gép nem rendelkezik konfigurált például tanúsítvány, hello című [toouse hello virtuális gépek önaláírt tanúsítvány](#to-use-the-virtual-machines-self-signed-certificate).</span><span class="sxs-lookup"><span data-stu-id="c1108-286">If hello script returns a blank line, hello VM does not have a certificate configured for example, see hello section [toouse hello Virtual Machines Self-signed Certificate](#to-use-the-virtual-machines-self-signed-certificate).</span></span>
     
     <span data-ttu-id="c1108-287">VAGY</span><span class="sxs-lookup"><span data-stu-id="c1108-287">OR</span></span>
   * <span data-ttu-id="c1108-288">A virtuális gép futtatásához mmc.exe hello, és adja meg az hello **tanúsítványok** beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="c1108-288">On hello VM Run mmc.exe and then add hello **Certificates** snap-in.</span></span>
   * <span data-ttu-id="c1108-289">A hello **megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, kattintson duplán a tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="c1108-289">Under hello **Trusted Root Certificate Authorities** node double-click your certificate name.</span></span> <span data-ttu-id="c1108-290">Hello VM hello önaláírt tanúsítványát használja, ha hello tanúsítvány van elnevezve hello VM hello DNS-nevét, és végződik **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="c1108-290">If you are using hello self-signed certificate of hello VM, hello certificate is named after hello DNS name of hello VM and ends with **cloudapp.net**.</span></span>
   * <span data-ttu-id="c1108-291">Kattintson a hello **részletek** fülre.</span><span class="sxs-lookup"><span data-stu-id="c1108-291">Click hello **Details** tab.</span></span>
   * <span data-ttu-id="c1108-292">Kattintson a **ujjlenyomat**.</span><span class="sxs-lookup"><span data-stu-id="c1108-292">Click **Thumbprint**.</span></span> <span data-ttu-id="c1108-293">hello ujjlenyomat hello érték jelenik meg hello részleteket tartalmazó mező, például af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span><span class="sxs-lookup"><span data-stu-id="c1108-293">hello value of hello thumbprint is displayed in hello details field, for example af 11 60 b6 4b 28 8d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48</span></span>
   * <span data-ttu-id="c1108-294">**Hello parancsfájl futtatása előtt**, távolítsa el Between hello értékpárok hello szóközöket.</span><span class="sxs-lookup"><span data-stu-id="c1108-294">**Before you run hello script**, remove hello spaces in between hello pairs of values.</span></span> <span data-ttu-id="c1108-295">Például af1160b64b288d890a8212ff6ba9c3664f319048</span><span class="sxs-lookup"><span data-stu-id="c1108-295">For example af1160b64b288d890a8212ff6ba9c3664f319048</span></span>
7. <span data-ttu-id="c1108-296">Módosítsa a hello **$httpsport** paraméter:</span><span class="sxs-lookup"><span data-stu-id="c1108-296">Modify hello **$httpsport** parameter:</span></span> 
   
   * <span data-ttu-id="c1108-297">Ha a HTTPS-végpont hello 443-as portot használja, majd nem kell tooupdate hello parancsfájlban ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="c1108-297">If you used port 443 for hello HTTPS endpoint, then you do not need tooupdate this parameter in hello script.</span></span> <span data-ttu-id="c1108-298">Ellenkező esetben használja hello HTTPS titkos végpont a virtuális gép hello konfigurálása során kiválasztott hello portértéket.</span><span class="sxs-lookup"><span data-stu-id="c1108-298">Otherwise use hello port value you selected when you configured hello HTTPS private endpoint on hello VM.</span></span>
8. <span data-ttu-id="c1108-299">Módosítsa a hello **$DNSName** paraméter:</span><span class="sxs-lookup"><span data-stu-id="c1108-299">Modify hello **$DNSName** parameter:</span></span> 
   
   * <span data-ttu-id="c1108-300">hello parancsfájl úgy van konfigurálva, a helyettesítő tanúsítvány $DNSName = "+".</span><span class="sxs-lookup"><span data-stu-id="c1108-300">hello script is configured for a wild card certificate $DNSName="+".</span></span> <span data-ttu-id="c1108-301">Ha nem akarja tooconfigure helyettesítő tanúsítvány kötés, megjegyzéssé $DNSName = "+"és a következő sorban, hello teljes $DNSNAme hivatkozás, ## $DNSName="$server.cloudapp.net hello engedélyezése".</span><span class="sxs-lookup"><span data-stu-id="c1108-301">If you do no want tooconfigure for a wildcard certificate binding, comment out $DNSName="+" and enable hello following line, hello full $DNSNAme reference, ##$DNSName="$server.cloudapp.net".</span></span>
     
       <span data-ttu-id="c1108-302">Módosítsa a hello $DNSName értéket, ha nem szeretné a Reporting Services toouse hello virtuális gép DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="c1108-302">Change hello $DNSName value if you do not want toouse hello virtual machine’s DNS name for Reporting Services.</span></span> <span data-ttu-id="c1108-303">Hello paraméter használatakor hello tanúsítványt kell használnia az ezt a nevet, és regisztrál egy DNS kiszolgálón lévő globálisan hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c1108-303">If you use hello parameter, hello certificate must also use this name and you register hello name globally on a DNS server.</span></span>
9. <span data-ttu-id="c1108-304">Reporting Services jelenleg konfigurált hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c1108-304">hello script is currently configured for  Reporting Services.</span></span> <span data-ttu-id="c1108-305">Ha a Reporting Services toorun hello parancsfájl, hello verzió részét módosíthatja hello elérési toohello névtér túl "v11", a Get-WmiObject hello utasítást.</span><span class="sxs-lookup"><span data-stu-id="c1108-305">If you want toorun hello script for  Reporting Services, modify hello version portion of hello path toohello namespace too“v11”, on hello Get-WmiObject statement.</span></span>
10. <span data-ttu-id="c1108-306">Hello parancsprogrammal.</span><span class="sxs-lookup"><span data-stu-id="c1108-306">Run hello script.</span></span>

<span data-ttu-id="c1108-307">**Érvényesítési**: tooverify, amely hello alapvető jelentéskészítő kiszolgáló működőképes, lásd: hello [ellenőrizze hello konfigurációs](#verify-the-connection) későbbi szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="c1108-307">**Validation**: tooverify that hello basic report server functionality is working, see hello [Verify hello configuration](#verify-the-connection) section later in this topic.</span></span> <span data-ttu-id="c1108-308">tooverify hello tanúsítványkötés nyisson meg egy parancssort rendszergazdai jogosultságokkal, és futtassa a parancsot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c1108-308">tooverify hello certificate binding open a command prompt with administrative privileges, and then run hello following command:</span></span>

    netsh http show sslcert

<span data-ttu-id="c1108-309">hello eredmény hello következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="c1108-309">hello result will include hello following:</span></span>

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-tooconfigure-hello-report-server"></a><span data-ttu-id="c1108-310">Használja a Configuration Manager tooConfigure hello jelentéskészítő kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="c1108-310">Use Configuration Manager tooConfigure hello Report Server</span></span>
<span data-ttu-id="c1108-311">Ha nem szeretné, hogy toorun hello PowerShell parancsfájl tooconfigure hello jelentéskészítő kiszolgálón, lépésekkel hello Ez a szakasz toouse hello Reporting Services natív üzemmódú configuration manager tooconfigure hello jelentéskészítő kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c1108-311">If you do not want toorun hello PowerShell script tooconfigure hello report server, follow hello steps in this section toouse hello Reporting Services native mode configuration manager tooconfigure hello report server.</span></span>

1. <span data-ttu-id="c1108-312">Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-312">From hello Azure classic portal, select hello VM and click connect.</span></span> <span data-ttu-id="c1108-313">Hello felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használata.</span><span class="sxs-lookup"><span data-stu-id="c1108-313">Use hello user name and password you configured when you created hello VM.</span></span>
   
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. <span data-ttu-id="c1108-315">Futtassa a Windows update és a frissítések toohello VM telepítése.</span><span class="sxs-lookup"><span data-stu-id="c1108-315">Run Windows update and install updates toohello VM.</span></span> <span data-ttu-id="c1108-316">Hello virtuális gép újraindítására szükség, ha hello virtuális gép újraindítása, és csatlakozzon újra a virtuális gép toohello a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-316">If a restart of hello VM is required, restart hello VM and reconnect toohello VM from hello Azure classic portal.</span></span>
3. <span data-ttu-id="c1108-317">A virtuális gép hello hello Start menüben írja be a **Reporting Services** , és nyissa meg **Reporting Services Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="c1108-317">From hello Start menu on hello VM, type **Reporting Services** and open **Reporting Services Configuration Manager**.</span></span>
4. <span data-ttu-id="c1108-318">Hagyja bejelölve az alapértelmezett értékeit hello **kiszolgálónév** és **jelentéskészítő kiszolgáló példánya**.</span><span class="sxs-lookup"><span data-stu-id="c1108-318">Leave hello default values for **Server Name** and **Report Server Instance**.</span></span> <span data-ttu-id="c1108-319">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-319">Click **Connect**.</span></span>
5. <span data-ttu-id="c1108-320">Hello bal oldali ablaktáblában kattintson **webes szolgáltatás URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="c1108-320">In hello left pane, click **Web Service URL**.</span></span>
6. <span data-ttu-id="c1108-321">Alapértelmezés szerint RS "Az összes hozzárendelt" IP-80-as HTTP-port van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c1108-321">By default, RS is configured for HTTP port 80 with IP “All Assigned”.</span></span> <span data-ttu-id="c1108-322">tooadd HTTPS:</span><span class="sxs-lookup"><span data-stu-id="c1108-322">tooadd HTTPS:</span></span>
   
   1. <span data-ttu-id="c1108-323">A **SSL-tanúsítvány**: select hello tanúsítványt toouse, [a virtuális gép neve] például. cloudapp.net.</span><span class="sxs-lookup"><span data-stu-id="c1108-323">In **SSL Certificate**: select hello certificate you want toouse, for example [VM name].cloudapp.net.</span></span> <span data-ttu-id="c1108-324">Ha nincsenek felsorolva tanúsítványok, című hello **2. lépés: hozzon létre egy kiszolgálói tanúsítványt** olvashat, hogyan tooinstall és megbízhatósági hello hello VM-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c1108-324">If no certificates are listed, see hello section **Step 2: Create a Server Certificate** for information on how tooinstall and trust hello certificate on hello VM.</span></span>
   2. <span data-ttu-id="c1108-325">A **SSL-Port**: válassza ki a 443-as.</span><span class="sxs-lookup"><span data-stu-id="c1108-325">Under **SSL Port**: choose 443.</span></span> <span data-ttu-id="c1108-326">Ha más magánhálózati port VM hello konfigurált hello HTTPS titkos végpont, használni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c1108-326">If you configured hello HTTPS private endpoint in hello VM with a different private port, use that value here.</span></span>
   3. <span data-ttu-id="c1108-327">Kattintson a **alkalmaz** és hello művelet toocomplete várja.</span><span class="sxs-lookup"><span data-stu-id="c1108-327">Click **Apply** and wait for hello operation toocomplete.</span></span>
7. <span data-ttu-id="c1108-328">Hello bal oldali ablaktáblában kattintson **adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="c1108-328">In hello left pane, click **Database**.</span></span>
   
   1. <span data-ttu-id="c1108-329">Kattintson a **Databas módosítása**e.</span><span class="sxs-lookup"><span data-stu-id="c1108-329">Click **Change Databas**e.</span></span>
   2. <span data-ttu-id="c1108-330">Kattintson a **hozzon létre egy új jelentéskészítő kiszolgáló adatbázisában** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="c1108-330">Click **Create a new report server database** and then click **Next**.</span></span>
   3. <span data-ttu-id="c1108-331">Hagyja hello alapértelmezett **kiszolgálónév**: neve hello VM legyen, és hagyja hello alapértelmezett **hitelesítési típus** , **aktuális felhasználó** – **integrált biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="c1108-331">Leave hello default **Server Name**: as hello VM name and leave hello default **Authentication Type** as **Current User** – **Integrated Security**.</span></span> <span data-ttu-id="c1108-332">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c1108-332">Click **Next**.</span></span>
   4. <span data-ttu-id="c1108-333">Hagyja hello alapértelmezett **adatbázisnév** , **ReportServer** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="c1108-333">Leave hello default **Database Name** as **ReportServer** and click **Next**.</span></span>
   5. <span data-ttu-id="c1108-334">Hagyja hello alapértelmezett **hitelesítési típus** , **szolgáltatás hitelesítő adatai** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="c1108-334">Leave hello default **Authentication Type** as **Service Credentials** and click **Next**.</span></span>
   6. <span data-ttu-id="c1108-335">Kattintson a **következő** a hello **összegzés** lap.</span><span class="sxs-lookup"><span data-stu-id="c1108-335">Click **Next** on hello **Summary** page.</span></span>
   7. <span data-ttu-id="c1108-336">Hello konfiguráció befejeztével kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="c1108-336">When hello configuration is complete, click **Finish**.</span></span>
8. <span data-ttu-id="c1108-337">Hello bal oldali ablaktáblában kattintson **Jelentéskezelő URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="c1108-337">In hello left pane, click **Report Manager URL**.</span></span> <span data-ttu-id="c1108-338">Hagyja hello alapértelmezett **virtuális könyvtár** , **jelentések** kattintson **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="c1108-338">Leave hello default **Virtual Directory** as **Reports** and click **Apply**.</span></span>
9. <span data-ttu-id="c1108-339">Kattintson a **kilépési** tooclose hello Reporting Services Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="c1108-339">Click **Exit** tooclose hello Reporting Services Configuration Manager.</span></span>

## <a name="step-4-open-windows-firewall-port"></a><span data-ttu-id="c1108-340">4. lépés: A Windows tűzfal Port megnyitása</span><span class="sxs-lookup"><span data-stu-id="c1108-340">Step 4: Open Windows Firewall Port</span></span>
> [!NOTE]
> <span data-ttu-id="c1108-341">Ha egy hello parancsfájlok tooconfigure hello jelentéskészítő kiszolgálón, ez a szakasz kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="c1108-341">If you used one of hello scripts tooconfigure hello report server, you can skip this section.</span></span> <span data-ttu-id="c1108-342">hello parancsfájl lépés tooopen hello tűzfalportot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1108-342">hello script included a step tooopen hello firewall port.</span></span> <span data-ttu-id="c1108-343">hello alapértelmezett port a 80-as HTTP és HTTPS – 443-as port volt.</span><span class="sxs-lookup"><span data-stu-id="c1108-343">hello default was port 80 for HTTP and port 443 for HTTPS.</span></span>
> 
> 

<span data-ttu-id="c1108-344">tooconnect távolról tooReport kezelő vagy hello jelentést Server hello virtuális gépen, a TCP-végpont hello virtuális gép szükséges.</span><span class="sxs-lookup"><span data-stu-id="c1108-344">tooconnect remotely tooReport Manager or hello Report Server on hello virtual machine, a TCP Endpoint is required on hello VM.</span></span> <span data-ttu-id="c1108-345">Ugyanaz a hello VM tűzfal port szükséges tooopen hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-345">It is required tooopen hello same port in hello VM’s firewall.</span></span> <span data-ttu-id="c1108-346">hello végpont jött létre, amikor a virtuális gép hello lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="c1108-346">hello endpoint was created when hello VM was provisioned.</span></span>

<span data-ttu-id="c1108-347">Ez a szakasz hogyan tooopen hello tűzfalportot alapvető információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="c1108-347">This section provides basic information on how tooopen hello firewall port.</span></span> <span data-ttu-id="c1108-348">További információkért lásd: [beállítani a tűzfalat a jelentéskészítő kiszolgáló elérése](https://technet.microsoft.com/library/bb934283.aspx)</span><span class="sxs-lookup"><span data-stu-id="c1108-348">For more information, see [Configure a Firewall for Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)</span></span>

> [!NOTE]
> <span data-ttu-id="c1108-349">Ha hello parancsfájl tooconfigure hello jelentéskészítő kiszolgáló használja, kihagyhatja ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c1108-349">If you used hello script tooconfigure hello report server, you can skip this section.</span></span> <span data-ttu-id="c1108-350">hello parancsfájl lépés tooopen hello tűzfalportot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c1108-350">hello script included a step tooopen hello firewall port.</span></span>
> 
> 

<span data-ttu-id="c1108-351">A magánhálózati port a HTTPS használatára konfigurált 443-astól eltérő, ha módosítsa megfelelően a következő parancsfájl hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-351">If you configured a private port for HTTPS other than 443, modify hello following script appropriately.</span></span> <span data-ttu-id="c1108-352">tooopen port **443-as** a Windows tűzfal hello, végezze el a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c1108-352">tooopen port **443** on hello Windows Firewall, complete hello following:</span></span>

1. <span data-ttu-id="c1108-353">Nyissa meg a Windows PowerShell ablakot rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="c1108-353">Open a Windows PowerShell window with administrative privileges.</span></span>
2. <span data-ttu-id="c1108-354">Hello a HTTPS-végpont a virtuális gép hello konfigurálása során a 443-astól eltérő port használata esetén a következő parancs hello hello port frissítése, és futtassa hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="c1108-354">If you used a port other than 443 when you configured hello HTTPS endpoint on hello VM, update hello port in hello following command and then run hello command:</span></span>
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. <span data-ttu-id="c1108-355">Hello parancs befejeződésekor **Ok** hello parancssor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c1108-355">When hello command completes, **Ok** is displayed in hello command prompt.</span></span>

<span data-ttu-id="c1108-356">tooverify, hogy hello port meg van nyitva, nyisson meg egy Windows PowerShell ablakot, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="c1108-356">tooverify that hello port is opened, open a Windows PowerShell window and run hello following command:</span></span>

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-hello-configuration"></a><span data-ttu-id="c1108-357">Hello konfigurációjának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c1108-357">Verify hello configuration</span></span>
<span data-ttu-id="c1108-358">rendszergazdai jogosultságokkal rendelkező tooverify hello alapvető jelentéskészítő kiszolgálói funkciók most működik, nyissa meg a böngészőt, és Tallózás toohello következő jelentést készít a kiszolgáló ad Jelentéskezelő URL-CÍMEK:</span><span class="sxs-lookup"><span data-stu-id="c1108-358">tooverify that hello basic report server functionality is now working, open your browser with administrative privileges and then browse toohello following report server ad report manager URLS:</span></span>

* <span data-ttu-id="c1108-359">Keresse meg a virtuális gép hello, a toohello jelentéskészítő kiszolgáló URL-címe:</span><span class="sxs-lookup"><span data-stu-id="c1108-359">On hello VM, browse toohello report server URL:</span></span>
  
        http://localhost/reportserver
* <span data-ttu-id="c1108-360">Keresse meg a virtuális gép hello, a toohello jelentés Manager URL-címe:</span><span class="sxs-lookup"><span data-stu-id="c1108-360">On hello VM, browse toohello report manger URL:</span></span>
  
        http://localhost/Reports
* <span data-ttu-id="c1108-361">A helyi számítógépről, keresse meg a toohello **távoli** hello VM Manager jelentést.</span><span class="sxs-lookup"><span data-stu-id="c1108-361">From your local computer, browse toohello **remote** report Manager on hello VM.</span></span> <span data-ttu-id="c1108-362">Hello DNS-név a következő példa megfelelő hello a frissítése.</span><span class="sxs-lookup"><span data-stu-id="c1108-362">Update hello DNS name in hello following example as appropriate.</span></span> <span data-ttu-id="c1108-363">Amikor a rendszer kéri a jelszót, használja a hello rendszergazdai hitelesítő adatokat, akkor jön létre, amikor a virtuális gép hello lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="c1108-363">When prompted for a password, use hello administrator credentials you created when hello VM was provisioned.</span></span> <span data-ttu-id="c1108-364">hello felhasználói név van hello [tartomány]\[felhasználónév] formátumban, ahol a hello tartománya hello VM számítógép nevét, például ssrsnativecloud\testuser.</span><span class="sxs-lookup"><span data-stu-id="c1108-364">hello user name is in hello [Domain]\[user name] format, where hello domain is hello VM computer name, for example ssrsnativecloud\testuser.</span></span> <span data-ttu-id="c1108-365">Ha nem használja a HTTP**S**, távolítsa el a hello **s** hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="c1108-365">If you are not using HTTP**S**, remove hello **s** in hello URL.</span></span> <span data-ttu-id="c1108-366">Című rész hello Tovább információt a további felhasználók létrehozása a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="c1108-366">See hello next section for information on creating additional users on VM.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/Reports
* <span data-ttu-id="c1108-367">A helyi számítógépről keresse meg a toohello távoli jelentéskészítő kiszolgáló URL-címe.</span><span class="sxs-lookup"><span data-stu-id="c1108-367">From your local computer, browse toohello remote report server URL.</span></span> <span data-ttu-id="c1108-368">Hello DNS-név a következő példa megfelelő hello a frissítése.</span><span class="sxs-lookup"><span data-stu-id="c1108-368">Update hello DNS name in hello following example as appropriate.</span></span> <span data-ttu-id="c1108-369">Ha HTTPS nem használ, távolítsa el a hello s hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="c1108-369">If you are not using HTTPS, remove hello s in hello URL.</span></span>
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a><span data-ttu-id="c1108-370">Felhasználók létrehozása és szerepkörök hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c1108-370">Create Users and Assign Roles</span></span>
<span data-ttu-id="c1108-371">Konfigurálásának és ellenőrzésének hello jelentés kiszolgáló, miután egy közös felügyeleti feladat toocreate egy vagy több felhasználó, és felhasználók tooReporting szolgáltatások szerepkörök hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="c1108-371">After configuring and verifying hello report server, a common administrative task is toocreate one or more users and assign users tooReporting Services roles.</span></span> <span data-ttu-id="c1108-372">További információkért lásd: hello következő:</span><span class="sxs-lookup"><span data-stu-id="c1108-372">For more information, see hello following:</span></span>

* [<span data-ttu-id="c1108-373">Helyi felhasználói fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1108-373">Create a local user account</span></span>](https://technet.microsoft.com/library/cc770642.aspx)
* <span data-ttu-id="c1108-374">[Felhasználói hozzáférés tooa jelentéskészítő kiszolgáló (Jelentéskezelő)](https://msdn.microsoft.com/library/ms156034.aspx))</span><span class="sxs-lookup"><span data-stu-id="c1108-374">[Grant User Access tooa Report Server (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))</span></span>
* [<span data-ttu-id="c1108-375">Létrehozása és kezelése a szerepkör-hozzárendelések</span><span class="sxs-lookup"><span data-stu-id="c1108-375">Create and Manage Role Assignments</span></span>](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a><span data-ttu-id="c1108-376">tooCreate és a jelentések közzététele toohello Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="c1108-376">tooCreate and Publish Reports toohello Azure Virtual Machine</span></span>
<span data-ttu-id="c1108-377">hello következő táblázat összefoglalja hello beállítások elérhető toopublish meglévő kiszolgálóról származó jelentések egy helyszíni számítógép toohello jelentés hello Microsoft Azure rendszerű virtuális gépeken üzemeltet egy részénél:</span><span class="sxs-lookup"><span data-stu-id="c1108-377">hello following table summarizes some of hello options available toopublish existing reports from an on-premises computer toohello report server hosted on hello Microsoft Azure Virtual Machine:</span></span>

* <span data-ttu-id="c1108-378">**RS.exe parancsfájl**: használata RS.exe parancsfájl toocopy jelentés elemeit és meglévő jelentés server tooyour Microsoft Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c1108-378">**RS.exe script**: Use RS.exe script toocopy report items from and existing report server tooyour Microsoft Azure Virtual Machine.</span></span> <span data-ttu-id="c1108-379">További információkért lásd: hello szakasz "natív üzemmódú tooNative módban – Ez a Microsoft Azure virtuális gép" a [minta Reporting Services rs.exe parancsfájl tooMigrate jelentés kiszolgálók közötti tartalom](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-379">For more information, see hello section “Native mode tooNative Mode – Microsoft Azure Virtual Machine” in [Sample Reporting Services rs.exe Script tooMigrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>
* <span data-ttu-id="c1108-380">**Jelentéskészítő**: hello virtuális gépet tartalmaz hello kattintson-Microsoft SQL Server jelentéskészítő egyszer verzióját.</span><span class="sxs-lookup"><span data-stu-id="c1108-380">**Report Builder**: hello virtual machine includes hello click-once version of Microsoft SQL Server Report Builder.</span></span> <span data-ttu-id="c1108-381">toostart jelentés jelentéskészítő hello először hello virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="c1108-381">toostart Report builder hello first time on hello virtual machine:</span></span>
  
  1. <span data-ttu-id="c1108-382">A böngészőben rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="c1108-382">Start your browser with administrative privileges.</span></span>
  2. <span data-ttu-id="c1108-383">Keresse meg a tooreport manager hello virtuális gépen, és kattintson a **jelentéskészítő** hello szalagon.</span><span class="sxs-lookup"><span data-stu-id="c1108-383">Browse tooreport manager on hello virtual machine and click **Report Builder**  in hello ribbon.</span></span>
     
     <span data-ttu-id="c1108-384">További információkért lásd: [telepítése, eltávolítása és a jelentéskészítő támogató](https://technet.microsoft.com/library/dd207038.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-384">For more information, see [Installing, Uninstalling, and Supporting Report Builder](https://technet.microsoft.com/library/dd207038.aspx).</span></span>
* <span data-ttu-id="c1108-385">**SQL Server Data Tools: Virtuális gép**: Ha SQL Server 2012-ben létrehozott hello VM, akkor az SQL Server Data Tools hello virtuális gépre van telepítve, és lehet használt toocreate **jelentés Server projektek** és virtuális hello szóló jelentések gép.</span><span class="sxs-lookup"><span data-stu-id="c1108-385">**SQL Server Data Tools: VM**:  If you created hello VM with SQL Server 2012, then SQL Server Data Tools is installed on hello virtual machine and can be used toocreate **Report Server Projects** and reports on hello virtual machine.</span></span> <span data-ttu-id="c1108-386">SQL Server Data Tools közzétehet hello jelentések toohello jelentéskészítő kiszolgáló hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c1108-386">SQL Server Data Tools can publish hello reports toohello report server on hello virtual machine.</span></span>
  
    <span data-ttu-id="c1108-387">Ha SQL server 2014 hello VM hozta létre, telepítheti az SQL Server Data Tools - BI visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c1108-387">If you created hello VM with SQL server 2014, you can install SQL Server Data Tools- BI for visual Studio.</span></span> <span data-ttu-id="c1108-388">További információkért lásd: hello következő:</span><span class="sxs-lookup"><span data-stu-id="c1108-388">For more information, see hello following:</span></span>
  
  * [<span data-ttu-id="c1108-389">Microsoft SQL Server Data Tools összetevővel - üzleti intelligencia, a Visual Studio 2013-hoz</span><span class="sxs-lookup"><span data-stu-id="c1108-389">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013</span></span>](https://www.microsoft.com/download/details.aspx?id=42313)
  * [<span data-ttu-id="c1108-390">Microsoft SQL Server Data Tools összetevővel - üzleti intelligencia, a Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c1108-390">Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=36843)
  * [<span data-ttu-id="c1108-391">SQL Server Data Tools összetevővel és az SQL Server Business Intelligence (SSDT-bA)</span><span class="sxs-lookup"><span data-stu-id="c1108-391">SQL Server Data Tools and SQL Server Business Intelligence (SSDT-BI)</span></span>](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* <span data-ttu-id="c1108-392">**SQL Server Data Tools: Távoli**: A helyi számítógépen, a Reporting Services-projekt létrehozása az SQL Server Data Tools összetevővel, amely tartalmazza a Reporting Services-jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="c1108-392">**SQL Server Data Tools: Remote**:  On your local computer, create a Reporting Services project in SQL Server Data Tools that contains Reporting Services reports.</span></span> <span data-ttu-id="c1108-393">Hello projekt tooconnect toohello webes szolgáltatás URL-CÍMEK konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="c1108-393">Configure hello project tooconnect toohello web service URL.</span></span>
  
    ![SSRS-projekt SSDT projektjének tulajdonságai](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* <span data-ttu-id="c1108-395">**Parancsfájl használata**: használja a parancsfájl toocopy jelentés tartalmához.</span><span class="sxs-lookup"><span data-stu-id="c1108-395">**Use script**: Use script toocopy report server content.</span></span> <span data-ttu-id="c1108-396">További információkért lásd: [minta Reporting Services rs.exe parancsfájl tooMigrate jelentés kiszolgálók közötti tartalom](https://msdn.microsoft.com/library/dn531017.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-396">For more information, see [Sample Reporting Services rs.exe Script tooMigrate Content between Report Servers](https://msdn.microsoft.com/library/dn531017.aspx).</span></span>

## <a name="minimize-cost-if-you-are-not-using-hello-vm"></a><span data-ttu-id="c1108-397">Ha nem használja a virtuális gép hello minimalizálása érdekében a költség</span><span class="sxs-lookup"><span data-stu-id="c1108-397">Minimize cost if you are not using hello VM</span></span>
> [!NOTE]
> <span data-ttu-id="c1108-398">toominimize költségek az az Azure virtuális gépek Ha nincsenek használatban, állítsa le a klasszikus Azure portálon hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-398">toominimize charges for your Azure Virtual Machines when not in use, shut down hello VM from hello Azure classic portal.</span></span> <span data-ttu-id="c1108-399">Hello Windows energiagazdálkodási beállításait hello VM le a virtuális gép tooshut belül használja, ha van is szó, ugyanaz a virtuális gép hello összeg hello.</span><span class="sxs-lookup"><span data-stu-id="c1108-399">If you use hello Windows power options inside a VM tooshut down hello VM, you are still charged hello same amount for hello VM.</span></span> <span data-ttu-id="c1108-400">tooreduce költség, a klasszikus Azure portálon hello VM hello le tooshut van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c1108-400">tooreduce charges, you need tooshut down hello VM in hello Azure classic portal.</span></span> <span data-ttu-id="c1108-401">Ha már nincs szüksége a virtuális gép hello, ne feledje toodelete hello virtuális gép, és hello hozzárendelt .vhd fájlok tooavoid tárolási költségek. További információkért lásd: gyakran ismételt kérdések hello szakasz [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="c1108-401">If you no longer need hello VM, remember toodelete hello VM and hello associated .vhd files tooavoid storage charges.For more information, see hello FAQ section at [Virtual Machines Pricing Details](https://azure.microsoft.com/pricing/details/virtual-machines/).</span></span>

## <a name="more-information"></a><span data-ttu-id="c1108-402">További információ</span><span class="sxs-lookup"><span data-stu-id="c1108-402">More Information</span></span>
### <a name="resources"></a><span data-ttu-id="c1108-403">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="c1108-403">Resources</span></span>
* <span data-ttu-id="c1108-404">A hasonló tartalomhoz kapcsolódó tooa egyetlen kiszolgáló telepítéséhez SQL Server Business Intelligence és a SharePoint 2013, lásd: [a Windows PowerShell tooCreate egy Azure virtuális gép az SQL Server BI és a SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-404">For similar content related tooa single server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Use Windows PowerShell tooCreate an Azure VM With SQL Server BI and SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).</span></span>
* <span data-ttu-id="c1108-405">Többkiszolgálós telepítésben, SQL Server Business Intelligence és a SharePoint 2013-at, lásd: hasonló tartalom kapcsolódó tooa [központi telepítése az SQL Server Business Intelligence Azure virtuális gépek](https://msdn.microsoft.com/library/dn321998.aspx).</span><span class="sxs-lookup"><span data-stu-id="c1108-405">For similar content related tooa multi-server deployment of SQL Server Business Intelligence and SharePoint 2013, see [Deploy SQL Server Business Intelligence in Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).</span></span>
* <span data-ttu-id="c1108-406">Általános információk az SQL Server Business Intelligence Azure virtuális gépek, kapcsolódó toodeployments: [SQL Server Business Intelligence Azure virtuális gépek](virtual-machines-windows-classic-ps-sql-bi.md).</span><span class="sxs-lookup"><span data-stu-id="c1108-406">For General information related toodeployments of SQL Server Business Intelligence in Azure Virtual Machines, see [SQL Server Business Intelligence in Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).</span></span>
* <span data-ttu-id="c1108-407">További információ az Azure számítási díjakat hello költség: hello virtuális gépek lapján [Azure árképzési Számológép](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="c1108-407">For more information about hello cost of Azure compute charges, see hello Virtual Machines tab of [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).</span></span>

### <a name="community-content"></a><span data-ttu-id="c1108-408">Közösségi tartalom</span><span class="sxs-lookup"><span data-stu-id="c1108-408">Community Content</span></span>
* <span data-ttu-id="c1108-409">Hogyan toocreate jelentéskészítési szolgáltatások natív mód jelentést parancsfájl használata nélkül server részletes ismertetését lásd: [üzemeltető SQL jelentési szolgáltatás az Azure virtuális gép](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span><span class="sxs-lookup"><span data-stu-id="c1108-409">For step by step instructions on how toocreate a Reporting Services Native mode report server without using script, see [Hosting SQL Reporting Service on Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).</span></span>

### <a name="links-tooother-resources-for-sql-server-in-azure-vms"></a><span data-ttu-id="c1108-410">Hivatkozások tooother erőforrások az SQL Server Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="c1108-410">Links tooother resources for SQL Server in Azure VMs</span></span>
[<span data-ttu-id="c1108-411">SQL Server Azure virtuális gépek – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c1108-411">SQL Server on Azure Virtual Machines Overview</span></span>](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

