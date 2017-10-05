---
title: "A replikáció az Azure Hyper-V állomás (a System Center VMM nélkül) előkészítése |} Microsoft Docs"
description: "Ismerteti, hogyan készíti elő a Hyper-V-gazdagépek a replikálást az Azure Site Recovery segítségével Azure"
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
ms.openlocfilehash: f9bcaa8e55be6e8fddaf88ebc3f18f5dbb2811e4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-prepare-hyper-v-hosts-for-replication-to-azure"></a><span data-ttu-id="17a91-103">6. lépés: Hyper-V-gazdagépek előkészítése a replikálás az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="17a91-103">Step 6: Prepare Hyper-V hosts for replication to Azure</span></span>

<span data-ttu-id="17a91-104">Ebben a cikkben az utasításokat követve készítse elő a helyszíni Hyper-V gazdagépek Azure Site Recovery kommunikál.</span><span class="sxs-lookup"><span data-stu-id="17a91-104">Use the instructions in this article to prepare on-premises Hyper-V hosts to interact with Azure Site Recovery.</span></span>

<span data-ttu-id="17a91-105">A cikk elolvasása után bármely fűzhetnek alsó, vagy a műszaki kérdései a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="17a91-105">After reading this article, post any comments at the bottom, or ask technical questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="prepare-hosts"></a><span data-ttu-id="17a91-106">Gazdagépek előkészítése</span><span class="sxs-lookup"><span data-stu-id="17a91-106">Prepare hosts</span></span>

- <span data-ttu-id="17a91-107">Győződjön meg arról, hogy a Hyper-V-gazdagépek megfelelnek a [Előfeltételek](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span><span class="sxs-lookup"><span data-stu-id="17a91-107">Make sure that the Hyper-V hosts meet the [prerequisites](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-to-azure-no-vmm).</span></span>
- <span data-ttu-id="17a91-108">Győződjön meg arról, hogy a gazdagépek férhessen hozzá a szükséges URL-címek:</span><span class="sxs-lookup"><span data-stu-id="17a91-108">Make sure that the hosts can access the required URLs:</span></span>

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- <span data-ttu-id="17a91-109">Ha IP-címeken alapuló tűzfalszabályokat használ, ellenőrizze, hogy engedélyezik-e az Azure-ral való kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="17a91-109">If you have IP address-based firewall rules, ensure they allow communication to Azure.</span></span>
- <span data-ttu-id="17a91-110">Engedélyezze az [Azure-adatközpont IP-tartományait](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS-portot (443).</span><span class="sxs-lookup"><span data-stu-id="17a91-110">Allow the [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653), and the HTTPS (443) port.</span></span>
- <span data-ttu-id="17a91-111">Engedélyezze az előfizetéshez tartozó Azure-régió, illetve az USA nyugati régiójának IP-tartományait (a hozzáférés-vezérléshez és az identitáskezeléshez szükséges).</span><span class="sxs-lookup"><span data-stu-id="17a91-111">Allow IP address ranges for the Azure region of your subscription, and for West US (used for Access Control and Identity Management).</span></span>

<span data-ttu-id="17a91-112">Site Recovery üzembe helyezése során egy Hyper-V helyre replikálni kívánt virtuális gépeket tartalmazó Hyper-V-gazdagépek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="17a91-112">During Site Recovery deployment, you add Hyper-V hosts that contain VMs you want to replicate to a Hyper-V site.</span></span> <span data-ttu-id="17a91-113">A Site Recovery Provider és Recovery Services Agent ügynököt minden gazdagépen van telepítve.</span><span class="sxs-lookup"><span data-stu-id="17a91-113">The Site Recovery Provider, and Recovery Services agent are installed on each host.</span></span> <span data-ttu-id="17a91-114">A Recovery Services-tároló regisztrálva van a Hyper-V helyet.</span><span class="sxs-lookup"><span data-stu-id="17a91-114">The Hyper-V site is registered in the Recovery Services vault.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17a91-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17a91-115">Next steps</span></span>

<span data-ttu-id="17a91-116">Ugrás a [7. lépés: a tároló létrehozása](hyper-v-site-walkthrough-create-vault.md)</span><span class="sxs-lookup"><span data-stu-id="17a91-116">Go to [Step 7: Create a vault](hyper-v-site-walkthrough-create-vault.md)</span></span>

