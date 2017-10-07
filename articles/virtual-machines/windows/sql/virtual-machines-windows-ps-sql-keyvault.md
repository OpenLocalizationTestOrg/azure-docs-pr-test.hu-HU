---
title: "SQL Server Windows-alapú virtuális gépek (erőforrás-kezelő) Azure Key Vault aaaIntegrate |} Microsoft Docs"
description: "Ismerje meg, hogyan tooautomate hello konfigurálása az SQL Server titkosítás használata az Azure Key Vault. Ez a témakör ismerteti, hogyan toouse Azure Key Vault-integráció az SQL Server virtuális gépek létrehozása a Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/23/2017
ms.author: jroth
ms.openlocfilehash: 0d36d3d075d6538c18cd5ecb43c19a4b000a99e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>SQL Server az Azure Key Vault-integráció konfigurálása az Azure virtuális gépeken (erőforrás-kezelő)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasszikus](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a>Áttekintés
Nincsenek több SQL Server titkosítási szolgáltatások, például a [átlátható adattitkosítás (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [oszlop a blokkszintű titkosítás (törlése)](https://msdn.microsoft.com/library/ms173744.aspx), és [a biztonsági másolat titkosításához](https://msdn.microsoft.com/library/dn449489.aspx). Titkosítási űrlapok toomanage megkövetelik, és hello titkosításhoz használt kriptográfiai kulcsok tárolásához. hello Azure Key Vault (AKV) szolgáltatás a tervezett tooimprove hello biztonságához és kezeléséhez ezeket a kulcsokat, a magas rendelkezésre állású és biztonságos helyen. Hello [SQL Server-összekötő](http://www.microsoft.com/download/details.aspx?id=45344) lehetővé teszi, hogy a SQL Server toouse ezeket a kulcsokat az Azure Key Vault.

Ha Ön a helyszíni SQL Server rendszert futtató gépek esetében nincs vannak [lépések követésével tooaccess Azure Key Vault a helyszíni SQL Server-számítógépről](https://msdn.microsoft.com/library/dn198405.aspx). De az SQL Server Azure virtuális gépeken, időt takaríthat meg hello segítségével *Azure Key Vault-integráció* szolgáltatás.

Ha ez a funkció engedélyezve van, automatikusan hello SQL Server-összekötőt telepíti, konfigurál hello EKM provider tooaccess Azure Key Vault, és létrehoz hello credential tooallow tooaccess meg a tároló. Ha nézett hello hello lépéseit azt már korábban említettük a helyszíni dokumentáció, láthatja, hogy ez a szolgáltatás automatizálja a 2. és 3. hello egyedül toodo még mindig manuálisan kell toocreate hello kulcstartó és a kulcsok. Ott hello teljes telepítése az SQL virtuális gép automatikus. Ez a funkció a telepítés befejezése után végre lehet hajtani ezeket az adatbázisokat vagy a biztonsági mentések titkosításához, a szokásos módon a T-SQL utasítás toobegin.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Engedélyezése és AKV-integráció konfigurálása
Kiépítés során AKV-integráció engedélyezéséhez, vagy konfigurálja úgy a meglévő virtuális gépekhez.

### <a name="new-vms"></a>Új virtuális gépek
Ha egy új SQL Server virtuális gép a Resource Manager kiépíteni, hello Azure-portálon biztosít egy lépés tooenable Azure Key Vault-integráció. hello Azure Key Vault szolgáltatás csak hello Enterprise, Developer és SQL Server kiadásai próbaverzióját érhető el.

![SQL Azure Key Vault-integráció](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Kiépítés részletes útmutatást lásd: [egy SQL Server rendszerű virtuális gép az Azure portál hello](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Meglévő virtuális gépek
Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet. Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.

![Meglévő virtuális gépek SQL AKV-integráció](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello automatikus Key Vault-integráció szakasz gombjára.

![Meglévő virtuális gépek SQL AKV-integráció konfigurálása](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.

> [!NOTE]
> Beállíthatja úgy is AKV-integráció sablon használatával. További információkért lásd: [Azure gyors üzembe helyezés sablon az Azure Key Vault-integráció](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

