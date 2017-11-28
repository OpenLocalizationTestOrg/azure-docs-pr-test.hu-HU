---
title: "Azure Active Directory jellemzői bérlői intercaction |} Microsoft Docs"
description: "A bérlők teljesen független erőforrásként megismerni az Azure Active bérlői bérlők kezelése"
services: active-tenant
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 2b862b75-14df-45f2-a8ab-2a3ff1e2eb08
ms.service: active-tenant
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/27/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017;it-pro
ms.reviewer: piotrci
ms.openlocfilehash: d25d2c731034d0785bbd404ec693c4c41d913d01
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a><span data-ttu-id="66f7d-103">Hogyan több Azure Active Directory-bérlő működjön ismertetése</span><span class="sxs-lookup"><span data-stu-id="66f7d-103">Understand how multiple Azure Active Directory tenants interact</span></span>

<span data-ttu-id="66f7d-104">Az Azure Active Directory (Azure AD), minden egyes bérlő teljesen független erőforrásként: egy társ logikailag független a többi bérlő, amelyeket Ön kezel.</span><span class="sxs-lookup"><span data-stu-id="66f7d-104">In Azure Active Directory (Azure AD), each tenent is a fully independent resource: a peer that is logically independent from the other tenants that you manage.</span></span> <span data-ttu-id="66f7d-105">Nincs szülő-gyermek kapcsolat bérlők között.</span><span class="sxs-lookup"><span data-stu-id="66f7d-105">There is no parent-child relationship between tenants.</span></span> <span data-ttu-id="66f7d-106">Ez a függetlenség bérlők tartalmaz erőforrás-függetlenség, a felügyelet és a szinkronizálás függetlenségét is jelenti.</span><span class="sxs-lookup"><span data-stu-id="66f7d-106">This independence between tenants includes resource independence, administrative independence, and synchronization independence.</span></span>

## <a name="resource-independence"></a><span data-ttu-id="66f7d-107">Erőforrás-függetlenség</span><span class="sxs-lookup"><span data-stu-id="66f7d-107">Resource independence</span></span>
* <span data-ttu-id="66f7d-108">Hoz létre, vagy egy bérlői erőforrás törlése, ha nincs hatással van a valamilyen más bérlőket, a külső felhasználók részleges kivétellel erőforrás.</span><span class="sxs-lookup"><span data-stu-id="66f7d-108">If you create or delete a resource in one tenant, it has no impact on any resource in another tenant, with the partial exception of external users.</span></span> 
* <span data-ttu-id="66f7d-109">Ha a tartomány neveinek és egy bérlő használ, azt semmilyen más bérlővel nem használható.</span><span class="sxs-lookup"><span data-stu-id="66f7d-109">If you use one of your domain names with one tenant, it cannot be used with any other tenant.</span></span>

## <a name="administrative-independence"></a><span data-ttu-id="66f7d-110">Felügyeleti függetlenség</span><span class="sxs-lookup"><span data-stu-id="66f7d-110">Administrative independence</span></span>
<span data-ttu-id="66f7d-111">Ha a "Contoso" bérlő nem rendszergazda jogosultságú felhasználók létrehoz egy tesztelési bérlőn "Test", majd:</span><span class="sxs-lookup"><span data-stu-id="66f7d-111">If a non-administrative user of tenant 'Contoso' creates a test tenant 'Test,' then:</span></span>

* <span data-ttu-id="66f7d-112">Alapértelmezés szerint egy bérlői létrehozó felhasználó esetleg fel, mert egy külső felhasználó új bérlőre, és a globális rendszergazdai szerepkörrel rendelkeznek, hogy a bérlő.</span><span class="sxs-lookup"><span data-stu-id="66f7d-112">By default, the user who creates a tenant is added as an external user in that new tenant, and assigned the global administrator role in that tenant.</span></span>
* <span data-ttu-id="66f7d-113">A bérlői "Contoso" rendszergazdái nincs közvetlen rendszergazdai jogosultságai a "Teszt" bérlő rendelkezik, kivéve, ha a "Teszt" rendszergazdája kifejezetten megadja számára ezeket a jogokat.</span><span class="sxs-lookup"><span data-stu-id="66f7d-113">The administrators of tenant 'Contoso' have no direct administrative privileges to tenant 'Test,' unless an administrator of 'Test' specifically grants them these privileges.</span></span> <span data-ttu-id="66f7d-114">Azonban a "Contoso" rendszergazdái való hozzáférés szabályozásának bérlői "Teszt" Ha azok szabályozhatja, hogy a felhasználói fiók létrehozása a "Teszt".</span><span class="sxs-lookup"><span data-stu-id="66f7d-114">However, administrators of 'Contoso' can control access to tenant 'Test' if they control the user account that created 'Test.'</span></span>
* <span data-ttu-id="66f7d-115">Ha Ön hozzáadása egy egy felhasználó egy Bérlői rendszergazda szerepkör, a változás nincs hatással a felhasználó rendelkezik-e egy másik bérlői rendszergazdai szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="66f7d-115">If you add/remove an administrator role for a user in one tenant, the change does not affect the administrator roles that the user has in another tenant.</span></span>

## <a name="synchronization-independence"></a><span data-ttu-id="66f7d-116">Szinkronizálási függetlenség</span><span class="sxs-lookup"><span data-stu-id="66f7d-116">Synchronization independence</span></span>
<span data-ttu-id="66f7d-117">Beállíthatja, hogy minden Azure AD-bérlő egymástól függetlenül, hogy a következők egyetlen példányából származó adatok szinkronizálásához:</span><span class="sxs-lookup"><span data-stu-id="66f7d-117">You can configure each Azure AD tenant independently to get data synchronized from a single instance of either:</span></span>

* <span data-ttu-id="66f7d-118">Az Azure AD Connect eszközzel történő szinkronizáláshoz; egyetlen AD-erdővel.</span><span class="sxs-lookup"><span data-stu-id="66f7d-118">The Azure AD Connect tool, to synchronize data with a single AD forest.</span></span>
* <span data-ttu-id="66f7d-119">Az Azure Active bérlő összekötő a Forefront Identity Manager, szinkronizálja az adatokat egy vagy több helyszíni erdővel és/vagy nem Azure AD-adatforrásokat a.</span><span class="sxs-lookup"><span data-stu-id="66f7d-119">The Azure Active tenant Connector for Forefront Identity Manager, to synchronize data with one or more on-premises forests, and/or non-Azure AD data sources.</span></span>

## <a name="add-an-azure-ad-tenant"></a><span data-ttu-id="66f7d-120">Az Azure AD-bérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66f7d-120">Add an Azure AD tenant</span></span>
<span data-ttu-id="66f7d-121">Az Azure-portálon az Azure AD-bérlő hozzáadásához jelentkezzen be [az Azure-portálon](https://portal.azure.com) az Azure AD globális rendszergazdai fiókkal, és a bal oldali, válassza ki a **új**.</span><span class="sxs-lookup"><span data-stu-id="66f7d-121">To add an Azure AD tenant in the Azure portal, sign in to [the Azure portal](https://portal.azure.com) with an account that is an Azure AD global administrator, and, on the left, select **New**.</span></span>

> [!NOTE]
> <span data-ttu-id="66f7d-122">Egyéb Azure-erőforrásokat, eltérően a bérlők csak egy Azure-előfizetés alsóbb szintű erőforrásai.</span><span class="sxs-lookup"><span data-stu-id="66f7d-122">Unlike other Azure resources, your tenants are not child resources of an Azure subscription.</span></span> <span data-ttu-id="66f7d-123">Ha az Azure-előfizetéshez megszakítva, vagy lejárt, továbbra is elérheti a bérlő adatokat az Azure PowerShell, az Azure Graph API vagy az Office 365 felügyeleti központot.</span><span class="sxs-lookup"><span data-stu-id="66f7d-123">If your Azure subscription is canceled or expired, you can still access your tenant data using Azure PowerShell, the Azure Graph API, or the Office 365 Admin Center.</span></span> <span data-ttu-id="66f7d-124">Egy másik előfizetést is társíthat a bérlő.</span><span class="sxs-lookup"><span data-stu-id="66f7d-124">You can also associate another subscription with the tenant.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="66f7d-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66f7d-125">Next steps</span></span>
<span data-ttu-id="66f7d-126">Az Azure AD licencelési problémák és ajánlott eljárások általános áttekintést, lásd: [Mi az Azure Active bérlői licencelése?](active-directory-licensing-whatis-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="66f7d-126">For a broad overview of Azure AD licensing issues and best practices, see [What is Azure Active tenant licensing?](active-directory-licensing-whatis-azure-portal.md)</span></span>
