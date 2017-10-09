---
title: "Azure AD Connect szinkronizálása: változó hello AD DS-fiókja jelszavát |} Microsoft Docs"
description: "Ez a témakör a dokumentum ismerteti, hogyan tooupdate az Azure AD Connect hello Tartományi fiók jelszavát hello után módosul."
services: active-directory
keywords: "AD DS-fiókjához, az Active Directory-fiókot, a jelszót"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2707c9246446612f8d083ecde876b3b4fb2435ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-ad-ds-account-password"></a>Hello AD DS fiók jelszavának módosítása
hello AD DS-fiókjához toohello felhasználói fiókot az Azure AD Connect toocommunicate a helyszíni Active Directory által használt hivatkozik. Ha módosítja az AD DS-fiókjához hello hello jelszó, frissítenie kell az Azure AD Connect szinkronizálási szolgáltatás hello új jelszót. Ellenkező esetben szinkronizálási már nem szinkronizálható megfelelően hello hello a helyszíni Active Directory és a következő hibák hello fog megjelenni:

* Hello a Synchronization Service Managert, bármely importálási vagy exportálási művelet a helyszíni AD meghiúsul **-start-hitelesítő adatok elhagyását** hiba.

* A Windows Eseménynapló hello alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6000** és üzenet **"hello"contoso.com"frissíti a kezelőügynököt folytán toorun hello hitelesítő adatok érvénytelenek voltak"** .


## <a name="how-tooupdate-hello-synchronization-service-with-new-password-for-ad-ds-account"></a>Hogyan tooupdate hello-szinkronizálási szolgáltatás új jelszót az AD DS-fiókjához
tooupdate hello szinkronizálási szolgáltatás hello új jelszóval:

1. Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás) hello.
</br>![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)  

2. Nyissa meg toohello **összekötők** fülre.

3. Jelölje be hello **Címtárösszekötőben** , amely megfelel a toohello AD DS-fiókjához, amelynek a jelszava megváltozott.

4. A **műveletek**, jelölje be **tulajdonságok**.

5. Hello előugró párbeszédpanelen válassza ki a **tooActive Directory-erdő csatlakozás**:

6. Adjon meg új jelszót hello hello Tartományi fiók hello **jelszó** szövegmező.

7. Kattintson a **OK** toosave hello új jelszót és Bezárás hello előugró párbeszédpanelen.

8. Indítsa újra az Azure AD Connect szinkronizálási szolgáltatás a Windows szolgáltatásvezérlő hello. Ez az tooensure eltávolított minden hivatkozás toohello régi jelszót hello memória-gyorsítótárból.

## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)

* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
