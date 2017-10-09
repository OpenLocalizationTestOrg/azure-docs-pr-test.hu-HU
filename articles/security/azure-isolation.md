---
title: "az Azure nyilvános Felhőjében hello aaaIsolation |} Microsoft Docs"
description: "További információk a felhőalapú számítási szolgáltatás, amely tartalmazza a számítási példányokért széles kijelölt & szolgáltatások automatikusan toomeet hello igényeinek az alkalmazás vagy a vállalati fel és le is méretezhető."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 271e5f0d00abcfd404ce6c50cfb7d1ac26360c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="isolation-in-hello-azure-public-cloud"></a>Az Azure nyilvános Felhőjében hello elkülönítési
##  <a name="introduction"></a>Bevezetés
### <a name="overview"></a>Áttekintés
tooassist jelenlegi és jövőbeli Azure ügyfelek megértéséhez, és felhasználja az hello különböző biztonsági képességek állnak rendelkezésre, és a környező hello Azure platformon, Microsoft kifejlesztette egy sorozatát tanulmányok, biztonsági áttekintése, ajánlott eljárások és Ellenőrzőlisták.
hello témakörök tartomány körének és mélysége, és rendszeresen frissülnek. Ez a dokumentum a sorozat része a következő hello absztrakt szakaszban foglaltak szerint.

### <a name="azure-platform"></a>Az Azure Platform
Azure olyan nyitott és rugalmas felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök hello legszélesebb kiválasztása. Megteheti például a következőt:
- Futtassa a Linux-tárolók Docker integrációs;
- Alkalmazások, JavaScript, Python, .NET, PHP, Java és Node.js; és
- Build vissza-végpontok iOS, Android és Windows eszközökhöz.

Microsoft Azure ugyanazon megoldást több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható hello támogatja.

Amikor létrehozása vagy áttelepítése informatikai eszközök, egy nyilvános felhőszolgáltatóhoz, akkor vannak hagyatkoznia adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatások és hello vezérlők toomanage hello biztonságát, a felhő alapú biztosítanak eszközök.

Azure infrastruktúrája hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez a készült, és amelyen vállalatok számára is saját igényeihez biztonsági megbízható alaprendszert biztosít. Emellett Azure tesz lehetővé az konfigurálható biztonsági beállítások és hello képességét toocontrol széles köréről őket, hogy a biztonsági toomeet hello egyéni követelményeinek a központi telepítések szabhatja testre. Ez a dokumentum segít az elvárásoknak.

### <a name="abstract"></a>Absztrakt

A Microsoft Azure lehetővé teszi toorun alkalmazások és a virtuális gépek (VM) a megosztott fizikai infrastruktúrát. Hello elsődleges gazdasági összefüggések toorunning egy felhőalapú környezetben futó alkalmazások egyik hello képességét toodistribute hello költségét megosztott erőforrások közötti több ügyfél. Ez az eljárás a több-bérlős multiplex erőforrások közötti kis költségek különböző ügyfelek révén növeli a hatékonyságot. Sajnos egyben bevezeti megosztás fizikai kiszolgálók és egyéb infrastruktúra erőforrások toorun hello kockázatát az érzékeny alkalmazások és a virtuális gépek is tartozhatnak tetszőleges tooan és a potenciálisan rosszindulatú felhasználó.

Ez a cikk ismerteti, hogyan Microsoft Azure el vannak különítve a rosszindulatú és nem rosszindulatú felhasználók ellen, és a megoldások különböző elkülönítési lehetőségek tooarchitects felajánlásával újratervezni útmutatóként szolgál. Ez a dokumentum az Azure platform és ügyfélkapcsolati biztonsági vezérlők hello technológia összpontosít, és nem kísérli meg tooaddress SLA-k, árképzési modellt és DevOps eljárásokkal kapcsolatos szempontok.

## <a name="tenant-level-isolation"></a>Bérlő szintű elkülönítés
Az egyik hello elsődleges előny a felhőalapú informatika egy megosztott, közös infrastruktúra fogalma egyszerre egy között számos olyan ügyfél, a skála tooeconomies vezető. Ez a több bérlős nevezik. Microsoft folyamatosan működik, hogy a Microsoft felhőalapú Azure hello több-bérlős architektúrák támogatja-e a biztonsági, titkosítás, adatvédelmi, integritás és rendelkezésre állás szabványok tooensure.

A hello felhőalapú munkahelyek a bérlő olyan ügyfelet vagy szervezetet birtokolja és kezeli, amely a felhőszolgáltatás adott példányát adható meg. Microsoft Azure által biztosított hello identitásplatformmal együttműködve a bérlő az Azure Active Directory (Azure AD), amely a szervezet megkap és a tulajdonában áll, amikor regisztrálnak egy Microsoft felhőszolgáltatásra egyszerűen dedikált példányát.

Mindegyik Azure AD-címtár önálló, és el van választva a többi Azure AD-címtártól. Csakúgy, mint a vállalat irodaépülete van egy biztonságos eszköz adott tooonly a szervezet, az Azure AD-címtár is egy biztonságos eszköz kizárólag az adott szervezet általi használatra kialakított toobe. Azure AD architektúrájával hello elkülöníti az ügyfél- és identitásadatok keveredése. Ez azt jelenti, hogy az adott Azure AD-címtár felhasználói és rendszergazdái véletlenül vagy kártételi szándékkal nem férhetnek hozzá más címtárak adataihoz.

### <a name="azure-tenancy"></a>Az Azure-Bérlőhöz
Az Azure-bérlőhöz (Azure-előfizetéshez) hivatkozik tooa "felhasználói/billing" kapcsolat és egy egyedi [bérlői](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant) a [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis). Bérlő szintű elkülönítés a Microsoft Azure-ban érhető el, az Azure Active Directoryval és [szerepköralapú vezérlők](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) által kínált. Minden Azure-előfizetés tartozik egy Azure Active Directory (AD) címtárban.

Felhasználók, csoportok és alkalmazások tartalmazza a kezelhetik az erőforrásokat a hello Azure-előfizetés. A következő hozzáférési jogokat hello Azure-portálon, az Azure parancssori eszközök és Azure szolgáltatásfelügyeleti API használatával rendelhet hozzá. Az Azure AD-bérlő elkülönül logikailag biztonsági határokat használja, így nem az ügyfél hozzáférési vagy veszélyeztetheti a közös bérlők szándékosan vagy véletlenül. Az Azure AD fut egy elkülönített hálózati szegmensen elkülönített "operációs rendszer nélküli" kiszolgálókon, ahol gazdaszintű hálózaticsomag-szűrés és a Windows tűzfal blokkolja kéretlen kapcsolatokat és a forgalom.

- Az Azure AD hozzáférési toodata csak a felhasználó hitelesítése keresztül egy [biztonságijogkivonat-szolgáltatás (STS)](https://docs.microsoft.com/azure/app-service-web/web-sites-authentication-authorization). Hello felhasználói fenntartása, az engedélyezett állapotát és a szerepkör hello engedélyezési rendszer toodetermine használja e hello szükséges hozzáférést toohello cél bérlő jogosult felhasználó ebben a munkamenetben.

![Az Azure-Bérlőhöz](./media/azure-isolation/azure-isolation-fig1.png)


- Bérlők diszkrét tárolói, és ezek között nincs kapcsolat van.

- Nem lehet hozzáférni között bérlők csak a bérlői rendszergazda összevonási vagy a többi bérlőtől felhasználói fiókok létesítését keresztül biztosít.

- Hello Azure AD szolgáltatás, és közvetlen hozzáférést AD a tooAzure háttér-rendszerek alkotó fizikai hozzáférés tooservers korlátozódik.

- Az Azure Active Directory-felhasználók nem hozzáférés toophysical eszközök vagy a helyek rendelkezik, és ezért nincs lehetőség őket toobypass hello logikai RBAC házirend ellenőrzésére is olvasható a következő.

A diagnosztika és karbantartási igényeire az operatív modell just-in-time jogosultság jogosultságszint-emelés rendszer által szükséges, és használja. Az Azure AD Privileged Identity Management (PIM) bemutatja hello egy jogosult rendszergazdának. [Jogosult rendszergazdák](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) szükséges jogosultsággal rendelkező felhasználók férhetnek hozzá a most majd, de nem minden nap lehet. hello szerepkör nem aktív, amíg hello felhasználói kell elérnie, akkor az aktiválási folyamat befejezéséhez, és egy aktív felügyeleti válik a előre meghatározott időn.

![Azure AD Privileged Identity Management](./media/azure-isolation/azure-isolation-fig2.png)

Az Azure Active Directory mindegyik bérlő saját védett tárolóban, szabályzatokat és engedélyeket tooand kizárólag által birtokolt vagy kezelt hello bérlői hello tárolóban levő üzemelteti.

hello bérlői tárolók fogalma mélyen ingrained hello címtárszolgáltatásban, rétegeket, a portálok összes hello módon toopersistent tárolási.

Akkor is, ha több Azure Active Directory-bérlő metaadatok tárolt hello azonos fizikai lemez, nincs eltérő hello címtárszolgáltatás, amely viszont határozza meg a bérlői rendszergazda hello által meghatározott hello tárolók közötti kapcsolat.

### <a name="azure-role-based-access-control-rbac"></a>Azure szerepköralapú hozzáférés-vezérlés (RBAC)
[Azure szerepköralapú hozzáférés-vezérlés (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) , adja meg a részletes hozzáféréskezelést az Azure elérhető Azure-előfizetés belül különböző összetevőket, tooshare segítségével. Az Azure RBAC lehetővé teszi a szervezeten belüli toosegregate feladatokat, és hozzáférést alapján a felhasználók milyen kell tooperform a munkájukat. Jogosultságot ad a Mindenki helyett korlátlan engedélyeket a Azure-előfizetés vagy erőforrásokhoz, engedélyezheti a csak bizonyos műveleteket.

Az Azure RBAC három alapvető szerepkörökhöz tooall erőforrástípusok rendelkezik:

- **Tulajdonos** rendelkezik teljes körű hozzáférési tooall erőforrások, például az hello jobb toodelegate hozzáférés tooothers.

- **A közreműködői** is létrehozása és kezelése az Azure-erőforrások minden típusú, de nem tudja engedélyezni a hozzáférést tooothers.

- **Olvasó** megtekintheti a meglévő Azure-erőforrások.

![Azure szerepköralapú hozzáférés-vezérlés](./media/azure-isolation/azure-isolation-fig3.png)

hello rest hello RBAC-szerepkörök az Azure-ban az adott Azure-erőforrások felügyelet lehetővé tételéhez. Virtuális gép közreműködő szerepkört hello például lehetővé teszi, hogy a felhasználó toocreate hello, és virtuális gépeket. Azt nem számukra az Azure Virtual Network access toohello vagy, amely a virtuális gép hello hello alhálózathoz csatlakozik.

[Beépített RBAC-szerepkörök](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) hello szerepkörök az Azure-ban elérhető listában. Hello műveletek és, hogy minden beépített szerepkörök toousers hatókört határoz meg. Ha saját szerepköröket még jobban kézben toodefine van szüksége, lásd: hogyan toobuild [egyéni szerepkörök az Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

Az Azure Active Directory néhány más képességet a következők:
- Az Azure AD lehetővé teszi az egyszeri bejelentkezés tooSaaS alkalmazások, függetlenül attól, hogy a rendszer hol tárolja. Egyes alkalmazások az Azure AD-vel összevontan működnek, mások jelszavas egyszeri bejelentkezést használnak. Összevont alkalmazásokat is képes támogatni a felhasználók átadása és [jelszótárolást](https://www.techopedia.com/definition/31415/password-vault).

- A hozzáférés toodata [Azure Storage](https://azure.microsoft.com/services/storage/) hitelesítéssel vezérlik. Minden tárfiók elsődleges kulccsal rendelkezik ([tárfiók kulcsa](https://docs.microsoft.com/azure/storage/storage-create-storage-account), vagy SAK) és egy másodlagos titkos kulcsot (közös hozzáférésű jogosultságkódot hello vagy SAS).

- Az Azure AD Identity keresztül összevonási szolgáltatás segítségével biztosítja [Active Directory összevonási szolgáltatások](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs), szinkronizáláshoz és a replikáció a helyszíni címtárakban.

- [Az Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) hello a multi-factor authentication szolgáltatás, amelyhez a felhasználók tooverify bejelentkezések a mobilalkalmazás, telefonhívással vagy szöveges üzenet használatával. Az Azure AD toohelp biztonságos helyszíni erőforrásokkal hello Azure multi-factor Authentication kiszolgálóval és az egyéni alkalmazások és könyvtárak hello SDK használatával használható.

- [Azure AD tartományi szolgáltatások](https://azure.microsoft.com/services/active-directory-ds/) csatlakoztathatja az Azure virtuális gépek tooan Active Directory-tartomány tartományvezérlők üzembe helyezésének nélkül. Jelentkezzen be toothese virtuális gépek a vállalati Active Directory hitelesítő adataival, és a tartományhoz csatlakozó virtuális gépek felügyelete az Azure virtuális gépeken futó csoportházirend tooenforce biztonsági alapterveket használatával.

- [Az Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) egy magas rendelkezésre állású globális identitás szolgáltatást biztosít a felhasználók felé néző alkalmazások identitások millióinak toohundreds méretezi. Mobil- és webes platformokba is integrálható. A felhasználók bejelentkezhetnek a tooall, testre szabható felhasználói élmény mellett az alkalmazások hitelesítő adatok létrehozásával vagy meglévő közösségi fiókjaik használatával.

### <a name="isolation-from-microsoft-administrators--data-deletion"></a>A Microsoft rendszergazdák & Adattörlés elkülönítési
Microsoft erős intézkedések tooprotect az adatok nem megfelelő hozzáférési vagy illetéktelen személyek általi használatra vesz igénybe. Ezeket a működési folyamatok és vezérlőelemek üzemelnek hello [Online szolgáltatási feltételek](http://aka.ms/Online-Services-Terms), melyik ajánlattal szerződéses kötelezettségek, amelyek szabályozzák az access tooyour adatokat.

-   A Microsoft szakemberei hello felhőben nincs alapértelmezett hozzáférési tooyour adatokat. Ehelyett kapnak hozzáférést, csak akkor, ha szükséges, a felügyeleti felügyelet alatt. Elérheti gondosan ellenőrzött és naplózott, és visszavonva, amikor már nincs szükség.

-   Microsoft felvételi előfordulhat, hogy más vállalatok tooprovide korlátozott szolgáltatások nevében. Alvállalkozók elérhessék az ügyfél csak toodeliver hello adatszolgáltatások, amelynek bíztunk őket tooprovide, és nem használhatják más célra használja azt. Továbbá azok szerződésben kötött toomaintain hello bizalmas kezelése mellett, a felhasználók információkat.

Üzleti szolgáltatások ellenőrzött minősítései közül például ISO/IEC 27001 rendszeresen ellenőrzi a Microsoft és akkreditált cégek, amelyek minta naplózás tooattest végrehajtására, hogy egyes hozzáférés naplózása, csak valós üzleti okokból. A saját felhasználói adatok bármikor, bármilyen okból mindig elérhető.

Ha törli az adatokat, a Microsoft Azure hello adatok, beleértve a gyorsítótárazott vagy biztonsági másolatok törli. Hatókör szolgáltatások az, hogy a törlési hello hello megőrzési időszak vége után 90 napon belül történik. (A hatókör szolgáltatások hello adatfeldolgozási feltételek szakaszában meghatározott a [Online szolgáltatási feltételek](http://aka.ms/Online-Services-Terms).)

Ha a meghajtó tárolására használt hardver meghibásodása esetén a rendszer biztonságosan [törlik vagy megsemmisítik](https://www.microsoft.com/trustcenter/Privacy/You-own-your-data) előtt Microsoft toohello gyártót cseréje vagy a javítási visszaadja. hello hello meghajtón található adatok felülírja a rendszer nem állíthatók vissza, amely adatok hello tooensure bármilyen módon.

## <a name="compute-isolation"></a>Számítási elkülönítési
Microsoft Azure biztosít a különböző felhőalapú számítási szolgáltatás, amely tartalmazza az számítási példányokért széles köre & szolgáltatások automatikusan toomeet hello igényeinek az alkalmazás vagy a vállalati fel és le is méretezhető. A számítási példány és a szolgáltatás kínál több szintek toosecure adatok elkülönítést konfigurációs hello rugalmasságot feláldozása nélkül, hogy az ügyfelek igény szerint.

### <a name="hyper-v--root-os-isolation-between-root-vm--guest-vms"></a>A Hyper-V & legfelső szintű OS elkülönítési legfelső szintű VM & Vendég virtuális gépek között
Azure compute platform gép virtualizálási alapul – tehát az összes felhasználói kód végrehajtása a Hyper-V virtuális gépen. Minden Azure csomópont (vagy a hálózati végpont) nincs olyan Hipervizorra, amely közvetlenül hello hardver felett fut, és egy csomópont felosztja egy változó száma a Vendég virtuális gépek (VM).


![A Hyper-V & legfelső szintű OS elkülönítési legfelső szintű VM & Vendég virtuális gépek között](./media/azure-isolation/azure-isolation-fig4.jpg)


Minden csomópont is rendelkezik egy különleges legfelső szintű virtuális Gépet futtató gazdagép operációs rendszer hello. A kritikus határ hello elkülönítési hello gyökér VM a hello Vendég virtuális gépek és hello Vendég virtuális gépek egymástól, hello hipervizor és hello gyökér operációs rendszer kezeli. a Microsoft évtizedekben operációs rendszer biztonsági élményt, és a Microsoft Hyper-V, tooprovide erős elkülönítést Vendég virtuális gépek újabb tanulva hello hipervizor/root OS párosítás kihasználja.

hello Azure platformon virtualizált környezetet használ. A felhasználói példányok működhetnek önálló virtuális gépek, amelyek nem rendelkeznek hozzáféréssel tooa fizikai gazdagép-kiszolgálón, és az elkülönítési kikényszeríti a fizikai processzor (ring-0/ring-3) a jogosultsági szintek segítségével.

A(z) 0 akkor hello jogosultsággal rendelkező és a 3 hello legalább. egy alacsonyabb szintű jogosultságokat igénylő Ring 1 hello vendég operációs rendszer fut, és alkalmazások hello minimális jogosultságokkal rendelkező Ring 3. A virtualizálási fizikai erőforrások tooa egyértelmű elkülönítése vezet a vendég operációs rendszer és a hipervizor hello két további biztonsági elkülönítése eredményezve között.

hello Azure hipervizor úgy viselkedik, mint egy micro kernel, és adja át az összes hardver hozzáférési kérelmek gazdagépről Vendég virtuális gépek toohello feldolgozásra a megosztott memória felületén VMBus hívása. Ez megakadályozza, hogy felhasználók nyers olvasási, írási és végrehajtási hozzáférést toohello rendszer, és csökkenti a rendszer az erőforrások megosztása hello kockázatát.

### <a name="advanced-vm-placement-algorithm--protection-from-side-channel-attacks"></a>Speciális virtuális gép elhelyezési algoritmus & ügyféloldali csatorna támadások elleni védelem
A kereszt-VM támadás két lépésből áll: egy ellenfél által felügyelt virtuális Gépen azonos hello áldozata virtuális gépek rendelkezésre álló gazdagép hello helyezi, és majd megsértése hello elkülönítési határt tooeither áldozata bizalmas információk ellopására vagy befolyásolja a teljesítményt greed vagy vandalizmus. Microsoft Azure két lépést védelmet biztosít egy speciális virtuális gép elhelyezési algoritmus és az összes ismert ügyféloldali csatorna támadások beleértve zajos szomszédos virtuális gépek védelmi használatával.

### <a name="hello-azure-fabric-controller"></a>hello Azure Fabric Controller
hello Azure Fabric Controller infrastruktúra erőforrások lefoglalása tootenant munkaterhelések felelős, és a hello állomás toovirtual gépekről egyirányú kommunikációt kezeli. hello VM forgalomba hozatalára algoritmus hello Azure fabric Controller magas kifinomult és majdnem lehetetlen toopredict, mint a fizikai gazdagépen szint.

![hello Azure Fabric Controller](./media/azure-isolation/azure-isolation-fig5.png)

hello Azure hipervizor kikényszeríti a memória, és a folyamat elkülönítése virtuális gépeket, és biztonságosan irányítja a hálózati forgalom tooguest OS bérlők. Ezzel a megoldással lehetőségét, és az ügyféloldali csatorna támadás VM szinten.

Az Azure hello legfelső szintű virtuális gép működése speciális: az a megerősített operációs rendszert futtat hello gyökér operációs rendszer neve, amelyen a háló ügynök (be). Eszközök kapcsolja toomanage vendégügynökök (GA) belül a vendég operációs rendszer az ügyfél virtuális gépek szerepelnek. Eszközök kezelésére is tárhely csomópontot.

hello Azure hipervizor gyűjteménye a gyökérkönyvtár OS/be, és az ügyfél virtuális gépek/gáz magában foglalja a számítási csomópont. A háló vezérlő (FC), mely kívül (számítási és tárolófürtöket kezeli külön FCs) számítási és tárolási csomópont már eszközök felügyeletét. Az ügyfél frissíti a az alkalmazás konfigurációs fájljának futtatása, ha hello FC kommunikál hello be, amely ezután csatlakozik a gáz, amely értesíti hello konfigurációváltozás hello alkalmazását. Hardverhiba hello esetben hello FC automatikusan található rendelkezésre álló hardver és indítsa újra a virtuális gép hello van.

![Az Azure Fabric Controller](./media/azure-isolation/azure-isolation-fig6.jpg)

A Fabric Controller tooan ügynök kommunikációs egyirányú. hello ügynök valósítja meg az SSL-védelemmel ellátott szolgáltatása, amely csak válaszol toorequests hello vezérlőből. Kapcsolatok toohello vezérlő vagy más kiemelt belső csomópontok nem indítható el. hello FC kezeli az összes, mintha nem megbízható.


![Fabric-vezérlő](./media/azure-isolation/azure-isolation-fig7.png)

Elkülönítési bővíti a legfelső szintű virtuális gép vendég virtuális gépek hello és hello Vendég virtuális gépek egymástól. A számítási csomópontok is el különítve a nagyobb védelme tárolócsomópontok.


hello hipervizor és hello gazda operációs rendszer adja meg a hálózati csomag - szűrők toohelp, hogy biztosítsa, hogy a nem megbízható virtuális gépek nem hamisított adatforgalmat generálnak vagy nem forgalmat toothem, közvetlen forgalom tooprotected infrastruktúra végpontok vagy küldés/fogadás nem megfelelő szórási forgalmat.


### <a name="additional-rules-configured-by-fabric-controller-agent-tooisolate-vm"></a>További szabályok által konfigurált Fabric Controller ügynök tooIsolate méretű VM
Alapértelmezés szerint minden forgalmat blokkol, amikor létrejön egy virtuális gép, és ezután hello fabric controller ügynök konfigurálja hello csomagok tooadd szabályok és kivételek engedélyezett tooallow forgalmának szűrésére.

Szabályok, amelyek program két kategóriába sorolhatók:

-   **Számítógép-konfiguráció vagy infrastruktúra szabályok:** alapértelmezés szerint minden kommunikációja blokkolva van. Nincs kivételek tooallow egy virtuális gép toosend és DHCP- és DNS-forgalom fogadására. Virtuális gépek is küldhet a forgalom toohello "nyilvános" internet és küldési forgalom tooother belüli virtuális gépek hello azonos Azure virtuális hálózatban, és az operációs rendszer aktiválási kiszolgáló hello. engedélyezett a kimenő célok hello virtuális gépek listája nem tartalmazza az Azure útválasztó alhálózatok, az Azure felügyeleti és más Microsoft tulajdonságai.

-   **Szerepkör konfigurációs fájl:** hello hello bérlői modell bejövő hozzáférés-vezérlési listák (ACL) alapján határozza meg.

### <a name="vlan-isolation"></a>VLAN elkülönítése
Az egyes csoportokban vannak a három VLAN-ok:

![VLAN elkülönítése](./media/azure-isolation/azure-isolation-fig8.jpg)


-   hello fő VLAN – összeköttetések használatára nem megbízható felhasználói csomópontok

-   hello FC VLAN – tartalmaz megbízható FCs és a támogató rendszerek

-   eszköz VLAN hello – tartalmazza a megbízható hálózatok és egyéb eszközök

Kommunikációs engedélyezett hello FC VLAN toohello fő VLAN, de nem indítható el a hello fő VLAN toohello FC VLAN. Kommunikációs hello fő VLAN toohello eszköz VLAN is le van tiltva. Ez biztosítja, hogy akkor is, ha egy felhasználói kód futtatásával csomópont biztonsága sérül, akkor nem támadás hello FC vagy VLAN-OK eszköz csomópontok.

## <a name="storage-isolation"></a>Tárolási elkülönítési
### <a name="logical-isolation-between-compute-and-storage"></a>Számítási és tárolási közötti logikai elkülönítés
Az alapvető tervezési részeként a Microsoft Azure elválasztja a Virtuálisgép-alapú számítási tárolóból. Ez az elkülönítés lehetővé teszi a számítási és tárolási tooscale egymástól függetlenül, így könnyebben tooprovide több-bérlős és elkülönítés.

Ezért az Azure Storage fut, nincs hálózati kapcsolat tooAzure külön hardveren számítási kivéve logikailag. [Ez](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf) azt jelenti, hogy a virtuális lemez létrehozása, amikor lemez ne legyen lefoglalva terület esetében a teljes kapacitás. Ehelyett egy táblát hoz létre, amely leképezhető címek hello virtuális lemez tooareas hello fizikai lemezen, és adott táblához kezdetben üres. **hello ügyfél írja az adatokat a hello virtuális lemezen, legyen lefoglalva terület hello fizikai lemezen, és egy mutató tooit hello tábla helyezi el a rendszer első alkalommal.**
### <a name="isolation-using-storage-access-control"></a>Elkülönítés használatával tároló hozzáférés-vezérlés
**Hozzáférés-vezérlés az Azure Storage** egy egyszerű hozzáférés-vezérlési modell tartalmaz. Minden Azure-előfizetés egy vagy több Storage-fiókokat hozhat létre. Minden Tárfiók, amely a Tárfiókban lévő használt toocontrol hozzáférés tooall adatokat egyetlen titkos kulccsal rendelkezik.

![Elkülönítés használatával tároló hozzáférés-vezérlés](./media/azure-isolation/azure-isolation-fig9.png)

**Adatelérés tooAzure tárolására (beleértve a táblák)** szabályozható a [SAS (közös hozzáférésű Jogosultságkód)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) jogkivonatot, amely hatókörű hozzáférést. hello SAS létrehozása révén hello aláírva lekérdezés sablon (URL) [SAK (Tárfiók kulcsa)](https://msdn.microsoft.com/library/azure/ee460785.aspx). Amely [URL-cím aláírt](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) adható meg tooanother folyamat (, van meghatalmazva), amely ezután töltse ki hello lekérdezés hello részleteit és hello kérés hello tároló szolgáltatás. SAS-kód lehetővé teszi toogrant Időalapú hozzáférés tooclients anélkül hogy felfedné hello tárolási fiók titkos kulcsot.

hello SAS azt jelenti, hogy azt is adja meg egy ügyfél korlátozott engedélyekkel, az adott időszakban, és meghatározott engedélyekkel vannak beállítva a tárfiókban lévő tooobjects. Azt is ezen engedélyek korlátozott anélkül, hogy tooshare a tárelérési kulcsok.

### <a name="ip-level-storage-isolation"></a>IP-szintű tárolási elkülönítési
Állítson be tűzfalak, és az IP-címtartományok definiálása a megbízható ügyfeleket. Az IP-címtartományt, csak olyan ügyfelek hello definiált tartományon belüli IP-cím túl kapcsolódhatnak[Azure Storage](https://docs.microsoft.com/azure/storage/storage-security-guide).

IP-tárolási adatok védelme a jogosulatlan felhasználók egy hálózati mechanizmus, amely egy dedikált vagy dedikált alagút forgalmi tooIP tárolási használt tooallocate révén.

### <a name="encryption"></a>Titkosítás
Azure kínálja a következő típusú titkosítás tooprotect adatokat:
-   Az átvitel során titkosítás

-   Titkosítás inaktív állapotban

#### <a name="encryption-in-transit"></a>Az átvitel során titkosítás
Titkosítás az átvitel során, akkor egy eszköz adatok védelmének hálózatok közötti átvitelekor. Az Azure Storage segítségével védetté tehet adatokat:

-   [Átviteli szintű titkosítást](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), például a HTTPS vagy abból az Azure Storage-adatok átvitel során.

-   [Hozzá kell fűznie titkosítási](../storage/common/storage-security-guide.md#using-encryption-during-transit-with-azure-file-shares), például az Azure fájlmegosztások SMB 3.0 titkosítást.

-   [Ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-security-guide#using-client-side-encryption-to-secure-data-that-you-send-to-storage), tooencrypt hello adatokhoz, mielőtt továbbított tárolási és toodecrypt hello adatokká tárolási kivitt után.

#### <a name="encryption-at-rest"></a>Aktívan titkosítása
A legtöbb szervezet számára [adatok titkosítását](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) kötelező lépés adatvédelmi, megfelelőségi és az adatok közös joghatóság alá felé. Nincsenek adatok, amelyek "Inaktív" titkosítás három Azure funkciókat:

-   [Storage szolgáltatás titkosítási](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-at-rest) lehetővé teszi, hogy hello tároló szolgáltatás automatikusan adatok titkosítása írásakor azt tooAzure tárolási toorequest.

-   [Ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-security-guide#client-side-encryption) aktívan titkosítási hello funkcióval is rendelkezik.

-   [Az Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) tooencrypt hello az operációs rendszer és az infrastruktúra-szolgáltatási virtuális gép által használt adatok lemezek lehetővé teszi.

#### <a name="azure-disk-encryption"></a>Azure Disk Encryption
[Az Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) a virtuális gépek (VM) segít cím szervezeti biztonsági és megfelelőségi követelményeknek a Virtuálisgép-lemezek (beleértve a rendszerindító és adatlemezek) titkosítja a kulcsokat és szabályozhatja a házirendek [Azure Key Tároló](https://azure.microsoft.com/services/key-vault/).

lemeztitkosítás megoldás a Windows hello alapul [Microsoft BitLocker meghajtótitkosítás](https://technet.microsoft.com/library/cc732774.aspx), és a Linux-megoldás hello alapul [dm-crypt](https://en.wikipedia.org/wiki/Dm-crypt).

hello megoldás támogatja a következő forgatókönyvek IaaS virtuális gépek esetén, ha engedélyezve van a Microsoft Azure-ban hello:
-   Az Azure Key Vault integrációja

-   Standard szint virtuális gépek: A, D, DS, G, GS és így tovább, adatsorozat IaaS virtuális gépeket

-   A Windows és Linux IaaS virtuális gépeket titkosítás engedélyezése

-   Az operációs rendszer és az adatok titkosításának letiltása Windows IaaS virtuális meghajtók

-   Linux IaaS virtuális gépeket az adatmeghajtók titkosításának letiltása

-   Az infrastruktúra-szolgáltatási virtuális gépeken futó Windows-ügyfél operációs rendszert futtató titkosítás engedélyezése

-   Csatlakoztatási elérési utakat tartalmazó kötetek titkosítás engedélyezése

-   A Linux virtuális gépeken (RAID) szétosztott lemezzel konfigurált titkosítás engedélyezése használatával [mdadm](https://en.wikipedia.org/wiki/Mdadm)

-   A Linux virtuális gépeken titkosítás engedélyezése [LVM (logikai kötetkezelő)](https://msdn.microsoft.com/library/windows/desktop/bb540532) adatlemezek

-   A Windows virtuális gépeken, a tárolóhelyek használatával konfigurált titkosítás engedélyezése

-   Minden Azure-nyilvános régió támogatottak.

hello megoldás nem támogatja a következő forgatókönyvek, funkcióit és technológia hello kiadásban hello:

-   Az alapszintű csomag IaaS virtuális gépeket

-   Linux IaaS virtuális gépeket egy operációs rendszer meghajtóján titkosításának letiltása

-   Infrastruktúra-szolgáltatási virtuális gépeket hello klasszikus virtuális gép létrehozási módszerének használatával létrehozott

-   Integráció a helyszíni kulcskezelő szolgáltatás

-   Az Azure Files (megosztott fájlrendszer), a hálózati fájlrendszer (NFS), a dinamikus köteteket és a Windows alapú szoftveres RAID-rendszerek használatára konfigurált virtuális gépek

## <a name="sql-azure-database-isolation"></a>Az SQL Azure Database elkülönítési
SQL Database rendszer hello Microsoft felhő alapú hello piacvezető Microsoft SQL Server motoron, és a kritikus fontosságú munkaterhelésekhez kezelésére képes relációs adatbázis-szolgáltatása. SQL-adatbázis kínál előre jelezhető adatok elkülönítési szinten fiók geográfiai / régió alapján, és a hálózat alapján – mindezt felügyelet.

### <a name="sql-azure-application-model"></a>Az SQL Azure-alkalmazásokban modell

[Microsoft SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-get-started) adatbázis az SQL Server technológiáira felhőalapú relációs adatbázis-szolgáltatás. Egy magas rendelkezésre állású, méretezhető, több-bérlős dokumentumadatbázis-szolgáltatás, a felhő Microsoft által üzemeltetett biztosít.

Az alkalmazás szempontjából SQL Azure biztosít a következő hierarchia hello: minden szinthez tartozik egy-a-többhöz elszigetelési szintek az alábbi.

![Az SQL Azure-alkalmazásokban modell](./media/azure-isolation/azure-isolation-fig10.png)

hello fiókja és -előfizetése a Microsoft Azure platform fogalmak tooassociate számlázási és kezelése.

Logikai kiszolgálók és adatbázisok SQL Azure-specifikus fogalmak és kezelt SQL Azure-ban megadott OData és TSQL-felületek használatával vagy az SQL Azure-portál, Azure-portál integrálva.

SQL Azure-kiszolgálók nem fizikai gép vagy Virtuálisgép-példányok, ehelyett olyan adatbázisok, felügyeleti és biztonsági szabályzataival, az úgynevezett "logikai master" tárolt megosztása gyűjteményei adatbázis.

![SQL Azure](./media/azure-isolation/azure-isolation-fig11.png)

Logikai fő adatbázisok a következők:

-   SQL-bejelentkezésekben használt tooconnect toohello kiszolgáló

-   Tűzfalszabályok

Számlázási és használati adatokat az SQL Azure-adatbázisok hello azonos logikai kiszolgáló lemezerőforrásai nem garantálja a hello toobe fizikai ugyanazon SQL Azure fürtben, ehelyett alkalmazások kell adnia hello cél adatbázisnevet kapcsolódás esetén.

Az ügyfél szempontjából, a logikai kiszolgáló akkor jön létre egy földrajzi grafikus régióban közben hello tényleges hello-kiszolgáló létrehozása történik, az egyik hello fürtök hello régióban.

### <a name="isolation-through-network-topology"></a>Hálózati topológia elkülönítését

Egy logikai kiszolgáló akkor jön létre, és a DNS-névvel regisztrálva van, hello DNS-név mutat toohello ún "átjáró címe" hello adott adatközpont ahol hello kiszolgáló lett elindítva.

Mögött hello VIP (virtuális IP-cím) az állapot nélküli átjáró szolgáltatások gyűjteménye van. Általánosságban elmondható átjárók beolvasása érintett koordinációs több adatforrást (master adatbázis, felhasználói adatbázis stb.) között szükség esetén. Átjáró szolgáltatások hello következő megvalósításához:
-   **TDS kapcsolat proxy használatát az ügynökökön.** Tartalmazzák a felhasználói adatbázis helyének hello háttér fürtben, hello bejelentkezési folyamat végrehajtása és majd továbbítási hello TDS csomagok toohello háttér és vissza.

-   **Adatbázis-kezelő.** Ez magában foglalja egy gyűjtemény munkafolyamatok toodo CREATE/ALTER/DROP database műveletek végrehajtására. Helyadatbázis-műveletekhez hello elemzés TDS-csomagokat, vagy explicit OData API-k is elindítható.

-   CREATE/ALTER/DROP bejelentkezési vagy felhasználói műveletek

-   Logikai kiszolgáló felügyeleti műveletei OData API-n keresztül

![Hálózati topológia elkülönítését](./media/azure-isolation/azure-isolation-fig12.png)

hello réteg mögött hello átjárók "Háttér" nevezik. Ez az egy magas rendelkezésre állású módon tárolja az összes hello adatokat. Minden adat az említett toobelong tooa "partíció" vagy "failover unit", azok legalább három replikák rendelkezik. Replikák tárolja és replikálja az SQL Server-motor és a feladatátvételt gyakran rendszer tooas "fabric" felügyelni.

Általában hello háttér-rendszer nem kommunikálni kimenő tooother rendszerek biztonsági okokból. Ez egy fenntartott toohello rendszerek hello előtér (átjáró) rétegben. hello átjáró réteg gépein korlátozott hello háttér-gépek toominimize hello támadási felületét jogosultságra jellegű védelmi mechanizmusaként.

### <a name="isolation-by-machine-function-and-access"></a>Elkülönítési funkcióval és hozzáférés
Az SQL Azure (áll másik gépre funkciók futó szolgáltatásokat. SQL Azure "Háttér" felhő adatbázis és az "előtér" (átjáró vagy felügyeleti) környezetek, általános elvet hello forgalom csak folyamatos háttér-be és ki nem osztható meg. hello előtér-környezet képes kommunikálni a toohello kívül más szolgáltatások globális, és általában csak korlátozott engedélyek szükségesek a hello háttér-(elég toocall hello belépési pontok az igényeinek tooinvoke).

## <a name="networking-isolation"></a>Hálózati elkülönítési
Az Azure-telepítés hálózati elkülönítési rétege rendelkezik. hello alábbi ábrán látható Azure biztosít toocustomers hálózati elkülönítési különböző rétegek. Ezek a rétegek natív hello Azure platformon, maga a, mind az ügyfél által megadott funkciók. Bejövő hello Internet, az Azure DDoS el vannak különítve a nagyméretű támadások elleni Azure. hello következő elkülönítési réteg az ügyfél által megadott nyilvános IP-címek (végpont), amelyek használt toodetermine mely forgalom teljen hello cloud service toohello a virtuális hálózaton keresztül. Natív Azure virtuális hálózatelkülönítés biztosítja minden más hálózati teljes elkülönítést, valamint, hogy csak a forgalom metódusok és konfigurált felhasználói elérési útját. Ezek elérési útja és módszerek hello következő rétege, ahol az NSG-k, UDR és virtuális hálózati berendezések lehet a használt toocreate elkülönítési határokat tooprotect hello alkalmazástelepítések hello védett hálózat.

![Hálózati elkülönítési](./media/azure-isolation/azure-isolation-fig13.png)

**A forgalom elkülönítése:** A [virtuális hálózati](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) hello forgalom elkülönítési határ a hello Azure platformon. Virtuális gépek (VM) több virtuális hálózat közvetlenül egy másik virtuális hálózatban, még akkor is, ha mindkét virtuális hálózat által létrehozott tooVMs hello ugyanazon ügyfél nem tud kommunikálni. Elkülönítési kritikus tulajdonság, amely biztosítja a felhasználói virtuális gépeket, kommunikációs titkos virtuális hálózaton belül marad.

[Alhálózati](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#subnets) elkülönítési a virtuális hálózat IP-címtartomány alapján további réteget biztosít. Hello virtuális hálózati IP-címek, a virtuális hálózatot lehet osztani érdekében több alhálózatra a szervezet és a biztonság. Virtuális gépek és PaaS szerepkör-példányt az toosubnets (azonos vagy azoktól eltérő) a Vneten belül további konfigurálás nélkül is kommunikálhatnak egymással. Beállíthatja úgy is [hálózati biztonsági csoport (NSG-k)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview#network-security-groups-nsg) tooallow vagy megtagadják a hálózati forgalom tooa Virtuálisgép-példány a hozzáférési lista (ACL) a NSG beállított szabályok alapján. Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni. Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello Virtuálisgép-példány alhálózaton.

## <a name="next-steps"></a>Következő lépések

- [Hálózati elkülönítési beállítások gépek vannak a Windows Azure virtuális hálózatok](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/)

Ez magában foglalja a hello klasszikus előtér- és a forgatókönyvben egy adott háttér-hálózat vagy az alhálózat gépek ahol csak engedélyezhetik bizonyos ügyfelek vagy egyéb számítógépek tooconnect tooa adott végpont egy IP-címek engedélyezési listája alapján.

- [Számítási elkülönítési](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

A Microsoft Azure biztosít a különböző számítási felhőszolgáltatásokhoz, amelyek tartalmazzák a számítási példányokért széles kijelölt & szolgáltatások automatikusan toomeet hello igényeinek az alkalmazás vagy a vállalati fel és le is méretezhető.

- [Tárolási elkülönítési](https://msenterprise.global.ssl.fastly.net/vnext/PDFs/A01_AzureSecurityWhitepaper20160415c.pdf)

A Microsoft Azure elválasztja az ügyfél Virtuálisgép-alapú számítási tárolóból. Ez az elkülönítés lehetővé teszi a számítási és tárolási tooscale egymástól függetlenül, így könnyebben tooprovide több-bérlős és elkülönítés. Ezért az Azure Storage fut, nincs hálózati kapcsolat tooAzure külön hardveren számítási kivéve logikailag. Összes kérelem futtatása, HTTP vagy HTTPS ügyfél beállítása alapján.

