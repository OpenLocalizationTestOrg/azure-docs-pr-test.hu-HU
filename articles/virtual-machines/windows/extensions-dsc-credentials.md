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
# <a name="passing-credentials-toohello-azure-dsc-extension-handler"></a><span data-ttu-id="66fc3-103">Hitelesítő adatok toohello Azure DSC bővítmény kezelő továbbításához</span><span class="sxs-lookup"><span data-stu-id="66fc3-103">Passing credentials toohello Azure DSC extension handler</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="66fc3-104">Ez a cikk az Azure-hello célállapot-konfiguráció bővítmény ismerteti.</span><span class="sxs-lookup"><span data-stu-id="66fc3-104">This article covers hello Desired State Configuration extension for Azure.</span></span> <span data-ttu-id="66fc3-105">Hello DSC-bővítmény kezelő áttekintést található [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="66fc3-105">An overview of hello DSC extension handler can be found at [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="passing-in-credentials"></a><span data-ttu-id="66fc3-106">A hitelesítő adatok sikeres</span><span class="sxs-lookup"><span data-stu-id="66fc3-106">Passing in credentials</span></span>
<span data-ttu-id="66fc3-107">Hello konfigurációs folyamat részeként szükség lehet a tooset felhasználói fiókok létrehozása szolgáltatások elérésére és a program telepítése a felhasználói környezetben.</span><span class="sxs-lookup"><span data-stu-id="66fc3-107">As a part of hello configuration process, you may need tooset up user accounts, access services, or install a program in a user context.</span></span> <span data-ttu-id="66fc3-108">toodo ezeket a módosításokat tooprovide hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="66fc3-108">toodo these things, you need tooprovide credentials.</span></span> 

<span data-ttu-id="66fc3-109">A DSC lehetővé teszi a paraméteres konfigurációk, ahol hitelesítő adatokat hello konfiguráció lett átadva, és biztonságos módon tárolja az MOF-fájlok.</span><span class="sxs-lookup"><span data-stu-id="66fc3-109">DSC allows for parameterized configurations in which credentials are passed into hello configuration and securely stored in MOF files.</span></span> <span data-ttu-id="66fc3-110">hello Azure-kiterjesztés kezelőjének egyszerűbbé teszi a hitelesítőadat-kezelés, adja meg a tanúsítványok automatikus kezelését.</span><span class="sxs-lookup"><span data-stu-id="66fc3-110">hello Azure Extension Handler simplifies credential management by providing automatic management of certificates.</span></span> 

<span data-ttu-id="66fc3-111">Vegye figyelembe a következő DSC konfigurációs parancsfájl, amely létrehoz egy helyi felhasználói fiókot hello megadott jelszóval hello:</span><span class="sxs-lookup"><span data-stu-id="66fc3-111">Consider hello following DSC configuration script that creates a local user account with hello specified password:</span></span>

<span data-ttu-id="66fc3-112">*user_configuration.ps1*</span><span class="sxs-lookup"><span data-stu-id="66fc3-112">*user_configuration.ps1*</span></span>

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

<span data-ttu-id="66fc3-113">Fontos tooinclude *csomópont localhost* hello konfiguráció részeként.</span><span class="sxs-lookup"><span data-stu-id="66fc3-113">It is important tooinclude *node localhost* as part of hello configuration.</span></span> <span data-ttu-id="66fc3-114">Hiányzik a jelen nyilatkozatot, hello lépések nem működnek, hello bővítmény kezelő kifejezetten hello csomópont localhost utasítás keresi.</span><span class="sxs-lookup"><span data-stu-id="66fc3-114">If this statement is missing, hello following steps do not work as hello extension handler specifically looks for hello node localhost statement.</span></span> <span data-ttu-id="66fc3-115">Fontos továbbá tooinclude hello typecast *[PsCredential]*, mert az adott típusú elindítja a hello bővítmény tooencrypt hello hitelesítő adatot.</span><span class="sxs-lookup"><span data-stu-id="66fc3-115">It is also important tooinclude hello typecast *[PsCredential]*, as this specific type triggers hello extension tooencrypt hello credential.</span></span> 

<span data-ttu-id="66fc3-116">A parancsfájl tooblob tároló közzététele:</span><span class="sxs-lookup"><span data-stu-id="66fc3-116">Publish this script tooblob storage:</span></span>

`Publish-AzureVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

<span data-ttu-id="66fc3-117">Állítsa be a hello Azure DSC-bővítményt, és adja meg a hello hitelesítő adat:</span><span class="sxs-lookup"><span data-stu-id="66fc3-117">Set hello Azure DSC extension and provide hello credential:</span></span>

```
$configurationName = "Main"
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = "user_configuration.ps1.zip"
$vm = Get-AzureVM "example-1"

$vm = Set-AzureVMDSCExtension -VM $vm -ConfigurationArchive $configurationArchive 
-ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureVM
```
## <a name="how-credentials-are-secured"></a><span data-ttu-id="66fc3-118">Hitelesítő adatok titkosításának módját</span><span class="sxs-lookup"><span data-stu-id="66fc3-118">How credentials are secured</span></span>
<span data-ttu-id="66fc3-119">Ezt a kódot futtató hitelesítő adatokat kér.</span><span class="sxs-lookup"><span data-stu-id="66fc3-119">Running this code prompts for a credential.</span></span> <span data-ttu-id="66fc3-120">Amennyiben azt biztosítja, tárolódik a memóriában rövid időre.</span><span class="sxs-lookup"><span data-stu-id="66fc3-120">Once it is provided, it is stored in memory briefly.</span></span> <span data-ttu-id="66fc3-121">Ha közzé van téve a `Set-AzureVmDscExtension` parancsmag, azt HTTPS toohello VM keresztül továbbított, ahol az Azure tárolja a lemezen, hello helyi VM tanúsítvány használatával titkosítja.</span><span class="sxs-lookup"><span data-stu-id="66fc3-121">When it is published with `Set-AzureVmDscExtension` cmdlet, it is transmitted over HTTPS toohello VM, where Azure stores it encrypted on disk, using hello local VM certificate.</span></span> <span data-ttu-id="66fc3-122">Akkor célszerű a memória, és újra titkosítani toopass röviden visszafejtett azt tooDSC.</span><span class="sxs-lookup"><span data-stu-id="66fc3-122">Then it is briefly decrypted in memory and re-encrypted toopass it tooDSC.</span></span>

<span data-ttu-id="66fc3-123">Ez a viselkedés eltér [nélkül hello bővítmény kezelő biztonságos konfigurációk használatával](https://msdn.microsoft.com/powershell/dsc/securemof).</span><span class="sxs-lookup"><span data-stu-id="66fc3-123">This behavior is different than [using secure configurations without hello extension handler](https://msdn.microsoft.com/powershell/dsc/securemof).</span></span> <span data-ttu-id="66fc3-124">hello Azure környezetet biztosít az egy módon tootransmit konfigurációs adatok tanúsítványok segítségével biztonságosan.</span><span class="sxs-lookup"><span data-stu-id="66fc3-124">hello Azure environment gives a way tootransmit configuration data securely via certificates.</span></span> <span data-ttu-id="66fc3-125">Hello DSC-bővítmény kezelő használata esetén nincs szükség tooprovide $CertificatePath vagy egy $CertificateID van / ConfigurationData $Thumbprint bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="66fc3-125">When using hello DSC extension handler, there is no need tooprovide $CertificatePath or a $CertificateID / $Thumbprint entry in ConfigurationData.</span></span>

## <a name="next-steps"></a><span data-ttu-id="66fc3-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66fc3-126">Next steps</span></span>
<span data-ttu-id="66fc3-127">Hello Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="66fc3-127">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="66fc3-128">Vizsgálja meg a hello [Azure Resource Manager sablon hello DSC-bővítmény](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="66fc3-128">Examine hello [Azure Resource Manager template for hello DSC extension](extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="66fc3-129">További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="66fc3-129">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

<span data-ttu-id="66fc3-130">a PowerShell DSC kezelhető toofind további funkciók [keresse meg a PowerShell-galériában hello](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) további DSC-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="66fc3-130">toofind additional functionality you can manage with PowerShell DSC, [browse hello PowerShell gallery](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) for more DSC resources.</span></span>

