---
title: "a VMware tooAzure replikáció a mobilitási szolgáltatás aaaInstall hello |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooinstall hello a mobilitási szolgáltatás ügynöke VMware tooAzure replikáció hello Azure Site Recovery szolgáltatásban."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d3b7bc9c4d84d13317e0b0b47adcf38e8c41d0bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-install-hello-mobility-service"></a><span data-ttu-id="4b111-103">10. lépés: Hello mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="4b111-103">Step 10: Install hello Mobility service</span></span>


<span data-ttu-id="4b111-104">Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni VMware virtuális gépek tooAzure, használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4b111-104">This article describes how tooconfigure source and target settings when replicating on-premises VMware virtual machines tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="4b111-105">hello mobilitási szolgáltatás a gépen végbemenő adatírásokat, és továbbítja őket toohello folyamatkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="4b111-105">hello Mobility service captures data writes on a machine, and forwards them toohello process server.</span></span> <span data-ttu-id="4b111-106">Azt kell telepíteni minden számítógépen, amelyet az tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4b111-106">It should be installed on each machine that you want tooreplicate tooAzure.</span></span>

<span data-ttu-id="4b111-107">Hello Site Recovery folyamat kiszolgálóról leküldéses telepítést használ, ha engedélyezve van a replikáció hello mobilitási szolgáltatás manuális telepítése, vagy a System Center Configuration Manager eszközzel.</span><span class="sxs-lookup"><span data-stu-id="4b111-107">You can install hello Mobility service manual, using a push installation from hello Site Recovery process server when replication is enabled, or use a tool System Center Configuration Manager.</span></span> <span data-ttu-id="4b111-108">Ügyfélleküldéses telepítés használatakor hello szolgáltatást, a virtuális gép hello Ha engedélyezve van a replikáció.</span><span class="sxs-lookup"><span data-stu-id="4b111-108">If you use push installation, hello service is installed on hello VM when replication is enabled.</span></span>

<span data-ttu-id="4b111-109">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4b111-109">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="4b111-110">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="4b111-110">Install manually</span></span>

1. <span data-ttu-id="4b111-111">Ellenőrizze a hello [Előfeltételek](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) manuális telepítésre.</span><span class="sxs-lookup"><span data-stu-id="4b111-111">Check hello [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="4b111-112">Hajtsa végre a [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) hello portál használatával manuális telepítésre.</span><span class="sxs-lookup"><span data-stu-id="4b111-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using hello portal.</span></span>
3. <span data-ttu-id="4b111-113">Ha jobban szeret tooinstall hello parancssorból, hajtsa végre az [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="4b111-113">If you prefer tooinstall from hello command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-hello-process-server"></a><span data-ttu-id="4b111-114">Hello folyamatkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="4b111-114">Install from hello process server</span></span>

<span data-ttu-id="4b111-115">Ha azt szeretné toopush hello mobilitási szolgáltatás telepítési hello folyamat kiszolgálóról, ha engedélyezi a virtuális gépek replikálása, egy hello folyamat server tooaccess hello virtuális gép által használt fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="4b111-115">If you want toopush hello Mobility service installation from hello process server when you enable replication for a VM, you need an account that can be used by hello process server tooaccess hello VM.</span></span> <span data-ttu-id="4b111-116">hello fiók hello ügyfélleküldéses telepítés csak szolgál.</span><span class="sxs-lookup"><span data-stu-id="4b111-116">hello account is only used for hello push installation.</span></span>

1. <span data-ttu-id="4b111-117">Rendelkeznie kell [hoztak létre fiókot](vmware-walkthrough-prepare-vmware.md) , amely leküldéses telepítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="4b111-117">You should have [created an account](vmware-walkthrough-prepare-vmware.md) that can be used for push installation.</span></span> <span data-ttu-id="4b111-118">Ezután meg hello fióknevet, amelyet a toouse az adatforrás-beállítások konfigurálásakor a Site Recovery üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="4b111-118">You then specify hello account you want toouse when you configure source settings during Site Recovery deployment.</span></span>
2. <span data-ttu-id="4b111-119">Hajtsa végre [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Ha azt szeretné, hogy toopush hello mobilitási szolgáltatás a Windows vagy Linux rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="4b111-119">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want toopush hello Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-methods"></a><span data-ttu-id="4b111-120">Más módszerrel</span><span class="sxs-lookup"><span data-stu-id="4b111-120">Other methods</span></span>

<span data-ttu-id="4b111-121">További információ [hello a mobilitási szolgáltatás a Configuration Manager telepítése](site-recovery-install-mobility-service-using-sccm.md), vagy [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="4b111-121">Learn more about [installing hello Mobility service using Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), or using [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b111-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b111-122">Next steps</span></span>

<span data-ttu-id="4b111-123">Nyissa meg túl[11. lépés: replikálás engedélyezése](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="4b111-123">Go too[Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>
