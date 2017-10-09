---
title: "Több tartomány aaaAzure AD Connect"
description: "Ez a dokumentum ismerteti, és az Office 365 és az Azure AD több felső szintű tartomány beállításával."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 5595fb2f-2131-4304-8a31-c52559128ea4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 91d87875ceacee4e34f132938e4bb0294fb954e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Többtartományos támogatás az Azure AD összevonási szolgáltatásához
hello alábbi dokumentáció nyújt útmutatást hogyan toouse több legfelső szintű tartományok és altartományok, ha az Office 365 vagy Azure AD-tartomány összevonását.

## <a name="multiple-top-level-domain-support"></a>Több felső szintű tartomány támogatása
Összevonását több, az Azure ad-vel a legfelső szintű tartományoknak nincs szükség, amikor egy legfelső szintű tartomány összevonását néhány további konfigurálást igényel.

Amikor egy tartományt az Azure AD össze van vonva, több tulajdonság beállítva hello tartományhoz az Azure-ban.  Egy fontos egyik IssuerUri lesz.  Ez az URI, amely használja az Azure AD tooidentify által hello token hello tartomány társítva.  hello URI tooresolve tooanything nem szükséges, de egy érvényes URI-Azonosítót kell lennie.  Alapértelmezés szerint az Azure AD állítja be a toohello hello összevonási szolgáltatás azonosítóját a helyszíni AD FS konfigurációs.

> [!NOTE]
> hello összevonási szolgáltatás azonosítója, amely egyedileg azonosítja az összevonási szolgáltatás URI.  hello összevonási szolgáltatás AD FS funkcionáló példányának, mint hello biztonságijogkivonat-szolgáltatás. 
> 
> 

View IssuerUri hello PowerShell-paranccsal is `Get-MsolDomainFederationSettings -DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

A probléma merül fel, ha azt szeretné, hogy tooadd több felső szintű tartomány.  Például tegyük fel, a telepítő összevonási az Azure AD között van, és a helyszíni környezetben.  A dokumentum használok bmcontoso.com.  Most már hozzáadtam egy második felső szintű tartomány bmfabrikam.com.

![Tartományok](./media/active-directory-multiple-domains/domains.png)

Ha azt a bmfabrikam.com tartomány toobe összevont tooconvert, azt hibaüzenetet kap.  hello ennek oka, Azure AD is rendelkezik, amely nem engedélyezi a hello IssuerUri tulajdonság toohave hello értékeként azonos értéket egynél több tartomány korlátozás.  

![Összevonási hiba](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain paraméter
tooworkaround, igazolnia kell, hogy egy másik IssuerUri, amely hello segítségével végezhető tooadd `-SupportMultipleDomain` paraméter.  A paraméter a következő parancsmagok hello használata:

* `New-MsolFederatedDomain`
* `Convert-MsolDomaintoFederated`
* `Update-MsolFederatedDomain`

Ez a paraméter lehetővé teszi az Azure AD hello IssuerUri konfigurálása, hogy hello tartomány nevét hello alapul.  Ez lesz egyedi könyvtárak között az Azure ad-ben.  Hello paraméter használata lehetővé teszi, hogy hello PowerShell-parancs toocomplete sikeresen megtörtént.

![Összevonási hiba](./media/active-directory-multiple-domains/convert.png)

Az új bmfabrikam.com tartomány hello-beállítások megnézi hello következő látható:

![Összevonási hiba](./media/active-directory-multiple-domains/settings.png)

Vegye figyelembe, hogy `-SupportMultipleDomain` hello nem változtatja meg más végpontok, amelyek továbbra is konfigurált toopoint tooour összevonási szolgáltatás adfs.bmcontoso.com.

Egy másik művelet, amely `-SupportMultipleDomain` does, hogy azt biztosítja, hogy hello AD FS rendszer tartalmazza-e az Azure AD kiállított jogkivonatokat a hello kibocsátó értéke. Hello felhasználók egyszerű Felhasználónevük hello tartomány része, és ez hello IssuerUri, azaz https://{upn utótag hello tartomány beállítását tesz} / adfs/services/megbízhatósági. 

Így során hitelesítési tooAzure AD vagy az Office 365, hello IssuerUri hello felhasználói jogkivonat eleme használt toolocate hello tartományhoz az Azure ad-ben.  Ha nem található egyező hello hitelesítése sikertelen lesz. 

Például, ha a felhasználói UPN van bsimon@bmcontoso.com, toohttp://bmcontoso.com/adfs/services/trust hello token AD FS problémák hello IssuerUri eleme lesz beállítva. Ez fog egyezni a hello Azure Active Directory beállítása, és a hitelesítés sikeres lesz.

hello az alábbiakban olvashat a logikát megvalósító hello egyéni jogcímszabályt:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


> [!IMPORTANT]
> A sorrend toouse hello - SupportMultipleDomain kapcsoló új tooadd tett kísérlet során, vagy alakítsa át a már hozzáadott tartomány, toohave kell beállítani az összevont megbízhatósági kapcsolathoz toosupport azokat eredetileg.  
> 
> 

## <a name="how-tooupdate-hello-trust-between-ad-fs-and-azure-ad"></a>Hogyan tooupdate hello megbízhatóság az AD FS és az Azure AD között
Ha nem állított be hello összevont megbízhatóság az AD FS és az Azure AD példányával között, szükség lehet a toore-megbízhatóság létrehozása.  Ennek az az oka eredetileg beállítása nélkül hello `-SupportMultipleDomain` paraméter, hello IssuerUri beállítása hello alapértelmezett értékkel.  A hello az alábbi képernyőfelvételen látható hello IssuerUri toohttps://adfs.bmcontoso.com/adfs/services/trust van beállítva.

Így mostantól, ha azt egy új tartomány sikeresen hozzáadta hello Azure AD portálon, majd próbálja meg a tooconvert használatával `Convert-MsolDomaintoFederated -DomainName <your domain>`, azt lekérése a következő hiba hello.

![Összevonási hiba](./media/active-directory-multiple-domains/trust1.png)

Ha megpróbál tooadd hello `-SupportMultipleDomain` kapcsoló azt fog kapni a következő hiba hello:

![Összevonási hiba](./media/active-directory-multiple-domains/trust2.png)

Egyszerűen próbált toorun `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` hello az eredeti tartomány is jelent hibát.

![Összevonási hiba](./media/active-directory-multiple-domains/trust3.png)

Hello lépésekkel tooadd további felső szintű tartomány alatt.  Ha már hozzáadott egy tartományhoz, és nem használta hello `-SupportMultipleDomain` eltávolítása és frissítése az eredeti tartomány hello lépései paraméter kezdődik.  Ha nem adja hozzá a legfelső szintű tartomány még indítható hello lépései a tartomány PowerShell az Azure AD Connect használatával való hozzáadását.

Használja a következő lépéseket tooremove hello Microsoft Online megbízhatósági hello és az eredeti tartomány.

1. Az AD FS összevonási kiszolgálón nyissa meg **AD FS kezelő.** 
2. Bontsa ki a hello bal oldali **megbízhatósági kapcsolatok** és **függő entitás Megbízhatóságai**
3. Jobb oldali hello törölje hello **Microsoft Office 365 Identitásplatformmal** bejegyzés.
   ![Távolítsa el a Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
4. A gépen, amelyen van [Active Directory modul Windows Powershellhez készült Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) telepítve van-e futtassa a következő hello: `$cred=Get-Credential`.  
5. Adja meg a összevonja hello Azure AD-tartomány hello felhasználónevet és jelszót egy globális rendszergazda
6. Adja meg a PowerShellben`Connect-MsolService -Credential $cred`
7. Adja meg a PowerShell `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Ez a hello eredeti tartomány.  Tartományok lenne a fenti hello használatával:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`

A következő lépéseket tooadd hello új legfelső szintű tartomány PowerShell-lel hello használata

1. A gépen, amelyen van [Active Directory modul Windows Powershellhez készült Azure](https://msdn.microsoft.com/library/azure/jj151815.aspx) telepítve van-e futtassa a következő hello: `$cred=Get-Credential`.  
2. Adja meg a összevonja hello Azure AD-tartomány hello felhasználónevet és jelszót egy globális rendszergazda
3. Adja meg a PowerShellben`Connect-MsolService -Credential $cred`
4. Adja meg a PowerShellben`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

A következő lépéseket tooadd hello új legfelső szintű tartomány Azure AD Connect használatával hello használata.

1. Indítsa el az Azure AD Connect hello asztalon vagy start menüben
2. "Egy további Azure AD-tartomány hozzáadása" Válasszon ![adnia egy további Azure AD-tartomány](./media/active-directory-multiple-domains/add1.png)
3. Adja meg az Azure AD és az Active Directory hitelesítő adatait.
4. Válassza ki a hello második tartományt összevonásra tooconfigure kívánja.
   ![Felvesz egy további Azure AD-tartomány](./media/active-directory-multiple-domains/add2.png)
5. Kattintson a telepítés

### <a name="verify-hello-new-top-level-domain"></a>Új legfelső szintű tartomány hello ellenőrzése
Hello PowerShell-paranccsal `Get-MsolDomainFederationSettings -DomainName <your domain>`megtekintheti hello IssuerUri frissítése.  hello az alábbi képernyőfelvétel hello összevonási beállítások lettek frissítve az az eredeti tartomány http://bmcontoso.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

Az új tartományon IssuerUri hello toohttps://bmfabrikam.com/adfs/services/trust van beállítva, és

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)

## <a name="support-for-sub-domains"></a>Altartományok támogatása
Amennyiben altartomány hozzáadásához miatt hello módon az Azure AD tartományok, azt öröklik a szülő hello hello-beállítások.  Ez azt jelenti, hogy hello IssuerUri toomatch hello szülők kell.

Így lehetővé teszi, hogy tegyük fel például, hogy rendelkezem bmcontoso.com, és adja meg az corp.bmcontoso.com.  Ez azt jelenti, hogy hello IssuerUri, az a felhasználó a corp.bmcontoso.com kell toobe **http://bmcontoso.com/adfs/services/trust.**  Hello szabványos szabály megvalósított felett az Azure AD, azonban hoz létre a jogkivonatot kibocsátó, az **http://corp.bmcontoso.com/adfs/services/trust.** ami nem fog egyezni a hello tartományi szükséges érték, és a hitelesítés sikertelen lesz.

### <a name="how-tooenable-support-for-sub-domains"></a>Hogyan támogatják a tooenable az altartományok
Az AD FS megbízható függő entitás a Microsoft Online rendelés toowork a hello körül toobe frissítve van szüksége.  toodo, konfigurálnia kell egyéni jogcímszabályt, így azt szalagok ki bármely altartományok hello felhasználói UPN-utótagot a hello egyéni kibocsátó érték kiszámításakor. 

a következő jogcím hello fog tegye a következőket:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

[!NOTE]
hello hello reguláris kifejezés utolsó számának beállítása hello hány szülő tartományt a legfelső szintű tartomány van. Itt az i bmcontoso.com rendelkezik, ezért két szülő tartomány szükség. Ha három szülő tartományok volt toobe tartott (vagyis: corp.bmcontoso.com), majd hello szám kellett volna lennie három. Széles Eventualy fel kell tüntetni, hello egyezés mindig lesz toomatch hello tartományok maximálisan engedélyezett. "a(z) {2,3}" fog egyezni a tartományok toothree (pl.: bmfabrikam.com és corp.bmcontoso.com).

A következő lépéseket tooadd hello egyéni jogcím toosupport altartományok használja.

1. Nyissa meg az AD FS-Szolgáltatáskezelés
2. Hello Microsoft Online RP megbízhatósági kattintson jobb gombbal, majd válassza ki a jogcímszabályok szerkesztése
3. Válassza ki a hello harmadik jogcímszabály, és cserélje le ![jogcímszabályok szerkesztése](./media/active-directory-multiple-domains/sub1.png)
4. Cserélje le a jelenlegi jogcím hello:
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
   
       with
   
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^.*@([^.]+\.)*?(?<domain>([^.]+\.?){2})$", "http://${domain}/adfs/services/trust/"));

    ![Cserélje ki a jogcímet](./media/active-directory-multiple-domains/sub2.png)

5. Kattintson az OK gombra.  Az Alkalmaz gombra.  Kattintson az OK gombra.  Zárja be az AD FS felügyeleti konzolt.

