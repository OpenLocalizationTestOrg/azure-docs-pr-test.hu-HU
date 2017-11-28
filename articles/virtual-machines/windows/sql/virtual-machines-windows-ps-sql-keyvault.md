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
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a><span data-ttu-id="d7263-104">SQL Server az Azure Key Vault-integráció konfigurálása az Azure virtuális gépeken (erőforrás-kezelő)</span><span class="sxs-lookup"><span data-stu-id="d7263-104">Configure Azure Key Vault Integration for SQL Server on Azure Virtual Machines (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d7263-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d7263-105">Resource Manager</span></span>](virtual-machines-windows-ps-sql-keyvault.md)
> * [<span data-ttu-id="d7263-106">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="d7263-106">Classic</span></span>](../classic/ps-sql-keyvault.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="d7263-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d7263-107">Overview</span></span>
<span data-ttu-id="d7263-108">Nincsenek több SQL Server titkosítási szolgáltatások, például a [átlátható adattitkosítás (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [oszlop a blokkszintű titkosítás (törlése)](https://msdn.microsoft.com/library/ms173744.aspx), és [a biztonsági másolat titkosításához](https://msdn.microsoft.com/library/dn449489.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7263-108">There are multiple SQL Server encryption features, such as [transparent data encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [column level encryption (CLE)](https://msdn.microsoft.com/library/ms173744.aspx), and [backup encryption](https://msdn.microsoft.com/library/dn449489.aspx).</span></span> <span data-ttu-id="d7263-109">Titkosítási űrlapok toomanage megkövetelik, és hello titkosításhoz használt kriptográfiai kulcsok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="d7263-109">These forms of encryption require you toomanage and store hello cryptographic keys you use for encryption.</span></span> <span data-ttu-id="d7263-110">hello Azure Key Vault (AKV) szolgáltatás a tervezett tooimprove hello biztonságához és kezeléséhez ezeket a kulcsokat, a magas rendelkezésre állású és biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="d7263-110">hello Azure Key Vault (AKV) service is designed tooimprove hello security and management of these keys in a secure and highly available location.</span></span> <span data-ttu-id="d7263-111">Hello [SQL Server-összekötő](http://www.microsoft.com/download/details.aspx?id=45344) lehetővé teszi, hogy a SQL Server toouse ezeket a kulcsokat az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="d7263-111">hello [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) enables SQL Server toouse these keys from Azure Key Vault.</span></span>

<span data-ttu-id="d7263-112">Ha Ön a helyszíni SQL Server rendszert futtató gépek esetében nincs vannak [lépések követésével tooaccess Azure Key Vault a helyszíni SQL Server-számítógépről](https://msdn.microsoft.com/library/dn198405.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7263-112">If you running SQL Server with on-premises machines, there are [steps you can follow tooaccess Azure Key Vault from your on-premises SQL Server machine](https://msdn.microsoft.com/library/dn198405.aspx).</span></span> <span data-ttu-id="d7263-113">De az SQL Server Azure virtuális gépeken, időt takaríthat meg hello segítségével *Azure Key Vault-integráció* szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d7263-113">But for SQL Server in Azure VMs, you can save time by using hello *Azure Key Vault Integration* feature.</span></span>

<span data-ttu-id="d7263-114">Ha ez a funkció engedélyezve van, automatikusan hello SQL Server-összekötőt telepíti, konfigurál hello EKM provider tooaccess Azure Key Vault, és létrehoz hello credential tooallow tooaccess meg a tároló.</span><span class="sxs-lookup"><span data-stu-id="d7263-114">When this feature is enabled, it automatically installs hello SQL Server Connector, configures hello EKM provider tooaccess Azure Key Vault, and creates hello credential tooallow you tooaccess your vault.</span></span> <span data-ttu-id="d7263-115">Ha nézett hello hello lépéseit azt már korábban említettük a helyszíni dokumentáció, láthatja, hogy ez a szolgáltatás automatizálja a 2. és 3.</span><span class="sxs-lookup"><span data-stu-id="d7263-115">If you looked at hello steps in hello previously mentioned on-premises documentation, you can see that this feature automates steps 2 and 3.</span></span> <span data-ttu-id="d7263-116">hello egyedül toodo még mindig manuálisan kell toocreate hello kulcstartó és a kulcsok.</span><span class="sxs-lookup"><span data-stu-id="d7263-116">hello only thing you would still need toodo manually is toocreate hello key vault and keys.</span></span> <span data-ttu-id="d7263-117">Ott hello teljes telepítése az SQL virtuális gép automatikus.</span><span class="sxs-lookup"><span data-stu-id="d7263-117">From there, hello entire setup of your SQL VM is automated.</span></span> <span data-ttu-id="d7263-118">Ez a funkció a telepítés befejezése után végre lehet hajtani ezeket az adatbázisokat vagy a biztonsági mentések titkosításához, a szokásos módon a T-SQL utasítás toobegin.</span><span class="sxs-lookup"><span data-stu-id="d7263-118">Once this feature has completed this setup, you can execute T-SQL statements toobegin encrypting your databases or backups as you normally would.</span></span>

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a><span data-ttu-id="d7263-119">Engedélyezése és AKV-integráció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d7263-119">Enabling and configuring AKV integration</span></span>
<span data-ttu-id="d7263-120">Kiépítés során AKV-integráció engedélyezéséhez, vagy konfigurálja úgy a meglévő virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="d7263-120">You can enable AKV integration during provisioning or configure it for existing VMs.</span></span>

### <a name="new-vms"></a><span data-ttu-id="d7263-121">Új virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="d7263-121">New VMs</span></span>
<span data-ttu-id="d7263-122">Ha egy új SQL Server virtuális gép a Resource Manager kiépíteni, hello Azure-portálon biztosít egy lépés tooenable Azure Key Vault-integráció.</span><span class="sxs-lookup"><span data-stu-id="d7263-122">If you are provisioning a new SQL Server virtual machine with Resource Manager, hello Azure portal provides a step tooenable Azure Key Vault integration.</span></span> <span data-ttu-id="d7263-123">hello Azure Key Vault szolgáltatás csak hello Enterprise, Developer és SQL Server kiadásai próbaverzióját érhető el.</span><span class="sxs-lookup"><span data-stu-id="d7263-123">hello Azure Key Vault feature is available only for hello Enterprise, Developer and Evaluation Editions of SQL Server.</span></span>

![SQL Azure Key Vault-integráció](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

<span data-ttu-id="d7263-125">Kiépítés részletes útmutatást lásd: [egy SQL Server rendszerű virtuális gép az Azure portál hello](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="d7263-125">For a detailed walkthrough of provisioning, see [Provision a SQL Server virtual machine in hello Azure Portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

### <a name="existing-vms"></a><span data-ttu-id="d7263-126">Meglévő virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="d7263-126">Existing VMs</span></span>
<span data-ttu-id="d7263-127">Meglévő SQL Server virtuális gépek válassza ki az SQL Server virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d7263-127">For existing SQL Server virtual machines, select your SQL Server virtual machine.</span></span> <span data-ttu-id="d7263-128">Válassza ki hello **SQL Server-konfigurációs** hello szakasza **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="d7263-128">Then select hello **SQL Server configuration** section of hello **Settings** blade.</span></span>

![Meglévő virtuális gépek SQL AKV-integráció](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

<span data-ttu-id="d7263-130">A hello **SQL Server-konfigurációs** panelen kattintson hello **szerkesztése** hello automatikus Key Vault-integráció szakasz gombjára.</span><span class="sxs-lookup"><span data-stu-id="d7263-130">In hello **SQL Server configuration** blade, click hello **Edit** button in hello Automated Key Vault integration section.</span></span>

![Meglévő virtuális gépek SQL AKV-integráció konfigurálása](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

<span data-ttu-id="d7263-132">Ha elkészült, kattintson a hello **OK** hello alsó részén hello gombjára **SQL Server-konfigurációs** panel toosave a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="d7263-132">When finished, click hello **OK** button on hello bottom of hello **SQL Server configuration** blade toosave your changes.</span></span>

> [!NOTE]
> <span data-ttu-id="d7263-133">Beállíthatja úgy is AKV-integráció sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="d7263-133">You can also configure AKV integration using a template.</span></span> <span data-ttu-id="d7263-134">További információkért lásd: [Azure gyors üzembe helyezés sablon az Azure Key Vault-integráció](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span><span class="sxs-lookup"><span data-stu-id="d7263-134">For more information, see [Azure quickstart template for Azure Key Vault integration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).</span></span>
> 
> 

[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]

