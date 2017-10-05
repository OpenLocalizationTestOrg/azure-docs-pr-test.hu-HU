---
title: "<span data-ttu-id=\"f27f9-101\">Azure Automation integrációs modul létrehozása | Microsoft Docs</span><span class=\"sxs-lookup\"><span data-stu-id=\"f27f9-101\">Create an Azure Automation Integration Module | Microsoft Docs</span></span>"
description: "<span data-ttu-id=\"f27f9-102\">Ez az oktatóprogram végigvezeti az integrációs modulok létrehozásán, tesztelésén és példahasználatán az Azure Automationben.</span><span class=\"sxs-lookup\"><span data-stu-id=\"f27f9-102\">Tutorial that walks you through the creation, testing, and example use of  integration modules in Azure Automation.</span></span>"
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 27798efb-08b9-45d9-9b41-5ad91a3df41e
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/13/2017
ms.author: magoedte
ms.openlocfilehash: aeb06276a52e5472667ae0a741fb3007a91910fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-integration-modules"></a><span data-ttu-id="f27f9-103">Azure Automation integrációs modulok</span><span class="sxs-lookup"><span data-stu-id="f27f9-103">Azure Automation Integration Modules</span></span>
<span data-ttu-id="f27f9-104">Az Azure Automation mögötti alapvető technológia a PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f27f9-104">PowerShell is the fundamental technology behind Azure Automation.</span></span> <span data-ttu-id="f27f9-105">Minthogy az Azure Automation a PowerShellre épül, a PowerShell-modulok kulcsfontosságúak az Azure Automation bővíthetősége szempontjából.</span><span class="sxs-lookup"><span data-stu-id="f27f9-105">Since Azure Automation is built on PowerShell, PowerShell modules are key to the extensibility of Azure Automation.</span></span> <span data-ttu-id="f27f9-106">Ez a cikk bemutatja az Azure Automation „integrációs modulnak” nevezett PowerShell-moduljainak használati jellemzőit, valamint gyakorlati tanácsokat nyújt saját PowerShell-moduljainak létrehozására, hogy azok biztosan működjenek integrációs modulként az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="f27f9-106">In this article, we will guide you through the specifics of Azure Automation’s use of PowerShell modules, referred to as “Integration Modules”, and best practices for creating your own PowerShell modules to make sure they work as Integration Modules within Azure Automation.</span></span> 

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="f27f9-107">Mi az a PowerShell-modul?</span><span class="sxs-lookup"><span data-stu-id="f27f9-107">What is a PowerShell Module?</span></span>
<span data-ttu-id="f27f9-108">Egy PowerShell-modul olyan PowerShell-parancsmagok csoportja, mint a **Get-Date** vagy a **Copy-Item**, amelyek használhatók a PowerShell konzolról, valamint olyan parancsfájlokból, munkafolyamatokból, forgatókönyvekből és PowerShell DSC-erőforrásokból, mint a WindowsFeature vagy File, amelyek használhatók PowerShell DSC konfigurációkból.</span><span class="sxs-lookup"><span data-stu-id="f27f9-108">A PowerShell module is a group of PowerShell cmdlets like **Get-Date** or **Copy-Item**, that can be used from the PowerShell console, scripts, workflows, runbooks, and PowerShell DSC resources like WindowsFeature or File, that can be used from PowerShell DSC configurations.</span></span> <span data-ttu-id="f27f9-109">A PowerShell összes funkciója parancsmagok és DSC-erőforrások révén jelenik meg, és minden parancsmag/DSC-erőforrás mögött egy PowerShell modul áll. Ezek jelentős része magához a PowerShellhez tartozik.</span><span class="sxs-lookup"><span data-stu-id="f27f9-109">All of the functionality of PowerShell is exposed through cmdlets and DSC resources, and every cmdlet/DSC resource is backed by a PowerShell module, many of which ship with PowerShell itself.</span></span> <span data-ttu-id="f27f9-110">A **Get-Date** parancsmag például része a Microsoft.PowerShell.Utility PowerShell-modulnak, a **Copy-Item** parancsmag része a Microsoft.PowerShell.Management PowerShell-modulnak, a Csomag DSC erőforrás pedig része a PSDesiredStateConfiguration PowerShell-modulnak.</span><span class="sxs-lookup"><span data-stu-id="f27f9-110">For example, the **Get-Date** cmdlet is part of the Microsoft.PowerShell.Utility PowerShell module, and **Copy-Item** cmdlet is part of the Microsoft.PowerShell.Management PowerShell module and the Package DSC resource is part of the PSDesiredStateConfiguration PowerShell module.</span></span> <span data-ttu-id="f27f9-111">A PowerShellben mindkét modul megtalálható.</span><span class="sxs-lookup"><span data-stu-id="f27f9-111">Both of these modules ship with PowerShell.</span></span> <span data-ttu-id="f27f9-112">Sok PowerShell-modul azonban nem része automatikusan a PowerShellnek, hanem olyan első vagy harmadik felek termékeivel érkeznek, mint a System Center 2012 Configuration Manager, illetve a hatalmas PowerShell-közösség biztosítja például a PowerShell-galériához hasonló helyeken.</span><span class="sxs-lookup"><span data-stu-id="f27f9-112">But many PowerShell modules do not ship as part of PowerShell, and are instead distributed with first or third-party products like System Center 2012 Configuration Manager or by the vast PowerShell community on places like PowerShell Gallery.</span></span>  <span data-ttu-id="f27f9-113">A modulok hasznosak, mert a beágyazott funkciók révén egyszerűbbé teszik a bonyolult feladatokat.</span><span class="sxs-lookup"><span data-stu-id="f27f9-113">The modules are useful because they make complex tasks simpler through encapsulated functionality.</span></span>  <span data-ttu-id="f27f9-114">További információt talál a [PowerShell-modulokról az MSDN webhelyén](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="f27f9-114">You can learn more about [PowerShell modules on MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span></span> 

## <a name="what-is-an-azure-automation-integration-module"></a><span data-ttu-id="f27f9-115">Mi az az Azure Automation integrációs modul?</span><span class="sxs-lookup"><span data-stu-id="f27f9-115">What is an Azure Automation Integration Module?</span></span>
<span data-ttu-id="f27f9-116">Az integrációs modul nem különbözik sokban a többi PowerShell-modultól.</span><span class="sxs-lookup"><span data-stu-id="f27f9-116">An Integration Module isn't very different from a PowerShell module.</span></span> <span data-ttu-id="f27f9-117">Egyszerűen egy olyan PowerShell-modul, amely opcionálisan tartalmaz egy további fájlt, egy Azure Automation kapcsolattípust megadó metaadatfájlt, amely a modul parancsmagjaival használható a forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="f27f9-117">Its simply a PowerShell module that optionally contains one additional file - a metadata file specifying an Azure Automation connection type to be used with the module's cmdlets in runbooks.</span></span> <span data-ttu-id="f27f9-118">Ezek a PowerShell-modulok (az opcionális fájllal vagy anélkül) importálhatók az Azure Automationbe, hogy a parancsmagjaik használhatók legyenek a forgatókönyvekben, és a DSC-erőforrásaik elérhetők legyenek használatra a DSC-konfigurációkon belülről.</span><span class="sxs-lookup"><span data-stu-id="f27f9-118">Optional file or not, these PowerShell modules can be imported into Azure Automation to make their cmdlets available for use within runbooks and their DSC resources available for use within DSC configurations.</span></span> <span data-ttu-id="f27f9-119">A színfalak mögött az Azure Automation tárolja ezeket a modulokat, és a forgatókönyv-feladat és a DSC-fordítási feladat a végrehajtásakor betölti őket az Azure Automation próbakörnyezetbe, ahol a rendszer végrehajtja a forgatókönyveket és a lefordítja a DSC-konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="f27f9-119">Behind the scenes, Azure Automation stores these modules, and at runbook job and DSC compilation job execution time, loads them into the Azure Automation sandboxes where runbooks are executed and DSC configurations are compiled.</span></span>  <span data-ttu-id="f27f9-120">A modulok DSC-erőforrásai is automatikusan kikerülnek az Automation DSC lekéréses kiszolgálóra, így a DSC-konfigurációkat alkalmazni próbáló gépek lekérhetik őket.</span><span class="sxs-lookup"><span data-stu-id="f27f9-120">Any DSC resources in modules are also automatically placed on the Automation DSC pull server, so that they can be pulled by machines attempting to apply DSC configurations.</span></span>  

<span data-ttu-id="f27f9-121">Az Azure Automation számos Azure PowerShell-modult tartalmaz használatra készen, hogy azonnal elkezdhesse az Azure-felügyelet automatizálását, továbbá importálhat PowerShell-modulokat is bármilyen integrálni kívánt rendszerhez, szolgáltatáshoz vagy eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="f27f9-121">We ship a number of Azure  PowerShell modules out of the box in Azure Automation for you to use so you can get started automating Azure management right away, but you can import PowerShell modules for whatever system, service, or tool you want to integrate with.</span></span> 

> [!NOTE]
> <span data-ttu-id="f27f9-122">Bizonyos modulok „globális modulként” érhetők el az Automation szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="f27f9-122">Certain modules are shipped as “global modules” in the Automation service.</span></span> <span data-ttu-id="f27f9-123">Ezek a globális modulok egy Automation-fiók létrehozásakor a rendelkezésére állnak, és időnként frissítjük őket, ami automatikusan leküldi a modulokat az Automation-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="f27f9-123">These global modules are available to you when you create an automation account, and we update them sometimes which automatically pushes them out to your automation account.</span></span> <span data-ttu-id="f27f9-124">Ha nem szeretné a modulok automatikus frissítését, bármikor importálhatja ugyanazon modulokat, így elsőbbséget élveznek majd azokkal a globális modulverziókkal szemben, amelyeket a szolgáltatásban közzéteszünk.</span><span class="sxs-lookup"><span data-stu-id="f27f9-124">If you don’t want them to be auto-updated, you can always import the same module yourself, and that will take precedence over the global module version of that module that we ship in the service.</span></span> 

<span data-ttu-id="f27f9-125">Az importálni kívánt integrációsmodul-csomag formátuma a modullal azonos nevet viselő, .zip kiterjesztésű tömörített fájl.</span><span class="sxs-lookup"><span data-stu-id="f27f9-125">The format in which you import an Integration Module package is a compressed file with the same name as the module and a .zip extension.</span></span> <span data-ttu-id="f27f9-126">A mappa tartalmazza a Windows PowerShell-modult és minden kiegészítő fájlt, beleértve a jegyzékfájlt (.psd1) is, ha a modul rendelkezik ilyennel.</span><span class="sxs-lookup"><span data-stu-id="f27f9-126">It contains the Windows PowerShell module and any supporting files, including a manifest file (.psd1) if the module has one.</span></span>

<span data-ttu-id="f27f9-127">Ha a modul esetleg tartalmaz egy Azure Automation kapcsolattípust, akkor tartalmaznia kell egy `<ModuleName>-Automation.json` nevű fájlt is, amely meghatározza a kapcsolattípus tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f27f9-127">If the module should contain an Azure Automation connection type, it must also contain a file with the name `<ModuleName>-Automation.json` that specifies the connection type properties.</span></span> <span data-ttu-id="f27f9-128">Ez a tömörített .zip fájl modulmappájában elhelyezett json-fájl, és tartalmazza a modul által képviselt rendszerhez vagy szolgáltatáshoz való csatlakozáshoz szükséges „kapcsolat” mezőit.</span><span class="sxs-lookup"><span data-stu-id="f27f9-128">This is a json file placed within the module folder of your compressed .zip file, and contains the fields of a “connection” that is required to connect to the system or service the module represents.</span></span> <span data-ttu-id="f27f9-129">Ennek eredményeképp létrejön egy kapcsolattípus az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="f27f9-129">This will end up creating a connection type in Azure Automation.</span></span> <span data-ttu-id="f27f9-130">Ezzel a fájllal megadhatja a modul kapcsolattípusához a mezőneveket, a típusait, valamint azt, hogy a mezőket titkosítani kell-e és/vagy hogy kötelezők-e.</span><span class="sxs-lookup"><span data-stu-id="f27f9-130">Using this file you can set the field names, types, and whether the fields should be encrypted and / or optional, for the connection type of the module.</span></span> <span data-ttu-id="f27f9-131">Az alábbi egy json fájlformátumú sablon:</span><span class="sxs-lookup"><span data-stu-id="f27f9-131">The following is a template in the json file format:</span></span>

```
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

<span data-ttu-id="f27f9-132">Ha már telepítette a Service Management Automation szolgáltatást, és létrehozott integrációsmodul-csomagokat az automatizálási forgatókönyvekhez, ez nagyon ismerős lesz.</span><span class="sxs-lookup"><span data-stu-id="f27f9-132">If you have deployed Service Management Automation and created Integration Modules packages for your automation runbooks, this should look very familiar to you.</span></span> 

## <a name="authoring-best-practices"></a><span data-ttu-id="f27f9-133">Szerzői gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="f27f9-133">Authoring Best Practices</span></span>
<span data-ttu-id="f27f9-134">Bár az integrációs modulok lényegében PowerShell modulok, számos dolog van még, amit érdemes meggondolni egy PowerShell-modul létrehozásakor, hogy a lehető legtöbb hasznukat vegye az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="f27f9-134">Even though Integration Modules are essentially PowerShell modules, there’s still a number of things we recommend you consider while authoring a PowerShell module, to make it most usable in Azure Automation.</span></span> <span data-ttu-id="f27f9-135">Ezeknek egy része kifejezetten az Azure Automationre vonatkozik, mások pedig ahhoz hasznosak, hogy a moduljai jól működjenek a PowerShell-munkafolyamatban, függetlenül attól, hogy használja-e az Automationt, vagy sem.</span><span class="sxs-lookup"><span data-stu-id="f27f9-135">Some of these are Azure Automation specific, and some of them are useful just to make your modules work well in PowerShell Workflow, regardless of whether or not you’re using Automation.</span></span> 

1. <span data-ttu-id="f27f9-136">A modul összes parancsmagjáról mellékeljen egy szinopszist, egy leírást és egy súgó URI-t.</span><span class="sxs-lookup"><span data-stu-id="f27f9-136">Include a synopsis, description, and help URI for every cmdlet in the module.</span></span> <span data-ttu-id="f27f9-137">A PowerShellben meghatározhat bizonyos súgóinformációkat a parancsmagokhoz, hogy a felhasználó segítséget kapjon a **Get-Help** parancsmaggal való használatukkor.</span><span class="sxs-lookup"><span data-stu-id="f27f9-137">In PowerShell, you can define certain help information for cmdlets to allow the user to receive help on using them with the **Get-Help** cmdlet.</span></span> <span data-ttu-id="f27f9-138">Az alábbi példát követve meghatározhatja a szinopszist és a súgó URI-t egy .psm1 fájlban megírt PowerShell-modulhoz.</span><span class="sxs-lookup"><span data-stu-id="f27f9-138">For example, here’s how you can define a synopsis and help URI for a PowerShell module written in a .psm1 file.</span></span><br>  
   
    ```
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='http://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> <span data-ttu-id="f27f9-139">Ha ezt az információt megadja, nem csak a súgó jelenik meg a **Get-Help** parancsmag segítségével a PowerShell konzolon, de megjeleníti ezt a súgó funkciót az Azure Automation felületén is.</span><span class="sxs-lookup"><span data-stu-id="f27f9-139">Providing this info will not only show this help using the **Get-Help** cmdlet in the PowerShell console, it will also expose this help functionality within Azure Automation.</span></span>  <span data-ttu-id="f27f9-140">Például amikor tevékenységet szúr be a forgatókönyvek létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="f27f9-140">For example, when inserting activities during runbook authoring.</span></span> <span data-ttu-id="f27f9-141">Ha a „Részletes súgó megtekintése” lehetőségre kattint, megnyílik a súgó URI az Azure Automation eléréséhez használt webböngésző új lapján.</span><span class="sxs-lookup"><span data-stu-id="f27f9-141">Clicking “View detailed help” will open the help URI in another tab of the web browser you’re using to access Azure Automation.</span></span><br><span data-ttu-id="f27f9-142">![Integrációs modul súgó](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span><span class="sxs-lookup"><span data-stu-id="f27f9-142">![Integration Module Help](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span></span>
2. <span data-ttu-id="f27f9-143">Ha a modul egy távoli rendszeren fut:</span><span class="sxs-lookup"><span data-stu-id="f27f9-143">If the module runs against a remote system,</span></span>

    <span data-ttu-id="f27f9-144">a.</span><span class="sxs-lookup"><span data-stu-id="f27f9-144">a.</span></span> <span data-ttu-id="f27f9-145">Tartalmaznia kell az integrációs modulhoz tartozó metaadatfájlt, amely meghatározza az adott távoli rendszerhez való csatlakozáshoz szükséges információkat, azaz a kapcsolat típusát.</span><span class="sxs-lookup"><span data-stu-id="f27f9-145">It should contain an Integration Module metadata file that defines the information needed to connect to that remote system, meaning the connection type.</span></span>  
    <span data-ttu-id="f27f9-146">b.</span><span class="sxs-lookup"><span data-stu-id="f27f9-146">b.</span></span> <span data-ttu-id="f27f9-147">A modul minden egyes parancsmagjának képesnek kell lennie egy kapcsolat objektum (az adott kapcsolattípus egy példánya) befogadására paraméterként.</span><span class="sxs-lookup"><span data-stu-id="f27f9-147">Each cmdlet in the module should be able to take in a connection object (an instance of that connection type) as a parameter.</span></span>  

    <span data-ttu-id="f27f9-148">A modulban lévő parancsmagokat könnyebb használni az Azure Automationben, ha lehetővé teszi, hogy egy objektumot a kapcsolattípus mezőivel paraméterként továbbítson a parancsmagnak.</span><span class="sxs-lookup"><span data-stu-id="f27f9-148">Cmdlets in the module become easier to use in Azure Automation if you allow passing an object with the fields of the connection type as a parameter to the cmdlet.</span></span> <span data-ttu-id="f27f9-149">Ily módon a felhasználóknak nem kell leképezniük a kapcsolat adategység-paramétereit a parancsmag megfelelő paramétereihez minden alkalommal, amikor meghívnak egy parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="f27f9-149">This way users don’t have to map parameters of the connection asset to the cmdlet's corresponding parameters each time they call a cmdlet.</span></span> <span data-ttu-id="f27f9-150">A fenti forgatókönyv-példa alapján egy CorpTwilio nevű Twilio-kapcsolati adategység segítségével éri el a Twiliót, és visszaadja az összes telefonszámot a fiókból.</span><span class="sxs-lookup"><span data-stu-id="f27f9-150">Based on the runbook example above, it uses a Twilio connection asset called CorpTwilio to access Twilio and return all the phone numbers in the account.</span></span>  <span data-ttu-id="f27f9-151">Figyelje meg, hogyan képezi le a kapcsolat mezőit a parancsmag paramétereire.</span><span class="sxs-lookup"><span data-stu-id="f27f9-151">Notice how it is mapping the fields of the connection to the parameters of the cmdlet?</span></span><br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    <span data-ttu-id="f27f9-152">Egyszerűbb és jobb megoldás erre, ha közvetlenül továbbítja a kapcsolat objektumot a parancsmagnak:</span><span class="sxs-lookup"><span data-stu-id="f27f9-152">An easier and better way to approach this is directly passing the connection object to the cmdlet -</span></span>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    <span data-ttu-id="f27f9-153">Ilyen viselkedést engedélyezhet a parancsmagjaihoz, ha lehetővé teszi, hogy elfogadjon egy kapcsolat objektumot közvetlenül paraméterként ahelyett, hogy ha csak egy paraméter kapcsolatmezői lennének.</span><span class="sxs-lookup"><span data-stu-id="f27f9-153">You can enable behavior like this for your cmdlets by allowing them to accept a connection object directly as a parameter, instead of just connection fields for parameters.</span></span> <span data-ttu-id="f27f9-154">Általában nincs szükség mindegyikhez egy paraméterkészletre, így egy nem Azure Automationt használó ügyfél is meghívhatja a parancsmagjait anélkül, hogy létrehozna egy kapcsolat objektumként funkcionáló kivonattáblát.</span><span class="sxs-lookup"><span data-stu-id="f27f9-154">Usually you’ll want a parameter set for each, so that a user not using Azure Automation can call your cmdlets without constructing a hashtable to act as the connection object.</span></span> <span data-ttu-id="f27f9-155">Az alábbi **SpecifyConnectionFields** paraméterkészlettel továbbíthatja egyenként a kapcsolatmező-tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="f27f9-155">Parameter set **SpecifyConnectionFields** below is used to pass the connection field properties one by one.</span></span> <span data-ttu-id="f27f9-156">A **UseConnectionObject** segítségével továbbíthatja egyenesen a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f27f9-156">**UseConnectionObject** lets you pass the connection straight through.</span></span> <span data-ttu-id="f27f9-157">Ahogy látja, a [Twilio PowerShell-modul](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) Send-TwilioSMS parancsmagja lehetővé teszi bármelyik továbbítást.</span><span class="sxs-lookup"><span data-stu-id="f27f9-157">As you can see, the Send-TwilioSMS cmdlet in the [Twilio PowerShell module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) allows passing either way:</span></span> 
   
    ```
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='http://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
3. <span data-ttu-id="f27f9-158">Adja meg a modulban lévő összes parancsmag kimenettípusát.</span><span class="sxs-lookup"><span data-stu-id="f27f9-158">Define output type for all cmdlets in the module.</span></span> <span data-ttu-id="f27f9-159">Egy parancsmag kimenettípusának megadása lehetővé teszi a tervezés közben az IntelliSense használatát, amely segít meghatározni egy parancsmag kimenetének tulajdonságait a megírásuk támogatására.</span><span class="sxs-lookup"><span data-stu-id="f27f9-159">Defining an output type for a cmdlet allows design-time IntelliSense to help you determine the output properties of the cmdlet, for use during authoring.</span></span> <span data-ttu-id="f27f9-160">Ez különösen hasznos az Automation-forgatókönyvek grafikus létrehozásakor, ahol a tervezés közbeni információk kulcsfontosságúak a könnyedfelhasználói élmény biztosításához a modulban.</span><span class="sxs-lookup"><span data-stu-id="f27f9-160">It is especially helpful during Automation runbook graphical authoring, where design time knowledge is key to an easy user experience with your module.</span></span><br><br> <span data-ttu-id="f27f9-161">![Grafikus runbook kimenettípusa](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span><span class="sxs-lookup"><span data-stu-id="f27f9-161">![Graphical Runbook Output Type](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span></span><br> <span data-ttu-id="f27f9-162">Ez hasonlít egy parancsmag PowerShell ISE-ben kapott kimenetének „type ahead” funkciójára, csak nem kell futtatni.</span><span class="sxs-lookup"><span data-stu-id="f27f9-162">This is similar to the "type ahead" functionality of a cmdlet's output in PowerShell ISE without having to run it.</span></span><br><br> <span data-ttu-id="f27f9-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span><span class="sxs-lookup"><span data-stu-id="f27f9-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span></span><br>
4. <span data-ttu-id="f27f9-164">A modul parancsmagjai elméletben nem fogadnak komplex objektumtípusokat paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="f27f9-164">Cmdlets in the module should not take complex object types for parameters.</span></span> <span data-ttu-id="f27f9-165">A PowerShell-munkafolyamat eltér a PowerShelltől abban, hogy komplex típusokat tárol deszerializált formában.</span><span class="sxs-lookup"><span data-stu-id="f27f9-165">PowerShell Workflow is different from PowerShell in that it stores complex types in deserialized form.</span></span> <span data-ttu-id="f27f9-166">Az egyszerű típusok egyszerűek maradnak, a komplex típusokat azonban a rendszer deszerializált változatokká konvertálja, amelyek lényegében tulajdonságcsomagok.</span><span class="sxs-lookup"><span data-stu-id="f27f9-166">Primitive types will stay as primitives, but complex types are converted to their deserialized versions, which are essentially property bags.</span></span> <span data-ttu-id="f27f9-167">Ha például a **Get-Process** parancsmagot használta egy forgatókönyvben (vagy egy PowerShell-munkafolyamatban), az egy [Deserialized.System.Diagnostic.Process] típusú, nem pedig a várt [System.Diagnostic.Process] típusú objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f27f9-167">For example, if you used the **Get-Process** cmdlet in a runbook (or a PowerShell Workflow for that matter), it would return an object of type [Deserialized.System.Diagnostic.Process], not the expected [System.Diagnostic.Process] type.</span></span> <span data-ttu-id="f27f9-168">Ez a típus ugyanazokkal a tulajdonságokkal rendelkezik, mint a nem deszerializált típus, de a metódusok nélkül.</span><span class="sxs-lookup"><span data-stu-id="f27f9-168">This type has all the same properties as the non-deserialized type, but none of the methods.</span></span> <span data-ttu-id="f27f9-169">Ha megpróbálja ezt az értéket paraméterként átadni egy parancsmagnak, ahol a parancsmag egy [System.Diagnostic.Process] értéket vár ehhez a paraméterhez, az alábbi hibaüzenetet kapja: *A 'process' paraméteren nem hajtható végre az argumentum-átalakítás. Hiba: „Nem lehetséges a „Deserialized.System.Diagnostics.Process” típusú „System.Diagnostics.Process (CcmExec)” érték konvertálása a következő típusra: „System.Diagnostics.Process”.*</span><span class="sxs-lookup"><span data-stu-id="f27f9-169">And if you try to pass this value as a parameter to a cmdlet, where the cmdlet expects a [System.Diagnostic.Process] value for this parameter, you’ll receive the following error: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert the "System.Diagnostics.Process (CcmExec)" value of type  "Deserialized.System.Diagnostics.Process" to type "System.Diagnostics.Process".*</span></span>   <span data-ttu-id="f27f9-170">Ennek az az oka, hogy típusbeli eltérés van a várt [System.Diagnostic.Process] típus és a megadott [Deserialized.System.Diagnostic.Process] típus között.</span><span class="sxs-lookup"><span data-stu-id="f27f9-170">This is because there is a type mismatch between the expected [System.Diagnostic.Process] type and the given [Deserialized.System.Diagnostic.Process] type.</span></span> <span data-ttu-id="f27f9-171">A probléma megoldása az, ha biztosítjuk, hogy a modul parancsmagjai ne fogadjanak be komplex típusú paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f27f9-171">The way around this issue is to ensure the cmdlets of your module do not take complex types for parameters.</span></span> <span data-ttu-id="f27f9-172">Itt látható ennek a téves megközelítése.</span><span class="sxs-lookup"><span data-stu-id="f27f9-172">Here is the wrong way to do it.</span></span>
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    <span data-ttu-id="f27f9-173">Ez pedig a helyes mód: befogad egy primitívet, amely belső használatával a parancsmag befogadhatja és használhatja a komplex objektumot.</span><span class="sxs-lookup"><span data-stu-id="f27f9-173">And here is the right way, taking in a primitive that can be used internally by the cmdlet to grab the complex object and use it.</span></span> <span data-ttu-id="f27f9-174">Minthogy a parancsmagokat a rendszer a PowerShell környezetében hajtja végre, nem a PowerShell-munkafolyamatéban, a parancsmagon belül $process lesz a helyes [System.Diagnostic.Process] típus.</span><span class="sxs-lookup"><span data-stu-id="f27f9-174">Since cmdlets execute in the context of PowerShell, not PowerShell Workflow, inside the cmdlet $process becomes the correct [System.Diagnostic.Process] type.</span></span>  
   
    ```
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   <span data-ttu-id="f27f9-175">A kapcsolati adategységek a forgatókönyvekben kivonattáblák, amelyek komplex típusúak, de úgy tűnik, hogy ezek a kivonattáblák bekerülhetnek a parancsmagokba –Connection paraméterként, és a rendszer nem ad vissza kivételt.</span><span class="sxs-lookup"><span data-stu-id="f27f9-175">Connection assets in runbooks are hashtables, which are a complex type, and yet these hashtables seem to be able to be passed into cmdlets for their –Connection parameter perfectly, with no cast exception.</span></span> <span data-ttu-id="f27f9-176">Technikailag bizonyos PowerShell-típusok képesek megfelelően átalakulni a szerializált formájukból a deszerializált formájukba, és így átadhatók a nem deszerializált típust fogadó paraméterként a parancsmagoknak.</span><span class="sxs-lookup"><span data-stu-id="f27f9-176">Technically, some PowerShell types are able to cast properly from their serialized form to their deserialized form, and hence can be passed into cmdlets for parameters accepting the non-deserialized type.</span></span> <span data-ttu-id="f27f9-177">A kivonattábla egy ezek közül.</span><span class="sxs-lookup"><span data-stu-id="f27f9-177">Hashtable is one of these.</span></span> <span data-ttu-id="f27f9-178">Lehetséges implementálni egy modul szerzőjének definiált típusait oly módon, hogy a deszerializálásuk is helyesen történjen meg, de ehhez néhány dolgot feláldozását érdemes fontolóra venni.</span><span class="sxs-lookup"><span data-stu-id="f27f9-178">It’s possible for a module author’s defined types to be implemented in a way that they can correctly deserialize as well, but there are some trade-offs to consider.</span></span> <span data-ttu-id="f27f9-179">A típusnak rendelkeznie kell alapértelmezett konstruktorral, minden tulajdonságának nyilvánosnak kell kennie, és rendelkeznie kell egy PSTypeConverter elemmel.</span><span class="sxs-lookup"><span data-stu-id="f27f9-179">The type needs to have a default constructor, have all of its properties public, and have a PSTypeConverter.</span></span> <span data-ttu-id="f27f9-180">A modulkészítő által nem birtokolt, már definiált típusoknál nincs mód a „javításra”, így azt javasoljuk, hogy általában kerülje a paramétereknél az összetett típusokat.</span><span class="sxs-lookup"><span data-stu-id="f27f9-180">However, for already-defined types that the module author does not own, there is no way to “fix” them, hence the recommendation to avoid complex types for parameters all together.</span></span> <span data-ttu-id="f27f9-181">Forgatókönyv-készítési tipp: Ha valamilyen okból a parancsmagjainak komplex típusú paraméterre van szüksége, vagy valaki más komplex típusú paramétert igénylő moduljára van szüksége, PowerShell-alapú munkafolyamat-forgatókönyvekben és helyi PowerShellben lévő PowerShell-munkafolyamatokban a megoldás az, hogy ha becsomagolja a komplex típust létrehozó parancsmagot és a komplex típust használó parancsmagot ugyanabba az InlineScript-tevékenységbe.</span><span class="sxs-lookup"><span data-stu-id="f27f9-181">Runbook Authoring tip: If for some reason your cmdlets need to take a complex type parameter, or you are using someone else’s module that requires a complex type parameter, the workaround in PowerShell Workflow runbooks and PowerShell Workflows in local PowerShell, is to wrap the cmdlet that generates the complex type and the cmdlet that consumes the complex type in the same InlineScript activity.</span></span> <span data-ttu-id="f27f9-182">Minthogy az InlineScript a tartalmait inkább a PowerShellhez, mint a PowerShell-munkafolyamathoz hasonlóan hajtja végre, a komplex típust létrehozó parancsmag a helyes típust hozza létre, nem a deszerializált komplex típust.</span><span class="sxs-lookup"><span data-stu-id="f27f9-182">Since InlineScript executes its contents as PowerShell rather than PowerShell Workflow, the cmdlet generating the complex type would produce that correct type, not the deserialized complex type.</span></span>
5. <span data-ttu-id="f27f9-183">A modul egyik parancsmagjának se legyen állapota.</span><span class="sxs-lookup"><span data-stu-id="f27f9-183">Make all cmdlets in the module stateless.</span></span> <span data-ttu-id="f27f9-184">A PowerShell-munkafolyamat a munkafolyamatban meghívott parancsmagok mindegyikét külön munkamenetben hívja meg.</span><span class="sxs-lookup"><span data-stu-id="f27f9-184">PowerShell Workflow runs every cmdlet called in the workflow in a different session.</span></span> <span data-ttu-id="f27f9-185">Ez azt jelenti, hogy azok a parancsmagok, amelyek ugyanazon modul más parancsmagjai által létrehozott/módosított munkamenet-állapottól függenek, nem fognak működni a PowerShell-alapú munkafolyamat-forgatókönyvekben.</span><span class="sxs-lookup"><span data-stu-id="f27f9-185">This means any cmdlets that depend on session state created / modified by other cmdlets in the same module will not work in PowerShell Workflow runbooks.</span></span>  <span data-ttu-id="f27f9-186">Az alábbi példa bemutatja, mit ne tegyen.</span><span class="sxs-lookup"><span data-stu-id="f27f9-186">Here is an example of what not to do.</span></span>
   
    ```
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
6. <span data-ttu-id="f27f9-187">A modulnak teljes egészében egy Xcopy-kompatibilis csomagban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f27f9-187">The module should be fully contained in an Xcopy-able package.</span></span> <span data-ttu-id="f27f9-188">Mivel az Azure Automation-modulokat a rendszer elosztja az Automation próbakörnyezetekbe, amikor a forgatókönyveket végre kell hajtani, függetlenül kell működniük a gazdagéptől, amelyen futnak.</span><span class="sxs-lookup"><span data-stu-id="f27f9-188">Because Azure Automation modules are distributed to the Automation sandboxes when runbooks need to execute, they need to work independently of the host they are running on.</span></span> <span data-ttu-id="f27f9-189">Ez azt jelenti, hogy képesnek kell lennie a modulcsomag tömörítésére és bármely más, azonos vagy újabb PowerShell-verziót futtató gazdagépre való áthelyezésére, valamint normális működtetésére, amikor a rendszer importálja az adott gazdagép PowerShell környezetébe.</span><span class="sxs-lookup"><span data-stu-id="f27f9-189">What this means is that you should be able to Zip up the module package, move it to any other host with the same or newer PowerShell version, and have it function as normal when imported into that host’s PowerShell environment.</span></span> <span data-ttu-id="f27f9-190">Annak érdekében, hogy ez megtörténjen, fontos, hogy a modul ne függjön egy fájltól sem a modulmappán kívül (ez a Azure Automationbe való importáláskor tömörített mappa), sem a gazdagép bármilyen egyedi beállításjegyzékétől, mint például a termék telepítésekor beállítottaktól.</span><span class="sxs-lookup"><span data-stu-id="f27f9-190">In order for that to happen, the module should not depend on any files outside the module folder (the folder that gets zipped up when importing into Azure Automation), or on any unique registry settings on a host, such as those set by the install of a product.</span></span> <span data-ttu-id="f27f9-191">Ha nem tartja be az ajánlott gyakorlatot, a modul nem lesz használható az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="f27f9-191">If this best practice is not followed, the module will not be usable in Azure Automation.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="f27f9-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f27f9-192">Next steps</span></span>

* <span data-ttu-id="f27f9-193">A PowerShell-alapú munkafolyamat-forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első PowerShell-alapú munkafolyamat-forgatókönyvem](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="f27f9-193">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="f27f9-194">További információk PowerShell-modulok létrehozásáról: [Windows PowerShell-modul írása](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="f27f9-194">To learn more about creating PowerShell Modules, see [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span></span>

