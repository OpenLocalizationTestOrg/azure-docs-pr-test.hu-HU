---
title: "Az Azure Automationben eszközök tanúsítvány |} Microsoft Docs"
description: "Tanúsítványok tárolhatja biztonságosan Azure Automation, azok a runbookok vagy DSC-konfigurációk hitelesítése az Azure és a harmadik féltől származó erőforrások elérhetők.  Ez a cikk ismerteti a tanúsítványok és a szöveges és a grafikus szerzői őket munkavégzés részleteit."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: ac9c22ae-501f-42b9-9543-ac841cf2cc36
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2016
ms.author: magoedte;bwren
ms.openlocfilehash: fd1737a420c132dace9307436bfea98a9bde94a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Automation szolgáltatásbeli tanúsítvány eszközök

Tanúsítványok tárolhatja biztonságosan Azure Automation, azok a runbookok vagy a DSC-konfigurációk használatával elérhetők a **Get-AzureRmAutomationRmCertificate** Azure Resource Manager erőforrások tevékenység. Ez lehetővé teszi a runbookok és a hitelesítési tanúsítványokat használó DSC-konfigurációk létrehozásához, vagy hozzáadja azokat az Azure vagy harmadik féltől származó erőforrások.

> [!NOTE] 
> Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók. Ezek az eszközök titkosítva, és tárolja az Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz. Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja. Előtt tárolása biztonságos eszköz, az automatizálási fiók kulcs visszafejtése a mestertanúsítvány, és majd az eszköz titkosításához használt.
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-parancsmagjai

A következő táblázatban található parancsmagokkal létrehozása és kezelése az automation tanúsítvány eszközök a Windows PowerShell segítségével. Részét képezi a [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.

|Parancsmagok|Leírás|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx)|Lekéri az információkat a runbookot vagy a DSC-konfiguráció használandó tanúsítványt. Maga a tanúsítvány csak Get-AutomationCertificate tevékenység kérhetnek le.|
|[Új AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603604.aspx)|Létrehoz egy új tanúsítványt az Azure Automation.|
[Remove-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603529.aspx)|Azure Automation szolgáltatásbeli tanúsítvány eltávolítása.|Létrehoz egy új tanúsítványt az Azure Automation.
|[Set-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603760.aspx)|Beleértve a tanúsítványfájl feltöltését és a jelszót a .pfx fájlhoz beállítás meglévő tanúsítvány tulajdonságainak beállítása.|
|[Adja hozzá AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|A megadott felhőszolgáltatás egy szolgáltatástanúsítványt feltöltését.|


## <a name="creating-a-new-certificate"></a>Egy új tanúsítvány létrehozása

Amikor létrehoz egy új tanúsítványt, .cer vagy .pfx-fájlt töltsön fel az Azure Automation. Ha a tanúsítvány exportálhatóként, majd azt át is az Azure Automation tanúsítványtároló kívül. Ha nem exportálható, majd csak használat belül a runbookot vagy a DSC-konfiguráció az aláíráshoz.


### <a name="to-create-a-new-certificate-with-the-azure-portal"></a>Új tanúsítvány létrehozása az Azure portállal

1. Az Automation-fiók, kattintson a **eszközök** csempére kattintva nyissa meg a **eszközök** panelen.
1. Kattintson a **tanúsítványok** csempére kattintva nyissa meg a **tanúsítványok** panelen.
1. Kattintson a **tanúsítvány hozzáadása** a panel tetején.
2. Adjon meg egy nevet a tanúsítványt a **neve** mezőbe.
2. Kattintson a **válasszon ki egy fájlt** alatt **tanúsítvány-fájl feltöltése** egy .cer vagy .pfx-fájl.  Ha kiválaszt egy .pfx fájlt, adja meg a jelszót, és hogy azt engedélyezni kell exportálni.
1. Kattintson a **létrehozása** menti az új tanúsítvány adategység.


### <a name="to-create-a-new-certificate-with-windows-powershell"></a>Új tanúsítvány létrehozása a Windows PowerShell használatával

A következő példa bemutatja, hogyan hozzon létre egy új Automation-tanúsítvány, és jelölje be a exportálható. Ez importálja a meglévő .pfx fájlt.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Olyan tanúsítványt használ

Kell használnia a **Get-AutomationCertificate** tevékenység tanúsítvány használatára. Nem használhatja a [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) parancsmag, mivel a tanúsítvány eszköz, de nem maga a tanúsítvány kapcsolatos információkat ad vissza.

### <a name="textual-runbook-sample"></a>Szöveges forgatókönyvként minta

Az alábbi mintakód bemutatja, hogyan tanúsítvány hozzáadása a runbookban egy felhőalapú szolgáltatás. Ez a példa a jelszó lekért egy titkosított automation változó.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Grafikus forgatókönyv minta

Hozzáadhat egy **Get-AutomationCertificate** egy grafikus forgatókönyvnek a tanúsítványt a könyvtár ablaktáblán grafikus szerkesztő csomagot jobb gombbal, majd válassza a **vászonra Hozzáadás**.

![Tanúsítvány felvétele a vászonra](media/automation-certificates/automation-certificate-add-to-canvas.png)

Az alábbi ábrán például egy grafikus runbookban levő olyan tanúsítványt használ.  Ez az ugyanebben a példában egy felhőalapú szolgáltatás szöveges runbookból egy tanúsítvány felvétele a fent látható.

![Példa grafikus készítése ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a>Következő lépések

- A tevékenységek a runbookban az célja, hogy végre logikai üzenetáramlásának szabályozására hivatkozások használata kapcsolatos további információkért lásd: [hivatkozások grafikus szerzői](automation-graphical-authoring-intro.md#links-and-workflow). 
