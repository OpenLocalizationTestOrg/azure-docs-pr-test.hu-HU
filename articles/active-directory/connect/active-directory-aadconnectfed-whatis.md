---
title: "aaaAzure AD Connect és összevonási |} Microsoft Docs"
description: "Ezen a lapon az hello központi hely, az összes AD FS-műveletek, amelyek használják az Azure AD Connect dokumentációját."
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: anandy
ms.openlocfilehash: dc70206eee2296c2320712ef2ade48ccebcc912d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect és összevonás
Az összevonási konfigurálását teszi az Azure Active Directory (Azure AD) Connect lehetővé a helyszíni Active Directory összevonási szolgáltatások (AD FS) és az Azure AD. Az összevonási bejelentkezéskor a felhasználók toosign tooAzure AD-alapú szolgáltatások helyszíni jelszavukat--, és engedélyezheti a vállalati hálózat hello, anélkül, hogy tooenter a jelszavuk ismételt. Hello összevonási beállítás az AD FS használatával telepítheti az AD FS új telepítést, vagy megadhat egy meglévő telepítését egy Windows Server 2012 R2-farmban.

Ez a témakör az hello otthoni az Azure AD Connect funkciók összevonási kapcsolatos információk. Hivatkozások felsorolja a kapcsolódó témakörök tooall. Hivatkozások tooAzure AD Connect, lásd: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Az Azure AD Connect: összevonási kapcsolatos témakörök
| Témakör | Azt ismerteti, és ha tooread azt |
|:--- |:--- |
| **Az Azure AD Connect felhasználói bejelentkezés lehetőségei** | |
| [Felhasználói bejelentkezés lehetőségei ismertetése](active-directory-aadconnect-user-signin.md) |Ismerje meg a különböző felhasználói bejelentkezés lehetőségei és hatásuk a hello Azure bejelentkezés felhasználói élményt. |
| **Az AD FS telepítése az Azure AD Connect használatával** | |
| [Előfeltételek](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Tekintse meg az AD FS segítségével az Azure AD Connect sikeres telepítés előfeltételeit hello. |
| [Az AD FS-farm konfigurálása](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Új AD FS-farm telepítése az Azure AD Connect használatával. |
| [Az Azure AD használatával másodlagos bejelentkezési Azonosítóval összevonni](active-directory-aadconnect-federation-management.md#alternateid) | Másodlagos bejelentkezési azonosítóval összevonás konfigurálása  |
| **Hello AD FS konfigurációjának módosítása** | |
| [Hello megbízhatóság javítása](active-directory-aadconnect-federation-management.md#repairthetrust) |Javítás hello aktuális megbízhatósági között a helyszíni AD FS és az Office 365 vagy az Azure. |
| [Új AD FS-kiszolgáló hozzáadása](active-directory-aadconnect-federation-management.md#addadfsserver) |Bontsa ki az AD FS-farm további AD FS-kiszolgálókon a kezdeti telepítés után. |
| [Új AD FS WAP-kiszolgáló hozzáadása](active-directory-aadconnect-federation-management.md#addwapserver) |Bontsa ki az AD FS-farm további webalkalmazás-proxykiszolgálóként (WAP) kiszolgáló a kezdeti telepítés után. |
| [Adjon hozzá egy új összevont tartományt](active-directory-aadconnect-federation-management.md#addfeddomain) |Adjon hozzá egy másik tartomány toobe összevont Azure AD-val. |
| [Frissítés hello SSL-tanúsítvány](active-directory-aadconnectfed-ssl-update.md)| Frissítse az AD FS-farm hello SSL-tanúsítványa. |
| **Más összevonási konfiguráció** | |
| [Több Azure AD-példány összevonása egyetlen AD FS-példánnyal](active-directory-aadconnectfed-single-adfs-multitenant-federation.md) | Az egyetlen AD FS-farm több Azure AD összevonni| 
| [Adja hozzá az egyéni vállalati emblémát/ábra](active-directory-aadconnect-federation-management.md#customlogo) |Módosítsa a hello bejelentkezés során tapasztal élmény hello egyéni embléma hello AD FS bejelentkezési oldalára megjelenített megadásával. |
| [Adjon meg egy bejelentkezési leírást](active-directory-aadconnect-federation-management.md#addsignindescription) |Módosítsa a hello bejelentkezés leírását hello AD FS bejelentkezési oldalára. |
| [AD FS jogcím szabályok módosítása](active-directory-aadconnect-federation-management.md#modclaims) |Módosíthatja, vagy adja hozzá a jogcímszabályok az AD FS megfelelő tooAzure AD Connect-szinkronizálás konfigurálása. |


## <a name="additional-resources"></a>További források
* [Összevonását két az Azure AD egyetlen AD FS-sel](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)
* [Az AD FS üzembe helyezése az Azure-ban](active-directory-aadconnect-azure-adfs.md)
* [Magas rendelkezésre állású határokon földrajzi AD FS üzembe helyezése az Azure Traffic Manager Azure-ban](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
