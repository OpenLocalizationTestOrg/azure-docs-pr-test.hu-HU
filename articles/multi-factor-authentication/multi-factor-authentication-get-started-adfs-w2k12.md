---
title: az AD FS a Windows Server Server aaaMFA |} Microsoft Docs
description: "Ez a cikk ismerteti, hogyan tooget el az Azure többtényezős hitelesítés és az AD FS a Windows Server 2012 R2 és a 2016."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 57208068-1e55-45b6-840f-fdcd13723074
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/29/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 656785abcc63a020add765a86670b488a3b84b51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-toowork-with-ad-fs-in-windows-server"></a>Azure multi-factor Authentication kiszolgáló toowork konfigurálása az AD FS a Windows Server
Ha az Active Directory összevonási szolgáltatások (AD FS) és toosecure felhőalapú vagy helyszíni erőforrásait, Azure multi-factor Authentication kiszolgáló toowork AD FS konfigurálható. Ezzel aktiválja a kétlépéses ellenőrzést a nagy értékű végpontokon.

Ebben a cikkben azt mutatjuk be, hogyan használja az Azure Multi-Factor Authentication-kiszolgálót az AD FS-sel Windows Server 2012 R2 vagy Windows Server 2016 rendszeren. További információkért olvassa el, hogyan túl[felhőalapú és helyszíni erőforrások biztonságba helyezése az Azure multi-factor Authentication kiszolgáló az AD FS 2.0 használatával](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-ad-fs-with-azure-multi-factor-authentication-server"></a>A Windows Server AD FS védelme az Azure Multi-Factor Authentication-kiszolgálóval
Azure multi-factor Authentication kiszolgáló telepítésekor hello alábbi beállítások közül választhat:

* Azure multi-factor Authentication kiszolgáló hello helyileg telepíteni az AD FS ugyanarra a kiszolgálóra
* Helyileg telepítse hello Azure multi-factor Authentication adapter hello AD FS-kiszolgálón, és egy másik számítógépre telepítse a multi-factor Authentication kiszolgáló

Mielőtt elkezdené, vegye figyelembe a következő információk hello:

* Biztosan nem szükséges tooinstall Azure multi-factor Authentication kiszolgáló az AD FS-kiszolgálón. Azonban az AD FS a Windows Server 2012 R2 vagy Windows Server 2016, az AD FS-t futtató hello multi-factor Authentication adapter kell telepítenie. Ha valamelyik támogatott verzióra, és telepítése az AD FS-adapter hello külön-külön az AD FS összevonási kiszolgáló hello kiszolgáló is telepíthető egy másik számítógépre. Hello lásd a következő eljárások toolearn hogyan tooinstall hello adapter külön-külön.
* Ha szervezete szöveges üzenet vagy mobilalkalmazás az ellenőrzési módszereket használ, vállalati beállítások között megadott hello karakterláncok tartalmaz egy helyőrző <$*application_name*$>. Az MFA-kiszolgáló 7.1-es verziójában megadhat egy alkalmazásnevet, amely felváltja a helyőrzőt. V7.0 vagy régebbi a helyőrző nem automatikusan helyébe hello AD FS-adapter használatakor. A régebbi verziókat távolítsa el hello helyőrző hello karakterláncokat az AD FS védelme.
* hello toosign a bejelentkezéshez használt fiók felhasználói jogok toocreate biztonsági csoportok kell rendelkeznie az Active Directory szolgáltatásban.
* hello multi-factor Authentication AD FS-adapter telepítővarázsló létrehoz egy biztonsági csoportot az Active Directory-példány a PhoneFactor Admins. Ezt követően hozzáadja a szolgáltatásfiók hello AD FS összevonási szolgáltatás toothis csoport. A tartományvezérlőn győződjön meg arról, hogy hello PhoneFactor-Adminisztrátorok csoport tényleg létrejött, és adott hello AD FS-szolgáltatásfiók tagja ennek a csoportnak. Ha szükséges, adja hozzá manuálisan hello AD FS szolgáltatás fiók toohello PhoneFactor-Adminisztrátorok csoporthoz a tartományvezérlőn.
* A webszolgáltatási SDK hello hello felhasználói portál telepítésével kapcsolatos információkért olvassa el [hello felhasználói portál telepítése az Azure multi-factor Authentication kiszolgáló.](multi-factor-authentication-get-started-portal.md)

### <a name="install-azure-multi-factor-authentication-server-locally-on-hello-ad-fs-server"></a>Telepítse az Azure multi-factor Authentication kiszolgáló helyben hello AD FS-kiszolgálón
1. Töltse le és telepítse az Azure Multi-Factor Authentication-kiszolgálót az AD FS-kiszolgálóra. A telepítéssel kapcsolatos információkért olvassa el [az Azure Multi-Factor Authentication-kiszolgáló használatának első lépéseit bemutató cikket](multi-factor-authentication-get-started-server.md).
2. Hello Azure multi-factor Authentication kiszolgáló kezelőkonzolján kattintson az hello **AD FS** ikonra. Hello beállítások is választhatók **felhasználó regisztrálásának engedélyezése** és **engedélyezése a felhasználók tooselect metódus**.
3. Válassza ki a szervezet szeretné toospecify további beállításokat.
4. Kattintson az **AD FS-adapter telepítése** gombra.
   
   <center>![Felhő](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>

5. Ha hello Active Directory ablak jelenik meg, ez azt jelenti, két dolgot. A számítógép tartományhoz csatlakoztatott tooa, és hello Active Directory konfigurálása hello AD FS-adapter és hello multi-factor Authentication szolgáltatás közötti kommunikáció biztonságához még nincs konfigurálva. Kattintson a **következő** tooautomatically a konfiguráláshoz, vagy válassza ki a hello **Active Directory automatikus konfigurálásának átugrása és konfigurálja a beállításokat manuálisan** jelölőnégyzetet. Kattintson a **Tovább** gombra.
6. Ha a helyi csoport a windows hello jelenik meg, ez azt jelenti, két dolgot. A számítógép nincs tartományhoz csatlakoztatott tooa, és hello helyi csoport konfigurálása hello AD FS-adapter és hello multi-factor Authentication közötti kommunikáció biztonságához még nincs konfigurálva. Kattintson a **következő** tooautomatically a konfiguráláshoz, vagy válassza ki a hello **helyi csoport automatikus konfigurálásának átugrása és konfigurálja a beállításokat manuálisan** jelölőnégyzetet. Kattintson a **Tovább** gombra.
7. Hello telepítővarázslójának elindításához kattintson **következő**. Azure multi-factor Authentication kiszolgáló hello PhoneFactor-Adminisztrátorok csoport hoz, és hozzáadja az AD FS hello szolgáltatás fiók toohello PhoneFactor-Adminisztrátorok csoporthoz.
   <center>![Felhő](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. A hello **a telepítő indítása** kattintson **következő**.
9. Hello multi-factor Authentication AD FS-adapter telepítője, kattintson **következő**.
10. Kattintson a **Bezárás** amikor hello telepítése befejeződött.
11. Ha hello adapter telepítve van, az AD FS-sel kell regisztrálni. Nyissa meg a Windows Powershellt, és futtassa a következő parancs hello:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
    <center>![Felhő](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. toouse az újonnan regisztrált adapter hello az AD FS globális hitelesítési házirend módosítása. Hello AD FS felügyeleti konzolon lépjen a toohello **hitelesítési házirendek** csomópont. A hello **multi-factor Authentication** területen kattintson a hello **szerkesztése** csatolása a következő toohello **globális beállítások** szakasz. A hello **globális hitelesítési házirend szerkesztése** ablakban válassza ki **multi-factor Authentication** lehetőséget további hitelesítési módszerként, majd kattintson **OK**. hello adapter WindowsAzureMultiFactorAuthentication néven van regisztrálva. Indítsa újra a hello regisztrációs tootake hatás hello AD FS szolgáltatást.

<center>![Felhő](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Ezen a ponton a multi-factor Authentication kiszolgáló be van állítva toobe egy további hitelesítési szolgáltató toouse az AD FS.

## <a name="install-a-standalone-instance-of-hello-ad-fs-adapter-by-using-hello-web-service-sdk"></a>Egy önálló példányát hello AD FS-adapter telepítése hello webszolgáltatási SDK használatával
1. A multi-factor Authentication kiszolgálót futtató kiszolgálón hello hello webszolgáltatási SDK telepítése
2. Fájlok másolása hello következő hello \Program Files\Multi-Factor Authentication kiszolgáló toohello címtárkiszolgálóra tervezett tooinstall hello AD FS-adapter:
   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config
3. Futtassa a hello MultiFactorAuthenticationAdfsAdapterSetup64.msi telepítőfájlt.
4. Hello multi-factor Authentication AD FS-adapter telepítője, kattintson **következő** toostart hello telepítését.
5. Kattintson a **Bezárás** amikor hello telepítése befejeződött.

## <a name="edit-hello-multifactorauthenticationadfsadapterconfig-file"></a>Szerkessze a MultiFactorAuthenticationAdfsAdapter.config fájlt hello
Kövesse az alábbi lépéseket tooedit hello MultiFactorAuthenticationAdfsAdapter.config fájlt:

1. Set hello **UseWebServiceSdk** csomópont túl**igaz**.  
2. Hello értékét **webservicesdkurl paraméternél** toohello hello multi-factor Authentication webszolgáltatási SDK URL-CÍMÉT. Például: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx*, ahol *certificatename* hello neve, a tanúsítványt.  
3. Szerkessze a hello Register-MultiFactorAuthenticationAdfsAdapter.ps1 parancsfájlt hozzáadásával *- ConfigurationFilePath &lt;elérési&gt;*  hello toohello végét `Register-AdfsAuthenticationProvider` parancsot, ahol  *&lt;elérési&gt;*  hello teljes elérési útja toohello MultiFactorAuthenticationAdfsAdapter.config fájlt.

### <a name="configure-hello-web-service-sdk-with-a-username-and-password"></a>Hello webszolgáltatási SDK konfigurálása felhasználónévvel és jelszóval
A webszolgáltatási SDK hello konfigurálása két lehetőség áll rendelkezésre. hello először egy felhasználónévvel és jelszóval, hello második ügyféltanúsítvánnyal. Kövesse az alábbi lépéseket az első lehetőség hello, vagy hagyja ki azokat, amelyek a hello második.  

1. Hello értékét **WebServiceSdkUsername** tooan fiók, amely hello PhoneFactor-Adminisztrátorok biztonsági csoport tagja. Használjon hello &lt;tartomány&gt;&#92;&lt; felhasználónév&gt; formátumban.  
2. Hello értékét **WebServiceSdkPassword** toohello megfelelő fiók jelszavát.

### <a name="configure-hello-web-service-sdk-with-a-client-certificate"></a>Hello webszolgáltatási SDK konfigurálása ügyféltanúsítvánnyal
Nem toouse felhasználónevet és jelszót, kövesse a lépéseket tooconfigure hello webszolgáltatási SDK ügyféltanúsítvánnyal.

1. Szerezzen be ügyféltanúsítványt a webszolgáltatási SDK hello futtató hello kiszolgáló hitelesítésszolgáltatójától. Ismerje meg, hogyan túl[Ügyféltanúsítványok beszerzése](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importálás hello ügyfél tanúsítvány toohello helyi számítógép személyes tanúsítványtárolójában hello webszolgáltatási SDK szolgáltatást futtató hello kiszolgálón. Győződjön meg arról, hogy hello hitelesítésszolgáltató nyilvános tanúsítványa a megbízható legfelső szintű tanúsítványok tanúsítványtárolójába.  
3. Hello nyilvános és titkos kulcsok hello ügyfél tooa .pfx tanúsítványfájl exportálása.  
4. A Base64 formátumban tooa .cer fájl hello a nyilvános kulcsának exportálásához.  
5. A Kiszolgálókezelőben, győződjön meg arról, hogy hello webkiszolgáló (IIS) \Web Server\Security\IIS ügyféltanúsítvány-hozzárendeléses hitelesítés funkció telepítve van. Ha nincs telepítve, válassza ki a **szerepkörök és szolgáltatások hozzáadása** tooadd ezt a szolgáltatást.  
6. Az IIS-kezelőben kattintson duplán **Konfigurációszerkesztő** hello webszolgáltatási SDK virtuális könyvtárat tartalmazó hello webhelyen. Ez egy fontos tooselect hello webhely, nem hello virtuális könyvtárat.  
7. Nyissa meg toohello **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** szakasz.  
8. Készlet túl engedélyezett**igaz**.  
9. A onetoonecertificatemappingsenabled túl**igaz**.  
10. Kattintson a hello **...**  következő toooneToOneMappings gombra, majd hello **Hozzáadás** hivatkozásra.  
11. Nyissa meg a korábban exportált hello Base64 .cer kiterjesztésű fájlba. Távolítsa el a *-----BEGIN CERTIFICATE-----* és az *-----END CERTIFICATE-----* elemet, továbbá minden sortörést. Másolja a hello eredményül kapott karakterláncot.  
12. Állítsa a hello az előző lépésben másolt tanúsítvány toohello karakterlánc.  
13. Készlet túl engedélyezett**igaz**.  
14. Állítsa be, amely tagja a PhoneFactor Admins biztonsági csoportba hello felhasználónév tooan fiókot. Használjon hello &lt;tartomány&gt;&#92;&lt; felhasználónév&gt; formátumban.  
15. Állítsa be a hello jelszó toohello megfelelő fiók jelszavát, és zárja be a Konfigurációszerkesztő.  
16. Kattintson a hello **alkalmaz** hivatkozásra.  
17. Hello webszolgáltatási SDK virtuális könyvtárát, kattintson duplán **hitelesítési**.  
18. Győződjön meg arról, hogy az ASP.NET megszemélyesítés és az egyszerű hitelesítés beállítása túl**engedélyezve**, és minden más elemet túl beállított**letiltott**.  
19. Hello webszolgáltatási SDK virtuális könyvtárát, kattintson duplán **SSL-beállítások**.  
20. Az ügyféltanúsítványok beállításhoz adja túl**elfogadás**, és kattintson a **alkalmaz**.  
21. Másolja az exportált korábbi toohello futtató kiszolgáló esetében az AD FS-adapter hello hello .pfx fájlt.  
22. Importálás hello .pfx fájl toohello helyi számítógép személyes tanúsítványtárolójába.  
23. Kattintson a jobb gombbal, és válassza ki **titkos kulcsok kezelése**, és majd adjon olvasási hozzáférési toohello használt fiókkal toosign toohello AD FS szolgáltatásban.  
24. Nyissa meg a hello ügyfél tanúsítványt, és másolja hello ujjlenyomat hello **részletek** fülre.  
25. Hello MultiFactorAuthenticationAdfsAdapter.config fájlt, állítson be **WebServiceSdkCertificateThumbprint** hello előző lépésben másolt toohello karakterlánc.  

Végezetül tooregister hello adapter, futtassa a hello \Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 parancsfájlt a PowerShell. hello adapter WindowsAzureMultiFactorAuthentication néven van regisztrálva. Indítsa újra a hello regisztrációs tootake hatás hello AD FS szolgáltatást.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Az Azure AD-erőforrások AD FS-sel való védelme
toosecure a felhő erőforrás olyan jogcímeket szabályt beállítani, hogy az Active Directory összevonási szolgáltatások hello multipleauthn jogcím bocsát ki, amikor a felhasználó sikeresen elvégzi a kétlépéses ellenőrzést. A jogcím átadódik tooAzure AD. Ez az eljárás toowalk hello lépéseit követve:

1. Nyissa meg az AD FS felügyeleti konzolt.
2. Hello bal oldalon válassza ki a **függő entitás Megbízhatóságai**.
3. Kattintson a jobb gombbal a **Microsoft Office 365 Identity Platform** elemre, és válassza a **Jogcímszabályok szerkesztése…** lehetőséget.

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. A Kiadás átalakítási szabályai részben kattintson a **Szabály hozzáadása** elemre.

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. Az átalakítási Jogcímszabály szabály varázsló hello, válassza a **továbbítása vagy szűrése egy bejövő jogcím** hello legördülő listán, és kattintson **következő**.

   ![Felhő](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Adjon nevet a szabálynak.
7. Válassza ki **hitelesítési módszerek hivatkozásai** , hello bejövő jogcím típusa.
8. Válassza **Az összes jogcímérték továbbítása** lehetőséget.
    ![Átalakítási jogcímszabály hozzáadása varázsló](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. Kattintson a **Befejezés** gombra. Zárja be a hello AD FS felügyeleti konzolon.

## <a name="related-topics"></a>Kapcsolódó témakörök
Hibaelhárítással kapcsolatos segítségért tekintse meg a hello [Azure multi-factor Authentication – gyakori kérdések](multi-factor-authentication-faq.md)
