---
title: Azure Automation PowerShell-munkafolyamati aaaLearning |} Microsoft Docs
description: "Ez a cikk szándék szerint egy gyors lecke szerzők ismeri a PowerShell toounderstand hello speciális eltéréseket PowerShell és a PowerShell-munkafolyamati és fogalmak alkalmazható tooAutomation runbookok között."
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
ms.openlocfilehash: 362c504eb96d31b99a826b128e6a591beecaa084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a><span data-ttu-id="10a71-103">Tanulási automatizálási runbookok Windows PowerShell munkafolyamat alapfogalmai</span><span class="sxs-lookup"><span data-stu-id="10a71-103">Learning key Windows PowerShell Workflow concepts for Automation runbooks</span></span> 
<span data-ttu-id="10a71-104">Azure Automation Runbookjai Windows PowerShell-munkafolyamatként vannak megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="10a71-104">Runbooks in Azure Automation are implemented as Windows PowerShell Workflows.</span></span>  <span data-ttu-id="10a71-105">A Windows PowerShell munkafolyamat tooa hasonló Windows PowerShell-parancsfájlt, de néhány jelentős különbség az, amely egyértelmű tooa új felhasználó.</span><span class="sxs-lookup"><span data-stu-id="10a71-105">A Windows PowerShell Workflow is similar tooa Windows PowerShell script but has some significant differences that can be confusing tooa new user.</span></span>  <span data-ttu-id="10a71-106">Ez a cikk nem tervezett toohelp PowerShell munkafolyamat runbookok írása, de javasolt PowerShell használatával, ha nem kell az ellenőrzőpontok runbookok írása.</span><span class="sxs-lookup"><span data-stu-id="10a71-106">While this article is intended toohelp you write runbooks using PowerShell workflow, we recommend you write runbooks using PowerShell unless you need checkpoints.</span></span>  <span data-ttu-id="10a71-107">Több szintaktikai különbségek vannak PowerShell munkafolyamat runbookok létrehozásakor, és ezek a különbségek szükséges egy kicsit nagyobb munkahelyi toowrite hatékony munkafolyamatokat.</span><span class="sxs-lookup"><span data-stu-id="10a71-107">There are several syntax differences when authoring PowerShell Workflow runbooks and these differences require a bit more work toowrite effective workflows.</span></span>  

<span data-ttu-id="10a71-108">Egy munkafolyamat olyan programozott, összekapcsolódó lépések, amelyek hosszan futó feladatokat végeznek el vagy több eszközön vagy felügyelt csomóponton keresztül hello végrehajtandó, több lépés koordinációját igénylik sorozatát.</span><span class="sxs-lookup"><span data-stu-id="10a71-108">A workflow is a sequence of programmed, connected steps that perform long-running tasks or require hello coordination of multiple steps across multiple devices or managed nodes.</span></span> <span data-ttu-id="10a71-109">hello egyszerű parancsprogramokhoz képest munkafolyamat előnyei közé tartozik, hello toosimultaneously egy műveletet több eszközön, és hello képességét tooautomatically helyreállni hibák után.</span><span class="sxs-lookup"><span data-stu-id="10a71-109">hello benefits of a workflow over a normal script include hello ability toosimultaneously perform an action against multiple devices and hello ability tooautomatically recover from failures.</span></span> <span data-ttu-id="10a71-110">A Windows PowerShell munkafolyamat által használt Windows folyamatkövető alaprendszer Windows PowerShell-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="10a71-110">A Windows PowerShell Workflow is a Windows PowerShell script that uses Windows Workflow Foundation.</span></span> <span data-ttu-id="10a71-111">Hello munkafolyamat készült Windows PowerShell-szintaxis, és a Windows PowerShell által elindított, feldolgozását a Windows Workflow Foundation.</span><span class="sxs-lookup"><span data-stu-id="10a71-111">While hello workflow is written with Windows PowerShell syntax and launched by Windows PowerShell, it is processed by Windows Workflow Foundation.</span></span>

<span data-ttu-id="10a71-112">Ebben a cikkben hello kérdésekben teljes részletekért lásd: [Ismerkedés a Windows PowerShell munkafolyamat](http://technet.microsoft.com/library/jj134242.aspx).</span><span class="sxs-lookup"><span data-stu-id="10a71-112">For complete details on hello topics in this article, see [Getting Started with Windows PowerShell Workflow](http://technet.microsoft.com/library/jj134242.aspx).</span></span>

## <a name="basic-structure-of-a-workflow"></a><span data-ttu-id="10a71-113">A munkafolyamatok alapvető szerkezete</span><span class="sxs-lookup"><span data-stu-id="10a71-113">Basic structure of a workflow</span></span>
<span data-ttu-id="10a71-114">hello első lépés tooconverting PowerShell parancsfájl tooa PowerShell munkafolyamat van csatolva hozzá azt hello **munkafolyamat** kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="10a71-114">hello first step tooconverting a PowerShell script tooa PowerShell workflow is enclosing it with hello **Workflow** keyword.</span></span>  <span data-ttu-id="10a71-115">Egy munkafolyamat kezdődik hello **munkafolyamat** kulcsszó hello hello parancsprogram kapcsos zárójelek között elhelyezett törzse követ.</span><span class="sxs-lookup"><span data-stu-id="10a71-115">A workflow starts with hello **Workflow** keyword followed by hello body of hello script enclosed in braces.</span></span> <span data-ttu-id="10a71-116">hello hello munkafolyamat neve követi hello **munkafolyamat** látható módon hello szintaxisa a következő kulcsszó:</span><span class="sxs-lookup"><span data-stu-id="10a71-116">hello name of hello workflow follows hello **Workflow** keyword as shown in hello following syntax:</span></span>

    Workflow Test-Workflow
    {
       <Commands>
    }

<span data-ttu-id="10a71-117">hello munkafolyamat hello nevének meg kell egyeznie az Automation-runbook hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="10a71-117">hello name of hello workflow must match hello name of hello Automation runbook.</span></span> <span data-ttu-id="10a71-118">Ha hello runbook importálja, majd hello fájlnév hello munkafolyamat nevének egyeznie kell, és kell végződnie *.ps1*.</span><span class="sxs-lookup"><span data-stu-id="10a71-118">If hello runbook is being imported, then hello filename must match hello workflow name and must end in *.ps1*.</span></span>

<span data-ttu-id="10a71-119">tooadd paraméterek toohello munkafolyamat, használjon hello **Param** , ahogy tooa parancsfájl kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="10a71-119">tooadd parameters toohello workflow, use hello **Param** keyword just as you would tooa script.</span></span>

## <a name="code-changes"></a><span data-ttu-id="10a71-120">Kódmódosítások</span><span class="sxs-lookup"><span data-stu-id="10a71-120">Code changes</span></span>
<span data-ttu-id="10a71-121">PowerShell-munkafolyamat kódot jelek majdnem azonos tooPowerShell parancsfájlkód néhány jelentős változások kivételével.</span><span class="sxs-lookup"><span data-stu-id="10a71-121">PowerShell workflow code looks almost identical tooPowerShell script code except for a few significant changes.</span></span>  <span data-ttu-id="10a71-122">a következő szakaszok hello változást, hogy toomake tooa PowerShell-parancsfájl ki azt a munkafolyamat toorun ismertetik.</span><span class="sxs-lookup"><span data-stu-id="10a71-122">hello following sections describe changes that you need toomake tooa PowerShell script for it toorun in a workflow.</span></span>

### <a name="activities"></a><span data-ttu-id="10a71-123">Tevékenységek</span><span class="sxs-lookup"><span data-stu-id="10a71-123">Activities</span></span>
<span data-ttu-id="10a71-124">Egy tevékenység készen egy adott feladat egy munkafolyamatot belül.</span><span class="sxs-lookup"><span data-stu-id="10a71-124">An activity is a specific task in a workflow.</span></span> <span data-ttu-id="10a71-125">Ugyanúgy, mint a parancsfájl egy vagy több parancsból áll, egy munkafolyamat áll egy vagy több, sorrendben végrehajtott tevékenységből áll.</span><span class="sxs-lookup"><span data-stu-id="10a71-125">Just as a script is composed of one or more commands, a workflow is composed of one or more activities that are carried out in a sequence.</span></span> <span data-ttu-id="10a71-126">A Windows PowerShell-munkafolyamat automatikusan alakít számos Windows PowerShell parancsmagok tooactivities hello egy munkafolyamat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="10a71-126">Windows PowerShell Workflow automatically converts many of hello Windows PowerShell cmdlets tooactivities when it runs a workflow.</span></span> <span data-ttu-id="10a71-127">Ha megadja e parancsmagok egyikét a runbookban, a Windows Workflow Foundation hello megfelelő tevékenységet futtatja.</span><span class="sxs-lookup"><span data-stu-id="10a71-127">When you specify one of these cmdlets in your runbook, hello corresponding activity is run by Windows Workflow Foundation.</span></span> <span data-ttu-id="10a71-128">Azon parancsmagok esetében nem tartozik megfelelő tevékenység, a Windows PowerShell munkafolyamat automatikusan futtat hello parancsmagot egy [InlineScript](#inlinescript) tevékenység.</span><span class="sxs-lookup"><span data-stu-id="10a71-128">For those cmdlets without a corresponding activity, Windows PowerShell Workflow automatically runs hello cmdlet within an [InlineScript](#inlinescript) activity.</span></span> <span data-ttu-id="10a71-129">Nincs olyan parancsmagokat, amelyek ki vannak zárva és nem használható egy munkafolyamatban, ha nem explicit módon adja meg azokat az InlineScript blokkon belül.</span><span class="sxs-lookup"><span data-stu-id="10a71-129">There is a set of cmdlets that are excluded and cannot be used in a workflow unless you explicitly include them in an InlineScript block.</span></span> <span data-ttu-id="10a71-130">Ezekről a fogalmakról további információkért lásd: [tevékenységek használata parancsfájl-munkafolyamatok](http://technet.microsoft.com/library/jj574194.aspx).</span><span class="sxs-lookup"><span data-stu-id="10a71-130">For further details on these concepts, see [Using Activities in Script Workflows](http://technet.microsoft.com/library/jj574194.aspx).</span></span>

<span data-ttu-id="10a71-131">Munkafolyamat-tevékenységek jellemzőkkel egy közös paraméterek tooconfigure a műveletet.</span><span class="sxs-lookup"><span data-stu-id="10a71-131">Workflow activities share a set of common parameters tooconfigure their operation.</span></span> <span data-ttu-id="10a71-132">Hello általános munkafolyamat-paraméterekkel kapcsolatos részletekért lásd: [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span><span class="sxs-lookup"><span data-stu-id="10a71-132">For details about hello workflow common parameters, see [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).</span></span>

### <a name="positional-parameters"></a><span data-ttu-id="10a71-133">Pozícióparaméterek</span><span class="sxs-lookup"><span data-stu-id="10a71-133">Positional parameters</span></span>
<span data-ttu-id="10a71-134">A tevékenységek és a munkafolyamat-parancsmagok pozícióparaméterek nem használható.</span><span class="sxs-lookup"><span data-stu-id="10a71-134">You can't use positional parameters with activities and cmdlets in a workflow.</span></span>  <span data-ttu-id="10a71-135">Ez azt jelenti az, hogy a paraméter nevét kell használnia.</span><span class="sxs-lookup"><span data-stu-id="10a71-135">All this means is that you must use parameter names.</span></span>

<span data-ttu-id="10a71-136">Vegyük példaként a következő kódot, amely lekérdezi az összes futó szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="10a71-136">For example, consider hello following code that gets all running services.</span></span>

     Get-Service | Where-Object {$_.Status -eq "Running"}

<span data-ttu-id="10a71-137">Ha toorun Ez ugyanazt a kódot egy munkafolyamatban, kap egy üzenetet, például "Paraméter beállítása nem lehet feloldani a megadott hello használatával elnevezett paramétereket."</span><span class="sxs-lookup"><span data-stu-id="10a71-137">If you try toorun this same code in a workflow, you receive a message like "Parameter set cannot be resolved using hello specified named parameters."</span></span>  <span data-ttu-id="10a71-138">toocorrect, adja meg a hello paraméter neve, ahogy hello következő.</span><span class="sxs-lookup"><span data-stu-id="10a71-138">toocorrect this, provide hello parameter name as in hello following.</span></span>

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a><span data-ttu-id="10a71-139">Deszerializált objektum</span><span class="sxs-lookup"><span data-stu-id="10a71-139">Deserialized objects</span></span>
<span data-ttu-id="10a71-140">A munkafolyamatokban objektumok deszerializálni vannak.</span><span class="sxs-lookup"><span data-stu-id="10a71-140">Objects in workflows are deserialized.</span></span>  <span data-ttu-id="10a71-141">Ez azt jelenti, hogy a tulajdonságai továbbra is elérhetők, de nem a módszerek.</span><span class="sxs-lookup"><span data-stu-id="10a71-141">This means that their properties are still available, but not their methods.</span></span>  <span data-ttu-id="10a71-142">Vegyük példaként a következő PowerShell-kódot, amely leállítja a szolgáltatások hello Stop metódus hello szolgáltatás objektum történő hello.</span><span class="sxs-lookup"><span data-stu-id="10a71-142">For example, consider hello following PowerShell code that stops a service using hello Stop method of hello Service object.</span></span>

    $Service = Get-Service -Name MyService
    $Service.Stop()

<span data-ttu-id="10a71-143">Ha megpróbál toorun Ez a munkafolyamat, hibaüzenet arról, hogy "a metódushívás nem támogatott a Windows PowerShell-munkafolyamat."</span><span class="sxs-lookup"><span data-stu-id="10a71-143">If you try toorun this in a workflow, you receive an error saying "Method invocation is not supported in a Windows PowerShell Workflow."</span></span>  

<span data-ttu-id="10a71-144">Egy elem toowrap két sort a kód egy [InlineScript](#inlinescript) esetben $Service lenne egy szolgáltatás-objektummal hello blokk letiltása.</span><span class="sxs-lookup"><span data-stu-id="10a71-144">One option is toowrap these two lines of code in an [InlineScript](#inlinescript) block in which case $Service would be a service object within hello block.</span></span>

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

<span data-ttu-id="10a71-145">Másik lehetőség is toouse végző másik parancsmag hello hello módszerként ugyanezeket a funkciókat, ha elérhető ilyen.</span><span class="sxs-lookup"><span data-stu-id="10a71-145">Another option is toouse another cmdlet that performs hello same functionality as hello method, if one is available.</span></span>  <span data-ttu-id="10a71-146">A mintában szereplő hello szolgáltatás leállítása a parancsmag nyújt hello hello Stop metódus, és azonos funkciókat hello következő használhatja egy munkafolyamathoz.</span><span class="sxs-lookup"><span data-stu-id="10a71-146">In our sample, hello Stop-Service cmdlet provides hello same functionality as hello Stop method, and you could use hello following for a workflow.</span></span>

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a><span data-ttu-id="10a71-147">Az InlineScript</span><span class="sxs-lookup"><span data-stu-id="10a71-147">InlineScript</span></span>
<span data-ttu-id="10a71-148">Hello **InlineScript** tevékenység akkor hasznos, ha szüksége toorun egy vagy több parancsból helyett PowerShell munkafolyamat hagyományos PowerShell-parancsfájlként.</span><span class="sxs-lookup"><span data-stu-id="10a71-148">hello **InlineScript** activity is useful when you need toorun one or more commands as traditional PowerShell script instead of PowerShell workflow.</span></span>  <span data-ttu-id="10a71-149">Egy munkafolyamat-parancsok tooWindows Workflow Foundation kerülnek feldolgozásra, amíg InlineScript blokkon belül lévő parancsok Windows PowerShell dolgoznak fel.</span><span class="sxs-lookup"><span data-stu-id="10a71-149">While commands in a workflow are sent tooWindows Workflow Foundation for processing, commands in an InlineScript block are processed by Windows PowerShell.</span></span>

<span data-ttu-id="10a71-150">Az InlineScript alább látható szintaxist a következő hello használja.</span><span class="sxs-lookup"><span data-stu-id="10a71-150">InlineScript uses hello following syntax shown below.</span></span>

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

<span data-ttu-id="10a71-151">Egy InlineScript kimeneti hello kimeneti tooa változó hozzárendelésével adhatja vissza.</span><span class="sxs-lookup"><span data-stu-id="10a71-151">You can return output from an InlineScript by assigning hello output tooa variable.</span></span> <span data-ttu-id="10a71-152">hello alábbi példa a szolgáltatás leáll, és majd kiírja hello szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="10a71-152">hello following example stops a service and then outputs hello service name.</span></span>

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


<span data-ttu-id="10a71-153">Az InlineScript blokkon belül lehet értéket átadni, de kell használnia **$Using** hatókör-módosítót.</span><span class="sxs-lookup"><span data-stu-id="10a71-153">You can pass values into an InlineScript block, but you must use **$Using** scope modifier.</span></span>  <span data-ttu-id="10a71-154">hello alábbi példa: azonos toohello előző példa azzal a különbséggel, hogy hello szolgáltatásnév által biztosított egy változó.</span><span class="sxs-lookup"><span data-stu-id="10a71-154">hello following example is identical toohello previous example except that hello service name is provided by a variable.</span></span>

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


<span data-ttu-id="10a71-155">Lehet, hogy az InlineScript tevékenység bizonyos munkafolyamatok alapvető fontosságú, amíg azok nem támogatják a munkafolyamat szerkezetek, és csak kell használni, amikor a következő okok miatt hello szükséges:</span><span class="sxs-lookup"><span data-stu-id="10a71-155">While InlineScript activities may be critical in certain workflows, they do not support workflow constructs and should only be used when necessary for hello following reasons:</span></span>

* <span data-ttu-id="10a71-156">Nem használhat [ellenőrzőpontokat](#checkpoints) egy InlineScript blokkon belül.</span><span class="sxs-lookup"><span data-stu-id="10a71-156">You cannot use [checkpoints](#checkpoints) inside an InlineScript block.</span></span> <span data-ttu-id="10a71-157">Ha hiba lép fel, hello blokk belül, hello elejétől hello blokk lehet folytatni.</span><span class="sxs-lookup"><span data-stu-id="10a71-157">If a failure occurs within hello block, it must be resumed from hello beginning of hello block.</span></span>
* <span data-ttu-id="10a71-158">Nem használhat [párhuzamos végrehajtása](#parallel-processing) egy InlineScriptBlock belül.</span><span class="sxs-lookup"><span data-stu-id="10a71-158">You cannot use [parallel execution](#parallel-processing) inside an InlineScriptBlock.</span></span>
* <span data-ttu-id="10a71-159">Az InlineScript méretezhetőséget biztosít a hello munkafolyamat van hatással, mert magánál tartja hello blokk teljes hosszán át hello InlineScript hello Windows PowerShell-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="10a71-159">InlineScript affects scalability of hello workflow since it holds hello Windows PowerShell session for hello entire length of hello InlineScript block.</span></span>

<span data-ttu-id="10a71-160">További információ az InlineScript használatával, lásd: [Windows PowerShell-parancsok futtatása munkafolyamatban](http://technet.microsoft.com/library/jj574197.aspx) és [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span><span class="sxs-lookup"><span data-stu-id="10a71-160">For more information on using InlineScript, see [Running Windows PowerShell Commands in a Workflow](http://technet.microsoft.com/library/jj574197.aspx) and [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).</span></span>

## <a name="parallel-processing"></a><span data-ttu-id="10a71-161">Párhuzamos feldolgozás</span><span class="sxs-lookup"><span data-stu-id="10a71-161">Parallel processing</span></span>
<span data-ttu-id="10a71-162">Egy Windows PowerShell-munkafolyamatok előnye hello képességét tooperform ahelyett, hogy párhuzamosan parancsokat egymás után, mint a szokásos parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="10a71-162">One advantage of Windows PowerShell Workflows is hello ability tooperform a set of commands in parallel instead of sequentially as with a typical script.</span></span>

<span data-ttu-id="10a71-163">Használhatja a hello **párhuzamos** kulcsszó toocreate parancsprogram-blokkot tartalmazzon egyidejűleg futó parancsokat.</span><span class="sxs-lookup"><span data-stu-id="10a71-163">You can use hello **Parallel** keyword toocreate a script block with multiple commands that run concurrently.</span></span> <span data-ttu-id="10a71-164">Ez az alább látható szintaxist a következő hello használ.</span><span class="sxs-lookup"><span data-stu-id="10a71-164">This uses hello following syntax shown below.</span></span> <span data-ttu-id="10a71-165">Ebben az esetben a Activity1 és az Activity2 kezdődik, hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="10a71-165">In this case, Activity1 and Activity2 starts at hello same time.</span></span> <span data-ttu-id="10a71-166">Activity3 csak az Activity1 és az Activity2 befejezése után elindul.</span><span class="sxs-lookup"><span data-stu-id="10a71-166">Activity3 starts only after both Activity1 and Activity2 have completed.</span></span>

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


<span data-ttu-id="10a71-167">Vegyük példaként a következő PowerShell-parancsok, amelyek több fájlok tooa hálózati helyre másolja hello.</span><span class="sxs-lookup"><span data-stu-id="10a71-167">For example, consider hello following PowerShell commands that copy multiple files tooa network destination.</span></span>  <span data-ttu-id="10a71-168">Ezek a parancsok egymás után futnak, úgy, hogy egy fájl Befejezés másolása hello mellett megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="10a71-168">These commands are run sequentially so that one file must finish copying before hello next is started.</span></span>     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

<span data-ttu-id="10a71-169">hello következő munkafolyamat fut, ezek azonos parancsok párhuzamos, hogy az összes másolásának megkezdése: hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="10a71-169">hello following workflow runs these same commands in parallel so that they all start copying at hello same time.</span></span>  <span data-ttu-id="10a71-170">Csak azután minden másolt hello üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="10a71-170">Only after they are all copied is hello completion message displayed.</span></span>

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


<span data-ttu-id="10a71-171">Hello használható **ForEach-Parallel** egy gyűjtemény minden elemén tooprocess parancsok egyidejűleg összeállításához.</span><span class="sxs-lookup"><span data-stu-id="10a71-171">You can use hello **ForEach -Parallel** construct tooprocess commands for each item in a collection concurrently.</span></span> <span data-ttu-id="10a71-172">hello hello gyűjtemény elemeinek feldolgozása párhuzamosan közben hello hello parancsfájlblokk parancsai egymás után futnak.</span><span class="sxs-lookup"><span data-stu-id="10a71-172">hello items in hello collection are processed in parallel while hello commands in hello script block run sequentially.</span></span> <span data-ttu-id="10a71-173">Ez az alább látható szintaxist a következő hello használ.</span><span class="sxs-lookup"><span data-stu-id="10a71-173">This uses hello following syntax shown below.</span></span> <span data-ttu-id="10a71-174">Ebben az esetben hello Activity1 kezdődik azonos időben hello gyűjtemény mindegyik elemére.</span><span class="sxs-lookup"><span data-stu-id="10a71-174">In this case, Activity1 starts at hello same time for all items in hello collection.</span></span> <span data-ttu-id="10a71-175">Minden elemhez a runbook az Activity2 Activity1 befejeződése után elindul.</span><span class="sxs-lookup"><span data-stu-id="10a71-175">For each item, Activity2 starts after Activity1 is complete.</span></span> <span data-ttu-id="10a71-176">Activity3 csak az Activity1 és az Activity2 az összes befejezése után elindul.</span><span class="sxs-lookup"><span data-stu-id="10a71-176">Activity3 starts only after both Activity1 and Activity2 have completed for all items.</span></span>

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

<span data-ttu-id="10a71-177">a következő példa hello hasonló toohello előző példa fájlok másolása a párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="10a71-177">hello following example is similar toohello previous example copying files in parallel.</span></span>  <span data-ttu-id="10a71-178">Ebben az esetben egy üzenet jelenik meg a fájl másolását követően.</span><span class="sxs-lookup"><span data-stu-id="10a71-178">In this case, a message is displayed for each file after it copies.</span></span>  <span data-ttu-id="10a71-179">Csak azután minden másolta, teljesen végső hello üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="10a71-179">Only after they are all completely copied is hello final completion message displayed.</span></span>

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
> <span data-ttu-id="10a71-180">A gyermekrunbookok a párhuzamosan futó, mivel ez toogive megbízhatatlan eredményekhez kimutatták nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="10a71-180">We do not recommend running child runbooks in parallel since this has been shown toogive unreliable results.</span></span>  <span data-ttu-id="10a71-181">hello néha hello gyermekrunbook kimenetét nem jelenik meg, és hatással lehet a beállításokat egy gyermek runbook más párhuzamos gyermek runbookokat hello</span><span class="sxs-lookup"><span data-stu-id="10a71-181">hello output from hello child runbook sometimes does not show up, and settings in one child runbook can affect hello other parallel child runbooks</span></span>
>

## <a name="checkpoints"></a><span data-ttu-id="10a71-182">Az ellenőrzőpontok</span><span class="sxs-lookup"><span data-stu-id="10a71-182">Checkpoints</span></span>
<span data-ttu-id="10a71-183">A *ellenőrzőpont* hello, amely tartalmazza a változók aktuális értékét hello hello-munkafolyamat aktuális állapotáról pillanatképet, és a kimeneti létrehozott toothat pont.</span><span class="sxs-lookup"><span data-stu-id="10a71-183">A *checkpoint* is a snapshot of hello current state of hello workflow that includes hello current value for variables and any output generated toothat point.</span></span> <span data-ttu-id="10a71-184">Ha egy munkafolyamatot fejeződik be a hiba, vagy fel van függesztve, majd hello fut, amikor legközelebb indul hello worfklow hello kezdetét helyett a legutóbbi ellenőrzőponttól.</span><span class="sxs-lookup"><span data-stu-id="10a71-184">If a workflow ends in error or is suspended, then hello next time it is run it will start from its last checkpoint instead of hello start of hello worfklow.</span></span>  <span data-ttu-id="10a71-185">Egy munkafolyamatban hello állíthat be ellenőrzőpontot **Checkpoint-Workflow** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="10a71-185">You can set a checkpoint in a workflow with hello **Checkpoint-Workflow** activity.</span></span>

<span data-ttu-id="10a71-186">A következő példakód hello, a kivételt, amely runbook az Activity2 hello munkafolyamat tooend után következik be.</span><span class="sxs-lookup"><span data-stu-id="10a71-186">In hello following sample code, an exception occurs after Activity2 causing hello workflow tooend.</span></span> <span data-ttu-id="10a71-187">Ha hello munkafolyamat fut újra, runbook az Activity2 futtatásával, mivel az csak az utolsó ellenőrzőpont hello beállítása után kezdődik.</span><span class="sxs-lookup"><span data-stu-id="10a71-187">When hello workflow is run again, it starts by running Activity2 since this was just after hello last checkpoint set.</span></span>

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

<span data-ttu-id="10a71-188">A munkafolyamat célszerű ellenőrzőpontokat beállítani, tevékenységeket, lehet, hogy nagyon eséllyel fordulnak elő tooexception, és nem kell ismételni, ha hello munkafolyamat folytatása után.</span><span class="sxs-lookup"><span data-stu-id="10a71-188">You should set checkpoints in a workflow after activities that may be prone tooexception and should not be repeated if hello workflow is resumed.</span></span> <span data-ttu-id="10a71-189">A munkafolyamat lehet, hogy hozzon létre például egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="10a71-189">For example, your workflow may create a virtual machine.</span></span> <span data-ttu-id="10a71-190">Beállíthat egy ellenőrzőpontot előtti és utáni hello parancsok toocreate hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="10a71-190">You could set a checkpoint both before and after hello commands toocreate hello virtual machine.</span></span> <span data-ttu-id="10a71-191">Ha hello létrehozása sikertelen, majd az hello parancsok volna kell ismételni, ha hello munkafolyamat újra elindult.</span><span class="sxs-lookup"><span data-stu-id="10a71-191">If hello creation fails, then hello commands would be repeated if hello workflow is started again.</span></span> <span data-ttu-id="10a71-192">Ha hello worfklow meghiúsul, miután hello létrehozás sikeresen befejeződik, majd hello virtuális gép nem létrehozza újra hello munkafolyamat folytatásakor.</span><span class="sxs-lookup"><span data-stu-id="10a71-192">If hello worfklow fails after hello creation succeeds, then hello virtual machine will not be created again when hello workflow is resumed.</span></span>

<span data-ttu-id="10a71-193">a következő példa hello több fájlok tooa hálózati helyre másolja, és beállítja az ellenőrzőpont után minden egyes fájl.</span><span class="sxs-lookup"><span data-stu-id="10a71-193">hello following example copies multiple files tooa network location and sets a checkpoint after each file.</span></span>  <span data-ttu-id="10a71-194">Ha hello hálózati hely nem vesztek el, majd hello munkafolyamat fejeződik be a hiba.</span><span class="sxs-lookup"><span data-stu-id="10a71-194">If hello network location is lost, then hello workflow ends in error.</span></span>  <span data-ttu-id="10a71-195">Amikor ismét elindul, folytatódik, ami azt jelenti, hogy csak a már másolt hello fájlok kimarad hello utolsó ellenőrzőpont.</span><span class="sxs-lookup"><span data-stu-id="10a71-195">When it is started again, it will resume at hello last checkpoint meaning that only hello files that have already been copied are skipped.</span></span>

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

<span data-ttu-id="10a71-196">Mivel a username hitelesítő hello hívása után nem maradnak [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) tevékenység vagy után utolsó ellenőrzőpont hello, tooset hello hitelesítő adatok toonull kell, és ezután kérheti le azokat újra hello eszköz áruházból után **A munkafolyamat-felfüggesztési** vagy ellenőrzőpont nevezik.</span><span class="sxs-lookup"><span data-stu-id="10a71-196">Because username credentials are not persisted after you call hello [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) activity or after hello last checkpoint, you need tooset hello credentials toonull and then retrieve them again from hello asset store after **Suspend-Workflow** or checkpoint is called.</span></span>  <span data-ttu-id="10a71-197">Ellenkező esetben jelenhet meg a következő hibaüzenet hello: *hello munkafolyamat-feladatot nem lehet folytatni, vagy mert adatmegőrzési adatok nem teljesen mentve, vagy nem mentett adatok megőrzését sérült. Hello munkafolyamat újra kell indítani.*</span><span class="sxs-lookup"><span data-stu-id="10a71-197">Otherwise, you may receive hello following error message: *hello workflow job cannot be resumed, either because persistence data could not be saved completely, or saved persistence data has been corrupted. You must restart hello workflow.*</span></span>

<span data-ttu-id="10a71-198">a következő ugyanazt a kódot hello bemutatja, hogyan toohandle Ez csak a PowerShell-munkafolyamati forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="10a71-198">hello following same code demonstrates how toohandle this in your PowerShell Workflow runbooks.</span></span>

    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first toocreate hello VM (code not shown)

          # Now add hello VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     }


<span data-ttu-id="10a71-199">Ez pedig nem szükséges vannak hitelesítéséhez a használatával egy egyszerű szolgáltatással konfigurált futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="10a71-199">This is not required if you are authenticating using a Run As account configured with a service principal.</span></span>  

<span data-ttu-id="10a71-200">Az ellenőrzőpontok kapcsolatos további információkért lásd: [ellenőrzőpontok felvétele tooa parancsfájl-munkafolyamathoz](http://technet.microsoft.com/library/jj574114.aspx).</span><span class="sxs-lookup"><span data-stu-id="10a71-200">For more information about checkpoints, see [Adding Checkpoints tooa Script Workflow](http://technet.microsoft.com/library/jj574114.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="10a71-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10a71-201">Next steps</span></span>
* <span data-ttu-id="10a71-202">a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="10a71-202">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
