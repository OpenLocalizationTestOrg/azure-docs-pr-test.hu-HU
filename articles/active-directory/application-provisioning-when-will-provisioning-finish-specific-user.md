---
title: "Megtudhatja, ha egy adott felhasználó tudnak csatlakozni egy alkalmazáshoz |} Microsoft Docs"
description: "Ha egy különösen fontos felhasználói tudnak csatlakozni egy alkalmazáshoz, a felhasználók átadása az Azure ad szolgáltatással konfigurált megállapítása"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: fcefb31904cfb77022db0358e9feee6a0479db81
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-to-access-an-application"></a><span data-ttu-id="be0e8-103">Annak megállapítása, ha egy adott felhasználó tudnak csatlakozni egy alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="be0e8-103">Find out when a specific user will be able to access an application</span></span>
<span data-ttu-id="be0e8-104">Ha automatikus felhasználólétesítés alkalmazással, az Azure AD létesítése és a frissítés felhasználói fiókokat az alkalmazáson belüli alapján automatikusan többek között a [felhasználók és csoportok hozzárendelése](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) időközönként rendszeresen ütemezett, általában 10 percenként.</span><span class="sxs-lookup"><span data-stu-id="be0e8-104">When using automatic user provisioning with an application, Azure AD automatically provision and update user accounts in an app based on things like [user and group assignment](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) at a regularly scheduled time interval, typically every 10 minutes.</span></span>

## <a name="how-long-does-it-take"></a><span data-ttu-id="be0e8-105">Mennyi időt vesz igénybe?</span><span class="sxs-lookup"><span data-stu-id="be0e8-105">How long does it take?</span></span>

<span data-ttu-id="be0e8-106">Úgy kell létrehozni, az adott felhasználó számára szükséges idő elsősorban attól függ-e már megtörtént egy kezdeti "teljes" szinkronizálást.</span><span class="sxs-lookup"><span data-stu-id="be0e8-106">The time it takes for a given user to be provisioned depends mainly on whether or not an initial “full” sync has already occurred.</span></span>

<span data-ttu-id="be0e8-107">Az első szinkronizálás az Azure AD között és az alkalmazások is igénybe vehet 20 percet az Azure AD-címtár és a felhasználók kialakítási hatókörében számát méretétől függően több órát.</span><span class="sxs-lookup"><span data-stu-id="be0e8-107">The first sync between Azure AD and an app can take anywhere from 20 minutes to several hours, depending on the size of the Azure AD directory and the number of users in scope for provisioning.</span></span> 

<span data-ttu-id="be0e8-108">A kezdeti szinkronizálás után az ezt követő szinkronizálások gyorsabb lehet (pl. 10 percen belül), szerint a létesítési szolgáltatás, amelyben a kezdeti szinkronizálást, és ezt követő szinkronizálások teljesítményének javítása után mindkét állapota egy vízjelek tárolja.</span><span class="sxs-lookup"><span data-stu-id="be0e8-108">Subsequent syncs after the initial sync be faster (e.g. within 10 minutes), as the provisioning service stores watermarks that represent the state of both systems after the initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-to-check-the-status-of-a-user"></a><span data-ttu-id="be0e8-109">A felhasználó állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="be0e8-109">How to check the status of a user</span></span>

<span data-ttu-id="be0e8-110">A kijelölt felhasználó a kiépítési állapotát tekintheti meg, tekintse meg a felügyeleti naplók az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="be0e8-110">To see the provisioning status for a selected user, consult the Audit logs in Azure AD.</span></span>

<span data-ttu-id="be0e8-111">A telepítési naplók érhetők el az Azure portálon, a **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásnév\] &gt; vizsgálati naplókban** fülre.</span><span class="sxs-lookup"><span data-stu-id="be0e8-111">The provisioning audit logs can be accessed in the Azure portal, in the **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab.</span></span> <span data-ttu-id="be0e8-112">A naplók szűrést végezni a **fiók** kategóriát csak az adott alkalmazáshoz létesítési események megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="be0e8-112">Filter the logs on the **Account Provisioning** category to only see the provisioning events for that app.</span></span> <span data-ttu-id="be0e8-113">A "egyező ID" attribútum-leképezésekhez számukra konfigurált alapján is kereshet.</span><span class="sxs-lookup"><span data-stu-id="be0e8-113">You can search for users based on the “matching ID” that was configured for them in the attribute mappings.</span></span> 

<span data-ttu-id="be0e8-114">Például, ha az "egyszerű felhasználónév" vagy "e-mail cím" az Azure AD-oldalán a megfelelő attribútumaként konfigurálva, és a felhasználó nem kiépítés értéke "audrey@contoso.com", majd keresse meg a naplókban talál "audrey@contoso.com", és tekintse át ezt követően eredményt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="be0e8-114">For example, if you configured the “user principal name” or “email address” as the matching attribute on the Azure AD side, and the user not being provisioning has a value of “audrey@contoso.com”, then search the audit logs for “audrey@contoso.com” and review then entries returned.</span></span>

<span data-ttu-id="be0e8-115">A telepítési naplók jegyezze fel a létesítési szolgáltatás által végzett összes műveletet többek között:</span><span class="sxs-lookup"><span data-stu-id="be0e8-115">The provisioning audit logs record all the operations performed by the provisioning service, including:</span></span>

* <span data-ttu-id="be0e8-116">Azure ad-val történő üzembe helyezéséhez hatókörébe hozzárendelt felhasználók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="be0e8-116">Querying Azure AD for assigned users that are in scope for provisioning</span></span>
* <span data-ttu-id="be0e8-117">A cél alkalmazás meglétét azoknak a felhasználóknak az lekérdezése</span><span class="sxs-lookup"><span data-stu-id="be0e8-117">Querying the target app for the existence of those users</span></span>
* <span data-ttu-id="be0e8-118">A felhasználói objektum, a rendszer közötti összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="be0e8-118">Comparing the user objects between the system</span></span>
* <span data-ttu-id="be0e8-119">Hozzáadása, frissítése vagy a felhasználói fiók letiltása a célrendszeren, az összehasonlítás alapján</span><span class="sxs-lookup"><span data-stu-id="be0e8-119">Adding, updating, or disabling the user account in the target system based on the comparison</span></span>

## <a name="next-steps"></a><span data-ttu-id="be0e8-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be0e8-120">Next steps</span></span>
<span data-ttu-id="be0e8-121">[Felhasználói kiépítésének és megszüntetésének biztosítása SaaS-alkalmazásokhoz az Azure Active Directoryval történő automatizálásához](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)".</span><span class="sxs-lookup"><span data-stu-id="be0e8-121">[Automate User Provisioning and Deprovisioning to SaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span></span>
