---
title: "Azure Active Directory Domain Services: az Azure virtuális hálózat DNS-beállításainak frissítése | Microsoft Docs"
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
ms.openlocfilehash: c704ee189072ce8ed196d1ef0a23edd528a10025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-azure-active-directory-domain-services-preview"></a><span data-ttu-id="c7186-103">Az Azure Active Directory Domain Services engedélyezése (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="c7186-103">Enable Azure Active Directory Domain Services (Preview)</span></span>

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a><span data-ttu-id="c7186-104">4. feladat: Az Azure virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="c7186-104">Task 4: update DNS settings for the Azure virtual network</span></span>
<span data-ttu-id="c7186-105">Az előző konfigurációs feladatokban sikeresen engedélyezte az Azure Active Directory Domain Services-t a címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="c7186-105">In the preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="c7186-106">A következő feladat annak biztosítása, hogy a virtuális hálózaton lévő összes számítógép képes legyen csatlakozni ezekhez a szolgáltatásokhoz, és használhassa azokat.</span><span class="sxs-lookup"><span data-stu-id="c7186-106">The next task is to ensure that computers within the virtual network can connect and consume these services.</span></span> <span data-ttu-id="c7186-107">Ez a cikk bemutatja, hogyan frissítheti a virtuális hálózat DNS-kiszolgálójának beállításait, hogy arra a két IP-címre mutassanak, amelyeken az Azure Active Directory Domain Services elérhető a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="c7186-107">In this article, you update the DNS server settings for your virtual network to point to the two IP addresses where Azure Active Directory Domain Services is available on the virtual network.</span></span>

<span data-ttu-id="c7186-108">A DNS-kiszolgáló azon virtuális hálózatra vonatkozó beállításainak frissítéséhez, amelyiken engedélyezte az Azure Active Directory Domain Services-t, végezze el a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="c7186-108">To update the DNS server setting for the virtual network in which you have enabled Azure Active Directory Domain Services, complete the following steps:</span></span>

1. <span data-ttu-id="c7186-109">A felügyelt tartomány teljes kiépítése után az **Overview** (Áttekintés) lapon jelenik meg az elvégzendő **szükséges konfigurációs lépések** listája.</span><span class="sxs-lookup"><span data-stu-id="c7186-109">The **Overview** tab lists a set of **Required configuration steps** to be performed after your managed domain is fully provisioned.</span></span> <span data-ttu-id="c7186-110">Az első konfigurációs lépés a **virtuális hálózathoz tartozó DNS-kiszolgáló beállításainak frissítése**.</span><span class="sxs-lookup"><span data-stu-id="c7186-110">The first configuration step is **Update DNS server settings for your virtual network**.</span></span>

    ![Tartományi szolgáltatások – Áttekintés lap a teljes kiépítés után](./media/getting-started/domain-services-provisioned-overview.png)

2. <span data-ttu-id="c7186-112">A tartomány teljes kiépítését követően két IP-cím jelenik meg ezen a csempén.</span><span class="sxs-lookup"><span data-stu-id="c7186-112">When your domain is fully provisioned, two IP addresses are displayed in this tile.</span></span> <span data-ttu-id="c7186-113">Mindkét IP-cím a felügyelt tartományhoz tartozó tartományvezérlőt jelöli.</span><span class="sxs-lookup"><span data-stu-id="c7186-113">Each of these IP addresses represents a domain controller for your managed domain.</span></span>

3. <span data-ttu-id="c7186-114">Az első IP-cím vágólapra való másolásához kattintson a mellette lévő másolás gombra.</span><span class="sxs-lookup"><span data-stu-id="c7186-114">To copy the first IP address to clipboard, click the copy button next to it.</span></span> <span data-ttu-id="c7186-115">Ezt követően kattintson a **Configure DNS servers** (DNS-kiszolgálók konfigurálása) gombra.</span><span class="sxs-lookup"><span data-stu-id="c7186-115">Then click the **Configure DNS servers** button.</span></span>

4. <span data-ttu-id="c7186-116">Illessze be az első IP-címet a **DNS servers** (DNS-kiszolgálók) panelen található **Add DNS server** (DNS-kiszolgáló hozzáadása) szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="c7186-116">Paste the first IP address into the **Add DNS server** textbox in the **DNS servers** blade.</span></span> <span data-ttu-id="c7186-117">A második IP-cím másolásához görgessen vízszintesen balra, majd illessze be az **Add DNS server** (DNS-kiszolgáló hozzáadása) szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="c7186-117">Scroll horizontally to the left to copy the second IP address and paste it into the **Add DNS server** textbox.</span></span>

    ![Tartományi szolgáltatások – DNS frissítése](./media/getting-started/domain-services-update-dns.png)

5. <span data-ttu-id="c7186-119">Ha végzett, kattintson a **Save** (Mentés) elemre a virtuális hálózathoz tartozó DNS-kiszolgáló frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c7186-119">Click **Save** when you are done to update the DNS servers for the virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="c7186-120">A hálózat virtuális gépei csak újraindítás után alkalmazzák az új DNS-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="c7186-120">Virtual machines in the network only get the new DNS settings after a restart.</span></span> <span data-ttu-id="c7186-121">Ha azt szeretné, hogy azonnal alkalmazzák a frissített DNS-beállításokat, kezdeményezzen újraindítást a portálon, a PowerShellen vagy a CLI-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="c7186-121">If you need them to get the updated DNS settings right away, trigger a restart either by the portal, PowerShell, or the CLI.</span></span>
>
>

## <a name="next-step"></a><span data-ttu-id="c7186-122">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="c7186-122">Next step</span></span>
[<span data-ttu-id="c7186-123">5. feladat: Jelszavak szinkronizálásának engedélyezése az Azure Active Directory Domain Services-re</span><span class="sxs-lookup"><span data-stu-id="c7186-123">Task 5: enable password synchronization to Azure Active Directory Domain Services</span></span>](active-directory-ds-getting-started-password-sync.md)
