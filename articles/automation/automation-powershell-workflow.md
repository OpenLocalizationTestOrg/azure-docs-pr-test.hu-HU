---
title: "Azure Automation PowerShell-munkafolyamati tanulási |} Microsoft Docs"
description: "Ez a cikk szándék szerint egy gyors lecke használta a Powershellt szerzők adott PowerShell és a PowerShell munkafolyamatok és a vonatkozó Automation-runbook fogalmak közötti különbségek megismeréséhez."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 84bf133e-5343-4e0e-8d6c-bb14304a70db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 4de812c7f863e42a6ed10c2312d61b8377e06431
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a><span data-ttu-id="1b590-103">Tanulási automatizálási runbookok Windows PowerShell munkafolyamat alapfogalmai</span><span class="sxs-lookup"><span data-stu-id="1b590-103">Learning key Windows PowerShell Workflow concepts for Automation runbooks</span></span> 
<span data-ttu-id="1b590-104">Azure Automation Runbookjai Windows PowerShell-munkafolyamatként vannak megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="1b590-104">Runbooks in Azure Automation are implemented as Windows PowerShell Workflows.</span></span>  <span data-ttu-id="1b590-105">A Windows PowerShell munkafolyamat hasonló Windows PowerShell-parancsfájlba, de néhány jelentős különbség az, hogy egy új felhasználóhoz zavaró lehet.</span><span class="sxs-lookup"><span data-stu-id="1b590-105">A Windows PowerShell Workflow is similar to a Windows PowerShell script but has some significant differences that can be confusing to a new user.</span></span>  <span data-ttu-id="1b590-106">Ez a cikk segítséget nyújt a runbookok használatával. a PowerShell-munkafolyamat írása készült, de javasolt PowerShell használatával, ha nem kell az ellenőrzőpontok runbookok írása.</span><span class="sxs-lookup"><span data-stu-id="1b590-106">While this article is intended to help you write runbooks using PowerShell workflow, we recommend you write runbooks using PowerShell unless you need checkpoints.</span></span>  <span data-ttu-id="1b590-107">Több szintaktikai különbségek vannak PowerShell munkafolyamat runbookok létrehozásakor, és ezek a különbségek egy kicsit nagyobb munkahelyi hatékony munkafolyamatok írása szükséges.</span><span class="sxs-lookup"><span data-stu-id="1b590-107">There are several syntax differences when authoring PowerShell Workflow runbooks and these differences require a bit more work to write effective workflows.</span></span>  

<span data-ttu-id="1b590-108">Egy munkafolyamat olyan programozott, összekapcsolódó lépések, amelyek hosszan futó feladatokat végeznek el vagy több lépés összehangolása szükséges több eszközön vagy felügyelt csomóponton keresztül sorozatát.</span><span class="sxs-lookup"><span data-stu-id="1b590-108">A workflow is a sequence of programmed, connected steps that perform long-running tasks or require the coordination of multiple steps across multiple devices or managed nodes.</span></span> <span data-ttu-id="1b590-109">Az egyszerű parancsprogramokhoz képest munkafolyamat előnyei többek között a egyidejűleg elvégezni egy műveletet több eszközön lehetőséget, és képes automatikusan helyreállni hibák után.</span><span class="sxs-lookup"><span data-stu-id="1b590-109">The benefits of a workflow over a normal script include the ability to simultaneously perform an action against multiple devices and the ability to automatically recover from failures.</span></span> <span data-ttu-id="1b590-110">A Windows PowerShell munkafolyamat által használt Windows folyamatkövető alaprendszer Windows PowerShell-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="1b590-110">A Windows PowerShell Workflow is a Windows PowerShell script that uses Windows Workflow Foundation.</span></span> <span data-ttu-id="1b590-111">A munkafolyamat készült Windows PowerShell-szintaxis, és a Windows PowerShell által elindított, feldolgozását a Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="1b590-111">While the workflow is written with Windows PowerShell syntax and launched by Windows PowerShell, it is processed by Windows Workflow Foundation.</span></span>

<span data-ttu-id="1b590-112">Az ebben a cikkben szereplő témakörök a részleteket lásd: [Ismerkedés a Windows PowerShell munkafolyamat](http://technet.microsoft.com/library/jj134242.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b590-112">For complete details on the topics in this article, see [Getting Started with Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx).</span></span>

## <a name="basic-structure-of-a-workflow"></a><span data-ttu-id="1b590-113">A munkafolyamatok alapvető szerkezete</span><span class="sxs-lookup"><span data-stu-id="1b590-113">Basic structure of a workflow</span></span>
<span data-ttu-id="1b590-114">Első lépése a PowerShell-parancsfájl átalakítása egy PowerShell-munkafolyamat van befoglaló azt a **munkafolyamat** kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="1b590-114">The first step to converting a PowerShell script to a PowerShell workflow is enclosing it with the **Workflow** keyword.</span></span>  <span data-ttu-id="1b590-115">Egy munkafolyamat kezdődik-e a **munkafolyamat** kulcsszót a parancsprogram kapcsos zárójelek között elhelyezett törzse követ.</span><span class="sxs-lookup"><span data-stu-id="1b590-115">A workflow starts with the **Workflow** keyword followed by the body of the script enclosed in braces.</span></span> <span data-ttu-id="1b590-116">A munkafolyamat neve követi a **munkafolyamat** kulcsszót, ahogy az a következő szintaxist:</span><span class="sxs-lookup"><span data-stu-id="1b590-116">The name of the workflow follows the **Workflow** keyword as shown in the following syntax:</span></span>

    Workflow Test-Workflow
    {
       <Commands>
    }

<span data-ttu-id="1b590-117">A munkafolyamat nevének meg kell egyeznie az Automation-forgatókönyv neve.</span><span class="sxs-lookup"><span data-stu-id="1b590-117">The name of the workflow must match the name of the Automation runbook.</span></span> <span data-ttu-id="1b590-118">Ha a runbook importálja, majd a fájlnevet meg kell egyeznie a munkafolyamat nevével, és kell végződnie *.ps1*.</span><span class="sxs-lookup"><span data-stu-id="1b590-118">If the runbook is being imported, then the filename must match the workflow name and must end in *.ps1*.</span></span>

<span data-ttu-id="1b590-119">Ha paramétereket szeretne felvenni a munkafolyamathoz, használja a **Param** , ahogy a parancsfájl kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="1b590-119">To add parameters to the workflow, use the **Param** keyword just as you would to a script.</span></span>

## <a name="code-changes"></a><span data-ttu-id="1b590-120">Kódmódosítások</span><span class="sxs-lookup"><span data-stu-id="1b590-120">Code changes</span></span>
<span data-ttu-id="1b590-121">PowerShell-munkafolyamat kódjának jelek majdnem azonos PowerShell parancsfájl kódot néhány jelentős változások kivételével.</span><span class="sxs-lookup"><span data-stu-id="1b590-121">PowerShell workflow code looks almost identical to PowerShell script code except for a few significant changes.</span></span>  <span data-ttu-id="1b590-122">A következő szakaszok ismertetik, amelyek egy PowerShell-parancsfájlba ki azt a munkafolyamat futtatásához szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1b590-122">The following sections describe changes that you need to make to a PowerShell script for it to run in a workflow.</span></span>

### <a name="activities"></a><span data-ttu-id="1b590-123">Tevékenységek</span><span class="sxs-lookup"><span data-stu-id="1b590-123">Activities</span></span>
<span data-ttu-id="1b590-124">Egy tevékenység készen egy adott feladat egy munkafolyamatot belül.</span><span class="sxs-lookup"><span data-stu-id="1b590-124">An activity is a specific task in a workflow.</span></span> <span data-ttu-id="1b590-125">Ugyanúgy, mint a parancsfájl egy vagy több parancsból áll, egy munkafolyamat áll egy vagy több, sorrendben végrehajtott tevékenységből áll.</span><span class="sxs-lookup"><span data-stu-id="1b590-125">Just as a script is composed of one or more commands, a workflow is composed of one or more activities that are carried out in a sequence.</span></span> <span data-ttu-id="1b590-126">Windows PowerShell-munkafolyamat automatikusan alakít számos Windows PowerShell-parancsmagok tevékenységben egy munkafolyamat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="1b590-126">Windows PowerShell Workflow automatically converts many of the Windows PowerShell cmdlets to activities when it runs a workflow.</span></span> <span data-ttu-id="1b590-127">Ha megadja e parancsmagok egyikét a runbookban, a megfelelő tevékenységet futtatja a Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="1b590-127">When you specify one of these cmdlets in your runbook, the corresponding activity is run by Windows Workflow Foundation.</span></span> <span data-ttu-id="1b590-128">Azon parancsmagok esetében nem tartozik megfelelő tevékenység, a Windows PowerShell-munkafolyamat automatikusan futtatja a parancsmagot egy [InlineScript](#inlinescript) tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1b590-128">For those cmdlets without a corresponding activity, Windows PowerShell Workflow automatically runs the cmdlet within an [InlineScript](#inlinescript) activity.</span></span> <span data-ttu-id="1b590-129">Nincs olyan parancsmagokat, amelyek ki vannak zárva és nem használható egy munkafolyamatban, ha nem explicit módon adja meg azokat az InlineScript blokkon belül.</span><span class="sxs-lookup"><span data-stu-id="1b590-129">There is a set of cmdlets that are excluded and cannot be used in a workflow unless you explicitly include them in an InlineScript block.</span></span> <span data-ttu-id="1b590-130">Ezekről a fogalmakról további információkért lásd: [tevékenységek használata parancsfájl-munkafolyamatok](http://technet.microsoft.com/library/jj574194.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b590-130">For further details on these concepts, see [Using Activities in Script Workflows](http://technet.microsoft.com/library/jj574194.aspx).</span></span>

<span data-ttu-id="1b590-131">Munkafolyamat-tevékenységek jellemzőkkel az általános paraméterek konfigurálása a műveletet.</span><span class="sxs-lookup"><span data-stu-id="1b590-131">Workflow activities share a set of common parameters to configure their operation.</span></span> <span data-ttu-id="1b590-132">További általános munkafolyamat-paraméterekkel kapcsolatos további információkért lásd: [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b590-132">For details about the workflow common parameters, see [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="positional-parameters"></a><span data-ttu-id="1b590-133">Pozícióparaméterek</span><span class="sxs-lookup"><span data-stu-id="1b590-133">Positional parameters</span></span>
<span data-ttu-id="1b590-134">A tevékenységek és a munkafolyamat-parancsmagok pozícióparaméterek nem használható.</span><span class="sxs-lookup"><span data-stu-id="1b590-134">You can't use positional parameters with activities and cmdlets in a workflow.</span></span>  <span data-ttu-id="1b590-135">Ez azt jelenti az, hogy a paraméter nevét kell használnia.</span><span class="sxs-lookup"><span data-stu-id="1b590-135">All this means is that you must use parameter names.</span></span>

<span data-ttu-id="1b590-136">Vegye figyelembe például az alábbi kódot, amely lekérdezi az összes futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="1b590-136">For example, consider the following code that gets all running services.</span></span>

     Get-Service | Where-Object {$_.Status -eq "Running"}

<span data-ttu-id="1b590-137">Ha ugyanazt a kódot egy munkafolyamatban való futtatásához, kap egy üzenetet, például "paraméter beállítása nem lehet feloldani a megadott nevesített paraméterek használatával."</span><span class="sxs-lookup"><span data-stu-id="1b590-137">If you try to run this same code in a workflow, you receive a message like "Parameter set cannot be resolved using the specified named parameters."</span></span>  <span data-ttu-id="1b590-138">Ennek elkerülése érdekében adja meg a paraméter neve, ahogy a következő.</span><span class="sxs-lookup"><span data-stu-id="1b590-138">To correct this, provide the parameter name as in the following.</span></span>

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a><span data-ttu-id="1b590-139">Deszerializált objektum</span><span class="sxs-lookup"><span data-stu-id="1b590-139">Deserialized objects</span></span>
<span data-ttu-id="1b590-140">A munkafolyamatokban objektumok deszerializálni vannak.</span><span class="sxs-lookup"><span data-stu-id="1b590-140">Objects in workflows are deserialized.</span></span>  <span data-ttu-id="1b590-141">Ez azt jelenti, hogy a tulajdonságai továbbra is elérhetők, de nem a módszerek.</span><span class="sxs-lookup"><span data-stu-id="1b590-141">This means that their properties are still available, but not their methods.</span></span>  <span data-ttu-id="1b590-142">Vegyük példaként a következő PowerShell-kódot, amely leállítja a szolgáltatást a Stop metódussal a szolgáltatás objektum.</span><span class="sxs-lookup"><span data-stu-id="1b590-142">For example, consider the following PowerShell code that stops a service using the Stop method of the Service object.</span></span>

    $Service = Get-Service -Name MyService
    $Service.Stop()

<span data-ttu-id="1b590-143">Ha ez a munkafolyamat futtatásához, hibaüzenet arról, hogy "a metódushívás nem támogatott a Windows PowerShell-munkafolyamat."</span><span class="sxs-lookup"><span data-stu-id="1b590-143">If you try to run this in a workflow, you receive an error saying "Method invocation is not supported in a Windows PowerShell Workflow."</span></span>  

<span data-ttu-id="1b590-144">Egy elem a kód két sort csomagolásához egy [InlineScript](#inlinescript) letiltása $Service esetben egy service objektum a blokkon belül lenne.</span><span class="sxs-lookup"><span data-stu-id="1b590-144">One option is to wrap these two lines of code in an [InlineScript](#inlinescript) block in which case $Service would be a service object within the block.</span></span>

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

<span data-ttu-id="1b590-145">Egy másik lehetőség egy használja egy másik parancsmagot, a módszer ugyanazt a funkciót hajtja végre, ha elérhető ilyen.</span><span class="sxs-lookup"><span data-stu-id="1b590-145">Another option is to use another cmdlet that performs the same functionality as the method, if one is available.</span></span>  <span data-ttu-id="1b590-146">A mintában a Stop-Service parancsmag a Stop metódus azonos funkciókat biztosítják, és a következő munkafolyamat használhat.</span><span class="sxs-lookup"><span data-stu-id="1b590-146">In our sample, the Stop-Service cmdlet provides the same functionality as the Stop method, and you could use the following for a workflow.</span></span>

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a><span data-ttu-id="1b590-147">Az InlineScript</span><span class="sxs-lookup"><span data-stu-id="1b590-147">InlineScript</span></span>
<span data-ttu-id="1b590-148">A **InlineScript** tevékenység akkor hasznos, ha egy vagy több parancsból PowerShell munkafolyamat helyett a hagyományos PowerShell parancsfájl futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="1b590-148">The **InlineScript** activity is useful when you need to run one or more commands as traditional PowerShell script instead of PowerShell workflow.</span></span>  <span data-ttu-id="1b590-149">Egy munkafolyamat-parancsok Windows Workflow Foundation feldolgozásra kerülnek, amíg InlineScript blokkon belül lévő parancsok Windows PowerShell dolgoznak fel.</span><span class="sxs-lookup"><span data-stu-id="1b590-149">While commands in a workflow are sent to Windows Workflow Foundation for processing, commands in an InlineScript block are processed by Windows PowerShell.</span></span>

<span data-ttu-id="1b590-150">Az InlineScript szintaxisa a következő alább látható.</span><span class="sxs-lookup"><span data-stu-id="1b590-150">InlineScript uses the following syntax shown below.</span></span>

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

<span data-ttu-id="1b590-151">A kimeneti rendel egy változó egy InlineScript kimeneti adhatja vissza.</span><span class="sxs-lookup"><span data-stu-id="1b590-151">You can return output from an InlineScript by assigning the output to a variable.</span></span> <span data-ttu-id="1b590-152">A következő példa a szolgáltatás leáll, és majd exportálja a szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="1b590-152">The following example stops a service and then outputs the service name.</span></span>

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="1b590-153">Az InlineScript blokkon belül lehet értéket átadni, de kell használnia **$Using** hatókör-módosítót.</span><span class="sxs-lookup"><span data-stu-id="1b590-153">You can pass values into an InlineScript block, but you must use **$Using** scope modifier.</span></span>  <span data-ttu-id="1b590-154">A következő példa megegyezik az előző példához, azzal a különbséggel, hogy a szolgáltatás neve változó által biztosított.</span><span class="sxs-lookup"><span data-stu-id="1b590-154">The following example is identical to the previous example except that the service name is provided by a variable.</span></span>

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"

        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="1b590-155">Amikor az InlineScript tevékenység lehet, hogy bizonyos munkafolyamatok alapvető fontosságú, azok nem támogatják a munkafolyamat szerkezetek, és csak kell használni, amikor erre szükség van a következő okok miatt:</span><span class="sxs-lookup"><span data-stu-id="1b590-155">While InlineScript activities may be critical in certain workflows, they do not support workflow constructs and should only be used when necessary for the following reasons:</span></span>

* <span data-ttu-id="1b590-156">Nem használhat [ellenőrzőpontokat](#checkpoints) egy InlineScript blokkon belül.</span><span class="sxs-lookup"><span data-stu-id="1b590-156">You cannot use [checkpoints](#checkpoints) inside an InlineScript block.</span></span> <span data-ttu-id="1b590-157">Ha hiba történik a blokkon belül, a blokk kezdetétől lehet folytatni.</span><span class="sxs-lookup"><span data-stu-id="1b590-157">If a failure occurs within the block, it must be resumed from the beginning of the block.</span></span>
* <span data-ttu-id="1b590-158">Nem használhat [párhuzamos végrehajtása](#parallel-processing) egy InlineScriptBlock belül.</span><span class="sxs-lookup"><span data-stu-id="1b590-158">You cannot use [parallel execution](#parallel-processing) inside an InlineScriptBlock.</span></span>
* <span data-ttu-id="1b590-159">InlineScript hatással van a munkafolyamat méretezhetőségét, mert magánál tartja a Windows PowerShell-munkamenetben az InlineScript blokk teljes hosszán.</span><span class="sxs-lookup"><span data-stu-id="1b590-159">InlineScript affects scalability of the workflow since it holds the Windows PowerShell session for the entire length of the InlineScript block.</span></span>

<span data-ttu-id="1b590-160">További információ az InlineScript használatával, lásd: [Windows PowerShell-parancsok futtatása munkafolyamatban](http://technet.microsoft.com/library/jj574197.aspx) és [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b590-160">For more information on using InlineScript, see [Running Windows PowerShell Commands in a Workflow](http://technet.microsoft.com/library/jj574197.aspx) and [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span></span>

## <a name="parallel-processing"></a><span data-ttu-id="1b590-161">Párhuzamos feldolgozás</span><span class="sxs-lookup"><span data-stu-id="1b590-161">Parallel processing</span></span>
<span data-ttu-id="1b590-162">A Windows PowerShell-munkafolyamatok egyik előnye hajthatnak végre parancsokat párhuzamos ahelyett, hogy egymás után, mint a szokásos parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1b590-162">One advantage of Windows PowerShell Workflows is the ability to perform a set of commands in parallel instead of sequentially as with a typical script.</span></span>

<span data-ttu-id="1b590-163">Használhatja a **párhuzamos** kulcsszót a parancsprogram-blokkot tartalmazzon egyidejűleg futó parancsokat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1b590-163">You can use the **Parallel** keyword to create a script block with multiple commands that run concurrently.</span></span> <span data-ttu-id="1b590-164">Ez szintaxisa a következő alább látható.</span><span class="sxs-lookup"><span data-stu-id="1b590-164">This uses the following syntax shown below.</span></span> <span data-ttu-id="1b590-165">Ebben az esetben Activity1 és az Activity2 elindul egy időben.</span><span class="sxs-lookup"><span data-stu-id="1b590-165">In this case, Activity1 and Activity2 starts at the same time.</span></span> <span data-ttu-id="1b590-166">Activity3 csak az Activity1 és az Activity2 befejezése után elindul.</span><span class="sxs-lookup"><span data-stu-id="1b590-166">Activity3 starts only after both Activity1 and Activity2 have completed.</span></span>

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


<span data-ttu-id="1b590-167">Vegyük példaként a következő PowerShell-parancsokat, amelyek több fájl másolása egy hálózati célra.</span><span class="sxs-lookup"><span data-stu-id="1b590-167">For example, consider the following PowerShell commands that copy multiple files to a network destination.</span></span>  <span data-ttu-id="1b590-168">Ezek a parancsok egymás után futnak, úgy, hogy egy fájl Befejezés másolása a következő megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="1b590-168">These commands are run sequentially so that one file must finish copying before the next is started.</span></span>     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

<span data-ttu-id="1b590-169">Az alábbi munkafolyamat fut. ugyanezek a parancsok párhuzamosan, hogy az összes egyszerre másolásának megkezdése</span><span class="sxs-lookup"><span data-stu-id="1b590-169">The following workflow runs these same commands in parallel so that they all start copying at the same time.</span></span>  <span data-ttu-id="1b590-170">Csak azok az összes után másolja a létrehozása után üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1b590-170">Only after they are all copied is the completion message displayed.</span></span>

    Workflow Copy-Files
    {
        Parallel
        {
            Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


<span data-ttu-id="1b590-171">Használhatja a **ForEach-Parallel** szerkezet használatával egyszerre egy gyűjtemény minden elemére feldolgozhat parancsokat.</span><span class="sxs-lookup"><span data-stu-id="1b590-171">You can use the **ForEach -Parallel** construct to process commands for each item in a collection concurrently.</span></span> <span data-ttu-id="1b590-172">A gyűjtemény elemeinek feldolgozása párhuzamosan történik, amíg a parancsfájlblokkban lévő parancsok egymás után futnak.</span><span class="sxs-lookup"><span data-stu-id="1b590-172">The items in the collection are processed in parallel while the commands in the script block run sequentially.</span></span> <span data-ttu-id="1b590-173">Ez szintaxisa a következő alább látható.</span><span class="sxs-lookup"><span data-stu-id="1b590-173">This uses the following syntax shown below.</span></span> <span data-ttu-id="1b590-174">Ebben az esetben Activity1 a gyűjtemény összes elemének azonos időpontban kezdődik.</span><span class="sxs-lookup"><span data-stu-id="1b590-174">In this case, Activity1 starts at the same time for all items in the collection.</span></span> <span data-ttu-id="1b590-175">Minden elemhez a runbook az Activity2 Activity1 befejeződése után elindul.</span><span class="sxs-lookup"><span data-stu-id="1b590-175">For each item, Activity2 starts after Activity1 is complete.</span></span> <span data-ttu-id="1b590-176">Activity3 csak az Activity1 és az Activity2 az összes befejezése után elindul.</span><span class="sxs-lookup"><span data-stu-id="1b590-176">Activity3 starts only after both Activity1 and Activity2 have completed for all items.</span></span>

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

<span data-ttu-id="1b590-177">A következő példa az előző példában a fájlok másolása a párhuzamos hasonlít.</span><span class="sxs-lookup"><span data-stu-id="1b590-177">The following example is similar to the previous example copying files in parallel.</span></span>  <span data-ttu-id="1b590-178">Ebben az esetben egy üzenet jelenik meg a fájl másolását követően.</span><span class="sxs-lookup"><span data-stu-id="1b590-178">In this case, a message is displayed for each file after it copies.</span></span>  <span data-ttu-id="1b590-179">Csak azután minden teljesen másolta az utolsó üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1b590-179">Only after they are all completely copied is the final completion message displayed.</span></span>

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }

        Write-Output "All files copied."
    }

> [!NOTE]
> <span data-ttu-id="1b590-180">A gyermekrunbookok a párhuzamosan futó, mivel ez kimutatták, hogy megbízhatatlan eredményekhez nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="1b590-180">We do not recommend running child runbooks in parallel since this has been shown to give unreliable results.</span></span>  <span data-ttu-id="1b590-181">Egyes esetekben a gyermekrunbook kimenetét nem jelenik meg, és egy gyermek runbook beállítás hatással lehet a más párhuzamos gyermekrunbookokra</span><span class="sxs-lookup"><span data-stu-id="1b590-181">The output from the child runbook sometimes does not show up, and settings in one child runbook can affect the other parallel child runbooks</span></span>
>

## <a name="checkpoints"></a><span data-ttu-id="1b590-182">Az ellenőrzőpontok</span><span class="sxs-lookup"><span data-stu-id="1b590-182">Checkpoints</span></span>
<span data-ttu-id="1b590-183">A *ellenőrzőpont* az aktuális állapot, amely tartalmazza a változók aktuális értékét és bármi addig létrejött adott pontra a munkafolyamat egy pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="1b590-183">A *checkpoint* is a snapshot of the current state of the workflow that includes the current value for variables and any output generated to that point.</span></span> <span data-ttu-id="1b590-184">Ha egy munkafolyamatot fejeződik be a hiba, vagy fel van függesztve, majd a következő futtatáskor elindul, azok az utolsó ellenőrzőponttól helyett a worfklow elindítása.</span><span class="sxs-lookup"><span data-stu-id="1b590-184">If a workflow ends in error or is suspended, then the next time it is run it will start from its last checkpoint instead of the start of the worfklow.</span></span>  <span data-ttu-id="1b590-185">Egy munkafolyamatban állíthat be ellenőrzőpontot a **Checkpoint-Workflow** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1b590-185">You can set a checkpoint in a workflow with the **Checkpoint-Workflow** activity.</span></span>

<span data-ttu-id="1b590-186">Az alábbi példakód a kivétel activity2 tevékenység után következik be, amely a munkafolyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="1b590-186">In the following sample code, an exception occurs after Activity2 causing the workflow to end.</span></span> <span data-ttu-id="1b590-187">Ha a munkafolyamat fut újra, runbook az Activity2 futtatásával, hisz itt volt beállítva az utolsó ellenőrzőpont után kezdődik.</span><span class="sxs-lookup"><span data-stu-id="1b590-187">When the workflow is run again, it starts by running Activity2 since this was just after the last checkpoint set.</span></span>

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

<span data-ttu-id="1b590-188">Akkor célszerű ellenőrzőpontokat beállítani egy munkafolyamat tevékenységek, amelyek nagyobb eséllyel okozhatnak kivételt, és nem kell ismételni, ha a munkafolyamat folytatása után.</span><span class="sxs-lookup"><span data-stu-id="1b590-188">You should set checkpoints in a workflow after activities that may be prone to exception and should not be repeated if the workflow is resumed.</span></span> <span data-ttu-id="1b590-189">A munkafolyamat lehet, hogy hozzon létre például egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="1b590-189">For example, your workflow may create a virtual machine.</span></span> <span data-ttu-id="1b590-190">Beállíthat egy ellenőrzőpontot előtt és után a virtuális gép létrehozására szolgáló parancsok.</span><span class="sxs-lookup"><span data-stu-id="1b590-190">You could set a checkpoint both before and after the commands to create the virtual machine.</span></span> <span data-ttu-id="1b590-191">Ha a létrehozás sikertelen, majd a parancsok volna ismételni, ha a munkafolyamat újra elindult.</span><span class="sxs-lookup"><span data-stu-id="1b590-191">If the creation fails, then the commands would be repeated if the workflow is started again.</span></span> <span data-ttu-id="1b590-192">Ha a worfklow meghiúsul, miután a létrehozás sikeresen befejeződik, majd a virtuális gép nem létrehozza újra a munkafolyamat folytatásakor.</span><span class="sxs-lookup"><span data-stu-id="1b590-192">If the worfklow fails after the creation succeeds, then the virtual machine will not be created again when the workflow is resumed.</span></span>

<span data-ttu-id="1b590-193">Az alábbi példában több fájlokat másolja fel egy hálózati helyre, és beállítja az ellenőrzőpont után minden egyes fájl.</span><span class="sxs-lookup"><span data-stu-id="1b590-193">The following example copies multiple files to a network location and sets a checkpoint after each file.</span></span>  <span data-ttu-id="1b590-194">Ha a hálózati hely nem vesztek el, majd a munkafolyamat fejeződik be a hiba.</span><span class="sxs-lookup"><span data-stu-id="1b590-194">If the network location is lost, then the workflow ends in error.</span></span>  <span data-ttu-id="1b590-195">Ha ismét elindul, az utolsó ellenőrzőpont, ami azt jelenti, hogy csak a már másolt fájlok kimarad, fog folytatódni.</span><span class="sxs-lookup"><span data-stu-id="1b590-195">When it is started again, it will resume at the last checkpoint meaning that only the files that have already been copied are skipped.</span></span>

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files)
        {
            Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }

        Write-Output "All files copied."
    }

<span data-ttu-id="1b590-196">Mivel a username hitelesítő hívása után nem maradnak a [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) tevékenység vagy az utolsó ellenőrzőpont után be kell állítani a hitelesítő adatokat. null, és ezután kérheti le azokat újra után az eszköz áruházból **Suspend-Workflow** vagy ellenőrzőpont nevezik.</span><span class="sxs-lookup"><span data-stu-id="1b590-196">Because username credentials are not persisted after you call the [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activity or after the last checkpoint, you need to set the credentials to null and then retrieve them again from the asset store after **Suspend-Workflow** or checkpoint is called.</span></span>  <span data-ttu-id="1b590-197">Ellenkező esetben a következő hibaüzenet jelenhet: *a munkafolyamat-feladatot nem lehet folytatni, vagy mert adatmegőrzési adatok nem teljesen mentve, vagy nem mentett adatok megőrzését sérült. A munkafolyamat újra kell indítani.*</span><span class="sxs-lookup"><span data-stu-id="1b590-197">Otherwise, you may receive the following error message: *The workflow job cannot be resumed, either because persistence data could not be saved completely, or saved persistence data has been corrupted. You must restart the workflow.*</span></span>

<span data-ttu-id="1b590-198">A következő ugyanazt a kódot kezelni ezt a PowerShell-munkafolyamati forgatókönyvek mutatja be.</span><span class="sxs-lookup"><span data-stu-id="1b590-198">The following same code demonstrates how to handle this in your PowerShell Workflow runbooks.</span></span>

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)

          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


<span data-ttu-id="1b590-199">Ez pedig nem szükséges vannak hitelesítéséhez a használatával egy egyszerű szolgáltatással konfigurált futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="1b590-199">This is not required if you are authenticating using a Run As account configured with a service principal.</span></span>  

<span data-ttu-id="1b590-200">Az ellenőrzőpontok kapcsolatos további információkért lásd: [ellenőrzőpontok felvétele Parancsprogramos munkafolyamatba](http://technet.microsoft.com/library/jj574114.aspx).</span><span class="sxs-lookup"><span data-stu-id="1b590-200">For more information about checkpoints, see [Adding Checkpoints to a Script Workflow](http://technet.microsoft.com/library/jj574114.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b590-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b590-201">Next steps</span></span>
* <span data-ttu-id="1b590-202">A PowerShell-alapú munkafolyamat-forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első PowerShell-alapú munkafolyamat-forgatókönyvem](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="1b590-202">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
