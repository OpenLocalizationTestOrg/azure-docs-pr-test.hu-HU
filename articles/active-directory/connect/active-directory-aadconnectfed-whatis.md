---
title: "Az Azure AD Connect és összevonási |} Microsoft Docs"
description: "Ezen a lapon az a központi hely, az Azure AD Connect használó AD FS-műveletek összes dokumentációját."
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: mtillman
editor: 
ms.assetid: f9107cf5-0131-499a-9edf-616bf3afef4d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2017
ms.author: anandy
ms.openlocfilehash: 04516e38e72405ca797a0d748d9ed825ae452966
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-and-federation"></a>Azure AD Connect és összevonás
Az összevonási konfigurálását teszi az Azure Active Directory (Azure AD) Connect lehetővé a helyszíni Active Directory összevonási szolgáltatások (AD FS) és az Azure AD. Az összevonási bejelentkezhet engedélyezheti a felhasználók jelentkezhetnek be az Azure AD-alapú szolgáltatásokhoz a helyszíni jelszavak – és a vállalati hálózaton, a jelszavuk ismételt megadása nélkül. Az AD FS összevonási beállításának használata esetén egy új telepítés Active Directory összevonási szolgáltatások telepítése, vagy megadhat egy meglévő telepítését egy Windows Server 2012 R2-farmban.

Ez a témakör az Azure AD Connect funkciók összevonási kapcsolatos információk érhető. Felsorolja az összes kapcsolódó témakörökre mutató hivatkozásokat tartalmaz. Az Azure AD Connect hivatkozások: [a helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).

## <a name="azure-ad-connect-federation-topics"></a>Az Azure AD Connect: összevonási kapcsolatos témakörök
| Témakör | Azt ismerteti, és mikor érdemes elolvasni |
|:--- |:--- |
| **Az Azure AD Connect felhasználói bejelentkezés lehetőségei** | |
| [Felhasználói bejelentkezés lehetőségei ismertetése](active-directory-aadconnect-user-signin.md) |Ismerje meg a különböző felhasználói bejelentkezés lehetőségei és az Azure bejelentkezési felhasználói élményét hatásuk. |
| **Az AD FS telepítése az Azure AD Connect használatával** | |
| [Előfeltételek](active-directory-aadconnect-get-started-custom.md#ad-fs-configuration-pre-requisites) |Tekintse meg az AD FS segítségével az Azure AD Connect sikeres telepítés előfeltételeit. |
| [Az AD FS-farm konfigurálása](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) |Új AD FS-farm telepítése az Azure AD Connect használatával. |
| [Az Azure AD használatával másodlagos bejelentkezési Azonosítóval összevonni](active-directory-aadconnect-federation-management.md#alternateid) | Másodlagos bejelentkezési azonosítóval összevonás konfigurálása  |
| **Az AD FS konfigurációjának módosítása** | |
| [Javítsa ki a bizalmi kapcsolat](active-directory-aadconnect-federation-management.md#repairthetrust) |Az aktuális megbízhatósági javítási között a helyszíni AD FS és az Office 365 vagy az Azure. |
| [Új AD FS-kiszolgáló hozzáadása](active-directory-aadconnect-federation-management.md#addadfsserver) |Bontsa ki az AD FS-farm további AD FS-kiszolgálókon a kezdeti telepítés után. |
| [Új AD FS WAP-kiszolgáló hozzáadása](active-directory-aadconnect-federation-management.md#addwapserver) |Bontsa ki az AD FS-farm további webalkalmazás-proxykiszolgálóként (WAP) kiszolgáló a kezdeti telepítés után. |
| [Adjon hozzá egy új összevont tartományt](active-directory-aadconnect-federation-management.md#addfeddomain) |Adjon hozzá egy másik tartomány átalakításakor az Azure ad-val. |
| [Az SSL-tanúsítvány frissítése](active-directory-aadconnectfed-ssl-update.md)| Az SSL-tanúsítványt az AD FS-farm frissítéséhez. |
| [Office 365 és az Azure AD összevonási tanúsítványok megújítása](active-directory-aadconnect-o365-certs.md)|Az Azure AD az Office 365 tanúsítvány megújítása.|
| **Más összevonási konfiguráció** | |
| [Több Azure AD-példány összevonása egyetlen AD FS-példánnyal](active-directory-aadconnectfed-single-adfs-multitenant-federation.md) | Az egyetlen AD FS-farm több Azure AD összevonni| 
| [Adja hozzá az egyéni vállalati emblémát/ábra](active-directory-aadconnect-federation-management.md#customlogo) |Módosítsa a bejelentkezés során tapasztal élmény jelenik meg az AD FS bejelentkezési oldalára egyéni embléma megadásával. |
| [Adjon meg egy bejelentkezési leírást](active-directory-aadconnect-federation-management.md#addsignindescription) |Módosítsa a bejelentkezés leírását az AD FS bejelentkezési oldalára. |
| [AD FS jogcím szabályok módosítása](active-directory-aadconnect-federation-management.md#modclaims) |Módosíthatja, vagy adja hozzá a jogcímszabályok az AD FS, amelyek megfelelnek az Azure AD Connect-szinkronizálás konfigurációs. |


## <a name="additional-resources"></a>További források
* [Összevonását két az Azure AD egyetlen AD FS-sel](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)
* [Az AD FS üzembe helyezése az Azure-ban](active-directory-aadconnect-azure-adfs.md)
* [Magas rendelkezésre állású határokon földrajzi AD FS üzembe helyezése az Azure Traffic Manager Azure-ban](../active-directory-adfs-in-azure-with-azure-traffic-manager.md)
