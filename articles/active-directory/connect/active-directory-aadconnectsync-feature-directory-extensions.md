---
title: "Azure AD Connect szinkronizálása: címtárbővítmények |} Microsoft Docs"
description: "Ez a témakör ismerteti a hello directory böngészőbővítmények funkciója az Azure AD Connectben."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 31525ae914aa4d9e047ea1515b460a8311d5c815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect szinkronizálása: címtárbővítmények
Címtárbővítmények lehetővé teszi tooextend hello séma saját attribútumait a helyszíni Active Directoryból az Azure AD-ben. Ez a funkció lehetővé teszi toobuild LOB-alkalmazások felhasználása attribútumok helyszíni toomanage folytatja a műveletet. Ezek az attribútumok felhasználhatók, keresztül [Azure AD Graph címtárbővítmények](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) vagy [Microsoft Graph](https://graph.microsoft.io/). Megtekintheti a hello attribútumok elérhető használatával [Azure AD Graph explorer](https://graphexplorer.cloudapp.net) és [Microsoft Graph explorer](https://graphexplorer2.azurewebsites.net/) kulcsattribútumokkal.

Jelenleg nincsenek Office 365 munkaterhelés igényel, ezek az attribútumok.

Mely további attribútumokat beállíthat kívánt toosynchronize hello egyéni beállítások útvonalán hello telepítővarázslóját.
![Séma kiterjesztése varázsló](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
hello telepítési hello attribútumok, amelyek érvényes erre a következő látható:

* Felhasználók és csoportok objektumtípusok
* Egyértékű attribútumok: String, Boolean, egész, bináris
* Többértékű attribútumok: karakterlánc, a bináris

az attribútumok listájában hello az Azure AD Connect telepítése során létrehozott hello sémagyorsítótár olvasható. Ha további attribútumokkal hello Active Directory-séma már ki van bővítve, majd hello [sémát frissíteni kell](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) előtt ezeket az új attribútumokat láthatók.

Az Azure AD-objektum too100 bővítmények attribútumok legfeljebb tartalmazhat. maximális hossz hello 250 karakterből áll. Ha egy attribútum-érték hosszabb, majd a függvény egésszé csonkítja hello szinkronizálási motor.

Az Azure AD Connect telepítése során az alkalmazás regisztrálva van, ahol elérhetők ezek az attribútumok. Ez az alkalmazás hello Azure-portálon tekintheti meg.  
![Séma kiterjesztése alkalmazás](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Ezek az attribútumok elérhetők Graph keresztül:  
![Graph](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

hello attribútumok fűzve előtagként a bővítmény\_{AppClientId}\_. hello AppClientId hello értékeként az összes attribútum az az Azure AD-bérlő esetében azonos értéket tartalmaz.

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
