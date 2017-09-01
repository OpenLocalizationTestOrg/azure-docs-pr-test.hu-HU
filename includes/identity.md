Éppen olyan fontos a nyilvános felhőben, mert a helyszíni identitás kezelése. Ennek érdekében az Azure támogatja több különböző felhőalapú identitás technológiák. Ezek tartalmazzák:

* Windows Server Active Directory (általában csak az AD-nevezett) használatával létrehozott Azure virtuális gépekkel virtuális gépeket a felhőben is futtathatja. Ez a megközelítés szabálykészletében Azure kiterjesztése a helyszíni adatközpontját a felhőre való használatakor.
* Azure Active Directory használatával a felhasználók egyszeri bejelentkezéshez történő adjon [(SaaS) szolgáltatott szoftver](https://azure.microsoft.com/overview/what-is-saas/) alkalmazások. A Microsoft Office 365 ezt a technológiát használja, például és Azure-ban vagy más felhőalapú platformokon futó alkalmazásokhoz is használható.
* A felhőben, vagy a helyszínen futó alkalmazások Azure Active Directory hozzáférés-vezérlés segítségével Facebook, Google, a Microsoft és egyéb identitás-szolgáltatóktól származó identitásokkal bejelentkezés, hogy a felhasználók.

A cikkből megtudhatja, mind a három ezek közül.

## <a name="table-of-contents"></a>Tartalomjegyzék
* [Windows Server Active Directory-beli virtuális gépek](#adinvm)
* [Az Azure Active Directoryval](#ad)
* [Az Azure Active Directory hozzáférés-vezérlés használatával](#ac)

## <a name="adinvm"></a>Windows Server Active Directory-beli virtuális gépek
Az Azure virtuális gépeken futó Windows Server AD helyileg futó hasonlítanak. [1. ábra](#fig1) ez megjelenésének jellemző példa látható.

![Az Azure Active Directory, a virtuális gépen](./media/identity/identity_01_ADinVM.png)

<a name="Fig1"></a>1. ábra: A Windows Server Active Directory egy szervezet helyszíni adatközpontját Azure virtuális hálózat használatával csatlakozik az Azure virtuális gépeken futtathatók.

Az itt bemutatott példában a Windows Server AD fut az Azure virtuális gépek, a platform IaaS technológia használatával létrehozott virtuális gépeken. Egy Azure virtuális hálózat használatával a helyszíni adatközponthoz csatlakozó virtuális hálózat virtuális gépeken, és néhány egyéb vannak csoportosítva. A virtuális hálózati kivágja ki egy csoportot a felhő virtuális gépek, amelyek a helyszíni hálózat virtuális magánhálózati (VPN) kapcsolaton keresztül kommunikál. Ennek során Ez engedélyezi, hogy ezeket az Azure virtuális gépek tűnik a helyszíni adatközpontját egy másik alhálózat. Az ábrán látható, két virtuális gépek futnak Windows Server AD-tartományvezérlő. Előfordulhat, hogy a virtuális hálózatban lévő más virtuális gépek futnia alkalmazások, például a SharePoint, vagy használja más módon, például a fejlesztéshez és teszteléshez. A helyszíni adatközpontját két Windows Server AD-tartományvezérlő is futtatja.

Nincsenek a tartományvezérlők, a felhőben futó a helyszíni összekötő számos lehetőség közül választhat:

* Ellenőrizze az összes egyetlen Active Directory-tartomány része.
* Hozzon létre külön AD tartományok helyszíni és a felhőben, amelyek ugyanabban az erdőben találhatók.
* Hozzon létre külön AD-erdőkkel a felhőben és helyszíni, majd csatlakoztassa az erdők erdők közötti bizalmi kapcsolatok vagy a Windows Server Active Directory összevonási szolgáltatások (AD FS), amely az Azure virtuális gépeken is futtatható.

Függetlenül a választott történik, a rendszergazda győződjön meg arról, hogy a helyszíni felhasználók érkező hitelesítési kérések Ugrás felhő tartományvezérlők csak akkor, ha szükséges, mert a felhőben való kapcsolatot várhatóan lassabb, mint a helyszíni hálózatokban. Egy másik szempont az, hogy csatlakozó felhőben és helyszíni tartományvezérlők a replikáció által generált forgalmat. A felhőben tartományvezérlők jellemzően a saját AD hely, amely lehetővé teszi a rendszergazda számára, hogy milyen gyakran ütemezése replikációs történik. Bár nem bájtok küldi, amelyek hatással lehetnek a rendszergazda replikációs lehetőségek egy Azure-adatközpontban kívül elküldött forgalom Azure költségekkel. Érdemes is mutat, hogy Azure támogatja a saját tartománynévrendszer (DNS), amíg ez a szolgáltatás (például a dinamikus DNS- és SRV-rekordok támogatása) Active Directory által igényelt funkciók hiányzik. Ebből kifolyólag a Windows Server AD futó Azure telepítése szükséges hozzá a saját DNS-kiszolgálók, a felhőben.

Windows Server AD az Azure virtuális gépeken futó is célszerű több különböző helyzetekben. Néhány példa:

* Azure virtuális gépek használatakor a saját adatközpont kiterjesztése alkalmazások előfordulhat, hogy futtatja a felhőben, amelyek a helyi tartományvezérlőt műveleteket, mint az integrált Windows-hitelesítés kéréseket, és az LDAP-lekérdezések kezelésére. A SharePoint, például együttműködő gyakran az Active Directoryban, és úgy is lehet futtatni a SharePoint-farm Azure-ban egy helyszíni címtárat, a felhőben tartományvezérlők beállításával jelentősen javítja a teljesítményt. (Fontos vegye figyelembe, hogy ez nem feltétlenül szükséges, azonban; bőven alkalmazások futtatható sikeresen csak a helyszíni tartományvezérlők használatához a felhőben.)
* Tegyük fel, hogy egy közösségihez fiókiroda nem rendelkezik az erőforrások a saját tartományvezérlők futtatásához. Napjainkban a felhasználóknak kell hitelesítenie a másik oldalon a világ tartományvezérlők - bejelentkezések lassú. Azure Active Directory szorosabb Microsoft-adatközpontban fut felgyorsítható a anélkül, hogy további kiszolgálókat a fiókirodában.
* Egy szervezet által használt Azure vész-helyreállítási előfordulhat, hogy egy kis készletét aktív virtuális gépek a felhőben, beleértve a tartományvezérlő karbantartása. Majd elkészíthetők a hely hibák máshol átvétele igény szerint.

Egyéb lehetőségek is vannak. Például Ön nem szükséges egy helyszíni adatközpontot csatlakozni a Windows Server AD a felhőben. Ha szeretne futtatni egy meghatározott felhasználók szolgáltatott SharePoint-farm, például, akik szeretné-e jelentkezni kizárólag felhőalapú felhasználókkal, létrehozhat egy különálló erdőben Azure. Ez a technológia használatát attól függ, hogy a célok vannak. (Az Azure-ral, használja a Windows Server AD útmutatást részletes [talál](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Az Azure Active Directoryval
SaaS-alkalmazásokhoz egyre nagyobb közös, felmerül a nyilvánvaló kérdés: milyen címtárszolgáltatás kell ezek a felhőalapú alkalmazások használnak? A Microsoft kérdésre adandó választ az Azure Active Directory.

Ez a címtárszolgáltatás használatát mutatja be a felhő két fő lehetőség áll rendelkezésre:

* Egyéni felhasználók számára, és csak az SaaS-alkalmazásokhoz használó szervezetek támaszkodhat Azure Active Directory, az egyetlen címtárszolgáltatás.
* A szervezetek, futtassa a Windows Server Active Directory a helyszíni címtár Azure Active Directoryhoz csatlakozni, majd segítségével a felhasználók az egyszeri bejelentkezés SaaS-alkalmazások biztosítják.

[2. ábra](#fig2) mutatja be az első két beállítás, ahol Azure Active Directory az összes szükséges.

![Az Azure Active Directory, a virtuális gépen](./media/identity/identity_02_AD.png)

<a name="fig2"></a>2. ábra: Az Azure Active Directory lehetővé teszi a szervezetek felhasználók egyszeri bejelentkezést a SaaS-alkalmazások, beleértve az Office 365.

Az ábrán látható, az Azure AD egy több-bérlős szolgáltatást. Ez azt jelenti, hogy egyidejűleg támogatja a sok különböző szervezetek, azok a felhasználók directory információt. Ebben a példában a szervezet A következő felhasználó próbál hozzáférni egy SaaS-alkalmazáshoz. Ez az alkalmazás az Office 365, például a SharePoint Online része lehet, illetve előfordulhat, hogy valami mást,-nem Microsoft-alkalmazások is használhatják ezt a technológiát. Az Azure AD a SAML 2.0 protokoll használatát támogatja, mert az adott alkalmazásból szükséges csak a való kommunikációhoz az iparági szabvány használatával. (Valójában alkalmazásokat, amelyek használják az Azure AD futtatható bármely adatközpontban, nem csak egy Azure-adatközpontban található.)

A folyamat akkor kezdődik, amikor a felhasználó fér hozzá SaaS-alkalmazás (1. lépés). Az alkalmazás használatához, a felhasználó Azure AD által kiadott tokennek e.

Ez a token a felhasználói azonosító információkat tartalmaz, és digitális aláírásával az Azure AD. Ahhoz, hogy a jogkivonat, a felhasználó saját maga megadásával hitelesíti magát az Azure AD egy felhasználónevet és jelszót (2. lépés). Az Azure AD majd a jogkivonatot ad vissza úgy kell (3. lépés).

Ez a token küldi el a rendszer a SaaS-alkalmazáshoz (4. lépés), amely ellenőrzi a jogkivonat aláírása és annak tartalmát (5. lépés) használja. Az alkalmazás általában a azonosító adatok számára, hogy a felhasználó számára engedélyezett a hozzáférés és más módon lehet, hogy milyen információkat tartalmaz a token fogja használni.

Ha az alkalmazás igényel, mint mi az a lexikális elem szerepel a felhasználó további információt, akkor kérhetnek a közvetlenül az Azure AD Graph API-val (6. lépés) Azure AD. Az Azure AD kezdeti verziója, a directory-séma használata rendkívül egyszerű: Ebben az esetben a felhasználók és a csoportok és a közöttük lévő kapcsolatok tartalmaz. Alkalmazások használhatják ezt az információt olvashat a felhasználók közötti kapcsolatok. Tegyük fel, hogy egy alkalmazás tudnia kell, aki a felhasználó kezelője eldöntheti, hogy ő engedélyezték-e az egyes adattömb elérésére. Az Azure AD Graph API-n keresztül lekérdezésével kaphat.

A Graph API egy szokásos RESTful protokollt használ, amely lehetővé teszi magától értetődő használja a legtöbb ügyfél, beleértve a mobil eszközöket. Az API-t is támogatja a bővítmények OData hozzáadása műveleteket, mint a lekérdezési nyelv ahhoz, hogy az ügyfelek hozzáférési adatok hasznosabb módon határozzák meg. (OData bővebben lásd: [bevezetéséről OData](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) A Graph API segítségével további tudnivalók a felhasználók közötti kapcsolatokat, mert a közösségi grafikon az Azure AD-sémát a egy adott szervezet (éppen ezért a Graph API-val rendelkezik meghívta) beágyazott megértéséhez alkalmazások segítségével. És az Azure AD Graph API-kérelmek hitelesítését egy alkalmazás OAuth 2.0 használja.

Ha a szervezet nem használja a Windows Server Active Directory – helyszíni kiszolgálók vagy tartományok - van, kizárólag felhőalapú alkalmazásokhoz az Azure AD használatára támaszkodik, használatával csak a felhőalapú címtárban adna a cég felhasználók egyszeri bejelentkezést minden olyan őket. Még amíg ez a forgatókönyv gyakori lekérdezi minden nap, a legtöbb szervezet továbbra is használja a helyi tartományokat létrehozni Windows Server Active Directory. Azure AD is rendelkezik egy hasznos szerepkör számára, hogy itt is, mint a [3. ábra](#fig3) jeleníti meg.

![A virtuális gép az Azure Active Directory](./media/identity/identity_03_AD.png)
<a id="fig3"></a>3. ábra: egy szervezet lehet összevonást végezni a Windows Server Active Directory az Azure Active Directoryval SaaS-alkalmazások a felhasználók egyszeri bejelentkezést biztosítanak.

Ebben a forgatókönyvben a felhasználók a szervezet B kívánja egy SaaS-alkalmazás eléréséhez. Mielőtt, a szervezet directory rendszergazdák automatikusan összevonási kapcsolatot kell létesítenie az Azure AD használata az AD FS-ben az ábrán látható módon. Ezeket a rendszergazdák is konfigurálnia kell a szervezet helyszíni Windows Server AD és az Azure AD közötti adatszinkronizálás. Ez automatikusan átmásolja felhasználói a helyszíni címtár az Azure ad Szolgáltatásba. Figyelje meg, ez lehetővé teszi: érvényben van a szervezet van kiterjesztése a helyszíni címtár kiterjeszti a felhőbe. A szervezet Windows Server AD és az Azure AD ily módon egyesítésével biztosítja egy címtárszolgáltatás, amely során továbbra is fennáll a kezdjen a helyszínen és a felhőben, egyetlen egységként kezelt.

Az Azure AD használatára, a felhasználó először jelentkezik be, a helyszíni Active Directory-tartomány megszokott módon (1. lépés). Ha ő próbál hozzáférni a SaaS-alkalmazáshoz (2. lépés), az alkalmazás (3. lépés) jogkivonatot kibocsátó rá, hogy az Azure AD összevonási folyamat eredményez. (További összevonási működésével kapcsolatban: [Windows jogcímalapú identitás: technológiák és forgatókönyvek](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Előtt, a token tartalmazza a felhasználói azonosító információkat, valamint digitális aláírásával az Azure AD. Ez a token küldi el a rendszer a SaaS-alkalmazáshoz (4. lépés), amely ellenőrzi a jogkivonat aláírása és annak tartalmát (5. lépés) használja. Az előző esetben a SaaS-alkalmazás a Graph API segítségével további információ a felhasználó Ha, és szükség esetén (6. lépés).

Az Azure AD ma, nem a teljes cserét hajt végre a helyi Windows Server AD. Mint már említettük a felhőalapú címtárban egy sokkal egyszerűbb sémával rendelkezik, és műveleteket, mint az csoportházirend, készülékekre vonatkozó információkat tárolja, és támogatja az LDAP is hiányzik. (Valójában egy Windows gép nem állítható be, hogy a felhasználók segítségével semmi, de az Azure AD bejelentkezés – Ez nem támogatott.) Ehelyett az eredetileg kitűzött célokat, az Azure AD közé tartozik, ha engedélyezi, hogy a vállalati felhasználók alkalmazásokat a felhőben anélkül, hogy egy külön bejelentkezési karbantartása és felszabadítása a helyszíni címtárban manuális szinkronizálása a helyszíni címtár és az összes A szervezet által használt SaaS-alkalmazáshoz. Idővel azonban várhatóan Ez a felhő címtárszolgáltatás a lehetséges forgatókönyvek bővítése megoldására.

## <a name="ac"></a>Az Azure Active Directory hozzáférés-vezérlés használatával
Felhőalapú identitás technológiák segítségével számos különféle problémák megoldásában. Az Azure Active Directory biztosíthat a szervezetek felhasználók egyszeri bejelentkezés több SaaS-alkalmazásokhoz, például. De identitás technológiák felhőalapú más módokon is használható.

Tegyük fel például, hogy egy alkalmazás által a felhasználóknak, hogy jelentkezzen be több által kiállított jogkivonatokat *identitás-szolgáltatóktól (IdPs)*. Sok különböző identitás-szolgáltatóktól Facebook, Google, a Microsoft és más, beleértve napjainkban létezik, és alkalmazások gyakran tudassa a felhasználókkal, jelentkezzen be valamelyik ezeket az identitásokat. Miért kell az alkalmazás ne karbantartása saját felhasználók és jelszavak listája, ha inkább támaszkodhat a már meglévő identitások? Meglévő identitások elfogadása teszi élettartama egyszerűbbé válik a felhasználók számára, akik kevesebb mint a felhasználónév és jelszó megjegyzése van, és dolgozó felhasználók számára az alkalmazás létrehozása már nem kell saját felhasználónevek és jelszavak listája karbantartása.

Azonban minden identitásszolgáltató kiállított jogkivonat bizonyos típusú, míg a jogkivonatok nem szabványos – minden IdP saját formátuma. Ezenkívül a jogkivonatok az információk is nem szabványos. Tegyük fel például, által kiállított jogkivonatokat, a Facebook, Google, és a Microsoft fogadni szándékozó alkalmazás tapasztalt azzal a egyedi kódot ír az egyes ezek különböző formátumokban kezelésére.

De miért ehhez? Miért nem hozzon létre, amelyek egy közös azonosító adatok ábrázolása a egyetlen token formátuma hozhat létre a köztes? Ezt a módszert akkor egyszerűbbé teszik a élettartama az alkalmazásokat létrehozó fejlesztőknek, mivel most kell kezelni, csak az egyiket a típusú jogkivonat. Az Azure Active Directory hozzáférés-vezérlés pontosan ezt végzi, és a felhőben köztes biztosít a különböző jogkivonatok használata. [4. ábra](#fig4) bemutatja, hogyan működik?

![A virtuális gép az Azure Active Directory](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>4. ábra: Azure Active Directory hozzáférés-vezérlés megkönnyíti az alkalmazások számára a más identitásszolgáltatók által kibocsátott fogadni identitás.

A folyamat akkor kezdődik, amikor egy felhasználó megpróbál hozzáférni az alkalmazáshoz böngészővel. Az alkalmazás átirányítja a felhasználókat rá, hogy a saját választott egy IdP (és az alkalmazás is megbízhatósági kapcsolatok). Ő hitelesíti saját magát a kiállító terjesztési hely, például írja be a felhasználónevet és jelszót (1. lépés), és az IdP egy saját (2. lépés) kapcsolatos információkat tartalmazó jogkivonatot ad vissza.

Ahogy az ábrán az látható, hozzáférés-vezérlés támogatja az számos különböző felhőalapú IdPs, beleértve a Google, a Yahoo, a Facebook, a Microsoft (korábbi nevén Windows Live ID) és a bármely OpenID-szolgáltató által létrehozott fiókokhoz. Azure Active Directory használatával létrehozott identitások is támogatja, és az AD FS, a Windows Server Active Directory összevonási keresztül. Célja fedik le a leggyakrabban használt identitások ma, hogy azok még kiállítja a felhőben, vagy a helyszíni IdPs.

Miután a felhasználó böngészőben a kiválasztott IdP egy IdP jogkivonatot, elküldi a token hozzáférés-vezérlés (3. lépés). Hozzáférés-vezérlés érvényesíti a jogkivonatot, meggyőződött arról, hogy, hogy valóban a kiállító terjesztési hely által kiállított, ezután létrehoz egy új jogkivonatot az ehhez az alkalmazáshoz meghatározott szabályok szerint. Például az Azure Active Directory hozzáférés-vezérlés egy több-bérlős szolgáltatást, de a bérlők helyett ügyfél szervezetek. Minden alkalmazás saját névtér kérheti le az ábrán látható módon, és adhatja meg az engedélyezési és több különböző szabályok.

Ezek a szabályok segítségével határozza meg, hogyan kell-e a különböző IdPs származó jogkivonatokat átalakítja a hozzáférési tokent az egyes alkalmazás-rendszergazda. Például ha különböző IdPs különböző típusait használják a lapblobok felhasználónevek, hozzáférés-vezérlés szabályok alakíthatja át minden közös felhasználónév típusú be ezeket. Hozzáférés-vezérlés majd visszaküldi az új jogkivonat a böngésző (4. lépés), amely elküldi az alkalmazás (5. lépés). Miután a hozzáférés-vezérlés token rendelkezik, az alkalmazás ellenőrzi, hogy ez a token valóban hozzáférés-vezérlés adta ki, majd a adatokkal (6. lépés) tartalmaz.

Amíg ez a folyamat kissé bonyolult tűnhetnek, ténylegesen teszi élettartama jelentősen egyszerűbb a létrehozó az alkalmazás. Helyett más adatokat tartalmazó különböző jogkivonatok kezelésére, az alkalmazás fogadhat továbbra is a megszokott adatokkal csak egyetlen token fogadása során több identitásszolgáltatók által kibocsátott identitásokat. Is helyett az egyes alkalmazásokat úgy, hogy különböző IdPs megbízható igényel, ezek a bizalmi kapcsolatok helyette megmarad által hozzáférés-vezérlés – kell az alkalmazás csak megbízható.

Érdemes mutat, hogy semmit sem tud hozzáférés-vezérlés Windows kötődik - is felhasználható egy Linux-alkalmazás, amely csak a Google és a Facebookon identitások elfogadva. És annak ellenére, hogy a hozzáférés-vezérlés az Azure Active Directory-család része, tulajdonképpen azt a mi volt az előző szakaszban leírt teljesen különálló szolgáltatásként. Mindkét technológiát identitás dolgozni, miközben azok Igen különbözőek problémák megoldása (bár a Microsoft már említett, hogy egy bizonyos ponton a két integrálni vár).

Majdnem minden egyes alkalmazás identitású működő fontos. A hozzáférés-vezérlés célja megkönnyítik a különböző identitás-szolgáltatóktól származó identitások támogató alkalmazások fejlesztéséhez. Ez a szolgáltatás tegyen a felhőben, Microsoft tette bármely bármilyen platformon futó alkalmazás számára elérhető.

## <a name="about-the-author"></a>A szerzőről
David Chappell egyszerű a Chappell & társult [www.davidchappell.com](http://www.davidchappell.com) San Francisco, kaliforniai.

