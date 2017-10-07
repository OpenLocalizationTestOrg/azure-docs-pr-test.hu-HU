---
title: "az Azure Automationben aaaCertificate eszközök |} Microsoft Docs"
description: "Tanúsítványok tárolhatja biztonságosan Azure Automation, a runbookot vagy a DSC-konfigurációk tooauthenticate Azure és a harmadik féltől származó erőforrások elleni hozzáférhetők.  Ez a cikk ismerteti a tanúsítvány hello adatait, és hogyan toowork velük a szöveges és a grafikus szerzői."
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
ms.openlocfilehash: 2c25bee937890438ea9022669be2c24c77a110b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="certificate-assets-in-azure-automation"></a>Azure Automation szolgáltatásbeli tanúsítvány eszközök

Tanúsítványok tárolhatja biztonságosan Azure Automation, azok a runbookok vagy hello segítségével a DSC-konfigurációk elérhetők **Get-AzureRmAutomationRmCertificate** Azure Resource Manager erőforrások tevékenység. Ez lehetővé teszi az toocreate runbookok és a DSC-konfigurációk, amelyek tanúsítványokat használnak a hitelesítéshez, vagy azokat beveszi tooAzure vagy harmadik féltől származó erőforrások.

> [!NOTE] 
> Az Azure Automationben biztonságos eszközök közé tartozik a hitelesítő adatokat, a tanúsítványokat, a kapcsolatok és a titkosított változók. Ezek az eszközök titkosítottak és a tárolt hello Azure Automation létrehozott egyedi kulcs segítségével minden egyes automation-fiókhoz. Ezt a kulcsot egy mestertanúsítvány titkosítja és az Azure Automationben tárolja. Tárolása biztonságos eszköz, mielőtt hello kulcs hello automation-fiók visszafejtése hello fő tanúsítványt használ, és akkor használja a tooencrypt hello eszköz.
> 

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell-parancsmagjai

hello parancsmagok a következő táblázat hello használt toocreate és a Windows PowerShell-lel automation-tanúsítvány eszközök kezelése. Ezek hello részét képezi [Azure PowerShell modul](../powershell-install-configure.md) elérhető Automation-forgatókönyveket és a DSC-konfigurációk.

|Parancsmagok|Leírás|
|:---|:---|
|[Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx)|Lekéri a runbookot vagy a DSC-konfiguráció egy tanúsítvány toouse kapcsolatos információkat. Maga hello tanúsítvány csak Get-AutomationCertificate tevékenység kérhetnek le.|
|[Új AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603604.aspx)|Létrehoz egy új tanúsítványt az Azure Automation.|
[Remove-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603529.aspx)|Azure Automation szolgáltatásbeli tanúsítvány eltávolítása.|Létrehoz egy új tanúsítványt az Azure Automation.
|[Set-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603760.aspx)|Beállítja egy létező tanúsítványt, beleértve a tanúsítványfájl hello és beállítás hello jelszót a .pfx fájlhoz feltöltése hello tulajdonságait.|
|[Adja hozzá AzureCertificate](https://msdn.microsoft.com/library/azure/dn495214.aspx)|Egy szolgáltatástanúsítványa hello feltöltések megadott felhőalapú szolgáltatás.|


## <a name="creating-a-new-certificate"></a>Egy új tanúsítvány létrehozása

Amikor létrehoz egy új tanúsítványt, egy .cer vagy .pfx fájlt tooAzure Automation töltse fel. Ha exportálható hello tanúsítványok, majd azt át is hello Azure Automation-tanúsítványtároló kívül. Ha nem exportálható, majd csak használat belül hello runbookot vagy a DSC-konfiguráció az aláíráshoz.


### <a name="toocreate-a-new-certificate-with-hello-azure-portal"></a>toocreate egy új tanúsítványt hello Azure-portálon

1. Kattintson az Automation-fiók hello **eszközök** csempe tooopen hello **eszközök** panelen.
1. Kattintson a hello **tanúsítványok** csempe tooopen hello **tanúsítványok** panelen.
1. Kattintson a **tanúsítvány hozzáadása** hello panel hello tetején.
2. Írja be egy nevet a tanúsítványnak hello hello **neve** mezőbe.
2. Kattintson a **válasszon ki egy fájlt** alatt **tanúsítvány-fájl feltöltése** toobrowse egy .cer vagy .pfx-fájl.  Ha kiválaszt egy .pfx fájlt, adja meg a jelszót, és hogy azt engedélyezni kell a toobe exportált.
1. Kattintson a **létrehozása** toosave hello új tanúsítvány adategység.


### <a name="toocreate-a-new-certificate-with-windows-powershell"></a>a Windows PowerShell használatával új tanúsítványt toocreate

hello következő példa bemutatja, hogyan toocreate egy új Automation tanúsítványt, és jelölje meg exportálható. Ez importálja a meglévő .pfx fájlt.

    $certName = 'MyCertificate'
    $certPath = '.\MyCert.pfx'
    $certPwd = ConvertTo-SecureString -String 'P@$$w0rd' -AsPlainText -Force
    $ResourceGroup = "ResourceGroup01"
    
    New-AzureRmAutomationCertificate -AutomationAccountName "MyAutomationAccount" -Name $certName -Path $certPath –Password $certPwd -Exportable -ResourceGroupName $ResourceGroup

## <a name="using-a-certificate"></a>Olyan tanúsítványt használ

Hello kell használnia **Get-AutomationCertificate** tevékenység toouse egy tanúsítványt. Nem használhat hello [Get-AzureRmAutomationCertificate](https://msdn.microsoft.com/library/mt603765.aspx) parancsmag óta hello tanúsítványeszköz, de nem maga hello tanúsítvány kapcsolatos információkat ad vissza.

### <a name="textual-runbook-sample"></a>Szöveges forgatókönyvként minta

a következő példakód hello bemutatja, hogyan tooadd egy tanúsítvány tooa felhőszolgáltatás egy runbookban. Ez a példa hello jelszó lekért egy titkosított automation változó.

    $serviceName = 'MyCloudService'
    $cert = Get-AutomationCertificate -Name 'MyCertificate'
    $certPwd = Get-AzureRmAutomationVariable -ResourceGroupName "ResouceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyCertPassword'
    Add-AzureCertificate -ServiceName $serviceName -CertToDeploy $cert

### <a name="graphical-runbook-sample"></a>Grafikus forgatókönyv minta

Hozzáadhat egy **Get-AutomationCertificate** tooa grafikus forgatókönyvnek hello tanúsítványa hello grafikus szerkesztőben, majd válassza a hello könyvtár ablaktáblán kattintson a jobb gombbal **toocanvas hozzáadása**.

![Vegye fel a tanúsítvány toohello vászonra](media/automation-certificates/automation-certificate-add-to-canvas.png)

hello következő kép bemutatja egy olyan tanúsítványt használ az olyan grafikus forgatókönyvnek példát.  Ez a hello ugyanebben a példában a tanúsítvány tooa felhőszolgáltatás hozzáadásához szöveges forgatókönyvként a fent látható.

![Példa grafikus készítése ](media/automation-certificates/graphical-runbook-add-certificate.png)


## <a name="next-steps"></a>Következő lépések

- legalább hivatkozások toocontrol hello tételéig tevékenységek a runbookban használatáról toolearn tervezett tooperform című [hivatkozások grafikus szerzői](automation-graphical-authoring-intro.md#links-and-workflow). 
