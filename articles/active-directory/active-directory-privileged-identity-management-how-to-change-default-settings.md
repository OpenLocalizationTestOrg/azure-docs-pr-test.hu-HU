---
title: "beállítások a aaaHow toomanage aktiválási |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange hello-alapértelmezett beállításait a kiemelt jogosultságú identitások hello Azure Active Directory Privileged Identity Management bővítmény."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: f6cbcb6a-8a89-4077-afd8-06c94a64f4aa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.openlocfilehash: 453bb6f8f8e0fd2598cb073ef86c1dd55458dc72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-role-activation-settings-in-azure-ad-privileged-identity-management"></a>Hogyan toomanage szerepkör aktiválási beállításokat az Azure AD Privileged Identity Management
A kiemelt szerepkörű rendszergazda testre szabhatja az Azure AD Privileged Identity Management (PIM) a szervezetek, beleértve a felhasználó számára a megfelelő szerepkör-hozzárendelés az aktiválás hello élmény módosítása.

## <a name="manage-hello-role-activation-settings"></a>Hello szerepkör aktiválási beállításainak kezelése
1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com) és select hello **Azure AD Privileged Identity Management** hello irányítópult alkalmazásából.
2. Válassza ki **kiemelt szerepköröket kezelése** > **beállítások** > **kiemelt szerepköröket**.
3. Válassza ki a hello szerepkört amelynek beállításait szeretné toomanage.

A hello-beállítások lapon az egyes szerepkörökhöz számos a konfigurálható beállítások. Ezek a beállítások csak a jogosult rendszergazdai, nem állandó rendszergazdák felhasználók számára is vonatkoznak.

**Aktiválás**: hello idő órában, amely a szerepkör érvényességének lejárata előtt. Ez az 1 és 72 óra közötti lehet.

**Értesítések**: dönthet úgy, hogy hello küldi e-mailek tooadmins erősítse meg, hogy a szerepkör aktiválta. Ez lehet hasznos, ha a jogosulatlan vagy illegitimate aktiválások észlelése.

**Incidens/kérelmezési jegye**: választhatja, függetlenül attól, toorequire jogosult rendszergazdák tooinclude jegy number, amikor azok a szerepkör aktiválásához. Ez akkor lehet hasznos, szerepkör-hozzáférési eseményeket végrehajtásakor.

**A multi-factor Authentication**: választhat-e toorequire felhasználók tooverify az identitásukat ahhoz, azok a szerepkörök aktiválásához többtényezős hitelesítéssel. Csak rendelkeznek tooverify a egyszer munkamenet, nem minden alkalommal, amikor azok a szerepkör aktiválásához. Nincsenek két tippek tookeep szem előtt tartva a többtényezős hitelesítés engedélyezése:

* E-mail címüket a Microsoft-fiókkal rendelkező felhasználók számára (általában @outlook.com, de nem minden esetben) az Azure MFA nem regisztrálható. Ha azt szeretné, hogy a Microsoft-fiókkal rendelkező tooassign szerepkörök toousers, kell tenni őket állandó rendszergazdák vagy a többtényezős hitelesítés letiltása adott szerepkörhöz.
* Nem tiltható le MFA kiemelt jogosultságokkal rendelkező szerepkörök az Azure AD és az Office365. Ez egy olyan biztonsági beállítás, mert ezeket a szerepköröket kell védeni:  
  
  * Alkalmazás-rendszergazda
  * Alkalmazás-Proxy kiszolgáló-rendszergazda
  * Számlázási rendszergazda  
  * Megfelelőségi rendszergazda  
  * CRM szolgáltatás-rendszergazda
  * Ügyfél kulcstároló hozzáférési jóváhagyó
  * Directory írója  
  * Exchange-rendszergazda  
  * Globális rendszergazda
  * Intune szolgáltatás-rendszergazda
  * Postaláda-rendszergazda  
  * Partner tier1 támogatása  
  * Partner tier2 támogatása  
  * Kiemelt szerepkörű rendszergazda   
  * Biztonsági rendszergazda  
  * SharePoint-rendszergazda  
  * Skype Vállalati verzió-rendszergazda  
  * Felhasználói fiók rendszergazdája  

További információ a többtényezős hitelesítés használata a PIM: [hogyan tooRequire MFA](active-directory-privileged-identity-management-how-to-require-mfa.md).

<!--PLACEHOLDER: Need an explanation of what hello temporary Global Administrator setting is for.-->

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

