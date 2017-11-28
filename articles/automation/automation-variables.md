---
title: "Az Azure Automationben változó eszközök |} Microsoft Docs"
description: "Változó eszközök értékeket összes forgatókönyve és az Azure Automation DSC-konfiguráció számára elérhető.  Ez a cikk ismerteti a változók és a szöveges és a grafikus szerzői őket munkavégzés részleteit."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: b880c15f-46f5-4881-8e98-e034cc5a66ec
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/09/2017
ms.author: magoedte;bwren
ms.openlocfilehash: dc00e1e5fa8df5cb55e7e2672137d1df44133773
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="variable-assets-in-azure-automation"></a><span data-ttu-id="42c69-104">Az Azure Automationben változó eszközök</span><span class="sxs-lookup"><span data-stu-id="42c69-104">Variable assets in Azure Automation</span></span>

<span data-ttu-id="42c69-105">Változó eszközök értékeket elérhető összes forgatókönyve és DSC-konfigurációk az automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="42c69-105">Variable assets are values that are available to all runbooks and DSC configurations in your automation account.</span></span> <span data-ttu-id="42c69-106">Azok létrehozott, módosított, és lekérése az Azure-portálon, a Windows PowerShell és a runbookot vagy a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="42c69-106">They can be created, modified, and retrieved from the Azure portal, Windows PowerShell, and from within a runbook or DSC configuration.</span></span> <span data-ttu-id="42c69-107">Automatizálási változók az alábbiak a következő esetekben lehet hasznos:</span><span class="sxs-lookup"><span data-stu-id="42c69-107">Automation variables are useful for the following scenarios:</span></span>

- <span data-ttu-id="42c69-108">Ossza meg több runbookok vagy a DSC-konfigurációk közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="42c69-108">Share a value between multiple runbooks or DSC configurations.</span></span>

- <span data-ttu-id="42c69-109">Ossza meg a DSC-konfiguráció vagy ugyanaz a runbook több feladat közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="42c69-109">Share a value between multiple jobs from the same runbook or DSC configuration.</span></span>

- <span data-ttu-id="42c69-110">A portálon vagy a Windows PowerShell parancssorban forgatókönyvek vagy a DSC-konfigurációk, például a virtuális gép nevét, egy adott erőforráscsoportot, egy AD-tartomány nevét, stb például adott listáját általános konfigurációs elemek készlete által használt érték kezelése.</span><span class="sxs-lookup"><span data-stu-id="42c69-110">Manage a value from the portal or from the Windows PowerShell command line that is used by runbooks or DSC configurations, such as a set of common configuration items like specific list of VM names, a specific resource group,  an AD domain name, etc.</span></span>  

<span data-ttu-id="42c69-111">Automatizálási változók megmaradnak, így azok továbbra is elérhetők, még akkor is, ha a runbookot vagy a DSC-konfiguráció nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="42c69-111">Automation variables are persisted so that they continue to be available even if the runbook or DSC configuration fails.</span></span>  <span data-ttu-id="42c69-112">Ez egy runbookot, amely ezután történik egy másik, vagy használja a ugyanaz a runbook vagy a DSC-konfiguráció fut, amikor legközelebb által értéket is lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="42c69-112">This also allows a value to be set by one runbook that is then used by another, or is used by the same runbook or DSC configuration the next time that it is run.</span></span>

<span data-ttu-id="42c69-113">Egy változó létrehozásakor megadhatja, hogy legyen tárolva titkosított.</span><span class="sxs-lookup"><span data-stu-id="42c69-113">When a variable is created, you can specify that it be stored encrypted.</span></span>  <span data-ttu-id="42c69-114">Ha titkosított változó, az Azure Automation tárolja biztonságos helyen és az értéke nem lehet beolvasni a [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) parancsmagot tartalmaz, az Azure PowerShell modul részét képezi.</span><span class="sxs-lookup"><span data-stu-id="42c69-114">When a variable is encrypted, it is stored securely in Azure Automation, and its value cannot be retrieved from the [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet that ships as part of the Azure PowerShell module.</span></span>  <span data-ttu-id="42c69-115">Származik, csak úgy lehetséges, hogy egy titkosított értéke lehet beolvasni a **Get-AutomationVariable** tevékenység egy runbookot vagy a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="42c69-115">The only way that an encrypted value can be retrieved is from the **Get-AutomationVariable** activity in a runbook or DSC configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="42c69-116">Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók.</span><span class="sxs-lookup"><span data-stu-id="42c69-116">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="42c69-117">Ezek az eszközök titkosítva, és tárolja az Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="42c69-117">These assets are encrypted and stored in the Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="42c69-118">Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja.</span><span class="sxs-lookup"><span data-stu-id="42c69-118">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="42c69-119">Előtt tárolása biztonságos eszköz, az automatizálási fiók kulcs visszafejtése a mestertanúsítvány, és majd az eszköz titkosításához használt.</span><span class="sxs-lookup"><span data-stu-id="42c69-119">Before storing a secure asset, the key for the automation account is decrypted using the master certificate and then used to encrypt the asset.</span></span>

## <a name="variable-types"></a><span data-ttu-id="42c69-120">Változó típusa</span><span class="sxs-lookup"><span data-stu-id="42c69-120">Variable types</span></span>

<span data-ttu-id="42c69-121">Az Azure portállal egy változó létrehozásakor meg kell adni egy adatok a legördülő listából, a portál írja be a változó értékének megfelelő vezérlőket tudja megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="42c69-121">When you create a variable with the Azure portal, you must specify a data type from the drop-down list so the portal can display the appropriate control for entering the variable value.</span></span> <span data-ttu-id="42c69-122">A változó nem korlátozódik ezt az adattípust, de meg kell adni a változót a Windows PowerShell használatával, ha azt szeretné, hogy más típusú értéket.</span><span class="sxs-lookup"><span data-stu-id="42c69-122">The variable is not restricted to this data type, but you must set the variable using Windows PowerShell if you want to specify a value of a different type.</span></span> <span data-ttu-id="42c69-123">Ha megad **nincs definiálva**, akkor állítja be a változó értékének **$null**, és az értéket meg kell adni a [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) parancsmag vagy **Set-AutomationVariable** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="42c69-123">If you specify **Not defined**, then the value of the variable will be set to **$null**, and you must set the value with the [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet or **Set-AutomationVariable** activity.</span></span>  <span data-ttu-id="42c69-124">Nem hozható létre, vagy módosítsa az értéket egy változó komplex típus a portálon, de megadhatja a Windows PowerShell használatával bármilyen típusú érték.</span><span class="sxs-lookup"><span data-stu-id="42c69-124">You cannot create or change the value for a complex variable type in the portal, but you can provide a value of any type using Windows PowerShell.</span></span> <span data-ttu-id="42c69-125">Összetett típusok változatlanul adódik vissza egy [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="42c69-125">Complex types will be returned as a [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span></span>

<span data-ttu-id="42c69-126">Több érték változóhoz tömb vagy hashtable létrehozásával, és mentse a változó tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="42c69-126">You can store multiple values to a single variable by creating an array or hashtable and saving it to the variable.</span></span>

<span data-ttu-id="42c69-127">A rendelkezésre álló Automation változó típusainak listáját a következők:</span><span class="sxs-lookup"><span data-stu-id="42c69-127">The following are a list of variable types available in Automation:</span></span>

* <span data-ttu-id="42c69-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="42c69-128">String</span></span>
* <span data-ttu-id="42c69-129">Egész szám</span><span class="sxs-lookup"><span data-stu-id="42c69-129">Integer</span></span>
* <span data-ttu-id="42c69-130">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="42c69-130">DateTime</span></span>
* <span data-ttu-id="42c69-131">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="42c69-131">Boolean</span></span>
* <span data-ttu-id="42c69-132">NULL értékű</span><span class="sxs-lookup"><span data-stu-id="42c69-132">Null</span></span>

## <a name="cmdlets-and-workflow-activities"></a><span data-ttu-id="42c69-133">Parancsmagok és a munkafolyamat-tevékenységek</span><span class="sxs-lookup"><span data-stu-id="42c69-133">Cmdlets and workflow activities</span></span>

<span data-ttu-id="42c69-134">A következő táblázat parancsmagjai a Windows PowerShell használatával automatizálási változók létrehozására és kezelésére szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="42c69-134">The cmdlets in the following table are used to create and manage Automation variables with Windows PowerShell.</span></span> <span data-ttu-id="42c69-135">Részét képezi a [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="42c69-135">They ship as part of the [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configuration.</span></span>

|<span data-ttu-id="42c69-136">Parancsmagok</span><span class="sxs-lookup"><span data-stu-id="42c69-136">Cmdlets</span></span>|<span data-ttu-id="42c69-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="42c69-137">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="42c69-138">Get-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="42c69-138">Get-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603849.aspx)|<span data-ttu-id="42c69-139">Egy létező változó értékét kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="42c69-139">Retrieves the value of an existing variable.</span></span>|
|[<span data-ttu-id="42c69-140">Új AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="42c69-140">New-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603613.aspx)|<span data-ttu-id="42c69-141">Új változót hoz létre, és beállítja az értékét.</span><span class="sxs-lookup"><span data-stu-id="42c69-141">Creates a new variable and sets its value.</span></span>|
|[<span data-ttu-id="42c69-142">Remove-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="42c69-142">Remove-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt619354.aspx)|<span data-ttu-id="42c69-143">Eltávolít egy létező változó.</span><span class="sxs-lookup"><span data-stu-id="42c69-143">Removes an existing variable.</span></span>|
|[<span data-ttu-id="42c69-144">Set-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="42c69-144">Set-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603601.aspx)|<span data-ttu-id="42c69-145">Beállítja egy létező változó értékét.</span><span class="sxs-lookup"><span data-stu-id="42c69-145">Sets the value for an existing variable.</span></span>|

<span data-ttu-id="42c69-146">A munkafolyamat-tevékenységek az alábbi táblázat automatizálási a runbookban található változók elérésére használhatók.</span><span class="sxs-lookup"><span data-stu-id="42c69-146">The workflow activities in the following table are used to access Automation variables in a runbook.</span></span> <span data-ttu-id="42c69-147">Ezek csak akkor használ, a runbookot vagy a DSC-konfiguráció, és nem az Azure PowerShell modul részét képezi.</span><span class="sxs-lookup"><span data-stu-id="42c69-147">They are only available for use in a runbook or DSC configuration, and do not ship as part of the Azure PowerShell module.</span></span>

|<span data-ttu-id="42c69-148">Munkafolyamat-tevékenységek</span><span class="sxs-lookup"><span data-stu-id="42c69-148">Workflow Activities</span></span>|<span data-ttu-id="42c69-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="42c69-149">Description</span></span>|
|:---|:---|
|<span data-ttu-id="42c69-150">Get-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="42c69-150">Get-AutomationVariable</span></span>|<span data-ttu-id="42c69-151">Egy létező változó értékét kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="42c69-151">Retrieves the value of an existing variable.</span></span>|
|<span data-ttu-id="42c69-152">Set-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="42c69-152">Set-AutomationVariable</span></span>|<span data-ttu-id="42c69-153">Beállítja egy létező változó értékét.</span><span class="sxs-lookup"><span data-stu-id="42c69-153">Sets the value for an existing variable.</span></span>|

> [!NOTE] 
> <span data-ttu-id="42c69-154">Kerülendő a változók használata a – Name paraméterében **Get-AutomationVariable** a runbookot vagy a DSC-konfiguráció számára, mivel ez megnehezítheti a runbookok vagy a DSC-konfiguráció és az Automation-változók közti függőségek tervezési időben.</span><span class="sxs-lookup"><span data-stu-id="42c69-154">You should avoid using variables in the –Name parameter of **Get-AutomationVariable**  in a runbook or DSC configuration since this can complicate discovering dependencies between runbooks or DSC configuration, and Automation variables at design time.</span></span>

## <a name="creating-a-new-automation-variable"></a><span data-ttu-id="42c69-155">Új automatizálási változó létrehozása</span><span class="sxs-lookup"><span data-stu-id="42c69-155">Creating a new Automation variable</span></span>

### <a name="to-create-a-new-variable-with-the-azure-portal"></a><span data-ttu-id="42c69-156">Új változó létrehozása az Azure portállal</span><span class="sxs-lookup"><span data-stu-id="42c69-156">To create a new variable with the Azure portal</span></span>

1. <span data-ttu-id="42c69-157">Az Automation-fiók, kattintson a **eszközök** csempe majd a a **eszközök** panelen válassza **változók**.</span><span class="sxs-lookup"><span data-stu-id="42c69-157">From your Automation account, click the **Assets** tile and then on the **Assets** blade, select **Variables**.</span></span>
2. <span data-ttu-id="42c69-158">Az a **változók** csempe, jelölje be **változó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="42c69-158">On the **Variables** tile, select **Add a variable**.</span></span>
3. <span data-ttu-id="42c69-159">Végezze el a beállításokat a a **új változó** panel megnyitásához, és kattintson **létrehozása** az új változó mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="42c69-159">Complete the options on the **New Variable** blade and click **Create** save the new variable.</span></span>

### <a name="to-create-a-new-variable-with-windows-powershell"></a><span data-ttu-id="42c69-160">Új változó létrehozása a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="42c69-160">To create a new variable with Windows PowerShell</span></span>

<span data-ttu-id="42c69-161">A [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) parancsmag új változót hoz létre, és beállítja a kezdeti értékhez.</span><span class="sxs-lookup"><span data-stu-id="42c69-161">The [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet creates a new variable and sets its initial value.</span></span> <span data-ttu-id="42c69-162">Kérheti le a érték [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span><span class="sxs-lookup"><span data-stu-id="42c69-162">You can retrieve the value using [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span></span> <span data-ttu-id="42c69-163">Ha az érték egy egyszerű típusú, majd, hogy ugyanolyan típusú adja vissza.</span><span class="sxs-lookup"><span data-stu-id="42c69-163">If the value is a simple type, then that same type is returned.</span></span> <span data-ttu-id="42c69-164">Ha egy összetett típus, akkor egy **PSCustomObject** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="42c69-164">If it’s a complex type, then a **PSCustomObject** is returned.</span></span>

<span data-ttu-id="42c69-165">Az alábbi Példaparancsok szemléltetik egy karakterlánc típusú változó létrehozása, és térjen vissza az értékét.</span><span class="sxs-lookup"><span data-stu-id="42c69-165">The following sample commands show how to create a variable of type string and then return its value.</span></span>

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

<span data-ttu-id="42c69-166">Az alábbi Példaparancsok szemléltetik egy összetett típus hozzon létre egy változót, és térjen vissza a tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="42c69-166">The following sample commands show how to create a variable with a complex type and then return its properties.</span></span> <span data-ttu-id="42c69-167">Ebben az esetben a virtuális gépek objektum **Get-AzureRmVm** szolgál.</span><span class="sxs-lookup"><span data-stu-id="42c69-167">In this case, a virtual machine object from **Get-AzureRmVm** is used.</span></span>

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a><span data-ttu-id="42c69-168">Egy változó a runbookot vagy a DSC-konfiguráció használata</span><span class="sxs-lookup"><span data-stu-id="42c69-168">Using a variable in a runbook or DSC configuration</span></span>

<span data-ttu-id="42c69-169">Használja a **Set-AutomationVariable** tevékenység beállítani egy automatizálási változó értékét a runbookot vagy a DSC-konfiguráció, és a **Get-AutomationVariable** ennek lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="42c69-169">Use the **Set-AutomationVariable** activity to set the value of an Automation variable in a runbook or DSC configuration, and the **Get-AutomationVariable** to retrieve it.</span></span>  <span data-ttu-id="42c69-170">Ne használja a **Set-AzureAutomationVariable** vagy **Get-AzureAutomationVariable** parancsmagok a runbookot vagy a DSC-konfiguráció számára, mert azok a munkafolyamat-tevékenységek-nél kevésbé hatékonyak.</span><span class="sxs-lookup"><span data-stu-id="42c69-170">You shouldn't use the **Set-AzureAutomationVariable** or  **Get-AzureAutomationVariable** cmdlets in a runbook or DSC configuration since they are less efficient than the workflow activities.</span></span>  <span data-ttu-id="42c69-171">Akkor is nem lehet lekérdezni a biztonságos változókról, **Get-AzureAutomationVariable**.</span><span class="sxs-lookup"><span data-stu-id="42c69-171">You also cannot retrieve the value of secure variables with **Get-AzureAutomationVariable**.</span></span>  <span data-ttu-id="42c69-172">Hozzon létre egy új változót a runbookot vagy a DSC-konfiguráció csak úgy, hogy használja a [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="42c69-172">The only way to create a new variable from within a runbook or DSC configuration is to use the [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)  cmdlet.</span></span>


### <a name="textual-runbook-samples"></a><span data-ttu-id="42c69-173">Szöveges forgatókönyvként minták</span><span class="sxs-lookup"><span data-stu-id="42c69-173">Textual runbook samples</span></span>

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a><span data-ttu-id="42c69-174">És egy egyszerű érték a változóból beolvasása</span><span class="sxs-lookup"><span data-stu-id="42c69-174">Setting and retrieving a simple value from a variable</span></span>

<span data-ttu-id="42c69-175">Az alábbi Példaparancsok szemléltetik egy szöveges forgatókönyvként változó beolvasása és beállítása.</span><span class="sxs-lookup"><span data-stu-id="42c69-175">The following sample commands show how to set and retrieve a variable in a textual runbook.</span></span> <span data-ttu-id="42c69-176">A példában feltételezzük, hogy az egész szám típusú nevű *NumberOfIterations* és *NumberOfRunnings* és nevű, karakterlánc típusú változó *példában* már létre van hozva.</span><span class="sxs-lookup"><span data-stu-id="42c69-176">In this sample, it is assumed that variables of type integer named *NumberOfIterations* and *NumberOfRunnings* and a variable of type string named *SampleMessage* have already been created.</span></span>

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a><span data-ttu-id="42c69-177">És egy összetett objektumot egy változóban beolvasása</span><span class="sxs-lookup"><span data-stu-id="42c69-177">Setting and retrieving a complex object in a variable</span></span>

<span data-ttu-id="42c69-178">Az alábbi mintakód bemutatja, hogyan szöveges forgatókönyvként összetett érték egy változó frissítése.</span><span class="sxs-lookup"><span data-stu-id="42c69-178">The following sample code shows how to update a variable with a complex value in a textual runbook.</span></span> <span data-ttu-id="42c69-179">Ez a példa egy Azure virtuális gépen a beolvasott **Get-AzureVM** és egy meglévő automatizálási változó mentve.</span><span class="sxs-lookup"><span data-stu-id="42c69-179">In this sample, an Azure virtual machine is retrieved with **Get-AzureVM** and saved to an existing Automation variable.</span></span>  <span data-ttu-id="42c69-180">A [változó típusok](#variable-types), ez egy PSCustomObject tárolja.</span><span class="sxs-lookup"><span data-stu-id="42c69-180">As explained in [Variable types](#variable-types), this is stored as a PSCustomObject.</span></span>

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


<span data-ttu-id="42c69-181">Az alábbi kódban értéke olvassa be a változót, és a virtuális gép indításához használt.</span><span class="sxs-lookup"><span data-stu-id="42c69-181">In the following code, the value is retrieved from the variable and used to start the virtual machine.</span></span>

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a><span data-ttu-id="42c69-182">És egy gyűjtemény változóként beolvasása</span><span class="sxs-lookup"><span data-stu-id="42c69-182">Setting and retrieving a collection in a variable</span></span>

<span data-ttu-id="42c69-183">Az alábbi mintakód bemutatja, hogyan használhat egy változót a szöveges forgatókönyvként összetett értékek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="42c69-183">The following sample code shows how to use a variable with a collection of complex values in a textual runbook.</span></span> <span data-ttu-id="42c69-184">Ez a példa több Azure virtuális gépeken a rendszer beolvassa **Get-AzureVM** és egy meglévő automatizálási változó mentve.</span><span class="sxs-lookup"><span data-stu-id="42c69-184">In this sample, multiple Azure virtual machines are retrieved with **Get-AzureVM** and saved to an existing Automation variable.</span></span>  <span data-ttu-id="42c69-185">A [változó típusok](#variable-types), ez PSCustomObjects gyűjteménye tárolja.</span><span class="sxs-lookup"><span data-stu-id="42c69-185">As explained in [Variable types](#variable-types), this is stored as a collection of PSCustomObjects.</span></span>

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

<span data-ttu-id="42c69-186">A következő kódrészlet a gyűjtemény olvassa be a változót, és minden virtuális gép indításához használt.</span><span class="sxs-lookup"><span data-stu-id="42c69-186">In the following code, the collection is retrieved from the variable and used to start each virtual machine.</span></span>

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a><span data-ttu-id="42c69-187">Grafikus forgatókönyv minták</span><span class="sxs-lookup"><span data-stu-id="42c69-187">Graphical runbook samples</span></span>

<span data-ttu-id="42c69-188">A grafikus runbookokban, vegye fel a **Get-AutomationVariable** vagy **Set-AutomationVariable** a változó a könyvtár ablaktáblán grafikus szerkesztő csomagot jobb gombbal, majd válassza a kívánt tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="42c69-188">In a graphical runbook, you add the **Get-AutomationVariable** or **Set-AutomationVariable** by right-clicking on the variable in the Library pane of the graphical editor and selecting the activity you want.</span></span>

![Vászonra változó hozzáadása](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a><span data-ttu-id="42c69-190">A beállítás értéke egy változó</span><span class="sxs-lookup"><span data-stu-id="42c69-190">Setting values in a variable</span></span>
<span data-ttu-id="42c69-191">Az alábbi ábrán egy változó frissíteni egy grafikus forgatókönyv egyszerű érték minta tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="42c69-191">The following image shows sample activities to update a variable with a simple value in a graphical runbook.</span></span> <span data-ttu-id="42c69-192">Ez a példa egy Azure virtuális gép a beolvasott **Get-AzureRmVM** és a számítógép nevét egy létező automatizálási változó karakterlánc típusú vannak mentve.</span><span class="sxs-lookup"><span data-stu-id="42c69-192">In this sample, a single Azure virtual machine is retrieved with **Get-AzureRmVM** and the computer name is saved to an existing Automation variable with a type of String.</span></span>  <span data-ttu-id="42c69-193">Nem számít, hogy a [egy folyamat vagy feladatütemezési](automation-graphical-authoring-intro.md#links-and-workflow) óta kimenet csak várhatóan egy adott objektum.</span><span class="sxs-lookup"><span data-stu-id="42c69-193">It doesn't matter whether the [link is a pipeline or sequence](automation-graphical-authoring-intro.md#links-and-workflow) since we only expect a single object in the output.</span></span>

![Egyszerű változó beállítása](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a><span data-ttu-id="42c69-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="42c69-195">Next Steps</span></span>

* <span data-ttu-id="42c69-196">Tevékenységek összekapcsolása a grafikus szerzői kapcsolatos további információkért lásd: [grafikus szerzői hivatkozások](automation-graphical-authoring-intro.md#links-and-workflow)</span><span class="sxs-lookup"><span data-stu-id="42c69-196">To learn more about connecting activities together in graphical authoring, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow)</span></span>
* <span data-ttu-id="42c69-197">A grafikus forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első grafikus forgatókönyvem](automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="42c69-197">To get started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span> 

