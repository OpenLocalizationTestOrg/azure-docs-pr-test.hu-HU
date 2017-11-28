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
ms.openlocfilehash: 484ff1a197a651bccb2b416448056acf69b0d8c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="03c45-103">Hello Azure-beli virtuális hálózat DNS-beállításainak frissítése</span><span class="sxs-lookup"><span data-stu-id="03c45-103">Update DNS settings for hello Azure virtual network</span></span>
## <a name="task-4-update-dns-settings-for-hello-azure-virtual-network"></a><span data-ttu-id="03c45-104">4. feladat: Frissítése hello Azure-beli virtuális hálózat DNS-beállításait</span><span class="sxs-lookup"><span data-stu-id="03c45-104">Task 4: Update DNS settings for hello Azure virtual network</span></span>
<span data-ttu-id="03c45-105">Hello megelőző konfigurációs feladatok Azure Active Directory tartományi szolgáltatások a címtáron sikeresen engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="03c45-105">In hello preceding configuration tasks, you have successfully enabled Azure Active Directory Domain Services for your directory.</span></span> <span data-ttu-id="03c45-106">hello következő feladata, hogy hello virtuális hálózaton belüli számítógépek csatlakozhat, és ezek a szolgáltatások felhasználásához tooensure.</span><span class="sxs-lookup"><span data-stu-id="03c45-106">hello next task is tooensure that computers within hello virtual network can connect and consume these services.</span></span> <span data-ttu-id="03c45-107">Ebben a cikkben frissítenie meg a virtuális hálózati toopoint toohello két IP-címek esetén érhető el a virtuális hálózati hello Azure Active Directory tartományi szolgáltatások hello DNS-kiszolgáló beállításai.</span><span class="sxs-lookup"><span data-stu-id="03c45-107">In this article, you update hello DNS server settings for your virtual network toopoint toohello two IP addresses where Azure Active Directory Domain Services is available on hello virtual network.</span></span>

> [!NOTE]
> <span data-ttu-id="03c45-108">Miután engedélyezte az Azure Active Directory tartományi szolgáltatások hello könyvtár, vegye figyelembe hello IP-címek az Azure Active Directory tartományi szolgáltatások hello megjelenő **konfigurálása** a címtár lapján.</span><span class="sxs-lookup"><span data-stu-id="03c45-108">After you've enabled Azure Active Directory Domain Services for hello directory, note hello IP addresses for Azure Active Directory Domain Services that are displayed on hello **Configure** tab of your directory.</span></span>
>
>

<span data-ttu-id="03c45-109">tooupdate hello DNS-kiszolgáló beállítása a hello virtuális hálózatot, amelyben engedélyezte az Azure Active Directory tartományi szolgáltatások teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="03c45-109">tooupdate hello DNS server setting for hello virtual network in which you have enabled Azure Active Directory Domain Services, complete hello following steps:</span></span>

1. <span data-ttu-id="03c45-110">Nyissa meg toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="03c45-110">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="03c45-111">Hello bal oldali ablaktáblában jelöljön ki **hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="03c45-111">In hello left pane, select **Networks**.</span></span>  
    <span data-ttu-id="03c45-112">Hello **hálózatok** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="03c45-112">hello **Networks** window opens.</span></span>

    ![Virtual networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. <span data-ttu-id="03c45-114">A hello **virtuális hálózatok** lap, válassza hello virtuális hálózatot, amelyiken engedélyezte Azure Active Directory tartományi szolgáltatások tooview tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="03c45-114">On hello **Virtual Networks** tab, select hello virtual network in which you enabled Azure Active Directory Domain Services tooview its properties.</span></span>
4. <span data-ttu-id="03c45-115">Kattintson a hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="03c45-115">Click hello **Configure** tab.</span></span>

    ![Virtual networks (Virtuális hálózatok) ablak](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. <span data-ttu-id="03c45-117">A hello **DNS-kiszolgálók** területen adja meg mindkét hello IP-címek hello megjelenő **tartományi szolgáltatások** szakaszt, hello **konfigurálása** a címtár lapján.</span><span class="sxs-lookup"><span data-stu-id="03c45-117">In hello **DNS servers** section, enter both of hello IP addresses that were displayed in hello **Domain Services** section on hello **Configure** tab of your directory.</span></span>
6. <span data-ttu-id="03c45-118">toosave hello DNS-kiszolgáló beállításai a virtuális hálózat hello munkaablakban hello ablakban hello alján kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="03c45-118">toosave hello DNS server settings for this virtual network, in hello task pane at hello bottom of hello window, click **Save**.</span></span>

   ![Hello hello virtuális hálózat DNS-kiszolgáló beállításainak frissítése](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
>  <span data-ttu-id="03c45-120">Hello hálózatban lévő virtuális gépek hello új DNS-beállítások csak get újraindítás után.</span><span class="sxs-lookup"><span data-stu-id="03c45-120">Virtual machines in hello network only get hello new DNS settings after a restart.</span></span> <span data-ttu-id="03c45-121">Ha módosítania kell azokat tooget hello frissített DNS-beállítások azonnal, indítás, akár hello portálon, a PowerShell vagy a hello CLI újraindítás.</span><span class="sxs-lookup"><span data-stu-id="03c45-121">If you need them tooget hello updated DNS settings right away, trigger a restart either by hello portal, PowerShell, or hello CLI.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="03c45-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="03c45-122">Next steps</span></span>
<span data-ttu-id="03c45-123">5. feladat: [jelszó-szinkronizálás tooAzure Active Directory tartományi szolgáltatások engedélyezése](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="03c45-123">Task 5: [Enable password synchronization tooAzure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)</span></span>
