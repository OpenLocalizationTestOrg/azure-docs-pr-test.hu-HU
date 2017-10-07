---
title: "az Azure Automationben aaaConnection eszközök |} Microsoft Docs"
description: "Azure Automation szolgáltatásbeli kapcsolódási eszközök hello szükséges adatokat tooconnect tooan külső szolgáltatás vagy alkalmazás a runbookot vagy a DSC-konfiguráció tartalmazza. Ez a cikk ismerteti a kapcsolatok hello részleteit, és hogyan toowork velük a szöveges és a grafikus szerzői."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: f0239017-5c66-4165-8cca-5dcb249b8091
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/13/2017
ms.author: magoedte; bwren
ms.openlocfilehash: f0f6b9fb960789b34af7b60eb1069313fdcf071c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connection-assets-in-azure-automation"></a>Azure Automation szolgáltatásbeli kapcsolódási eszközök

Automation szolgáltatásbeli kapcsolódási eszköz hello szükséges adatokat tooconnect tooan külső szolgáltatás vagy alkalmazás a runbookot vagy a DSC-konfiguráció tartalmazza. Ide tartozhat például a felhasználónév és jelszó hozzáadása tooconnection információkat, például egy URL-cím és port a hitelesítéshez szükséges adatokat. kapcsolat hello értékének megakadályozza a hello tulajdonságokat tooa adott alkalmazás egy eszköz a megakadályozását toocreating csatlakozás több változót. hello felhasználó szerkesztheti-e a kapcsolat egy helyen hello értékek, és egyetlen paramétert is adjon át egy kapcsolat tooa runbookot vagy a DSC-konfiguráció hello neve. hello tulajdonságai, a kapcsolat érhetők el a hello runbookot vagy a DSC-konfiguráció hello **Get-AutomationConnection** tevékenység.

Amikor kapcsolatot hoz létre, meg kell adnia egy *kapcsolattípus*. hello kapcsolódási típus a sablont, amely tulajdonságait határozza meg. hello kapcsolat határozza meg a kapcsolódási típus definiált tulajdonságok értékeit. Kapcsolattípusok integrációs modulok a hozzáadott tooAzure Automation vagy hello létre [Azure Automation API](http://msdn.microsoft.com/library/azure/mt163818.aspx) Ha hello integrációs modul kapcsolattípus tartalmaz, és importálja az Automation-fiók. Ellenkező esetben szüksége lesz egy metaadatok fájl toospecify Automation kapcsolat típusú toocreate.  Ezzel kapcsolatos további információkért lásd: [integrációs modulok](automation-integration-modules.md).  

>[!NOTE] 
>Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók. Ezek az eszközök titkosítottak és a tárolt hello Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz. Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja. Tárolása biztonságos eszköz, mielőtt hello kulcs hello automation-fiók visszafejtése hello fő tanúsítványt használ, és akkor használja a tooencrypt hello eszköz.

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-parancsmagjai

hello parancsmagok a következő táblázat hello használt toocreate és a Windows PowerShell-lel Automation kapcsolatok kezelése. Ezek hello részét képezi [Azure PowerShell modul](/powershell/azure/overview) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.

|Parancsmag|Leírás|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Lekéri a kapcsolat. Egy kivonattáblát a hello kapcsolat mezők értékekkel hello tartalmazza.|
|[Új AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Létrehoz egy új kapcsolatot.|
|[Remove-AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Eltávolít egy létező kapcsolatot.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Egy létező kapcsolat adott mezőjének hello értéket.|

## <a name="activities"></a>Tevékenységek

a következő táblázat hello hello tevékenységek használt tooaccess kapcsolatok egy runbookot vagy a DSC-konfiguráció a rendszer.

|Tevékenységek|Leírás|
|---|---|
|[Get-AutomationConnection](/powershell/module/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Egy kapcsolat toouse lekérdezi. Hello kapcsolat tulajdonságait hello kivonatoló tábla visszaadása.|

>[!NOTE] 
>Kerülendő a változók használata hello – Name paraméterében **Get - AutomationConnection** óta ez megnehezítheti a runbookot vagy a DSC-konfigurációk és kapcsolati objektumok közti függőségek tervezési időben.

## <a name="creating-a-new-connection"></a>Az új kapcsolat létrehozása

### <a name="toocreate-a-new-connection-with-hello-azure-portal"></a>toocreate új kapcsolatot hello Azure-portálon

1. Kattintson az automation-fiók hello **eszközök** rész tooopen hello **eszközök** panelen.
2. Kattintson a hello **kapcsolatok** rész tooopen hello **kapcsolatok** panelen.
3. Kattintson a **kapcsolatot** hello panel hello tetején.
4. A hello **típus** legördülő menüben válassza hello típusú kapcsolat toocreate szeretné. hello űrlap jelent adott típusú hello tulajdonságait.
5. Hello űrlap kitöltése és kattintson a **létrehozása** toosave hello új kapcsolatot.

### <a name="toocreate-a-new-connection-with-hello-azure-classic-portal"></a>a klasszikus Azure portálon hello új kapcsolatot toocreate

1. Az automation-fiók kattintson **eszközök** hello ablak hello tetején.
2. Hello ablak hello alul kattintson **beállítás hozzáadása**.
3. Kattintson a **kapcsolat hozzáadása a**.
4. A hello **kapcsolattípus** legördülő menüben válassza hello típusú kapcsolat toocreate szeretné.  hello varázsló megjelennek az adott típusú hello tulajdonságait.
5. Hello varázsló befejezéséhez, majd kattintson a hello jelölőnégyzet toosave hello új kapcsolatot.

### <a name="toocreate-a-new-connection-with-windows-powershell"></a>a Windows PowerShell használatával új kapcsolat toocreate

Új kapcsolat létrehozása a Windows PowerShell használatával hello [New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) parancsmag. Ez a parancsmag nevű paraméterrel rendelkezik **ConnectionFieldValues** vár, amely egy [kivonattábla](http://technet.microsoft.com/library/hh847780.aspx) értékek meghatározása az egyes hello kapcsolattípus által meghatározott hello tulajdonság.

Ha ismeri a hello Automation [Futtatás mint fiók](automation-sec-configure-azure-runas-account.md) tooauthenticate runbookokat hello szolgáltatás egyszerű, használatával hello PowerShell-parancsfájlt is egy alternatív toocreating hello hello Futtatás mint fiók hello portálról létrehoz egy új kapcsolódási eszköz a következő Példaparancsok hello segítségével.  

    $ConnectionAssetName = "AzureRunAsConnection"
    $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
    New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues 

Képes toouse hello parancsfájl toocreate hello kapcsolódási eszköz, mert az Automation-fiók létrehozásakor automatikusan tartalmaz több globális modulok együtt hello kapcsolattípus alapértelmezés szerint **AzurServicePrincipal**toocreate hello **AzureRunAsConnection** kapcsolódási eszköz.  Ennek oka az szem előtt, fontos tookeep megpróbálnak toocreate egy új kapcsolat eszköz tooconnect tooa szolgáltatás vagy alkalmazás egy másik hitelesítési módszert, mert hello kapcsolat típusa nem már definiálva van az Automation-fiók meghiúsul.  További információk hogyan toocreate saját kapcsolat írja be az egyéni vagy a modulnak a hello [PowerShell-galériában](https://www.powershellgallery.com), lásd: [integrációs modulok](automation-integration-modules.md)
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>A runbookot vagy a DSC-konfiguráció kapcsolat használatával

Egy kapcsolat a runbookot vagy a DSC-konfiguráció hello lekérése **Get-AutomationConnection** parancsmag.  Nem használhat hello [Get-AzureRmAutomationConnection](https://docs.microsoft.com/powershell/resourcemanager/azurerm.automation/v1.0.12/Get-AzureRmAutomationConnection?redirectedfrom=msdn) tevékenység.  Ez a tevékenység hello értékek hello kapcsolat különböző mezőinek hello kéri le, és adja vissza őket egy [kivonattábla](http://go.microsoft.com/fwlink/?LinkID=324844) amelyek ezután felhasználhatók a hello megfelelő parancsaival hello runbookot vagy a DSC-konfiguráció.

### <a name="textual-runbook-sample"></a>Szöveges forgatókönyvként minta

hello következő mintaparancsok bemutatják, hogyan toouse hello futtató fiók már említettük, az Azure Resource Manager erőforrást a runbookban tooauthenticate.  Hello kapcsolatot használ az eszköz hello Futtatás mint fiókot, amely hello-alapú szolgáltatás egyszerű hivatkozik, amely nem hitelesítő adatait.  

    $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint 

### <a name="graphical-runbook-samples"></a>Grafikus forgatókönyv minták

Hozzáadhat egy **Get-AutomationConnection** tevékenység tooa grafikus forgatókönyvnek hello kapcsolaton hello grafikus szerkesztőben, majd válassza a hello könyvtár ablaktáblán kattintson a jobb gombbal **toocanvas hozzáadása**.

![](media/automation-connections/connection-add-canvas.png)

hello alábbi ábrán egy példa egy grafikus forgatókönyvnek kapcsolat használatával.  Ez a hello ugyanebben a példában az hello futtató fiók használata a szöveges forgatókönyvként fent látható.  Ez a példa hello **konstans** hello meg van adva **RunAs kapcsolat beolvasása** tevékenység egy olyan kapcsolatobjektum használ.  A [folyamatkapcsolódása](automation-graphical-authoring-intro.md#links-and-workflow) óta hello ServicePrincipalCertificate paraméterhalmaz vár egy adott objektum itt szolgál.

![](media/automation-connections/automation-get-connection-object.png)

## <a name="next-steps"></a>Következő lépések

- Felülvizsgálati [hivatkozások grafikus szerzői](automation-graphical-authoring-intro.md#links-and-workflow) hogyan toodirect és vezérlés hello áramlását a runbook logikája toounderstand.  

- További információ az Azure Automation PowerShell-modulok és használata ajánlott eljárások létrehozásához a saját PowerShell-modulok toowork integrációs modulok belül Azure Automation toolearn lásd [integrációs modulok](automation-integration-modules.md).  
