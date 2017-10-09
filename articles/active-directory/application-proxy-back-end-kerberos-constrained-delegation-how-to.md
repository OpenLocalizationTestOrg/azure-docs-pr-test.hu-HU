---
title: "Az alkalmazásproxy alkalmazás toouse Kerberos által korlátozott delegálás aaaHow tooconfigure |} Microsoft Docs"
description: "Hogyan tooconfigure Kerberos által korlátozott delegálás az Azure AD alkalmazásproxy alkalmazáshoz."
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
ms.openlocfilehash: 93dac264ff1d3b99dead1d9c759c37c59373cbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application-toouse-kerberos-constrained-delegation"></a>Hogyan tooconfigure az alkalmazásproxy alkalmazás toouse Kerberos által korlátozott delegálás

hello módszer áll rendelkezésre elérése érdekében az alkalmazások némileg eltérőek lehetnek az alkalmazás tooapplication és, hogy Azure Application Proxy kínál hello kezdő verzióról jobb hello lehetőségek közül SSO toopublished a Kerberos által korlátozott delegálás (KCD). Ez az összekötő állomás esetén korlátozott konfigurált tooperform kerberos hitelesítési toobackend alkalmazások, felhasználók nevében.

hello tényleges eljárás a Kerberos által korlátozott Delegálás engedélyezéséhez viszonylag egyszerű, és általában a különböző összetevőkkel és hitelesítési folyamat, amely elősegíti a SSO igényelnek nem több, mint egy általános hello megértése. Keresés, ahol Kerberos által korlátozott Delegálás SSO nem működik megfelelően, információkat toohelp jó forrásai elhárításában, ritka lehet.

Ez a cikk, próbálja meg tooprovide egyetlen pont, hibaelhárításához és önálló javítása néhány hello leggyakoribb problémák látható segítségül hivatkozással. Hello azonos idő ajánlat további útmutatás t talál diagnosztizálása összetettebb hello és törlesztett végrehajtás.

Vegye figyelembe, hogy ez a cikk lehetővé teszi a következő feltételek hello:

-   Azure-alkalmazásproxy megfelelően van telepítve a [dokumentáció](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable) és általános toonon Kerberos által korlátozott Delegálás alkalmazásokat a várt módon működik.

-   hello közzétett célalkalmazás alapul kerberos IIS és a Microsoft végrehajtását.

-   hello kiszolgáló és az alkalmazás állomások egyetlen Active Directory-tartományban találhatók. Részletes tájékoztatást közötti tartományok és erdők forgatókönyvek megtalálhatók a [Kerberos által korlátozott Delegálás tanulmány](http://aka.ms/KCDPaper).

-   hello tulajdonos alkalmazás közzé van téve egy Azure-ban bérlői előtti hitelesítés engedélyezve van, és a felhasználók-azonosítóknak tooauthenticate tooAzure keresztül űrlap alapú hitelesítés. Gazdag ügyfél-hitelesítési forgatókönyvei nem vonatkozik ez a cikk, de a jövőbeli hello bármikor hozzáadni.

## <a name="prerequisites"></a>Előfeltételek

Azure-alkalmazásproxy is telepíthető az infrastruktúra lényegében bármilyen típusú, vagy a környezet és hello architektúrák minden bizonnyal szervezet tooorganization eltérőek lehetnek. Egyik leggyakoribb oka hello Kerberos által korlátozott Delegálás kapcsolatos problémák nem hello környezetekben, de meglehetősen egyszerű hibás konfigurációt vagy általános felügyeletet.

Ezért azt tanácsoljuk, mindig toostart annak biztosítása, hogy teljesül-összes hello Előfeltételek jelenjen meg a fő szerint [Kerberos által korlátozott Delegálás egyszeri bejelentkezés használatával hello alkalmazásproxy cikk](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) elindítása előtt a hibaelhárítás.

Különösen hello fejezet foglalkozik a Kerberos által korlátozott Delegálás konfigurálása 2012R2, mivel ez alkalmazza az előző verziók Windows de ugyanakkor számos egyéb szempontot szem előtt tartva nem alapvetően más megközelítést tooconfiguring Kerberos által korlátozott Delegálás:

-   A rendszer nem ritka, hogy az egy tartomány tagja server tooopen egy adott tartományvezérlőt egy biztonságos csatorna párbeszédpanelen. Ezután lépjen tooanother párbeszédpanel egy adott időpontban, így összekötő állomások általában nem szabad korlátozott toobeing képes toocommunicate csak adott helyi hely tartományvezérlők a.

-   Pont fölött hasonló toohello tartományok közötti forgatókönyvek, amelyek egy összekötő állomás tooDCs hello helyi hálózati szegélyhálózati kívül található közvetlen átirányítások támaszkodnak. Ebben a forgatókönyvben egyaránt van arra is engedélyezi a forgalom és újabb verziók esetében, amelyek megfelelnek a más megfelelő tartományokban, ellenkező esetben delegálás tooDCs fontos toomake sikertelen.

-   Ahol lehetséges, lehetőleg ne tároljon minden aktív IP-CÍMEK/Azonosítók eszközök közötti összekötő gazdagépek és -tartományvezérlő, mivel azok néha keresztül zavaró és zavarják a core keresztüli RPC-adatforgalmat

Mint amennyit szeretnénk tooresolve problémák gyors és hatékony, időbe telhet, ezért lehetőség szerint érdemes próbálja és a delegálás a legegyszerűbb forgatókönyvek hello. hello alkalmazná több változót, hello több előfordulhat, hogy a toocontend. Például korlátozza a tesztelési tooa egyetlen összekötőt időt takaríthat meg értékes, és további összekötők hello problémák megoldása után lehet hozzáadni.

Néhány környezeti tényezők is eredményezhetnek tooan probléma újra, ha lehetséges próbálja és hello architektúra tooa operációs rendszer minimális tesztelési minimalizálása érdekében. Például helytelenül konfigurált belső tűzfal hozzáférés-vezérlési listák nem ritka, ezért lehetőség van egy összekötő rögtön keresztül toohello tartományvezérlők és a háttéralkalmazás engedélyezett származó összes forgalmat. 

Hello abszolút legjobb hely tooposition összekötők ténylegesen lehet legközelebb tootheir célok. Egy kép tűzfal beágyazott Bár a tesztelés egyszerűen hozzáadja a szükségtelen összetettségét, és csak sikerült meghosszabbítani a vizsgálatok.

Igen milyen számít Kerberos által korlátozott Delegálás probléma ennek ellenére? Jól nincs több klasszikus jelzésének hibás és hello első jelentkezik a probléma általában manifest magukat a Kerberos által korlátozott Delegálás SSO hello böngésző.

   ![Helytelen a Kerberos által korlátozott konfigurációs hiba](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

   ![Engedélyezési toomissing engedélyek miatt sikertelen volt](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

felhasználói hozzáférés toohello alkalmazás összes viselniük hello tooperform SSO sikertelenek lesznek, és ezért elutasítja az azonos tünete hello.

## <a name="troubleshooting"></a>Hibaelhárítás

Hogyan majd hibaelhárítása hello probléma és a megfigyelt jelenségek függenek. Előtt fog minden további volna javasoljuk, hogy a következő hivatkozások, hello nem még rendelkezik olyan hasznos információkat tartalmaznak, mint:

-   [Alkalmazásproxy problémák és hibaüzenetek hibaelhárítása](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot)

-   [Kerberos-hibákhoz, és a jelenség](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-troubleshoot#kerberos-errors)

-   [Egyszeri bejelentkezési modellel működik ha helyszíni és felhőalapú Identitások nem azonosak.](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd#working-with-sso-when-on-premises-and-cloud-identities-are-not-identical)

Ha ez sokkal, majd hello fő probléma mindenképpen létezik. Szüksége toodig mélyebb, ezért először hello folyamat három különböző szakaszokat elháríthatja az elválasztó.

**Ügyfelek előhitelesítése** -hello külső felhasználó hitelesítéséhez tooAzure böngésző használatával.

Képes toopre alatt-hitelesítéshez tooAzure elengedhetetlen a Kerberos által korlátozott Delegálás SSO toofunction. Ez kell lehet tesztelni és címzett először, ha probléma merül fel. Vegye figyelembe, hogy hello előhitelesítési szakaszhoz hozzá van nincs kapcsolat tooKCD vagy hello közzétett alkalmazást. Viszonylag könnyen toocorrect ellentmondások által megerősítést ellenőrzése hello tulajdonos fiók létezik-e az Azure-ban meg kell, és nem le van tiltva/letiltva. a rendszer általában elegendő leíró toounderstand hello OK hello hiba választ hello böngészőben. Ha nem biztos a más kapcsolatos problémák elhárítása docs tooverify is ellenőrizheti.

**Delegálás szolgáltatás** -hello Azure Proxy connector beszerzése a kerberos szolgáltatásjegy a KDC (Kerberos kulcskiosztó központ), a felhasználók nevében.

a külső kommunikáció hello hello ügyfél és a hello Azure előtér között kell nincs hatással a Kerberos által korlátozott Delegálás megszüntethesse Önnek, másik győződjön meg arról, hogy működik. Ez a helyzet hello Azure Proxy szolgáltatás lehet a megadott felhasználói azonosító érvénytelen, amely lehet használt tooobtain a kerberos jegyet. Nélkül a Kerberos által korlátozott Delegálás nincs lehetőség, és sikertelen lesz.

Ahogy korábban említettük, hello böngésző hibaüzenetek általában megadja a néhány jó a dolgok miért nem működnek. Győződjön meg arról, hogy toonote hello Tevékenységazonosító és időbélyeg hello válaszként, ez lehetővé teszi toocorrelate hello viselkedés tooactual események hello Azure Proxy-eseménynaplóban találhatók.

   ![Helytelen a Kerberos által korlátozott konfigurációs hiba](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

És hello megfelelő bejegyzések látott hello Eseménynapló lesz látható a 13019 vagy 12027 eseményként is rögzíti. Található hello eseménynaplók összekötő **alkalmazási és Szolgáltatásnaplójában** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Összekötő**&gt;**Admin**.

   ![Alkalmazásproxy eseménynaplójából 13019 esemény](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

   ![Alkalmazásproxy eseménynaplójából 12027 esemény](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

-   Használjon egy A rekordot hello alkalmazás címe, és nem egy olyan CNAME rekordot a belső DNS-kiszolgáló

-   Újból ellenőrizze a hello összekötőt üzemeltető rendelkezik hello jogok toodelegate toohello cél fiókhoz tartozó egyszerű Szolgáltatásnevet, és hogy a kijelölt **bármely hitelesítési protokoll** van kiválasztva. Ez a fő tárgyalja [SSO konfigurációs cikk](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd)

-   Győződjön meg arról, hogy csak egyetlen példány futhat hello egyszerű Szolgáltatásnevet az Active Directory fennálló kiállításával egy **setspn - x** bármely tartományi tag gazdagépen cmd parancssorból

-   Ellenőrizze, hogy ha folyamatban van a tartományi házirend toosee kényszerített toolimit hello [kerberos kiállított jogkivonatokat mérete legfeljebb](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/), mert a jogkivonat beszerezni, ha az megakadályozása hello összekötő toobe túlzott található

A hálózati nyomkövetés rögzítése hello cseréjét hello összekötő állomás- és a KDC között hello ajánlott következő lépés az beszerzése több alacsony szintű részletes hello problémák lesz. Ezt a található [mélyreható kapcsolatos problémák elhárítása papír](https://aka.ms/proxytshootpaper).

Ha jegykezelési megfelelőnek tűnik, meg kell jelennie egy eseményre hello naplók figyelmezteti a felhasználókat arra, hogy a hitelesítés a 401-es visszaadó toohello alkalmazás miatt sikertelen volt. Ez általában azt jelenti, hogy hello célalkalmazás elutasítása a jegyet, ezért folytassa a következő szakasz a következő hello.

**A célalkalmazás** -hello fogyasztói hello összekötő által biztosított hello kerberos jegy

Ezen szakasza hello csatlakozó van várt toohave küldi a kerberos szolgáltatás jegy toohello háttér, fejlécként hello első alkalmazásigénylés belül.

-   Hello alkalmazással hello portálon megadott belső URL-ellenőrizze, hogy hello alkalmazás közvetlenül hello böngészőből hello összekötő állomáson elérhető. Ezután bejelentkezhet sikeresen megtörtént. Ezzel a részletek hello összekötő hibaelhárítás lapon található.

-   Továbbra is a hello összekötő gazdagépen, győződjön meg arról, hogy hello a hitelesítési hello böngésző között, és hello alkalmazás használja a kerberos hello következő tevékenységek végrehajtásával:

1.  A fejlesztői eszközök futtatása (**F12**) a Internet Explorer vagy [Fiddler](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) hello összekötő gazdagépről. Nyissa meg toohello alkalmazást hello belső URL-cím, és WWW engedélyezési fejléceket hello alkalmazásból hello válaszként visszaadott kínált hello ellenőrzéséhez vagy egyeztető tooensure vagy kerberos jelen. Egy későbbi kerberos blob hello válaszként visszaadott hello böngésző toohello az alkalmazás általában megkezdi a **YII**, ezért ez a kerberos Play alatt jól jelzi. NTLM hello ugyanakkor mindig indítsa el a **TlRMTVNTUAAB**, amely szól, amikor a Base64 kódolású anyag dekódolni NTLMSSP. Ha látja **TlRMTVNTUAAB** elején hello hello blob, ez azt jelenti, hogy van-e a Kerberos **nem** érhető el. Ha ez nem látható, a Kerberos valószínű érhető el.

  * Vegye figyelembe, ha Fiddler használatával ezt a módszert igényelnének ideiglenesen letilthatja az hello alkalmazás konfiguráció az IIS-ben a bővített védelmet.

     ![Hálózati hálózatfelügyeleti böngészőablakban](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

    *Ábra:* ez nem TIRMTVNTUAAB kezdődik, mert ez az, hogy rendelkezésre áll-e a Kerberos példa. Ez az YII nem kezdődő Kerberos Blob egy példát.

2.  NTLM ideiglenesen eltávolítása hello szolgáltatók listája IIS helyen, és hozzáférés alkalmazásában közvetlenül az Internet Explorer összekötő gazdagépen. Az NTLM már nem a hello szolgáltatók listája csak Kerberos használatával képes tooaccess hello alkalmazás kell lennie. Ha ez nem sikerül, majd, amelyek arra utalnak, hogy van-e probléma hello alkalmazást, és a Kerberos-hitelesítés nem működik.

Kerberos nem érhető el, ha ellenőrzés hello alkalmazás hitelesítési beállításait, hogy az IIS toomake egyezteti az alatta csak NTLM legfelső szerepel. (Nem Negotiate: kerberos vagy Negotiate: PKU2U). Csak továbbra is, ha Kerberos működőképességét.

   ![Windows-hitelesítésszolgáltatók](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)
   
-   A Kerberos és NTLM helyen lehetővé teszi, hogy most ideiglenesen letilthatja az előhitelesítési hello alkalmazás hello portálon. Tekintse meg, ha hozzá tud férni a hello internet hello külső URL-cím használatával. Rákérdezéses tooauthenticate kell lennie, és képes toodo kell lennie, ezért hello a fiókot használja a használt hello előző lépésben. Ha nem, ez az a probléma hello a háttéralkalmazás és nem a Kerberos által korlátozott Delegálás minden jelzi.

-   Most újra engedélyezi az előhitelesítési hello portálon, és a hitelesítéshez Azure szolgáltatáson keresztül próbált meg külső URL-CÍMEN keresztül tooconnect toohello alkalmazás. Ha az egyszeri bejelentkezés sikertelen volt, majd kell megjelennie hello böngészőt, valamint az esemény 13022 hello tiltott hibaüzenet:

    *Microsoft AAD alkalmazásproxy-összekötő nem tudja hitelesíteni hello felhasználót, mert hello háttérkiszolgáló válaszol tooKerberos hitelesítési kísérlet egy HTTP 401-es hiba miatt.*

    ![Tiltott hibaüzenetet HTTTP 401](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

-   Ellenőrizze, hello IIS alkalmazás tooensure hello konfigurált alkalmazáskészlet ugyanazt a fiókot, amely egyszerű Szolgáltatásnevet hello konfigurációja szemben az ad-ben, lépjen az IIS-ben, az alábbi ábra szemlélteti konfigurált toouse hello

    ![IIS alkalmazás konfigurációs ablak](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

    Miután eldöntötte, hello identitás, ki hello következőt egy cmd Rákérdezés toomake, ezt a fiókot a szóban forgó SPN hello mindenképpen van beállítva a. Például **setspn – q http/spn.wacketywack.com**

    ![SetSPN parancssori ablakban](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

-   Ellenőrizze, hogy hello portal hello alkalmazások beállításainak képest van meghatározva. SPN van hello azonos SPN hello cél AD fiók hello alkalmazás alkalmazáskészlet által használt konfigurált hello

   ![SPN-konfiguráció Azure-portálon](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)
   
-   Nyissa meg az IIS és a select hello **Konfigurációszerkesztő** hello alkalmazás lehetőséget, és keresse meg a túl**system.webServer/security/authentication/windowsAuthentication** toomake meg arról, hogy **UseAppPoolCredentials** tootrue van beállítva.

   ![Az IIS konfigurációs app készletek hitelesítőadat-beállítás](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

Ugyanakkor nem a Kerberos-műveletek hello a teljesítmény fokozása hasznos, is hagyja engedélyezve van a Kernel mód hatására hello hello jegyre kért szolgáltatás toobe visszafejteni a számítógépfiókot. Más néven: hello helyi rendszer, így a készlet tootrue break Kerberos által korlátozott Delegálás Ha hello alkalmazás kiszolgálófarm több kiszolgáló között.

-   Egy további ellenőrzést, akkor is érdemes lehet toodisable hello **kiterjesztett** védelmi túl. Eddig észlelt forgatókönyvek, ahol ez bizonyult toobreak engedélyezésekor a olyan speciális konfigurációk esetén a Kerberos által korlátozott Delegálás ahol hello alapértelmezett webhely almappa alkalmazás van közzétéve. Ez maga a névtelen hitelesítés csak, hello teljes párbeszédpanelek szürkén jelenik meg arra vonatkozóan, gyermekobjektum akkor nem kell öröklődés esetleges aktív beállításokat hagyja van konfigurálva. De ha lehetséges volna mindig javasoljuk, ezzel engedélyezve van, ezért feltétlenül tesztelése, de ne feledkezzen toorestore a tooenabled.

További ellenőrzést kell rendelkeznie elhelyezése, nyomon követése toostart használ a közzétett alkalmazást. Megkezdheti és további összekötők is fel léptetéses konfigurálva toodelegate, de ha olyan további majd akkor javasoljuk, hogy a a részletesebb műszaki útmutató olvasási [hello teljes körű útmutatót hibaelhárítása az Azure AD alkalmazás Proxy](https://aka.ms/proxytshootpaper)

Ha még nem tooprogress a problémát, a támogatási ehhez boldogok tooassist-nál nagyobb legyen, és továbbra is itt. Hozzon létre egy támogatási jegy közvetlenül hello portálon belül, és a mérnökök tooyou lesznek elérhetők.

## <a name="other-scenarios"></a>Egyéb forgatókönyvek

-   Azure alkalmazásproxy kéri a Kerberos-jegy kérelem tooan alkalmazásának elküldése előtt. 3. fél alkalmazásmódjai például a Tableau kiszolgáló nincs megelégedve a módszert, és nem vár több hagyományos az hello egyeztetések tootake ellenőrzéséhez. hello első kérelme, mert névtelen, így hello alkalmazás toorespond hello hitelesítési típussal, amely támogatja a 401-es keresztül.

-   Kettős Ugrás hitelesítés – általában esetekben használatos, ahol a kérelmet rétegzett, egy háttér és első vége, megkövetelését hitelesítést, például az SQL Reporting Services.

## <a name="next-steps"></a>Következő lépések
[Kerberos által korlátozott delegálás (KCD) konfigurálása a felügyelt tartományhoz](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-enable-kcd)
