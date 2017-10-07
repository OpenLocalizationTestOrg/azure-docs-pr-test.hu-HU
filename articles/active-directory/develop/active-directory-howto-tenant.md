---
title: "az Azure AD-bérlő aaaHow tooget |} Microsoft Docs"
description: "Hogyan tooget Azure Active Directory-alapú alkalmazások regisztrálásához és fordításához a bérlői."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: dcc6b3109528cf763bda9bd527344ea9ab5c0d69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-an-azure-active-directory-tenant"></a>Hogyan tooget egy Azure Active Directory-bérlőben
Az Azure Active Directory (Azure AD) alatt a [bérlők](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) a szervezetek képviselői.  Hello Azure AD szolgáltatás, amely a szervezetek megkapnak és a tulajdonában áll, amikor regisztrálnak egy Microsoft felhőszolgáltatásra, például az Azure, a Microsoft Intune vagy az Office 365 dedikált példányát is.  Mindegyik Azure AD-bérlő önálló, és elkülönül a többi Azure AD-bérlőtől.  

A bérlő Kezelőkód hello felhasználók egy vállalati és hello információit – a jelszavak, felhasználói profil adatainak, engedélyek és így tovább.  Csoportok, alkalmazások és tooan szervezethez és annak biztonságához kapcsolódó egyéb információkat is tartalmaz.

tooallow az Azure AD felhasználók toosign tooyour alkalmazásban, regisztrálnia kell az alkalmazás a saját bérlőjében.  Az Azure AD-bérlőkben az alkalmazások közzététele **teljesen ingyenes**.  Valójában a legtöbb fejlesztő több bérlőt és alkalmazást is létrehoz kísérleti, fejlesztési, előkészítési és tesztelési célokból.  A szervezeteknek, amelyek a regisztrálás és az alkalmazásra is lehetősége van toopurchase licencek Ha speciális címtárfunkciókat tootake előnyeit.

Szóval, hogyan szerezhet be Azure AD-bérlőt?  hello folyamat lehet kissé eltérő, ha Ön:

* [Meglévő Office 365-előfizetéssel rendelkezik](#use-an-existing-office-365-subscription)
* [Meglévő Azure-előfizetése társítva van egy Microsoft-fiókkal](#use-an-msa-azure-subscription)
* [Meglévő Azure-előfizetése társítva van egy szervezeti fiókkal](#use-an-organizational-azure-subscription)
* [A fenti hello közül egyikkel sem rendelkezik & szeretné, hogy teljesen új toostart](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Meglévő Office 365-előfizetést használ
Ha van meglévő Office 365-előfizetése, akkor már Azure AD-bérlővel is rendelkezik! Bejelentkezhet toohello [Azure-portálon](https://portal.azure.com) fiókhoz az Office 365 és az Azure AD használatának megkezdéséhez.

## <a name="use-an-msa-azure-subscription"></a>MSA Azure-előfizetést használ
Ha korábban regisztrált egy Azure-előfizetésre egyéni Microsoft-fiókkal, már van bérlője.  Ha jelentkezik be a toohello [Azure Portal](https://portal.azure.com), akkor automatikusan vannak, naplózva lesznek tooyour alapértelmezett bérlőt. Biztos, hogy elférjen - szabad toouse ennél a bérlőnél, akkor tekintse meg, de érdemes lehet toocreate egy szervezeti rendszergazdai fiókot.

toodo Igen, kövesse az alábbi lépéseket.  Azt is megteheti előfordulhat, hogy kívánja toocreate egy új bérlőt, és hozzon létre egy rendszergazda bérlőben hasonló folyamata.

1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com) az egyéni fiókjával
2. Keresse meg a toohello "Azure Active Directory" szakaszát hello portal (hello bal oldali navigációs sávban, alatt található **több szolgáltatások**)
3. Érdemes automatikusan bejelentkezve toohello "Alapértelmezett címtárat", ha nem könyvtárak hello jobb felső sarokban a fiók nevére kattintva válthat.
4. A hello **gyorsan elvégezhető** területen válasszon **hozzáadni egy felhasználót**.
5. A felhasználó hozzáadása űrlapon hello, adja meg a következő adatok hello:

   * Név: (adjon meg egy megfelelő értéket)
   * Felhasználónév: (válasszon egy felhasználónevet ehhez a rendszergazdához)
   * Profil: (írja hello megfelelő értékeket keresztnév, utolsó nevét, beosztás és szervezeti egység)
   * Szerepkör: Globális rendszergazda
6. Miután végrehajtotta hello felhasználó hozzáadása űrlapot, és ideiglenes jelszót hello hello új rendszergazda felhasználó kap, toorecord meg arról, hogy ezt a jelszót mivel ennek rendelés toochange hello jelszó a felhasználónak a toologin kell. Hello jelszó is küldhet közvetlenül toohello felhasználónak egy másodlagos e-mail-címre.
7. Kattintson a **létrehozása** toocreate hello új felhasználó.
8. Jelentkezzen be a toochange hello ideiglenes jelszó [https://login.microsoftonline.com](https://login.microsoftonline.com) az új felhasználói fiókot, és módosítsa a hello jelszó kérésekor.

## <a name="use-an-organizational-azure-subscription"></a>Szervezeti Azure-előfizetést használ
Ha korábban regisztrált egy Azure-előfizetésre a szervezeti fiókjával, már van bérlője.  A hello [Azure Portal](https://portal.azure.com), keresse meg a bérlő túl Navigálás "Több szolgáltatások" és "Az Azure Active Directory."  Ennél a bérlőnél ismertető elférjen szabad toouse áll.

## <a name="start-from-scratch"></a>Kezdés a nulláról
Ha az összes fenti hello értelmetlen tooyou, ne aggódjon.  Egyszerűen látogasson el [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) toosign az Azure szolgáltatáshoz egy új szervezettel.  Hello folyamat befejezése után fog a saját Azure AD-bérlő hello meg regisztrációkor választott tartománynévvel.  A hello [Azure Portal](https://portal.azure.com), túl navigálva megkeresheti a bérlőt "Azure Active Directory" hello bal oldali sávban a

Iratkozik fel Azure hello folyamat részeként fogja szükséges tooprovide hitelkártya adatait.  Nyugodtan folytathatja a folyamatot – nem kell fizetnie az alkalmazások Azure AD-ben történő közzétételéért vagy az új bérlők létrehozásáért.
