---
title: "Amikor egy adott felhasználó lesz-e az alkalmazás képes tooaccess aaaFind |} Microsoft Docs"
description: "Ha különösen fontos felhasználói kell tudni tooaccess alkalmazás kimenő toofind konfigurációjától felhasználói történő üzembe helyezéséhez az Azure AD"
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
ms.openlocfilehash: bb9520499dcc8bbbe6fae05c5238c8852815ea0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-when-a-specific-user-will-be-able-tooaccess-an-application"></a><span data-ttu-id="f473d-103">Egy adott felhasználó mindig tudja tooaccess alkalmazás megállapítása</span><span class="sxs-lookup"><span data-stu-id="f473d-103">Find out when a specific user will be able tooaccess an application</span></span>
<span data-ttu-id="f473d-104">Ha automatikus felhasználólétesítés alkalmazással, az Azure AD létesítése és a frissítés felhasználói fiókokat az alkalmazáson belüli alapján automatikusan többek között a [felhasználók és csoportok hozzárendelése](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) időközönként rendszeresen ütemezett, általában 10 percenként.</span><span class="sxs-lookup"><span data-stu-id="f473d-104">When using automatic user provisioning with an application, Azure AD automatically provision and update user accounts in an app based on things like [user and group assignment](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) at a regularly scheduled time interval, typically every 10 minutes.</span></span>

## <a name="how-long-does-it-take"></a><span data-ttu-id="f473d-105">Mennyi időt vesz igénybe?</span><span class="sxs-lookup"><span data-stu-id="f473d-105">How long does it take?</span></span>

<span data-ttu-id="f473d-106">az egy adott felhasználó toobe kiépített szükséges hello időtartamát elsősorban attól függ-e már megtörtént egy kezdeti "teljes" szinkronizálást.</span><span class="sxs-lookup"><span data-stu-id="f473d-106">hello time it takes for a given user toobe provisioned depends mainly on whether or not an initial “full” sync has already occurred.</span></span>

<span data-ttu-id="f473d-107">első szinkronizálás az Azure AD között hello és az alkalmazások is igénybe vehet 20 perc tooseveral óra, hello Azure Active directory és a felhasználók kialakítási hatókörében hello száma hello méretétől függően.</span><span class="sxs-lookup"><span data-stu-id="f473d-107">hello first sync between Azure AD and an app can take anywhere from 20 minutes tooseveral hours, depending on hello size of hello Azure AD directory and hello number of users in scope for provisioning.</span></span> 

<span data-ttu-id="f473d-108">Ezt követő szinkronizálások után hello kezdeti szinkronizálás lehet gyorsabb (pl. 10 percen belül), mivel hello szolgáltatás kiépítését, amelyek megfelelnek a hello állapot mindkét rendszer hello a kezdeti szinkronizálás ezt követő szinkronizálások teljesítményének javítása után vízjelek tárolja.</span><span class="sxs-lookup"><span data-stu-id="f473d-108">Subsequent syncs after hello initial sync be faster (e.g. within 10 minutes), as hello provisioning service stores watermarks that represent hello state of both systems after hello initial sync, improving performance of subsequent syncs.</span></span>

## <a name="how-toocheck-hello-status-of-a-user"></a><span data-ttu-id="f473d-109">Hogyan toocheck hello egy felhasználó állapota</span><span class="sxs-lookup"><span data-stu-id="f473d-109">How toocheck hello status of a user</span></span>

<span data-ttu-id="f473d-110">toosee hello kiépítési állapot a kiválasztott felhasználó, tekintse át a hello naplók az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f473d-110">toosee hello provisioning status for a selected user, consult hello Audit logs in Azure AD.</span></span>

<span data-ttu-id="f473d-111">kiépítés naplók hello hello hello Azure portálon érhetők el **Azure Active Directory &gt; vállalati alkalmazások &gt; \[alkalmazásnév\] &gt; vizsgálati naplókban**fülre. Szűrő hello bejelentkezik hello **fiók** kategória tooonly, tekintse meg az adott alkalmazáshoz események kiépítés hello.</span><span class="sxs-lookup"><span data-stu-id="f473d-111">hello provisioning audit logs can be accessed in hello Azure portal, in hello **Azure Active Directory &gt; Enterprise Apps &gt; \[Application Name\] &gt; Audit Logs** tab. Filter hello logs on hello **Account Provisioning** category tooonly see hello provisioning events for that app.</span></span> <span data-ttu-id="f473d-112">Felhasználók hello "egyező ID" attribútum-leképezésekhez hello számukra konfigurált alapján is kereshet.</span><span class="sxs-lookup"><span data-stu-id="f473d-112">You can search for users based on hello “matching ID” that was configured for them in hello attribute mappings.</span></span> 

<span data-ttu-id="f473d-113">Például, ha hello "egyszerű felhasználónév" vagy "e-mail cím" hello Azure AD oldalon attribútum megfelelő hello, konfigurálta, és nem kiépítés hello felhasználó érték "audrey@contoso.com", majd keresési hello auditnaplókat a"audrey@contoso.com", és tekintse át a majd az eredményt adott vissza.</span><span class="sxs-lookup"><span data-stu-id="f473d-113">For example, if you configured hello “user principal name” or “email address” as hello matching attribute on hello Azure AD side, and hello user not being provisioning has a value of “audrey@contoso.com”, then search hello audit logs for “audrey@contoso.com” and review then entries returned.</span></span>

<span data-ttu-id="f473d-114">kiépítés naplózási hello naplózza az összes szolgáltatás, kiépítését hello által végrehajtott műveletek hello rekord többek között:</span><span class="sxs-lookup"><span data-stu-id="f473d-114">hello provisioning audit logs record all hello operations performed by hello provisioning service, including:</span></span>

* <span data-ttu-id="f473d-115">Azure ad-val történő üzembe helyezéséhez hatókörébe hozzárendelt felhasználók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="f473d-115">Querying Azure AD for assigned users that are in scope for provisioning</span></span>
* <span data-ttu-id="f473d-116">Azoknak a felhasználóknak hello megléte hello cél alkalmazás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="f473d-116">Querying hello target app for hello existence of those users</span></span>
* <span data-ttu-id="f473d-117">Hello felhasználói objektum hello rendszer közötti összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="f473d-117">Comparing hello user objects between hello system</span></span>
* <span data-ttu-id="f473d-118">Hozzáadása, frissítése vagy hello összehasonlítás alapján hello célrendszerben hello felhasználói fiók letiltása</span><span class="sxs-lookup"><span data-stu-id="f473d-118">Adding, updating, or disabling hello user account in hello target system based on hello comparison</span></span>

## <a name="next-steps"></a><span data-ttu-id="f473d-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f473d-119">Next steps</span></span>
<span data-ttu-id="f473d-120">[Felhasználói kiépítés és megszüntetés tooSaaS alkalmazásokat az Azure Active Directoryval automatizálásához](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)".</span><span class="sxs-lookup"><span data-stu-id="f473d-120">[Automate User Provisioning and Deprovisioning tooSaaS Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning)''</span></span>
