---
title: "Az Azure AD Privileged Identity Management adatvédelmi varázslója"
description: "Az Azure Active Directory Privileged Identity Management bővítmény, az első használatakor választhat egy biztonsági varázslót. Ez a cikk a varázsló lépéseit ismerteti."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: a53a3719-8cc7-4fc7-8164-aafca192871b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: billmath
ms.custom: pim ; H1Hack27Feb2017
ms.openlocfilehash: 260d178f3d8158411b3ad266e3b0d15edbebc722
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="using-the-security-wizard-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="b546a-104">Az Azure AD Privileged Identity Management biztonsági varázslóval</span><span class="sxs-lookup"><span data-stu-id="b546a-104">Using the security wizard in Azure AD Privileged Identity Management</span></span> 
<span data-ttu-id="b546a-105">Ha Ön az első személy a szervezeténél Azure Privileged Identity Management (PIM) futtatásához, választhat egy varázslót.</span><span class="sxs-lookup"><span data-stu-id="b546a-105">If you're the first person to run Azure Privileged Identity Management (PIM) for your organization, you will be presented with a wizard.</span></span> <span data-ttu-id="b546a-106">A varázsló segít megérteni a biztonsági kockázatok kiemelt jogosultságú identitások és a PIM használata a kockázatok csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="b546a-106">The wizard helps you understand the security risks of privileged identities and how to use PIM to reduce those risks.</span></span> <span data-ttu-id="b546a-107">Nem kell módosítaniuk a varázslóban a meglévő szerepkör-hozzárendelések, ha később szeretné végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="b546a-107">You don't need to make any changes to existing role assignments in the wizard, if you prefer to do it later.</span></span>

## <a name="what-to-expect"></a><span data-ttu-id="b546a-108">Mi várható?</span><span class="sxs-lookup"><span data-stu-id="b546a-108">What to expect</span></span>
<span data-ttu-id="b546a-109">PIM segítségével a szervezet megkezdése előtt minden szerepkör-hozzárendeléseket a rendszer állandó: a felhasználók mindig vannak ezek a szerepkörök akkor is, ha a jogosultságait jelenleg nincs szükségük.</span><span class="sxs-lookup"><span data-stu-id="b546a-109">Before your organization starts using PIM, all role assignments are permanent: the users are always in these roles even if they do not presently need their privileges.</span></span>  <span data-ttu-id="b546a-110">Az első lépés a varázsló mutatja magas szintű jogosultságokat igénylő szerepkörök listáját és a felhasználók jelenleg ezeket a szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="b546a-110">The first step of the wizard shows you a list of high-privileged roles and how many users are currently in those roles.</span></span> <span data-ttu-id="b546a-111">További információt felhasználók egy adott szerep megtekintheti, vagy több nem ismeri.</span><span class="sxs-lookup"><span data-stu-id="b546a-111">You can drill in to a particular role to learn more about users if one or more of them are unfamiliar.</span></span>

<span data-ttu-id="b546a-112">A varázsló második lépése lehetővé teszi rendszergazdai szerepkör-hozzárendelések módosítása.</span><span class="sxs-lookup"><span data-stu-id="b546a-112">The second step of the wizard gives you an opportunity to change administrator's role assignments.</span></span>  

> [!WARNING]
> <span data-ttu-id="b546a-113">Fontos, hogy rendelkezik legalább egy globális rendszergazda, és több kiemelt szerepkörű rendszergazda egy olyan szervezeti fiókkal (nem Microsoft-fiókkal).</span><span class="sxs-lookup"><span data-stu-id="b546a-113">It is important that you have at least one global administrator, and more than one privileged role administrator with an organizational account (not a Microsoft account).</span></span> <span data-ttu-id="b546a-114">Ha csak egy kiemelt szerepkörű rendszergazda, a szervezet csak akkor tudja kezelni a PIM, ha, hogy a fiókot törölték.</span><span class="sxs-lookup"><span data-stu-id="b546a-114">If there is only one privileged role administrator, the organization will not be able to manage PIM if that account is deleted.</span></span>
> <span data-ttu-id="b546a-115">Azt is vegye állandó, ha egy felhasználó Microsoft-fiókkal (fiókkal bejelentkezni a Microsoft-szolgáltatásokat, mint a Skype és Outlook.com-ot használnak) szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="b546a-115">Also, keep role assignments permanent if a user has a Microsoft account (An account they use to sign in to Microsoft services like Skype and Outlook.com).</span></span> <span data-ttu-id="b546a-116">Ha azt tervezi, az adott szerepkörhöz tartozó aktiválást megkövetelő, hogy a felhasználó lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="b546a-116">If you plan to require MFA for activation for that role, that user will be locked out.</span></span>
> 
> 

<span data-ttu-id="b546a-117">Módosításokat hajtott végre, miután a varázsló már nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b546a-117">After you have made changes, the wizard will no longer show up.</span></span> <span data-ttu-id="b546a-118">A következő használatakor, vagy egy másik kiemelt szerepkörű rendszergazda PIM, látni fogja a PIM irányítópult.</span><span class="sxs-lookup"><span data-stu-id="b546a-118">The next time you or another privileged role administrator use PIM, you will see the PIM dashboard.</span></span>  

* <span data-ttu-id="b546a-119">Ha azt szeretné, adja hozzá vagy felhasználók eltávolítása a szerepkörök vagy-hozzárendelések módosítása az állandó támogatható, további információ: [hozzáadása vagy eltávolítása a felhasználói szerepkör](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span><span class="sxs-lookup"><span data-stu-id="b546a-119">If you would like to add or remove users from roles or change assignments from permanent to eligible, read more at [how to add or remove a user's role](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span></span>
* <span data-ttu-id="b546a-120">Ha azt szeretné, a további felhasználók hozzáférésének kezelése a PIM, további információ: [hogyan kezelhetik a PIM hozzáférést](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="b546a-120">If you would like to give more users access to manage PIM, read more at [how to give access to manage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b546a-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b546a-121">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

