---
title: "A mobilitási szolgáltatást a fizikai kiszolgáló telepítése Azure replikációs |} Microsoft Docs"
description: "A cikkből megtudhatja, hogyan telepíthet a mobilitási szolgáltatás ügynöke fizikai kiszolgálók replikálása Azure-bA az Azure Site Recovery szolgáltatással."
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
ms.openlocfilehash: d73267d7a64221a3138af19e9a2d5dd15809b927
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="step-9-install-the-mobility-service"></a><span data-ttu-id="740c5-103">9. lépés: A mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="740c5-103">Step 9: Install the Mobility service</span></span>


<span data-ttu-id="740c5-104">Ez a cikk ismerteti, hogyan kell telepíteni a mobilitási szolgáltatás összetevőt, a helyszíni windowsos/Linuxos fizikai kiszolgálók replikálása Azure-ba, amikor használatával a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="740c5-104">This article describes how to install the Mobility service component when replicating on-premises Windows/Linux physical servers to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="740c5-105">A mobilitási szolgáltatást a gépen végbemenő adatírásokat, és továbbítja őket a folyamatkiszolgálónak.</span><span class="sxs-lookup"><span data-stu-id="740c5-105">The Mobility service captures data writes on a machine, and forwards them to the process server.</span></span> <span data-ttu-id="740c5-106">Az Azure-bA replikálni kívánt összes kiszolgálón kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="740c5-106">It should be installed on each server that you want to replicate to Azure.</span></span>

<span data-ttu-id="740c5-107">Telepítheti a mobilitási szolgáltatást manuálisan, vagy a Site Recovery folyamat kiszolgálóról ügyfélleküldéses telepítést használ, ha engedélyezve van a replikáció, vagy egy eszköz, például a System Center Configuration Manager segítségével.</span><span class="sxs-lookup"><span data-stu-id="740c5-107">You can install the Mobility service manually, or using a push installation from the Site Recovery process server when replication is enabled, or using a tool such as System Center Configuration Manager.</span></span> <span data-ttu-id="740c5-108">Ha ügyfélleküldéses telepítést használ, a szolgáltatás telepítve van a kiszolgálón replikációs engedélyezésekor.</span><span class="sxs-lookup"><span data-stu-id="740c5-108">If you use push installation, the service is installed on the server when you enable replication.</span></span>

<span data-ttu-id="740c5-109">Ebben a cikkben, vagy az alsó megjegyzések és kérdéseket küldje a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="740c5-109">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="740c5-110">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="740c5-110">Install manually</span></span>

1. <span data-ttu-id="740c5-111">Ellenőrizze a [Előfeltételek](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) manuális telepítésre.</span><span class="sxs-lookup"><span data-stu-id="740c5-111">Check the [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="740c5-112">Hajtsa végre a [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) manuális telepítésre a portál használatával.</span><span class="sxs-lookup"><span data-stu-id="740c5-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using the portal.</span></span>
3. <span data-ttu-id="740c5-113">Ha inkább a parancssorból telepíti, hajtsa végre az [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="740c5-113">If you prefer to install from the command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-the-process-server"></a><span data-ttu-id="740c5-114">A folyamatkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="740c5-114">Install from the process server</span></span>

<span data-ttu-id="740c5-115">Ha azt szeretné, majd a mobilitási szolgáltatás telepítési adatok a folyamatkiszolgálótól, ha engedélyezi a gépek replikálása, egy a gép eléréséhez használható a folyamatkiszolgáló-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="740c5-115">If you want to push the Mobility service installation from the process server when you enable replication for a machine, you need an account that can be used by the process server to access the machine.</span></span> <span data-ttu-id="740c5-116">A fiók csak szolgál a leküldéses telepítés.</span><span class="sxs-lookup"><span data-stu-id="740c5-116">The account is only used for the push installation.</span></span>

1. <span data-ttu-id="740c5-117">Ha még nem hozott létre egy fiókot, ehhez használja ezeket az irányelveket:</span><span class="sxs-lookup"><span data-stu-id="740c5-117">If you haven't created an account, do so using these guidelines:</span></span>

    - <span data-ttu-id="740c5-118">A tartományi vagy helyi fiók használható</span><span class="sxs-lookup"><span data-stu-id="740c5-118">You can use a domain or local account</span></span>
    - <span data-ttu-id="740c5-119">A Windows Ha nem használ egy olyan tartományi fiók szeretné tiltsa le a távoli felhasználói hozzáférés-vezérlés a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="740c5-119">For Windows, if you're not using a domain account, you need to disable Remote User Access control on the local machine.</span></span> <span data-ttu-id="740c5-120">Ehhez a nyilvántartásában **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, adja hozzá a DWORD bejegyzést **LocalAccountTokenFilterPolicy**, 1 értékű.</span><span class="sxs-lookup"><span data-stu-id="740c5-120">To do this, in the register under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, add the DWORD entry **LocalAccountTokenFilterPolicy**, with a value of 1.</span></span>
    - <span data-ttu-id="740c5-121">Ha azt szeretné, a beállításjegyzék-bejegyzés hozzáadása a Windows a a CLI-t, írja be:</span><span class="sxs-lookup"><span data-stu-id="740c5-121">If you want to add the registry entry for Windows from a CLI, type:</span></span>

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - <span data-ttu-id="740c5-122">A Linux a fióknak kell lennie a forráskiszolgálón Linux legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="740c5-122">For Linux, the account should be root on the source Linux server.</span></span>

2. <span data-ttu-id="740c5-123">Hajtsa végre [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Ha azt szeretné, hogy a mobilitási szolgáltatás leküldéses Windows vagy Linux rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="740c5-123">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want to push the Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-installation-methods"></a><span data-ttu-id="740c5-124">Egyéb telepítési módszerek</span><span class="sxs-lookup"><span data-stu-id="740c5-124">Other installation methods</span></span>

- <span data-ttu-id="740c5-125">[További tudnivalók](site-recovery-install-mobility-service-using-sccm.md) a mobilitási szolgáltatást a Configuration Manager telepítése</span><span class="sxs-lookup"><span data-stu-id="740c5-125">[Learn about](site-recovery-install-mobility-service-using-sccm.md) installing the Mobility service using Configuration Manager</span></span>
- <span data-ttu-id="740c5-126">[További tudnivalók](site-recovery-automate-mobility-service-install.md) telepítése az Azure Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="740c5-126">[Learn about](site-recovery-automate-mobility-service-install.md) installing with Azure Automation DSC.</span></span>


## <a name="next-steps"></a><span data-ttu-id="740c5-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="740c5-127">Next steps</span></span>

<span data-ttu-id="740c5-128">Ugrás a [10. lépés: replikálás engedélyezése](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="740c5-128">Go to [Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>
