---
title: "a Hyper-V replikáció tooAzure System Center VMM aaaPrepare |} Microsoft Docs"
description: "Ismerteti, hogyan tooprepare System Center VMM-kiszolgáló a Hyper-V replikáció tooAzure Azure Site Recovery segítségével"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 773b06afaf7d3eea1fe64f050bf3970943cf466a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-tooazure"></a><span data-ttu-id="0def1-103">6. lépés: Felkészülés a Hyper-V replikáció tooAzure VMM-kiszolgáló és a Hyper-V-gazdagépek</span><span class="sxs-lookup"><span data-stu-id="0def1-103">Step 6: Prepare VMM servers and Hyper-V hosts for Hyper-V replication tooAzure</span></span>

<span data-ttu-id="0def1-104">Beállítása után [Azure összetevők](vmm-to-azure-walkthrough-prepare-azure.md) hello központi telepítéséhez használja a cikk tooprepare helyszíni VMM-kiszolgáló és a Hyper-V gazdagépek toointeract hello utasításait az Azure Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="0def1-104">After setting up [Azure components](vmm-to-azure-walkthrough-prepare-azure.md) for hello deployment, use hello instructions in this article tooprepare on-premises VMM servers and Hyper-V hosts toointeract with Azure Site Recovery.</span></span>

<span data-ttu-id="0def1-105">A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="0def1-105">After reading this article, post any comments at hello bottom, or ask technical questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-vmm-servers"></a><span data-ttu-id="0def1-106">VMM-kiszolgáló előkészítése</span><span class="sxs-lookup"><span data-stu-id="0def1-106">Prepare VMM servers</span></span>

- <span data-ttu-id="0def1-107">Meg kell legalább egy VMM-kiszolgálót a Site Recovery replikáció (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) hello támogatási követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="0def1-107">You need at least one VMM server that meet hello support requirements for Site Recovery replication (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).</span></span>
- <span data-ttu-id="0def1-108">Győződjön meg arról, hogy előkészítése után a VMM-kiszolgáló hello [hálózatleképezés](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span><span class="sxs-lookup"><span data-stu-id="0def1-108">Make sure you've prepared hello VMM server for [network mapping](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure).</span></span>
- <span data-ttu-id="0def1-109">Győződjön meg arról, hogy hello VMM-kiszolgáló el tudja érni az URL-címek</span><span class="sxs-lookup"><span data-stu-id="0def1-109">Make sure that hello VMM server can access these URLs</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="0def1-110">Ha IP-címeken alapuló tűzfalszabályok szabály, győződjön meg arról, kommunikációs tooAzure lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="0def1-110">If you have IP address-based firewall rules, ensure they allow communication tooAzure.</span></span>
- <span data-ttu-id="0def1-111">Hello engedélyezése [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653), és a HTTPS (443) port hello.</span><span class="sxs-lookup"><span data-stu-id="0def1-111">Allow hello [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and hello HTTPS (443) port.</span></span>
- <span data-ttu-id="0def1-112">Lehetővé teszi az IP-címtartományok hello Azure-régió, az előfizetés, valamint az USA nyugati (hozzáférés-vezérlés és az Identitáskezeléshez használt).</span><span class="sxs-lookup"><span data-stu-id="0def1-112">Allow IP address ranges for hello Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="0def1-113">Site Recovery üzembe helyezése során hello Site Recovery Provider letöltése, és telepítse azt minden VMM-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="0def1-113">During Site Recovery deployment, you download hello Site Recovery Provider and install it on each VMM server.</span></span> <span data-ttu-id="0def1-114">a Recovery Services-tároló hello hello VMM-kiszolgáló regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="0def1-114">hello VMM server is registered in hello Recovery Services vault.</span></span>




## <a name="next-steps"></a><span data-ttu-id="0def1-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0def1-115">Next steps</span></span>

<span data-ttu-id="0def1-116">Nyissa meg túl[7. lépés: a tároló létrehozása](vmm-to-azure-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="0def1-116">Go too[Step 7: Create a vault](vmm-to-azure-walkthrough-create-vault.md)</span></span>

