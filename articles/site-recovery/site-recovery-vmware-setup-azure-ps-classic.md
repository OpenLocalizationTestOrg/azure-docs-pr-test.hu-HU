---
title: " A Azure(Classic) folyamat kiszolgáló kezeléséhez |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset fel a feladatátvételi folyamat Server(Classic) az Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: eadcc0236c77c9ebbbc885c4a7ee81098f1f4e72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-process-server-running-in-azure-classic"></a><span data-ttu-id="832e5-103">Egy folyamat (klasszikus) Azure-t futtató kiszolgáló kezelése</span><span class="sxs-lookup"><span data-stu-id="832e5-103">Manage a Process Server running in Azure (Classic)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="832e5-104">Klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="832e5-104">Azure Classic </span></span>](./site-recovery-vmware-setup-azure-ps-classic.md)
> * [<span data-ttu-id="832e5-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="832e5-105">Resource Manager</span></span>](./site-recovery-vmware-setup-azure-ps-resource-manager.md)

<span data-ttu-id="832e5-106">A feladat-visszavétel során nagy késleltetésű hello Azure virtuális hálózat és a helyszíni hálózat között esetén ajánlott toodeploy Folyamatkiszolgáló az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="832e5-106">During failback, it is recommended toodeploy Process Server in Azure if there is high latency between hello Azure Virtual Network and your on-premises network.</span></span> <span data-ttu-id="832e5-107">Ez a cikk ismerteti, hogyan beállításához, konfigurálásához és hello folyamat kiszolgálók Azure-beli kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="832e5-107">This article describes how you can set up, configure, and manage hello process servers running in Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="832e5-108">Ez a cikk ha használt klasszikus telepítési modell hello hello virtuális gépek a feladatátvétel során használt toobe.</span><span class="sxs-lookup"><span data-stu-id="832e5-108">This article is toobe used if you used Classic as hello deployment model for hello virtual machines during failover.</span></span> <span data-ttu-id="832e5-109">Ha Ön használhat erőforrás-kezelő telepítési modell kövesse hello lépéseit hello [hogyan tooset akár & a feladat-visszavételi Folyamatkiszolgáló (erőforrás-kezelő) konfigurálása](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="832e5-109">If you used Resource Manager as hello deployment model follow hello steps in [How tooset up & configure a Failback Process Server (Resource Manager)](./site-recovery-vmware-setup-azure-ps-resource-manager.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="832e5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="832e5-110">Prerequisites</span></span>

[!INCLUDE [site-recovery-vmware-process-server-prereq](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]

## <a name="deploy-a-process-server-on-azure"></a><span data-ttu-id="832e5-111">Az Azure-on folyamat-kiszolgáló központi telepítése</span><span class="sxs-lookup"><span data-stu-id="832e5-111">Deploy a Process Server on Azure</span></span>

1. <span data-ttu-id="832e5-112">Azure piactér, hozzon létre egy virtuális gép hello **Microsoft Azure Site helyreállítási folyamat kiszolgáló V2**</span><span class="sxs-lookup"><span data-stu-id="832e5-112">In Azure Marketplace, create a virtual machine using hello **Microsoft Azure Site Recovery Process Server V2**</span></span> </br>
    <span data-ttu-id="832e5-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span><span class="sxs-lookup"><span data-stu-id="832e5-113">![Marketplace_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image.png)</span></span>
2. <span data-ttu-id="832e5-114">Győződjön meg arról, hogy kiválassza hello üzembe helyezési modellel, **klasszikus**</span><span class="sxs-lookup"><span data-stu-id="832e5-114">Ensure that you select hello deployment model as **Classic**</span></span> </br><span data-ttu-id="832e5-115">
  ![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span><span class="sxs-lookup"><span data-stu-id="832e5-115">
![Marketplace_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/marketplace-ps-image-classic.png)</span></span>
3. <span data-ttu-id="832e5-116">Hello hozzon létre virtuális gép varázsló > alapbeállítások, győződjön meg arról, átadja a hello virtuális gépek hello előfizetésben és helyen toowhere választja.</span><span class="sxs-lookup"><span data-stu-id="832e5-116">In hello Create virtual machine wizard > Basic Settings, ensure you select hello Subscription and Location toowhere you failed over hello virtual machines.</span></span></br><span data-ttu-id="832e5-117">
  ![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span><span class="sxs-lookup"><span data-stu-id="832e5-117">
![create_image_1](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-basic-info.png)</span></span>
4. <span data-ttu-id="832e5-118">Győződjön meg arról, hogy hello a virtuális gép csatlakozik toohello Azure Virtual Network toowhich hello átadja a virtuális géphez van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="832e5-118">Ensure that hello virtual machine is connected toohello Azure Virtual Network toowhich hello failed over virtual machine is connected.</span></span></br><span data-ttu-id="832e5-119">
  ![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span><span class="sxs-lookup"><span data-stu-id="832e5-119">
![create_image_2](./media/site-recovery-vmware-setup-azure-ps-classic/azureps-classic-settings.png)</span></span>
5. <span data-ttu-id="832e5-120">Hello Folyamatkiszolgáló virtuális gép üzembe helyezése után meg kell a toolog, és regisztrálhatja azt az hello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="832e5-120">Once hello Process Server virtual machine is provisioned, you need toolog in and register it with hello Configuration Server.</span></span>

> [!NOTE]
> <span data-ttu-id="832e5-121">toobe képes toouse a Folyamatkiszolgálót a feladat-visszavétel tooregister kell azt hello helyszíni konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="832e5-121">toobe able toouse this Process Server for failback, you need tooregister it with hello on-premises configuration server.</span></span>

## <a name="registering-hello-process-server-running-in-azure-tooa-configuration-server-running-on-premises"></a><span data-ttu-id="832e5-122">Hello (Azure-beli) Folyamatkiszolgáló tooa konfigurációs kiszolgáló (a helyszínen fut) regisztrálása</span><span class="sxs-lookup"><span data-stu-id="832e5-122">Registering hello Process Server (running in Azure) tooa Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

## <a name="upgrading-hello-process-server-toolatest-version"></a><span data-ttu-id="832e5-123">Hello Folyamatkiszolgáló toolatest verziójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="832e5-123">Upgrading hello Process Server toolatest version.</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-upgrade-process-server.md)]

## <a name="unregistering-hello-process-server-running-in-azure-from-a-configuration-server-running-on-premises"></a><span data-ttu-id="832e5-124">Regisztrációjának megszüntetése hello Folyamatkiszolgáló (Azure-beli) a konfigurációs kiszolgálóról (a helyszínen fut)</span><span class="sxs-lookup"><span data-stu-id="832e5-124">Unregistering hello Process Server (running in Azure) from a Configuration Server (running on-premises)</span></span>

[!INCLUDE [site-recovery-vmware-upgrade-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]
