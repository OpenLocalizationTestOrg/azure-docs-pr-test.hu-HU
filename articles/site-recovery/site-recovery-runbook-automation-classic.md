---
title: "aaaAdd az Azure automation runbookjai toorecovery tervek hello klasszikus portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure Site Recovery most már lehetővé teszi tooextend helyreállítási tervek segítségével az Azure Automation toocomplete összetett feladatokat helyreállítási tooAzure során"
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: f24eaa62-9dea-4fce-92e1-a72513ca0289
ms.service: site-recovery
ms.devlang: powershell
ms.tgt_pltfrm: na
ms.topic: article
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: 3bb7420911afbce289b656f28823b1923e8af0c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-azure-automation-runbooks-toorecovery-plans-in-hello-classic-portal"></a><span data-ttu-id="64fe9-103">Adja hozzá az Azure automation runbookjai toorecovery tervek hello klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="64fe9-103">Add Azure automation runbooks toorecovery plans in hello classic portal</span></span>
<span data-ttu-id="64fe9-104">Ez az oktatóanyag leírja, hogyan integrálható az Azure Site Recovery Azure Automation tooprovide bővítési toorecovery tervek.</span><span class="sxs-lookup"><span data-stu-id="64fe9-104">This tutorial describes how Azure Site Recovery integrates with Azure Automation tooprovide extensibility toorecovery plans.</span></span> <span data-ttu-id="64fe9-105">A helyreállítási terv lehet levezényelni a védett replikációs toosecondary felhő- és a replikációs tooAzure forgatókönyvek az Azure Site Recovery segítségével virtuális gépeket a helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="64fe9-105">Recovery plans can orchestrate recovery of your virtual machines protected using Azure Site Recovery for both replication toosecondary cloud and replication tooAzure scenarios.</span></span> <span data-ttu-id="64fe9-106">Hello helyreállítási létrehozása során is segítenek **következetesen pontos**, **ismételhető**, és **automatizált**.</span><span class="sxs-lookup"><span data-stu-id="64fe9-106">They also help in making hello recovery **consistently accurate**, **repeatable**, and **automated**.</span></span> <span data-ttu-id="64fe9-107">A virtuális gépek tooAzure vannak feladatátvétele, ha integráció az Azure Automation szolgáltatásban, terjeszti ki a helyreállítási tervek, és lehetővé teszi az funkció tooexecute runbookok, így hatékony automatizálási feladatok.</span><span class="sxs-lookup"><span data-stu-id="64fe9-107">If you are failing over your virtual machines tooAzure, integration with Azure Automation extends the recovery plans and gives you capability tooexecute runbooks, thus allowing powerful automation tasks.</span></span>

<span data-ttu-id="64fe9-108">Ha Ön már nem hallott az Azure Automation még, iratkozzon fel [Itt](https://azure.microsoft.com/services/automation/) , és töltse le a mintaparancsfájlok [Itt](https://azure.microsoft.com/documentation/scripts/).</span><span class="sxs-lookup"><span data-stu-id="64fe9-108">If you have not heard about Azure Automation yet, sign up [here](https://azure.microsoft.com/services/automation/) and download their sample scripts [here](https://azure.microsoft.com/documentation/scripts/).</span></span> <span data-ttu-id="64fe9-109">Tudjon meg többet az [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) , és hogyan tooorchestrate helyreállítási tooAzure helyreállítással tervez [Itt](https://azure.microsoft.com/blog/?p=166264).</span><span class="sxs-lookup"><span data-stu-id="64fe9-109">Read more about [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) and how tooorchestrate recovery tooAzure using recovery plans [here](https://azure.microsoft.com/blog/?p=166264).</span></span>

<span data-ttu-id="64fe9-110">Rövid ebben az oktatóanyagban hogyan integrálhatja az Azure Automation-runbook helyreállítási tervek a következő fog keresni.</span><span class="sxs-lookup"><span data-stu-id="64fe9-110">In this short tutorial, we will look at how you can integrate Azure Automation runbooks into recovery plans.</span></span> <span data-ttu-id="64fe9-111">Azt, amely korábban a kézi beavatkozás szükséges egyszerű feladatok automatizálásához, és tekintse meg, hogyan tooconvert egy többszörös lépésenként helyreállítási egy kattintással indítható helyreállítási művelet.</span><span class="sxs-lookup"><span data-stu-id="64fe9-111">We will automate simple tasks that earlier required manual intervention and see how tooconvert a multi step recovery into a single-click recovery action.</span></span> <span data-ttu-id="64fe9-112">Áttekintjük azt is, hogy hogyan háríthatóak el egy egyszerű parancsprogram Ha azt nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="64fe9-112">We will also look at how you can troubleshoot a simple script if it goes wrong.</span></span>

## <a name="protect-hello-application-tooazure"></a><span data-ttu-id="64fe9-113">Hello alkalmazás tooAzure védelme</span><span class="sxs-lookup"><span data-stu-id="64fe9-113">Protect hello application tooAzure</span></span>
<span data-ttu-id="64fe9-114">Ossza meg velünk kezdődhet egy egyszerű alkalmazást, amely két virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="64fe9-114">Let us begin with a simple application consisting of two virtual machines.</span></span> <span data-ttu-id="64fe9-115">Itt a Fabrikam a HRweb alkalmazás van.</span><span class="sxs-lookup"><span data-stu-id="64fe9-115">Here, we have a HRweb application of Fabrikam.</span></span> <span data-ttu-id="64fe9-116">Fabrikam-HRweb-előtér- és a Fabrikam-Hrweb-háttérrendszer hello két virtuális gép Azure Site Recovery segítségével tooAzure védett.</span><span class="sxs-lookup"><span data-stu-id="64fe9-116">Fabrikam-HRweb-frontend and Fabrikam-Hrweb-backend are hello two virtual machines protected tooAzure using Azure Site Recovery.</span></span> <span data-ttu-id="64fe9-117">tooprotect hello virtuális gépek Azure Site Recovery segítségével hajtsa végre az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="64fe9-117">tooprotect hello virtual machines using Azure Site Recovery, follow hello steps below.</span></span>

1. <span data-ttu-id="64fe9-118">A virtuális gépek védelmének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="64fe9-118">Enable protection for your virtual machines.</span></span>
2. <span data-ttu-id="64fe9-119">Győződjön meg arról, hogy hello virtuális gépek kezdeti replikáció befejezése replikálja.</span><span class="sxs-lookup"><span data-stu-id="64fe9-119">Ensure that hello virtual machines have completed initial replication and are replicating.</span></span>
3. <span data-ttu-id="64fe9-120">Várjon, amíg hello kezdeti replikálása befejeződik, és hello replikációs állapot szerint védett.</span><span class="sxs-lookup"><span data-stu-id="64fe9-120">Wait till hello initial replication completes and hello Replication status says Protected.</span></span>

## ![](media/site-recovery-runbook-automation/01.png)
<span data-ttu-id="64fe9-121">Ebben az oktatóanyagban létre fogunk hozni egy helyreállítási terv hello Fabrikam HRweb alkalmazás toofailover hello alkalmazás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="64fe9-121">In this tutorial, we will create a recovery plan for hello Fabrikam HRweb application toofailover hello application tooAzure.</span></span> <span data-ttu-id="64fe9-122">Majd integrálni fogjuk azt egy runbookhoz, amely létre fog hozni egy olyan végpont a hello Azure virtuális gép tooserve weblapok 80-as porton, a feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="64fe9-122">Then we will integrate it with a runbook that will create an endpoint on hello failed over Azure virtual machine tooserve web pages at port 80.</span></span>

<span data-ttu-id="64fe9-123">Először hozzon létre egy helyreállítási tervet, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="64fe9-123">First, let's create a recovery plan for our application.</span></span>

## <a name="create-hello-recovery-plan"></a><span data-ttu-id="64fe9-124">Hello helyreállítási terv létrehozása</span><span class="sxs-lookup"><span data-stu-id="64fe9-124">Create hello recovery plan</span></span>
<span data-ttu-id="64fe9-125">toorecover hello alkalmazás tooAzure kell toocreate helyreállítási tervet.</span><span class="sxs-lookup"><span data-stu-id="64fe9-125">toorecover hello application tooAzure, you need toocreate a recovery plan.</span></span>
<span data-ttu-id="64fe9-126">A helyreállítási terv, megadhatja, hogy a virtuális gépek helyreállítási hello sorrendjének használatával.</span><span class="sxs-lookup"><span data-stu-id="64fe9-126">Using a recovery plan you can specify hello order of recovery of the virtual machines.</span></span> <span data-ttu-id="64fe9-127">1. csoportba kerülnek hello virtuális gép lesz helyreállítására és először, és hello virtuális gép 2 csoportban kövesse.</span><span class="sxs-lookup"><span data-stu-id="64fe9-127">hello virtual machine placed in group 1 will recover and start first, and then hello virtual machine in group 2 will follow.</span></span>

<span data-ttu-id="64fe9-128">Hozzon létre egy helyreállítási terv, amely alatt a következőképpen néz.</span><span class="sxs-lookup"><span data-stu-id="64fe9-128">Create a Recovery Plan that looks like below.</span></span>

![](media/site-recovery-runbook-automation/12.png)

<span data-ttu-id="64fe9-129">További információk a helyreállítási terv, olvassa el a dokumentációt tooread [Itt](https://msdn.microsoft.com/library/azure/dn788799.aspx "Itt").</span><span class="sxs-lookup"><span data-stu-id="64fe9-129">tooread more about recovery plans, read documentation [here](https://msdn.microsoft.com/library/azure/dn788799.aspx "here").</span></span>

<span data-ttu-id="64fe9-130">Következő lépésként hozzon létre hello szükséges összetevők az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="64fe9-130">Next, let's create hello necessary artifacts in Azure Automation.</span></span>

## <a name="create-hello-automation-account-and-its-assets"></a><span data-ttu-id="64fe9-131">Hello automation-fiók és az eszközök létrehozása</span><span class="sxs-lookup"><span data-stu-id="64fe9-131">Create hello automation account and its assets</span></span>
<span data-ttu-id="64fe9-132">Egy Azure Automation-fiók toocreate forgatókönyv van szüksége.</span><span class="sxs-lookup"><span data-stu-id="64fe9-132">You need an Azure Automation account toocreate runbooks.</span></span> <span data-ttu-id="64fe9-133">Ha még nem rendelkezik fiókkal, keresse meg a tooAzure Automation lapon kimaradásával ![](media/site-recovery-runbook-automation/02.png), és hozzon létre egy új fiókot.</span><span class="sxs-lookup"><span data-stu-id="64fe9-133">If you do not already have an account, navigate tooAzure Automation tab denoted by ![](media/site-recovery-runbook-automation/02.png)and create a new account.</span></span>

1. <span data-ttu-id="64fe9-134">Adjon meg egy név tooidentify a hello fiók.</span><span class="sxs-lookup"><span data-stu-id="64fe9-134">Give hello account a name tooidentify with.</span></span>
2. <span data-ttu-id="64fe9-135">Adjon meg egy földrajzi régiót, ahol azt szeretné, hogy tooplace hello fiók.</span><span class="sxs-lookup"><span data-stu-id="64fe9-135">Specify a geographical region where you want tooplace hello account.</span></span>

<span data-ttu-id="64fe9-136">Ajánlott tooplace hello fiók hello és hello az ASR-tárolónak ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="64fe9-136">It is recommended tooplace hello account in hello same region as hello ASR vault.</span></span>

![](media/site-recovery-runbook-automation/03.png)

<span data-ttu-id="64fe9-137">Ezután hozzon létre a következő eszközök a fiók hello hello.</span><span class="sxs-lookup"><span data-stu-id="64fe9-137">Next, create hello following assets in hello Account.</span></span>

### <a name="add-a-subscription-name-as-asset"></a><span data-ttu-id="64fe9-138">Adja hozzá egy előfizetés nevét eszköz</span><span class="sxs-lookup"><span data-stu-id="64fe9-138">Add a subscription name as asset</span></span>
1. <span data-ttu-id="64fe9-139">Egy új beállítással ![](media/site-recovery-runbook-automation/04.png) a hello Azure Automation-eszközök, és válassza ki a túl![](media/site-recovery-runbook-automation/05.png)</span><span class="sxs-lookup"><span data-stu-id="64fe9-139">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in hello Azure Automation Assets and select too![](media/site-recovery-runbook-automation/05.png)</span></span>
2. <span data-ttu-id="64fe9-140">Válassza ki a változó típusú hello **karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="64fe9-140">Select hello variable type as **String**</span></span>
3. <span data-ttu-id="64fe9-141">Adja meg a változó neve, mint a **AzureSubscriptionName**</span><span class="sxs-lookup"><span data-stu-id="64fe9-141">Specify variable name as **AzureSubscriptionName**</span></span>

   ![](media/site-recovery-runbook-automation/06.png)
4. <span data-ttu-id="64fe9-142">Adja meg a tényleges Azure-előfizetés neve hello változó értékét.</span><span class="sxs-lookup"><span data-stu-id="64fe9-142">Specify your actual Azure Subscription name as hello variable value.</span></span>

   ![](media/site-recovery-runbook-automation/07_1.png)

<span data-ttu-id="64fe9-143">Azonosíthatja, hogy a fiókot az Azure-portálon hello hello beállítások lapján az előfizetés hello nevét.</span><span class="sxs-lookup"><span data-stu-id="64fe9-143">You can identify hello name of your subscription from hello settings page of your account on hello Azure portal.</span></span>

### <a name="add-an-azure-login-credential-as-asset"></a><span data-ttu-id="64fe9-144">Adja hozzá az Azure bejelentkezési hitelesítő adatát eszköz</span><span class="sxs-lookup"><span data-stu-id="64fe9-144">Add an Azure login credential as asset</span></span>
<span data-ttu-id="64fe9-145">Azure Automation szolgáltatásbeli Azure PowerShell tooconnect toothe előfizetés használ, és a hello összetevőinek nem működik.</span><span class="sxs-lookup"><span data-stu-id="64fe9-145">Azure Automation uses Azure PowerShell tooconnect toothe subscription and operates on hello artifacts there.</span></span> <span data-ttu-id="64fe9-146">Ehhez szüksége hitelesítés használatával a Microsoft-fiókjával vagy a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="64fe9-146">For this, you need to authenticate using your Microsoft account or a work or school account.</span></span>
<span data-ttu-id="64fe9-147">Egy eszköz toobe biztonságosan hello runbook által használt hello fiók hitelesítő adatait tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="64fe9-147">You can store hello account credentials in an asset toobe used securely by hello runbook.</span></span>

1. <span data-ttu-id="64fe9-148">Egy új beállítással ![](media/site-recovery-runbook-automation/04.png) a hello Azure Automation-eszközök és kiválasztása![](media/site-recovery-runbook-automation/09.png)</span><span class="sxs-lookup"><span data-stu-id="64fe9-148">Add a new setting ![](media/site-recovery-runbook-automation/04.png) in hello Azure Automation Assets and select ![](media/site-recovery-runbook-automation/09.png)</span></span>
2. <span data-ttu-id="64fe9-149">Válassza ki a hello hitelesítőadat-típus szerint **Windows PowerShell-hitelesítő adat**</span><span class="sxs-lookup"><span data-stu-id="64fe9-149">Select hello Credential type as **Windows PowerShell Credential**</span></span>
3. <span data-ttu-id="64fe9-150">Adja meg a hello néven **AzureCredential**</span><span class="sxs-lookup"><span data-stu-id="64fe9-150">Specify hello name as **AzureCredential**</span></span>

   ![](media/site-recovery-runbook-automation/10.png)
4. <span data-ttu-id="64fe9-151">Adja meg a hello levő felhasználónevet és jelszót toosign-a.</span><span class="sxs-lookup"><span data-stu-id="64fe9-151">Specify hello username and password toosign-in with.</span></span>

<span data-ttu-id="64fe9-152">Most mindkét ezek a beállítások érhetők el az eszközök.</span><span class="sxs-lookup"><span data-stu-id="64fe9-152">Now both these settings are available in your assets.</span></span>

![](media/site-recovery-runbook-automation/11.png)

<span data-ttu-id="64fe9-153">További információ a PowerShell tooconnect tooyour előfizetésébe kap hogyan [Itt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="64fe9-153">More information about how tooconnect tooyour subscription via PowerShell is given [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="64fe9-154">Ezután létrehoz egy forgatókönyvet az Azure Automationben, hozzáadhat egy végpont hello előtér-virtuális gép a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="64fe9-154">Next, you will create a runbook in Azure Automation that can add an endpoint for hello front-end virtual machine after failover.</span></span>

## <a name="azure-automation-context"></a><span data-ttu-id="64fe9-155">Azure automation-környezet</span><span class="sxs-lookup"><span data-stu-id="64fe9-155">Azure automation context</span></span>
<span data-ttu-id="64fe9-156">Az ASR átadja egy környezeti változó toohello runbook toohelp determinisztikus parancsfájlokat írhat.</span><span class="sxs-lookup"><span data-stu-id="64fe9-156">ASR passes a context variable toohello runbook toohelp you write deterministic scripts.</span></span> <span data-ttu-id="64fe9-157">Egy sikerült is, hogy előre jelezhető hello felhőalapú szolgáltatás és a virtuális gép hello hello nevét, de akkor fordul elő, hogy azt nem mindig hello eset toocertain forgatókönyvek használhatók, mint ahol hello neve hello virtuális gép neve lehet, hogy változásai miatt egy hello miatt az Azure-ban toounsupported karaktereket.</span><span class="sxs-lookup"><span data-stu-id="64fe9-157">One could argue that hello names of hello Cloud Service and hello Virtual Machine are predictable, but happens that it is not always hello case owing toocertain scenarios such as hello one where hello name of hello virtual machine name might have changed due toounsupported characters in Azure.</span></span> <span data-ttu-id="64fe9-158">Ezért ezek az információk átadása toohello automatikus rendszer-Helyreállítás helyreállítási terv hello részeként *környezet*.</span><span class="sxs-lookup"><span data-stu-id="64fe9-158">Hence this information is passed toohello ASR recovery plan as part of hello *context*.</span></span>

<span data-ttu-id="64fe9-159">Az alábbiakban példája hello környezeti változó megjelenését.</span><span class="sxs-lookup"><span data-stu-id="64fe9-159">Below is an example of how hello context variable looks.</span></span>

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


<span data-ttu-id="64fe9-160">hello az alábbi táblázat tartalmazza a nevet és leírást a hello környezetben minden egyes változójánál.</span><span class="sxs-lookup"><span data-stu-id="64fe9-160">hello table below contains name and description for each variable in hello context.</span></span>

| <span data-ttu-id="64fe9-161">**Változó neve**</span><span class="sxs-lookup"><span data-stu-id="64fe9-161">**Variable name**</span></span> | <span data-ttu-id="64fe9-162">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="64fe9-162">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="64fe9-163">RecoveryPlanName</span><span class="sxs-lookup"><span data-stu-id="64fe9-163">RecoveryPlanName</span></span> |<span data-ttu-id="64fe9-164">Futtatandó terv nevét.</span><span class="sxs-lookup"><span data-stu-id="64fe9-164">Name of plan being run.</span></span> <span data-ttu-id="64fe9-165">Segítséget nyújt egy nevet adtuk meg művelet elvégzése hello azonos parancsfájl</span><span class="sxs-lookup"><span data-stu-id="64fe9-165">Helps you take action based on name using hello same script</span></span> |
| <span data-ttu-id="64fe9-166">FailoverType</span><span class="sxs-lookup"><span data-stu-id="64fe9-166">FailoverType</span></span> |<span data-ttu-id="64fe9-167">Meghatározza, hogy hello feladatátvételi teszt, tervezett vagy nem tervezett.</span><span class="sxs-lookup"><span data-stu-id="64fe9-167">Specifies whether hello failover is test, planned, or unplanned.</span></span> |
| <span data-ttu-id="64fe9-168">FailoverDirection</span><span class="sxs-lookup"><span data-stu-id="64fe9-168">FailoverDirection</span></span> |<span data-ttu-id="64fe9-169">Adja meg, hogy helyreállítási tooprimary vagy másodlagos</span><span class="sxs-lookup"><span data-stu-id="64fe9-169">Specify whether recovery is tooprimary or secondary</span></span> |
| <span data-ttu-id="64fe9-170">Csoportazonosító</span><span class="sxs-lookup"><span data-stu-id="64fe9-170">GroupID</span></span> |<span data-ttu-id="64fe9-171">Azonosítsa a hello csoportszámmal belül hello helyreállítási terv hello csomag futtatásakor</span><span class="sxs-lookup"><span data-stu-id="64fe9-171">Identify hello group number within hello recovery plan when hello plan is running</span></span> |
| <span data-ttu-id="64fe9-172">VmMap</span><span class="sxs-lookup"><span data-stu-id="64fe9-172">VmMap</span></span> |<span data-ttu-id="64fe9-173">Hello hello csoportban lévő összes virtuális gép tömbje</span><span class="sxs-lookup"><span data-stu-id="64fe9-173">Array of all hello virtual machines in hello group</span></span> |
| <span data-ttu-id="64fe9-174">VMMap kulcs</span><span class="sxs-lookup"><span data-stu-id="64fe9-174">VMMap key</span></span> |<span data-ttu-id="64fe9-175">Egyedi kulcs (GUID) az egyes virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="64fe9-175">Unique key (GUID) for each VM.</span></span> <span data-ttu-id="64fe9-176">Ugyanaz, mint a hello hello virtuális gép VMM-azonosító, ha alkalmazható rendelkezik hello.</span><span class="sxs-lookup"><span data-stu-id="64fe9-176">It's hello same as hello VMM ID of hello virtual machine where applicable.</span></span> |
| <span data-ttu-id="64fe9-177">RoleName</span><span class="sxs-lookup"><span data-stu-id="64fe9-177">RoleName</span></span> |<span data-ttu-id="64fe9-178">Hello Azure virtuális Gépen, amelyik a helyreállítandó neve</span><span class="sxs-lookup"><span data-stu-id="64fe9-178">Name of hello Azure VM that's being recovered</span></span> |
| <span data-ttu-id="64fe9-179">CloudServiceName</span><span class="sxs-lookup"><span data-stu-id="64fe9-179">CloudServiceName</span></span> |<span data-ttu-id="64fe9-180">Azure Cloud Service neve alapján mely hello a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="64fe9-180">Azure Cloud Service name under which hello virtual machine is created.</span></span> |

<span data-ttu-id="64fe9-181">tooidentify hello VmMap kulcs hello környezetben is sikerült nyissa meg a virtuális gép tulajdonságlapján toohello az ASR-ben, és nézze meg hello VM GUID tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="64fe9-181">tooidentify hello VmMap Key in hello context you could also go toohello VM properties page in ASR and look at hello VM GUID property.</span></span>

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a><span data-ttu-id="64fe9-182">Az Automation-runbook Szerző</span><span class="sxs-lookup"><span data-stu-id="64fe9-182">Author an Automation runbook</span></span>
<span data-ttu-id="64fe9-183">Most hozzon létre hello runbook tooopen 80-as port hello előtér virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="64fe9-183">Now create hello runbook tooopen port 80 on hello front-end virtual machine.</span></span>

1. <span data-ttu-id="64fe9-184">Hozzon létre egy új runbookot hello Azure Automation-fiók hello nevű **OpenPort80**</span><span class="sxs-lookup"><span data-stu-id="64fe9-184">Create a new runbook in hello Azure Automation account with hello name **OpenPort80**</span></span>

   ![](media/site-recovery-runbook-automation/14.png)
2. <span data-ttu-id="64fe9-185">Keresse meg a toohello hello runbook Szerző nézet, és írja be a hello vázlat módban.</span><span class="sxs-lookup"><span data-stu-id="64fe9-185">Navigate toohello Author view of hello runbook and enter hello draft mode.</span></span>
3. <span data-ttu-id="64fe9-186">Először adja meg a változó toouse hello hello helyreállítási terv környezetben</span><span class="sxs-lookup"><span data-stu-id="64fe9-186">First specify hello variable toouse as hello recovery plan context</span></span>

   ```
       param (
           [Object]$RecoveryPlanContext
       )

   ```
4. <span data-ttu-id="64fe9-187">Ezután csatlakoztassa toohello feliratkozás hello hitelesítő adatok és az előfizetés neve</span><span class="sxs-lookup"><span data-stu-id="64fe9-187">Next connect toohello subscription using hello credential and subscription name</span></span>

   ```
       $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

       # Connect tooAzure
       $AzureAccount = Add-AzureAccount -Credential $Cred
       $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
       Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
   ```

   <span data-ttu-id="64fe9-188">Vegye figyelembe, hogy használja-e hello Azure eszközök – **AzureCredential** és **AzureSubscriptionName** itt.</span><span class="sxs-lookup"><span data-stu-id="64fe9-188">Note that you use hello Azure assets – **AzureCredential** and **AzureSubscriptionName** here.</span></span>
5. <span data-ttu-id="64fe9-189">Most adja meg a hello végpont adatait, és hello hello virtuális gép legyen tooexpose hello végpont GUID.</span><span class="sxs-lookup"><span data-stu-id="64fe9-189">Now specify hello endpoint details and hello GUID of hello virtual machine for which you want tooexpose hello endpoint.</span></span> <span data-ttu-id="64fe9-190">A case hello előtér-virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="64fe9-190">In this case hello front-end virtual machine.</span></span>

   ```
       # Specify hello parameters toobe used by hello script
       $AEProtocol = "TCP"
       $AELocalPort = 80
       $AEPublicPort = 80
       $AEName = "Port 80 for HTTP"
       $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
   ```

   <span data-ttu-id="64fe9-191">Azt határozza meg, hello Azure-végpont protokoll, a virtuális gép hello helyi port és a csatlakoztatott nyilvános port.</span><span class="sxs-lookup"><span data-stu-id="64fe9-191">This specifies hello Azure endpoint protocol, local port on hello VM and its mapped public port.</span></span> <span data-ttu-id="64fe9-192">Ezek a változók hello végpontok tooVMs hozzáadott Azure parancsok paramétereket igényel.</span><span class="sxs-lookup"><span data-stu-id="64fe9-192">These variables are parameters     required by hello Azure commands that add endpoints tooVMs.</span></span> <span data-ttu-id="64fe9-193">hello VMGUID hello hello virtuális gép toooperate kell a GUID érvényes.</span><span class="sxs-lookup"><span data-stu-id="64fe9-193">hello VMGUID holds hello GUID of hello virtual machine you need toooperate on.</span></span>
6. <span data-ttu-id="64fe9-194">hello parancsfájl most bontsa ki a virtuális gép GUID adott hello hello környezetben, és hozzon létre egy végpontot, által hivatkozott hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="64fe9-194">hello script will now extract hello context for hello given VM GUID and create an endpoint on hello virtual machine referenced by it.</span></span>

   ```
       #Read hello VM GUID from hello context
       $VM = $RecoveryPlanContext.VmMap.$VMGUID

       if ($VM -ne $null)
       {
           # Invoke pipeline commands within an InlineScript

           $EndpointStatus = InlineScript {
               # Invoke hello necessary pipeline commands tooadd a Azure Endpoint tooa specified Virtual Machine
               # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

               $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                   Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                   Update-AzureVM
               Write-Output $Status
           }
       }
   ```
7. <span data-ttu-id="64fe9-195">Amikor ez befejeződik, kattintson a közzététel ![](media/site-recovery-runbook-automation/20.png) tooallow a parancsfájl toobe végrehajtási érhető el.</span><span class="sxs-lookup"><span data-stu-id="64fe9-195">Once this is complete, hit Publish ![](media/site-recovery-runbook-automation/20.png) tooallow your script toobe available for execution.</span></span>

<span data-ttu-id="64fe9-196">hello teljes parancsfájlt az alábbi táblázat a referenciaként a</span><span class="sxs-lookup"><span data-stu-id="64fe9-196">hello complete script is given below for your reference</span></span>

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect tooAzure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify hello parameters toobe used by hello script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read hello VM GUID from hello context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke hello necessary pipeline commands tooadd an Azure Endpoint tooa specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-hello-script-toohello-recovery-plan"></a><span data-ttu-id="64fe9-197">Hello parancsfájl toohello helyreállítási terv hozzáadása</span><span class="sxs-lookup"><span data-stu-id="64fe9-197">Add hello script toohello recovery plan</span></span>
<span data-ttu-id="64fe9-198">Ha készen áll a hello parancsfájl, hozzáadhatja azt korábban létrehozott toohello helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="64fe9-198">Once hello script is ready, you can add it toohello recovery plan that you created earlier.</span></span>

1. <span data-ttu-id="64fe9-199">Hello létrehozott helyreállítási csomagot válasszon tooadd parancsfájl után a csoport 2.</span><span class="sxs-lookup"><span data-stu-id="64fe9-199">In hello recovery plan you created, choose tooadd a script after the group 2.</span></span> ![](media/site-recovery-runbook-automation/15.png)
2. <span data-ttu-id="64fe9-200">Adja meg a parancsfájl nevét.</span><span class="sxs-lookup"><span data-stu-id="64fe9-200">Specify a script name.</span></span> <span data-ttu-id="64fe9-201">Ez az ehhez a parancsprogramhoz a megjelenítő hello helyreállítási terv belül csak egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="64fe9-201">This is just a friendly name for this script for showing within hello Recovery plan.</span></span>
3. <span data-ttu-id="64fe9-202">Válassza ki hello feladatátvételi tooAzure parancsfájl – hello Azure Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="64fe9-202">In hello failover tooAzure script – Select hello Azure Automation Account name.</span></span>
4. <span data-ttu-id="64fe9-203">Azure Runbookokat hello válasszon Ön által létrehozott hello runbookot.</span><span class="sxs-lookup"><span data-stu-id="64fe9-203">In hello Azure Runbooks, select hello runbook you authored.</span></span>

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a><span data-ttu-id="64fe9-204">Elsődleges ügyféloldali parancsprogramok</span><span class="sxs-lookup"><span data-stu-id="64fe9-204">Primary side scripts</span></span>
<span data-ttu-id="64fe9-205">Ha egy feladatátvevő tooAzure állnak végrehajtás alatt, is beállíthatja tooexecute elsődleges ügyféloldali parancsprogramok.</span><span class="sxs-lookup"><span data-stu-id="64fe9-205">When you are executing a failover tooAzure, you can also choose tooexecute primary side scripts.</span></span> <span data-ttu-id="64fe9-206">Ezek a parancsfájlok a feladatátvétel során hello VMM-kiszolgálón fog futni.</span><span class="sxs-lookup"><span data-stu-id="64fe9-206">These scripts will run on hello VMM server during failover.</span></span>
<span data-ttu-id="64fe9-207">Elsődleges ügyféloldali parancsprogramok csak csak a leállás előtti és utáni leállítási fázisból áll.</span><span class="sxs-lookup"><span data-stu-id="64fe9-207">Primary side scripts are only available only for pre-shutdown and post shutdown stages.</span></span> <span data-ttu-id="64fe9-208">Ennek oka az, amikor egy olyan vészhelyzet esetén éri általában nem érhető el várhatóan hello elsődleges hely toobe.</span><span class="sxs-lookup"><span data-stu-id="64fe9-208">This is because we expect hello primary site toobe typically unavailable when a disaster strikes.</span></span>
<span data-ttu-id="64fe9-209">Egy nem tervezett feladatátvétel során csak akkor, ha az elsődleges helyen végrehajtott műveletekről csatlakozott a program megpróbálja toorun hello elsődleges ügyféloldali parancsprogramok.</span><span class="sxs-lookup"><span data-stu-id="64fe9-209">During an unplanned failover, only if you opt in for primary site operations, it will attempt toorun hello primary side scripts.</span></span> <span data-ttu-id="64fe9-210">Ha nincsenek elérhető-e, vagy időtúllépés, hello feladatátvételi továbbra is toorecover hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="64fe9-210">If they are not reachable or timeout, hello failover will continue toorecover hello virtual machines.</span></span>
<span data-ttu-id="64fe9-211">Elsődleges ügyféloldali parancsprogramok esetén VMware vagy fizikai/Hyper-v helyek nélkül védett VMM tooAzure - feladatátvételi tooAzure közben nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="64fe9-211">Primary side scripts are un-available for VMware/Physical/Hyper-v Sites without VMM protected tooAzure - while you failover tooAzure.</span></span>
<span data-ttu-id="64fe9-212">Azonban ha az Azure tooon helyszíni, elsődleges ügyféloldali parancsprogramok (Runbookok) feladatátvételi használható VMware kivételével az összes cél.</span><span class="sxs-lookup"><span data-stu-id="64fe9-212">However, when you failback from Azure tooon-premises, primary side scripts (Runbooks) can be used for all targets except VMware.</span></span>

## <a name="test-hello-recovery-plan"></a><span data-ttu-id="64fe9-213">Tesztelési hello helyreállítási terv</span><span class="sxs-lookup"><span data-stu-id="64fe9-213">Test hello recovery plan</span></span>
<span data-ttu-id="64fe9-214">Hello runbook toohello terv hozzáadását követően kezdeményezheti a feladatátvételi tesztet, és a működés ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="64fe9-214">Once you have added hello runbook toohello plan you can initiate a test failover and see it in action.</span></span> <span data-ttu-id="64fe9-215">Mindig ajánlott toorun a teszt feladatátvételi tootest az alkalmazás- és hello helyreállítási terv tooensure, hogy nincsenek-e hibák.</span><span class="sxs-lookup"><span data-stu-id="64fe9-215">It is always recommended toorun a test failover tootest your application and hello recovery plan tooensure that there are no errors.</span></span>

1. <span data-ttu-id="64fe9-216">Válassza ki a helyreállítási terv hello és a feladatátvételi teszt indíthatnak.</span><span class="sxs-lookup"><span data-stu-id="64fe9-216">Select hello recovery plan and initiate a test failover.</span></span>
2. <span data-ttu-id="64fe9-217">Hello terv végrehajtásakor láthatja, hogy hello runbook által végrehajtott, vagy nem keresztül annak állapotát.</span><span class="sxs-lookup"><span data-stu-id="64fe9-217">During hello plan execution, you can see whether hello runbook has executed or not via its status.</span></span>

   ![](media/site-recovery-runbook-automation/17.png)
3. <span data-ttu-id="64fe9-218">Azt is láthatja, hello részletes hello Azure Automation feladatok lapon hello runbook runbook végrehajtási állapota.</span><span class="sxs-lookup"><span data-stu-id="64fe9-218">You can also see hello detailed runbook execution status on hello Azure Automation jobs page for hello runbook.</span></span>

   ![](media/site-recovery-runbook-automation/18.png)
4. <span data-ttu-id="64fe9-219">Hello feladatátvételi hello forgatókönyv végrehajtási eredményének, leszámítva befejeződése után megtekintheti a hello végrehajtása sikeres-e, vagy nem a hello Azure virtuális gép meglátogatása, és megnézi a hello végpontok.</span><span class="sxs-lookup"><span data-stu-id="64fe9-219">After hello failover completes, apart from hello runbook execution result, you can see whether hello execution is successful or not by visiting hello Azure virtual machine page and looking at hello endpoints.</span></span>

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a><span data-ttu-id="64fe9-220">Mintaszkriptek</span><span class="sxs-lookup"><span data-stu-id="64fe9-220">Sample scripts</span></span>
<span data-ttu-id="64fe9-221">Amíg azt telefonon automatizálása egy gyakran használt feladat ebben az oktatóanyagban egy végpont tooan Azure virtuális gép hozzáadása, akkor megteheti más erőteljes automatizálás feladatot használ az Azure automation.</span><span class="sxs-lookup"><span data-stu-id="64fe9-221">While we walked through automating one commonly used task of adding an endpoint tooan Azure virtual machine in this tutorial, you could do a number of other powerful automation tasks using Azure automation.</span></span> <span data-ttu-id="64fe9-222">A Microsoft és a hello Azure Automation közösségi biztosít a példa runbookok, amelyek segítségével elsajátíthatja a saját megoldások és segédprogram runbookok, amelyek nagyobb automatizálási feladatok építőelemeiként használhat.</span><span class="sxs-lookup"><span data-stu-id="64fe9-222">Microsoft and hello Azure Automation community provide sample runbooks which can help you get started creating your own solutions, and utility runbooks, which you can use as building blocks for larger automation tasks.</span></span> <span data-ttu-id="64fe9-223">Indítsa el a használatuk hello gyűjteményből, és az Azure Site Recovery segítségével alkalmazások hatékony egyetlen kattintással a helyreállítási terv létrehozása.</span><span class="sxs-lookup"><span data-stu-id="64fe9-223">Start using them from hello gallery and build  powerful one-click recovery plans for your applications using Azure Site Recovery.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64fe9-224">További források</span><span class="sxs-lookup"><span data-stu-id="64fe9-224">Additional Resources</span></span>
[<span data-ttu-id="64fe9-225">Azure Automation szolgáltatásbeli áttekintése</span><span class="sxs-lookup"><span data-stu-id="64fe9-225">Azure Automation Overview</span></span>](http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure automatizálása – áttekintés")

[<span data-ttu-id="64fe9-226">Azure Automation-parancsfájlok minta</span><span class="sxs-lookup"><span data-stu-id="64fe9-226">Sample Azure Automation Scripts</span></span>](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=User&f\[0\].Value=SC%20Automation%20Product%20Team&f\[0\].Text=SC%20Automation%20Product%20Team "Azure automatizálási parancsfájlokat minta")
