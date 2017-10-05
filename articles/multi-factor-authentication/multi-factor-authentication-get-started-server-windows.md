---
title: "Windows-hitelesítés és Azure MFA-kiszolgáló | Microsoft Docs"
description: "Ez az Azure Multi-Factor Authentication-oldal segítséget nyújt a Windows-hitelesítés és az Azure Multi-Factor Authentication-kiszolgáló telepítéséhez."
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
ms.openlocfilehash: 7e6384ea8fea686b5cad1a3bc3007252b9cfcd65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="windows-authentication-and-azure-multi-factor-authentication-server"></a><span data-ttu-id="c5c22-103">Windows-hitelesítés és Azure Multi-Factor Authentication-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="c5c22-103">Windows Authentication and Azure Multi-Factor Authentication Server</span></span>
<span data-ttu-id="c5c22-104">Az Azure Multi-Factor Authentication-kiszolgáló Windows-hitelesítés szakaszának segítségével engedélyezheti és konfigurálhatja a Windows-hitelesítést alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c5c22-104">Use the Windows Authentication section of the Azure Multi-Factor Authentication Server to enable and configure Windows authentication for applications.</span></span> <span data-ttu-id="c5c22-105">A Windows-hitelesítés beállítása előtt tartsa szem előtt az alábbi listát:</span><span class="sxs-lookup"><span data-stu-id="c5c22-105">Before you set up Windows Authentication, keep the following list in mind:</span></span>

* <span data-ttu-id="c5c22-106">Beállítás után indítsa újra a terminálszolgáltatások Azure Multi-Factor Authentication szolgáltatását a beállítás életbe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5c22-106">After setup, reboot the Azure Multi-Factor Authentication for Terminal Services to take effect.</span></span>
* <span data-ttu-id="c5c22-107">Ha az „Azure Multi-Factor Authentication felhasználói egyeztetés megkövetelése” lehetőség be van jelölve, és Ön nem szerepel a felhasználói listán, az újraindítás után nem fog tudni bejelentkezni a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="c5c22-107">If ‘Require Azure Multi-Factor Authentication user match’ is checked and you are not in the user list, you will not be able to log into the machine after reboot.</span></span>
* <span data-ttu-id="c5c22-108">A Megbízható IP-címek attól függnek, hogy az alkalmazás képes-e biztosítani az ügyfél IP-címének hitelesítését.</span><span class="sxs-lookup"><span data-stu-id="c5c22-108">Trusted IPs is dependent on whether the application can provide the client IP with the authentication.</span></span> <span data-ttu-id="c5c22-109">Jelenleg csak a Terminálszolgáltatások támogatott.</span><span class="sxs-lookup"><span data-stu-id="c5c22-109">Currently only Terminal Services is supported.</span></span>  

> [!NOTE]
> <span data-ttu-id="c5c22-110">Ez a szolgáltatás nem támogatott a Terminálszolgáltatások védelmének biztosítására Windows Server 2012 R2-n.</span><span class="sxs-lookup"><span data-stu-id="c5c22-110">This feature is not supported to secure Terminal Services on Windows Server 2012 R2.</span></span>

## <a name="to-secure-an-application-with-windows-authentication-use-the-following-procedure"></a><span data-ttu-id="c5c22-111">Ha egy alkalmazás védelmét Windows-hitelesítéssel szeretné biztosítani, kövesse az alábbi eljárást.</span><span class="sxs-lookup"><span data-stu-id="c5c22-111">To secure an application with Windows Authentication, use the following procedure.</span></span>
1. <span data-ttu-id="c5c22-112">Az Azure Multi-Factor Authentication-kiszolgálón kattintson a Windows-hitelesítés ikonra.</span><span class="sxs-lookup"><span data-stu-id="c5c22-112">In the Azure Multi-Factor Authentication Server click the Windows Authentication icon.</span></span>
   <span data-ttu-id="c5c22-113">![Windows-hitelesítés](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span><span class="sxs-lookup"><span data-stu-id="c5c22-113">![Windows Authentication](./media/multi-factor-authentication-get-started-server-windows/windowsauth.png)</span></span>
2. <span data-ttu-id="c5c22-114">Jelölje be a **Windows-hitelesítés engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c5c22-114">Check the **Enable Windows Authentication** checkbox.</span></span> <span data-ttu-id="c5c22-115">Alapértelmezés szerint a jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="c5c22-115">By default, this box is unchecked.</span></span>
3. <span data-ttu-id="c5c22-116">Az Alkalmazások lapon a rendszergazda konfigurálhatja egy vagy több alkalmazás esetében a Windows-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c5c22-116">The Applications tab allows the administrator to configure one or more applications for Windows Authentication.</span></span>
4. <span data-ttu-id="c5c22-117">Kiszolgáló vagy alkalmazás kiválasztása – meghatározza, hogy a kiszolgáló/alkalmazás engedélyezve van-e.</span><span class="sxs-lookup"><span data-stu-id="c5c22-117">Select a server or application – specify whether the server/application is enabled.</span></span> <span data-ttu-id="c5c22-118">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5c22-118">Click **OK**.</span></span>
5. <span data-ttu-id="c5c22-119">Kattintson a **Hozzáadás…** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5c22-119">Click **Add…**</span></span>
6. <span data-ttu-id="c5c22-120">A Megbízható IP-címek lapon beállíthatja, hogy a rendszer kihagyja az Azure Multi-Factor Authenticationt adott IP-címekről származó Windows-munkamenetek esetén.</span><span class="sxs-lookup"><span data-stu-id="c5c22-120">The Trusted IPs tab allows you to skip Azure Multi-Factor Authentication for Windows sessions originating from specific IPs.</span></span> <span data-ttu-id="c5c22-121">Ha például az alkalmazottak az alkalmazást az irodából és otthonról használják, dönthet úgy, hogy nem szeretné, ha a telefonjaik folyamatosan csörögnének az Azure Multi-Factor Authentication miatt az irodában.</span><span class="sxs-lookup"><span data-stu-id="c5c22-121">For example, if employees use the application from the office and from home, you may decide you don't want their phones ringing for Azure Multi-Factor Authentication while at the office.</span></span> <span data-ttu-id="c5c22-122">Ehhez az irodai alhálózatot Megbízható IP-címek bejegyzésként kell megadni.</span><span class="sxs-lookup"><span data-stu-id="c5c22-122">For this, you would specify the office subnet as Trusted IPs entry.</span></span>
7. <span data-ttu-id="c5c22-123">Kattintson a **Hozzáadás…** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5c22-123">Click **Add…**</span></span>
8. <span data-ttu-id="c5c22-124">Válassza az **Egyetlen IP**-cím lehetőséget, ha egyetlen IP-címet szeretne kihagyni.</span><span class="sxs-lookup"><span data-stu-id="c5c22-124">Select **Single IP** if you would like to skip a single IP address.</span></span>
9. <span data-ttu-id="c5c22-125">Válassza az **IP-címtartomány** lehetőséget, ha egy teljes IP-címtartományt szeretne kihagyni.</span><span class="sxs-lookup"><span data-stu-id="c5c22-125">Select **IP Range** if you would like to skip an entire IP range.</span></span> <span data-ttu-id="c5c22-126">Példa: 10.63.193.1-10.63.193.100.</span><span class="sxs-lookup"><span data-stu-id="c5c22-126">Example 10.63.193.1-10.63.193.100.</span></span>
10. <span data-ttu-id="c5c22-127">Válassza az **Alhálózat** lehetőséget, ha egy IP-címtartományt szeretne megadni alhálózat megjelöléssel.</span><span class="sxs-lookup"><span data-stu-id="c5c22-127">Select **Subnet** if you would like to specify a range of IPs using subnet notation.</span></span> <span data-ttu-id="c5c22-128">Adja meg az alhálózat kezdő IP-címét, és válassza ki a megfelelő hálózati maszkot a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="c5c22-128">Enter the subnet's starting IP and pick the appropriate netmask from the drop-down list.</span></span>
11. <span data-ttu-id="c5c22-129">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5c22-129">Click **OK**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5c22-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5c22-130">Next steps</span></span>

- [<span data-ttu-id="c5c22-131">Külső felektől származó VPN-készülékek konfigurálása Azure MFA-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="c5c22-131">Configure third-party VPN appliances for Azure MFA Server</span></span>](multi-factor-authentication-advanced-vpn-configurations.md)

- [<span data-ttu-id="c5c22-132">Meglévő hitelesítési infrastruktúrájának kiegészítése az Azure MFA NPS bővítményével</span><span class="sxs-lookup"><span data-stu-id="c5c22-132">Augment your existing authentication infrastructure with the NPS extension for Azure MFA</span></span>](multi-factor-authentication-nps-extension.md)
