---
title: "aaaAzure feltételes hozzáférést az Active Directory – gyakori kérdések |} Microsoft Docs"
description: "Első válaszok toofrequently kérdések a feltételes hozzáférés az Azure Active Directoryban."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d23acbb01217d7e9717d1a43de1b46a929404118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a>Azure Active Directory – gyakori kérdések a feltételes hozzáférés

## <a name="which-applications-work-with-conditional-access-policies"></a>Mely alkalmazások használható feltételes hozzáférési szabályzatokat?

A feltételes hozzáférési házirendekkel együtt használható alkalmazások kapcsolatos információkért lásd: [alkalmazások és az Azure Active Directoryban feltételes hozzáférési szabályai használó](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Feltételes hozzáférési szabályzatok érvényesítik a B2B együttműködés és a vendégfelhasználók?

Üzleti vállalatközi (B2B) együttműködés felhasználók számára a házirendek érvényben vannak. Azonban néhány esetben a felhasználó nem feltétlenül tudja toosatisfy hello házirend követelményeinek. A Vendég felhasználó szervezet például előfordulhat, hogy támogatja a többtényezős hitelesítés. 



## <a name="does-a-sharepoint-online-policy-also-apply-tooonedrive-for-business"></a>A SharePoint Online-szabályzat is vonatkozik tooOneDrive vállalati?

Igen. A SharePoint Online-szabályzat a vállalati tooOneDrive is vonatkozik.


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Miért nem állítható be egy házirendet az ügyfélalkalmazások, például a Word és Outlook?

Feltételes hozzáférési szabályzatot állítja be a szolgáltatások elérésére vonatkozó követelmények. Hitelesítési toothat szolgáltatás esetén van kényszerítve. hello házirend nincs beállítva közvetlenül egy ügyfélalkalmazást. Ehelyett azt akkor érvényes, ha egy ügyfél meghívja a szolgáltatást. A SharePoint beállított házirend például SharePoint hívása tooclients vonatkozik. Exchange a beállított házirend tooOutlook vonatkozik.

## <a name="does-a-conditional-access-policy-apply-tooservice-accounts"></a>Nem érvényes egy feltételes hozzáférési házirend tooservice fiókok?

Feltételes hozzáférési házirendek alkalmazása tooall felhasználói fiókokat. Ez magában foglalja a felhasználói fiókok, szolgáltatásfiókok használatosak. Gyakran a felügyelet nélküli futtató szolgáltatásfiókot nem megfelelnek a hello feltételes hozzáférési házirendet. Például a multi-factor authentication lehet szükség. Szolgáltatásfiókok k zárhatók ki a házirend a feltételes hozzáférési házirend beállítások használatával. 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a>Graph API-k érhetők el a feltételes hozzáférési szabályzatok konfigurálása?

Jelenleg nincs. 

## <a name="what-is-hello-default-exclusion-policy-for-unsupported-device-platforms"></a>Mi az a hello alapértelmezett kizárási házirend nem támogatott eszközplatformok?

Feltételes hozzáférési házirendek jelenleg, iOS és Android rendszerű eszközök felhasználóinak szelektív érvényesítik. Alkalmazások eszköz platformokon, alapértelmezés szerint nem érinti hello feltételes hozzáférési házirend az iOS és Android-eszközökön. A bérlői rendszergazda választhat toooverride hello globális házirend toodisallow hozzáférés toousers nem támogatott platformokon.


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a>Hogyan működik a Microsoft Teams feltételes hozzáférési szabályzatokat?  

Microsoft Teams erősen támaszkodik a Exchange Online és SharePoint Online core termelékenység forgatókönyvek esetén, mint az értekezletek, naptárak és fájlmegosztás. Ezekbe a felhőalkalmazásokba beállított feltételes hozzáférési házirendek tooMicrosoft csapatok alkalmazni, amikor egy felhasználó bejelentkezik.

Microsoft Teams is támogatott külön-külön, a felhőalapú alkalmazások az Azure Active Directory feltételes hozzáférési házirendek. Felhőalapú alkalmazások beállított tanúsítvány hatóság házirendek tooMicrosoft csapatok alkalmazzák, amikor a felhasználó jelentkezik be.

Microsoft Teams asztali ügyfelek Windows és Mac modern hitelesítést támogatja. Modern hitelesítéssel bejelentkezés hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office ügyfélalkalmazások alapján különböző platformokon. 
