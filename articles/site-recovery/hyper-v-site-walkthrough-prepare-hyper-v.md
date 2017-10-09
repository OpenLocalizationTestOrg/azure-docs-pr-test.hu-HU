---
title: "aaaPrepare Hyper-V gazdagépek (a System Center VMM nélkül) a replikációs tooAzure |} Microsoft Docs"
description: "Ismerteti, hogyan tooprepare Hyper-V gazdagépek, az Azure Site Recovery segítségével replikációs tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0f204e24-8d78-4076-95c5-8137d1be9c01
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: 714b229d5efbd66a9844bd09e36ac3f69919a6bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-tooazure"></a><span data-ttu-id="29cd1-103">6. lépés: Hyper-V-gazdagépek előkészítése replikációs tooAzure</span><span class="sxs-lookup"><span data-stu-id="29cd1-103">Step 6: Prepare Hyper-V hosts for replication tooAzure</span></span>

<span data-ttu-id="29cd1-104">Ez a cikk tooprepare hello utasításait használatát a helyszíni Hyper-V gazdagépek toointeract az Azure Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="29cd1-104">Use hello instructions in this article tooprepare on-premises Hyper-V hosts toointeract with Azure Site Recovery.</span></span>

<span data-ttu-id="29cd1-105">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="29cd1-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-hosts"></a><span data-ttu-id="29cd1-106">Gazdagépek előkészítése</span><span class="sxs-lookup"><span data-stu-id="29cd1-106">Prepare hosts</span></span>

- <span data-ttu-id="29cd1-107">Győződjön meg arról, hogy hello Hyper-V gazdagépek megfelelnek-e a hello [Előfeltételek](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span><span class="sxs-lookup"><span data-stu-id="29cd1-107">Make sure that hello Hyper-V hosts meet hello [prerequisites](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span></span>
- <span data-ttu-id="29cd1-108">Győződjön meg arról, hogy a hello állomások hozzáférhet-e a szükséges hello URL-címek:</span><span class="sxs-lookup"><span data-stu-id="29cd1-108">Make sure that hello hosts can access hello required URLs:</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="29cd1-109">Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="29cd1-109">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span>
- <span data-ttu-id="29cd1-110">Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.</span><span class="sxs-lookup"><span data-stu-id="29cd1-110">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span>
- <span data-ttu-id="29cd1-111">Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).</span><span class="sxs-lookup"><span data-stu-id="29cd1-111">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="29cd1-112">Site Recovery üzembe helyezése során tooreplicate tooa Hyper-V hely kívánt virtuális gépeket tartalmazó Hyper-V-gazdagépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="29cd1-112">During Site Recovery deployment, you add Hyper-V hosts that contain VMs you want tooreplicate tooa Hyper-V site.</span></span> <span data-ttu-id="29cd1-113">hello Site Recovery Provider és Recovery Services Agent ügynököt minden gazdagépen van telepítve.</span><span class="sxs-lookup"><span data-stu-id="29cd1-113">hello Site Recovery Provider, and Recovery Services agent are installed on each host.</span></span> <span data-ttu-id="29cd1-114">hello Hyper-V site Recovery Services-tároló hello regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="29cd1-114">hello Hyper-V site is registered in hello Recovery Services vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29cd1-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29cd1-115">Next steps</span></span>

<span data-ttu-id="29cd1-116">Nyissa meg túl[7. lépés: a tároló létrehozása](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="29cd1-116">Go too[Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

