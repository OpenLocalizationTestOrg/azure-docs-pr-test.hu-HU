---
title: "az Azure AD alkalmazásproxy aaaCustom tartományok |} Microsoft Docs"
description: "Egyéni tartományok az Azure AD alkalmazásproxy kezelését hello app hello URL van hello azonos függetlenül, hogy a felhasználók elérhetik azt."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7a433c411976077210a2435c3c087991c7430755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Egyéni tartományok az Azure AD alkalmazásproxy használata

Amikor közzétesz egy alkalmazást az Azure Active Directory-proxyn keresztül történő, a felhasználók toogo toowhen azok távolról dolgozik létrehozása egy külső URL-CÍMÉT. Az URL-cím beolvasása hello alapértelmezett tartomány *yourtenant.msappproxy.net*. Például ha közzétett alkalmazás nevű költségek és a bérlő neve Contoso, majd hello külső URL-cím https://expenses-contoso.msappproxy.net lenne. Ha saját tartománynevét szeretné toouse, az alkalmazás az egyéni tartománynév konfigurálása 

Azt javasoljuk, hogy állít be egyéni tartományok az alkalmazások, amikor csak lehetséges. Egyéni tartományok hello előnyei a következők:

- A felhasználók férhetnek toohello alkalmazás hello URL-CÍMÉRE, hogy működnek-belül vagy kívül a hálózaton.
- Ha az alkalmazásokat kell hello azonos belső és külső URL-címeket, mutató hivatkozások egy alkalmazás tooanother hello vállalati hálózaton kívül is toowork továbbra is. 
- A vállalati arculat szabályozza, és hello URL-címeket szeretne létrehozni. 


## <a name="configure-a-custom-domain"></a>Az egyéni tartománynév konfigurálása

### <a name="prerequisites"></a>Előfeltételek

Az egyéni tartománynév konfigurálása előtt győződjön meg arról, hogy rendelkezik-e hello előkészített követelményeknek: 
- A [ellenőrzött tartomány hozzáadása az Active Directory tooAzure](active-directory-domains-add-azure-portal.md).
- Egyéni tanúsítvány hello tartomány, a PFX-fájl hello formában. 
- A helyszíni alkalmazások [proxyn keresztül történő közzétett](application-proxy-publish-azure-portal.md).

### <a name="configure-your-custom-domain"></a>Az egyéni tartomány konfigurálása

Ha készen áll a három követelményekről, kövesse a lépéseket tooset be az egyéni tartomány:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Keresse meg a túl**Azure Active Directory** > **vállalati alkalmazások** > **összes alkalmazás** hello alkalmazást válasszon, és azt szeretné, hogy toomanage.
3. Válassza ki **alkalmazásproxy**. 
4. Hello külső URL-cím mezőben az egyéni tartomány hello legördülő lista tooselect használja. Ha nem látja a tartomány hello listában, majd azt még nem nincs ellenőrizve még. 
5. Válassza ki **mentése**
5. Hello **tanúsítvány** letiltott mező aktívvá válik. Válassza ezt a mezőt. 

   ![Kattintson a tooupload tanúsítvány](./media/active-directory-application-proxy-custom-domains/certificate.png)

   Ha már feltöltött tanúsítványt a tartomány, a hello tanúsítványmezőt hello tanúsítvány adatait jeleníti meg. 

6. Hello PFX-tanúsítvány feltöltése, és írja be a hello jelszót hello tanúsítványt. 
7. Válassza ki **mentése** toosave a módosításokat. 
8. Adja hozzá a [DNS-rekord](../dns/dns-operations-recordsets-portal.md) , hogy az átirányítások hello URL-cím toohello új külső msappproxy.net tartomány. 

>[!TIP] 
>Csak akkor kell egy tanúsítványt tooupload egyéni tartományonként. Tanúsítvány feltöltése után kiválaszthatja hello egyéni tartomány, amikor egy új alkalmazás közzététele, és nincs további konfigurációs toodo hello DNS-rekord kivételével. 

## <a name="manage-certificates"></a>Tanúsítványok kezelése

### <a name="certificate-format"></a>A tanúsítvány formátuma
Nincs a tanúsítvány-aláírás módszerek hello korlátozva. Elliptikus görbe alapú titkosítást (ECC), a tulajdonos alternatív nevére (SAN) és a közös tanúsítvány egyéb használatát támogatja. 

Helyettesítő tanúsítvány használható, amíg hello helyettesítő hello szükséges külső URL-címe megegyezik. 

Önaláírt tanúsítványok is használható. Ha a titkos hitelesítésszolgáltatót használ, hello CDP (tanúsítvány-visszavonási pont terjesztési pont) hello tanúsítvány nyilvános kell lennie.

### <a name="changing-hello-domain"></a>Hello tartomány módosítása
Hello külső URL-cím legördülő lista az alkalmazás összes ellenőrzött tartományok jelennek meg. toochange hello tartomány, csak olyan frissítés, amely hello alkalmazás mezőben. Ha hello listán, nem kívánt hello tartomány [ellenőrzött tartományt adja hozzá azt](active-directory-domains-add-azure-portal.md). Ha még nem rendelkezik társított tanúsítvány egy tartományba, kövesse a lépéseket 5-7 tooadd hello tanúsítványt. Ezt követően ne hello DNS rekord tooredirect hello új külső URL-címről. 

### <a name="certificate-management"></a>Tanúsítványkezelés
Azonos tanúsítvány egyszerre több alkalmazás, kivéve, ha hello alkalmazások megosztott egy külső gazdagép hello is használhatja. 

Megjelenik egy figyelmeztetés, ha a tanúsítvány lejár, felszólító tooupload hello portálon keresztül másik tanúsítvánnyal. Ha hello tanúsítvány visszavonása esetén előfordulhat, hogy jelennek meg a felhasználók biztonsági figyelmeztetés hello alkalmazáshoz való hozzáféréskor. A tanúsítványok visszavonási ellenőrzést nem végezzük.  tooupdate hello tanúsítvány egy adott alkalmazáshoz, keresse meg a toohello alkalmazás, és utasításai 5-7 egyéni tartományok konfigurálása a közzétett alkalmazások tooupload egy új tanúsítványt. Ha hello régi tanúsítvány más alkalmazás nem használatban van, az automatikusan törlődik. 

Minden Tanúsítványkezelő jelenleg egyes lapjain keresztül így toomanage tanúsítványok hello vonatkozó kérelmek hello környezetében kell. 

## <a name="next-steps"></a>Következő lépések
* [Egyszeri bejelentkezés engedélyezése](active-directory-application-proxy-sso-using-kcd.md) tooyour közzétett alkalmazást az Azure AD-alapú hitelesítés.
* [A feltételes hozzáférés engedélyezése](active-directory-application-proxy-conditional-access.md) tooyour közzétett alkalmazást.
* [Az egyéni tartomány nevét tooAzure AD hozzá](active-directory-domains-add-azure-portal.md)


