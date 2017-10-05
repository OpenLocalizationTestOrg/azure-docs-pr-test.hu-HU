---
title: "Hitelesítőadat-eszköz az Azure Automationben |} Microsoft Docs"
description: "Az Azure Automationben hitelesítő eszközök tartalmaz a hitelesítő adatokat, amelyek segítségével a runbookot vagy a DSC-konfiguráció számára elérhető erőforrások hitelesítéséhez. Ez a cikk ismerteti, hogyan hitelesítő eszközök létrehozása és azok a runbookot vagy a DSC-konfiguráció használatát."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 3209bf73-c208-425e-82b6-df49860546dd
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: bwren
ms.openlocfilehash: e2857515f3842a875ef7b5a9327392818931168f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="credential-assets-in-azure-automation"></a>Azure Automation szolgáltatásbeli hitelesítőadat eszközök
Automation szolgáltatásbeli hitelesítőadat-eszköz rendelkezik egy [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) biztonsági hitelesítő adatok, például a felhasználónevet és jelszót tartalmazó objektum. A Runbookok és a DSC-konfigurációk használhat parancsmagokat, fogadja el a hitelesítést egy PSCredential objektumot, vagy azokat lehet, hogy bontsa ki a felhasználónevet és jelszót a PSCredential objektum bizonyos alkalmazás vagy a hitelesítés megkövetelése a szolgáltatás biztosításához. Egy hitelesítő adat tulajdonságait az Azure Automationben biztonságosan tároljuk, és a runbookot vagy a DSC-konfiguráció érhetők el a [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) tevékenység.

> [!NOTE]
> Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók. Ezek az eszközök titkosítva, és tárolja az Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz. Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja. Előtt tárolása biztonságos eszköz, az automatizálási fiók kulcs visszafejtése a mestertanúsítvány, és majd az eszköz titkosításához használt.  

## <a name="windows-powershell-cmdlets"></a>A Windows PowerShell-parancsmagok
A következő táblázatban található parancsmagokkal létrehozása és kezelése az automatizálási hitelesítő eszközök a Windows PowerShell segítségével.  Részét képezi a [Azure PowerShell modul](/powershell/azure/overview) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.

| Parancsmagok | Leírás |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Lekéri a hitelesítőadat-eszköz kapcsolatos információkat. Csak kérheti le a hitelesítő adatokat, maga a **Get-AutomationPSCredential** tevékenység. |
| [Új AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Új automatizálási hitelesítő adatot hoz létre. |
| [Remove - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Eltávolítja az automatizálási hitelesítő adatok. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Egy meglévő automatizálási hitelesítő adat tulajdonságainak beállítása. |

## <a name="runbook-activities"></a>Runbook-tevékenységek
Az alábbi táblázatban a tevékenységek a runbookok és a DSC-konfigurációk hitelesítő adatok eléréséhez használt.

| Tevékenységek | Leírás |
|:--- |:--- |
| Get-AutomationPSCredential |Lekérdezi a runbookot vagy a DSC-konfiguráció használandó hitelesítő adatokat. Értéket ad vissza egy [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objektum. |

> [!NOTE]
> Kerülendő a változók használata – Name paraméterében Get-AutomationPSCredential, mivel ez nehezíti a runbookok vagy a DSC-konfigurációk közti függőségek, és eszközök hitelesítőadat-tervezési időben.
> 
> 

## <a name="creating-a-new-credential-asset"></a>Egy új hitelesítőadat-eszköz létrehozása

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Egy új hitelesítőadat-eszköz létrehozása az Azure portállal
1. Az automation-fiók, kattintson a **eszközök** nyissa meg a kijelző a **eszközök** panelen.
2. Kattintson a **hitelesítő adatok** nyissa meg a kijelző a **hitelesítő adatok** panelen.
3. Kattintson a **a hitelesítő adatok hozzáadása** a panel tetején.
4. Töltse ki az űrlapot, és kattintson a **létrehozása** az új hitelesítő adatok mentése.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Új hitelesítőadat-eszköz létrehozása a Windows PowerShell használatával
Az alábbi Példaparancsok szemléltetik egy új automation-hitelesítő adat létrehozása. Egy PSCredential objektum első létrehozása a nevét és jelszavát és a hitelesítőadat-eszköz létrehozásához szükségesek. Másik lehetőségként használhatja a **Get-Credential** parancsmag adja meg egy nevet és jelszót kéri.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

### <a name="to-create-a-new-credential-asset-with-the-azure-classic-portal"></a>Egy új hitelesítőadat-eszköz létrehozása a klasszikus Azure portálon
1. Az automation-fiók kattintson **eszközök** az ablak tetején.
2. Az ablak alján kattintson **beállítás hozzáadása**.
3. Kattintson a **hitelesítő adatok hozzáadása**.
4. Az a **hitelesítőadat-típus** legördülő menüből válassza **PowerShell-hitelesítő adat**.
5. Fejezze be a varázslót, és kattintson a jelölőnégyzetbe, az új hitelesítő adatok mentése.

## <a name="using-a-powershell-credential"></a>PowerShell-hitelesítő adat használata
Hitelesítőadat-eszköz a runbookot vagy a DSC-konfiguráció lekérése a **Get-AutomationPSCredential** tevékenység. Ez visszaad egy [PSCredential objektum](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) használható, amely egy PSCredential paraméter szükséges parancsmag vagy tevékenység. A külön-külön használandó hitelesítő objektum tulajdonságainak visszakeresése. Az objektum a felhasználónév és a biztonságos jelszó tulajdonsággal rendelkezik, vagy használhatja a **GetNetworkCredential** metódus vissza egy [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) objektum, amely egy nem biztonságos verzióját a a jelszó.

### <a name="textual-runbook-sample"></a>Szöveges forgatókönyvként minta
Az alábbi Példaparancsok szemléltetik egy PowerShell-hitelesítő adat használata a runbookokban. Ebben a példában a hitelesítő adatok lekérésére és a felhasználónév és jelszó hozzárendelt változók.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Grafikus forgatókönyv minta
Hozzáadhat egy **Get-AutomationPSCredential** tevékenység és a hitelesítő adatokat a könyvtár ablaktáblán grafikus szerkesztő csomagot jobb gombbal, majd válassza a grafikus forgatókönyvnek **vászonra Hozzáadás**.

![Hitelesítő adatok vászonra hozzáadása](media/automation-credentials/credential-add-canvas.png)

Az alábbi ábrán egy példa egy grafikus forgatókönyvnek a hitelesítő adat segítségével.  Ebben az esetben használja a hitelesítés az Azure-erőforrások runbook leírtak [Runbookok hitelesítéséhez az Azure AD felhasználói fiókkal](automation-create-aduser-account.md).  Az első tevékenységet, amely hozzáfér az Azure-előfizetés hitelesítő adatait kéri le.  A **Add-AzureAccount** tevékenység használja ezeket a hitelesítő adatokat a hitelesítés a tevékenységeket, amelyek azt követően származnak.  A [folyamatkapcsolódása](automation-graphical-authoring-intro.md#links-and-workflow) óta a következő helyen **Get-AutomationPSCredential** egy adott objektum által várt paraméterekkel.  

![Hitelesítő adatok vászonra hozzáadása](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>A DSC PowerShell-hitelesítő adat használata
Azure Automation DSC-konfigurációja is hivatkozni lehessen hitelesítő eszközök használata során **Get-AutomationPSCredential**, hitelesítő eszközök is adhatók át a keresztül paraméterek, ha szükséges. További információkért lásd: [Azure Automation DSC-konfigurációja fordítása](automation-dsc-compile.md#credential-assets).

## <a name="next-steps"></a>Következő lépések
* A grafikus szerzői hivatkozások kapcsolatos további információkért lásd: [grafikus szerzői hivatkozások](automation-graphical-authoring-intro.md#links-and-workflow)
* Az Automation szolgáltatásban, a különböző hitelesítési módszerek ismertetése: [Azure Automation szolgáltatásbeli biztonsági](automation-security-overview.md)
* A grafikus forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első grafikus forgatókönyvem](automation-first-runbook-graphical.md).
* A PowerShell-alapú munkafolyamat-forgatókönyvekkel való ismerkedéshez tekintse meg a következőt: [Az első PowerShell-alapú munkafolyamat-forgatókönyvem](automation-first-runbook-textual.md) 

