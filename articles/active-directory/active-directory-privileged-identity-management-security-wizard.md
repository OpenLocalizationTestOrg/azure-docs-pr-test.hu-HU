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
# <a name="using-hello-security-wizard-in-azure-ad-privileged-identity-management"></a>Az Azure AD Privileged Identity Management hello biztonsági varázsló segítségével 
Ha a szervezet hello első személy toorun Azure Privileged Identity Management (PIM), választhat egy varázslót. hello a varázsló segít megérteni a kiemelt jogosultságú identitások hello biztonsági kockázatok és hogyan toouse PIM tooreduce kockázatok. Nem kell toomake a módosítások tooexisting szerepkör-hozzárendelések hello varázslóban, ha toodo inkább később.

## <a name="what-tooexpect"></a>Milyen tooexpect
PIM segítségével a szervezet megkezdése előtt minden szerepkör-hozzárendeléseket a rendszer állandó: hello felhasználók szerepelnek mindig ezeket a szerepköröket akkor is, ha a jogosultságait jelenleg nincs szükségük.  hello varázsló első lépése hello bemutatja, magas szintű jogosultságokat igénylő szerepkörök listáját, és hány felhasználó jelenleg ezeket a szerepköröket. Megtekintheti a tooa adott szerepkör toolearn több azokról a felhasználókról, ha egy vagy több nem ismeri.

hello varázsló második lépése hello lehetővé teszi a lehetőség toochange rendszergazda szerepkör-hozzárendelések.  

> [!WARNING]
> Fontos, hogy rendelkezik legalább egy globális rendszergazda, és több kiemelt szerepkörű rendszergazda egy olyan szervezeti fiókkal (nem Microsoft-fiókkal). Ha csak egy kiemelt szerepkörű rendszergazda, hello szervezete nem lesz képes toomanage PIM Ha, hogy a fiókot törölték.
> Azt is vegye állandó, ha egy felhasználó Microsoft-fiókkal (egy fiókkal toosign tooMicrosoft szolgáltatásokat, mint a Skype és Outlook.com-ot használnak) szerepkör-hozzárendelések. Ha azt tervezi, hogy az adott szerepkörhöz tartozó aktiválást MFA toorequire, hogy a felhasználó lesz zárolva.
> 
> 

Módosításokat hajtott végre, miután hello varázsló már nem jelennek meg. Hello, vagy egy másik kiemelt szerepkörű rendszergazda használja a PIM, amikor legközelebb látni fogja hello PIM irányítópult.  

* Ha szeretné tooadd, például vagy felhasználók eltávolítása a szerepkörök vagy -hozzárendelések módosítása az állandó tooeligible, további információ: [hogyan tooadd vagy egy felhasználói szerepkör eltávolítása](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
* Ha azt szeretné, hogy toogive további felhasználók férhetnek hozzá az toomanage PIM, további információ: [toogive hogyan érik el a PIM toomanage](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

