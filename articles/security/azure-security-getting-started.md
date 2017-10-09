---
title: "a Microsoft Azure biztonsági használatába aaaGetting |} Microsoft Docs"
description: "Ez a cikk a Microsoft Azure biztonsági képességei és a szervezet számára, hogy az eszközök tooa felhőszolgáltatóként általános szempontok áttekintése."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 8d8a0088-c85a-48e7-bd04-2bc7b78b0691
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: c61dd99ffd0143bc7b165367dadadc977ffcf47b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-microsoft-azure-security"></a>A Microsoft Azure Security használatának első lépései
Build vagy informatikai eszközök tooa felhőszolgáltatóként át, meg vannak hagyatkoznia adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatásaival és hello vezérlők toomanage hello biztonsági a felhő alapú eszközök biztosítanak.

Azure infrastruktúrája hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez a készült, és amelyen vállalatok számára is saját igényeihez biztonsági megbízható alaprendszert biztosít. Emellett Azure tesz lehetővé az konfigurálható biztonsági beállítások és hello képességét toocontrol széles köréről őket, hogy a biztonsági toomeet hello egyéni követelményeinek a központi telepítések szabhatja testre.

Az Azure biztonsági szolgáltatásait áttekintő cikkünkben a következőkkel foglalkozunk:

* Az Azure-szolgáltatások és funkciók toohelp használható secure a szolgáltatások és az adatok Azure-ban.
* Hogyan biztosítja a Microsoft hello Azure-infrastruktúra toohelp védelme adatait és alkalmazásait.

## <a name="identity-and-access-management"></a>Identitás- és hozzáférés-kezelés
Hozzáférési tooIT infrastruktúra, az adatok és alkalmazások vezérlése fontos. A Microsoft Azure ezeket a képességeket nyújt, például az Azure Active Directory (Azure AD), az Azure Storage és a támogatási szolgáltatások számos szabványok és API-k által.

[Az Azure AD](../active-directory/active-directory-whatis.md) identitás tárház és motor, amely hitelesítési, engedélyezési és hozzáférés-vezérlés biztosít a szervezet felhasználók, csoportok, és objektumokat. Az Azure AD is kínál a fejlesztők egy hatékony módot toointegrate Identitáskezelés az alkalmazásokban. Szabványos protokollok, mint [SAML 2.0](https://en.wikipedia.org/wiki/SAML_2.0), [WS-Federation](https://msdn.microsoft.com/library/bb498017.aspx), és [OpenID Connect](http://openid.net/connect/) bejelentkezési lehetséges adja meg a platformokon, például .NET, Java, Node.js és a PHP.

a Graph API REST-alapú hello lehetővé teszi, hogy a fejlesztők tooread és írási toohello könyvtárat az bármely platformra. Támogatásával [OAuth 2.0](http://oauth.net/2/), fejlesztők létrehozhatják mobil és webes alkalmazásokhoz, amely integrálható a Microsoft és külső webes API-khoz, és állítsa be a saját biztonságos webes API-k. Nyílt forráskódú klienskódtárak érhetők el .Net, Windows Áruház, iOS és Android platformra, további kódtárak pedig fejlesztés alatt állnak.

### <a name="how-azure-enables-identity-and-access-management"></a>Hogyan biztosítja az Azure az identitás- és hozzáférés-kezelést?
Az Azure AD használható szervezete önálló, felhőbeli címtáraként, illetve a meglévő helyszíni Active Directoryval integrált megoldásként. Az integrációs szolgáltatások közé tartozik például a címtár-szinkronizálás és az egyszeri bejelentkezés (SSO). Ezek a meglévő helyszíni identitások hello reach kiterjeszti hello felhő, és hello rendszergazdai és felhasználói élmény javítása.

Az identitás- és hozzáférés-kezelés egyéb funkciói többek között a következők:

* Az Azure AD lehetővé teszi, hogy [SSO](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) tooSaaS alkalmazások, függetlenül attól, hogy a rendszer hol tárolja. Egyes alkalmazások az Azure AD-vel összevontan működnek, mások jelszavas egyszeri bejelentkezést használnak. Az összevont alkalmazások emellett a felhasználóátadást és a jelszótárolást is támogathatják.
* A hozzáférés toodata [Azure Storage](https://azure.microsoft.com/services/storage/) hitelesítéssel vezérlik. Minden tárfiók elsődleges kulccsal rendelkezik ([tárfiók kulcsa](https://msdn.microsoft.com/library/azure/ee460785.aspx), vagy SAK) és egy másodlagos titkos kulcsot (közös hozzáférésű jogosultságkódot hello vagy SAS).
* Az Azure AD Identity keresztül összevonási szolgáltatás segítségével biztosítja [Active Directory összevonási szolgáltatások](../active-directory/fundamentals-identity.md), szinkronizáláshoz és a replikáció a helyszíni címtárakban.
* [Az Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md) hello a multi-factor authentication szolgáltatás, amelyhez a felhasználók tooverify bejelentkezések a mobilalkalmazás, telefonhívással vagy szöveges üzenet használatával. Az Azure AD toohelp biztonságos helyszíni erőforrásokkal hello Azure multi-factor Authentication kiszolgálóval és az egyéni alkalmazások és könyvtárak hello SDK használatával használható.
* [Azure AD tartományi szolgáltatások](https://azure.microsoft.com/services/active-directory-ds/) lehetővé teszi, hogy az Azure virtuális gépek tooa tartományhoz tartományvezérlők üzembe helyezésének nélkül. Jelentkezzen be toothese virtuális gépek a vállalati Active Directory hitelesítő adataival, és a tartományhoz csatlakozó virtuális gépek felügyelete az Azure virtuális gépeken futó csoportházirend tooenforce biztonsági alapterveket használatával.
* [Az Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) egy magas rendelkezésre állású globális identitás szolgáltatást biztosít a felhasználók felé néző alkalmazások identitások millióinak toohundreds méretezi. Mobil- és webes platformokba is integrálható. A felhasználók bejelentkezhetnek a tooall, testre szabható felhasználói élmény mellett az alkalmazások új vagy meglévő közösségi fiókjaik használatával.

## <a name="data-access-control-and-encryption"></a>Adatok hozzáférés-vezérlése és titkosítása
Microsoft alkalmazza a feladataik elválasztását hello alapelveket és [legalacsonyabb jogosultsági szint](https://en.wikipedia.org/wiki/Principle_of_least_privilege) Azure művelet során. Az Azure támogatási személyzet hozzáférés toodata a explicit engedélyre van szüksége, és nyújtott alapon "just-in-time", amely a naplóba, és ellenőrzi, majd visszavonva hello engagement befejezése után.

Azure is biztosít több lehetőségeket biztosít az átvitel során, és inaktív adatok védelmét. Ez magában foglalja az adatok, fájlok, alkalmazások, szolgáltatások, kommunikációs és meghajtók titkosítását. Információk az Azure-ban helyezés előtt titkosítja, és is a helyszíni adatközpontját a kulcsok tárolása.

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-getting-started/sec-azgsfig1.PNG)

### <a name="azure-encryption-technologies"></a>Azure titkosítási technológiák
A rendszergazdai hozzáférés tooyour előfizetési környezetéhez adatokat gyűjthet a [Azure AD Reporting](../active-directory/active-directory-reporting-audit-events.md). Konfigurálható [a BitLocker meghajtótitkosítás](https://technet.microsoft.com/library/cc732774.aspx) Azure bizalmas adatokat tartalmazó virtuális merevlemezen.

Egyéb funkciói, amely segítséget nyújt az adatok biztonságát tookeep Azure-ban a következők:

* Alkalmazás fejlesztők létrehozhatják hello alkalmazásokba az Azure-ban telepíteni azokat a hello Windows segítségével titkosítási [CryptoAPI](https://msdn.microsoft.com/library/ms867086.aspx) és a .NET-keretrendszer.
* Teljes ellenőrzés Azure Blob Storage ügyféloldali titkosítással hello kulcsok. hello társzolgáltatás soha nem hello kulcsok látja, és nem képes hello adatok visszafejtése.
* [Az Azure Rights Management (Azure RMS)](https://technet.microsoft.com/library/jj585026.aspx) (a hello [RMS SDK](https://msdn.microsoft.com/library/dn758244.aspx)) fájl- és adatok szintű titkosítást és adatvesztés megelőzési keresztül csoportházirend-alapú hozzáférés-felügyeletet biztosít.
* Az Azure által támogatott [tábla- és oszlop titkosítás (TDE/törlése)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database.aspx) az SQL Server virtuális gépeket, és támogatja a külső helyszíni kulcskezelő kiszolgálók adatközpontokban.
* Tárfiókkulcsok, megosztott hozzáférési aláírásokkal, felügyeleti tanúsítványok, és más olyan egyedi tooeach Azure-bérlőhöz.
* Azure [StorSimple](http://www.microsoft.com/server-cloud/products/storsimple/overview.aspx) hibrid tárolási tooAzure tárolási feltöltés előtt titkosítja a 128 bites nyilvános/titkos kulcspár keresztül.
* Azure támogatja, és számos titkosítási mechanizmusok, beleértve az SSL/TLS, az IPsec és az AES, attól függően, hogy hello adattípusok, tárolók és átviteli módokat használja.

## <a name="virtualization"></a>Virtualizáció
hello Azure platformon virtualizált környezetet használ. A felhasználói példányok működhetnek önálló virtuális gépek, amelyek nem rendelkeznek hozzáféréssel tooa fizikai gazdagép-kiszolgálón, és kikényszeríti a elkülönítés használatával fizikai [processzor (ring-0/ring-3) a jogosultsági szintek](https://en.wikipedia.org/wiki/Protection_ring).

A(z) 0 akkor hello jogosultsággal rendelkező és a 3 hello legalább. egy alacsonyabb szintű jogosultságokat igénylő Ring 1 hello vendég operációs rendszer fut, és alkalmazások hello minimális jogosultságokkal rendelkező Ring 3. A virtualizálási fizikai erőforrások tooa egyértelmű elkülönítése vezet a vendég operációs rendszer és a hipervizor hello két további biztonsági elkülönítése eredményezve között.

hello Azure hipervizor úgy viselkedik, mint egy micro kernel, és adja át az összes hardver hozzáférési kérelmek gazdagépről Vendég virtuális gépek toohello feldolgozásra a megosztott memória felületén VMBus hívása. Ez megakadályozza, hogy felhasználók nyers olvasási, írási és végrehajtási hozzáférést toohello rendszer, és csökkenti a rendszer az erőforrások megosztása hello kockázatát.

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-getting-started/sec-azgsfig2.PNG)

### <a name="how-azure-implements-virtualization"></a>Hogyan valósítja meg az Azure a virtualizációt?
Azure által megvalósított hello hipervizor, és a háló vezérlő ügynök által konfigurált hipervizor tűzfal (csomagszűrők) használja. Ez segít megvédeni a bérlőket a jogosulatlan hozzáféréstől. Alapértelmezés szerint minden forgalmat blokkol, amikor létrejön egy virtuális gép, és ezután hello fabric controller ügynök konfigurálja hello csomagok szűrő tooadd *szabályok és kivételek* tooallow engedélyezett forgalmat.

Az itt programozott szabályok két kategóriába sorolhatók:

* **Számítógép-konfiguráció vagy infrastruktúra szabályok**: alapértelmezés szerint minden kommunikációja blokkolva van. Nincs kivételek tooallow egy virtuális gép toosend és DHCP- és DNS-forgalom fogadására. Virtuális gépek is elküldheti forgalom toohello "nyilvános" internet és küldési forgalom tooother belüli virtuális gépek hello fürt és hello OS aktiválási kiszolgálón. a célok megengedett kimenő hello virtuális gépek listája nem tartalmazza a Azure útválasztó alhálózatok, Azure felügyeleti biztonsági vége és más Microsoft tulajdonságai.
* **Szerepkör konfigurációs fájl**: hello hello bérlői modell bejövő hozzáférés-vezérlési listák (ACL) alapján határozza meg. Például ha egy bérlőt egy előtér-webkiszolgáló 80-as porton egyes virtuális gépen, majd Azure megnyitja TCP 80-as port tooall IP-cím beállításakor a végpont a hello [Azure klasszikus üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md). Ha hello virtuális gépen fut, hátsó vége vagy feldolgozói szerepkör van, ezt követően megnyitja hello feldolgozói szerepkör egyetlen toohello virtuális gép hello belül azonos bérlői.

## <a name="isolation"></a>Elkülönítés
Egy másik fontos felhőalapú biztonsági vonatkozó követelmény akkor elválasztási tooprevent központi telepítések a megosztott, több-bérlős architektúrák közötti adatokat a jogosulatlan és nem szándékos átvitelét.

Az Azure valósít meg [hálózati hozzáférés-vezérlés](https://azure.microsoft.com/blog/network-isolation-options-for-machines-in-windows-azure-virtual-networks/) és elkülönítése keresztül VLAN-elkülönítéssel, hozzáférés-vezérlési listákat, terheléselosztók, és az IP-szűrők betöltése. Korlátozza a külső forgalom bejövő tooports és protokollok a virtuális gépeken futó adhat meg. Az Azure valósít meg hálózati szűrési megtévesztésre tooprevent forgalom, és korlátozhatja a bejövő és kimenő forgalmat tootrusted webplatform-összetevők. Olyan forgalmi szabályzatok települnek a határon lévő védelmi eszközökre, amelyek alapértelmezés szerint letiltják a forgalmat.

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-getting-started/sec-azgsfig3.PNG)

Hálózati címfordítás (NAT) használatos tooseparate belső hálózati forgalom és a külső forgalom. A belső forgalom kívülről nem irányítható. A kívülről irányítható [virtuális IP-címeket](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) a rendszer [belső dinamikus IP-címekre](http://blogs.msdn.com/b/cloud_solution_architect/archive/2014/11/08/vips-dips-and-pips-in-microsoft-azure.aspx) fordítja, amelyek csak az Azure-on belül irányíthatók.

Külső forgalom tooAzure virtuális gépek hozzáférés-vezérlési listák keresztül van tűzfallal, útválasztók, terheléselosztók, és 3. rétegbeli kapcsolók. Csak bizonyos ismert protokollok engedélyezettek. Hozzáférés-vezérlési listák a Vendég virtuális gépekről származó tooother VLAN-kezelésre szolgáló hely toolimit forgalom. Emellett a forgalom IP-szűrők a hello gazda operációs rendszer további korlátok hello forgalom mindkét adatok kapcsolat és hálózati rétegek keresztül szűrve.

### <a name="how-azure-implements-isolation"></a>Hogyan valósítja meg az Azure az elkülönítést?
hello Azure Fabric Controller infrastruktúra erőforrások lefoglalása tootenant munkaterhelések felelős, és a hello állomás toovirtual gépekről egyirányú kommunikációt kezeli. hello Azure hipervizor kikényszeríti a memória, és a folyamat elkülönítése virtuális gépeket, és biztonságosan irányítja a hálózati forgalom tooguest OS bérlők. Azure is a bérlők számára, a tárolás és a virtuális hálózatok elkülönítési valósítja meg.

* Minden Azure AD-bérlő logikailag elkülönített biztonsági határokat használatával.
* Az Azure storage-fiókok egyedi tooeach előfizetés, és hozzáférést kell hitelesíteni a tárfiók kulcsának használatával.
* Virtuális hálózatok logikailag elszigetelt egyedi magánhálózati IP-címek, a tűzfalak és a hozzáférés-vezérlési listák IP keresztül. A terheléselosztók útvonal-forgalom toohello megfelelő bérlők végpontdefiníciók alapján.

## <a name="virtual-networks-and-firewalls"></a>Virtuális hálózatok és a tűzfalon
Hello [elosztott és a virtuális hálózatok](http://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx) Azure súgó gondoskodjon arról, hogy a saját hálózati forgalom logikailag más Azure virtuális hálózat adatforgalmának szigetelve.

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-getting-started/sec-azgsfig4.PNG)

Az előfizetés is tartalmaz a több elkülönített magánhálózatot (és tűzfal, a terheléselosztás és a hálózati címfordítás).

Azure biztosít a hálózati elkülönítés az egyes Azure-ban három elsődleges szint fürt toologically elkülönített forgalmat. [Virtuális helyi hálózatok](https://azure.microsoft.com/services/virtual-network/) (VLAN-ok) szolgálnak hello Azure hálózati hello részeinek tooseparate ügyfél forgalmát. Hozzáférés toohello Azure-hálózat külső hello fürtről nem használható terheléselosztók keresztül.

Hello hipervizor virtuális kapcsolón, meg kell felelnie a hálózati forgalom tooand a virtuális gépekről. hello IP-szűrő összetevőjét hello legfelső szintű OS elkülöníti hello legfelső szintű a virtuális gép hello Vendég virtuális gépek és hello Vendég virtuális gépek egymástól. Segítségével végzi a bérlő-csomópontok és hello közötti forgalom toorestrict kommunikációs szűrés nyilvános Internet (ügyfél hello szolgáltatás konfigurációja alapján), elkülönítése azokat a többi bérlőtől.

hello IP-szűrő megakadályozza, hogy a Vendég virtuális gépek:

* Hamisított forgalom létrehozásakor.
* Toothem forgalmat fogadó nem foglalkozik.
* Irányítja a forgalmat tooprotected infrastruktúra végpontok.
* Küldésekor vagy fogadásakor nem megfelelő szórási forgalmat.

A virtuális gépek alakzatot elhelyezhet [Azure virtuális hálózataihoz](https://azure.microsoft.com/documentation/services/virtual-network/). Ezek a virtuális hálózatok konfigurálja a helyszíni környezetben hasonló toohello hálózatok is rendelkezésre állnak hol általában társítva van egy virtuális kapcsolóhoz. Virtuális gépek toohello azonos virtuális hálózatban kommunikálhatnak egymással csatlakoztatva további konfiguráció nélkül. Beállíthatja úgy is más alhálózatok virtuális hálózaton belül.

Hello Azure virtuális hálózati technológiák toohelp biztonságos kommunikáció követően a virtuális hálózaton használható:

* [**Hálózati biztonsági csoportokkal (NSG-k)**](../virtual-network/virtual-networks-nsg.md). Használhatja az NSG toocontrol forgalom tooone vagy több virtuálisgép-példánya a virtuális hálózat. A hálózati biztonsági csoport olyan hozzáférés-vezérlési szabályokat tartalmaz, amelyek engedélyezik vagy megtagadják a forgalmat a forgalom iránya, a protokoll, a forrás címe és portja, illetve a cél címe és portja alapján.
* [**Felhasználó által definiált útválasztási**](../virtual-network/virtual-networks-udr-overview.md). Hello a csomagok útválasztását a virtuális készülék hello következő ugrás tooa alhálózatot toogo tooa virtuális hálózati biztonsági készülékek áramló csomagok számára megadott felhasználó által definiált útvonalak létrehozásával azt is szabályozhatja.
* [**IP-továbbítás**](../virtual-network/virtual-networks-udr-overview.md). A virtuális hálózati biztonsági készülékek képes tooreceive bejövő forgalmat, amely nincs megcímzett tooitself kell lennie. a virtuális gép tooreceive forgalom tooallow címzett tooother a célok, hello virtuális gép IP-továbbítás engedélyezi.
* [**Alagúthasználat kényszerítése**](../vpn-gateway/vpn-gateway-about-forced-tunneling.md). A kényszerített bújtatás lehetővé teszi, hogy átirányítási vagy "kényszerített" az összes internetes-forgalmat a virtuális gépek virtuális hálózati hátsó tooyour helyszíni helyen vizsgálati és naplózási pont-pont VPN-alagúton keresztül állítja elő
* [**Végponti ACL-eket**](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Azt is szabályozhatja, hogy mely számítógépek engedélyezettek bejövő kapcsolatok hello Internet tooa virtuális gépről a virtuális hálózaton végponti ACL-eket meghatározásával.
* [**Partnerhálózati biztonsági megoldások**](https://azure.microsoft.com/marketplace/). Számos hálózati biztonsági partnermegoldások, amely hello Azure piactéren érheti el.

### <a name="how-azure-implements-virtual-networks-and-firewalls"></a>Hogyan Azure megvalósítja-e a virtuális hálózatok és a tűzfalon
Azure csomagokat szűrő tűzfalak megvalósítja az összes gazdagép és Vendég virtuális gép alapértelmezés szerint. Windows operációs rendszer lemezképet rögzíthet hello Azure piactérről is a Windows tűzfal alapértelmezés szerint engedélyezve van. Nyilvánosan elérhető Azure-hálózatok vezérlő kommunikáció hello határain terheléselosztók IP hozzáférés-vezérlési listák felhasználói rendszergazdák által kezelt alapján.

Amikor az Azure a normál működés részeként vagy egy katasztrófa során áthelyezi a felhasználói adatokat, ezt privát, titkosított kommunikációs csatornákon keresztül teszi. A virtuális hálózatok és a tűzfalon Azure toouse által alkalmazott egyéb funkciók a következők:

* **Natív gazdagép tűzfalának**: Azure Service Fabric és az Azure Storage futtatnak egy natív az operációs rendszer, amelyek nem hipervizor rendelkezik. Ezért hello előző két szabálykészlet hello windows tűzfal van beállítva. Tárolási natív toooptimize teljesítmény fut.
* **Gazdagép tűzfalának**: hello állomás tűzfal tooprotect hello gazda operációs rendszer hello hipervizor futtató. hello szabályokat programozott tooallow csak hello Service Fabric controller és irányítják a mezőkbe tootalk toohello gazda operációs rendszer egy adott portot. hello más kivételek olyan tooallow DHCP válasz és a DNS-válaszok. Azure hello sablon tűzfalszabály hello gazda operációs rendszer a gép konfigurációs fájlját használja. hello gazdagéphez a Windows tűzfal beállításai toopermit kommunikáció csak ismert, hitelesített forrásból származó külső támadások elleni védi.
* **Vendég tűzfal**: hello virtuális gép kapcsoló csomagszűrők azonban programozott (például a Windows tűzfal adat hello hello vendég operációs rendszer) különböző szoftverfrissítési hello szabályai replikálja. hello Vendég virtuális gép tűzfal lehet konfigurált toorestrict kommunikációs tooor hello Vendég virtuális gépen, akkor is, ha hello kommunikációs hello kiszolgáló IP-szűrő konfigurációja által engedélyezett. Például választhatja, hogy toouse hello Vendég virtuális gép tűzfal toorestrict kommunikációját két a Vnetek lett konfigurálva tooconnect tooone egy másik.
* **Tárolási tűzfal (Keretrendszer)**: hello tűzfal hello tárolási kezelőfelületre szűrők forgalom toobe csak a 80-as/443-as és egyéb szükséges segédprogram portokkal. hello tűzfal hello tárolási háttérben futó korlátozza a kommunikáció toocome csak a tárolási előtér-kiszolgálókról.
* **Virtuális hálózati átjáró**: hello [Azure virtuális hálózati átjáró](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) , a munkaterhelés Azure virtuális hálózat tooyour kapcsolódás hello létesítmények közötti átjáró helyszíni hely szolgál. Tooon helyszíni helyek keresztül szükség tooconnect [IPsec-helyek VPN-alagutat](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), illetve [ExpressRoute](../expressroute/expressroute-introduction.md) kapcsolatok. Az IPsec/IKE VPN-alagutat hello átjárók IKE kézfogások hajtsa végre, és hello IPsec S2S VPN-alagutat hello virtuális hálózatok és a helyszíni helyek közötti létrehozásához. Virtuális hálózati átjárók is leáll [pont-pont VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

## <a name="secure-remote-access"></a>Biztonságos távoli elérése
Hello felhőben tárolt adatok kell rendelkezik a megfelelő védelmi szolgáltatás engedélyezve tooprevent biztonsági réseket és bizalmassága és az átvitel közben sértetlenségét. Ide tartoznak a hálózati vezérlők, amelyek illeszkednek a szervezetek házirendalapú, ellenőrizhető identitás- és hozzáférés-felügyeleti mechanizmusaihoz.

Beépített kriptográfiai technológia lehetővé teszi tooencrypt kommunikációs belül és között központi telepítések, Azure-régiók között, valamint a Azure tooon helyszíni adatközpontokkal. Rendszergazdai hozzáférés toovirtual gépek keresztül [távoli asztali munkamenetek](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), [távoli Windows PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/09/07/weekend-scripter-remoting-the-cloud-with-windows-azure-and-powershell.aspx), és hello Azure-portálon mindig titkosítja.

toosecurely kiterjesztése a helyszíni adatközpont toohello felhőalapú, az Azure biztosít a [telephelyek közötti VPN](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) és [pont – hely típusú VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md), és a dedikált hivatkozások [ExpressRoute](../expressroute/expressroute-introduction.md)(virtuális hálózatok VPN-kapcsolaton keresztül titkosított kapcsolatok tooAzure).

### <a name="how-azure-implements-secure-remote-access"></a>Hogyan valósítja meg az Azure a biztonságos távoli hozzáférést?
Azure-portálon kapcsolatok toohello mindig kell hitelesíteni, és igényelnek-e az SSL/TLS. Felügyeleti tanúsítványok tooenable biztonságos felügyeletének konfigurálásához. Például a szabványos biztonsági protokollokat [SSTP](https://technet.microsoft.com/magazine/2007.06.cableguy.aspx) és [IPsec](https://en.wikipedia.org/wiki/IPsec) teljes mértékben támogatottak.

Az [Azure ExpressRoute](../expressroute/expressroute-introduction.md) használatával privát kapcsolatok hozhatók létre az Azure-adatközpontok és a helyszíni vagy a bérelt kiszolgálói környezetben üzemelő infrastruktúra között. Az ExpressRoute-kapcsolatok ne lépje át hello nyilvános internethez. További megbízhatóságát, gyorsabb sebességű, kisebb késések és nagyobb biztonságot nyújtana tipikus internetes hivatkozások biztosítanak. Néhány esetben az adatok közötti átviteléhez a helyszíni helyek és Azure ExpressRoute kapcsolattal is partíciónként akár jelentős költségelőnyökhöz juthat.

## <a name="logging-and-monitoring"></a>Naplózás és figyelés
Azure biztonsági-kapcsolódó eseményeket, amelyek létrehozzák elővizsgálati hitelesített naplózása biztosít, amely visszafejtett toobe ellenálló tootampering. Ez magában foglalja a rendszer-információkat, például a virtuális gépek Azure-infrastruktúra és az Azure AD and security event logs. Biztonsági figyelésének tartalmazza, például a DHCP vagy DNS-kiszolgáló IP-címek; változásait események gyűjtése megkísérelt hozzáférésnek tooports, protokollok vagy IP-címek zárolt azért; biztonsági házirend, vagy a tűzfal beállításainak; változásai fiók vagy csoport létrehozása; nem várt folyamatok vagy és illesztőprogram-telepítőfájl.

![Microsoft Antimalware szolgáltatás az Azure-ban](./media/azure-security-getting-started/sec-azgsfig5.PNG)

A naplók rögzítik a rendszerjogosultságú felhasználói hozzáféréseket és tevékenységeket, a hitelesített és jogosulatlan hozzáférési kísérleteket, a rendszerkivételeket, és ezeket az információbiztonsági eseményeket a rendszer a megadott ideig megőrzi. a naplók hello megőrzési oka közül naplógyűjtést és adatmegőrzési tooyour saját követelmények konfigurálása.

### <a name="how-azure-implements-logging-and-monitoring"></a>Hogyan valósítja meg az Azure a naplózást és a figyelést?
Azure telepíti a felügyeleti ügynökök (MA) és az Azure biztonsági Monitor (ASM) ügynökök tooeach számítási, tárolási vagy hálócsomópont felügyelete alatt álló akár natív vagy virtuális. Minden felügyeleti ügynök konfigurált tooauthenticate tooa team tárolási szolgáltatásfiók hello Azure tanúsítványtárolóból kapott, és előre előre konfigurált diagnosztikai tanúsítvánnyal és az esemény adatainak toohello tárfiók. Ügynökök nincsenek telepített toocustomers virtuális gépek.

Az Azure rendszergazdák naplók a hitelesített és ellenőrzött toohello naplók webes portálon keresztül férnek hozzá. A rendszergazdák feldolgozhatják, szűrhetik és elemezhetik a naplókat, illetve összefüggéseket kereshetnek közöttük. hello Azure szolgáltatás team storage-fiókok a naplók védi a közvetlen rendszergazdai hozzáférés toohelp megakadályozása napló illetéktelen módosítások ellen.

A Microsoft naplókat gyűjt, az hello Syslog protokollt használó hálózati eszközök és a kiszolgálók használatával a Microsoft naplózási szolgáltatások (ACS). Ezek a naplók, amelyből gyanús események kapcsolatos riasztások akkor jönnek létre jelentkezzen az adatbázisba kerülnek. hello rendszergazdai hozzáférést, és ezek a naplók elemzése.

[Az Azure Diagnostics](https://msdn.microsoft.com/library/azure/gg433048.aspx) egyik szolgáltatása, amely lehetővé teszi az Azure-ban futó alkalmazáshoz toocollect diagnosztikai adatokat Azure. Ez az a Hibakeresés és hibaelhárítás, méri a teljesítményt, erőforrás-használat, a forgalom elemzése, a kapacitástervezési figyelés és naplózás diagnosztikai adatokat. Hello diagnosztikai adatgyűjtés után, azok átvitt tooan adatmegőrzési Azure storage-fiók. Átvitel vagy ütemezése vagy az igény szerinti.

## <a name="threat-mitigation"></a>Fenyegetés-megoldás
Továbbá tooisolation, titkosítási és szűrés Azure fenyegetés megoldás mechanizmusok és folyamatok tooprotect infrastruktúra és a szolgáltatások számos funkcióit használja. Ezek közé tartoznak a belső szabályozások és használt technológiák toodetect, és szervizelheti azokat a speciális fenyegetések, például DDoS, a jogosultságok eszkalálását és a hello [OWASP felső 10-es](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project).

hello biztonsági vezérlők és kockázatkezelési eljárások hely toosecure csökkentheti a felhőalapú infrastruktúra rendelkezik-e a Microsoft hello biztonsági események kockázatát. A hello incidens esemény hello biztonsági incidens Management (SIM) csapat hello Microsoft Online biztonsági szolgáltatásait és megfelelőségi (OSSC) csapaton belüli kész toorespond bármikor.

### <a name="how-azure-implements-threat-mitigation"></a>Hogyan valósítja meg az Azure a veszélyelhárítást?
Azure biztonsági vezérlők rendelkezik hely tooimplement fenyegetés megoldás, és a is toohelp ügyfelek csökkentheti a környezetükben lévő potenciális fenyegetések ellen. hello következő összefoglaljuk hello Azure által biztosított fenyegetés megoldás képességeket:

* [Az Azure kártevőirtó](azure-security-antimalware.md) összes infrastruktúra-kiszolgálók alapértelmezés szerint engedélyezve van. Opcionálisan engedélyezheti azt a saját virtuális gépekről.
* A Microsoft fenntartja a folyamatos kiszolgálók, hálózatok és alkalmazások toodetect fenyegetések átívelő figyelésére is alkalmas, és megelőzheti a biztonsági réseket. Automatizált riasztások értesíthetők a rendszergazdák rendellenes jelenségeket, így azokat a külső és belső fenyegetések tootake kiigazító intézkedéseket.
* Külső biztonsági megoldások telepítheti az előfizetések, például a webalkalmazási tűzfalak belül [Barracuda](https://techlib.barracuda.com/ng54/deployonazure).
* A Microsoft megközelítés toopenetration tesztelés kiterjedjen "[piros-összevonás](http://download.microsoft.com/download/C/1/9/C1990DBA-502F-4C2A-848D-392B93D9B9C3/Microsoft_Enterprise_Cloud_Red_Teaming.pdf)," magában foglalja a Microsoft Információbiztonsági szakemberei támadása (nem vevő) éles rendszerek Azure tootest védelmekkel valós, szemben a speciális , állandó fenyegetések.
* Integrált telepítési rendszerek hello terjesztési és biztonsági javítások telepítési kezeléséhez hello Azure platformon.

## <a name="next-steps"></a>Következő lépések
[Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/)

[Az Azure Security csapat blogja](http://blogs.msdn.com/b/azuresecurity/)

[Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

[Active Directory blog](http://blogs.technet.com/b/ad/)
