---
title: "aaaPassing-felhasználó hitelesítő adatait a DSC használata tooAzure |} Microsoft Docs"
description: "Áttekintés biztonságosan átadja a felhasználó hitelesítő adatait tooAzure virtuális gépek PowerShell célállapot-konfiguráció"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: 306ecd3fd481f49a0beca5052fc7531a52999330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="passing-credentials-toohello-azure-dsc-extension-handler"></a>Hitelesítő adatok toohello Azure DSC bővítmény kezelő továbbításához
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Ez a cikk az Azure-hello célállapot-konfiguráció bővítmény ismerteti. Hello DSC-bővítmény kezelő áttekintést található [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="passing-in-credentials"></a>A hitelesítő adatok sikeres
Hello konfigurációs folyamat részeként szükség lehet a tooset felhasználói fiókok létrehozása szolgáltatások elérésére és a program telepítése a felhasználói környezetben. toodo ezeket a módosításokat tooprovide hitelesítő adatok szükségesek. 

A DSC lehetővé teszi a paraméteres konfigurációk, ahol hitelesítő adatokat hello konfiguráció lett átadva, és biztonságos módon tárolja az MOF-fájlok. hello Azure-kiterjesztés kezelőjének egyszerűbbé teszi a hitelesítőadat-kezelés, adja meg a tanúsítványok automatikus kezelését. 

Vegye figyelembe a következő DSC konfigurációs parancsfájl, amely létrehoz egy helyi felhasználói fiókot hello megadott jelszóval hello:

*user_configuration.ps1*

```
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )    
    Node localhost {       
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        } 
    }  
} 
```

Fontos tooinclude *csomópont localhost* hello konfiguráció részeként. Hiányzik a jelen nyilatkozatot, hello lépések nem működnek, hello bővítmény kezelő kifejezetten hello csomópont localhost utasítás keresi. Fontos továbbá tooinclude hello typecast *[PsCredential]*, mert az adott típusú elindítja a hello bővítmény tooencrypt hello hitelesítő adatot. 

A parancsfájl tooblob tároló közzététele:

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Állítsa be a hello Azure DSC-bővítményt, és adja meg a hello hitelesítő adat:

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a>Hitelesítő adatok titkosításának módját
Ezt a kódot futtató hitelesítő adatokat kér. Amennyiben azt biztosítja, tárolódik a memóriában rövid időre. Ha közzé van téve a `Set-AzureVmDscExtension` parancsmag, azt HTTPS toohello VM keresztül továbbított, ahol az Azure tárolja a lemezen, hello helyi VM tanúsítvány használatával titkosítja. Akkor célszerű a memória, és újra titkosítani toopass röviden visszafejtett azt tooDSC.

Ez a viselkedés eltér [nélkül hello bővítmény kezelő biztonságos konfigurációk használatával](https://msdn.microsoft.com/powershell/dsc/securemof). hello Azure környezetet biztosít az egy módon tootransmit konfigurációs adatok tanúsítványok segítségével biztonságosan. Hello DSC-bővítmény kezelő használata esetén nincs szükség tooprovide $CertificatePath vagy egy $CertificateID van / ConfigurationData $Thumbprint bejegyzést.

## <a name="next-steps"></a>Következő lépések
Hello Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Vizsgálja meg a hello [Azure Resource Manager sablon hello DSC-bővítmény](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview). 

a PowerShell DSC kezelhető toofind további funkciók [keresse meg a PowerShell-galériában hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) további DSC-erőforrásokat.

