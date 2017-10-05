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
ms.openlocfilehash: 8bee2a25f196d645b27f30f21305b1550e44e07a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="update-dns-settings-for-the-azure-virtual-network"></a><span data-ttu-id="2d090-103">Az Azure virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="2d090-103">Update DNS settings for the Azure virtual network</span></span>
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a><span data-ttu-id="2d090-104">4. feladat: Az Azure virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="2d090-104">Task 4: Update DNS settings for the Azure virtual network</span></span>
<span data-ttu-id="2d090-105">Az előző konfigurációs feladatokban sikeresen engedélyezte az Azure Active Directory Domain Services-t a címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="2d090-105">In the preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="2d090-106">A következő feladat annak biztosítása, hogy a virtuális hálózaton lévő összes számítógép képes legyen csatlakozni ezekhez a szolgáltatásokhoz, és használhassa azokat.</span><span class="sxs-lookup"><span data-stu-id="2d090-106">The next task is to ensure that computers within the virtual network can connect and consume these services.</span></span> <span data-ttu-id="2d090-107">Ez a cikk bemutatja, hogyan frissítheti a virtuális hálózat DNS-kiszolgálójának beállításait, hogy arra a két IP-címre mutassanak, amelyeken az Azure Active Directory Domain Services elérhető a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="2d090-107">In this article, you update the DNS server settings for your virtual network to point to the two IP addresses where Azure Active Directory Domain Services is available on the virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="2d090-108">Miután engedélyezte az Azure Active Directory Domain Services-t a címtárhoz, jegyezze fel az Azure Active Directory Domain Services a címtár **Configure** (Konfigurálás) lapján megjelenített IP-címeit.</span><span class="sxs-lookup"><span data-stu-id="2d090-108">After you've enabled Azure Active Directory Domain Services for the directory, note the IP addresses for Azure Active Directory Domain Services that are displayed on the **Configure** tab of your directory.</span></span>
>
>

<span data-ttu-id="2d090-109">A DNS-kiszolgáló azon virtuális hálózatra vonatkozó beállításainak frissítéséhez, amelyiken engedélyezte az Azure Active Directory Domain Servicest, végezze el a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="2d090-109">To update the DNS server setting for the virtual network in which you have enabled Azure Active Directory Domain Services, complete the following steps:</span></span>

1. <span data-ttu-id="2d090-110">Nyissa meg a [klasszikus Azure portált](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="2d090-110">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="2d090-111">A bal oldali panelen válassza ki a **Networks** (Hálózatok) elemet.</span><span class="sxs-lookup"><span data-stu-id="2d090-111">In the left pane, select **Networks**.</span></span>  
    <span data-ttu-id="2d090-112">Ekkor megnyílik a **Networks** (Hálózatok) ablak.</span><span class="sxs-lookup"><span data-stu-id="2d090-112">The **Networks** window opens.</span></span>

    ![Virtual networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. <span data-ttu-id="2d090-114">A **Virtual Networks** (Virtuális hálózatok) lapon válassza ki a virtuális hálózatot, amelyiken engedélyezte az Azure Active Directory Domain Services-t, és tekintse meg a tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="2d090-114">On the **Virtual Networks** tab, select the virtual network in which you enabled Azure Active Directory Domain Services to view its properties.</span></span>
4. <span data-ttu-id="2d090-115">Kattintson a **Configure** (Konfigurálás) lapra.</span><span class="sxs-lookup"><span data-stu-id="2d090-115">Click the **Configure** tab.</span></span>

    ![Virtual networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. <span data-ttu-id="2d090-117">A **DNS servers** (DNS-kiszolgálók) szakaszban adja meg mindkét IP-címet, amely a címtár **Configure** (Konfigurálás) lapjának **Domain Services** (Tartományi szolgáltatások) szakaszában megjelentek.</span><span class="sxs-lookup"><span data-stu-id="2d090-117">In the **DNS servers** section, enter both of the IP addresses that were displayed in the **Domain Services** section on the **Configure** tab of your directory.</span></span>
6. <span data-ttu-id="2d090-118">A DNS-kiszolgáló virtuális hálózatra vonatkozó beállításainak mentéséhez kattintson a **Save** (Mentés) gombra az ablak alján található feladatpanelen.</span><span class="sxs-lookup"><span data-stu-id="2d090-118">To save the DNS server settings for this virtual network, in the task pane at the bottom of the window, click **Save**.</span></span>

   ![A DNS-kiszolgáló virtuális hálózatra vonatkozó beállításainak frissítése](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  <span data-ttu-id="2d090-120">A hálózat virtuális gépei csak újraindítás után alkalmazzák az új DNS-beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2d090-120">Virtual machines in the network only get the new DNS settings after a restart.</span></span> <span data-ttu-id="2d090-121">Ha azt szeretné, hogy azonnal alkalmazzák a frissített DNS-beállításokat, kezdeményezzen újraindítást a portálon, a PowerShellen vagy a CLI-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="2d090-121">If you need them to get the updated DNS settings right away, trigger a restart either by the portal, PowerShell, or the CLI.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="2d090-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2d090-122">Next steps</span></span>
<span data-ttu-id="2d090-123">5. feladat: [Jelszavak szinkronizálásának engedélyezése az Azure Active Directory Domain Services-re](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="2d090-123">Task 5: [Enable password synchronization to Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)</span></span>
