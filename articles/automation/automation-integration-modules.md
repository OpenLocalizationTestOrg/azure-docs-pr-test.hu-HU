---
title: "egy Azure Automation-integrációs modul aaaCreate |} Microsoft Docs"
description: "Útmutató, amely végigvezeti az Azure Automationben integrációs modulok hello létrehozása, tesztelése és példa használatát."
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
ms.openlocfilehash: d4064d8b106496f4dab442024131886c4affccab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-integration-modules"></a><span data-ttu-id="373e0-103">Azure Automation integrációs modulok</span><span class="sxs-lookup"><span data-stu-id="373e0-103">Azure Automation Integration Modules</span></span>
<span data-ttu-id="373e0-104">PowerShell hello technológia Azure Automation mögött.</span><span class="sxs-lookup"><span data-stu-id="373e0-104">PowerShell is hello fundamental technology behind Azure Automation.</span></span> <span data-ttu-id="373e0-105">Azure Automation PowerShell épül, mivel a PowerShell-modulok kulcs toohello bővítési az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="373e0-105">Since Azure Automation is built on PowerShell, PowerShell modules are key toohello extensibility of Azure Automation.</span></span> <span data-ttu-id="373e0-106">Ebben a cikkben azt végigvezeti Azure Automation PowerShell-modulok használata hello mintaadatokról tooas "Integrációs modulok" említett és gyakorlati tanácsok a saját PowerShell modulok toomake meg arról, hogy működnek-integrációs modulok belül létrehozásához Azure Automation szolgáltatásbeli.</span><span class="sxs-lookup"><span data-stu-id="373e0-106">In this article, we will guide you through hello specifics of Azure Automation’s use of PowerShell modules, referred tooas “Integration Modules”, and best practices for creating your own PowerShell modules toomake sure they work as Integration Modules within Azure Automation.</span></span> 

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="373e0-107">Mi az a PowerShell-modul?</span><span class="sxs-lookup"><span data-stu-id="373e0-107">What is a PowerShell Module?</span></span>
<span data-ttu-id="373e0-108">Egy PowerShell-modul olyan PowerShell-parancsmagokat például **Get-Date** vagy **elem**, amely használható a hello PowerShell-konzolban, a parancsfájlok, a munkafolyamatok, a runbookokat, és a PowerShell DSC erőforrások – például WindowsFeature vagy a fájl, a PowerShell DSC-konfigurációk használható.</span><span class="sxs-lookup"><span data-stu-id="373e0-108">A PowerShell module is a group of PowerShell cmdlets like **Get-Date** or **Copy-Item**, that can be used from hello PowerShell console, scripts, workflows, runbooks, and PowerShell DSC resources like WindowsFeature or File, that can be used from PowerShell DSC configurations.</span></span> <span data-ttu-id="373e0-109">Összes hello funkció PowerShell parancsmagok és a DSC-erőforrások ki van téve, és minden parancsmag/DSC erőforrás egy PowerShell-modul által támogatott sok, amely küldje el a PowerShell használatával saját magát.</span><span class="sxs-lookup"><span data-stu-id="373e0-109">All of hello functionality of PowerShell is exposed through cmdlets and DSC resources, and every cmdlet/DSC resource is backed by a PowerShell module, many of which ship with PowerShell itself.</span></span> <span data-ttu-id="373e0-110">Például hello **Get-Date** parancsmag hello Microsoft.PowerShell.Utility PowerShell modulja részét képezi, és **elem** parancsmag hello Microsoft.PowerShell.Management PowerShell-modulja részét képezi, és hello csomag DSC erőforrás hello PSDesiredStateConfiguration PowerShell-modulja részét képezi.</span><span class="sxs-lookup"><span data-stu-id="373e0-110">For example, hello **Get-Date** cmdlet is part of hello Microsoft.PowerShell.Utility PowerShell module, and **Copy-Item** cmdlet is part of hello Microsoft.PowerShell.Management PowerShell module and hello Package DSC resource is part of hello PSDesiredStateConfiguration PowerShell module.</span></span> <span data-ttu-id="373e0-111">A PowerShellben mindkét modul megtalálható.</span><span class="sxs-lookup"><span data-stu-id="373e0-111">Both of these modules ship with PowerShell.</span></span> <span data-ttu-id="373e0-112">Azonban számos PowerShell-modult nem PowerShell részét képezi, és ehelyett eszközzel együtt első vagy a külső termékek, például a System Center 2012 Configuration Manager vagy hello túlnyomó PowerShell közösségi vagy hasonló PowerShell-galériában a.</span><span class="sxs-lookup"><span data-stu-id="373e0-112">But many PowerShell modules do not ship as part of PowerShell, and are instead distributed with first or third-party products like System Center 2012 Configuration Manager or by hello vast PowerShell community on places like PowerShell Gallery.</span></span>  <span data-ttu-id="373e0-113">hello modulok hasznosak, mert azok összetett feladatok egyszerűbbé teszik beágyazott funkció segítségével.</span><span class="sxs-lookup"><span data-stu-id="373e0-113">hello modules are useful because they make complex tasks simpler through encapsulated functionality.</span></span>  <span data-ttu-id="373e0-114">További információt talál a [PowerShell-modulokról az MSDN webhelyén](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="373e0-114">You can learn more about [PowerShell modules on MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span></span> 

## <a name="what-is-an-azure-automation-integration-module"></a><span data-ttu-id="373e0-115">Mi az az Azure Automation integrációs modul?</span><span class="sxs-lookup"><span data-stu-id="373e0-115">What is an Azure Automation Integration Module?</span></span>
<span data-ttu-id="373e0-116">Az integrációs modul nem különbözik sokban a többi PowerShell-modultól.</span><span class="sxs-lookup"><span data-stu-id="373e0-116">An Integration Module isn't very different from a PowerShell module.</span></span> <span data-ttu-id="373e0-117">A egyszerűen egy PowerShell-modul egy további fájl - egy metaadat-fájlt, amely megadja egy Azure Automation szolgáltatásbeli kapcsolódási típus toobe használt runbookokat hello modul parancsmagjai opcionálisan tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="373e0-117">Its simply a PowerShell module that optionally contains one additional file - a metadata file specifying an Azure Automation connection type toobe used with hello module's cmdlets in runbooks.</span></span> <span data-ttu-id="373e0-118">Nem kötelező a fájl vagy nem, a PowerShell-modulok importálhatók az Azure Automation toomake az elérhető parancsmagok használható a runbookok és a DSC forrásanyag is elérhető a DSC-konfigurációk belüli használatra.</span><span class="sxs-lookup"><span data-stu-id="373e0-118">Optional file or not, these PowerShell modules can be imported into Azure Automation toomake their cmdlets available for use within runbooks and their DSC resources available for use within DSC configurations.</span></span> <span data-ttu-id="373e0-119">Hello háttérben az Azure Automation ezek a modulok tárolja, és a runbook-feladatok és DSC fordítási feladat végrehajtási ideje, betölti a hello Azure Automation védőfalak ahol runbookok végrehajtása és a DSC-konfigurációk fordításának őket.</span><span class="sxs-lookup"><span data-stu-id="373e0-119">Behind hello scenes, Azure Automation stores these modules, and at runbook job and DSC compilation job execution time, loads them into hello Azure Automation sandboxes where runbooks are executed and DSC configurations are compiled.</span></span>  <span data-ttu-id="373e0-120">A modulok DSC erőforrásokat is automatikusan kerül hello Automation DSC lekérési kiszolgálójával, úgy, hogy azok tooapply a DSC-konfigurációk kísérlet gépek kell húzni.</span><span class="sxs-lookup"><span data-stu-id="373e0-120">Any DSC resources in modules are also automatically placed on hello Automation DSC pull server, so that they can be pulled by machines attempting tooapply DSC configurations.</span></span>  

<span data-ttu-id="373e0-121">Azt küldje el egy Azure PowerShell-modulok száma kívül hello be az Azure Automationben meg toouse, Ismerkedés az Azure felügyeleti azonnal automatizálása, de bármilyen rendszer, a szolgáltatás vagy a kívánt toointegrate az eszköz PowerShell-modulok importálhatók.</span><span class="sxs-lookup"><span data-stu-id="373e0-121">We ship a number of Azure  PowerShell modules out of hello box in Azure Automation for you toouse so you can get started automating Azure management right away, but you can import PowerShell modules for whatever system, service, or tool you want toointegrate with.</span></span> 

> [!NOTE]
> <span data-ttu-id="373e0-122">Bizonyos modulok szállítják "globális modulok" hello Automation szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="373e0-122">Certain modules are shipped as “global modules” in hello Automation service.</span></span> <span data-ttu-id="373e0-123">A globális modulokra elérhető tooyou automation-fiók létrehozásakor, és frissítjük őket néha amely automatikusan továbbítja őket a kimenő tooyour automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="373e0-123">These global modules are available tooyou when you create an automation account, and we update them sometimes which automatically pushes them out tooyour automation account.</span></span> <span data-ttu-id="373e0-124">Ha nem szeretné, automatikus frissített, mindig importálhatja toobe hello ugyanabban a modulban saját magának, és, hogy elsőbbséget élvez a Microsoft hello szolgáltatásban szállított modult hello globális modul verziója.</span><span class="sxs-lookup"><span data-stu-id="373e0-124">If you don’t want them toobe auto-updated, you can always import hello same module yourself, and that will take precedence over hello global module version of that module that we ship in hello service.</span></span> 

<span data-ttu-id="373e0-125">hello Integrációsmodul-csomag importálni formátuma hello azonos nevet hello modul és a .zip kiterjesztésű tömörített fájlban.</span><span class="sxs-lookup"><span data-stu-id="373e0-125">hello format in which you import an Integration Module package is a compressed file with hello same name as hello module and a .zip extension.</span></span> <span data-ttu-id="373e0-126">Hello Windows PowerShell-modult és minden kiegészítő fájlt, beleértve a jegyzékfájlt (.psd1), ha hello modul rendelkezik ilyennel tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="373e0-126">It contains hello Windows PowerShell module and any supporting files, including a manifest file (.psd1) if hello module has one.</span></span>

<span data-ttu-id="373e0-127">Amennyiben hello modul tartalmaznia kell egy Azure Automation szolgáltatásbeli kapcsolattípus, hello nevű fájl is tartalmazhat `<ModuleName>-Automation.json` , amely megadja, hogy hello kapcsolattípus tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="373e0-127">If hello module should contain an Azure Automation connection type, it must also contain a file with hello name `<ModuleName>-Automation.json` that specifies hello connection type properties.</span></span> <span data-ttu-id="373e0-128">Ez a tömörített .zip fájl hello modul mappában elhelyezett json-fájlt, és hello mezőket tartalmaz egy "kapcsolat", amely szükséges tooconnect toohello rendszer vagy szolgáltatás hello modul jelöli.</span><span class="sxs-lookup"><span data-stu-id="373e0-128">This is a json file placed within hello module folder of your compressed .zip file, and contains hello fields of a “connection” that is required tooconnect toohello system or service hello module represents.</span></span> <span data-ttu-id="373e0-129">Ennek eredményeképp létrejön egy kapcsolattípus az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="373e0-129">This will end up creating a connection type in Azure Automation.</span></span> <span data-ttu-id="373e0-130">Hello mezőnevek, állíthatja be a fájlt használó meg kell adnia, és hogy hello mezőket kell titkosított és / vagy nem kötelező, hello kapcsolattípushoz hello modul.</span><span class="sxs-lookup"><span data-stu-id="373e0-130">Using this file you can set hello field names, types, and whether hello fields should be encrypted and / or optional, for hello connection type of hello module.</span></span> <span data-ttu-id="373e0-131">hello az alábbiakban olvashat egy sablon hello json formátumban:</span><span class="sxs-lookup"><span data-stu-id="373e0-131">hello following is a template in hello json file format:</span></span>

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

<span data-ttu-id="373e0-132">Ha a Szolgáltatáskezelési automatizálás telepítése és integrációs modulok csomagok létrehozni az automation-forgatókönyveket, ez tisztában tooyou kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="373e0-132">If you have deployed Service Management Automation and created Integration Modules packages for your automation runbooks, this should look very familiar tooyou.</span></span> 

## <a name="authoring-best-practices"></a><span data-ttu-id="373e0-133">Szerzői gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="373e0-133">Authoring Best Practices</span></span>
<span data-ttu-id="373e0-134">Annak ellenére, hogy integrációs modulok alapvetően a PowerShell-modulok, van-e még több minden javasoljuk, hogy egy PowerShell-modult, toomake készítése során érdemes azt az Azure Automationben legtöbb használható.</span><span class="sxs-lookup"><span data-stu-id="373e0-134">Even though Integration Modules are essentially PowerShell modules, there’s still a number of things we recommend you consider while authoring a PowerShell module, toomake it most usable in Azure Automation.</span></span> <span data-ttu-id="373e0-135">Azure Automation adott ezek némelyike, és a modulok kiválóan működjenek az PowerShell munkafolyamat hasznos csak toomake őket függetlenül Automation használ-e.</span><span class="sxs-lookup"><span data-stu-id="373e0-135">Some of these are Azure Automation specific, and some of them are useful just toomake your modules work well in PowerShell Workflow, regardless of whether or not you’re using Automation.</span></span> 

1. <span data-ttu-id="373e0-136">Például a kivonatát, leírás, és minden hello modul a parancsmag súgójában URI.</span><span class="sxs-lookup"><span data-stu-id="373e0-136">Include a synopsis, description, and help URI for every cmdlet in hello module.</span></span> <span data-ttu-id="373e0-137">PowerShell, megadhat bizonyos parancsmagok tooallow hello felhasználói tooreceive súgó vonatkozó információk a használatukat hello **Get-Help** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="373e0-137">In PowerShell, you can define certain help information for cmdlets tooallow hello user tooreceive help on using them with hello **Get-Help** cmdlet.</span></span> <span data-ttu-id="373e0-138">Az alábbi példát követve meghatározhatja a szinopszist és a súgó URI-t egy .psm1 fájlban megírt PowerShell-modulhoz.</span><span class="sxs-lookup"><span data-stu-id="373e0-138">For example, here’s how you can define a synopsis and help URI for a PowerShell module written in a .psm1 file.</span></span><br>  
   
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
   <br> <span data-ttu-id="373e0-139">Ezeket az adatokat biztosító nem csak jelennek meg a Súgó hello segítségével **Get-Help** parancsmag hello PowerShell-konzolban, azt is megmutatják Azure Automation belül a súgót.</span><span class="sxs-lookup"><span data-stu-id="373e0-139">Providing this info will not only show this help using hello **Get-Help** cmdlet in hello PowerShell console, it will also expose this help functionality within Azure Automation.</span></span>  <span data-ttu-id="373e0-140">Például amikor tevékenységet szúr be a forgatókönyvek létrehozása alatt.</span><span class="sxs-lookup"><span data-stu-id="373e0-140">For example, when inserting activities during runbook authoring.</span></span> <span data-ttu-id="373e0-141">Kattintson a "Részletes súgó megtekintése" fog nyitott hello súgó URI hello egy másik lapján böngészőben tooaccess Azure Automation használata.</span><span class="sxs-lookup"><span data-stu-id="373e0-141">Clicking “View detailed help” will open hello help URI in another tab of hello web browser you’re using tooaccess Azure Automation.</span></span><br><span data-ttu-id="373e0-142">![Integrációs modul súgó](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span><span class="sxs-lookup"><span data-stu-id="373e0-142">![Integration Module Help](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span></span>
2. <span data-ttu-id="373e0-143">Ha egy távoli rendszer fut hello modul</span><span class="sxs-lookup"><span data-stu-id="373e0-143">If hello module runs against a remote system,</span></span>

    <span data-ttu-id="373e0-144">a.</span><span class="sxs-lookup"><span data-stu-id="373e0-144">a.</span></span> <span data-ttu-id="373e0-145">Az integrációs modul metaadatait tartalmazó fájl, amely meghatározza a hello szükséges információkat tooconnect toothat távoli rendszerbe, azaz hello kapcsolattípus kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="373e0-145">It should contain an Integration Module metadata file that defines hello information needed tooconnect toothat remote system, meaning hello connection type.</span></span>  
    <span data-ttu-id="373e0-146">b.</span><span class="sxs-lookup"><span data-stu-id="373e0-146">b.</span></span> <span data-ttu-id="373e0-147">Hello modul található minden parancsmaghoz kell egy kapcsolati objektumban (egy adott kapcsolattípus példányát) képes tootake paraméterként.</span><span class="sxs-lookup"><span data-stu-id="373e0-147">Each cmdlet in hello module should be able tootake in a connection object (an instance of that connection type) as a parameter.</span></span>  

    <span data-ttu-id="373e0-148">Hello modul parancsmagjai az Azure Automationben könnyebb toouse vált, ha hello mezők hello kapcsolattípus objektum átadásakor, paraméter toohello parancsmag lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="373e0-148">Cmdlets in hello module become easier toouse in Azure Automation if you allow passing an object with hello fields of hello connection type as a parameter toohello cmdlet.</span></span> <span data-ttu-id="373e0-149">A felhasználók minden alkalommal, meghívják a parancsmag nem rendelkezik a megfelelő hello kapcsolat eszköz toohello parancsmag-paraméterek toomap paraméterek.</span><span class="sxs-lookup"><span data-stu-id="373e0-149">This way users don’t have toomap parameters of hello connection asset toohello cmdlet's corresponding parameters each time they call a cmdlet.</span></span> <span data-ttu-id="373e0-150">A fenti hello runbook példa alapján, használ egy CorpTwilio tooaccess Twilio nevű Twilio-kapcsolódási eszköz, és térjen vissza az összes hello telefonszámok hello fiók.</span><span class="sxs-lookup"><span data-stu-id="373e0-150">Based on hello runbook example above, it uses a Twilio connection asset called CorpTwilio tooaccess Twilio and return all hello phone numbers in hello account.</span></span>  <span data-ttu-id="373e0-151">Figyelje meg, hogyan hozzárendeli azt hello mezők hello kapcsolat toohello paraméterek hello parancsmag?</span><span class="sxs-lookup"><span data-stu-id="373e0-151">Notice how it is mapping hello fields of hello connection toohello parameters of hello cmdlet?</span></span><br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    <span data-ttu-id="373e0-152">Az egyszerűbb és jobb módon tooapproach ez, közvetlenül hogy hello kapcsolati objektum toohello parancsmag-</span><span class="sxs-lookup"><span data-stu-id="373e0-152">An easier and better way tooapproach this is directly passing hello connection object toohello cmdlet -</span></span>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    <span data-ttu-id="373e0-153">A parancsmagok ilyen viselkedése lehetővé teheti, közvetlenül paraméterként, paraméterek csak kapcsolat mezők helyett egy olyan kapcsolatobjektum tooaccept engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="373e0-153">You can enable behavior like this for your cmdlets by allowing them tooaccept a connection object directly as a parameter, instead of just connection fields for parameters.</span></span> <span data-ttu-id="373e0-154">Általában érdemes egy paramétert az egyes, állítsa be, hogy a felhasználó nem használja az Azure Automation a parancsmagok meghívása nélkül hozhat létre a kivonattábla tooact hello kapcsolat objektumként.</span><span class="sxs-lookup"><span data-stu-id="373e0-154">Usually you’ll want a parameter set for each, so that a user not using Azure Automation can call your cmdlets without constructing a hashtable tooact as hello connection object.</span></span> <span data-ttu-id="373e0-155">A paraméterhalmaz **SpecifyConnectionFields** használt toopass hello kapcsolat tulajdonságai egyenként alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="373e0-155">Parameter set **SpecifyConnectionFields** below is used toopass hello connection field properties one by one.</span></span> <span data-ttu-id="373e0-156">**UseConnectionObject** megadható, hogy át hello rögtön a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="373e0-156">**UseConnectionObject** lets you pass hello connection straight through.</span></span> <span data-ttu-id="373e0-157">Ahogy látja, hello hello küldési-TwilioSMS parancsmag [Twilio PowerShell modul](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) lehetővé teszi, hogy mindkét módszer sikeres:</span><span class="sxs-lookup"><span data-stu-id="373e0-157">As you can see, hello Send-TwilioSMS cmdlet in hello [Twilio PowerShell module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) allows passing either way:</span></span> 
   
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
3. <span data-ttu-id="373e0-158">Adja meg az összes parancsmag kimeneti típusát hello modulban.</span><span class="sxs-lookup"><span data-stu-id="373e0-158">Define output type for all cmdlets in hello module.</span></span> <span data-ttu-id="373e0-159">Kimeneti típusát meghatározó, a parancsmag lehetővé teszi, hogy a tervezési idejű IntelliSense meghatározhatja, hogy hello toohelp kimeneti hello parancsmag használható létrehozási tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="373e0-159">Defining an output type for a cmdlet allows design-time IntelliSense toohelp you determine hello output properties of hello cmdlet, for use during authoring.</span></span> <span data-ttu-id="373e0-160">Ez akkor hasznos automatizálási runbook grafikus létrehozási, ahol tervezési idő Tudásbázis a kulcs tooan könnyen nyújtotta felhasználói élmény, a modul.</span><span class="sxs-lookup"><span data-stu-id="373e0-160">It is especially helpful during Automation runbook graphical authoring, where design time knowledge is key tooan easy user experience with your module.</span></span><br><br> <span data-ttu-id="373e0-161">![Grafikus runbook kimenettípusa](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span><span class="sxs-lookup"><span data-stu-id="373e0-161">![Graphical Runbook Output Type](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span></span><br> <span data-ttu-id="373e0-162">Ez a hasonló toohello "írja be azokat, amelyek" funkcióját parancsmag kimenetét a PowerShell ISE anélkül, hogy toorun azt.</span><span class="sxs-lookup"><span data-stu-id="373e0-162">This is similar toohello "type ahead" functionality of a cmdlet's output in PowerShell ISE without having toorun it.</span></span><br><br> <span data-ttu-id="373e0-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span><span class="sxs-lookup"><span data-stu-id="373e0-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span></span><br>
4. <span data-ttu-id="373e0-164">Hello modul parancsmagjai sem tarthat összetett objektumtípusok paraméterekhez.</span><span class="sxs-lookup"><span data-stu-id="373e0-164">Cmdlets in hello module should not take complex object types for parameters.</span></span> <span data-ttu-id="373e0-165">A PowerShell-munkafolyamat eltér a PowerShelltől abban, hogy komplex típusokat tárol deszerializált formában.</span><span class="sxs-lookup"><span data-stu-id="373e0-165">PowerShell Workflow is different from PowerShell in that it stores complex types in deserialized form.</span></span> <span data-ttu-id="373e0-166">Az egyszerű típusok egyszerű maradnak, de összetett típusok deszerializálni konvertált tootheir verziókat, amelyek alapvetően tulajdonságcsomagok.</span><span class="sxs-lookup"><span data-stu-id="373e0-166">Primitive types will stay as primitives, but complex types are converted tootheir deserialized versions, which are essentially property bags.</span></span> <span data-ttu-id="373e0-167">Például, ha a használt hello **Get-Process** parancsmag egy runbook (vagy egy adott függetlenül attól, hogy a PowerShell-munkafolyamat), azt kellene visszaadnia [Deserialized.System.Diagnostic.Process] típusú objektum, nem a várt [hello System.Diagnostic.Process] típusú.</span><span class="sxs-lookup"><span data-stu-id="373e0-167">For example, if you used hello **Get-Process** cmdlet in a runbook (or a PowerShell Workflow for that matter), it would return an object of type [Deserialized.System.Diagnostic.Process], not hello expected [System.Diagnostic.Process] type.</span></span> <span data-ttu-id="373e0-168">Ez a típus rendelkezik az összes hello ugyanazok a tulajdonságok szerint hello nem deszerializálni típusú, de hello metódusok egyike.</span><span class="sxs-lookup"><span data-stu-id="373e0-168">This type has all hello same properties as hello non-deserialized type, but none of hello methods.</span></span> <span data-ttu-id="373e0-169">Ha megpróbál toopass ezt az értéket a paraméter tooa parancsmag, ahol hello a parancsmagnak a paraméter [System.Diagnostic.Process] értékét, mint kapni fog a következő hiba hello és: *argumentum átalakítása "folyamat" paraméter nem lehet feldolgozni. . Hiba: "hello"System.Diagnostics.Process (CcmExec)"érték"Deserialized.System.Diagnostics.Process"tootype"System.Diagnostics.Process"típus nem konvertálható.*</span><span class="sxs-lookup"><span data-stu-id="373e0-169">And if you try toopass this value as a parameter tooa cmdlet, where hello cmdlet expects a [System.Diagnostic.Process] value for this parameter, you’ll receive hello following error: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert hello "System.Diagnostics.Process (CcmExec)" value of type  "Deserialized.System.Diagnostics.Process" tootype "System.Diagnostics.Process".*</span></span>   <span data-ttu-id="373e0-170">Ez azért, mert a hello közötti Típuseltérés várt [System.Diagnostic.Process] típusa és hello adott [Deserialized.System.Diagnostic.Process] típusú.</span><span class="sxs-lookup"><span data-stu-id="373e0-170">This is because there is a type mismatch between hello expected [System.Diagnostic.Process] type and hello given [Deserialized.System.Diagnostic.Process] type.</span></span> <span data-ttu-id="373e0-171">hello a probléma megoldásához módja tooensure hello parancsmagok a modul nem hajtja végre a paramétereket összetett típusok.</span><span class="sxs-lookup"><span data-stu-id="373e0-171">hello way around this issue is tooensure hello cmdlets of your module do not take complex types for parameters.</span></span> <span data-ttu-id="373e0-172">Íme hello helytelen módon toodo azt.</span><span class="sxs-lookup"><span data-stu-id="373e0-172">Here is hello wrong way toodo it.</span></span>
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    <span data-ttu-id="373e0-173">És itt hello megfelelő módon, egy primitív hello parancsmag által belsőleg használható véve toograb hello összetett objektumot, és használni.</span><span class="sxs-lookup"><span data-stu-id="373e0-173">And here is hello right way, taking in a primitive that can be used internally by hello cmdlet toograb hello complex object and use it.</span></span> <span data-ttu-id="373e0-174">Parancsmagok PowerShell hello környezetében hajtható végre, mert a PowerShell munkafolyamat-nem, belül hello parancsmag $process válik hello megfelelő [System.Diagnostic.Process] típusú.</span><span class="sxs-lookup"><span data-stu-id="373e0-174">Since cmdlets execute in hello context of PowerShell, not PowerShell Workflow, inside hello cmdlet $process becomes hello correct [System.Diagnostic.Process] type.</span></span>  
   
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
   <span data-ttu-id="373e0-175">Kapcsolati objektumok runbookok szórótáblában, amelyek összetett típus, és úgy tűnik, még ezen szórótáblában toobe képes toobe parancsmagjai lett átadva a – kapcsolati paraméter nem konvertálható kivétellel tökéletesen,.</span><span class="sxs-lookup"><span data-stu-id="373e0-175">Connection assets in runbooks are hashtables, which are a complex type, and yet these hashtables seem toobe able toobe passed into cmdlets for their –Connection parameter perfectly, with no cast exception.</span></span> <span data-ttu-id="373e0-176">Technikai szempontból néhány PowerShell típust megfelelően szerializált űrlap deszerializálni tootheir formájukban a tud toocast, és ezért adhatók át a parancsmagok elfogadása hello nem deszerializálni típusú paraméter.</span><span class="sxs-lookup"><span data-stu-id="373e0-176">Technically, some PowerShell types are able toocast properly from their serialized form tootheir deserialized form, and hence can be passed into cmdlets for parameters accepting hello non-deserialized type.</span></span> <span data-ttu-id="373e0-177">A kivonattábla egy ezek közül.</span><span class="sxs-lookup"><span data-stu-id="373e0-177">Hashtable is one of these.</span></span> <span data-ttu-id="373e0-178">Lehetséges, hogy egy modul Szerző definiált típusok toobe oly módon, hogy azok is megfelelően deszerializálása is implementálva, de néhány kompromisszumot tooconsider.</span><span class="sxs-lookup"><span data-stu-id="373e0-178">It’s possible for a module author’s defined types toobe implemented in a way that they can correctly deserialize as well, but there are some trade-offs tooconsider.</span></span> <span data-ttu-id="373e0-179">hello típus igények toohave egy alapértelmezett konstruktorral, az összes a Tulajdonságok nyilvános és egy PSTypeConverter rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="373e0-179">hello type needs toohave a default constructor, have all of its properties public, and have a PSTypeConverter.</span></span> <span data-ttu-id="373e0-180">Azonban a már definiált típusok adott hello modul szerzője nem saját nem túl "javítás" őket, ezért a javaslat tooavoid összetett típusok paraméterek teljes hello van.</span><span class="sxs-lookup"><span data-stu-id="373e0-180">However, for already-defined types that hello module author does not own, there is no way too“fix” them, hence hello recommendation tooavoid complex types for parameters all together.</span></span> <span data-ttu-id="373e0-181">Runbook szerzői tipp: bizonyos okból a parancsmagok tootake összetett írja be kell paraméter, vagy használja valaki más modul, amely egy összetett típusú paraméter, a PowerShell-munkafolyamati forgatókönyvek hello megoldás és a PowerShell-munkafolyamatok helyi van szükség PowerShell, toowrap hello parancsmag által generált hello komplex típus és hello parancsmag, amely akkor hello összetett típus hello az InlineScript tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="373e0-181">Runbook Authoring tip: If for some reason your cmdlets need tootake a complex type parameter, or you are using someone else’s module that requires a complex type parameter, hello workaround in PowerShell Workflow runbooks and PowerShell Workflows in local PowerShell, is toowrap hello cmdlet that generates hello complex type and hello cmdlet that consumes hello complex type in hello same InlineScript activity.</span></span> <span data-ttu-id="373e0-182">InlineScript tartalma helyett a PowerShell-munkafolyamat létrehozásakor hello összetett típus hello parancsmag eredményezne, hogy megfelelő típusú PowerShell végre, mert nem hello deszerializálni összetett típus.</span><span class="sxs-lookup"><span data-stu-id="373e0-182">Since InlineScript executes its contents as PowerShell rather than PowerShell Workflow, hello cmdlet generating hello complex type would produce that correct type, not hello deserialized complex type.</span></span>
5. <span data-ttu-id="373e0-183">Végezze el az összes parancsmag hello modul állapot nélküli.</span><span class="sxs-lookup"><span data-stu-id="373e0-183">Make all cmdlets in hello module stateless.</span></span> <span data-ttu-id="373e0-184">PowerShell-munkafolyamat minden hello munkafolyamat egy másik munkamenetben meghívta parancsmagot futtatja.</span><span class="sxs-lookup"><span data-stu-id="373e0-184">PowerShell Workflow runs every cmdlet called in hello workflow in a different session.</span></span> <span data-ttu-id="373e0-185">Ez azt jelenti, hogy bármely parancsmag-készlettel létrehozva / más parancsmagok a ugyanabban a modulban nem fog működni a PowerShell-munkafolyamati forgatókönyvek hello módosította a munkamenet-állapot függ.</span><span class="sxs-lookup"><span data-stu-id="373e0-185">This means any cmdlets that depend on session state created / modified by other cmdlets in hello same module will not work in PowerShell Workflow runbooks.</span></span>  <span data-ttu-id="373e0-186">Íme egy példa, mit nem toodo.</span><span class="sxs-lookup"><span data-stu-id="373e0-186">Here is an example of what not toodo.</span></span>
   
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
6. <span data-ttu-id="373e0-187">hello modul teljesen tartalmazza, az Xcopy képes csomagot.</span><span class="sxs-lookup"><span data-stu-id="373e0-187">hello module should be fully contained in an Xcopy-able package.</span></span> <span data-ttu-id="373e0-188">Mivel az Azure Automation-modul elosztott toohello Automation védőfalak amikor runbookokat kell tooexecute, toowork függetlenül hello gazdagépen futnak a szükséges.</span><span class="sxs-lookup"><span data-stu-id="373e0-188">Because Azure Automation modules are distributed toohello Automation sandboxes when runbooks need tooexecute, they need toowork independently of hello host they are running on.</span></span> <span data-ttu-id="373e0-189">Ez azt jelenti, hogy meg kell elő hello modul csomagot tud tooZip, tooany helyezze, a másik állomást hello ugyanazon vagy egy újabb PowerShell verziót, és rendelkezik az adott gazdagépen PowerShell környezetre való importálásakor normál működéséhez.</span><span class="sxs-lookup"><span data-stu-id="373e0-189">What this means is that you should be able tooZip up hello module package, move it tooany other host with hello same or newer PowerShell version, and have it function as normal when imported into that host’s PowerShell environment.</span></span> <span data-ttu-id="373e0-190">Ahhoz, hogy adott toohappen hello modul kell nem függ semmilyen fájl hello (hello mappa, amely lekérdezi zip be, amikor importálja az Azure Automation) modul mappájában, vagy bármely olyan gazdagépen egyedi beállításjegyzék-beállítások, például hello által beállított telepítse a termék.</span><span class="sxs-lookup"><span data-stu-id="373e0-190">In order for that toohappen, hello module should not depend on any files outside hello module folder (hello folder that gets zipped up when importing into Azure Automation), or on any unique registry settings on a host, such as those set by hello install of a product.</span></span> <span data-ttu-id="373e0-191">Ha ez az ajánlott eljárás nem követik, hello modul nem lesz használható az Azure Automationben.</span><span class="sxs-lookup"><span data-stu-id="373e0-191">If this best practice is not followed, hello module will not be usable in Azure Automation.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="373e0-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="373e0-192">Next steps</span></span>

* <span data-ttu-id="373e0-193">a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="373e0-193">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="373e0-194">toolearn PowerShell modulokhoz, több létrehozásáról lásd: [Windows PowerShell-modul írása](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="373e0-194">toolearn more about creating PowerShell Modules, see [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span></span>

