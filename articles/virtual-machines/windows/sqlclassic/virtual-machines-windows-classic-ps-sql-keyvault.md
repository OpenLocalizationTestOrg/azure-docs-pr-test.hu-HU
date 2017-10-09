---
title: "SQL Server Windows-alapú virtuális gépek (klasszikus) Azure Key Vault aaaIntegrate |} Microsoft Docs"
description: "Ismerje meg, hogyan tooautomate hello konfigurálása az SQL Server titkosítás használata az Azure Key Vault. Ez a témakör ismerteti, hogyan hozzon létre toouse Azure Key Vault-integráció az SQL Server virtuális gépekkel hello klasszikus üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: ab8d41a7-1971-4032-ab71-eb435c455dc1
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 02/17/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 54664875b76dac7271d5a9f00b3f41fdc9c08491
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-classic"></a>SQL Server az Azure Key Vault-integráció konfigurálása az Azure virtuális gépeken (klasszikus)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasszikus](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Áttekintés
Nincsenek több SQL Server titkosítási szolgáltatások, például a [átlátható adattitkosítás (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [oszlop a blokkszintű titkosítás (törlése)](https://msdn.microsoft.com/library/ms173744.aspx), és [a biztonsági másolat titkosításához](https://msdn.microsoft.com/library/dn449489.aspx). Titkosítási űrlapok toomanage megkövetelik, és hello titkosításhoz használt kriptográfiai kulcsok tárolásához. hello Azure Key Vault (AKV) szolgáltatás a tervezett tooimprove hello biztonságához és kezeléséhez ezeket a kulcsokat, a magas rendelkezésre állású és biztonságos helyen. Hello [SQL Server-összekötő](http://www.microsoft.com/download/details.aspx?id=45344) lehetővé teszi, hogy a SQL Server toouse ezeket a kulcsokat az Azure Key Vault.

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

SQL Server helyszíni gépeken futtatja, hogy van-e [lépések követésével tooaccess Azure Key Vault a helyszíni SQL Server-számítógépről](https://msdn.microsoft.com/library/dn198405.aspx). De az SQL Server Azure virtuális gépeken, időt takaríthat meg hello segítségével *Azure Key Vault-integráció* szolgáltatás. Az Azure PowerShell-parancsmagok néhány tooenable ezzel a szolgáltatással automatizálhatja hello konfiguráció egy SQL virtuális gép tooaccess szükséges a kulcstartót.

Ha ez a funkció engedélyezve van, automatikusan hello SQL Server-összekötőt telepíti, konfigurál hello EKM provider tooaccess Azure Key Vault, és létrehoz hello credential tooallow tooaccess meg a tároló. Ha nézett hello hello lépéseit azt már korábban említettük a helyszíni dokumentáció, láthatja, hogy ez a szolgáltatás automatizálja a 2. és 3. hello egyedül toodo még mindig manuálisan kell toocreate hello kulcstartó és a kulcsok. Ott hello teljes telepítése az SQL virtuális gép automatikus. Ez a funkció a telepítés befejezése után végre lehet hajtani ezeket az adatbázisokat vagy a biztonsági mentések titkosításához, a szokásos módon a T-SQL utasítás toobegin.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV-integráció konfigurálása
Használjon PowerShell tooconfigure Azure Key Vault-integráció. a következő szakaszok hello adja meg a szükséges hello paraméterek áttekintése és egy minta PowerShell-parancsfájlt.

### <a name="install-hello-sql-server-iaas-extension"></a>Hello SQL Server IaaS-bővítmény telepítése
Első, [hello SQL Server IaaS-bővítmény telepítése](../classic/sql-server-agent-extension.md).

### <a name="understand-hello-input-parameters"></a>Hello bemeneti paraméterek ismertetése
hello következő tábla listák hello paraméterek szükséges toorun hello PowerShell-parancsfájl hello a következő szakaszban.

| Paraméter | Leírás | Példa |
| --- | --- | --- |
| **$akvURL** |**hello kulcstároló URL-címe** |"https://contosokeyvault.vault.azure.net/" |
| **$spName** |**Egyszerű szolgáltatásnév** |"5-4e11-af04eb07b669ccf2 fde2b411 - 33d" |
| **$spSecret** |**Egyszerű titok** |"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =" |
| **$credName** |**Hitelesítő adat neve**: AKV-integráció hitelesítő adatot hoz létre az SQL Serverben, így hello VM toohave hozzáférés toohello kulcstároló. Válasszon egy nevet ennek a hitelesítő adatnak. |"mycred1" |
| **$vmName** |**Virtuális gép neve**: hello egy korábban létrehozott SQL virtuális gép nevét. |"myvmname" |
| **$serviceName** |**Szolgáltatásnév**: hello SQL virtuális gép társított hello felhőalapú szolgáltatás neve. |"mycloudservicename" |

### <a name="enable-akv-integration-with-powershell"></a>A PowerShell-lel AKV-integráció engedélyezése
Hello **New-AzureVMSqlServerKeyVaultCredentialConfig** parancsmag létrehoz egy konfigurációs objektumot hello Azure Key Vault-integráció szolgáltatáshoz. Hello **Set-AzureVMSqlServerExtension** konfigurálja ezt az integrációt hello **KeyVaultCredentialSettings** paraméter. a következő lépéseket megjelenítése hogyan hello toouse ezeket a parancsokat.

1. Az Azure PowerShell először konfigurálnia hello bemeneti paraméterek az adott értékek hello a jelen témakör korábbi szakaszokban ismertetett módon. a következő parancsfájl hello csak példaként szolgál.
   
        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2. Majd használjon hello alábbi tooconfigure parancsfájlt, és AKV-integráció engedélyezése.
   
        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

hello SQL infrastruktúra-szolgáltatási ügynök bővítmény frissíti hello SQL virtuális gép ezen új konfigurációjával.

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

