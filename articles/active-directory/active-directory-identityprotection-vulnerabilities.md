---
title: "Azure Active Directory Identity Protection által észlelt aaaVulnerabilities |} Microsoft Docs"
description: "Azure Active Directory Identity Protection által észlelt hello biztonsági rések áttekintése."
services: active-directory
keywords: "az Azure active directory azonosító adatok védelmét, a cloud app discovery, alkalmazások, biztonság, kockázat, kockázati szint, biztonsági rés, biztonsági házirend kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 92233a5b-cb34-4d28-88cc-d5d29c0f3256
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 5e1cb401f8b566a180eb46e3420a090bcfc66767
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Azure Active Directory Identity Protection által észlelt biztonsági rések
Biztonsági rések a környezetben, amely is kihasználható egy támadó gyengeségei miatt. Azt javasoljuk, hogy a biztonsági rések tooimprove hello biztonságot, ha a szervezet cím, és hogy a támadók kihasználva őket.


![biztonsági rések](./media/active-directory-identityprotection-vulnerabilities/101.png "biztonsági réseket")



hello alább láthatja hello biztonsági rések Identity Protection által jelentett áttekintést.

## <a name="multi-factor-authentication-registration-not-configured"></a>A multi-factor authentication regisztráció nincs konfigurálva
A biztonsági rés segítségével szabályozhatja a szervezet Azure multi-factor Authentication hello telepítését. 

Azure multi-factor Authentication hitelesítési biztonsági toouser hitelesítési egy második réteget biztosít. Segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint. Erős hitelesítés, könnyen ellenőrzési lehetőségek széles keresztül biztosítja – a telefonhívás, szöveges üzenet vagy mobilalkalmazás értesítés vagy ellenőrző kód és a 3. fél OATH-tokeneket.

Azt javasoljuk, hogy a felhasználói bejelentkezések Azure multi-factor Authentication hitelesítés szükséges. A multi-factor authentication kulcsfontosságú szerepet játszik a kockázat-alapú feltételes hozzáférési házirendek Identity Protection keresztül érhető el.

További részletekért lásd: [Mi az Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="unmanaged-cloud-apps"></a>Nem felügyelt felhőalkalmazások
A biztonsági rés segít azonosítani a szervezet nem felügyelt felhőalkalmazások.

A modern vállalatok számára az informatikai részlegeket gyakran nem tudnak a összes hello felhőalapú alkalmazásokhoz, hogy a szervezetek használó felhasználókat toodo a munkahelyi. Miért a rendszergazdák jogosulatlan hozzáférés toocorporate adatokat, a lehetséges adatszivárgás és az egyéb biztonsági kockázatok kétségei vannak könnyen toosee. 

Azt javasoljuk, hogy a szervezet és központi telepítése a Cloud App Discovery nem felügyelt toodiscover felhőalkalmazásokhoz toomanage ezeket az alkalmazásokat az Azure Active Directoryval.

További részletekért lásd: [keresése a nem felügyelt felhőalapú alkalmazásokhoz, a Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

## <a name="security-alerts-from-privileged-identity-management"></a>Privileged Identity Management biztonsági riasztásai
A biztonsági rés segít felderíteni, és oldja meg a szervezet a kiemelt jogosultságú identitások kapcsolatos riasztások.  

tooenable felhasználók toocarry jogosultságokhoz kötött műveletek ki, a szervezeteknek kell toogrant felhasználók ideiglenes és állandó kiemelt férhetnek hozzá az Azure AD-erőforrások Azure vagy az Office 365 vagy más Szolgáltatottszoftver-alkalmazásoknál. Minden egyes kiemelt felhasználók növekszik hello támadási felületét a szervezet. A biztonsági rés segítséget nyújt a felhasználók azonosítása a szükségtelen rendszerjogosultságú hozzáférés és a megfelelő lépéseket tooreduce végrehajtása vagy a kiküszöbölése hello kockázatot jelentenek. 

Azt javasoljuk, hogy a szervezet Azure AD Privileged Identity Management toomanage, vezérlő, és a figyelő emelt szintű identitások és azok az Azure AD hozzáférési tooresources, valamint más Microsoft online szolgáltatások, például az Office 365-öt vagy a Microsoft Intune használja.

További részletekért lásd: [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="see-also"></a>Lásd még:
* [Az Azure Active Directory azonosító adatok védelmét](active-directory-identityprotection.md)

