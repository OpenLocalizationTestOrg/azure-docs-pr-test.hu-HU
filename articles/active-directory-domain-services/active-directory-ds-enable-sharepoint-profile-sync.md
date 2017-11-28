---
title: "Az Azure Active Directory tartományi szolgáltatások: A SharePoint felhasználói profil szolgáltatás támogatásának engedélyezése |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások felügyelt tartományok toosupport profilszinkronizálás SharePoint-kiszolgáló konfigurálása"
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
ms.openlocfilehash: 9de4f810380309e8f6436fc24412701645978f1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-managed-domain-toosupport-profile-synchronization-for-sharepoint-server"></a><span data-ttu-id="33615-103">SharePoint-kiszolgáló egy felügyelt tartomány toosupport profil szinkronizálás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="33615-103">Configure a managed domain toosupport profile synchronization for SharePoint Server</span></span>
<span data-ttu-id="33615-104">A SharePoint Server tartalmaz egy felhasználói profil szolgáltatást, a felhasználói profil szinkronizáláshoz használt.</span><span class="sxs-lookup"><span data-stu-id="33615-104">SharePoint Server includes a User Profile Service that is used for user profile synchronization.</span></span> <span data-ttu-id="33615-105">tooset mentése felhasználóiprofil-szolgáltatás hello, megfelelő engedélyeket kap az Active Directory-tartomány toobe kell.</span><span class="sxs-lookup"><span data-stu-id="33615-105">tooset up hello User Profile Service, appropriate permissions need toobe granted on an Active Directory domain.</span></span> <span data-ttu-id="33615-106">További információkért lásd: [Active Directory tartományi szolgáltatások engedélyeket kap a SharePoint Server 2013 profilszinkronizálás](https://technet.microsoft.com/library/hh296982.aspx).</span><span class="sxs-lookup"><span data-stu-id="33615-106">For more information, see [grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).</span></span>

<span data-ttu-id="33615-107">Ez a cikk azt ismerteti, hogyan konfigurálhatja az Azure AD tartományi szolgáltatások felügyelt tartományok toodeploy hello SharePoint Server felhasználóiprofil-szinkronizálás szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="33615-107">This article explains how you can configure Azure AD Domain Services managed domains toodeploy hello SharePoint Server User Profile Sync service.</span></span>

## <a name="hello-aad-dc-service-accounts-group"></a><span data-ttu-id="33615-108">hello "AAD DC-szolgáltatásfiókok" csoport</span><span class="sxs-lookup"><span data-stu-id="33615-108">hello 'AAD DC Service Accounts' group</span></span>
<span data-ttu-id="33615-109">A biztonsági csoport neve "**AAD DC szolgáltatásfiókok**" hello "Felhasználók" szervezeti egységet a felügyelt tartomány belül érhető el.</span><span class="sxs-lookup"><span data-stu-id="33615-109">A security group called '**AAD DC Service Accounts**' is available within hello 'Users' organizational unit on your managed domain.</span></span> <span data-ttu-id="33615-110">Láthatja, hogy ez a csoport hello **Active Directory – felhasználók és számítógépek** MMC beépülő modul felügyelt tartományra.</span><span class="sxs-lookup"><span data-stu-id="33615-110">You can see this group in hello **Active Directory Users and Computers** MMC snap-in on your managed domain.</span></span>

![Az AAD DC szolgáltatásfiókok biztonsági csoport](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

<span data-ttu-id="33615-112">A biztonsági csoport tagjai jogosultsággal a következő delegált hello:</span><span class="sxs-lookup"><span data-stu-id="33615-112">Members of this security group are delegated hello following privileges:</span></span>
- <span data-ttu-id="33615-113">hello "Directory változások replikálása" jogosultság hello legfelső szintű DSE hello a felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="33615-113">hello 'Replicate Directory Changes' privilege on hello root DSE of hello managed domain.</span></span>
- <span data-ttu-id="33615-114">hello hello konfigurációelnevezési környezetében "Directory változások replikálása" jogosultság (cn = konfigurációtároló) hello a felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="33615-114">hello 'Replicate Directory Changes' privilege on hello Configuration naming context (cn=configuration container) of hello managed domain.</span></span>

<span data-ttu-id="33615-115">Ez a biztonsági csoport tagja is hello beépített csoport **Windows 2000 előtti rendszerrel kompatibilis hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="33615-115">This security group is also a member of hello built-in group **Pre-Windows 2000 Compatible Access**.</span></span>

![Az AAD DC szolgáltatásfiókok biztonsági csoport](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-toosupport-sharepoint-server-user-profile-sync"></a><span data-ttu-id="33615-117">A felügyelt tartományra toosupport SharePoint Server felhasználóiprofil-szinkronizálás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="33615-117">Enable your managed domain toosupport SharePoint Server user profile sync</span></span>
<span data-ttu-id="33615-118">A SharePoint felhasználói profil szinkronizálási toohello használt hello szolgáltatásfiók is hozzáadhat **AAD DC szolgáltatásfiókok** csoport.</span><span class="sxs-lookup"><span data-stu-id="33615-118">You can add hello service account used for SharePoint user profile synchronization toohello **AAD DC Service Accounts** group.</span></span> <span data-ttu-id="33615-119">Ennek eredményeképpen a hello szinkronizálási fiók lekérdezi a megfelelő jogosultságokkal tooreplicate módosítások toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="33615-119">As a result, hello synchronization account gets adequate privileges tooreplicate changes toohello directory.</span></span> <span data-ttu-id="33615-120">Ez a konfigurációs lépés lehetővé teszi, hogy a SharePoint Server felhasználói profil szinkronizálási toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="33615-120">This configuration step enables SharePoint Server user profile sync toowork correctly.</span></span>

![AAD DC szolgáltatásfiókokat - tagok hozzáadása](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD DC szolgáltatásfiókokat - tagok hozzáadása](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a><span data-ttu-id="33615-123">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="33615-123">Related Content</span></span>
* [<span data-ttu-id="33615-124">Technikai útmutató - támogatás Active Directory tartományi szolgáltatások engedélyeit a SharePoint Server 2013 profilszinkronizálás</span><span class="sxs-lookup"><span data-stu-id="33615-124">Technical Reference - Grant Active Directory Domain Services permissions for profile synchronization in SharePoint Server 2013</span></span>](https://technet.microsoft.com/library/hh296982.aspx)
