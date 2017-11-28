---
title: "aaaWindows hitelesítés és az Azure MFA kiszolgáló |} Microsoft Docs"
description: "Ez a hello Azure multi-factor Authentication hitelesítés oldal, amely segítséget nyújt a Windows-hitelesítés és az Azure multi-factor Authentication kiszolgáló üzembe helyezése."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 19a4043f-c4ce-43c0-80e7-2548ee92cb74
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/06/2017
ms.author: kgremban
ms.reviewer: yossib
ms.custom: it-pro
ms.openlocfilehash: 0fc38fd751966bf883d4eae7c48055988922af80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a><span data-ttu-id="d694f-103">Windows-hitelesítés és Azure Multi-Factor Authentication-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="d694f-103">Windows Authentication and Azure Multi-Factor Authentication Server</span></span>
<span data-ttu-id="d694f-104">Hello Azure multi-factor Authentication kiszolgáló tooenable szakasza hello Windows-hitelesítés használatára, és az alkalmazások a Windows-hitelesítés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d694f-104">Use hello Windows Authentication section of hello Azure Multi-Factor Authentication Server tooenable and configure Windows authentication for applications.</span></span> <span data-ttu-id="d694f-105">Windows-hitelesítés beállítása előtt tartsa szem előtt a lista a következő hello:</span><span class="sxs-lookup"><span data-stu-id="d694f-105">Before you set up Windows Authentication, keep hello following list in mind:</span></span>

* <span data-ttu-id="d694f-106">A telepítő, a rendszer újraindítása után hello Azure multi-factor Authentication hitelesítés Terminálszolgáltatások tootake hatás.</span><span class="sxs-lookup"><span data-stu-id="d694f-106">After setup, reboot hello Azure Multi-Factor Authentication for Terminal Services tootake effect.</span></span>
* <span data-ttu-id="d694f-107">Ha "Azure multi-factor Authentication felhasználói egyeztetés megkövetelése" jelölőnégyzet be van jelölve, és nem hello felhasználói listán, nem lesz képes toolog hello géppé a rendszer újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="d694f-107">If ‘Require Azure Multi-Factor Authentication user match’ is checked and you are not in hello user list, you will not be able toolog into hello machine after reboot.</span></span>
* <span data-ttu-id="d694f-108">Megbízható IP-címek függ, hogy hello alkalmazás képes biztosítani a hello ügyfél IP-Címét hello hitelesítés-e.</span><span class="sxs-lookup"><span data-stu-id="d694f-108">Trusted IPs is dependent on whether hello application can provide hello client IP with hello authentication.</span></span> <span data-ttu-id="d694f-109">Jelenleg csak a Terminálszolgáltatások támogatott.</span><span class="sxs-lookup"><span data-stu-id="d694f-109">Currently only Terminal Services is supported.</span></span>  

> [!NOTE]
> <span data-ttu-id="d694f-110">Ez a funkció nem támogatott toosecure Terminálszolgáltatások a Windows Server 2012 R2 rendszeren.</span><span class="sxs-lookup"><span data-stu-id="d694f-110">This feature is not supported toosecure Terminal Services on Windows Server 2012 R2.</span></span>

## <a name="toosecure-an-application-with-windows-authentication-use-hello-following-procedure"></a><span data-ttu-id="d694f-111">az alkalmazás Windows-hitelesítést, a következő eljárás használható hello toosecure.</span><span class="sxs-lookup"><span data-stu-id="d694f-111">toosecure an application with Windows Authentication, use hello following procedure.</span></span>
1. <span data-ttu-id="d694f-112">Kattintson az Azure multi-factor Authentication kiszolgáló hello hello Windows-hitelesítés ikonra.</span><span class="sxs-lookup"><span data-stu-id="d694f-112">In hello Azure Multi-Factor Authentication Server click hello Windows Authentication icon.</span></span>
   <span data-ttu-id="d694f-113">![Windows-hitelesítés](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span><span class="sxs-lookup"><span data-stu-id="d694f-113">![Windows Authentication](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span></span>
2. <span data-ttu-id="d694f-114">Ellenőrizze a hello **Windows-hitelesítés engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d694f-114">Check hello **Enable Windows Authentication** checkbox.</span></span> <span data-ttu-id="d694f-115">Alapértelmezés szerint a jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="d694f-115">By default, this box is unchecked.</span></span>
3. <span data-ttu-id="d694f-116">hello alkalmazások lap révén hello rendszergazda tooconfigure egy vagy több alkalmazást a Windows-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d694f-116">hello Applications tab allows hello administrator tooconfigure one or more applications for Windows Authentication.</span></span>
4. <span data-ttu-id="d694f-117">Jelöljön ki egy kiszolgáló vagy az alkalmazás – adja meg, hogy a hello kiszolgáló/alkalmazás engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="d694f-117">Select a server or application – specify whether hello server/application is enabled.</span></span> <span data-ttu-id="d694f-118">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d694f-118">Click **OK**.</span></span>
5. <span data-ttu-id="d694f-119">Kattintson a **Hozzáadás…** gombra.</span><span class="sxs-lookup"><span data-stu-id="d694f-119">Click **Add…**</span></span>
6. <span data-ttu-id="d694f-120">hello megbízható IP-címek lap lehetővé teszi a megadott IP-címekről tooskip többtényezős hitelesítés a Windows Azure-munkamenetek.</span><span class="sxs-lookup"><span data-stu-id="d694f-120">hello Trusted IPs tab allows you tooskip Azure Multi-Factor Authentication for Windows sessions originating from specific IPs.</span></span> <span data-ttu-id="d694f-121">Például ha az alkalmazottak hello alkalmazás használatához hello irodából és otthonról, előfordulhat, hogy dönthet úgy, hogy az Azure multi-factor Authentication hello irodában mellőzi.</span><span class="sxs-lookup"><span data-stu-id="d694f-121">For example, if employees use hello application from hello office and from home, you may decide you don't want their phones ringing for Azure Multi-Factor Authentication while at hello office.</span></span> <span data-ttu-id="d694f-122">Ehhez a hello irodai alhálózatot megbízható IP-címek bejegyzésének kellene megadnia.</span><span class="sxs-lookup"><span data-stu-id="d694f-122">For this, you would specify hello office subnet as Trusted IPs entry.</span></span>
7. <span data-ttu-id="d694f-123">Kattintson a **Hozzáadás…** gombra.</span><span class="sxs-lookup"><span data-stu-id="d694f-123">Click **Add…**</span></span>
8. <span data-ttu-id="d694f-124">Válassza ki **egyetlen IP-cím** Ha azt szeretné, hogy tooskip egyetlen IP-címnek.</span><span class="sxs-lookup"><span data-stu-id="d694f-124">Select **Single IP** if you would like tooskip a single IP address.</span></span>
9. <span data-ttu-id="d694f-125">Válassza ki **IP-címtartomány** Ha azt szeretné, hogy tooskip egy teljes IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="d694f-125">Select **IP Range** if you would like tooskip an entire IP range.</span></span> <span data-ttu-id="d694f-126">Példa: 10.63.193.1-10.63.193.100.</span><span class="sxs-lookup"><span data-stu-id="d694f-126">Example 10.63.193.1-10.63.193.100.</span></span>
10. <span data-ttu-id="d694f-127">Válassza ki **alhálózati** Ha szeretné toospecify IP-címtartományra alhálózati jelöléssel.</span><span class="sxs-lookup"><span data-stu-id="d694f-127">Select **Subnet** if you would like toospecify a range of IPs using subnet notation.</span></span> <span data-ttu-id="d694f-128">Hello alhálózat kezdő IP-Címét adja meg, és válasszon hello hello legördülő listából válassza ki a megfelelő alhálózati maszkot.</span><span class="sxs-lookup"><span data-stu-id="d694f-128">Enter hello subnet's starting IP and pick hello appropriate netmask from hello drop-down list.</span></span>
11. <span data-ttu-id="d694f-129">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d694f-129">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d694f-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d694f-130">Next steps</span></span>

- [<span data-ttu-id="d694f-131">Külső felektől származó VPN-készülékek konfigurálása Azure MFA-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="d694f-131">Configure third-party VPN appliances for Azure MFA Server</span></span>](multi-factor-authentication-advanced-vpn-configurations.md)

- [<span data-ttu-id="d694f-132">A meglévő hitelesítési infrastruktúrát a hálózati házirend-kiszolgáló bővítmény hello kiegészítheti az Azure MFA számára</span><span class="sxs-lookup"><span data-stu-id="d694f-132">Augment your existing authentication infrastructure with hello NPS extension for Azure MFA</span></span>](multi-factor-authentication-nps-extension.md)
