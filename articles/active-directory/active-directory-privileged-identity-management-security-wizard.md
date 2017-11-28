---
title: "aaaThe az Azure AD Privileged Identity Management adatvédelmi varázslója"
description: "hello hello Azure Active Directory Privileged Identity Management bővítmény, első használatakor választhat egy biztonsági varázslót. Ez a cikk hello varázslóval hello lépéseit írja le."
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
ms.openlocfilehash: 0b3019134d3c7cfac33b3acfcf430b4d4f67b119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-security-wizard-in-azure-ad-privileged-identity-management"></a><span data-ttu-id="5df38-104">Az Azure AD Privileged Identity Management hello biztonsági varázsló segítségével</span><span class="sxs-lookup"><span data-stu-id="5df38-104">Using hello security wizard in Azure AD Privileged Identity Management</span></span> 
<span data-ttu-id="5df38-105">Ha a szervezet hello első személy toorun Azure Privileged Identity Management (PIM), választhat egy varázslót.</span><span class="sxs-lookup"><span data-stu-id="5df38-105">If you're hello first person toorun Azure Privileged Identity Management (PIM) for your organization, you will be presented with a wizard.</span></span> <span data-ttu-id="5df38-106">hello a varázsló segít megérteni a kiemelt jogosultságú identitások hello biztonsági kockázatok és hogyan toouse PIM tooreduce kockázatok.</span><span class="sxs-lookup"><span data-stu-id="5df38-106">hello wizard helps you understand hello security risks of privileged identities and how toouse PIM tooreduce those risks.</span></span> <span data-ttu-id="5df38-107">Nem kell toomake a módosítások tooexisting szerepkör-hozzárendelések hello varázslóban, ha toodo inkább később.</span><span class="sxs-lookup"><span data-stu-id="5df38-107">You don't need toomake any changes tooexisting role assignments in hello wizard, if you prefer toodo it later.</span></span>

## <a name="what-tooexpect"></a><span data-ttu-id="5df38-108">Milyen tooexpect</span><span class="sxs-lookup"><span data-stu-id="5df38-108">What tooexpect</span></span>
<span data-ttu-id="5df38-109">PIM segítségével a szervezet megkezdése előtt minden szerepkör-hozzárendeléseket a rendszer állandó: hello felhasználók szerepelnek mindig ezeket a szerepköröket akkor is, ha a jogosultságait jelenleg nincs szükségük.</span><span class="sxs-lookup"><span data-stu-id="5df38-109">Before your organization starts using PIM, all role assignments are permanent: hello users are always in these roles even if they do not presently need their privileges.</span></span>  <span data-ttu-id="5df38-110">hello varázsló első lépése hello bemutatja, magas szintű jogosultságokat igénylő szerepkörök listáját, és hány felhasználó jelenleg ezeket a szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="5df38-110">hello first step of hello wizard shows you a list of high-privileged roles and how many users are currently in those roles.</span></span> <span data-ttu-id="5df38-111">Megtekintheti a tooa adott szerepkör toolearn több azokról a felhasználókról, ha egy vagy több nem ismeri.</span><span class="sxs-lookup"><span data-stu-id="5df38-111">You can drill in tooa particular role toolearn more about users if one or more of them are unfamiliar.</span></span>

<span data-ttu-id="5df38-112">hello varázsló második lépése hello lehetővé teszi a lehetőség toochange rendszergazda szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="5df38-112">hello second step of hello wizard gives you an opportunity toochange administrator's role assignments.</span></span>  

> [!WARNING]
> <span data-ttu-id="5df38-113">Fontos, hogy rendelkezik legalább egy globális rendszergazda, és több kiemelt szerepkörű rendszergazda egy olyan szervezeti fiókkal (nem Microsoft-fiókkal).</span><span class="sxs-lookup"><span data-stu-id="5df38-113">It is important that you have at least one global administrator, and more than one privileged role administrator with an organizational account (not a Microsoft account).</span></span> <span data-ttu-id="5df38-114">Ha csak egy kiemelt szerepkörű rendszergazda, hello szervezete nem lesz képes toomanage PIM Ha, hogy a fiókot törölték.</span><span class="sxs-lookup"><span data-stu-id="5df38-114">If there is only one privileged role administrator, hello organization will not be able toomanage PIM if that account is deleted.</span></span>
> <span data-ttu-id="5df38-115">Azt is vegye állandó, ha egy felhasználó Microsoft-fiókkal (egy fiókkal toosign tooMicrosoft szolgáltatásokat, mint a Skype és Outlook.com-ot használnak) szerepkör-hozzárendelések.</span><span class="sxs-lookup"><span data-stu-id="5df38-115">Also, keep role assignments permanent if a user has a Microsoft account (An account they use toosign in tooMicrosoft services like Skype and Outlook.com).</span></span> <span data-ttu-id="5df38-116">Ha azt tervezi, hogy az adott szerepkörhöz tartozó aktiválást MFA toorequire, hogy a felhasználó lesz zárolva.</span><span class="sxs-lookup"><span data-stu-id="5df38-116">If you plan toorequire MFA for activation for that role, that user will be locked out.</span></span>
> 
> 

<span data-ttu-id="5df38-117">Módosításokat hajtott végre, miután hello varázsló már nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5df38-117">After you have made changes, hello wizard will no longer show up.</span></span> <span data-ttu-id="5df38-118">Hello, vagy egy másik kiemelt szerepkörű rendszergazda használja a PIM, amikor legközelebb látni fogja hello PIM irányítópult.</span><span class="sxs-lookup"><span data-stu-id="5df38-118">hello next time you or another privileged role administrator use PIM, you will see hello PIM dashboard.</span></span>  

* <span data-ttu-id="5df38-119">Ha szeretné tooadd, például vagy felhasználók eltávolítása a szerepkörök vagy -hozzárendelések módosítása az állandó tooeligible, további információ: [hogyan tooadd vagy egy felhasználói szerepkör eltávolítása](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span><span class="sxs-lookup"><span data-stu-id="5df38-119">If you would like tooadd or remove users from roles or change assignments from permanent tooeligible, read more at [how tooadd or remove a user's role](active-directory-privileged-identity-management-how-to-add-role-to-user.md).</span></span>
* <span data-ttu-id="5df38-120">Ha azt szeretné, hogy toogive további felhasználók férhetnek hozzá az toomanage PIM, további információ: [toogive hogyan érik el a PIM toomanage](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span><span class="sxs-lookup"><span data-stu-id="5df38-120">If you would like toogive more users access toomanage PIM, read more at [how toogive access toomanage in PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5df38-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5df38-121">Next steps</span></span>
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

