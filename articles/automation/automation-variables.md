---
title: "az Azure Automationben aaaVariable eszközök |} Microsoft Docs"
description: "Változó eszközök értékeket elérhető tooall runbookokat és az Azure Automation DSC-konfigurációk.  Ez a cikk ismerteti a változók hello részleteit, és hogyan toowork velük a szöveges és a grafikus szerzői."
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
ms.openlocfilehash: f9daa49fc1dc883ffb218a9adf26e36df1d6bb27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="variable-assets-in-azure-automation"></a><span data-ttu-id="7ca6a-104">Az Azure Automationben változó eszközök</span><span class="sxs-lookup"><span data-stu-id="7ca6a-104">Variable assets in Azure Automation</span></span>

<span data-ttu-id="7ca6a-105">Változó eszközök értékeket elérhető tooall runbookok és a DSC-konfigurációk az automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-105">Variable assets are values that are available tooall runbooks and DSC configurations in your automation account.</span></span> <span data-ttu-id="7ca6a-106">Azok létrehozott, módosított, és lekérése hello Azure-portálon, a Windows PowerShell és a runbookot vagy a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-106">They can be created, modified, and retrieved from hello Azure portal, Windows PowerShell, and from within a runbook or DSC configuration.</span></span> <span data-ttu-id="7ca6a-107">A következő forgatókönyvek hello automatizálási változók hasznosak:</span><span class="sxs-lookup"><span data-stu-id="7ca6a-107">Automation variables are useful for hello following scenarios:</span></span>

- <span data-ttu-id="7ca6a-108">Ossza meg több runbookok vagy a DSC-konfigurációk közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-108">Share a value between multiple runbooks or DSC configurations.</span></span>

- <span data-ttu-id="7ca6a-109">Egy érték elérhetővé tétele több feladat hello ugyanaz a runbook vagy DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-109">Share a value between multiple jobs from hello same runbook or DSC configuration.</span></span>

- <span data-ttu-id="7ca6a-110">Hello portálon vagy a hello Windows PowerShell parancssorból forgatókönyvek vagy a DSC-konfigurációk, például a virtuális gép nevét, egy adott erőforráscsoportot, egy AD-tartomány nevét, stb például adott listáját általános konfigurációs elemek készlete által használt érték kezelése.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-110">Manage a value from hello portal or from hello Windows PowerShell command line that is used by runbooks or DSC configurations, such as a set of common configuration items like specific list of VM names, a specific resource group,  an AD domain name, etc.</span></span>  

<span data-ttu-id="7ca6a-111">Automatizálási változók megmaradnak, így akkor is elérhető toobe, még akkor is, ha hello runbookot vagy a DSC-konfiguráció nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-111">Automation variables are persisted so that they continue toobe available even if hello runbook or DSC configuration fails.</span></span>  <span data-ttu-id="7ca6a-112">Ezenkívül lehetővé teszi egy érték toobe állítja be, amely ezután történik egy másik, vagy hello használja egy runbook ugyanaz a runbook vagy DSC konfigurációs hello, amikor legközelebb futtatja azt.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-112">This also allows a value toobe set by one runbook that is then used by another, or is used by hello same runbook or DSC configuration hello next time that it is run.</span></span>

<span data-ttu-id="7ca6a-113">Egy változó létrehozásakor megadhatja, hogy legyen tárolva titkosított.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-113">When a variable is created, you can specify that it be stored encrypted.</span></span>  <span data-ttu-id="7ca6a-114">Ha titkosított változó, az Azure Automation tárolja biztonságos helyen és az értéke nem lehet beolvasni hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) parancsmagot tartalmaz, hello Azure PowerShell modul részét képezi.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-114">When a variable is encrypted, it is stored securely in Azure Automation, and its value cannot be retrieved from hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) cmdlet that ships as part of hello Azure PowerShell module.</span></span>  <span data-ttu-id="7ca6a-115">hello csak titkosított érték lekérhető, a hello **Get-AutomationVariable** tevékenység egy runbookot vagy a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-115">hello only way that an encrypted value can be retrieved is from hello **Get-AutomationVariable** activity in a runbook or DSC configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="7ca6a-116">Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-116">Secure assets in Azure Automation include credentials, certificates, connections, and encrypted variables.</span></span> <span data-ttu-id="7ca6a-117">Ezek az eszközök titkosítottak és a tárolt hello Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-117">These assets are encrypted and stored in hello Azure Automation using a unique key that is generated for each automation account.</span></span> <span data-ttu-id="7ca6a-118">Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-118">This key is encrypted by a master certificate and stored in Azure Automation.</span></span> <span data-ttu-id="7ca6a-119">Tárolása biztonságos eszköz, mielőtt hello kulcs hello automation-fiók visszafejtése hello fő tanúsítványt használ, és akkor használja a tooencrypt hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-119">Before storing a secure asset, hello key for hello automation account is decrypted using hello master certificate and then used tooencrypt hello asset.</span></span>

## <a name="variable-types"></a><span data-ttu-id="7ca6a-120">Változó típusa</span><span class="sxs-lookup"><span data-stu-id="7ca6a-120">Variable types</span></span>

<span data-ttu-id="7ca6a-121">Hello Azure-portál a változó létrehozásakor meg kell adni egy adatok hello legördülő listából, hello portal megjelenítheti hello megfelelő vezérlő beírásához hello változó értékét.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-121">When you create a variable with hello Azure portal, you must specify a data type from hello drop-down list so hello portal can display hello appropriate control for entering hello variable value.</span></span> <span data-ttu-id="7ca6a-122">hello változó nem korlátozott toothis adatok típusa, de értékre kell állítani a Windows PowerShell használatával, ha azt szeretné, hogy toospecify hello változó egy eltérő típusú.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-122">hello variable is not restricted toothis data type, but you must set hello variable using Windows PowerShell if you want toospecify a value of a different type.</span></span> <span data-ttu-id="7ca6a-123">Ha megad **nincs definiálva**, majd hello hello változó értéke lesz túl**$null**, és meg kell adni a hello hello értékének [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) parancsmag vagy **Set-AutomationVariable** tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-123">If you specify **Not defined**, then hello value of hello variable will be set too**$null**, and you must set hello value with hello [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) cmdlet or **Set-AutomationVariable** activity.</span></span>  <span data-ttu-id="7ca6a-124">Nem hozható létre, vagy módosítsa hello hello portálon változó komplex típusok esetében, de megadhatja a Windows PowerShell használatával bármilyen típusú érték.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-124">You cannot create or change hello value for a complex variable type in hello portal, but you can provide a value of any type using Windows PowerShell.</span></span> <span data-ttu-id="7ca6a-125">Összetett típusok változatlanul adódik vissza egy [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ca6a-125">Complex types will be returned as a [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).</span></span>

<span data-ttu-id="7ca6a-126">Több értékek tooa egy változó létrehozása a tömb vagy hibás, és mentse toohello változó tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-126">You can store multiple values tooa single variable by creating an array or hashtable and saving it toohello variable.</span></span>

<span data-ttu-id="7ca6a-127">Az alábbiakban hello változó Típuslista Automation érhető el:</span><span class="sxs-lookup"><span data-stu-id="7ca6a-127">hello following are a list of variable types available in Automation:</span></span>

* <span data-ttu-id="7ca6a-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="7ca6a-128">String</span></span>
* <span data-ttu-id="7ca6a-129">Egész szám</span><span class="sxs-lookup"><span data-stu-id="7ca6a-129">Integer</span></span>
* <span data-ttu-id="7ca6a-130">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="7ca6a-130">DateTime</span></span>
* <span data-ttu-id="7ca6a-131">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="7ca6a-131">Boolean</span></span>
* <span data-ttu-id="7ca6a-132">NULL értékű</span><span class="sxs-lookup"><span data-stu-id="7ca6a-132">Null</span></span>

## <a name="cmdlets-and-workflow-activities"></a><span data-ttu-id="7ca6a-133">Parancsmagok és a munkafolyamat-tevékenységek</span><span class="sxs-lookup"><span data-stu-id="7ca6a-133">Cmdlets and workflow activities</span></span>

<span data-ttu-id="7ca6a-134">hello parancsmagok a következő táblázat hello használt toocreate és kezelése Windows PowerShell-automatizálási változók.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-134">hello cmdlets in hello following table are used toocreate and manage Automation variables with Windows PowerShell.</span></span> <span data-ttu-id="7ca6a-135">Ezek hello részét képezi [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-135">They ship as part of hello [Azure PowerShell module](../powershell-install-configure.md) which is available for use in Automation runbooks and DSC configuration.</span></span>

|<span data-ttu-id="7ca6a-136">Parancsmagok</span><span class="sxs-lookup"><span data-stu-id="7ca6a-136">Cmdlets</span></span>|<span data-ttu-id="7ca6a-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="7ca6a-137">Description</span></span>|
|:---|:---|
|[<span data-ttu-id="7ca6a-138">Get-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="7ca6a-138">Get-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603849.aspx)|<span data-ttu-id="7ca6a-139">Lekéri a hello egy létező változó értékét.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-139">Retrieves hello value of an existing variable.</span></span>|
|[<span data-ttu-id="7ca6a-140">Új AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="7ca6a-140">New-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603613.aspx)|<span data-ttu-id="7ca6a-141">Új változót hoz létre, és beállítja az értékét.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-141">Creates a new variable and sets its value.</span></span>|
|[<span data-ttu-id="7ca6a-142">Remove-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="7ca6a-142">Remove-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt619354.aspx)|<span data-ttu-id="7ca6a-143">Eltávolít egy létező változó.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-143">Removes an existing variable.</span></span>|
|[<span data-ttu-id="7ca6a-144">Set-AzureRmAutomationVariable</span><span class="sxs-lookup"><span data-stu-id="7ca6a-144">Set-AzureRmAutomationVariable</span></span>](https://msdn.microsoft.com/library/mt603601.aspx)|<span data-ttu-id="7ca6a-145">Beállítja egy létező változó értékét hello.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-145">Sets hello value for an existing variable.</span></span>|

<span data-ttu-id="7ca6a-146">a következő táblázat hello hello munkafolyamat tevékenységei használt tooaccess egy runbook automatizálási változók.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-146">hello workflow activities in hello following table are used tooaccess Automation variables in a runbook.</span></span> <span data-ttu-id="7ca6a-147">Ezek csak akkor használ, a runbookot vagy a DSC-konfiguráció, és nem hello Azure PowerShell-modulja részét képezi.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-147">They are only available for use in a runbook or DSC configuration, and do not ship as part of hello Azure PowerShell module.</span></span>

|<span data-ttu-id="7ca6a-148">Munkafolyamat-tevékenységek</span><span class="sxs-lookup"><span data-stu-id="7ca6a-148">Workflow Activities</span></span>|<span data-ttu-id="7ca6a-149">Leírás</span><span class="sxs-lookup"><span data-stu-id="7ca6a-149">Description</span></span>|
|:---|:---|
|<span data-ttu-id="7ca6a-150">Get-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="7ca6a-150">Get-AutomationVariable</span></span>|<span data-ttu-id="7ca6a-151">Lekéri a hello egy létező változó értékét.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-151">Retrieves hello value of an existing variable.</span></span>|
|<span data-ttu-id="7ca6a-152">Set-AutomationVariable</span><span class="sxs-lookup"><span data-stu-id="7ca6a-152">Set-AutomationVariable</span></span>|<span data-ttu-id="7ca6a-153">Beállítja egy létező változó értékét hello.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-153">Sets hello value for an existing variable.</span></span>|

> [!NOTE] 
> <span data-ttu-id="7ca6a-154">Kerülendő a változók használata hello – Name paraméterében **Get-AutomationVariable** a runbookot vagy a DSC-konfiguráció számára, mivel ez megnehezítheti a runbookok vagy DSC-konfiguráció és automatizálás közti függőségek változók tervezési időben.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-154">You should avoid using variables in hello –Name parameter of **Get-AutomationVariable**  in a runbook or DSC configuration since this can complicate discovering dependencies between runbooks or DSC configuration, and Automation variables at design time.</span></span>

## <a name="creating-a-new-automation-variable"></a><span data-ttu-id="7ca6a-155">Új automatizálási változó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7ca6a-155">Creating a new Automation variable</span></span>

### <a name="toocreate-a-new-variable-with-hello-azure-portal"></a><span data-ttu-id="7ca6a-156">toocreate egy új változót a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7ca6a-156">toocreate a new variable with hello Azure portal</span></span>

1. <span data-ttu-id="7ca6a-157">Kattintson az Automation-fiók hello **eszközök** csempe majd a hello **eszközök** panelen válassza **változók**.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-157">From your Automation account, click hello **Assets** tile and then on hello **Assets** blade, select **Variables**.</span></span>
2. <span data-ttu-id="7ca6a-158">A hello **változók** csempe, jelölje be **változó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-158">On hello **Variables** tile, select **Add a variable**.</span></span>
3. <span data-ttu-id="7ca6a-159">Fejezze be a hello hello-beállítások **új változó** panel megnyitásához, és kattintson **létrehozása** hello új változó mentse.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-159">Complete hello options on hello **New Variable** blade and click **Create** save hello new variable.</span></span>

### <a name="toocreate-a-new-variable-with-windows-powershell"></a><span data-ttu-id="7ca6a-160">toocreate egy új változót a Windows PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7ca6a-160">toocreate a new variable with Windows PowerShell</span></span>

<span data-ttu-id="7ca6a-161">Hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) parancsmag új változót hoz létre, és beállítja a kezdeti értékhez.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-161">hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) cmdlet creates a new variable and sets its initial value.</span></span> <span data-ttu-id="7ca6a-162">Kérheti le hello érték [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span><span class="sxs-lookup"><span data-stu-id="7ca6a-162">You can retrieve hello value using [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx).</span></span> <span data-ttu-id="7ca6a-163">Ha hello érték egyszerű típus, majd, hogy ugyanolyan típusú adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-163">If hello value is a simple type, then that same type is returned.</span></span> <span data-ttu-id="7ca6a-164">Ha egy összetett típus, akkor egy **PSCustomObject** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-164">If it’s a complex type, then a **PSCustomObject** is returned.</span></span>

<span data-ttu-id="7ca6a-165">hello következő minta parancsok megjelenítése hogyan toocreate típusú változó karakterláncot, és térjen vissza az értékét.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-165">hello following sample commands show how toocreate a variable of type string and then return its value.</span></span>

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

<span data-ttu-id="7ca6a-166">hello következő minta parancsok megjelenítése hogyan toocreate egy összetett változó, írja be, és térjen vissza a tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-166">hello following sample commands show how toocreate a variable with a complex type and then return its properties.</span></span> <span data-ttu-id="7ca6a-167">Ebben az esetben a virtuális gépek objektum **Get-AzureRmVm** szolgál.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-167">In this case, a virtual machine object from **Get-AzureRmVm** is used.</span></span>

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a><span data-ttu-id="7ca6a-168">Egy változó a runbookot vagy a DSC-konfiguráció használata</span><span class="sxs-lookup"><span data-stu-id="7ca6a-168">Using a variable in a runbook or DSC configuration</span></span>

<span data-ttu-id="7ca6a-169">Használjon hello **Set-AutomationVariable** tevékenység tooset hello értékének egy automatizálási változó a runbookot vagy a DSC-konfiguráció és a hello **Get-AutomationVariable** tooretrieve azt.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-169">Use hello **Set-AutomationVariable** activity tooset hello value of an Automation variable in a runbook or DSC configuration, and hello **Get-AutomationVariable** tooretrieve it.</span></span>  <span data-ttu-id="7ca6a-170">Hello ne használja **Set-AzureAutomationVariable** vagy **Get-AzureAutomationVariable** parancsmagok a runbookot vagy a DSC-konfiguráció számára, mert azok hello munkafolyamat-tevékenységek-nél kevésbé hatékonyak.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-170">You shouldn't use hello **Set-AzureAutomationVariable** or  **Get-AzureAutomationVariable** cmdlets in a runbook or DSC configuration since they are less efficient than hello workflow activities.</span></span>  <span data-ttu-id="7ca6a-171">Még nem lehet lekérdezni a biztonságos változók értékének hello **Get-AzureAutomationVariable**.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-171">You also cannot retrieve hello value of secure variables with **Get-AzureAutomationVariable**.</span></span>  <span data-ttu-id="7ca6a-172">csak úgy toocreate egy új változót egy runbookon belüli hello vagy a DSC-konfiguráció toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-172">hello only way toocreate a new variable from within a runbook or DSC configuration is toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx)  cmdlet.</span></span>


### <a name="textual-runbook-samples"></a><span data-ttu-id="7ca6a-173">Szöveges forgatókönyvként minták</span><span class="sxs-lookup"><span data-stu-id="7ca6a-173">Textual runbook samples</span></span>

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a><span data-ttu-id="7ca6a-174">És egy egyszerű érték a változóból beolvasása</span><span class="sxs-lookup"><span data-stu-id="7ca6a-174">Setting and retrieving a simple value from a variable</span></span>

<span data-ttu-id="7ca6a-175">hello következő minta parancsok megjelenítése hogyan tooset és a szöveges forgatókönyvként változó beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-175">hello following sample commands show how tooset and retrieve a variable in a textual runbook.</span></span> <span data-ttu-id="7ca6a-176">A példában feltételezzük, hogy az egész szám típusú nevű *NumberOfIterations* és *NumberOfRunnings* és nevű, karakterlánc típusú változó *példában* már létre van hozva.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-176">In this sample, it is assumed that variables of type integer named *NumberOfIterations* and *NumberOfRunnings* and a variable of type string named *SampleMessage* have already been created.</span></span>

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a><span data-ttu-id="7ca6a-177">És egy összetett objektumot egy változóban beolvasása</span><span class="sxs-lookup"><span data-stu-id="7ca6a-177">Setting and retrieving a complex object in a variable</span></span>

<span data-ttu-id="7ca6a-178">hello következő minta kód bemutatja, hogyan tooupdate szöveges forgatókönyvként összetett érték változó.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-178">hello following sample code shows how tooupdate a variable with a complex value in a textual runbook.</span></span> <span data-ttu-id="7ca6a-179">Ez a példa egy Azure virtuális gépen a beolvasott **Get-AzureVM** és mentett tooan meglévő automatizálási változó.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-179">In this sample, an Azure virtual machine is retrieved with **Get-AzureVM** and saved tooan existing Automation variable.</span></span>  <span data-ttu-id="7ca6a-180">A [változó típusok](#variable-types), ez egy PSCustomObject tárolja.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-180">As explained in [Variable types](#variable-types), this is stored as a PSCustomObject.</span></span>

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


<span data-ttu-id="7ca6a-181">A következő kód hello hello érték beolvasva hello változó és használt toostart hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-181">In hello following code, hello value is retrieved from hello variable and used toostart hello virtual machine.</span></span>

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a><span data-ttu-id="7ca6a-182">És egy gyűjtemény változóként beolvasása</span><span class="sxs-lookup"><span data-stu-id="7ca6a-182">Setting and retrieving a collection in a variable</span></span>

<span data-ttu-id="7ca6a-183">hello következő minta kód bemutatja, hogyan toouse egy változó, szöveges forgatókönyvként összetett értékek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-183">hello following sample code shows how toouse a variable with a collection of complex values in a textual runbook.</span></span> <span data-ttu-id="7ca6a-184">Ez a példa több Azure virtuális gépeken a rendszer beolvassa **Get-AzureVM** és mentett tooan meglévő automatizálási változó.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-184">In this sample, multiple Azure virtual machines are retrieved with **Get-AzureVM** and saved tooan existing Automation variable.</span></span>  <span data-ttu-id="7ca6a-185">A [változó típusok](#variable-types), ez PSCustomObjects gyűjteménye tárolja.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-185">As explained in [Variable types](#variable-types), this is stored as a collection of PSCustomObjects.</span></span>

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

<span data-ttu-id="7ca6a-186">A következő kód hello a hello gyűjtemény hello változó lekért és a használt toostart minden virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-186">In hello following code, hello collection is retrieved from hello variable and used toostart each virtual machine.</span></span>

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a><span data-ttu-id="7ca6a-187">Grafikus forgatókönyv minták</span><span class="sxs-lookup"><span data-stu-id="7ca6a-187">Graphical runbook samples</span></span>

<span data-ttu-id="7ca6a-188">A grafikus runbookokban hello hozzáadása **Get-AutomationVariable** vagy **Set-AutomationVariable** hello változó hello grafikus szerkesztő hello könyvtár ablaktáblán a jobb gombbal, és hello kiválasztása kívánt tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-188">In a graphical runbook, you add hello **Get-AutomationVariable** or **Set-AutomationVariable** by right-clicking on hello variable in hello Library pane of hello graphical editor and selecting hello activity you want.</span></span>

![Változó toocanvas hozzáadása](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a><span data-ttu-id="7ca6a-190">A beállítás értéke egy változó</span><span class="sxs-lookup"><span data-stu-id="7ca6a-190">Setting values in a variable</span></span>
<span data-ttu-id="7ca6a-191">hello következő kép bemutatja minta tevékenységek tooupdate egy változó, egy egyszerű érték egy grafikus runbook.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-191">hello following image shows sample activities tooupdate a variable with a simple value in a graphical runbook.</span></span> <span data-ttu-id="7ca6a-192">Ez a példa egy Azure virtuális gép a beolvasott **Get-AzureRmVM** és hello számítógépnév tooan meglévő automatizálási változó mentett karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-192">In this sample, a single Azure virtual machine is retrieved with **Get-AzureRmVM** and hello computer name is saved tooan existing Automation variable with a type of String.</span></span>  <span data-ttu-id="7ca6a-193">Nem számít, hello [egy folyamat vagy feladatütemezési](automation-graphical-authoring-intro.md#links-and-workflow) óta hello kimenet csak várhatóan egy adott objektum.</span><span class="sxs-lookup"><span data-stu-id="7ca6a-193">It doesn't matter whether hello [link is a pipeline or sequence](automation-graphical-authoring-intro.md#links-and-workflow) since we only expect a single object in hello output.</span></span>

![Egyszerű változó beállítása](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a><span data-ttu-id="7ca6a-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7ca6a-195">Next Steps</span></span>

* <span data-ttu-id="7ca6a-196">További információ az tevékenységek összekapcsolása a grafikus szerzői toolearn lásd: [grafikus szerzői hivatkozások](automation-graphical-authoring-intro.md#links-and-workflow)</span><span class="sxs-lookup"><span data-stu-id="7ca6a-196">toolearn more about connecting activities together in graphical authoring, see [Links in graphical authoring](automation-graphical-authoring-intro.md#links-and-workflow)</span></span>
* <span data-ttu-id="7ca6a-197">Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="7ca6a-197">tooget started with Graphical runbooks, see [My first graphical runbook](automation-first-runbook-graphical.md)</span></span> 

