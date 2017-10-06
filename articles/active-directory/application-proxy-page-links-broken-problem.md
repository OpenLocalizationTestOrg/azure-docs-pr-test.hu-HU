---
title: "hello oldalon aaaLinks nem működnek az alkalmazásproxy alkalmazás |} Microsoft Docs"
description: "Hogyan tootroubleshoot érintő problémákat működésképtelen integrálva van az Azure AD alkalmazásproxy-alkalmazásokban"
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
ms.openlocfilehash: 77c1e22d27c7a6436d8e57e105037c2328180481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="links-on-hello-page-dont-work-for-an-application-proxy-application"></a>Hello lapon lévő hivatkozások nem működnek az alkalmazásproxy alkalmazás

Ez a cikk segítséget tootroubleshoot miért érdemes az Azure Active Directory Alkalmazásproxyjával alkalmazás lévő hivatkozások nem működnek megfelelően.

## <a name="overview"></a>Áttekintés 
Miután közzétette az alkalmazásproxy-alkalmazások, hello csak hivatkozásokat, hogy a munkahelyi hello alkalmazás alapértelmezés szerint hello belül található hivatkozások toodestinations közzétett gyökér URL-CÍMÉT. hello alkalmazásokon belül hello hivatkozások nem működnek, hello belső URL-cím hello alkalmazás valószínűleg nem tartalmazza az összes hello cél hivatkozások hello alkalmazáson belül.

**Miért történik ez?** Amikor egy alkalmazás egy hivatkozásra kattint, alkalmazásproxy záma tooresolve hello az URL-címet vagy egy belső URL-cím belül azonos hello alkalmazás, vagy a kívülről elérhető URL-címként. Ha hello hivatkozás mutat tooan belső URL-címet, amely nem szerepel a következőben hello azonos alkalmazás, nem tartozik a gyűjtők a tooeither és nem talált hibát eredményez.

## <a name="ways-you-can-resolve-broken-links"></a>Hibás hivatkozások oldhatja módjai

Nincsenek három módon tooresolve probléma. az alábbi hello lehetőségek szerepel az növekvő bonyolultsági szerepelnek.

1.  Ellenőrizze, hogy hello belső URL-címet, amely tartalmazza az összes hello ide tartozó hivatkozásokat hello alkalmazás legfelső szintű. Ez lehetővé teszi, hogy a közzétett tartalom hello belül azonos feloldani az összes hivatkozások toobe alkalmazás.

    Ha módosítsa hello belső URL-címet, de nem szeretné, hogy a felhasználók kezdőlapján toochange hello, a módosítás hello Kezdőlap toohello korábban közzétett belső URL-cím. Túl címen ehhez "Azure Active Directory" -&gt; App regisztrációk -&gt; hello alkalmazás - kijelölése&gt; tulajdonságok. A Tulajdonságok lapon látható hello mező "Kezdőlap URL-címe" amely is toobe hello szükséges kezdőlapja.

2.  Ha az alkalmazások teljes tartományneveit (FQDN) használ, akkor [egyéni tartományok](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) toopublish az alkalmazások. Ez a funkció lehetővé teszi, hogy a hello azonos URL-cím toobe mind belső használatú és külső beállításait.

    Ez a beállítás biztosítja, hogy hello hivatkozások az alkalmazás kívülről elérhető proxyn keresztül történő óta hello hivatkozások belül hello toointernal URL-címeket is felismeri kívülről. Vegye figyelembe, hogy az összes hivatkozás továbbra is kell toobelong tooa közzétett alkalmazást. Azonban ez a beállítás hello hivatkozások nem szükséges toobelong toohello ugyanahhoz az alkalmazáshoz és toomultiple alkalmazások is tartozhatnak.

3.  Ha egyik sem valósítható meg, csatlakozik egy új funkciója, amelyek URL-cím fordítása/újraírását hello előzetes verzióját. Ezzel a lehetőséggel belső URL-címeket és hivatkozásokat tartalmaz, amelyek az alkalmazások hello HTML törzs léteznie kell a lefordított, vagy "leképezve", toohello közzétett külső Proxy URL-címek. Ez csak akkor működik, a HTML- vagy CSS hello hivatkozások, és ez segít a hivatkozás segítségével JS jön létre. 

Ennek eredményeképpen, erősen ajánlott hello segítségével [egyéni tartományok](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) megoldás Ha lehetséges. Ha szeretné, hogy toojoin hello preview, e-mailben < aadapfeedback@microsoft.com > hello applicationId(s) együtt.

## <a name="next-steps"></a>Következő lépések
[A meglévő helyszíni proxykiszolgálókkal működik](application-proxy-working-with-proxy-servers.md)

