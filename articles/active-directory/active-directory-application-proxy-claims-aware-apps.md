---
title: "aaaClaims-t támogató alkalmazások – Azure AD alkalmazás Proxy |} Microsoft Docs"
description: "Hogyan toopublish helyszíni fogadja el az AD FS-jogcímek biztonságos távoli hozzáférést a felhasználók által az ASP.NET-alkalmazások."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: kgremban
ms.openlocfilehash: 7be633225de700226c7c94815eb91b3de2b61cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Az alkalmazásproxy jogcímbarát alkalmazásokkal való munka
[Jogcímbarát alkalmazások](https://msdn.microsoft.com/library/windows/desktop/bb736227.aspx) hajtsa végre egy átirányítási toohello biztonsági jogkivonat szolgáltatás (STS). hello STS hello felhasználói jogkivonat cserébe kéri a hitelesítő adatokat, és ezután átirányítja a felhasználókat hello toohello alkalmazásában. Néhány módon tooenable alkalmazásproxy toowork ezek átirányítással van. Ez a cikk tooconfigure a központi telepítés az jogcímbarát alkalmazáshoz használni. 

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy hello STS hello jogcímbarát alkalmazáshoz átirányítja a felhasználókat a helyszíni hálózaton kívülről elérhető toois. Elérhetővé teheti hello STS proxyn keresztül, az ilyen, vagy kívül kapcsolatok engedélyezése. 

## <a name="publish-your-application"></a>Az alkalmazás közzététele

1. Az alkalmazás toohello utasításokat leírtak szerint közzétételére [alkalmazások közzétételére az alkalmazásproxy](application-proxy-publish-azure-portal.md).
2. Keresse meg a toohello alkalmazáslap hello portál, és válassza a **egyszeri bejelentkezés**.
3. Ha úgy döntött, hogy **Azure Active Directory** , a **előhitelesítési módszer**, jelölje be **az Azure AD az egyszeri bejelentkezés le van tiltva** , a **belső Hitelesítési módszer**. Ha úgy döntött, hogy **csatlakoztatott** , a **előhitelesítési módszer**, toochange semmi nem szükséges.

## <a name="configure-adfs"></a>AD FS konfigurálása

Az AD FS a jogcímbarát alkalmazások az alábbi két módszer egyikével állíthatja be. hello első az egyéni tartományok használatával. hello második WS-Federation jelenti. 

### <a name="option-1-custom-domains"></a>1. lehetőség: Egyéni tartományok

Ha az összes hello belső URL-címeket, az alkalmazások: teljesen minősített tartománynevek (FQDN), majd konfigurálhatja [egyéni tartományok](active-directory-application-proxy-custom-domains.md) az alkalmazások számára. Használjon hello egyéni tartományok toocreate külső URL-címek, amelyek megegyeznek a belső URL-címek hello hello. Ha a külső URL-címek megegyeznek a belső URL-címeket, majd hello STS átirányítások működik, hogy a felhasználók is a helyi vagy távoli. 

### <a name="option-2-ws-federation"></a>2. lehetőség: WS-Federation

1. Nyissa meg az AD FS kezelése.
2. Nyissa meg túl**függő entitás Megbízhatóságai**, kattintson a jobb gombbal az alkalmazásproxy közzétett hello alkalmazás, és válassza a **tulajdonságok**.  

   ![Függő entitás Megbízhatóságai kattintson a jobb gombbal az alkalmazás neve – képernyőkép](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. A hello **végpontok** lap **típusú végpont**, jelölje be **WS-Federation**.
4. Alatt **megbízható URL-cím**, adja meg a megadott hello az alkalmazásproxy hello URL-cím **külső URL-cím** kattintson **OK**.  

   ![Adja hozzá a végpont - állítsa be a megbízható URL-cím érték – képernyőkép](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Következő lépések
* [Egyszeri bejelentkezés engedélyezése a](application-proxy-sso-overview.md) , amelyek nem jogcímbarát alkalmazásokhoz
* [Natív ügyfél alkalmazások toointeract proxy alkalmazások engedélyezése](active-directory-application-proxy-native-client.md)


