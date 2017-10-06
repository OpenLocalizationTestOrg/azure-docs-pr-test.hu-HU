---
title: "aaaUsage forgatókönyvek és az Azure AD-csatlakozás telepítési szempontjai |} Microsoft Docs"
description: "Ismerteti, hogyan rendszergazdák állíthat be az Azure AD Join a saját végfelhasználóik számára (az alkalmazottak, a diákok, más felhasználókat). Emellett ismerteti a különböző valós forgatókönyv hello használatához az Azure AD Join."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
tags: azure-classic-portal
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: markvi
ms.openlocfilehash: 7e57971481aa312ebf8a69999d194f9dcc3d4708
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="usage-scenarios-and-deployment-considerations-for-azure-ad-join"></a>Használati forgatókönyvek és az Azure AD-csatlakozás telepítési szempontjai
## <a name="usage-scenarios-for-azure-ad-join"></a>Az Azure AD Join használati forgatókönyvek
### <a name="scenario-1-businesses-largely-in-hello-cloud"></a>1. forgatókönyv: Vállalatok nagymértékben hello felhőben
Az Azure Active Directory Join (Azure AD Join) előnyei Ha jelenleg működik és identitáskezelést saját üzleti hello felhőben vagy rövidesen áthelyezi toohello felhő. Használhatja az Azure AD toosign tooWindows 10 a létrehozott fiók. Keresztül [először futtassa a felhasználói élmény (FRX) folyamat hello](active-directory-azureadjoin-user-frx.md), vagy az Azure AD összeillesztésével [hello beállítások menü](active-directory-azureadjoin-user-upgrade.md), a felhasználók kapcsolódhatnak a gépek tooAzure AD.  A felhasználók is élvezheti az egyszeri bejelentkezés (SSO) hozzáférést túl felhőalapú erőforrásokat, például az Office 365, a böngészőjük vagy egy Office-alkalmazásban.

### <a name="scenario-2-educational-institutions"></a>2. forgatókönyv: Oktatási intézmények
Oktatási intézmények általában rendelkeznek kétféle felhasználói: faculty és a diákok. Faculty tagok minősülnek hello szervezet hosszabb távú tagjai. A helyszíni fiókokat hoz létre nekik kívánatos. A diákok hello szervezet shorter-term tagjai és a fiókok kezelheti az Azure ad-ben. Ez azt jelenti, hogy directory méretezési továbbíthatja toohello felhő helyett a helyszínen tárolt alatt. Azt is jelenti, hogy a diákok tooWindows a saját Azure AD-fiókokkal a képes toosign kell, és szerezze be a hozzáférési tooOffice 365 erőforrások egy Office-alkalmazásban.

### <a name="scenario-3-retail-businesses"></a>3. forgatókönyv: Kiskereskedelemben
Kereskedelmi cégek határozza dolgozó munkatársak és a hosszú távú alkalmazottak rendelkezik. Általában a helyi fiókok létrehozásához, és tartományhoz csatlakoztatott számítógépeken használja a hosszabb távú teljes munkaidejű alkalmazottak számára. De határozza munkavállalók hello szervezet shorter-term tagjai, és is kívánatos toomanage a fiókok, ahol a felhasználói licencek könnyebben átvihetők. Ha a felhasználói fiókokat hozhat létre az Office 365 licencek hello felhőben, ezek a felhasználók beolvasása bejelentkezés tooWindows és Office-alkalmazások az Azure AD-fiókot, amíg a licenccel rendelkező nagyobb rugalmasságot karbantartása, hogy kilép a hello előnyeit.

### <a name="scenario-4-additional-scenarios"></a>4. forgatókönyv: További forgatókönyvek
Azt a korábbiakban említettük hello előnyeit, valamint a felhasználók saját eszközöket tooAzure AD csatlakozni egy egyszerűsített csatlakozó élmény, hatékony kezelése, automatikus mobileszköz-kezelési beléptetés és egyszeri bejelentkezés tooAzure miatt nem kihasználhatja Az AD és a helyszíni erőforrások.  

## <a name="deployment-considerations-for-azure-ad-join"></a>Azure AD-csatlakozás telepítési szempontjai
### <a name="enable-your-users-toojoin-a-company-owned-device-directly-tooazure-ad"></a>Engedélyezze a felhasználók toojoin a vállalat által birtokolt eszköz közvetlenül tooAzure AD
A vállalatok csak felhőalapú fiókok toopartner vállalatok és a szervezetek biztosít. Ezek a partnerek egyszerűen ezután hozzáférhetnek vállalati alkalmazások és erőforrások eléréséről az egyszeri bejelentkezés. Ebben a forgatókönyvben az alkalmazandó toousers, akik elsősorban a hello felhő, például az Office 365-öt vagy SaaS-alkalmazásokhoz Azure AD hitelesítésében támaszkodó erőforrások elérését.

### <a name="prerequisites"></a>Előfeltételek
**Hello vállalati szintű (rendszergazda)**

* Azure-előfizetések az Azure Active Directoryval  

**Hello felhasználói szinten**

* Windows 10 (Professional és Enterprise kiadás)

### <a name="administrator-tasks"></a>Rendszergazdai feladatok
* [Az eszközregisztráció beállítása](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Felhasználói feladatok
* [A telepítés során az Azure AD-val a Windows 10 új eszköz beállítása](active-directory-azureadjoin-user-frx.md)
* [Egy Windows 10-es eszköz beállítása az Azure AD hello-beállítások menüjében](active-directory-azureadjoin-user-upgrade.md)
* [Csatlakozás egy Windows 10-es eszköz személyes tooyour szervezet](active-directory-azureadjoin-personal-device.md)

## <a name="enable-byod-in-your-organization-for-windows-10"></a>A Windows 10-re a szervezetében BYOD engedélyezése
A felhasználók és az alkalmazottak toouse beállíthat a személyes Windows eszközök (használata BYOD) tooaccess vállalati alkalmazások és erőforrások. A felhasználók adjon hozzá az Azure AD fiók (munkahelyi vagy iskolai fiókok) tooa személyes Windows eszköz tooaccess erőforrásokat biztonságos és megfelelő módon.

### <a name="prerequisites"></a>Előfeltételek
**Hello vállalati szintű (rendszergazda)**

* Azure AD-előfizetés

**Hello felhasználói szinten**

* Windows 10 (Professional és Enterprise kiadás)

### <a name="administrator-tasks"></a>Rendszergazdai feladatok
* [Az eszközregisztráció beállítása](active-directory-azureadjoin-setup.md)

### <a name="user-tasks"></a>Felhasználói feladatok
* [Csatlakozás egy Windows 10-es eszköz személyes tooyour szervezet](active-directory-azureadjoin-personal-device.md)

## <a name="additional-information"></a>További információ
* [Windows 10 Enterprise hello: módon toouse eszközök munkára](active-directory-azureadjoin-windows10-devices-overview.md)
* [Felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási kiterjesztése](active-directory-azureadjoin-user-upgrade.md)
* [Microsoft Passporton keresztül jelszó nélkül identitások hitelesítése](active-directory-azureadjoin-passport.md)
* [További információk az Azure AD Join használati forgatókönyveiről](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Csatlakozás a tartományhoz csatlakozó eszközök tooAzure AD-hez Windows 10-es környezetben](active-directory-azureadjoin-devices-group-policy.md)
* [Az Azure AD Join beállítása](active-directory-azureadjoin-setup.md)

