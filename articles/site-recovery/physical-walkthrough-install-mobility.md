---
title: "a fizikai kiszolgáló tooAzure replikáció a mobilitási szolgáltatás aaaInstall hello |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooinstall hello a mobilitási szolgáltatás ügynöke fizikai kiszolgálók replikálása tooAzure hello Azure Site Recovery szolgáltatásban."
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
ms.openlocfilehash: 48fd2c0ffe67875ed446c8167c2ae7f90d3f537c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-install-hello-mobility-service"></a><span data-ttu-id="85730-103">9. lépés: Hello mobilitási szolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="85730-103">Step 9: Install hello Mobility service</span></span>


<span data-ttu-id="85730-104">Ez a cikk ismerteti, hogyan tooinstall hello mobilitási szolgáltatás összetevőt replikálása esetén a helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="85730-104">This article describes how tooinstall hello Mobility service component when replicating on-premises Windows/Linux physical servers tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="85730-105">hello mobilitási szolgáltatás a gépen végbemenő adatírásokat, és továbbítja őket toohello folyamatkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="85730-105">hello Mobility service captures data writes on a machine, and forwards them toohello process server.</span></span> <span data-ttu-id="85730-106">Akkor kell telepíteni az egyes kiszolgálókon, amelyet az tooreplicate tooAzure.</span><span class="sxs-lookup"><span data-stu-id="85730-106">It should be installed on each server that you want tooreplicate tooAzure.</span></span>

<span data-ttu-id="85730-107">Hello mobilitási szolgáltatást manuálisan telepítheti, vagy a leküldéses telepítéssel hello Site Recovery feldolgozni kiszolgáló, amikor a replikáció engedélyezett-e, vagy egy eszköz, például a System Center Configuration Manager segítségével.</span><span class="sxs-lookup"><span data-stu-id="85730-107">You can install hello Mobility service manually, or using a push installation from hello Site Recovery process server when replication is enabled, or using a tool such as System Center Configuration Manager.</span></span> <span data-ttu-id="85730-108">Az ügyfélleküldéses telepítés használatakor hello szolgáltatást hello kiszolgálón replikációs engedélyezésekor.</span><span class="sxs-lookup"><span data-stu-id="85730-108">If you use push installation, hello service is installed on hello server when you enable replication.</span></span>

<span data-ttu-id="85730-109">Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="85730-109">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="install-manually"></a><span data-ttu-id="85730-110">Manuális telepítés</span><span class="sxs-lookup"><span data-stu-id="85730-110">Install manually</span></span>

1. <span data-ttu-id="85730-111">Ellenőrizze a hello [Előfeltételek](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) manuális telepítésre.</span><span class="sxs-lookup"><span data-stu-id="85730-111">Check hello [prerequisites](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) for manual installation.</span></span>
2. <span data-ttu-id="85730-112">Hajtsa végre a [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) hello portál használatával manuális telepítésre.</span><span class="sxs-lookup"><span data-stu-id="85730-112">Follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) for manual installation using hello portal.</span></span>
3. <span data-ttu-id="85730-113">Ha jobban szeret tooinstall hello parancssorból, hajtsa végre az [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span><span class="sxs-lookup"><span data-stu-id="85730-113">If you prefer tooinstall from hello command line, follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).</span></span>

## <a name="install-from-hello-process-server"></a><span data-ttu-id="85730-114">Hello folyamatkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="85730-114">Install from hello process server</span></span>

<span data-ttu-id="85730-115">Ha azt szeretné toopush hello mobilitási szolgáltatás telepítési hello folyamat kiszolgálóról, ha engedélyezi a gépek replikálása, egy hello folyamat tooaccess hello kiszolgálógép által használt fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="85730-115">If you want toopush hello Mobility service installation from hello process server when you enable replication for a machine, you need an account that can be used by hello process server tooaccess hello machine.</span></span> <span data-ttu-id="85730-116">hello fiók hello ügyfélleküldéses telepítés csak szolgál.</span><span class="sxs-lookup"><span data-stu-id="85730-116">hello account is only used for hello push installation.</span></span>

1. <span data-ttu-id="85730-117">Ha még nem hozott létre egy fiókot, ehhez használja ezeket az irányelveket:</span><span class="sxs-lookup"><span data-stu-id="85730-117">If you haven't created an account, do so using these guidelines:</span></span>

    - <span data-ttu-id="85730-118">A tartományi vagy helyi fiók használható</span><span class="sxs-lookup"><span data-stu-id="85730-118">You can use a domain or local account</span></span>
    - <span data-ttu-id="85730-119">A Windows Ha nem használ egy olyan tartományi fiók kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="85730-119">For Windows, if you're not using a domain account, you need toodisable Remote User Access control on hello local machine.</span></span> <span data-ttu-id="85730-120">Ez, hello regisztrálni a toodo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, hello DWORD bejegyzés hozzáadása **LocalAccountTokenFilterPolicy**, 1 értékű.</span><span class="sxs-lookup"><span data-stu-id="85730-120">toodo this, in hello register under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, add hello DWORD entry **LocalAccountTokenFilterPolicy**, with a value of 1.</span></span>
    - <span data-ttu-id="85730-121">Ha a parancssori felület a Windows hello bejegyzés tooadd szeretne, írja be:</span><span class="sxs-lookup"><span data-stu-id="85730-121">If you want tooadd hello registry entry for Windows from a CLI, type:</span></span>

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - <span data-ttu-id="85730-122">Linux hello fióknak root forráskiszolgálón hello Linux kell lennie.</span><span class="sxs-lookup"><span data-stu-id="85730-122">For Linux, hello account should be root on hello source Linux server.</span></span>

2. <span data-ttu-id="85730-123">Hajtsa végre [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Ha azt szeretné, hogy toopush hello mobilitási szolgáltatás a Windows vagy Linux rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="85730-123">Then follow [these instructions](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) if you want toopush hello Mobility service on VMs running Windows or Linux.</span></span>

## <a name="other-installation-methods"></a><span data-ttu-id="85730-124">Egyéb telepítési módszerek</span><span class="sxs-lookup"><span data-stu-id="85730-124">Other installation methods</span></span>

- <span data-ttu-id="85730-125">[További tudnivalók](site-recovery-install-mobility-service-using-sccm.md) hello a mobilitási szolgáltatás a Configuration Manager telepítése</span><span class="sxs-lookup"><span data-stu-id="85730-125">[Learn about](site-recovery-install-mobility-service-using-sccm.md) installing hello Mobility service using Configuration Manager</span></span>
- <span data-ttu-id="85730-126">[További tudnivalók](site-recovery-automate-mobility-service-install.md) telepítése az Azure Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="85730-126">[Learn about](site-recovery-automate-mobility-service-install.md) installing with Azure Automation DSC.</span></span>


## <a name="next-steps"></a><span data-ttu-id="85730-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85730-127">Next steps</span></span>

<span data-ttu-id="85730-128">Nyissa meg túl[10. lépés: replikálás engedélyezése](physical-walkthrough-enable-replication.md)</span><span class="sxs-lookup"><span data-stu-id="85730-128">Go too[Step 10: Enable replication](physical-walkthrough-enable-replication.md)</span></span>
