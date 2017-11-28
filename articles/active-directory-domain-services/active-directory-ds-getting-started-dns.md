---
title: "Az Azure Active Directory tartományi szolgáltatások: Frissítse a hello Azure-beli virtuális hálózat DNS-beállítások |} Microsoft Docs"
description: "Első lépések az Azure Active Directory tartományi szolgáltatások használatával"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2017
ms.author: maheshu
ms.openlocfilehash: e6eaff555cb9b7bb89ab7581d8de0b8cfc844529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a><span data-ttu-id="a7193-103">Az Azure Active Directory Domain Services engedélyezése (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="a7193-103">Enable Azure Active Directory Domain Services (Preview)</span></span>

## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="a7193-104">4. feladat: hello Azure-beli virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="a7193-104">Task 4: update DNS settings for hello Azure virtual network</span></span>
<span data-ttu-id="a7193-105">Hello megelőző konfigurációs feladatok Azure Active Directory tartományi szolgáltatások a címtáron sikeresen engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="a7193-105">In hello preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="a7193-106">hello következő feladata, hogy hello virtuális hálózaton belüli számítógépek csatlakozhat, és ezek a szolgáltatások felhasználásához tooensure.</span><span class="sxs-lookup"><span data-stu-id="a7193-106">hello next task is tooensure that computers within hello virtual network can connect and consume these services.</span></span> <span data-ttu-id="a7193-107">Ebben a cikkben frissítenie meg a virtuális hálózati toopoint toohello két IP-címek esetén érhető el a virtuális hálózati hello Azure Active Directory tartományi szolgáltatások hello DNS-kiszolgáló beállításai.</span><span class="sxs-lookup"><span data-stu-id="a7193-107">In this article, you update hello DNS server settings for your virtual network toopoint toohello two IP addresses where Azure Active Directory Domain Services is available on hello virtual network.</span></span>

<span data-ttu-id="a7193-108">tooupdate hello DNS-kiszolgáló beállítása a hello virtuális hálózatot, amelyben engedélyezte az Azure Active Directory tartományi szolgáltatások teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a7193-108">tooupdate hello DNS server setting for hello virtual network in which you have enabled Azure Active Directory Domain Services, complete hello following steps:</span></span>

1. <span data-ttu-id="a7193-109">Hello **áttekintése** lapon számos olyan **szükséges konfigurációs lépések** toobe hajtson végre, miután a felügyelt tartományok teljesen ki van építve.</span><span class="sxs-lookup"><span data-stu-id="a7193-109">hello **Overview** tab lists a set of **Required configuration steps** toobe performed after your managed domain is fully provisioned.</span></span> <span data-ttu-id="a7193-110">hello első konfigurációs lépés **DNS-kiszolgáló beállításainak frissítése a virtuális hálózat**.</span><span class="sxs-lookup"><span data-stu-id="a7193-110">hello first configuration step is **Update DNS server settings for your virtual network**.</span></span>

    ![Tartományi szolgáltatások – Áttekintés lap a teljes kiépítés után](./media/getting-started/domain-services-provisioned-overview.png)

2. <span data-ttu-id="a7193-112">A tartomány teljes kiépítését követően két IP-cím jelenik meg ezen a csempén.</span><span class="sxs-lookup"><span data-stu-id="a7193-112">When your domain is fully provisioned, two IP addresses are displayed in this tile.</span></span> <span data-ttu-id="a7193-113">Mindkét IP-cím a felügyelt tartományhoz tartozó tartományvezérlőt jelöli.</span><span class="sxs-lookup"><span data-stu-id="a7193-113">Each of these IP addresses represents a domain controller for your managed domain.</span></span>

3. <span data-ttu-id="a7193-114">toocopy hello első IP tooclipboard cím, kattintson a Tovább tooit a hello Másolás gombra.</span><span class="sxs-lookup"><span data-stu-id="a7193-114">toocopy hello first IP address tooclipboard, click hello copy button next tooit.</span></span> <span data-ttu-id="a7193-115">Kattintson a hello **konfigurálja a DNS-kiszolgálók** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7193-115">Then click hello **Configure DNS servers** button.</span></span>

4. <span data-ttu-id="a7193-116">Illessze be a hello első IP-címet a hello **hozzáadása a DNS-kiszolgáló** textbox hello **DNS-kiszolgálók** panelen.</span><span class="sxs-lookup"><span data-stu-id="a7193-116">Paste hello first IP address into hello **Add DNS server** textbox in hello **DNS servers** blade.</span></span> <span data-ttu-id="a7193-117">Görgessen vízszintesen toohello bal oldali toocopy hello második IP-címet, majd illessze be hello **hozzáadása a DNS-kiszolgáló** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a7193-117">Scroll horizontally toohello left toocopy hello second IP address and paste it into hello **Add DNS server** textbox.</span></span>

    ![Tartományi szolgáltatások – DNS frissítése](./media/getting-started/domain-services-update-dns.png)

5. <span data-ttu-id="a7193-119">Kattintson a **mentése** Ha elkészült a tooupdate hello DNS-kiszolgálók hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="a7193-119">Click **Save** when you are done tooupdate hello DNS servers for hello virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="a7193-120">Hello hálózatban lévő virtuális gépek hello új DNS-beállítások csak get újraindítás után.</span><span class="sxs-lookup"><span data-stu-id="a7193-120">Virtual machines in hello network only get hello new DNS settings after a restart.</span></span> <span data-ttu-id="a7193-121">Ha módosítania kell azokat tooget hello frissített DNS-beállítások azonnal, indítás, akár hello portálon, a PowerShell vagy a hello CLI újraindítás.</span><span class="sxs-lookup"><span data-stu-id="a7193-121">If you need them tooget hello updated DNS settings right away, trigger a restart either by hello portal, PowerShell, or hello CLI.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="a7193-122">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="a7193-122">Next step</span></span>
[<span data-ttu-id="a7193-123">5. feladat: jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a7193-123">Task 5: enable password synchronization tooAzure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-password-sync.md)
