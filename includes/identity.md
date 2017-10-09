Éppen olyan fontos hello nyilvános felhőben, mert a helyszíni identitás kezelése. Ennek toohelp, Azure támogatja több különböző felhőalapú identitás technológiák. Ezek tartalmazzák:

* Windows Server Active Directory (általában csak az AD-nevezett) használatával létrehozott Azure virtuális gépek virtuális gépek hello felhőben futtatása. Ez a megközelítés szabálykészletében Azure tooextend használatakor a helyszíni adatközpontját hello felhőbe.
* Használhatja az Azure Active Directory toogive a felhasználók egyszeri bejelentkezésre túl[(SaaS) szolgáltatott szoftver](https://azure.microsoft.com/overview/what-is-saas/) alkalmazások. A Microsoft Office 365 ezt a technológiát használja, például és Azure-ban vagy más felhőalapú platformokon futó alkalmazásokhoz is használható.
* Hello az alkalmazások felhőalapú vagy helyszíni Azure Active Directory hozzáférés-vezérlés toolet felhasználói bejelentkezés Facebook, Google, a Microsoft és egyéb identitás-szolgáltatóktól származó identitásokkal használhatja.

A cikkből megtudhatja, mind a három ezek közül.

## <a name="table-of-contents"></a>Tartalomjegyzék
* [Windows Server Active Directory-beli virtuális gépek](#adinvm)
* [Az Azure Active Directoryval](#ad)
* [Az Azure Active Directory hozzáférés-vezérlés használatával](#ac)

## <a name="adinvm"></a>Windows Server Active Directory-beli virtuális gépek
Az Azure virtuális gépeken futó Windows Server AD helyileg futó hasonlítanak. [1. ábra](#fig1) ez megjelenésének jellemző példa látható.

![Az Azure Active Directory, a virtuális gépen](./media/identity/identity_01_ADinVM.png)

<a name="Fig1"></a>1. ábra: A Windows Server Active Directory Azure virtuális hálózat használatával csatlakozik az Azure virtuális gépek tooan szervezet helyszíni adatközpontban futtathatja.

Hello példában is látható itt Windows Server AD fut az Azure virtuális gépek hello platformon IaaS technológia használatával létrehozott virtuális gépeken. Virtuális gépeken, és néhány egyéb használata Azure-beli virtuális hálózathoz csatlakozó virtuális hálózati tooan olyan helyszíni adatközpontban vannak csoportosítva. virtuális hálózati hello kivágja ki egy csoportot a felhő virtuális gépek hello a helyszíni hálózat virtuális magánhálózati (VPN) kapcsolaton keresztül kommunikálnak. Ennek során Ez engedélyezi, hogy ezeket az Azure virtuális gépek tűnik egy másik alhálózat toohello olyan helyszíni adatközpontban. Hello. ábrán látható, két virtuális gépek futnak a Windows Server AD-tartományvezérlő. hello hello virtuális hálózatban lévő más virtuális gépek előfordulhat, hogy alkalmazásokat futtatnak, például a SharePoint, vagy használja más módon, például a fejlesztéshez és teszteléshez. hello olyan helyszíni adatközpontban két Windows Server AD-tartományvezérlő is futtatja.

Nincsenek a helyszínen futó az hello felhőben csatlakozó hello tartományvezérlők számos lehetőség közül választhat:

* Ellenőrizze az összes egyetlen Active Directory-tartomány része.
* Hozzon létre külön AD tartományok helyszíni és hello részét képező hello felhőben ugyanabban az erdőben.
* Hozzon létre külön AD-erdőkkel hello felhőben és helyszíni, majd kösse a hello erdők erdők közötti bizalmi kapcsolatok vagy a Windows Server Active Directory összevonási szolgáltatások (AD FS), amely az Azure virtuális gépeken is futtatható.

Függetlenül a választott történik, a rendszergazda győződjön meg arról, hogy a helyszíni felhasználók érkező hitelesítési kérések lépjen toocloud tartományvezérlők csak akkor, ha szükséges, mivel hello hivatkozás toohello felhő valószínűleg toobe lassabb, mint a helyszíni hálózatokban. Egy másik tényező tooconsider csatlakozó felhőben és helyszíni tartományvezérlők a replikáció által generált hello forgalom. Tartományvezérlők hello felhőben jellemzően a saját AD hely, amely lehetővé teszi, hogy egy rendszergazda tooschedule milyen gyakran történik a replikáció. Bár nem bájtok küldött, amely hatással lehet a hello rendszergazda replikációs lehetőségek egy Azure-adatközpontban kívül elküldött forgalom Azure költségekkel. Érdemes is mutat, hogy Azure támogatja a saját tartománynévrendszer (DNS), amíg ez a szolgáltatás (például a dinamikus DNS- és SRV-rekordok támogatása) Active Directory által igényelt funkciók hiányzik. Emiatt az Azure-on futó Windows Server AD szükséges hello felhőben saját DNS-kiszolgálók beállítását.

Windows Server AD az Azure virtuális gépeken futó is célszerű több különböző helyzetekben. Néhány példa:

* Azure virtuális gépek használatakor a saját adatközpont kiterjesztése hello felhő helyi tartományi vezérlők toohandle műveleteket, mint az integrált Windows-hitelesítés kéréseket, és az LDAP-lekérdezések igénylő alkalmazások előfordulhat, hogy futtatja. A SharePoint, például együttműködő gyakran az Active Directoryban, és úgy is lehetséges toorun egy SharePoint-farm Azure-ban egy helyszíni címtárat, hello felhőben tartományvezérlők beállításával jelentősen javítja a teljesítményt. (Azt, hogy ez nem feltétlenül szükséges, azonban fontos toorealize, bőven alkalmazások csak a helyszíni tartományvezérlők használatához hello felhőben sikeresen futtatható.)
* Tegyük fel, hogy egy közösségihez fiókiroda nem rendelkezik hello erőforrások toorun saját tartományvezérlők. Napjainkban a felhasználóknak kell hitelesítenie a hello toodomain vezérlők hello world másik oldalán - bejelentkezések lassú. Azure Active Directory szorosabb Microsoft-adatközpontban fut felgyorsítható a anélkül, hogy további kiszolgálókat hello fiókirodában.
* Egy szervezet által használt Azure vész-helyreállítási előfordulhat, hogy egy kis készletét aktív virtuális gépek hello felhőben, beleértve a tartományvezérlő karbantartása. Majd azok elő tooexpand ezen a helyen, szükséges tootake keresztül máshol hibák.

Egyéb lehetőségek is vannak. Például még nem szükséges a Windows Server AD a hello felhő tooan tooconnect helyszíni adatközpontban. Ha toorun egy meghatározott felhasználók szolgáltatott SharePoint-farm, például, akik szeretné-e jelentkezni kizárólag felhőalapú felhasználókkal, létrehozhat egy különálló erdőben az Azure-on. Ez a technológia használatát attól függ, hogy a célok vannak. (Az Azure-ral, használja a Windows Server AD útmutatást részletes [talál](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Az Azure Active Directoryval
SaaS-alkalmazásokhoz egyre nagyobb közös, felmerül a nyilvánvaló kérdés: milyen címtárszolgáltatás kell ezek a felhőalapú alkalmazások használnak? A Microsoft választ toothat kérdés az Azure Active Directoryban.

Ez a címtárszolgáltatás használatát mutatja be hello felhő két fő lehetőség áll rendelkezésre:

* Egyéni felhasználók számára, és csak az SaaS-alkalmazásokhoz használó szervezetek támaszkodhat Azure Active Directory, az egyetlen címtárszolgáltatás.
* A szervezetek, futtassa a Windows Server Active Directory a helyszíni címtár tooAzure Active Directory connect, majd toogive használja a felhasználók az egyszeri bejelentkezés tooSaaS alkalmazások.

[2. ábra](#fig2) hello szemlélteti első két beállítás, ahol Azure Active Directory az összes szükséges.

![Az Azure Active Directory, a virtuális gépen](./media/identity/identity_02_AD.png)

<a name="fig2"></a>2. ábra: Az Azure Active Directory lehetővé teszi egy szervezet felhasználói számára egyszeri bejelentkezés tooSaaS alkalmazások, beleértve az Office 365.

Hello. ábrán látható, akkor az Azure AD egy olyan több-bérlős szolgáltatás. Ez azt jelenti, hogy egyidejűleg támogatja a sok különböző szervezetek, azok a felhasználók directory információt. Ebben a példában A szervezet egy felhasználó próbál tooaccess egy SaaS-alkalmazáshoz. Ez az alkalmazás az Office 365, például a SharePoint Online része lehet, illetve előfordulhat, hogy valami mást,-nem Microsoft-alkalmazások is használhatják ezt a technológiát. Mivel az Azure AD hello SAML 2.0-s protokollt támogatja, minden olyan szükséges egy alkalmazás képessége toointeract hello használ az iparági szabványnak. (Valójában alkalmazásokat, amelyek használják az Azure AD futtatható bármely adatközpontban, nem csak egy Azure-adatközpontban található.)

hello folyamat akkor kezdődik, amikor hello felhasználó fér hozzá SaaS-alkalmazás (1. lépés). toouse az alkalmazás hello felhasználói Azure AD által kiadott tokennek kell megjeleníteni.

Ez a token hello felhasználói azonosító információkat tartalmaz, és digitális aláírásával az Azure AD. tooget hello jogkivonatot, hello felhasználó hitelesíti magát tooAzure AD, adja meg a felhasználónevet és jelszót (2. lépés). Az Azure AD majd szükség hello jogkivonatot ad vissza (3. lépés).

Ez a token küldi el a rendszer toohello SaaS-alkalmazáshoz (4. lépés), amely hello jogkivonat aláírást ellenőrzi és annak tartalmát (5. lépés) használja. Általában hello alkalmazás hello azonosító adatok hello jogkivonatot tartalmaz toodecide milyen információkat hello használhatók tooaccess használja, és lehet, hogy egyéb módon.

Ha hello alkalmazás mint mi található hello token hello felhasználóval kapcsolatos további információra van szüksége, azt kérhetnek ez hello Azure AD Graph API-val (6. lépés) az Azure AD-ről. Az Azure AD hello kezdeti verziójában, directory-séma hello használata rendkívül egyszerű: Ebben az esetben a felhasználók és a csoportok és a közöttük lévő kapcsolatok tartalmaz. Alkalmazások használhatják a felhasználók közötti kapcsolatok kapcsolatos információk toolearn. Tegyük fel, hogy egy alkalmazást kell végrehajtani, aki a felhasználó kezelője toodecide e hozzáférése toosome adattömb tooknow. Az Azure AD Graph API hello keresztül lekérdezésével kaphat.

hello Graph API szokásos RESTful protokollt, így a legtöbb ügyfelekről, beleértve a mobileszközök egyszerű toouse használ. hello API OData műveleteket, mint egy lekérdezési nyelv toolet ügyfelek hozzáférési adatok hozzáadása a leghasznosabb módon által meghatározott hello extensions is támogatja. (OData bővebben lásd: [bevezetéséről OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Mivel hello Graph API-val kapcsolatos felhasználók kapcsolatai használt toolearn, hello közösségi graph hello Azure AD-sémát egy adott szervezet (éppen ezért nevezik hello Graph API-val) a beágyazott megértéséhez alkalmazások lehetővé teszi. És maga tooauthenticate tooAzure AD Graph API kér, az alkalmazás OAuth 2.0 használja.

Ha a szervezet nem használja a Windows Server Active Directory – helyszíni kiszolgálók vagy tartományok - van, kizárólag felhőalapú alkalmazásokhoz az Azure AD használatára támaszkodik, csak a felhőalapú címtárban használatával ad hello vállalkozás felhasználók egyszeri bejelentkezés tooall közülük. Még amíg ez a forgatókönyv gyakori lekérdezi minden nap, a legtöbb szervezet továbbra is használja a helyi tartományokat létrehozni Windows Server Active Directory. Az Azure AD egy hasznos szerepkör tooplay rendelkezik, valamint-ként [3. ábra](#fig3) jeleníti meg.

![A virtuális gép az Azure Active Directory](./media/identity/identity_03_AD.png)
<a id="fig3"></a>3. ábra: a szervezetek is összevonni Windows Server Active Directory Azure Active Directory toogive a felhasználók egyszeri bejelentkezésre tooSaaS alkalmazások.

Ebben a forgatókönyvben a szervezet B egy felhasználó egy SaaS-alkalmazás által tooaccess. Mielőtt, hello szervezet directory rendszergazdák automatikusan összevonási kapcsolatot kell létesítenie az Azure AD használata az AD FS-ben hello. ábrán látható módon. Ezeket a rendszergazdák is konfigurálnia kell a adatszinkronizálás hello szervezet helyszíni Windows Server AD és az Azure AD között. Ez automatikusan átmásolja felhasználói hello a helyszíni címtár tooAzure AD. Figyelje meg, ez lehetővé teszi: A gyakorlatban hello szervezet van kiterjeszti a helyszíni címtár hello felhő. Hello szervezet kombinálja a Windows Server AD és az Azure AD ily módon ad egy címtárszolgáltatás közben továbbra is fennáll a erőforrásigényét mind a helyszíni és felhőben hello egyetlen egységként kezelhető.

az Azure AD toouse, hello felhasználó először jelentkezik be a tooher a helyszíni Active Directory-tartomány megszokott módon (1. lépés). Ha ő megpróbál tooaccess hello SaaS-alkalmazáshoz (2. lépés), hello összevonási folyamat ehhez az alkalmazáshoz (3. lépés) jogkivonatot kibocsátó rá, hogy az Azure AD eredményez. (További összevonási működésével kapcsolatban: [Windows jogcímalapú identitás: technológiák és forgatókönyvek](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Előtt, a token hello felhasználói azonosító információkat tartalmaz, és az Azure ad digitális aláírását. Ez a token küldi el a rendszer toohello SaaS-alkalmazáshoz (4. lépés), amely hello jogkivonat aláírást ellenőrzi és annak tartalmát (5. lépés) használja. Hello előző esetben hello SaaS-alkalmazás használhat hello Graph API toolearn további információk a felhasználóhoz, ha, és szükség esetén (6. lépés).

Az Azure AD ma, nem a teljes cserét hajt végre a helyi Windows Server AD. Mint már említettük hello címtárak egy sokkal egyszerűbb sémával rendelkezik, és műveleteket, mint a csoportházirend, hello képességét toostore információ a gépeket, és támogatja az LDAP is hiányzik. (Valójában egy Windows-számítógép nem lehet a felhasználók bejelentkezését tooit semmi, de az Azure AD használatával konfigurált toolet – ez nem támogatott.) Ehelyett hello eredetileg kitűzött célokat az Azure AD közé tartozik, ha engedélyezi, hogy a vállalati felhasználók alkalmazásokat hello felhőben anélkül, hogy egy külön bejelentkezési karbantartása és felszabadítása a helyszíni manuális szinkronizálása a helyszíni címtár és a címtárban minden a szervezet által használt SaaS-alkalmazáshoz. Az idő múlásával azonban várhatóan a felhő directory szolgáltatás tooaddress a lehetséges forgatókönyvek bővítése.

## <a name="ac"></a>Az Azure Active Directory hozzáférés-vezérlés használatával
Felhőalapú identitás technológiák használt toosolve számos különféle problémák lehetnek. Az Azure Active Directory biztosíthat egy szervezet felhasználói számára egységes bejelentkezés toomultiple SaaS-alkalmazásokhoz, például. De hello cloud identity technológiák más módokon is használható.

Tegyük fel például, hogy az alkalmazás által a felhasználói bejelentkezés használatával több által kiállított jogkivonatokat toolet *identitás-szolgáltatóktól (IdPs)*. Sok különböző identitás-szolgáltatóktól Facebook, Google, a Microsoft és más, beleértve napjainkban létezik, és alkalmazások gyakran tudassa a felhasználókkal, jelentkezzen be valamelyik ezeket az identitásokat. Miért kell egy alkalmazás többet jelzi toomaintain saját felhasználók és jelszavak listája Ha inkább támaszkodhat a már meglévő identitások? Meglévő identitások elfogadása teszi élettartama egyszerűbb, mind a felhasználók számára, akik egy felhasználónév és jelszó tooremember kevesebb mint, valamint a hello személyek hello alkalmazás, akik nem szükséges toomaintain létrehozása a saját felhasználónevek és jelszavak listája.

Azonban minden identitásszolgáltató kiállított jogkivonat bizonyos típusú, míg a jogkivonatok nem szabványos – minden IdP saját formátuma. Ezenkívül a jogkivonatok hello információkat is nem szabványos. Egy kiállító, azaz tooaccept jogkivonatok szándékozó alkalmazás, a Facebook, a Google és a Microsoft hello kérdéssel ezek különböző formátumokban mindegyikének egyedi kódot toohandle rendszerekben tapasztalt.

De miért ehhez? Miért nem hozzon létre, amelyek egy közös azonosító adatok ábrázolása a egyetlen token formátuma hozhat létre a köztes? Ezt a módszert akkor egyszerűbbé teszik a élettartama hello alkalmazásokat létrehozó fejlesztőknek, mert azokat most toohandle csak egyféle jogkivonat a. Az Azure Active Directory hozzáférés-vezérlés pontosan ezt végzi, és hello felhő közvetítő biztosít a különböző jogkivonatok használata. [4. ábra](#fig4) bemutatja, hogyan működik?

![A virtuális gép az Azure Active Directory](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>4. ábra: Azure Active Directory hozzáférés-vezérlés megkönnyíti az alkalmazások tooaccept identitás jogkivonatok más identitásszolgáltatók által kibocsátott.

hello folyamat akkor kezdődik, amikor egy felhasználó megpróbál tooaccess hello alkalmazáshoz böngészővel. hello alkalmazás átirányítja a saját tooan futtassanak a kiállító terjesztési hely (és adott hello alkalmazáshoz is megbízhatónak tekinti). Ő hitelesíti saját magát toothis IdP, például írja be a felhasználónevet és jelszót (1. lépés), és az hello IdP egy saját (2. lépés) kapcsolatos információkat tartalmazó jogkivonatot ad vissza.

Hello. ábrán látható, a hozzáférés-vezérlés számos különböző felhőalapú IdPs, beleértve a Google, a Yahoo, a Facebook, a Microsoft (korábbi nevén Windows Live ID) és a bármely OpenID-szolgáltató által létrehozott fiókokat is támogatja. Azure Active Directory használatával létrehozott identitások is támogatja, és az AD FS, a Windows Server Active Directory összevonási keresztül. hello célja toocover leggyakrabban használt hello identitások ma, hogy azok még kiállító IdPs hello felhőalapú vagy helyszíni.

Miután hello felhasználó böngészőben a kiválasztott IdP egy IdP jogkivonatot, ez elküldi a token tooAccess vezérlő (3. lépés). Hozzáférés-vezérlés érvényesíti hello jogkivonatot, és meggyőződött arról, hogy, hogy valóban a kiállító terjesztési hely által kiállított, ezután létrehoz egy új jogkivonatot, ehhez az alkalmazáshoz meghatározott toohello szabályok szerint. Például az Azure Active Directory hozzáférés-vezérlés egy több-bérlős szolgáltatást, de hello bérlők helyett ügyfél szervezetek. Minden alkalmazás kérheti le a saját névtér hello. ábrán látható módon, és adhatja meg az engedélyezési és több különböző szabályok.

Ezek a szabályok segítségével határozza meg, hogyan kell-e a különböző IdPs származó jogkivonatokat átalakítja a hozzáférési tokent az egyes alkalmazás-rendszergazda. Például ha különböző IdPs különböző típusait használják a lapblobok felhasználónevek, hozzáférés-vezérlés szabályok alakíthatja át minden közös felhasználónév típusú be ezeket. Hozzáférés-vezérlés ezután elküldi az új token hátsó toohello böngésző (4. lépés), amely elküldi toohello alkalmazás (5. lépés). Amennyiben rendelkezik hello hozzáférés-vezérlés jogkivonatot, hello alkalmazás ellenőrzi, hogy ez a token valóban hozzáférés-vezérlés adta ki, majd hello adatokkal (6. lépés) tartalmaz.

Amíg ez a folyamat kissé bonyolult tűnhetnek, ténylegesen teszi élettartama jelentősen egyszerűbb hello létrehozó hello alkalmazás. Helyett más adatokat tartalmazó különböző jogkivonatok kezeléséhez, hello alkalmazás fogadhat továbbra is a megszokott adatokkal csak egyetlen token fogadása során több identitásszolgáltatók által kibocsátott identitásokat. Is helyett igényelnek, mindegyik alkalmazás toobe konfigurált tootrust különböző IdPs, ezek a bizalmi kapcsolatok helyette megmarad által hozzáférés-vezérlés – kell az alkalmazás csak megbízható.

Érdemes mutat, hogy semmit sem tud hozzáférés-vezérlés a feltételekhez tooWindows – is használhatják a Linux-alkalmazás, amely csak Google és a Facebookon identitások elfogadva. És annak ellenére, hogy a hozzáférés-vezérlés hello Azure Active Directory-család része, tulajdonképpen azt a mi hello előző szakaszban ismertetett teljesen különálló szolgáltatásként. Mindkét technológiát identitás dolgozni, miközben azok Igen különbözőek problémák megoldása (bár Microsoft említett rendelkezik egy bizonyos ponton azt várja toointegrate hello két).

Majdnem minden egyes alkalmazás identitású működő fontos. hello hozzáférés-vezérlés célja toomake könnyebb a fejlesztők toocreate alkalmazások, amelyek különböző identitás-szolgáltatóktól származó identitások fogadnak el. Ez a szolgáltatás tegyen hello felhőben, Microsoft tette bármilyen platformon futó elérhető tooany alkalmazás.

## <a name="about-hello-author"></a>Hello Szerző kapcsolatos
David Chappell egyszerű a Chappell & társult [www.davidchappell.com](http://www.davidchappell.com) San Francisco, kaliforniai.

