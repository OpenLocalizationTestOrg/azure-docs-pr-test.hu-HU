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
# <a name="variable-assets-in-azure-automation"></a>Az Azure Automationben változó eszközök

Változó eszközök értékeket elérhető tooall runbookok és a DSC-konfigurációk az automation-fiókban. Azok létrehozott, módosított, és lekérése hello Azure-portálon, a Windows PowerShell és a runbookot vagy a DSC-konfiguráció. A következő forgatókönyvek hello automatizálási változók hasznosak:

- Ossza meg több runbookok vagy a DSC-konfigurációk közötti értéket.

- Egy érték elérhetővé tétele több feladat hello ugyanaz a runbook vagy DSC-konfiguráció.

- Hello portálon vagy a hello Windows PowerShell parancssorból forgatókönyvek vagy a DSC-konfigurációk, például a virtuális gép nevét, egy adott erőforráscsoportot, egy AD-tartomány nevét, stb például adott listáját általános konfigurációs elemek készlete által használt érték kezelése.  

Automatizálási változók megmaradnak, így akkor is elérhető toobe, még akkor is, ha hello runbookot vagy a DSC-konfiguráció nem sikerül.  Ezenkívül lehetővé teszi egy érték toobe állítja be, amely ezután történik egy másik, vagy hello használja egy runbook ugyanaz a runbook vagy DSC konfigurációs hello, amikor legközelebb futtatja azt.

Egy változó létrehozásakor megadhatja, hogy legyen tárolva titkosított.  Ha titkosított változó, az Azure Automation tárolja biztonságos helyen és az értéke nem lehet beolvasni hello [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx) parancsmagot tartalmaz, hello Azure PowerShell modul részét képezi.  hello csak titkosított érték lekérhető, a hello **Get-AutomationVariable** tevékenység egy runbookot vagy a DSC-konfiguráció.

> [!NOTE]
> Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók. Ezek az eszközök titkosítottak és a tárolt hello Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz. Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja. Tárolása biztonságos eszköz, mielőtt hello kulcs hello automation-fiók visszafejtése hello fő tanúsítványt használ, és akkor használja a tooencrypt hello eszköz.

## <a name="variable-types"></a>Változó típusa

Hello Azure-portál a változó létrehozásakor meg kell adni egy adatok hello legördülő listából, hello portal megjelenítheti hello megfelelő vezérlő beírásához hello változó értékét. hello változó nem korlátozott toothis adatok típusa, de értékre kell állítani a Windows PowerShell használatával, ha azt szeretné, hogy toospecify hello változó egy eltérő típusú. Ha megad **nincs definiálva**, majd hello hello változó értéke lesz túl**$null**, és meg kell adni a hello hello értékének [Set-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913767.aspx) parancsmag vagy **Set-AutomationVariable** tevékenység.  Nem hozható létre, vagy módosítsa hello hello portálon változó komplex típusok esetében, de megadhatja a Windows PowerShell használatával bármilyen típusú érték. Összetett típusok változatlanul adódik vissza egy [PSCustomObject](http://msdn.microsoft.com/library/system.management.automation.pscustomobject.aspx).

Több értékek tooa egy változó létrehozása a tömb vagy hibás, és mentse toohello változó tárolhatja.

Az alábbiakban hello változó Típuslista Automation érhető el:

* Karakterlánc
* Egész szám
* Dátum és idő
* Logikai érték
* NULL értékű

## <a name="cmdlets-and-workflow-activities"></a>Parancsmagok és a munkafolyamat-tevékenységek

hello parancsmagok a következő táblázat hello használt toocreate és kezelése Windows PowerShell-automatizálási változók. Ezek hello részét képezi [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfiguráció.

|Parancsmagok|Leírás|
|:---|:---|
|[Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx)|Lekéri a hello egy létező változó értékét.|
|[Új AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx)|Új változót hoz létre, és beállítja az értékét.|
|[Remove-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt619354.aspx)|Eltávolít egy létező változó.|
|[Set-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603601.aspx)|Beállítja egy létező változó értékét hello.|

a következő táblázat hello hello munkafolyamat tevékenységei használt tooaccess egy runbook automatizálási változók. Ezek csak akkor használ, a runbookot vagy a DSC-konfiguráció, és nem hello Azure PowerShell-modulja részét képezi.

|Munkafolyamat-tevékenységek|Leírás|
|:---|:---|
|Get-AutomationVariable|Lekéri a hello egy létező változó értékét.|
|Set-AutomationVariable|Beállítja egy létező változó értékét hello.|

> [!NOTE] 
> Kerülendő a változók használata hello – Name paraméterében **Get-AutomationVariable** a runbookot vagy a DSC-konfiguráció számára, mivel ez megnehezítheti a runbookok vagy DSC-konfiguráció és automatizálás közti függőségek változók tervezési időben.

## <a name="creating-a-new-automation-variable"></a>Új automatizálási változó létrehozása

### <a name="toocreate-a-new-variable-with-hello-azure-portal"></a>toocreate egy új változót a hello Azure-portálon

1. Kattintson az Automation-fiók hello **eszközök** csempe majd a hello **eszközök** panelen válassza **változók**.
2. A hello **változók** csempe, jelölje be **változó hozzáadása**.
3. Fejezze be a hello hello-beállítások **új változó** panel megnyitásához, és kattintson **létrehozása** hello új változó mentse.

### <a name="toocreate-a-new-variable-with-windows-powershell"></a>toocreate egy új változót a Windows PowerShell használatával

Hello [New-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603613.aspx) parancsmag új változót hoz létre, és beállítja a kezdeti értékhez. Kérheti le hello érték [Get-AzureRmAutomationVariable](https://msdn.microsoft.com/library/mt603849.aspx). Ha hello érték egyszerű típus, majd, hogy ugyanolyan típusú adja vissza. Ha egy összetett típus, akkor egy **PSCustomObject** adja vissza.

hello következő minta parancsok megjelenítése hogyan toocreate típusú változó karakterláncot, és térjen vissza az értékét.

    New-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

hello következő minta parancsok megjelenítése hogyan toocreate egy összetett változó, írja be, és térjen vissza a tulajdonságait. Ebben az esetben a virtuális gépek objektum **Get-AzureRmVm** szolgál.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Egy változó a runbookot vagy a DSC-konfiguráció használata

Használjon hello **Set-AutomationVariable** tevékenység tooset hello értékének egy automatizálási változó a runbookot vagy a DSC-konfiguráció és a hello **Get-AutomationVariable** tooretrieve azt.  Hello ne használja **Set-AzureAutomationVariable** vagy **Get-AzureAutomationVariable** parancsmagok a runbookot vagy a DSC-konfiguráció számára, mert azok hello munkafolyamat-tevékenységek-nél kevésbé hatékonyak.  Még nem lehet lekérdezni a biztonságos változók értékének hello **Get-AzureAutomationVariable**.  csak úgy toocreate egy új változót egy runbookon belüli hello vagy a DSC-konfiguráció toouse hello [New-AzureAutomationVariable](http://msdn.microsoft.com/library/dn913771.aspx) parancsmag.


### <a name="textual-runbook-samples"></a>Szöveges forgatókönyvként minták

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>És egy egyszerű érték a változóból beolvasása

hello következő minta parancsok megjelenítése hogyan tooset és a szöveges forgatókönyvként változó beolvasása. A példában feltételezzük, hogy az egész szám típusú nevű *NumberOfIterations* és *NumberOfRunnings* és nevű, karakterlánc típusú változó *példában* már létre van hozva.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>És egy összetett objektumot egy változóban beolvasása

hello következő minta kód bemutatja, hogyan tooupdate szöveges forgatókönyvként összetett érték változó. Ez a példa egy Azure virtuális gépen a beolvasott **Get-AzureVM** és mentett tooan meglévő automatizálási változó.  A [változó típusok](#variable-types), ez egy PSCustomObject tárolja.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm


A következő kód hello hello érték beolvasva hello változó és használt toostart hello virtuális gépet.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>És egy gyűjtemény változóként beolvasása

hello következő minta kód bemutatja, hogyan toouse egy változó, szöveges forgatókönyvként összetett értékek gyűjteménye. Ez a példa több Azure virtuális gépeken a rendszer beolvassa **Get-AzureVM** és mentett tooan meglévő automatizálási változó.  A [változó típusok](#variable-types), ez PSCustomObjects gyűjteménye tárolja.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

A következő kód hello a hello gyűjtemény hello változó lekért és a használt toostart minden virtuális gép.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }


### <a name="graphical-runbook-samples"></a>Grafikus forgatókönyv minták

A grafikus runbookokban hello hozzáadása **Get-AutomationVariable** vagy **Set-AutomationVariable** hello változó hello grafikus szerkesztő hello könyvtár ablaktáblán a jobb gombbal, és hello kiválasztása kívánt tevékenységet.

![Változó toocanvas hozzáadása](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>A beállítás értéke egy változó
hello következő kép bemutatja minta tevékenységek tooupdate egy változó, egy egyszerű érték egy grafikus runbook. Ez a példa egy Azure virtuális gép a beolvasott **Get-AzureRmVM** és hello számítógépnév tooan meglévő automatizálási változó mentett karakterlánc típusú.  Nem számít, hello [egy folyamat vagy feladatütemezési](automation-graphical-authoring-intro.md#links-and-workflow) óta hello kimenet csak várhatóan egy adott objektum.

![Egyszerű változó beállítása](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Következő lépések

* További információ az tevékenységek összekapcsolása a grafikus szerzői toolearn lásd: [grafikus szerzői hivatkozások](automation-graphical-authoring-intro.md#links-and-workflow)
* Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md) 

