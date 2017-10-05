---
title: "Telepítse a mobilitási szolgáltatást, a VMware Azure replikációs |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan telepíteni a mobilitási szolgáltatás ügynöke, a VMware Azure replikálás az Azure Site Recovery szolgáltatással."
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
ms.openlocfilehash: bc520bd2ea54208889861a7a3b275e3008a05d53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="step-10-install-the-mobility-service"></a><span data-ttu-id="6c07a-103">10. lépés: A mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="6c07a-103">Step 10: Install the Mobility service</span></span>


<span data-ttu-id="6c07a-104">A cikk ismerteti a forrás és cél beállítások konfigurálása, ha a helyszíni VMware virtuális gépek replikálása Azure-ba, használja a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="6c07a-104">This article describes how to configure source and target settings when replicating on-premises VMware virtual machines to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="6c07a-105">A mobilitási szolgáltatást a gépen végbemenő adatírásokat, és továbbítja őket a folyamatkiszolgálónak.</span><span class="sxs-lookup"><span data-stu-id="6c07a-105">The Mobility service captures data writes on a machine, and forwards them to the process server.</span></span> <span data-ttu-id="6c07a-106">Az egyes Azure-bA replikálni kívánt gépek telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6c07a-106">It should be installed on each machine that you want to replicate to Azure.</span></span>

<span data-ttu-id="6c07a-107">A Site Recovery folyamat kiszolgálóról ügyfélleküldéses telepítést használ, ha engedélyezve van a replikáció a mobilitási szolgáltatás manuális telepítése, vagy a System Center Configuration Manager eszközzel.</span><span class="sxs-lookup"><span data-stu-id="6c07a-107">You can install the Mobility service manual, using a push installation from the Site Recovery process server when replication is enabled, or use a tool System Center Configuration Manager.</span></span> <span data-ttu-id="6c07a-108">Ha ügyfélleküldéses telepítést használ, a szolgáltatás telepítve van a virtuális gép replikációs engedélyezésekor a rendszer.</span><span class="sxs-lookup"><span data-stu-id="6c07a-108">If you use push installation, the service is installed on the VM when replication is enabled.</span></span>

<span data-ttu-id="6c07a-109">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="6c07a-109">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="6c07a-110">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="6c07a-110">Install manually</span></span>

1. <span data-ttu-id="6c07a-111">Ellenőrizze a [Előfeltételek](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) manuális telepítésre.</span><span class="sxs-lookup"><span data-stu-id="6c07a-111">Check the [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="6c07a-112">Hajtsa végre a [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) manuális telepítésre a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6c07a-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using the portal.</span></span>
3. <span data-ttu-id="6c07a-113">Ha inkább a parancssorból telepíti, hajtsa végre az [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="6c07a-113">If you prefer to install from the command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-the-process-server"></a><span data-ttu-id="6c07a-114">A folyamatkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="6c07a-114">Install from the process server</span></span>

<span data-ttu-id="6c07a-115">Ha kíván leküldeni a mobilitási szolgáltatás telepítési adatok a folyamatkiszolgálótól, ha engedélyezi a virtuális gépek replikálása, akkor egy fiókot, amelyet a virtuális gép eléréséhez a folyamatkiszolgáló által használható.</span><span class="sxs-lookup"><span data-stu-id="6c07a-115">If you want to push the Mobility service installation from the process server when you enable replication for a VM, you need an account that can be used by the process server to access the VM.</span></span> <span data-ttu-id="6c07a-116">A fiók csak szolgál a leküldéses telepítés.</span><span class="sxs-lookup"><span data-stu-id="6c07a-116">The account is only used for the push installation.</span></span>

1. <span data-ttu-id="6c07a-117">Rendelkeznie kell [hoztak létre fiókot](vmware-walkthrough-prepare-vmware.md) , amely leküldéses telepítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="6c07a-117">You should have [created an account](vmware-walkthrough-prepare-vmware.md) that can be used for push installation.</span></span> <span data-ttu-id="6c07a-118">Majd adja meg a Site Recovery üzembe helyezése során az adatforrás-beállítások konfigurálásakor használni kívánt fiókot.</span><span class="sxs-lookup"><span data-stu-id="6c07a-118">You then specify the account you want to use when you configure source settings during Site Recovery deployment.</span></span>
2. <span data-ttu-id="6c07a-119">Hajtsa végre [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Ha azt szeretné, hogy a mobilitási szolgáltatás leküldéses Windows vagy Linux rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="6c07a-119">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want to push the Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-methods"></a><span data-ttu-id="6c07a-120">Más módszerrel</span><span class="sxs-lookup"><span data-stu-id="6c07a-120">Other methods</span></span>

<span data-ttu-id="6c07a-121">További információ [a mobilitási szolgáltatást a Configuration Manager telepítése](site-recovery-install-mobility-service-using-sccm.md), vagy [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span><span class="sxs-lookup"><span data-stu-id="6c07a-121">Learn more about [installing the Mobility service using Configuration Manager](site-recovery-install-mobility-service-using-sccm.md), or using [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6c07a-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c07a-122">Next steps</span></span>

<span data-ttu-id="6c07a-123">Ugrás a [11. lépés: replikálás engedélyezése](vmware-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="6c07a-123">Go to [Step 11: Enable replication](vmware-walkthrough-enable-replication.md)</span></span>
