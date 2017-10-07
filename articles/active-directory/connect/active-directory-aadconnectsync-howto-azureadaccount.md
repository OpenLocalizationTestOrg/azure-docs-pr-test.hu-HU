---
title: "Azure AD Connect szinkronizálása: hogyan toomanage hello Azure AD szolgáltatás fiókja |} Microsoft Docs"
description: "Ez a témakör dokumentumok hogyan toorestore hello Azure AD szolgáltatás fiókja."
services: active-directory
keywords: "AADSTS70002, AADSTS50054, hogyan tooreset hello jelszavát hello Azure AD Connect szinkronizálási szolgáltatás összekötő szolgáltatás fiókja"
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 6077043a-27f1-4304-a44b-81dc46620f24
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: e563518eae173de42a1d40bb5a76e63f29f9da42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-how-toomanage-hello-azure-ad-service-account"></a>Azure AD Connect szinkronizálása: hogyan toomanage hello Azure AD szolgáltatás fiókja
hello Azure AD-összekötő által használt szolgáltatásfiók hello toobe szolgáltatás szabad kellene. Ha tooreset a hitelesítő adatok szükségesek, akkor ez a témakör értéke meg. Ha például egy globális rendszergazda által hibát hello jelszó alaphelyzetbe állítása a PowerShell használatával hello szolgáltatásfiók rendelkezik.

## <a name="reset-hello-credentials"></a>Hello hitelesítő adatok alaphelyzetbe állítása
Ha hello szolgáltatásfiók definiált hello Azure AD-összekötő nem tud kapcsolatba lépni az Azure AD tooauthentication problémák miatt, hello vissza tudja állítani.

1. Jelentkezzen be toohello az Azure AD Connect szinkronizálási kiszolgálót, és indítsa el a Powershellt.
2. Futtassa az `Add-ADSyncAADServiceAccount` parancsot.  
   ![PowerShell-parancsmag addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Adja meg Azure AD globális rendszergazda hitelesítő adatait.

Ez a parancsmag hello szolgáltatásfiók hello jelszavának alaphelyzetbe állítása és az Azure ad-ben és a szinkronizálási motor hello is frissítse.

## <a name="known-issues-these-steps-can-solve"></a>A lépések segítségével megoldható ismert problémák
Ez a szakasz a hitelesítő adatok alaphelyzetbe állítása a következőn hello Azure AD-szolgáltatásfiók által kijavított ügyfelei által jelentett hiba.

- - -
Esemény 6900  
hello kiszolgáló váratlan hibába ütközött a jelszó-módosítási értesítés feldolgozása során:  
AADSTS70002: Hiba a érvényesítéséhez hitelesítő adatokat. AADSTS50054: Régi jelszó-hitelesítéshez használt.

- - -
Esemény 659  
Hiba történt a jelszó szinkronizálása konfigurálása beolvasása. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Hiba a érvényesítéséhez hitelesítő adatokat. AADSTS50054: Régi jelszó-hitelesítéshez használt.

## <a name="next-steps"></a>Következő lépések
**Áttekintő témakör**

* [Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

