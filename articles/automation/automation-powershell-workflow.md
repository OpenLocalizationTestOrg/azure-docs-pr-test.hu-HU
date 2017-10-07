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
# <a name="learning-key-windows-powershell-workflow-concepts-for-automation-runbooks"></a>Tanulási automatizálási runbookok Windows PowerShell munkafolyamat alapfogalmai 
Azure Automation Runbookjai Windows PowerShell-munkafolyamatként vannak megvalósítva.  A Windows PowerShell munkafolyamat tooa hasonló Windows PowerShell-parancsfájlt, de néhány jelentős különbség az, amely egyértelmű tooa új felhasználó.  Ez a cikk nem tervezett toohelp PowerShell munkafolyamat runbookok írása, de javasolt PowerShell használatával, ha nem kell az ellenőrzőpontok runbookok írása.  Több szintaktikai különbségek vannak PowerShell munkafolyamat runbookok létrehozásakor, és ezek a különbségek szükséges egy kicsit nagyobb munkahelyi toowrite hatékony munkafolyamatokat.  

Egy munkafolyamat olyan programozott, összekapcsolódó lépések, amelyek hosszan futó feladatokat végeznek el vagy több eszközön vagy felügyelt csomóponton keresztül hello végrehajtandó, több lépés koordinációját igénylik sorozatát. hello egyszerű parancsprogramokhoz képest munkafolyamat előnyei közé tartozik, hello toosimultaneously egy műveletet több eszközön, és hello képességét tooautomatically helyreállni hibák után. A Windows PowerShell munkafolyamat által használt Windows folyamatkövető alaprendszer Windows PowerShell-parancsfájl. Hello munkafolyamat készült Windows PowerShell-szintaxis, és a Windows PowerShell által elindított, feldolgozását a Windows Workflow Foundation.

Ebben a cikkben hello kérdésekben teljes részletekért lásd: [Ismerkedés a Windows PowerShell munkafolyamat](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="basic-structure-of-a-workflow"></a>A munkafolyamatok alapvető szerkezete
hello első lépés tooconverting PowerShell parancsfájl tooa PowerShell munkafolyamat van csatolva hozzá azt hello **munkafolyamat** kulcsszó.  Egy munkafolyamat kezdődik hello **munkafolyamat** kulcsszó hello hello parancsprogram kapcsos zárójelek között elhelyezett törzse követ. hello hello munkafolyamat neve követi hello **munkafolyamat** látható módon hello szintaxisa a következő kulcsszó:

    Workflow Test-Workflow
    {
       <Commands>
    }

hello munkafolyamat hello nevének meg kell egyeznie az Automation-runbook hello hello nevét. Ha hello runbook importálja, majd hello fájlnév hello munkafolyamat nevének egyeznie kell, és kell végződnie *.ps1*.

tooadd paraméterek toohello munkafolyamat, használjon hello **Param** , ahogy tooa parancsfájl kulcsszó.

## <a name="code-changes"></a>Kódmódosítások
PowerShell-munkafolyamat kódot jelek majdnem azonos tooPowerShell parancsfájlkód néhány jelentős változások kivételével.  a következő szakaszok hello változást, hogy toomake tooa PowerShell-parancsfájl ki azt a munkafolyamat toorun ismertetik.

### <a name="activities"></a>Tevékenységek
Egy tevékenység készen egy adott feladat egy munkafolyamatot belül. Ugyanúgy, mint a parancsfájl egy vagy több parancsból áll, egy munkafolyamat áll egy vagy több, sorrendben végrehajtott tevékenységből áll. A Windows PowerShell-munkafolyamat automatikusan alakít számos Windows PowerShell parancsmagok tooactivities hello egy munkafolyamat futtatásakor. Ha megadja e parancsmagok egyikét a runbookban, a Windows Workflow Foundation hello megfelelő tevékenységet futtatja. Azon parancsmagok esetében nem tartozik megfelelő tevékenység, a Windows PowerShell munkafolyamat automatikusan futtat hello parancsmagot egy [InlineScript](#inlinescript) tevékenység. Nincs olyan parancsmagokat, amelyek ki vannak zárva és nem használható egy munkafolyamatban, ha nem explicit módon adja meg azokat az InlineScript blokkon belül. Ezekről a fogalmakról további információkért lásd: [tevékenységek használata parancsfájl-munkafolyamatok](http://technet.microsoft.com/library/jj574194.aspx).

Munkafolyamat-tevékenységek jellemzőkkel egy közös paraméterek tooconfigure a műveletet. Hello általános munkafolyamat-paraméterekkel kapcsolatos részletekért lásd: [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Pozícióparaméterek
A tevékenységek és a munkafolyamat-parancsmagok pozícióparaméterek nem használható.  Ez azt jelenti az, hogy a paraméter nevét kell használnia.

Vegyük példaként a következő kódot, amely lekérdezi az összes futó szolgáltatások hello.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Ha toorun Ez ugyanazt a kódot egy munkafolyamatban, kap egy üzenetet, például "Paraméter beállítása nem lehet feloldani a megadott hello használatával elnevezett paramétereket."  toocorrect, adja meg a hello paraméter neve, ahogy hello következő.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deszerializált objektum
A munkafolyamatokban objektumok deszerializálni vannak.  Ez azt jelenti, hogy a tulajdonságai továbbra is elérhetők, de nem a módszerek.  Vegyük példaként a következő PowerShell-kódot, amely leállítja a szolgáltatások hello Stop metódus hello szolgáltatás objektum történő hello.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Ha megpróbál toorun Ez a munkafolyamat, hibaüzenet arról, hogy "a metódushívás nem támogatott a Windows PowerShell-munkafolyamat."  

Egy elem toowrap két sort a kód egy [InlineScript](#inlinescript) esetben $Service lenne egy szolgáltatás-objektummal hello blokk letiltása.

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    }

Másik lehetőség is toouse végző másik parancsmag hello hello módszerként ugyanezeket a funkciókat, ha elérhető ilyen.  A mintában szereplő hello szolgáltatás leállítása a parancsmag nyújt hello hello Stop metódus, és azonos funkciókat hello következő használhatja egy munkafolyamathoz.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>Az InlineScript
Hello **InlineScript** tevékenység akkor hasznos, ha szüksége toorun egy vagy több parancsból helyett PowerShell munkafolyamat hagyományos PowerShell-parancsfájlként.  Egy munkafolyamat-parancsok tooWindows Workflow Foundation kerülnek feldolgozásra, amíg InlineScript blokkon belül lévő parancsok Windows PowerShell dolgoznak fel.

Az InlineScript alább látható szintaxist a következő hello használja.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Egy InlineScript kimeneti hello kimeneti tooa változó hozzárendelésével adhatja vissza. hello alábbi példa a szolgáltatás leáll, és majd kiírja hello szolgáltatás neve.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Az InlineScript blokkon belül lehet értéket átadni, de kell használnia **$Using** hatókör-módosítót.  hello alábbi példa: azonos toohello előző példa azzal a különbséggel, hogy hello szolgáltatásnév által biztosított egy változó.

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


Lehet, hogy az InlineScript tevékenység bizonyos munkafolyamatok alapvető fontosságú, amíg azok nem támogatják a munkafolyamat szerkezetek, és csak kell használni, amikor a következő okok miatt hello szükséges:

* Nem használhat [ellenőrzőpontokat](#checkpoints) egy InlineScript blokkon belül. Ha hiba lép fel, hello blokk belül, hello elejétől hello blokk lehet folytatni.
* Nem használhat [párhuzamos végrehajtása](#parallel-processing) egy InlineScriptBlock belül.
* Az InlineScript méretezhetőséget biztosít a hello munkafolyamat van hatással, mert magánál tartja hello blokk teljes hosszán át hello InlineScript hello Windows PowerShell-munkamenetet.

További információ az InlineScript használatával, lásd: [Windows PowerShell-parancsok futtatása munkafolyamatban](http://technet.microsoft.com/library/jj574197.aspx) és [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).

## <a name="parallel-processing"></a>Párhuzamos feldolgozás
Egy Windows PowerShell-munkafolyamatok előnye hello képességét tooperform ahelyett, hogy párhuzamosan parancsokat egymás után, mint a szokásos parancsfájlt.

Használhatja a hello **párhuzamos** kulcsszó toocreate parancsprogram-blokkot tartalmazzon egyidejűleg futó parancsokat. Ez az alább látható szintaxist a következő hello használ. Ebben az esetben a Activity1 és az Activity2 kezdődik, hello ugyanannyi időt vesz igénybe. Activity3 csak az Activity1 és az Activity2 befejezése után elindul.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Vegyük példaként a következő PowerShell-parancsok, amelyek több fájlok tooa hálózati helyre másolja hello.  Ezek a parancsok egymás után futnak, úgy, hogy egy fájl Befejezés másolása hello mellett megkezdése előtt.     

    Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

hello következő munkafolyamat fut, ezek azonos parancsok párhuzamos, hogy az összes másolásának megkezdése: hello azonos idő.  Csak azután minden másolt hello üzenet jelenik meg.

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


Hello használható **ForEach-Parallel** egy gyűjtemény minden elemén tooprocess parancsok egyidejűleg összeállításához. hello hello gyűjtemény elemeinek feldolgozása párhuzamosan közben hello hello parancsfájlblokk parancsai egymás után futnak. Ez az alább látható szintaxist a következő hello használ. Ebben az esetben hello Activity1 kezdődik azonos időben hello gyűjtemény mindegyik elemére. Minden elemhez a runbook az Activity2 Activity1 befejeződése után elindul. Activity3 csak az Activity1 és az Activity2 az összes befejezése után elindul.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

a következő példa hello hasonló toohello előző példa fájlok másolása a párhuzamos.  Ebben az esetben egy üzenet jelenik meg a fájl másolását követően.  Csak azután minden másolta, teljesen végső hello üzenet jelenik meg.

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
> A gyermekrunbookok a párhuzamosan futó, mivel ez toogive megbízhatatlan eredményekhez kimutatták nem ajánlott.  hello néha hello gyermekrunbook kimenetét nem jelenik meg, és hatással lehet a beállításokat egy gyermek runbook más párhuzamos gyermek runbookokat hello
>

## <a name="checkpoints"></a>Az ellenőrzőpontok
A *ellenőrzőpont* hello, amely tartalmazza a változók aktuális értékét hello hello-munkafolyamat aktuális állapotáról pillanatképet, és a kimeneti létrehozott toothat pont. Ha egy munkafolyamatot fejeződik be a hiba, vagy fel van függesztve, majd hello fut, amikor legközelebb indul hello worfklow hello kezdetét helyett a legutóbbi ellenőrzőponttól.  Egy munkafolyamatban hello állíthat be ellenőrzőpontot **Checkpoint-Workflow** tevékenység.

A következő példakód hello, a kivételt, amely runbook az Activity2 hello munkafolyamat tooend után következik be. Ha hello munkafolyamat fut újra, runbook az Activity2 futtatásával, mivel az csak az utolsó ellenőrzőpont hello beállítása után kezdődik.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

A munkafolyamat célszerű ellenőrzőpontokat beállítani, tevékenységeket, lehet, hogy nagyon eséllyel fordulnak elő tooexception, és nem kell ismételni, ha hello munkafolyamat folytatása után. A munkafolyamat lehet, hogy hozzon létre például egy virtuális gépet. Beállíthat egy ellenőrzőpontot előtti és utáni hello parancsok toocreate hello virtuális gépet. Ha hello létrehozása sikertelen, majd az hello parancsok volna kell ismételni, ha hello munkafolyamat újra elindult. Ha hello worfklow meghiúsul, miután hello létrehozás sikeresen befejeződik, majd hello virtuális gép nem létrehozza újra hello munkafolyamat folytatásakor.

a következő példa hello több fájlok tooa hálózati helyre másolja, és beállítja az ellenőrzőpont után minden egyes fájl.  Ha hello hálózati hely nem vesztek el, majd hello munkafolyamat fejeződik be a hiba.  Amikor ismét elindul, folytatódik, ami azt jelenti, hogy csak a már másolt hello fájlok kimarad hello utolsó ellenőrzőpont.

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

Mivel a username hitelesítő hello hívása után nem maradnak [Suspend-Workflow](https://technet.microsoft.com/library/jj733586.aspx) tevékenység vagy után utolsó ellenőrzőpont hello, tooset hello hitelesítő adatok toonull kell, és ezután kérheti le azokat újra hello eszköz áruházból után **A munkafolyamat-felfüggesztési** vagy ellenőrzőpont nevezik.  Ellenkező esetben jelenhet meg a következő hibaüzenet hello: *hello munkafolyamat-feladatot nem lehet folytatni, vagy mert adatmegőrzési adatok nem teljesen mentve, vagy nem mentett adatok megőrzését sérült. Hello munkafolyamat újra kell indítani.*

a következő ugyanazt a kódot hello bemutatja, hogyan toohandle Ez csak a PowerShell-munkafolyamati forgatókönyvek.

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


Ez pedig nem szükséges vannak hitelesítéséhez a használatával egy egyszerű szolgáltatással konfigurált futtató fiókot.  

Az ellenőrzőpontok kapcsolatos további információkért lásd: [ellenőrzőpontok felvétele tooa parancsfájl-munkafolyamathoz](http://technet.microsoft.com/library/jj574114.aspx).

## <a name="next-steps"></a>Következő lépések
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)
