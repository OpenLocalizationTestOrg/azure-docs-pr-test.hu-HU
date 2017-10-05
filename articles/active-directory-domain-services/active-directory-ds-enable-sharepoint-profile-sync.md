---
title: "Az Azure Active Directory tartományi szolgáltatások: A SharePoint felhasználói profil szolgáltatás támogatásának engedélyezése |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások felügyelt tartományok profilszinkronizálás támogatja a SharePoint-kiszolgáló konfigurálása"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c3c923b5c9cd652f0c5b0ec98c1cda740f180122
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a><span data-ttu-id="b81a7-103">Egy felügyelt tartomány profilszinkronizálás támogatja a SharePoint-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b81a7-103">Configure a managed domain to support profile synchronization for SharePoint Server</span></span>
<span data-ttu-id="b81a7-104">A SharePoint Server tartalmaz egy felhasználói profil szolgáltatást, a felhasználói profil szinkronizáláshoz használt.</span><span class="sxs-lookup"><span data-stu-id="b81a7-104">SharePoint Server includes a User Profile Service that is used for user profile synchronization.</span></span> <span data-ttu-id="b81a7-105">Állítsa be a felhasználói profil szolgáltatás, megfelelő engedélyekkel kell egy Active Directory-tartományhoz kell biztosítani.</span><span class="sxs-lookup"><span data-stu-id="b81a7-105">To set up the User Profile Service, appropriate permissions need to be granted on an Active Directory domain.</span></span> <span data-ttu-id="b81a7-106">További információkért lásd: [Active Directory tartományi szolgáltatások engedélyeket kap a SharePoint Server 2013 profilszinkronizálás](https://technet.microsoft.com/library/hh296982.aspx).</span><span class="sxs-lookup"><span data-stu-id="b81a7-106">For more information, see [grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span></span>

<span data-ttu-id="b81a7-107">Ez a cikk azt ismerteti, hogyan konfigurálhatja a felügyelt tartományok Azure AD tartományi szolgáltatásokat a SharePoint Server felhasználóiprofil-szinkronizálás szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b81a7-107">This article explains how you can configure Azure AD Domain Services managed domains to deploy the SharePoint Server User Profile Sync service.</span></span>

## <a name="the-aad-dc-service-accounts-group"></a><span data-ttu-id="b81a7-108">A "AAD DC-szolgáltatásfiókok" csoport</span><span class="sxs-lookup"><span data-stu-id="b81a7-108">The 'AAD DC Service Accounts' group</span></span>
<span data-ttu-id="b81a7-109">A biztonsági csoport neve "**AAD DC szolgáltatásfiókok**" érhető el a "Felhasználók" szervezeti egységet a felügyelt tartományon belül.</span><span class="sxs-lookup"><span data-stu-id="b81a7-109">A security group called '**AAD DC Service Accounts**' is available within the 'Users' organizational unit on your managed domain.</span></span> <span data-ttu-id="b81a7-110">Ez a csoport megjelenik a **Active Directory – felhasználók és számítógépek** MMC beépülő modul felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="b81a7-110">You can see this group in the **Active Directory Users and Computers** MMC snap-in on your managed domain.</span></span>

![Az AAD DC szolgáltatásfiókok biztonsági csoport](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

<span data-ttu-id="b81a7-112">A biztonsági csoport tagjai delegált a következő engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="b81a7-112">Members of this security group are delegated the following privileges:</span></span>
- <span data-ttu-id="b81a7-113">A legfelső szintű a felügyelt tartomány DSE "Directory változások replikálása" jogosultság.</span><span class="sxs-lookup"><span data-stu-id="b81a7-113">The 'Replicate Directory Changes' privilege on the root DSE of the managed domain.</span></span>
- <span data-ttu-id="b81a7-114">A konfigurációs névhasználati környezetében a "Directory változások replikálása" jogosultság (cn = konfigurációtároló) a felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="b81a7-114">The 'Replicate Directory Changes' privilege on the Configuration naming context (cn=configuration container) of the managed domain.</span></span>

<span data-ttu-id="b81a7-115">Ez a biztonsági csoport tagja is a beépített csoport **Windows 2000 előtti rendszerrel kompatibilis hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="b81a7-115">This security group is also a member of the built-in group **Pre-Windows 2000 Compatible Access**.</span></span>

![Az AAD DC szolgáltatásfiókok biztonsági csoport](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a><span data-ttu-id="b81a7-117">A felügyelt tartományok támogatásához a SharePoint Server felhasználóiprofil-szinkronizálás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b81a7-117">Enable your managed domain to support SharePoint Server user profile sync</span></span>
<span data-ttu-id="b81a7-118">A SharePoint felhasználói profil szinkronizálás használt fiókkal is hozzáadhat a **AAD DC szolgáltatásfiókok** csoport.</span><span class="sxs-lookup"><span data-stu-id="b81a7-118">You can add the service account used for SharePoint user profile synchronization to the **AAD DC Service Accounts** group.</span></span> <span data-ttu-id="b81a7-119">Ennek eredményeképpen a szinkronizálási fiók lekérdezi a megfelelő jogosultságokkal replikálni a módosításokat a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="b81a7-119">As a result, the synchronization account gets adequate privileges to replicate changes to the directory.</span></span> <span data-ttu-id="b81a7-120">Ez a konfigurációs lépés lehetővé teszi, hogy a SharePoint Server felhasználóiprofil-szinkronizálás megfelelő működéséhez.</span><span class="sxs-lookup"><span data-stu-id="b81a7-120">This configuration step enables SharePoint Server user profile sync to work correctly.</span></span>

![AAD DC szolgáltatásfiókokat - tagok hozzáadása](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC szolgáltatásfiókokat - tagok hozzáadása](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a><span data-ttu-id="b81a7-123">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="b81a7-123">Related Content</span></span>
* [<span data-ttu-id="b81a7-124">Technikai útmutató - támogatás Active Directory tartományi szolgáltatások engedélyeit a SharePoint Server 2013 profilszinkronizálás</span><span class="sxs-lookup"><span data-stu-id="b81a7-124">Technical Reference - Grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013</span></span>](https://technet.microsoft.com/library/hh296982.aspx)
