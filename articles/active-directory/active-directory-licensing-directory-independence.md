---
title: "az Azure Active Directory aaaCharacteristics bérlői intercaction |} Microsoft Docs"
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
ms.openlocfilehash: 57b677665c7cb4aee63f518c39d26754fe71a999
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-how-multiple-azure-active-directory-tenants-interact"></a><span data-ttu-id="ba1b7-103">Hogyan több Azure Active Directory-bérlő működjön ismertetése</span><span class="sxs-lookup"><span data-stu-id="ba1b7-103">Understand how multiple Azure Active Directory tenants interact</span></span>

<span data-ttu-id="ba1b7-104">Az Azure Active Directory (Azure AD), minden egyes bérlő teljesen független erőforrásként: logikailag független a társ hello más bérlők, amelyeket Ön kezel.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-104">In Azure Active Directory (Azure AD), each tenent is a fully independent resource: a peer that is logically independent from hello other tenants that you manage.</span></span> <span data-ttu-id="ba1b7-105">Nincs szülő-gyermek kapcsolat bérlők között.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-105">There is no parent-child relationship between tenants.</span></span> <span data-ttu-id="ba1b7-106">Ez a függetlenség bérlők tartalmaz erőforrás-függetlenség, a felügyelet és a szinkronizálás függetlenségét is jelenti.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-106">This independence between tenants includes resource independence, administrative independence, and synchronization independence.</span></span>

## <a name="resource-independence"></a><span data-ttu-id="ba1b7-107">Erőforrás-függetlenség</span><span class="sxs-lookup"><span data-stu-id="ba1b7-107">Resource independence</span></span>
* <span data-ttu-id="ba1b7-108">Hoz létre, vagy egy bérlői erőforrás törlése, ha nincs hatással van a valamilyen más bérlőket, a külső felhasználók hello részleges kivétellel erőforrás.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-108">If you create or delete a resource in one tenant, it has no impact on any resource in another tenant, with hello partial exception of external users.</span></span> 
* <span data-ttu-id="ba1b7-109">Ha a tartomány neveinek és egy bérlő használ, azt semmilyen más bérlővel nem használható.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-109">If you use one of your domain names with one tenant, it cannot be used with any other tenant.</span></span>

## <a name="administrative-independence"></a><span data-ttu-id="ba1b7-110">Felügyeleti függetlenség</span><span class="sxs-lookup"><span data-stu-id="ba1b7-110">Administrative independence</span></span>
<span data-ttu-id="ba1b7-111">Ha a "Contoso" bérlő nem rendszergazda jogosultságú felhasználók létrehoz egy tesztelési bérlőn "Test", majd:</span><span class="sxs-lookup"><span data-stu-id="ba1b7-111">If a non-administrative user of tenant 'Contoso' creates a test tenant 'Test,' then:</span></span>

* <span data-ttu-id="ba1b7-112">Alapértelmezés szerint hello létrehozó felhasználó esetleg a bérlők külső felhasználónak számít, hogy új bérlőt, és a hozzárendelt hello globális rendszergazdai szerepkör bérlőre van adva.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-112">By default, hello user who creates a tenant is added as an external user in that new tenant, and assigned hello global administrator role in that tenant.</span></span>
* <span data-ttu-id="ba1b7-113">Bérlői "Contoso" rendszergazdái hello nem lesznek közvetlen rendszergazdai jogosultságai tootenant "Teszt" rendelkeznek, kivéve, ha a "Teszt" rendszergazdája kifejezetten megadja számára ezeket a jogokat.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-113">hello administrators of tenant 'Contoso' have no direct administrative privileges tootenant 'Test,' unless an administrator of 'Test' specifically grants them these privileges.</span></span> <span data-ttu-id="ba1b7-114">Azonban a "Contoso" rendszergazdái szabályozhatja hozzáférés tootenant "Teszt" Ha irányítanak hello felhasználói fiók létrehozása a "Teszt".</span><span class="sxs-lookup"><span data-stu-id="ba1b7-114">However, administrators of 'Contoso' can control access tootenant 'Test' if they control hello user account that created 'Test.'</span></span>
* <span data-ttu-id="ba1b7-115">Ha Ön hozzáadása egy egy felhasználó egy Bérlői rendszergazda szerepkör, hello módosítása nem nem befolyásolják hello rendszergazdai szerepkörök adott hello felhasználó rendelkezik-e egy másik-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-115">If you add/remove an administrator role for a user in one tenant, hello change does not affect hello administrator roles that hello user has in another tenant.</span></span>

## <a name="synchronization-independence"></a><span data-ttu-id="ba1b7-116">Szinkronizálási függetlenség</span><span class="sxs-lookup"><span data-stu-id="ba1b7-116">Synchronization independence</span></span>
<span data-ttu-id="ba1b7-117">Beállíthatja, hogy minden Azure AD bérlői egymástól függetlenül tooget származó adatok szinkronizálásához a következők egyetlen példányából:</span><span class="sxs-lookup"><span data-stu-id="ba1b7-117">You can configure each Azure AD tenant independently tooget data synchronized from a single instance of either:</span></span>

* <span data-ttu-id="ba1b7-118">hello Azure AD Connect eszközt, egyetlen AD-erdővel rendelkező toosynchronize adatokat.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-118">hello Azure AD Connect tool, toosynchronize data with a single AD forest.</span></span>
* <span data-ttu-id="ba1b7-119">hello Azure Active bérlő összekötő a Forefront Identity Manager toosynchronize adatokat egy vagy több helyszíni erdővel és/vagy a nem Azure AD-adatforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-119">hello Azure Active tenant Connector for Forefront Identity Manager, toosynchronize data with one or more on-premises forests, and/or non-Azure AD data sources.</span></span>

## <a name="add-an-azure-ad-tenant"></a><span data-ttu-id="ba1b7-120">Az Azure AD-bérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ba1b7-120">Add an Azure AD tenant</span></span>
<span data-ttu-id="ba1b7-121">túl bejelentkezés tooadd hello Azure-portálon az Azure AD-bérlő[hello Azure-portálon](https://portal.azure.com) az Azure AD globális rendszergazdai fiókkal, és hello bal oldali, válassza ki a **új**.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-121">tooadd an Azure AD tenant in hello Azure portal, sign in too[hello Azure portal](https://portal.azure.com) with an account that is an Azure AD global administrator, and, on hello left, select **New**.</span></span>

> [!NOTE]
> <span data-ttu-id="ba1b7-122">Egyéb Azure-erőforrásokat, eltérően a bérlők csak egy Azure-előfizetés alsóbb szintű erőforrásai.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-122">Unlike other Azure resources, your tenants are not child resources of an Azure subscription.</span></span> <span data-ttu-id="ba1b7-123">Ha az Azure-előfizetéshez megszakítva, vagy lejárt, továbbra is elérheti a bérlő adatokat az Azure PowerShell, hello Azure Graph API vagy hello Office 365 felügyeleti központban.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-123">If your Azure subscription is canceled or expired, you can still access your tenant data using Azure PowerShell, hello Azure Graph API, or hello Office 365 Admin Center.</span></span> <span data-ttu-id="ba1b7-124">Hello bérlővel másik előfizetést is rendelhet.</span><span class="sxs-lookup"><span data-stu-id="ba1b7-124">You can also associate another subscription with hello tenant.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="ba1b7-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba1b7-125">Next steps</span></span>
<span data-ttu-id="ba1b7-126">Az Azure AD licencelési problémák és ajánlott eljárások általános áttekintést, lásd: [Mi az Azure Active bérlői licencelése?](active-directory-licensing-whatis-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ba1b7-126">For a broad overview of Azure AD licensing issues and best practices, see [What is Azure Active tenant licensing?](active-directory-licensing-whatis-azure-portal.md)</span></span>
