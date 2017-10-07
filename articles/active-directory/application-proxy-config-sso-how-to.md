---
title: "aaaHow tooconfigure egyszeri bejelentkezés tooan proxyval alkalmazás |} Microsoft Docs"
description: "Hogyan konfigurálhat egyszeri bejelentkezés tooyour application proxy alkalmazás gyorsan"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: e1289203177c77b3a8bcc9058c5c0b8ae50f243e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-single-sign-on-tooan-application-proxy-application"></a>Hogyan tooconfigure egyszeri bejelentkezés tooan proxyval alkalmazás

Egyszeri bejelentkezés (SSO) lehetővé teszi a felhasználók tooaccess többször hitelesítése nélkül. Lehetővé teszi, hogy a hello egyetlen hitelesítési toooccur hello felhőben Azure Active Directoryban, és lehetővé teszi, hogy hello szolgáltatást vagy az összekötő tooimpersonate hello felhasználói toocomplete semmilyen további hitelesítési kihívást hello alkalmazásból.

## <a name="how-tooconfigure-single-sign-on"></a>Hogyan tooconfigure egyszeri bejelentkezést
tooconfigure SSO, először győződjön meg arról, hogy az alkalmazás Azure Active Directoryn keresztül előhitelesítéshez van konfigurálva. toodo, lépjen túl**Azure Active Directory**  - &gt; **vállalati alkalmazások**  - &gt; **összes alkalmazás**  - &gt; Az alkalmazás  **- &gt; alkalmazásproxy**. Ezen a lapon látható hello "Előtti hitelesítés" mezőben, és győződjön meg arról, hogy túl beállításai "Azure Active Directoryban. 

Hello előtti hitelesítési módszerekkel kapcsolatos további információkért lásd: hello negyedik lépése [alkalmazás közzétételi dokumentum](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

   ![Előhitelesítési módszer az Azure portálon](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>Egyszeri bejelentkezési módok konfigurálása az alkalmazás Proxy alkalmazások
Ezután konfiguráljuk hello adott típusú egyszeri bejelentkezést. hello bejelentkezés módszerek besorolt alapján milyen típusú hitelesítés hello a háttéralkalmazás használja. Alkalmazás Proxy alkalmazások bejelentkezés három típusú támogatja:

-   **Jelszó alapú bejelentkezés**: jelszó alapú bejelentkezés alkalmas bármely alkalmazás által használt toosign a felhasználónév és jelszó megadására. Konfigurációs lépések megtalálhatók a [jelszó-SSO konfigurációs dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#bring-your-own-password-sso-applications).

-   **Integrált Windows-hitelesítés**: az integrált Windows-hitelesítéssel (IWA) alkalmazások esetén az egyszeri bejelentkezés engedélyezve van a Kerberos által korlátozott delegálás (KCD). Az Active Directory tooimpersonate felhasználók és a toosend Application Proxy összekötők engedélyezi, és a nevükben jogkivonatokat fogadni. Kerberos által korlátozott Delegálás konfigurálása a részletek megtalálhatók a hello [egyszeri bejelentkezéshez a Kerberos által korlátozott Delegálás dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd).

-   **Fejléc-alapú bejelentkezés**: fejléc-alapú bejelentkezés az partnerség keresztül engedélyezhető, és néhány további beállításokra van szükség. Hello partneri kapcsolat áll fenn, és a részletes útmutatást ad konfigurálása egyszeri bejelentkezéshez tooan alkalmazás fejlécek használ a hitelesítéshez, lásd: hello [PingAccess az Azure AD-dokumentáció](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access).

Mindkét beállítás a "Vállalati alkalmazások", és a nyitó hello tooyour alkalmazás címen található **egyszeri bejelentkezés** lap hello bal oldali menüben. Vegye figyelembe, hogy az alkalmazás régi portál hello jött létre, nem jelenhet meg ezeket a beállításokat.

Ez a lap is megjelenik egy további bejelentkezési beállítás: csatolt Sign-On. Ez is támogatott alkalmazásproxy. Vegye figyelembe azonban, hogy ez a beállítás nem egyszeri bejelentkezés toohello alkalmazás hozzáadása. Említett hello alkalmazás előfordulhat, hogy már rendelkezik egyszeri bejelentkezés megvalósítása, például az Active Directory összevonási szolgáltatások egy másik szolgáltatás használatával. 

Ez a beállítás lehetővé teszi, hogy egy rendszergazda toocreate egy kapcsolat tooan alkalmazás, amely a felhasználók először megnyílik a hello alkalmazáshoz való hozzáféréskor. Például ha egy alkalmazást, amely konfigurált tooauthenticate felhasználók Active Directory összevonási szolgáltatások 2.0 eszköz használatával, a rendszergazda használhat hello "csatolt bejelentkezés" beállítás toocreate egy hivatkozás tooit hello hozzáférési panel.

## <a name="next-steps"></a>Következő lépések
[Adja meg az egyszeri bejelentkezés tooyour alkalmazások alkalmazásproxyval](active-directory-application-proxy-sso-using-kcd.md)
