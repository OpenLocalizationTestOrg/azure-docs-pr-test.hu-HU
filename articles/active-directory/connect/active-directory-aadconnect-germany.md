---
title: "aaaAzure AD csatlakozás a Microsoft Cloud-Németország"
description: "Az Azure AD Connect integrálja a helyszíni címtárakat az Azure Active Directoryval. Ez lehetővé teszi az Azure ad-vel integrált Office 365, az Azure és az SaaS-alkalmazásokhoz közös identitás tooprovide."
keywords: "Bevezetés tooAzure AD Connect, az Azure AD Connect áttekintése, mi az az Azure AD Connect, az active directory, Németország, fekete erdő telepítése"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 2bcb0caf-5d97-46cb-8c32-bda66cc22dad
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: f32194fa6c365614f68e5d1ddcf0dac44d223292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-in-microsoft-cloud-germany---public-preview"></a>Azure AD Connect a Microsoft Cloud németországi adatközpontjában – nyilvános előzetes verzió
## <a name="introduction"></a>Introduction (Bevezetés)
Az Azure AD Connect szinkronizálást biztosít a helyszíni Active Directory és az Azure Active Directory között.
Jelenleg számos hello forgatókönyvei [Microsoft Cloud Németország](https://www.microsoft.com/de-de/cloud/deutschland/default.aspx) hello operátort kell használnia. A Microsoft Cloud Németország használatakor figyelembe kell venni hello alábbi:

* következő URL-címeket kell megnyitni a szinkronizálási toooccur proxykiszolgáló sikeresen hello:
  
  * *.microsoftonline.de
  * *.windows.net
  * * Visszavont tanúsítványok listái (CRL-ek)
* Azure AD-címtár tooyour bejelentkezéskor hello onmicrosoft.de tartományban olyan fiókot kell használnia.
* hello a következő szolgáltatások nem érhetők el:
  * Azure AD Connect Health
  * Automatikus frissítések
 
## <a name="download"></a>Letöltés
Az Azure AD Connect letölthető hello Azure AD Connect panel hello portálon találja meg.  Használja a hello utasításokat toolocate hello Azure AD Connect panel alatt.

### <a name="hello-azure-ad-connect-blade"></a>hello Azure AD Connect panel
Miután bejelentkezett az Azure-portálon toohello, hello a következő:

1. Nyissa meg tooBrowse
2. Válassza az Azure Active Directory elemet.
3. Ezután válassza az Azure AD Connect elemet.

Hello következő kell megjelennie:

![Az Azure AD Connect panel](media/active-directory-aadconnect-germany/germany1.png)

hello következő táblázatban hello szolgáltatások hello panelen látható.

| Cím | Leírás |
| --- | --- |
| SZINKRONIZÁLÁS ÁLLAPOTA |Arról tájékoztatja, hogy a szinkronizálás engedélyezve van-e, vagy le van tiltva. |
| LEGUTÓBBI SZINKRONIZÁLÁS |hello legutóbbi sikeres szinkronizálás befejeződött. |
| ÖSSZEVONT TARTOMÁNYOK |Jelenleg konfigurált összevont tartományt hello számát jeleníti meg. |

## <a name="installation"></a>Telepítés
az Azure AD Connect tooinstall, hello dokumentáció használható [Itt](active-directory-aadconnect.md#install-azure-ad-connect).

## <a name="advanced-features-and-additional-information"></a>Speciális szolgáltatások és további információk
Ha további tájékoztatást, illetve az egyéni beállításokkal vagy speciális konfigurációkkal kapcsolatos útmutatást keres, kezdje a [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md) című cikkel.  Ezen a lapon jelenít meg adatokat és hivatkozásokat tooadditional útmutatást.

