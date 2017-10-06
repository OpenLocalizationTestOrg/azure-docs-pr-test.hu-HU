---
title: "az Azure Automationben aaaCredential eszközök |} Microsoft Docs"
description: "Azure Automation szolgáltatásbeli hitelesítőadat eszközök hitelesítő adatokat, amelyek lehet hello runbookot vagy a DSC-konfiguráció által elért használt tooauthenticate tooresources tartalmaz. Ez a cikk ismerteti, hogyan toocreate hitelesítőadat-eszközöket, és használja őket a runbookot vagy a DSC-konfiguráció."
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
ms.openlocfilehash: 46f23a8f79d5863265af9cf84f6003e30f8e7d39
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="credential-assets-in-azure-automation"></a>Azure Automation szolgáltatásbeli hitelesítőadat eszközök
Automation szolgáltatásbeli hitelesítőadat-eszköz rendelkezik egy [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) biztonsági hitelesítő adatok, például a felhasználónevet és jelszót tartalmazó objektum. Runbookok és a DSC-konfigurációk használhat parancsmagokat, fogadja el a hitelesítést egy PSCredential objektumot, vagy azok hello felhasználónevet és a jelszót a hello PSCredential objektum tooprovide toosome alkalmazás vagy a szolgáltatás hitelesítést igénylő előfordulhat, hogy kibontásához. hello tulajdonságok a hitelesítő adatokat az Azure Automationben biztonságosan tároljuk, és hello runbookot vagy a DSC-konfiguráció hello érhetők el [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) tevékenység.

> [!NOTE]
> Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók. Ezek az eszközök titkosítottak és a tárolt hello Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz. Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja. Tárolása biztonságos eszköz, mielőtt hello kulcs hello automation-fiók visszafejtése hello fő tanúsítványt használ, és akkor használja a tooencrypt hello eszköz.  

## <a name="windows-powershell-cmdlets"></a>A Windows PowerShell-parancsmagok
hello parancsmagok a következő táblázat hello használt toocreate és a Windows PowerShell-lel automatizálási hitelesítő eszközök kezelése.  Ezek hello részét képezi [Azure PowerShell modul](/powershell/azure/overview) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.

| Parancsmagok | Leírás |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Lekéri a hitelesítőadat-eszköz kapcsolatos információkat. Csak le hello hitelesítő adat, maga a **Get-AutomationPSCredential** tevékenység. |
| [Új AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Új automatizálási hitelesítő adatot hoz létre. |
| [Remove - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Eltávolítja az automatizálási hitelesítő adatok. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Beállítja egy meglévő automatizálási hitelesítő adatok tulajdonságainak hello. |

## <a name="runbook-activities"></a>Runbook-tevékenységek
a következő táblázat hello hello tevékenységek használt tooaccess hitelesítő adatokat a runbook és a DSC-konfigurációk.

| Tevékenységek | Leírás |
|:--- |:--- |
| Get-AutomationPSCredential |Lekérdezi a hitelesítő adatok toouse a runbookot vagy a DSC-konfiguráció. Értéket ad vissza egy [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) objektum. |

> [!NOTE]
> Kerülendő a változók használata hello – Name paraméterében Get-AutomationPSCredential, mivel ez nehezíti a runbookok vagy a DSC-konfigurációk közti függőségek, és eszközök hitelesítőadat-tervezési időben.
> 
> 

## <a name="creating-a-new-credential-asset"></a>Egy új hitelesítőadat-eszköz létrehozása

### <a name="toocreate-a-new-credential-asset-with-hello-azure-portal"></a>toocreate új hitelesítőadat-eszköz hello Azure-portálon
1. Kattintson az automation-fiók hello **eszközök** rész tooopen hello **eszközök** panelen.
2. Hello kattintson **hitelesítő adatok** rész tooopen hello **hitelesítő adatok** panelen.
3. Kattintson a **a hitelesítő adatok hozzáadása** hello panel hello tetején.
4. Hello űrlap kitöltése és kattintson a **létrehozása** toosave hello új hitelesítő adatok.

### <a name="toocreate-a-new-credential-asset-with-windows-powershell"></a>a Windows PowerShell használatával új hitelesítőadat-eszköz toocreate
hello következő mintaparancsok bemutatják, hogyan toocreate egy új Automation szolgáltatásbeli hitelesítőadat-e. Egy PSCredential objektum első létrehozása hello nevét és jelszavát, és akkor használja a toocreate hello hitelesítőadat-eszköz. Másik megoldásként használhat hello **Get-Credential** parancsmag toobe tootype a nevet és jelszót kéri.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

### <a name="toocreate-a-new-credential-asset-with-hello-azure-classic-portal"></a>a klasszikus Azure portálon hello új hitelesítőadat-eszköz toocreate
1. Az automation-fiók kattintson **eszközök** hello ablak hello tetején.
2. Hello ablak hello alul kattintson **beállítás hozzáadása**.
3. Kattintson a **hitelesítő adatok hozzáadása**.
4. A hello **hitelesítőadat-típus** legördülő menüből válassza **PowerShell-hitelesítő adat**.
5. Hello varázsló befejezéséhez, majd kattintson a hello jelölőnégyzet toosave hello új hitelesítő adatok.

## <a name="using-a-powershell-credential"></a>PowerShell-hitelesítő adat használata
A runbookot vagy a DSC-konfiguráció hello hitelesítőadat-eszköz lekérése **Get-AutomationPSCredential** tevékenység. Ez visszaad egy [PSCredential objektum](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) használható, amely egy PSCredential paraméter szükséges parancsmag vagy tevékenység. Hello hitelesítő objektum toouse hello tulajdonságainak külön-külön is beolvasása. hello objektum tulajdonsággal rendelkezik hello felhasználónév és a biztonságos jelszó hello, vagy használhatja a hello **GetNetworkCredential** metódus tooreturn egy [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) objektum, amely egy nem biztonságos hello jelszó verzióját.

### <a name="textual-runbook-sample"></a>Szöveges forgatókönyvként minta
hello következő mintaparancsok bemutatják, hogyan toouse egy PowerShell hitelesítő adatokat, a runbookot. Ebben a példában hello hitelesítő adatok lekérésére és a felhasználónév és jelszó hozzárendelt toovariables.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Grafikus forgatókönyv minta
Hozzáadhat egy **Get-AutomationPSCredential** tevékenység tooa grafikus forgatókönyvnek kattintson a jobb gombbal a hello hello könyvtár ablaktáblán hello grafikus szerkesztőben, majd válassza a hitelesítő adatok **toocanvas hozzáadása**.

![Adja hozzá a hitelesítő adatok toocanvas](media/automation-credentials/credential-add-canvas.png)

hello alábbi ábrán egy példa egy grafikus forgatókönyvnek a hitelesítő adat segítségével.  Ebben az esetben, melynél a runbook tooAzure erőforrások használt tooprovide hitelesítési leírtak [Runbookok hitelesítéséhez az Azure AD felhasználói fiókkal](automation-create-aduser-account.md).  hello első tevékenység kéri le a hozzáférési toohello Azure-előfizetéssel rendelkező hello hitelesítő adatok.  Hello **Add-AzureAccount** tevékenység tevékenységeket is, majd a hitelesítő adatok tooprovide hitelesítést használ.  A [folyamatkapcsolódása](automation-graphical-authoring-intro.md#links-and-workflow) óta a következő helyen **Get-AutomationPSCredential** egy adott objektum által várt paraméterekkel.  

![Adja hozzá a hitelesítő adatok toocanvas](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>A DSC PowerShell-hitelesítő adat használata
Azure Automation DSC-konfigurációja is hivatkozni lehessen hitelesítő eszközök használata során **Get-AutomationPSCredential**, hitelesítő eszközök is adhatók át a keresztül paraméterek, ha szükséges. További információkért lásd: [Azure Automation DSC-konfigurációja fordítása](automation-dsc-compile.md#credential-assets).

## <a name="next-steps"></a>Következő lépések
* toolearn hivatkozások grafikus szerzői bővebben lásd: [grafikus szerzői hivatkozások](automation-graphical-authoring-intro.md#links-and-workflow)
* toounderstand hello különböző hitelesítési módszerek az Automation szolgáltatásban, lásd: [Azure Automation szolgáltatásbeli biztonsági](automation-security-overview.md)
* Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md) 

