---
title: "egy Azure Site Recovery segítségével többrétegű Dynamics AX telepítési aaaReplicate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooreplicate és az Azure Site Recovery segítségével Dynamics AX védelme"
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/24/2017
ms.author: asgang
ms.openlocfilehash: b974315ec50ab2ec43846b3d3f95c7de88b72fc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="57cf2-103">Egy Azure Site Recovery segítségével többrétegű Dynamics AX-alkalmazás replikálása</span><span class="sxs-lookup"><span data-stu-id="57cf2-103">Replicate a multi-tier Dynamics AX application using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="57cf2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="57cf2-104">Overview</span></span>


<span data-ttu-id="57cf2-105">A Microsoft Dynamics AX hello legnépszerűbb ERP-megoldások között vállalatok toostandardized folyamat egyik helyszínen, kezelheti az erőforrásokat és a megfelelőségi leegyszerűsítve.</span><span class="sxs-lookup"><span data-stu-id="57cf2-105">Microsoft Dynamics AX is one of hello most popular ERP solution among enterprises toostandardized process across locations, manage resources and simplifying compliance.</span></span> <span data-ttu-id="57cf2-106">Considering hello alkalmazás üzleti kritikus tooan szervezet nagyon fontos, hogy toobe, ha bármely katasztrófa alkalmazás kell működik, és minimális időtartam.</span><span class="sxs-lookup"><span data-stu-id="57cf2-106">Considering hello application is business critical tooan organization it is very important toobe sure that if any disaster, application should be up and running in minimum time.</span></span>

<span data-ttu-id="57cf2-107">A Microsoft Dynamics AX jelenleg nem biztosít bármely out-of-az-box vész-helyreállítási funkciók.</span><span class="sxs-lookup"><span data-stu-id="57cf2-107">Today, Microsoft Dynamics AX  does not provide any out-of-the-box disaster recovery capabilities.</span></span> <span data-ttu-id="57cf2-108">A Microsoft Dynamics AX sok kiszolgáló összetevőből áll például objektum alkalmazáskiszolgáló, Active Directory (AD), az SQL adatbázis-kiszolgáló, a SharePoint Server, a Reporting Server stb toomanage hello vész-helyreállítási az egyes összetevők manuálisan nem csak költséges de is hibákhoz vezethet.</span><span class="sxs-lookup"><span data-stu-id="57cf2-108">Microsoft Dynamics AX consists of many server components like Application Object Server, Active Directory (AD), SQL Database Server, SharePoint Server, Reporting Server etc. toomanage hello disaster recovery of each of these components manually is not only expensive but also error-prone.</span></span>

<span data-ttu-id="57cf2-109">Ez a cikk ismerteti részletesen hogyan hozhat létre egy vész-helyreállítási megoldást a Dynamics AX alkalmazás a [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="57cf2-109">This article explains in detail about how you can create a disaster recovery solution for your Dynamics AX application using [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="57cf2-110">Tervezett/nem tervezett/feladatátvételi tesztet egy kattintással helyreállítási tervet, a támogatott konfigurációk és az Előfeltételek is magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="57cf2-110">It also covers planned/unplanned/test failovers using one-click recovery plan, supported configurations, and prerequisites.</span></span>
<span data-ttu-id="57cf2-111">Az Azure Site Recovery-alapú vész-helyreállítási megoldást teljesen tesztelni, hitelesített, és a Microsoft Dynamics AX által ajánlott.</span><span class="sxs-lookup"><span data-stu-id="57cf2-111">Azure Site Recovery based disaster recovery solution is fully tested, certified, and recommended by Microsoft Dynamics AX.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="57cf2-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57cf2-112">Prerequisites</span></span>

<span data-ttu-id="57cf2-113">Hello Előfeltételek befejeződött a következő végrehajtási vész-helyreállítási Azure Site Recovery segítségével Dynamics AX-alkalmazáshoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="57cf2-113">Implementing disaster recovery for Dynamics AX application using Azure Site Recovery requires hello following pre-requisites completed.</span></span>

<span data-ttu-id="57cf2-114">• Egy helyszíni Dynamics AX-telepítés beállítása</span><span class="sxs-lookup"><span data-stu-id="57cf2-114">•   An on-premises Dynamics AX deployment has been set up</span></span>

<span data-ttu-id="57cf2-115">• Az azure Site Recovery Services-tároló létrehozása a Microsoft Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="57cf2-115">•   Azure Site Recovery Services vault has been created in Microsoft Azure subscription</span></span>

<span data-ttu-id="57cf2-116">• Ha Azure a helyreállítási helyen futtatja hello Azure virtuális gép Readiness Assessment eszközt a virtuális gépek tooensure, amelyek kompatibilisek a Azure virtuális gépek és az Azure Site Recovery Services</span><span class="sxs-lookup"><span data-stu-id="57cf2-116">•   If Azure is your recovery site, run hello Azure Virtual Machine Readiness Assessment tool  on VMs tooensure that they are compatible with Azure VMs and Azure Site Recovery Services</span></span>


## <a name="site-recovery-support"></a><span data-ttu-id="57cf2-117">Webhely-helyreállítási támogatás</span><span class="sxs-lookup"><span data-stu-id="57cf2-117">Site Recovery support</span></span>

<span data-ttu-id="57cf2-118">Hello célból történő létrehozásának ebben a cikkben VMware virtuális gépek, a Windows Server 2012 R2 Enterprise a Dynamics AX 2012R3 használták.</span><span class="sxs-lookup"><span data-stu-id="57cf2-118">For hello purpose of creating this article, VMware virtual machines with Dynamics AX  2012R3 on Windows Server 2012 R2 Enterprise were used.</span></span> <span data-ttu-id="57cf2-119">Mivel helyreállítási helyreplikálásának alkalmazás független, hello javaslatok itt megadott várható toohold a következő helyzetekben is.</span><span class="sxs-lookup"><span data-stu-id="57cf2-119">As site recovery replication is application agnostic, hello recommendations provided here are expected toohold on following scenarios as well.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="57cf2-120">Forrása és célja</span><span class="sxs-lookup"><span data-stu-id="57cf2-120">Source and target</span></span>

<span data-ttu-id="57cf2-121">**A forgatókönyv**</span><span class="sxs-lookup"><span data-stu-id="57cf2-121">**Scenario**</span></span> | <span data-ttu-id="57cf2-122">**másodlagos hely tooa**</span><span class="sxs-lookup"><span data-stu-id="57cf2-122">**tooa secondary site**</span></span> | <span data-ttu-id="57cf2-123">**tooAzure**</span><span class="sxs-lookup"><span data-stu-id="57cf2-123">**tooAzure**</span></span>
--- | --- | ---
<span data-ttu-id="57cf2-124">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="57cf2-124">**Hyper-V**</span></span> | <span data-ttu-id="57cf2-125">Igen</span><span class="sxs-lookup"><span data-stu-id="57cf2-125">Yes</span></span> | <span data-ttu-id="57cf2-126">Igen</span><span class="sxs-lookup"><span data-stu-id="57cf2-126">Yes</span></span>
<span data-ttu-id="57cf2-127">**VMware**</span><span class="sxs-lookup"><span data-stu-id="57cf2-127">**VMware**</span></span> | <span data-ttu-id="57cf2-128">Igen</span><span class="sxs-lookup"><span data-stu-id="57cf2-128">Yes</span></span> | <span data-ttu-id="57cf2-129">Igen</span><span class="sxs-lookup"><span data-stu-id="57cf2-129">Yes</span></span>
<span data-ttu-id="57cf2-130">**Fizikai kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="57cf2-130">**Physical server**</span></span> | <span data-ttu-id="57cf2-131">Igen</span><span class="sxs-lookup"><span data-stu-id="57cf2-131">Yes</span></span> | <span data-ttu-id="57cf2-132">Igen</span><span class="sxs-lookup"><span data-stu-id="57cf2-132">Yes</span></span>

## <a name="enable-dr-of-dynamics-ax-application-using-azure-site-recovery"></a><span data-ttu-id="57cf2-133">Azure Site Recovery segítségével DR Dynamics AX-alkalmazás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="57cf2-133">Enable DR of Dynamics AX application using Azure Site Recovery</span></span>
### <a name="protect-your-dynamics-ax-application"></a><span data-ttu-id="57cf2-134">A Dynamics AX alkalmazás védelme</span><span class="sxs-lookup"><span data-stu-id="57cf2-134">Protect your Dynamics AX application</span></span>
<span data-ttu-id="57cf2-135">Minden összetevője hello Dynamics AX igényeinek toobe védett tooenable hello teljes alkalmazás replikáció és a helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="57cf2-135">Each component of hello Dynamics AX needs toobe protected tooenable hello complete application replication and recovery.</span></span> <span data-ttu-id="57cf2-136">Ez a szakasz a következőkkel foglalkozik:</span><span class="sxs-lookup"><span data-stu-id="57cf2-136">This section covers:</span></span>

<span data-ttu-id="57cf2-137">**1. Az Active Directory védelme**</span><span class="sxs-lookup"><span data-stu-id="57cf2-137">**1. Protection of Active Directory**</span></span>

<span data-ttu-id="57cf2-138">**2. SQL-réteg védelme**</span><span class="sxs-lookup"><span data-stu-id="57cf2-138">**2. Protection of SQL Tier**</span></span>

<span data-ttu-id="57cf2-139">**3. Alkalmazás-és webes réteg**</span><span class="sxs-lookup"><span data-stu-id="57cf2-139">**3. Protection of App and Web Tiers**</span></span>

<span data-ttu-id="57cf2-140">**4. Hálózati konfigurációja**</span><span class="sxs-lookup"><span data-stu-id="57cf2-140">**4. Networking configuration**</span></span>

<span data-ttu-id="57cf2-141">**5. Helyreállítási terv**</span><span class="sxs-lookup"><span data-stu-id="57cf2-141">**5. Recovery Plan**</span></span>

### <a name="1-setup-ad-and-dns-replication"></a><span data-ttu-id="57cf2-142">1. A telepítő AD és a DNS-replikáció</span><span class="sxs-lookup"><span data-stu-id="57cf2-142">1. Setup AD and DNS replication</span></span>

<span data-ttu-id="57cf2-143">Az Active Directory szükséges a Dynamics AX alkalmazás toofunction hello vész-Helyreállítási helyen.</span><span class="sxs-lookup"><span data-stu-id="57cf2-143">Active Directory is required on hello DR site for Dynamics AX application toofunction.</span></span> <span data-ttu-id="57cf2-144">Nincsenek ajánlott és két választási lehetőség, hello az ügyfél helyszíni környezetben hello összetettsége alapján.</span><span class="sxs-lookup"><span data-stu-id="57cf2-144">There are two recommended choices based on hello complexity of hello customer’s on-premises environment.</span></span>

<span data-ttu-id="57cf2-145">**1. lehetőséget**</span><span class="sxs-lookup"><span data-stu-id="57cf2-145">**Option 1**</span></span>

<span data-ttu-id="57cf2-146">Ha hello vevő kis számú alkalmazást és annak teljes egyetlen tartományvezérlővel rendelkezik helyszíni hely és fog kell feladatátadás hello teljes webhelyet együtt, akkor azt javasoljuk, az ASR-replikáció tooreplicate hello DC gép toosecondary hely ( a alkalmazható hely tooSite és a hely tooAzure).</span><span class="sxs-lookup"><span data-stu-id="57cf2-146">If hello customer has a small number of applications and a single domain controller for his entire on-premises site and will be failing over hello entire site together, then we recommend using ASR-Replication tooreplicate hello DC machine toosecondary site (applicable for both Site tooSite and Site tooAzure).</span></span>

<span data-ttu-id="57cf2-147">**2. lehetőséget**</span><span class="sxs-lookup"><span data-stu-id="57cf2-147">**Option 2**</span></span>

<span data-ttu-id="57cf2-148">Ha hello felhasználói kérelmek nagy számú és fut-e az Active Directory-erdőt és a rendszer feladatátvételi néhány alkalmazás egyszerre, akkor azt javasoljuk, hogy egy további tartományvezérlőt hello vész-Helyreállítási helyen beállítása (másodlagos helyre vagy az Azure-ban).</span><span class="sxs-lookup"><span data-stu-id="57cf2-148">If hello customer has a large number of applications and is running an Active Directory forest and will fail-over few applications at a time, then we recommend setting up an additional domain controller on hello DR site (secondary site or in Azure).</span></span>

<span data-ttu-id="57cf2-149">Tekintse meg a túl[útmutatója a tartományvezérlő elérhetővé tétele a vész-Helyreállítási helyen](site-recovery-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="57cf2-149">Please refer too[companion guide on making a domain controller available on DR site](site-recovery-active-directory.md).</span></span> <span data-ttu-id="57cf2-150">Ez a dokumentum hátralévő indulunk ki vész-Helyreállítási helyen rendelkezésre áll a tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="57cf2-150">For remainder of this document we will assume a DC is available on DR site.</span></span>

### <a name="2-setup-sql-server-replication"></a><span data-ttu-id="57cf2-151">2. SQL Server-replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="57cf2-151">2. Setup SQL Server replication</span></span>
<span data-ttu-id="57cf2-152">Tekintse meg a javasolt beállítást védelmének hello részletes műszaki útmutatást toocompanion útmutató [SQL réteg](site-recovery-sql.md).</span><span class="sxs-lookup"><span data-stu-id="57cf2-152">Please refer toocompanion guide  for detailed technical guidance on hello recommended option for protecting [SQL tier](site-recovery-sql.md).</span></span>

### <a name="3-enable-protection-for-dynamics-ax-client-and-aos-vms"></a><span data-ttu-id="57cf2-153">3. A Dynamics AX-ügyfél és AOS virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="57cf2-153">3. Enable protection for Dynamics AX client and AOS VMs</span></span>
<span data-ttu-id="57cf2-154">Hajtsa végre a megfelelő Azure Site Recovery konfigurációs alapján e hello virtuális gépek vannak telepítve [Hyper-V](site-recovery-hyper-v-site-to-azure.md) vagy a [VMware](site-recovery-vmware-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="57cf2-154">Perform relevant Azure Site Recovery configuration based on whether hello VMs are deployed on [Hyper-V](site-recovery-hyper-v-site-to-azure.md) or on [VMware](site-recovery-vmware-to-azure.md).</span></span>

> [!TIP]
> <span data-ttu-id="57cf2-155">Ajánlott összeomlás-konzisztens gyakoriság tooconfigure érték 15 perc.</span><span class="sxs-lookup"><span data-stu-id="57cf2-155">Recommended Crash consistent frequency tooconfigure is 15 minutes.</span></span>
>

<span data-ttu-id="57cf2-156">hello pillanatkép alatt hello védelmi állapot Dynamics összetevő virtuális gépek "VMware-hely tooAzure" védelmi forgatókönyvben jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="57cf2-156">hello below snapshot shows hello protection status of Dynamics component VMs in ‘VMware site tooAzure’ protection scenario.</span></span>
<span data-ttu-id="57cf2-157">![Védett elemek](./media/site-recovery-dynamics-ax/protecteditems.png)</span><span class="sxs-lookup"><span data-stu-id="57cf2-157">![Protected items ](./media/site-recovery-dynamics-ax/protecteditems.png)</span></span>

### <a name="4-configure-networking"></a><span data-ttu-id="57cf2-158">4. A hálózatkezelés konfigurálását</span><span class="sxs-lookup"><span data-stu-id="57cf2-158">4. Configure Networking</span></span>
<span data-ttu-id="57cf2-159">Konfigurálja a virtuális gép számítási és hálózati beállításai</span><span class="sxs-lookup"><span data-stu-id="57cf2-159">Configure VM Compute and Network Settings</span></span>

<span data-ttu-id="57cf2-160">Hello AX ügyfél és AOS virtuális gépek hálózati beállítások konfigurálása a Azure Site Recovery így hello Virtuálisgép-hálózatok csatolt toohello jobb vész-Helyreállítási hálózati feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="57cf2-160">For hello AX client and AOS VMs configure network settings in Azure Site Recovery so that hello VM networks get attached toohello right DR network after failover.</span></span> <span data-ttu-id="57cf2-161">Győződjön meg ezek a rétegek hello vész-Helyreállítási hálózatot irányítható toohello SQL réteg.</span><span class="sxs-lookup"><span data-stu-id="57cf2-161">Ensure hello DR network for these tiers is routable toohello SQL tier.</span></span>

<span data-ttu-id="57cf2-162">Kiválaszthatja a hello VM hello replikált elemek tooconfigure hello hálózati beállítások hello pillanatkép az alábbi ábrán.</span><span class="sxs-lookup"><span data-stu-id="57cf2-162">You can select hello VM in hello replicated items tooconfigure hello network settings as shown in hello snapshot below.</span></span>

* <span data-ttu-id="57cf2-163">AOS kiszolgálók hello megfelelő rendelkezésre állási csoport kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="57cf2-163">For AOS servers select hello correct availability set.</span></span>

* <span data-ttu-id="57cf2-164">Ha statikus IP-címet használ, akkor adja meg a használni kívánt virtuális gép tootake a hello hello hello IP **cél IP-címet** mező ![hálózati beállításai](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span><span class="sxs-lookup"><span data-stu-id="57cf2-164">If you are using a static IP then specify hello IP that you want hello virtual machine tootake in hello **Target IP** field ![Network Settings ](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)</span></span>



### <a name="5-creating-a-recovery-plan"></a><span data-ttu-id="57cf2-165">5. Helyreállítási terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="57cf2-165">5. Creating a recovery plan</span></span>

<span data-ttu-id="57cf2-166">Az Azure Site Recovery tooautomate hello feladatátvételi folyamat egy helyreállítási tervet is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="57cf2-166">You can create a recovery plan in Azure Site Recovery tooautomate hello failover process.</span></span> <span data-ttu-id="57cf2-167">Adja hozzá a helyreállítási terv hello app réteget és web réteget.</span><span class="sxs-lookup"><span data-stu-id="57cf2-167">Add app tier and web tier in hello Recovery Plan.</span></span> <span data-ttu-id="57cf2-168">Rendezze őket a különböző csoporthoz, amely hello előtér leállítás előtt app réteget.</span><span class="sxs-lookup"><span data-stu-id="57cf2-168">Order them in different groups so that hello front-end shutdown before app tier.</span></span>

1)  <span data-ttu-id="57cf2-169">Jelölje ki az előfizetés hello Azure Site Recovery tárolójából, majd kattintson a helyreállítási tervek csempe.</span><span class="sxs-lookup"><span data-stu-id="57cf2-169">Select hello Azure Site Recovery vault in your subscription and click on ‘Recovery Plans’ tile.</span></span>

2)  <span data-ttu-id="57cf2-170">Kattintson a "+ a helyreállítási terv, és adjon meg egy nevet.</span><span class="sxs-lookup"><span data-stu-id="57cf2-170">Click on ‘+ Recovery plan and specify a name.</span></span>

3)  <span data-ttu-id="57cf2-171">Válassza ki a hello "Forrás" és "Target".</span><span class="sxs-lookup"><span data-stu-id="57cf2-171">Select hello ‘Source’ and ‘Target’.</span></span> <span data-ttu-id="57cf2-172">hello cél Azure-bA vagy másodlagos hely lehet.</span><span class="sxs-lookup"><span data-stu-id="57cf2-172">hello target can be Azure or secondary site.</span></span> <span data-ttu-id="57cf2-173">Abban az esetben, ha úgy dönt, hogy Azure, meg kell adnia hello telepítési modell</span><span class="sxs-lookup"><span data-stu-id="57cf2-173">In case you choose Azure, you must specify hello deployment model</span></span>

![Helyreállítási terv létrehozása](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4)  <span data-ttu-id="57cf2-175">Jelölje ki a hello AOS és az ügyfél virtuális gépek toohello helyreállítási tervet, majd kattintson a ✓.</span><span class="sxs-lookup"><span data-stu-id="57cf2-175">Select hello AOS and client VMs toohello recovery plan and click ✓.</span></span>
<span data-ttu-id="57cf2-176">![Helyreállítási terv létrehozása](./media/site-recovery-dynamics-ax/selectvms.png)</span><span class="sxs-lookup"><span data-stu-id="57cf2-176">![Create Recovery Plan](./media/site-recovery-dynamics-ax/selectvms.png)</span></span>


![Helyreállítási terv](./media/site-recovery-dynamics-ax/recoveryplan.png)

<span data-ttu-id="57cf2-178">Az alábbiakban leírtak szerint számos lépés hozzáadásával testre hello helyreállítási terv Dynamics AX-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="57cf2-178">You can customize hello recovery plan for Dynamics AX application by adding various steps as detailed below.</span></span> <span data-ttu-id="57cf2-179">pillanatkép fent hello hello teljes helyreállítási terv hello lépéseket hozzáadása után jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="57cf2-179">hello above snapshot shows hello complete recovery plan after adding all hello steps.</span></span>

<span data-ttu-id="57cf2-180">*Lépéseket:*</span><span class="sxs-lookup"><span data-stu-id="57cf2-180">*Steps:*</span></span>

<span data-ttu-id="57cf2-181">*1. SQL Server feladatátvételt lépései*</span><span class="sxs-lookup"><span data-stu-id="57cf2-181">*1. SQL Server fail over steps*</span></span>

<span data-ttu-id="57cf2-182">Tekintse meg a túl["SQL Server vész-Helyreállítási megoldás"](site-recovery-sql.md) helyreállítási lépéseket adott tooSQL server részleteit útmutatója.</span><span class="sxs-lookup"><span data-stu-id="57cf2-182">Refer too[‘SQL Server DR Solution’](site-recovery-sql.md) companion guide  for details about recovery steps specific tooSQL server.</span></span>

<span data-ttu-id="57cf2-183">*2. Feladatátvételi csoport 1: Feladatok átadása hello AOS virtuális gépeken a*</span><span class="sxs-lookup"><span data-stu-id="57cf2-183">*2. Failover Group 1: Fail over hello AOS VMs*</span></span>

<span data-ttu-id="57cf2-184">Győződjön meg arról, hogy kiválasztott helyreállítási pont hello PIT lehetséges toohello adatbázisként zárja be, de nem előre.</span><span class="sxs-lookup"><span data-stu-id="57cf2-184">Make sure that hello recovery point selected is as close as possible toohello database PIT but not ahead.</span></span>

<span data-ttu-id="57cf2-185">*3. Parancsfájl: Hozzáadás terheléselosztó (csak az E-A)* (az Azure automation) keresztül parancsfájl hozzáadása után AOS Virtuálisgép-csoport, a load balancer tooit tooadd megjelenik.</span><span class="sxs-lookup"><span data-stu-id="57cf2-185">*3. Script: Add load balancer (Only E-A)* Add a script (via Azure automation) after AOS VM group comes up tooadd a load balancer tooit.</span></span> <span data-ttu-id="57cf2-186">Ezt a feladatot használhatja egy parancsfájl toodo.</span><span class="sxs-lookup"><span data-stu-id="57cf2-186">You can use a script toodo this task.</span></span> <span data-ttu-id="57cf2-187">Tekintse meg a cikk [hogyan tooadd terheléselosztóját többrétegű alkalmazást vész-Helyreállítási](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span><span class="sxs-lookup"><span data-stu-id="57cf2-187">Refer article [how tooadd load balancer for multi-tier application DR](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/)</span></span>

<span data-ttu-id="57cf2-188">*4. Feladatátvételi csoport 2: Hello AX ügyfél virtuális gépek feladatátvételt.*</span><span class="sxs-lookup"><span data-stu-id="57cf2-188">*4. Failover Group 2: Fail over hello AX client VMs.*</span></span>
<span data-ttu-id="57cf2-189">Feladatok átadása hello webes réteg virtuális gépek hello helyreállítási terv részeként.</span><span class="sxs-lookup"><span data-stu-id="57cf2-189">Fail over hello web tier VMs as part of hello recovery plan.</span></span>


### <a name="doing-a-test-failover"></a><span data-ttu-id="57cf2-190">A teszt feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="57cf2-190">Doing a test failover</span></span>

<span data-ttu-id="57cf2-191">Tekintse meg a vész-Helyreállítási megoldás too'AD "és"SQL Server vész-Helyreállítási megoldás"kiegészítő útmutatók adott tooAD szempontok és SQL server feladatátvételi teszt során.</span><span class="sxs-lookup"><span data-stu-id="57cf2-191">Refer too‘AD DR Solution ’ and ‘SQL Server DR solution ’ companion guides for considerations specific tooAD and SQL server respectively during Test Failover.</span></span>

1.  <span data-ttu-id="57cf2-192">Nyissa meg tooAzure portálon, és válassza ki a Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="57cf2-192">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="57cf2-193">Kattintson a hello helyreállítási tervben készült Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="57cf2-193">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="57cf2-194">Kattintson a "Test Failover".</span><span class="sxs-lookup"><span data-stu-id="57cf2-194">Click on ‘Test Failover’.</span></span>
4.  <span data-ttu-id="57cf2-195">Válassza ki a hello virtuális hálózati toostart hello teszt feladatátvételi folyamatot.</span><span class="sxs-lookup"><span data-stu-id="57cf2-195">Select hello virtual network toostart hello test fail-over process.</span></span>
5.  <span data-ttu-id="57cf2-196">Hello másodlagos környezetben működik, ha az érvényesítést végezheti el.</span><span class="sxs-lookup"><span data-stu-id="57cf2-196">Once hello secondary environment is up, you can perform your validations.</span></span>
6.  <span data-ttu-id="57cf2-197">Ha hello ellenőrzés befejeződött, kiválaszthatja, hogy érvényesítést végrehajtani, és hello teszt feladatátvételi környezet törölve lesznek.</span><span class="sxs-lookup"><span data-stu-id="57cf2-197">Once hello validations are complete, you can select ‘Validations complete’ and hello test failover environment will be cleaned.</span></span>

<span data-ttu-id="57cf2-198">Hajtsa végre a [Ez az útmutató](site-recovery-test-failover-to-azure.md) toodo feladatátvételi tesztet.</span><span class="sxs-lookup"><span data-stu-id="57cf2-198">Follow [this guidance](site-recovery-test-failover-to-azure.md) toodo a test failover.</span></span>

### <a name="doing-a-failover"></a><span data-ttu-id="57cf2-199">A feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="57cf2-199">Doing a failover</span></span>

1.  <span data-ttu-id="57cf2-200">Nyissa meg tooAzure portálon, és válassza ki a Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="57cf2-200">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="57cf2-201">Kattintson a hello helyreállítási tervben készült Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="57cf2-201">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="57cf2-202">Kattintson a "Failover", és válassza a "Failover".</span><span class="sxs-lookup"><span data-stu-id="57cf2-202">Click on ‘Failover’ and select ‘ Failover’.</span></span>
4.  <span data-ttu-id="57cf2-203">Jelölje ki a hello célhálózat, majd kattintson a ✓ toostart hello feladatátvételi folyamat.</span><span class="sxs-lookup"><span data-stu-id="57cf2-203">Select hello target network and click ✓ toostart hello failover process.</span></span>

<span data-ttu-id="57cf2-204">Hajtsa végre a [Ez az útmutató](site-recovery-failover.md) Ha feladatátvételt végez.</span><span class="sxs-lookup"><span data-stu-id="57cf2-204">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

### <a name="perform-a-failback"></a><span data-ttu-id="57cf2-205">A feladat-visszavételt végrehajtani</span><span class="sxs-lookup"><span data-stu-id="57cf2-205">Perform a Failback</span></span>

<span data-ttu-id="57cf2-206">Tekintse meg a kiszolgáló vész-Helyreállítási megoldás too'SQL "útmutatója szempontok adott tooSQL kiszolgáló feladat-visszavétel során.</span><span class="sxs-lookup"><span data-stu-id="57cf2-206">Refer too‘SQL Server DR Solution ’ companion guide for considerations specific tooSQL server during Failback.</span></span>

1.  <span data-ttu-id="57cf2-207">Nyissa meg tooAzure portálon, és válassza ki a Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="57cf2-207">Go tooAzure  portal and select your Site Recovery vault.</span></span>
2.  <span data-ttu-id="57cf2-208">Kattintson a hello helyreállítási tervben készült Dynamics AX.</span><span class="sxs-lookup"><span data-stu-id="57cf2-208">Click on hello recovery plan created for Dynamics AX.</span></span>
3.  <span data-ttu-id="57cf2-209">Kattintson a "Failover", és válassza ki a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="57cf2-209">Click on ‘Failover’ and select failover.</span></span>
4.  <span data-ttu-id="57cf2-210">Kattintson a "Módosítás irány".</span><span class="sxs-lookup"><span data-stu-id="57cf2-210">Click on ‘Change Direction’.</span></span>
5.  <span data-ttu-id="57cf2-211">Válassza ki hello megfelelő beállítást - adatok szinkronizálása és a virtuális gép létrehozásának beállításai</span><span class="sxs-lookup"><span data-stu-id="57cf2-211">Select hello appropriate options - data synchronization and VM creation options</span></span>
6.  <span data-ttu-id="57cf2-212">Kattintson a ✓ toostart hello feladat-visszavétel"folyamat.</span><span class="sxs-lookup"><span data-stu-id="57cf2-212">Click ✓ toostart hello ‘Failback’ process.</span></span>


<span data-ttu-id="57cf2-213">Hajtsa végre a [Ez az útmutató](site-recovery-failback-azure-to-vmware.md) Ha egy feladat-visszavétel során.</span><span class="sxs-lookup"><span data-stu-id="57cf2-213">Follow [this guidance](site-recovery-failback-azure-to-vmware.md) when you are doing a failback.</span></span>

##<a name="summary"></a><span data-ttu-id="57cf2-214">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="57cf2-214">Summary</span></span>
<span data-ttu-id="57cf2-215">Azure Site Recovery segítségével, a Dynamics AX-alkalmazás egy teljes automatizált vészhelyreállítási tervet hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="57cf2-215">Using Azure Site Recovery, you can create a complete automated disaster recovery plan for your Dynamics AX application.</span></span> <span data-ttu-id="57cf2-216">Hello feladatátvételi bárhonnan másodpercen belül is kezdeményezhető a hello eseményeket, a szüneteltetése és hello alkalmazás első lépéseivel perc múlva.</span><span class="sxs-lookup"><span data-stu-id="57cf2-216">You can initiate hello failover within seconds from anywhere in hello event of a disruption and get hello application up and running in minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="57cf2-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57cf2-217">Next steps</span></span>
<span data-ttu-id="57cf2-218">Olvasási [milyen számítási feladatokat tud védeni?](site-recovery-workload.md) toolearn további információk az Azure Site Recovery vállalati munkaterhelések védelme.</span><span class="sxs-lookup"><span data-stu-id="57cf2-218">Read [What workloads can I protect?](site-recovery-workload.md) toolearn more about protecting enterprise workloads with Azure Site Recovery.</span></span>
