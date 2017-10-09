---
title: "aaaApplication lapján nem jelennek meg helyesen az alkalmazásproxy-alkalmazás |} Microsoft Docs"
description: "Amikor hello lap nem megfelelően jelennek meg az alkalmazás Proxy alkalmazás útmutatást integrálva van az Azure AD"
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
ms.openlocfilehash: f4abaa4e94c512868f2085affe59cac443784a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Alkalmazás lapján nem jelennek meg helyesen az alkalmazásproxy-alkalmazáshoz

Ez a cikk segítséget Azure Active Directory Alkalmazásproxyjával alkalmazások tootroubleshoot problémái toohello lap Navigálás, de hello oldalon valami mégsem helyes-e.

## <a name="overview"></a>Áttekintés
Alkalmazásproxy alkalmazások közzététele, amikor csak a legfelső szintű lapok érhetők el hello alkalmazáshoz való hozzáféréskor. Hello lapján nem jelennek meg megfelelően, ha lehet, hogy hello legfelső szintű belső használt URL-cím hello alkalmazás hiányzik néhány lap erőforrást. tooresolve, győződjön meg arról, hogy a közzétett *összes* hello lap erőforrások hello az alkalmazás részeként.

Ez a jelenség hello a hálózati követő megnyitásával ellenőrizheti (például a Fiddler, vagy az F12 eszközök Internet Explorer vagy Edge) hello lap és 404-es hibákat keres. Azt jelzi, hogy hello lapok, amelyek jelenleg nem található, és közzétett toobe lehet szükség.

Ebben az esetben, például azt feltételezik, miután közzétette költségek kérelmet egy belső URL-címe használatával <http://myapps/expenses>, de hello alkalmazása használja-e hello stíluslap <http://myapps/style.css>. Ebben az esetben hello stíluslap nincs közzétéve az alkalmazásban, a 404-es hiba történt miközben a rendszer tooload style.css hello költségek app betöltése kivételt. Ebben a példában hello probléma szeretné feloldani egy belső URL-címével hello alkalmazás közzététele <http://myapp/> helyette.

## <a name="problems-with-publishing-as-one-application"></a>Egy alkalmazás közzétételi kapcsolatos problémák

Ha még nincs lehetséges toopublish belül minden erőforrás hello ugyanahhoz az alkalmazáshoz, kell toopublish több alkalmazás és a hivatkozások között.

toodo Igen, azt javasoljuk, hello [egyéni tartományok](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) megoldás. Azonban az ehhez a megoldáshoz szükségesek, hogy Ön a tulajdonosa hello tanúsítványt a tartomány és az alkalmazások teljes tartományneveit (FQDN) használja. Egyéb lehetőségek, lásd: hello [hivatkozások dokumentáció hibaelhárítása](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).

## <a name="next-steps"></a>Következő lépések
[Az Azure AD-alkalmazásproxy használó alkalmazások közzététele](application-proxy-publish-azure-portal.md)
