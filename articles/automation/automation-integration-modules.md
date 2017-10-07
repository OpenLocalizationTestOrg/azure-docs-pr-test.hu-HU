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
# <a name="azure-automation-integration-modules"></a>Azure Automation integrációs modulok
PowerShell hello technológia Azure Automation mögött. Azure Automation PowerShell épül, mivel a PowerShell-modulok kulcs toohello bővítési az Azure Automation. Ebben a cikkben azt végigvezeti Azure Automation PowerShell-modulok használata hello mintaadatokról tooas "Integrációs modulok" említett és gyakorlati tanácsok a saját PowerShell modulok toomake meg arról, hogy működnek-integrációs modulok belül létrehozásához Azure Automation szolgáltatásbeli. 

## <a name="what-is-a-powershell-module"></a>Mi az a PowerShell-modul?
Egy PowerShell-modul olyan PowerShell-parancsmagokat például **Get-Date** vagy **elem**, amely használható a hello PowerShell-konzolban, a parancsfájlok, a munkafolyamatok, a runbookokat, és a PowerShell DSC erőforrások – például WindowsFeature vagy a fájl, a PowerShell DSC-konfigurációk használható. Összes hello funkció PowerShell parancsmagok és a DSC-erőforrások ki van téve, és minden parancsmag/DSC erőforrás egy PowerShell-modul által támogatott sok, amely küldje el a PowerShell használatával saját magát. Például hello **Get-Date** parancsmag hello Microsoft.PowerShell.Utility PowerShell modulja részét képezi, és **elem** parancsmag hello Microsoft.PowerShell.Management PowerShell-modulja részét képezi, és hello csomag DSC erőforrás hello PSDesiredStateConfiguration PowerShell-modulja részét képezi. A PowerShellben mindkét modul megtalálható. Azonban számos PowerShell-modult nem PowerShell részét képezi, és ehelyett eszközzel együtt első vagy a külső termékek, például a System Center 2012 Configuration Manager vagy hello túlnyomó PowerShell közösségi vagy hasonló PowerShell-galériában a.  hello modulok hasznosak, mert azok összetett feladatok egyszerűbbé teszik beágyazott funkció segítségével.  További információt talál a [PowerShell-modulokról az MSDN webhelyén](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Mi az az Azure Automation integrációs modul?
Az integrációs modul nem különbözik sokban a többi PowerShell-modultól. A egyszerűen egy PowerShell-modul egy további fájl - egy metaadat-fájlt, amely megadja egy Azure Automation szolgáltatásbeli kapcsolódási típus toobe használt runbookokat hello modul parancsmagjai opcionálisan tartalmazó. Nem kötelező a fájl vagy nem, a PowerShell-modulok importálhatók az Azure Automation toomake az elérhető parancsmagok használható a runbookok és a DSC forrásanyag is elérhető a DSC-konfigurációk belüli használatra. Hello háttérben az Azure Automation ezek a modulok tárolja, és a runbook-feladatok és DSC fordítási feladat végrehajtási ideje, betölti a hello Azure Automation védőfalak ahol runbookok végrehajtása és a DSC-konfigurációk fordításának őket.  A modulok DSC erőforrásokat is automatikusan kerül hello Automation DSC lekérési kiszolgálójával, úgy, hogy azok tooapply a DSC-konfigurációk kísérlet gépek kell húzni.  

Azt küldje el egy Azure PowerShell-modulok száma kívül hello be az Azure Automationben meg toouse, Ismerkedés az Azure felügyeleti azonnal automatizálása, de bármilyen rendszer, a szolgáltatás vagy a kívánt toointegrate az eszköz PowerShell-modulok importálhatók. 

> [!NOTE]
> Bizonyos modulok szállítják "globális modulok" hello Automation szolgáltatásban. A globális modulokra elérhető tooyou automation-fiók létrehozásakor, és frissítjük őket néha amely automatikusan továbbítja őket a kimenő tooyour automation-fiók. Ha nem szeretné, automatikus frissített, mindig importálhatja toobe hello ugyanabban a modulban saját magának, és, hogy elsőbbséget élvez a Microsoft hello szolgáltatásban szállított modult hello globális modul verziója. 

hello Integrációsmodul-csomag importálni formátuma hello azonos nevet hello modul és a .zip kiterjesztésű tömörített fájlban. Hello Windows PowerShell-modult és minden kiegészítő fájlt, beleértve a jegyzékfájlt (.psd1), ha hello modul rendelkezik ilyennel tartalmaz.

Amennyiben hello modul tartalmaznia kell egy Azure Automation szolgáltatásbeli kapcsolattípus, hello nevű fájl is tartalmazhat `<ModuleName>-Automation.json` , amely megadja, hogy hello kapcsolattípus tulajdonságait. Ez a tömörített .zip fájl hello modul mappában elhelyezett json-fájlt, és hello mezőket tartalmaz egy "kapcsolat", amely szükséges tooconnect toohello rendszer vagy szolgáltatás hello modul jelöli. Ennek eredményeképp létrejön egy kapcsolattípus az Azure Automationben. Hello mezőnevek, állíthatja be a fájlt használó meg kell adnia, és hogy hello mezőket kell titkosított és / vagy nem kötelező, hello kapcsolattípushoz hello modul. hello az alábbiakban olvashat egy sablon hello json formátumban:

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

Ha a Szolgáltatáskezelési automatizálás telepítése és integrációs modulok csomagok létrehozni az automation-forgatókönyveket, ez tisztában tooyou kell kinéznie. 

## <a name="authoring-best-practices"></a>Szerzői gyakorlati tanácsok
Annak ellenére, hogy integrációs modulok alapvetően a PowerShell-modulok, van-e még több minden javasoljuk, hogy egy PowerShell-modult, toomake készítése során érdemes azt az Azure Automationben legtöbb használható. Azure Automation adott ezek némelyike, és a modulok kiválóan működjenek az PowerShell munkafolyamat hasznos csak toomake őket függetlenül Automation használ-e. 

1. Például a kivonatát, leírás, és minden hello modul a parancsmag súgójában URI. PowerShell, megadhat bizonyos parancsmagok tooallow hello felhasználói tooreceive súgó vonatkozó információk a használatukat hello **Get-Help** parancsmag. Az alábbi példát követve meghatározhatja a szinopszist és a súgó URI-t egy .psm1 fájlban megírt PowerShell-modulhoz.<br>  
   
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
   <br> Ezeket az adatokat biztosító nem csak jelennek meg a Súgó hello segítségével **Get-Help** parancsmag hello PowerShell-konzolban, azt is megmutatják Azure Automation belül a súgót.  Például amikor tevékenységet szúr be a forgatókönyvek létrehozása alatt. Kattintson a "Részletes súgó megtekintése" fog nyitott hello súgó URI hello egy másik lapján böngészőben tooaccess Azure Automation használata.<br>![Integrációs modul súgó](media/automation-integration-modules/automation-integration-module-activitydesc.png)
2. Ha egy távoli rendszer fut hello modul

    a. Az integrációs modul metaadatait tartalmazó fájl, amely meghatározza a hello szükséges információkat tooconnect toothat távoli rendszerbe, azaz hello kapcsolattípus kell tartalmaznia.  
    b. Hello modul található minden parancsmaghoz kell egy kapcsolati objektumban (egy adott kapcsolattípus példányát) képes tootake paraméterként.  

    Hello modul parancsmagjai az Azure Automationben könnyebb toouse vált, ha hello mezők hello kapcsolattípus objektum átadásakor, paraméter toohello parancsmag lehetővé teszi. A felhasználók minden alkalommal, meghívják a parancsmag nem rendelkezik a megfelelő hello kapcsolat eszköz toohello parancsmag-paraméterek toomap paraméterek. A fenti hello runbook példa alapján, használ egy CorpTwilio tooaccess Twilio nevű Twilio-kapcsolódási eszköz, és térjen vissza az összes hello telefonszámok hello fiók.  Figyelje meg, hogyan hozzárendeli azt hello mezők hello kapcsolat toohello paraméterek hello parancsmag?<br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Az egyszerűbb és jobb módon tooapproach ez, közvetlenül hogy hello kapcsolati objektum toohello parancsmag-
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    A parancsmagok ilyen viselkedése lehetővé teheti, közvetlenül paraméterként, paraméterek csak kapcsolat mezők helyett egy olyan kapcsolatobjektum tooaccept engedélyezheti. Általában érdemes egy paramétert az egyes, állítsa be, hogy a felhasználó nem használja az Azure Automation a parancsmagok meghívása nélkül hozhat létre a kivonattábla tooact hello kapcsolat objektumként. A paraméterhalmaz **SpecifyConnectionFields** használt toopass hello kapcsolat tulajdonságai egyenként alábbiakban található. **UseConnectionObject** megadható, hogy át hello rögtön a kapcsolat. Ahogy látja, hello hello küldési-TwilioSMS parancsmag [Twilio PowerShell modul](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) lehetővé teszi, hogy mindkét módszer sikeres: 
   
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
3. Adja meg az összes parancsmag kimeneti típusát hello modulban. Kimeneti típusát meghatározó, a parancsmag lehetővé teszi, hogy a tervezési idejű IntelliSense meghatározhatja, hogy hello toohelp kimeneti hello parancsmag használható létrehozási tulajdonságai. Ez akkor hasznos automatizálási runbook grafikus létrehozási, ahol tervezési idő Tudásbázis a kulcs tooan könnyen nyújtotta felhasználói élmény, a modul.<br><br> ![Grafikus runbook kimenettípusa](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Ez a hasonló toohello "írja be azokat, amelyek" funkcióját parancsmag kimenetét a PowerShell ISE anélkül, hogy toorun azt.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
4. Hello modul parancsmagjai sem tarthat összetett objektumtípusok paraméterekhez. A PowerShell-munkafolyamat eltér a PowerShelltől abban, hogy komplex típusokat tárol deszerializált formában. Az egyszerű típusok egyszerű maradnak, de összetett típusok deszerializálni konvertált tootheir verziókat, amelyek alapvetően tulajdonságcsomagok. Például, ha a használt hello **Get-Process** parancsmag egy runbook (vagy egy adott függetlenül attól, hogy a PowerShell-munkafolyamat), azt kellene visszaadnia [Deserialized.System.Diagnostic.Process] típusú objektum, nem a várt [hello System.Diagnostic.Process] típusú. Ez a típus rendelkezik az összes hello ugyanazok a tulajdonságok szerint hello nem deszerializálni típusú, de hello metódusok egyike. Ha megpróbál toopass ezt az értéket a paraméter tooa parancsmag, ahol hello a parancsmagnak a paraméter [System.Diagnostic.Process] értékét, mint kapni fog a következő hiba hello és: *argumentum átalakítása "folyamat" paraméter nem lehet feldolgozni. . Hiba: "hello"System.Diagnostics.Process (CcmExec)"érték"Deserialized.System.Diagnostics.Process"tootype"System.Diagnostics.Process"típus nem konvertálható.*   Ez azért, mert a hello közötti Típuseltérés várt [System.Diagnostic.Process] típusa és hello adott [Deserialized.System.Diagnostic.Process] típusú. hello a probléma megoldásához módja tooensure hello parancsmagok a modul nem hajtja végre a paramétereket összetett típusok. Íme hello helytelen módon toodo azt.
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    És itt hello megfelelő módon, egy primitív hello parancsmag által belsőleg használható véve toograb hello összetett objektumot, és használni. Parancsmagok PowerShell hello környezetében hajtható végre, mert a PowerShell munkafolyamat-nem, belül hello parancsmag $process válik hello megfelelő [System.Diagnostic.Process] típusú.  
   
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
   Kapcsolati objektumok runbookok szórótáblában, amelyek összetett típus, és úgy tűnik, még ezen szórótáblában toobe képes toobe parancsmagjai lett átadva a – kapcsolati paraméter nem konvertálható kivétellel tökéletesen,. Technikai szempontból néhány PowerShell típust megfelelően szerializált űrlap deszerializálni tootheir formájukban a tud toocast, és ezért adhatók át a parancsmagok elfogadása hello nem deszerializálni típusú paraméter. A kivonattábla egy ezek közül. Lehetséges, hogy egy modul Szerző definiált típusok toobe oly módon, hogy azok is megfelelően deszerializálása is implementálva, de néhány kompromisszumot tooconsider. hello típus igények toohave egy alapértelmezett konstruktorral, az összes a Tulajdonságok nyilvános és egy PSTypeConverter rendelkezik. Azonban a már definiált típusok adott hello modul szerzője nem saját nem túl "javítás" őket, ezért a javaslat tooavoid összetett típusok paraméterek teljes hello van. Runbook szerzői tipp: bizonyos okból a parancsmagok tootake összetett írja be kell paraméter, vagy használja valaki más modul, amely egy összetett típusú paraméter, a PowerShell-munkafolyamati forgatókönyvek hello megoldás és a PowerShell-munkafolyamatok helyi van szükség PowerShell, toowrap hello parancsmag által generált hello komplex típus és hello parancsmag, amely akkor hello összetett típus hello az InlineScript tevékenységet. InlineScript tartalma helyett a PowerShell-munkafolyamat létrehozásakor hello összetett típus hello parancsmag eredményezne, hogy megfelelő típusú PowerShell végre, mert nem hello deszerializálni összetett típus.
5. Végezze el az összes parancsmag hello modul állapot nélküli. PowerShell-munkafolyamat minden hello munkafolyamat egy másik munkamenetben meghívta parancsmagot futtatja. Ez azt jelenti, hogy bármely parancsmag-készlettel létrehozva / más parancsmagok a ugyanabban a modulban nem fog működni a PowerShell-munkafolyamati forgatókönyvek hello módosította a munkamenet-állapot függ.  Íme egy példa, mit nem toodo.
   
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
6. hello modul teljesen tartalmazza, az Xcopy képes csomagot. Mivel az Azure Automation-modul elosztott toohello Automation védőfalak amikor runbookokat kell tooexecute, toowork függetlenül hello gazdagépen futnak a szükséges. Ez azt jelenti, hogy meg kell elő hello modul csomagot tud tooZip, tooany helyezze, a másik állomást hello ugyanazon vagy egy újabb PowerShell verziót, és rendelkezik az adott gazdagépen PowerShell környezetre való importálásakor normál működéséhez. Ahhoz, hogy adott toohappen hello modul kell nem függ semmilyen fájl hello (hello mappa, amely lekérdezi zip be, amikor importálja az Azure Automation) modul mappájában, vagy bármely olyan gazdagépen egyedi beállításjegyzék-beállítások, például hello által beállított telepítse a termék. Ha ez az ajánlott eljárás nem követik, hello modul nem lesz használható az Azure Automationben.  

## <a name="next-steps"></a>Következő lépések

* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)
* toolearn PowerShell modulokhoz, több létrehozásáról lásd: [Windows PowerShell-modul írása](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)

