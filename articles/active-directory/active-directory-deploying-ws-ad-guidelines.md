---
title: "a központi telepítése Windows Server Active Directory telepítése Azure virtuális gépekre aaaGuidelines |} Microsoft Docs"
description: "Ha tudja, hogyan toodeploy AD tartományi szolgáltatások és a helyszíni, AD összevonási szolgáltatások megtudhatja, hogyan működnek az Azure virtuális gépeken."
services: active-directory
documentationcenter: 
author: femila
manager: femila
editor: 
ms.assetid: 04df4c46-e6b6-4754-960a-57b823d617fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/26/2017
ms.author: femila
ms.openlocfilehash: 9ad5a5f138a6402cbb656d9160545846051207b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guidelines-for-deploying-windows-server-active-directory-on-azure-virtual-machines"></a>Windows Server Active Directory telepítése Azure virtuális gépekre vonatkozó irányelvek
Ez a cikk azt ismerteti, hogy telepítené a Windows Server Active Directory tartományi szolgáltatások (AD DS) és az Active Directory összevonási szolgáltatások (AD FS) helyszíni és a Microsoft Azure virtuális gépeken történő üzembe fontos különbségei hello.

## <a name="scope-and-audience"></a>Hatókör és a célközönség
hello cikk célja az azok helyszíni Active Directory üzembe helyezésével járó már észlelt. Az Active Directory telepítése a Microsoft Azure virtuális gépek vagy az Azure virtuális hálózatok és a hagyományos a helyszíni Active Directory-környezetekben hello különbségei magában foglalja. Az Azure virtuális gépek és az Azure virtuális hálózatot olyan infrastruktúra,--szolgáltatás (IaaS) számára készült számítási erőforrások hello felhőben szervezetek tooleverage részét képezik.

A nem ismeri az AD központi telepítése, lásd: hello [Active Directory tartományi szolgáltatások telepítési útmutatója](https://technet.microsoft.com/library/cc753963) vagy [az AD FS központi telepítésének megtervezése](https://technet.microsoft.com/library/dn151324.aspx) szükség szerint.

Ez a cikk feltételezi, hogy a hello olvasó ismeri a következő fogalmak hello:

* Windows Server AD DS üzembe helyezése és kezelése
* A DNS-infrastruktúra-toosupport egy Windows Server Active Directory tartományi szolgáltatások telepítését és konfigurálását
* Windows Server AD FS telepítése és kezelése
* Telepítése, konfigurálása és kezelése a függő entitások alkalmazásainak (webhelyek és webalkalmazások szolgáltatások), Windows Server AD FS-jogkivonatokat médiafolyamainak fogadására
* Általános virtuális gép fogalmakkal, mint a hogyan tooconfigure a virtuális gép, virtuális lemezeket, és a virtuális hálózatok

Ez a cikk emel ki, melyik Windows Server AD DS vagy AD FS részben telepített a helyszínen, míg részben telepítve az Azure virtuális gépeken egy hibrid környezet-forgatókönyv hello követelményei. hello dokumentum először hello kritikus különbségei és a helyszíni és a tervezési és üzembe helyezési érintő fontos döntési pontokat az Azure virtuális gépeken futó Windows Server AD DS és AD FS tartalmazza. hello papír hello részeinek irányelvek ismerteti az egyes hello döntési pontokat további információkhoz juthat, és hogyan tooapply hello irányelvek toovarious telepítési forgatókönyvek.

Ez a cikk nem tér hello konfigurálása [Azure Active Directory](http://azure.microsoft.com/services/active-directory/), ez az identitás kezelése és hozzáférés-vezérlés képességeinek felhőalkalmazásokhoz biztosító REST-alapú szolgáltatás. Az Azure Active Directory (Azure AD) és a Windows Server Active Directory tartományi Szolgáltatásokban, azonban tervezett toowork együtt tooprovide egy identitás- és hozzáférés-kezelési megoldást mai hibrid informatikai környezetekben, és a modern alkalmazások. toohelp hello különbségek és a Windows Server Active Directory tartományi szolgáltatások és az Azure AD közötti kapcsolatokat, vegye figyelembe a következőket hello:

1. Előfordulhat, hogy futtatja a Windows Server Active Directory tartományi szolgáltatások hello felhőben Azure virtuális gépeken futó Azure tooextend használata esetén a helyszíni adatközpontját hello felhőbe.
2. Használhatja az Azure AD toogive a felhasználók egyszeri bejelentkezésre tooSoftware,--szolgáltatás (SaaS) alkalmazások. A Microsoft Office 365 ezt a technológiát használja, például és Azure-ban vagy más felhőalapú platformokon futó alkalmazásokhoz is használható.
3. Használhatja az Azure AD (a hozzáférés-vezérlési szolgáltatásban) toolet felhasználók bejelentkezés Facebook, Google, a Microsoft és más identity providers tooapplications hello felhőalapú vagy helyszíni tárolt származó identitásokkal.

Ezek a különbségek kapcsolatos további információkért lásd: [Azure Identity](fundamentals-identity.md).

## <a name="related-resources"></a>Kapcsolódó források (lehet, hogy a cikkek angol nyelvűek)
Előfordulhat, hogy töltse le és futtassa a hello [Azure virtuális gép Readiness Assessment](https://www.microsoft.com/download/details.aspx?id=40898). hello assessment automatikusan vizsgálja meg a helyszíni környezetben és testre szabott jelentést kell készítenie a hello alapján útmutatás található az ebben a témakörben toohelp hello környezet tooAzure az áttelepítést.

Javasoljuk, hogy először is tekintse át hello oktatóanyagok útmutatók és videók, a következő témakörök hello:

* [Az Azure portál hello Cloud-Only virtuális hálózat konfigurálása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)
* [A telephelyek közötti VPN hello Azure-portál konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md)
* [Egy új Active Directory-erdő telepítése Azure virtuális hálózaton](active-directory-new-forest-virtual-machine.md)
* [Az Active Directory replika tartományvezérlő telepítése az Azure-on](active-directory-install-replica-active-directory-domain-controller.md)
* [A Microsoft Azure informatikai szakemberek IaaS: (01) virtuális gép – alapok](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
* [A Microsoft Azure informatikai szakemberek IaaS: (05) létrehozása virtuális hálózatok és a létesítmények közötti kapcsolat](https://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)

## <a name="introduction"></a>Bevezetés
hello Azure virtuális gépeken futó Windows Server Active Directory telepítésének alapvető követelményei eltérőek nagyon kevés a központi telepítése, akkor a helyszíni virtuális gépek (és toosome mértékben, fizikai gépek). Például hello a kis-és Windows Server AD DS-ben, ha hello tartományvezérlők (DC), az Azure virtuális gépeken telepített replikák egy meglévő helyszíni vállalati tartományban/erdőben, és nagymértékben hello Azure-telepítés lehet kezelni, hello ugyanúgy, mint Előfordulhat, hogy minden további Windows Server Active Directory hely kezeli. Ez azt jelenti, hogy alhálózatok definiálni kell a Windows Server AD DS-ben létrehozott hely, hello alhálózatok toothat helyhez kapcsolódó és tooother helyeket használva a megfelelő helykapcsolatok csatlakoztatva. Azonban bizonyos különbségek vannak, amelyek közös tooall Azure-telepítések és néhány, amely eltérő függően toohello adott központi telepítési forgatókönyv. Két alapvető különbség az alább vázolt:

### <a name="azure-virtual-machines-may-need-connectivity-toohello-on-premises-corporate-network"></a>Az Azure virtuális gépek esetleg csatlakozási toohello helyszíni vállalati hálózathoz.
Csatlakozás az Azure virtuális gépek hátsó tooan helyszíni vállalati hálózat Azure virtuális hálózat, amely tartalmazza a pont-pont vagy a hely pont közötti virtuális magánhálózati (VPN) szükséges összetevő képes tooseamlessly csatlakozzon az Azure virtuális gépek és a helyszíni gépeket. A VPN-összetevő azt is lehetővé teszi a helyszíni tartományi tag számítógépekre tooaccess egy Windows Server Active Directory-tartományhoz, amelynek tartományvezérlők kizárólag az Azure virtuális gépeken üzemeltetett. Fontos toonote azonban, hogy hello VPN meghiúsul, a hitelesítés és a Windows Server Active Directory függő más műveletek is sikertelenek lesznek. Felhasználók is képes toosign meglévő gyorsítótárazott hitelesítő adataival, minden társ-társ vagy ügyfél-kiszolgáló hitelesítési kísérleteket, mely jegyekhez rendelkezik még toobe kiadott vagy elavult sikertelen lesz.

Lásd: [virtuális hálózati](http://azure.microsoft.com/documentation/services/virtual-network/) bemutatója videó és lépésről lépésre haladó oktatóprogramot, beleértve a listájának [a telephelyek közötti VPN konfigurálása az Azure-portálon hello](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Egy Azure virtuális hálózaton, egy a helyszíni hálózati kapcsolattal nem rendelkező Windows Server Active Directory is telepítheti. Ebben a témakörben hello irányelvek azonban azt feltételezik, hogy egy Azure virtuális hálózatot használja, mert IP-címzési alapvető tooWindows Server képességeket biztosít.
> 
> 

### <a name="static-ip-addresses-must-be-configured-with-azure-powershell"></a>Statikus IP-címek az Azure PowerShell kell konfigurálni.
Dinamikus címek alapértelmezés szerint kiosztott, de hello Set-AzureStaticVNetIP parancsmag tooassign egy statikus IP-címet használja helyette. Egy statikus IP-címet, amely szolgáltatásjavításnak és a virtuális gép leállítási vagy újraindítási keresztül egészen addig megmarad állítja. További információkért lásd: [statikus belső IP-címet a virtuális gépek](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/).

## <a name="BKMK_Glossary"></a>Kifejezések és meghatározások
hello különböző Azure technológia, amely ebben a cikkben lesz hivatkozva feltételek nem teljes listája látható.

* **Az Azure virtuális gépek**: hello IaaS ajánlat, amely lehetővé teszi az ügyfelek toodeploy futó virtuális gépeket szinte bármilyen hagyományosan az Azure-ban a helyszíni kiszolgáló munkaterhelését.
* **Azure-beli virtuális hálózat**: hello hálózati szolgáltatás, amely lehetővé teszi, hogy a felhasználók létrehozása és kezelése az Azure virtuális hálózatairól és biztonságosan kapcsolja őket az Azure-ban tootheir saját helyszíni hálózati infrastruktúra a virtuális magánhálózat (VPN) segítségével.
* **Virtuális IP-cím**: az internet felé néző IP-címet, amely nem kötött tooa adott számítógép vagy a hálózati kártya. Felhőszolgáltatások rendelt egy virtuális IP-címet, amely átirányított tooan Azure virtuális gép hálózati forgalom fogadására. Egy virtuális IP-cím egy tulajdonságát egy egy vagy több Azure virtuális gépet tartalmazó felhő-szolgáltatás. Ne feledje, hogy az Azure virtuális hálózat legalább egy felhő-szolgáltatások tartalmazhatnak. Virtuális IP-címet adja meg a natív terheléselosztási képességeit.
* **Dinamikus IP-cím**: hello IP-címet, csak belső azt. Akkor kell beállítani, mint egy statikus IP-cím (Set-AzureStaticVNetIP hello parancsmag használatával) hello tartományvezérlő/DNS-kiszolgálói szerepkört futtató virtuális gépekhez.
* **Szolgáltatás javító**: hello folyamat, amely Azure automatikusan ad egy szolgáltatás tooa futó állapot újra után azt észleli, hogy hello szolgáltatást nem sikerült. Javítás szolgáltatás az egyik rugalmasságot és rendelkezésre állást támogató Azure hello aspektusait. Amíg nem valószínű, hello eredménye a következő incidens javító egy virtuális gépen futó tartományvezérlő szolgáltatás hasonló tooan nem tervezett újraindítás, de néhány-hatásai:
  
  * hello virtuális hálózati adaptert a virtuális gép hello változik.
  * hello hello virtuális hálózati adapter MAC-cím változik.
  * hello hello VM processzor/CPU azonosítója változik.
  * hello hello virtuális hálózati adapter IP-konfiguráció változatlan marad, amíg a virtuális gép hello csatolt tooa virtuális hálózati és hello a virtuális gép IP-cím statikus.
  
  Ezek közül a viselkedésmódok nincs hatással a Windows Server Active Directory, mert a hello MAC-cím vagy a processzor/CPU-azonosítója nem függőség van, és minden Windows Server Active Directory-környezetekben Azure toobe futó Azure virtuális hálózat fent leírt módon történő használata ajánlott .

## <a name="is-it-safe-toovirtualize-windows-server-active-directory-domain-controllers"></a>Az azt a Windows Server Active Directory-tartományvezérlők biztonságos toovirtualize?
Windows Server Active Directory-tartományvezérlők telepítése Azure virtuális gépeken van tulajdonos toohello azonos útmutatók tartományvezérlők helyileg futó virtuális gépen. A biztonságos célszerű futtató virtualizált tartományvezérlők, mindaddig, amíg biztonsági mentése és visszaállítása a tartományvezérlők vonatkozó irányelvek követi. Megkötések és iránymutatásokat futtató virtualizált tartományvezérlők kapcsolatos további információkért lásd: [tartományvezérlők futtatása Hyper-v](https://technet.microsoft.com/library/dd363553).

Hipervizorok nyújtanak, vagy trivialize, amely sok elosztott rendszerek, beleértve a Windows Server Active Directory problémákat okozhat. Például egy fizikai kiszolgálón klónozni egy lemezt, vagy nem támogatott módszerek tooroll hátsó hello állapot egy kiszolgáló, beleértve a San-ok használatával és így tovább használni de történt, amely a fizikai kiszolgálón sokkal nehezebb, mint a hipervizor a virtuális gép pillanatkép visszaállítása. Az Azure eredményező hello azonos funkciót kínál nemkívánatos feltétel. Például nem másolja a tartományvezérlőket VHD-fájlok helyett rendszeres biztonsági mentést végez, mivel visszaállítás helyzet toousing pillanatkép visszaállítása hasonló szolgáltatások.

Ilyen visszagörgetése bevezetni a frissítési sorszámok buborékjairól, hogy azok a tartományvezérlők közötti toopermanently eltérő állapotok okozhat. Problémák, mint okozhat, amelyek:

* Fennmaradó objektumokról
* Inkonzisztens jelszavak
* Inkonzisztens attribútumértékek
* Séma nem egyezik meg, ha a séma-főkiszolgáló hello vissza lesz állítva.

Hogyan tartományvezérlők érintett kapcsolatos további információkért lásd: [USN and USN visszaállítása](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx#usn_and_usn_rollback).

Windows Server 2012 rendszertől kezdődően [további védelmet beépített tooAD DS](https://technet.microsoft.com/library/hh831734.aspx). Ezek a funkciók védelme a virtualizált tartományvezérlők hello, fent említett problémák elleni mindaddig, amíg az alapul szolgáló platform hello a hipervizor támogatja a virtuális gép Generációazonosítóját. Azure virtuális gép Generációazonosítóját, ami azt jelenti, hogy a Windows Server 2012 vagy újabb Azure virtuális gépek futtató tartományvezérlők hello további védelmet támogatja.

> [!NOTE]
> Kell állítsa le és indítsa újra a virtuális gép futó hello tartományvezérlői szerepkör Azure hello vendég operációs rendszerben hello használata helyett **Leállítás** hello Azure porta beállítást. Napjainkban hello portál tooshut le a virtuális gépek használata azt eredményezi, hello VM toobe felszabadítása. Felszabadított virtuális gép nem díjfizetési hello előnye van, de is alaphelyzetbe állítja hello virtuális gép Generációazonosítóját, amely nem kívánatos egy tartományvezérlő. Amikor hello virtuális gép Generációazonosítóját, hello invocationID hello AD DS-adatbázis is alaphelyzetbe áll, hello RID-verem a program elveti és SYSVOL nem mérvadó van megjelölve. További információkért lásd: [bemutatása tooActive Directory tartományi szolgáltatások (AD DS) virtualizálása](https://technet.microsoft.com/library/hh831734.aspx) és [DFSR biztonságos virtualizálása](http://blogs.technet.com/b/filecab/archive/2013/04/05/safely-virtualizing-dfsr.aspx).
> 
> 

## <a name="why-deploy-windows-server-ad-ds-on-azure-virtual-machines"></a>Miért központi telepítése Windows Server AD DS Azure virtuális gépeken?
Windows Server Active Directory tartományi szolgáltatások számos telepítési forgatókönyv a központi telepítésben lévő Azure virtuális gépek is jól alkalmazható. Tegyük fel például egy vállalat, amelyet egy távoli helyen Ázsiában tooauthenticate felhasználók Európában. hello vállalati nem korábban már telepítve van a Windows Server Active Directory tartományvezérlők Ázsiában lejáró költségek toohello toodeploy őket, és korlátozott szakértői toomanage hello kiszolgáló telepítés utáni. Ennek eredményeképpen Ázsia érkező hitelesítési kérések által kiszolgált tartományvezérlők, amelyek az Európai optimálisnál eredményekkel. Ebben az esetben is telepíthet a tartományvezérlő a virtuális gép által megadott hello Ázsiában Azure datacenter belül kell futtatni. A tartományvezérlő tooan toohello távoli helyen lesz növelheti a hitelesítés teljesítményét közvetlenül csatlakoztatott Azure virtuális hálózat csatolása.

Azure a is jól alkalmazható, mint a helyettesítő toootherwise költséges vész-helyreállítási helyeken. hello viszonylag alacsony költségű a tartományvezérlők és az Azure-on egyetlen virtuális hálózat kis számú tárolására vonzó alternatív jelöli.

Végül érdemes lehet egy hálózati alkalmazás, például a SharePoint, a Windows Server Active Directory szükséges, de nem függőségi rendelkezzen a helyszíni hálózati vagy a vállalati Windows Server Active Directory hello hello Azure toodeploy. Ebben az esetben a SharePoint Azure toomeet hello egy elkülönített erdők üzembe helyezése kiszolgáló követelményei az optimális. Hálózati kapcsolat toohello a helyszíni hálózati és a vállalati Active Directory hello igénylő alkalmazások telepítése ebben az esetben is támogatott.

> [!NOTE]
> 3. rétegbeli kapcsolatot biztosít, mivel hello VPN összetevője, amely egy Azure virtuális hálózatra és egy helyszíni hálózat közötti kapcsolat is engedélyezheti a helyszíni tooleverage futtató tartományvezérlők Azure virtuális gépek Azure-on futó kiszolgálókon virtuális hálózat. De hello VPN nem érhető el, ha közötti kommunikáció a helyszíni számítógépek és az Azure-alapú tartományvezérlők nem fog működni, hitelesítési és egyéb különféle hibák.  
> 
> 

## <a name="contrasts-between-deploying-windows-server-active-directory-domain-controllers-on-azure-virtual-machines-versus-on-premises"></a>Windows Server Active Directory tartományvezérlők Azure virtuális gépeken és a helyszíni üzembe helyezésének közötti nézete
* Bármely, amely tartalmazza az több mint egy virtuális Windows Server Active Directory környezet esetén szükséges toouse Azure virtuális hálózat az IP-cím konzisztenciáját. Vegye figyelembe, hogy ez az útmutató feltételezi, hogy a tartományvezérlők futnak-e az Azure virtuális hálózaton.
* Csakúgy, mint a helyszíni tartományvezérlők, statikus IP-címek használata ajánlott. Egy statikus IP-cím csak Azure PowerShell használatával konfigurálható. Lásd: [statikus belső IP-cím, virtuális gépek](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/) további részleteket. Ha figyelő rendszerek vagy egyéb megoldások, amelyek statikus IP-címének konfigurációja hello vendég operációs rendszerben keressen, rendelhet hello azonos statikus IP cím toohello hálózati adapter tulajdonságainak hello virtuális gép. De vegye figyelembe, hogy hello hálózati adapter elvesznek, ha hello VM megy keresztül szolgáltatásjavításnak vagy hello portálon le van állítva, és rendelkezik a cím felszabadítása. Ebben az esetben hello statikus IP-cím hello vendégen toobe kell alaphelyzetbe állítása.
* Virtuális gépek telepítése a virtuális hálózaton nem utalnak (vagy igényelnek) hátsó tooan a helyszíni hálózati kapcsolódási; hello virtuális hálózati csupán engedélyezi ezt a lehetőséget. Azure és a helyszíni hálózat között titkos kommunikációt egy virtuális hálózatot kell létrehoznia. Toodeploy hello a helyi hálózaton a VPN-végpontnak kell. hello VPN Azure toohello helyszíni hálózatról van megnyitva. További információkért lásd: [virtuális hálózat áttekintése](../virtual-network/virtual-networks-overview.md) és [a telephelyek közötti VPN konfigurálása az Azure portál hello](../vpn-gateway/vpn-gateway-site-to-site-create.md).

> [!NOTE]
> Egy beállítás túl[hozzon létre egy pont – hely VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) van elérhető tooconnect egyes Windows-alapú számítógépek közvetlenül tooan Azure-beli virtuális hálózat.
> 
> 

* Függetlenül attól, hogy akkor hozzon létre egy virtuális hálózati vagy nem, a kimenő forgalom, de nem érkező Azure percalapú. Különböző Windows Server Active Directory tervezési döntések ütköznek azokkal hatással lehet a kimenő forgalom hozza létre a központi telepítés. Például egy írásvédett tartományvezérlő telepítéséhez (RODC) korlátozza a kilépő forgalmat, mert nem replikálja kimenő. De hello döntési toodeploy írásvédett tartományvezérlő toobe hello kell tooperform mérlegelni kell írási műveletek elleni hello DC és hello [kompatibilitási](https://technet.microsoft.com/library/cc755190) , alkalmazások és szolgáltatások hello hely rendelkezik-e az RODC-k. Forgalom költségekkel kapcsolatos további információkért lásd: [Azure díjszabása:-a-áttekintő](http://azure.microsoft.com/pricing/).
* Teljes felügyeletet gyakorolhat milyen server erőforrások toouse virtuális gépek a helyszíni, például a memória, lemez méretét, és egyéb, miközben az Azure ki kell választania egy előre konfigurált kiszolgáló méretének megtekintéséhez. Egy tartományvezérlő adatlemez hozzáadása toohello operációsrendszer-lemez rendelés toostore hello Windows Server Active Directory-adatbázisban van szükség.

## <a name="can-you-deploy-windows-server-ad-fs-on-azure-virtual-machines"></a>Telepítheti a Windows Server AD FS az Azure virtuális gépeken?
Igen, a Windows Server AD FS az Azure virtuális gépeken telepítheti, és hello [ajánlott eljárások az AD FS üzembe](https://technet.microsoft.com/library/dn151324.aspx) helyszíni egyaránt alkalmazni tooAD FS deployment az Azure-on. Azonban néhány gyakorlati tanácsok hello többek között az terheléselosztás és magas rendelkezésre állású túl Mi az AD FS kínál maga technológiákra szüksége. Hello alapul szolgáló infrastruktúra kell megadni. Most tekintse át azokat az ajánlott eljárásokat, és tekintse meg, hogyan azok elérhető Azure virtuális gépek és az Azure virtuális hálózat használatával.

1. **Soha nem fedheti fel a biztonsági jogkivonat-szolgáltatás (STS) kiszolgálók közvetlenül toohello Internet.**
   
    Ez azért fontos, mert hello STS biztonsági jogkivonatokat bocsát ki. Ennek eredményeképpen STS-kiszolgálók, mint például az AD FS-kiszolgáló kell kezelni hello azonos szintű védelmet tartományvezérlőként. Ha biztonságijogkivonat-szolgáltatás biztonsága sérül, a rosszindulatú felhasználók kell hello képességét tooissue hozzáférési jogkivonatok potenciálisan tartalmazó jogcímeket azok választhatja toorelying alkalmazásait és a megbízó szervezetek STS-kiszolgálóival.
2. **A tartományvezérlőket az Active Directory azonos hálózati hello AD FS-kiszolgálóként hello lévő összes felhasználói tartományban.**
   
    AD FS-kiszolgálók Active Directory tartományi szolgáltatások tooauthenticate felhasználók használja. Ajánlott toodeploy tartományvezérlők hello azonos hálózati hello AD FS-kiszolgálóként. Ez biztosítja az üzletmenet folytonosságát abban az esetben közötti kapcsolat hello hello Azure-hálózatot, és a helyszíni hálózat sérült, és kisebb késést biztosít és bejelentkezések jobb teljesítményét.
3. **Telepítse a magas rendelkezésre állású és hello terheléselosztás több AD FS-csomópont.**
   
    A legtöbb esetben hello sikertelen volt-e olyan alkalmazás, amely lehetővé teszi, hogy az AD FS nem fogadható el, mert a biztonsági jogkivonatok gyakran igénylő alkalmazások hello akkreditált képviseletének kritikus. Ennek eredményeképpen és az AD FS most hello kritikus út tooaccessing alapvető fontosságú alkalmazások helyezkedik el, hello AD FS szolgáltatás használatával több AD FS-proxyk és AD FS-kiszolgáló magas rendelkezésre állású kell lennie. a kérelmek, a terheléselosztók tooachieve terjesztési általában elé hello AD FS proxy és a hello AD FS-kiszolgálók vannak telepítve.
4. **Telepítse az internet-hozzáférés egy vagy több webalkalmazás-Proxy-csomópontokat.**
   
    Hello AD FS szolgáltatás által védett tooaccess alkalmazások a felhasználóknak kell, amikor a hello AD FS szolgáltatási igények toobe érhetők el az internet hello. Ez hello webalkalmazás-Proxy szolgáltatás telepítésével érhető el. Erősen ajánlott toodeploy több mint egy csomópont a magas rendelkezésre állás és a terheléselosztás hello alkalmazásában.
5. **Hello webalkalmazás-Proxy csomópontok toointernal hálózati erőforrásokhoz való hozzáférés korlátozása.**
   
    tooallow külső felhasználók tooaccess AD FS a hello internet, toodeploy webalkalmazás-Proxy csomópontok (vagy AD FS Proxy Windows Server korábbi verzióiban) szükséges. Webalkalmazás-proxy csomópontra van közvetlenül hello toohello Internet kitéve. Nem szükséges toobe tartományhoz, és csak kell hozzáférésük toohello AD FS-kiszolgálók 443-as és a 80-as TCP-porton. Erősen ajánlott, hogy kommunikációs tooall más számítógépeken (különösen tartományvezérlőkön) le van tiltva.
   
    Ez az általában elért helyszíni DMZ protokollt. Tűzfalak használja egy engedélyezési lista módját hello DMZ toohello a helyszíni hálózati forgalmát művelet toorestrict, (Ez azt jelenti, hogy csak a hello forgalmát megadott IP-címek és over megadott portok engedélyezett, és az összes többi forgalom le van tiltva).

hello következő diagramon láthatók a hagyományos helyszíni AD FS üzembe helyezése.

![A hagyományos a helyszíni Active Directory összevonási szolgáltatások telepítési ábrája](media/active-directory-deploying-ws-ad-guidelines/ADFS_onprem.png)

Azonban Azure nem biztosít natív, teljes körű tűzfal funkció, mert más beállítások használt toobe toorestrict forgalmat kell. hello következő táblázatban minden egyes beállítás, és annak előnyeit és hátrányait.

| Beállítás | Előnye | Hátránya |
| --- | --- | --- |
| [Az Azure hálózati hozzáférés-vezérlési listák](../virtual-network/virtual-networks-acl.md) |Kevésbé költséges és egyszerűbb kezdeti konfigurálása |További hálózati hozzáférés-vezérlési lista konfigurációra szükség, ha bármely új virtuális gépek hozzáadásakor toohello központi telepítés |
| [Barracuda NG tűzfal](https://www.barracuda.com/products/ngfirewall) |Engedélyezési művelet, és azt módhoz nincs hálózati hozzáférés-vezérlési lista konfigurációja |Nagyobb költségek és a kezdeti telepítés |

hello magas szintű lépései toodeploy AD FS ebben az esetben a következők:

1. Virtuális hálózat létrehozása közötti kapcsolatot nyújthassanak, segítségével a VPN-en vagy [ExpressRoute](http://azure.microsoft.com/services/expressroute/).
2. A tartományvezérlőket hello virtuális hálózaton. Ez a lépés nem kötelező, de ajánlott.
3. Telepítse az AD FS-kiszolgálók tartományhoz hello virtuális hálózaton.
4. Hozzon létre egy [belső elosztott terhelésű készlet](http://azure.microsoft.com/blog/internal-load-balancing/) , amely magában foglalja a hello AD FS-kiszolgálók és hello virtuális hálózaton (egy dinamikus IP-cím) belüli új privát IP-címet használja.
   
   1. DNS toocreate hello FQDN toopoint toohello titkos (dinamikus) IP-címének frissítése hello belső elosztott terhelésű készlet.
5. Hozzon létre egy felhőalapú szolgáltatás (vagy egy különálló virtuális hálózati) hello webalkalmazás-Proxy csomópontok.
6. Hello webalkalmazás-Proxy csomópontok hello felhőalapú szolgáltatás, vagy a virtuális hálózati telepítése
   
   1. Hozzon létre egy külső elosztott terhelésű készlet, amely hello webalkalmazás-Proxy csomópontot tartalmaz.
   2. Hello külső DNS neve (FQDN) toopoint toohello felhőalapú szolgáltatás nyilvános IP-címének frissítése (hello virtuális IP-cím).
   3. Konfigurálja az AD FS proxy toouse hello toohello belső elosztott terhelésű készlet hello AD FS-kiszolgáló a megfelelő teljes Tartománynevet.
   4. Frissítse a jogcím-alapú webhelyek toouse hello külső FQDN a jogcím-szolgáltató.
7. Korlátozhatja a hozzáférést a webalkalmazás-Proxy tooany gép hello AD FS virtuális hálózat között.

toorestrict forgalom hello elosztott terhelésű készlet hello Azure belső terheléselosztóhoz kell toobe csak forgalom tooTCP porthoz 80-as és 443-as konfigurálva, és az összes többi forgalom toohello belső dinamikus IP-címe hello elosztott terhelésű készlet megszakad.

![Az AD FS hálózati hozzáférés-vezérlési listák 80-as TCP 443-as kiegészítve ábrája engedélyezett.](media/active-directory-deploying-ws-ad-guidelines/ADFS_ACLs.png)

Csak a következő források hello által megengedett lenne forgalom toohello AD FS-kiszolgálók:

* hello Azure belső terheléselosztót.
* IP-cím hello hello a helyszíni hálózat rendszergazdájának.

> [!WARNING]
> hello tervezési akadályozza meg a webalkalmazás-Proxy csomópontok elérni az összes többi virtuális gépe hello Azure-beli virtuális hálózat vagy hello a helyi hálózaton lévő összes helyet. Amely végezhető el a tűzfalszabályok beállítása a hello helyszíni készülék Express Route-kapcsolatok vagy hello VPN-eszköz pont-pont VPN-kapcsolatokhoz.
> 
> 

A hátránya toothis beállítás akkor hello kell tooconfigure hello hálózati hozzáférés-vezérlési listák több eszközöket, beleértve a belső terheléselosztó hello AD FS-kiszolgálók és hozzáadják a virtuális hálózati toohello más kiszolgálók számára. Ha bármely hozzáadott eszköz toohello telepítési hálózati hozzáférés-vezérlési listák toorestrict forgalom tooit beállítása nélkül hello teljes telepítést is lehetnek kitéve. Ha valaha is megváltozik hello webalkalmazás-Proxy csomópontok hello IP-címe, a hello hálózati hozzáférés-vezérlési listák alaphelyzetbe kell állítani (ami azt jelenti, hogy hello proxyk konfigurált toouse kell [statikus dinamikus IP-címek](http://azure.microsoft.com/blog/static-internal-ip-address-for-virtual-machines/)).

![Az AD FS Azure hálózati hozzáférés-vezérlési LISTÁK.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure.png)

Másik lehetőség is toouse hello [Barracuda NG tűzfal](https://www.barracuda.com/products/ngfirewall) készülék toocontrol forgalom és az AD FS proxy hello AD FS kiszolgálók között. Ez a beállítás megfelel az ajánlott eljárások és a magas rendelkezésre állású, és kevesebb felügyeleti hello kezdeti telepítés után igényel, mert hello Barracuda NG tűzfal készülék tűzfal felügyeleti engedélyezett üzemmódot biztosít, és telepítheti közvetlenül az Azure virtuális hálózat. Az új kiszolgáló hozzáadása toohello telepítési bármikor, amely kiküszöböli hello kell tooconfigure hálózati hozzáférés-vezérlési listák. Azonban ez a beállítás a kezdeti telepítés egyszerűbb és olcsóbb hozzáadja.

Ebben az esetben két virtuális hálózatok helyett egy vannak telepítve. Felhívjuk őket VNet1 és VNet2. VNet1 hello proxykat tartalmaz, és VNet2 hello STSs és hello hálózati kapcsolat hátsó toohello vállalati hálózat tartalmazza. Fizikailag (ámbár gyakorlatilag) VNet1 van ezért egymástól el vannak különítve a VNet2 és viszont hello vállalati hálózatról. VNet1 majd csatlakoztatott tooVNet2 használ egy különleges bújtatási technológia, átviteli független hálózati architektúra (TINA) néven ismert. hello TINA alagút csatolt tooeach hello virtuális hálózatok használatával Barracuda NG tűzfal – egy Barracuda egyes hello virtuális hálózatok.  A magas rendelkezésre állású javasoljuk, hogy minden virtuális hálózaton; két kígyókról központi telepítése egy aktív hello más passzív. Rendkívül hatékony firewalling képességeket, amelyek lehetővé teszik a számunkra a hagyományos helyszínen DMZ toomimic hello működését az Azure-ban kínálnak.

![Az AD FS tűzfal Azure-on.](media/active-directory-deploying-ws-ad-guidelines/ADFS_Azure_firewall.png)

További információkért lásd: [AD FS: kiterjesztése a helyszíni jogcímbarát előtér-alkalmazás toohello Internet](#BKMK_CloudOnlyFed).

### <a name="an-alternative-tooad-fs-deployment-if-hello-goal-is-office-365-sso-alone"></a>Az alternatív tooAD FS központi telepítése, ha hello, Office 365 egyszeri bejelentkezés önmagában
Ha a cél csak tooenable bejelentkezés az Office 365, nincs egy másik alternatív toodeploying AD FS elemet. Ebben az esetben egyszerűen telepítheti a DirSync a jelszó szinkronizálása a helyszíni és elérése hello azonos végeredménynek az minimális központi telepítés bonyolultságát, mivel ez a módszer nem szükséges az AD FS- vagy Azure.

hello alábbi táblázat összehasonlítja hello bejelentkezési folyamatok működése, és az AD FS telepítése nélkül.

| Az Office 365 egyszeri bejelentkezés az AD FS és a DirSync használatával | Office 365 azonos bejelentkezés DirSync + jelszó-szinkronizálás használatával |
| --- | --- |
| 1. hello felhasználó bejelentkezik a vállalati hálózat tooa és hitelesített tooWindows Server Active Directory. |1. hello felhasználó bejelentkezik a vállalati hálózat tooa és hitelesített tooWindows Server Active Directory. |
| 2. hello felhasználó megpróbál az Office 365 tooaccess (vagyok @contoso.com). |2. hello felhasználó megpróbál az Office 365 tooaccess (vagyok @contoso.com). |
| 3. Az Office 365 hello felhasználói tooAzure AD irányítja át. |3. Az Office 365 hello felhasználói tooAzure AD irányítja át. |
| 4. Mivel az Azure AD hello felhasználói nem tudják hitelesíteni magukat, és megértette, hogy van-e a helyszíni Active Directory összevonási szolgáltatások megbízhatóság, hello felhasználói tooAD FS irányítja át. |4. Az Azure AD közvetlenül a Kerberos jegyek nem fogad, és nincs megbízhatósági kapcsolat létezik, így mindent lekér hello felhasználó adja meg a hitelesítő adatokat. |
| 5. hello felhasználói küld egy Kerberos jegy toohello AD FS STS. |5. hello felhasználó ad hello megegyezik a helyszíni jelszót, és az Azure AD hello felhasználónévvel és jelszóval DirSync által szinkronizált érvényesíti azokat. |
| 6. AD FS átalakítja hello Kerberos jegy toohello token formátuma/jogcímeket, és átirányítja a felhasználókat hello felhasználói tooAzure AD szükséges. |6. Az Azure AD hello felhasználói tooOffice 365 irányítja át. |
| 7. hello felhasználó hitelesíti tooAzure AD (egy másik átalakítása következik be). |7. hello felhasználói bejelentkezés tooOffice 365 és az OWA hello Azure AD-token használatával. |
| 8. Az Azure AD hello felhasználói tooOffice 365 irányítja át. | |
| 9. hello felhasználói beavatkozás nélkül bejelentkezve tooOffice 365. | |

A hello Office 365 a DirSync a jelszó-szinkronizálási forgatókönyv (nem az AD FS) az egyszeri bejelentkezés helyébe "azonos bejelentkezés" hol "azonos" egyszerűen azt jelenti, hogy felhasználók újra meg kell adnia a helyszíni hitelesítő adatokkal való hozzáférés az Office 365 során. Vegye figyelembe, hogy ezeket az adatokat a böngésző toohelp hello felhasználói által megjegyzett lehet csökkenteni a további utasításokat.

### <a name="additional-food-for-thought"></a>További étele for gondolat
* Ha az AD FS proxy Azure virtuális géphez, kapcsolat toohello AD FS-kiszolgáló szükséges. Ha a helyszíni, javasoljuk, hogy használja-e az AD FS kiszolgálón hello virtuális hálózati tooallow hello webalkalmazás-Proxy csomópontok toocommunicate által biztosított hello pont-pont VPN-kapcsolatot.
* Ha telepít egy AD FS-kiszolgáló egy Azure virtuális számítógép, kapcsolat tooWindows kiszolgáló Active Directory tartományi vezérlőket, Attribútumtárak, és konfigurációs adatbázis szükséges, és után is szükség lehet egy expressroute-on vagy VPN-kapcsolat pont-pont közötti hello Azure virtuális hálózat és hello a helyszíni hálózat.
* Díjak érvényesek az Azure virtuális gépek (kimenő forgalom) tooall forgalmát. Ha költség hello vezetői tényező, ajánlott toodeploy hello webalkalmazás-Proxy csomópontok Azure hello AD FS kiszolgálók helyszíni változatlanul is. Ha hello AD FS-kiszolgálók az Azure virtuális gépeken is van telepítve, a további költségek felmerült tooauthenticate a helyi felhasználók lesz. Kimenő forgalom költséget egy áll-e a rendszer áthaladó hello ExpressRoute vagy hello VPN webhelyek közötti kapcsolattal függetlenül.
* Ha úgy dönt, hogy toouse Azure natív-kiszolgáló terheléselosztási lehetőségeket biztosít az AD FS-kiszolgáló magas rendelkezésre állású, vegye figyelembe, hogy mintavételek menüpontban, amelyek használt toodetermine hello állapotát hello virtuális gépeit hello felhőalapú szolgáltatás terheléselosztást biztosít. Az Azure virtuális gépek (a megakadályozását tooweb vagy feldolgozói szerepkörök) hello esetben egy egyéni mintavételt kell használható, mert hello ügynök, amely válaszol a toohello alapértelmezett mintavételt nincs jelen az Azure virtuális gépeken. Az egyszerűség kedvéért egy egyéni TCP-Hálózatfigyelővel használhatja – ehhez csak, hogy a TCP-kapcsolat (TCP SZIN szegmens küldött és toowith TCP SZIN ACK-szegmens válaszolt) kell-e sikeresen létrehozott toodetermine virtuális gép állapotát. Egyéni tesztműveleti toouse hello bármely TCP-portot a virtuális gépek aktívan toowhich konfigurálhatja.

> [!NOTE]
> Gépek, amelyek azonos beállítva portok, közvetlen Internet toohello (például a 80-as és 443-as port) nem lehet megosztani tooexpose hello kell hello ugyanazt a felhőalapú szolgáltatás. Ezért javasoljuk, hogy a rendelés tooavoid lehetséges a Windows Server AD FS kiszolgálók átfedésben van egy alkalmazás a portokra vonatkozó követelmények és a Windows Server AD FS közötti dedikált felhőalapú szolgáltatás létrehozása.
> 
> 

## <a name="deployment-scenarios"></a>Üzembe helyezési forgatókönyvek
a következő szakasz hello alkotómunkájának központi telepítési forgatókönyvek toodraw figyelmet tooimportant kapcsolatos szempontokról olvashat, amelyek figyelembe kell venni. Minden egyes forgatókönyv hivatkozások toomore hello döntést és részleteit tényezők tooconsider rendelkezik.

1. [Active Directory tartományi szolgáltatások: Az AD DS-t támogató alkalmazás nem követelmény a vállalati hálózati kapcsolatra központi telepítését.](#BKMK_CloudOnly)
   
    Például egy internetes elérésű SharePoint-szolgáltatás telepítve van egy Azure virtuális gépen. hello nincs függőségben van a vállalati hálózati erőforrásokat. hello alkalmazás szükséges a Windows Server Active Directory tartományi szolgáltatások, de nem igényel hello vállalati Windows Server Active Directory tartományi Szolgáltatásokban.
2. [Az AD FS: Kiterjesztése a helyszíni jogcímbarát előtér-alkalmazás toohello Internet](#BKMK_CloudOnlyFed)
   
    Például egy megjelölt jogcímbarát alkalmazáshoz, amelyeket már sikeresen telepítve a helyi vállalati felhasználók által használt kell toobecome hello Internet érhető el. hello alkalmazásnak kell közvetlenül hello interneten keresztül elérhető vállalati identitásokat a saját üzleti partnerek és a meglévő vállalati felhasználók toobe.
3. [Active Directory tartományi szolgáltatások: A Windows Server AD DS-t támogató toohello vállalati hálózati kapcsolatot igénylő alkalmazás központi telepítése](#BKMK_HybridExt)
   
    Például egy LDAP-kompatibilis alkalmazás, amely támogatja a Windows-hitelesítést, és a Windows Server Active Directory tartományi szolgáltatások használja, mint a tárház konfigurációs és a felhasználói profil adatainak telepítve van egy Azure virtuális gépen. Hello alkalmazás tooleverage hello meglévő vállalati Windows Server Active Directory tartományi szolgáltatások esetében fontos, és adja meg az egyszeri bejelentkezés. hello alkalmazás nincs jogcímbarát.

### <a name="BKMK_CloudOnly"></a>1. Active Directory tartományi szolgáltatások: Az AD DS-t támogató alkalmazás nem követelmény a vállalati hálózati kapcsolatra központi telepítését.
![Csak felhőalapú Active Directory tartományi szolgáltatások telepítési](media/active-directory-deploying-ws-ad-guidelines/ADDS_cloud.png)
**1. ábra**

#### <a name="description"></a>Leírás
A SharePoint Azure virtuális gépként van telepítve, és hello nincs függőségben van a vállalati hálózati erőforrásokat. hello alkalmazás szükséges azonban a Windows Server AD DS *nem* szükséges hello vállalati Windows Server Active Directory tartományi Szolgáltatásokban. Nincs Kerberos vagy összevont Megbízhatóságok szükség, mivel a felhasználók saját maguk is hello felhőben Azure virtuális gépeken futtatott Windows Server Active Directory tartományi szolgáltatások hello tartományába hello alkalmazáson keresztül kiosztott.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>A forgatókönyv szempontjait és hogyan identitástechnológiai területein alkalmazása toohello forgatókönyv
* [Hálózati topológia](#BKMK_NetworkTopology): hozzon létre egy Azure virtuális hálózatra létesítmények közötti kapcsolathoz (más néven hely-hely kapcsolatot) nélkül.
* [Tartományvezérlő telepítési konfiguráció](#BKMK_DeploymentConfig): üzembe helyezhet egy új tartományvezérlőt egy új, egyetlen tartományból, Windows Server Active Directory-erdőben. Ezzel együtt hello Windows DNS-kiszolgálót kell telepíteni.
* [Windows Server Active Directory-helyek topológiájával](#BKMK_ADSiteTopology): használata hello alapértelmezett Windows Server Active Directory-helyhez (összes számítógép lesz alapértelmezett-First-Site-Name).
* [IP-címzést és a DNS-](#BKMK_IPAddressDNS):
  
  * Állítsa be egy statikus IP-címet hello DC hello Set-AzureStaticVNetIP Azure PowerShell-parancsmag használatával.
  * Telepítse és konfigurálja a Windows Server DNS az Azure x hello tartományvezérlőn zajlik.
  * Hello virtuális hálózati tulajdonságok konfigurálása hello nevű és hello hello tartományvezérlő és DNS-kiszolgáló szerepkört üzemeltető virtuális gép IP-címét.
* [Globális katalógus](#BKMK_GC): hello hello erdő első tartományvezérlő globáliskatalógus-kiszolgálónak kell lennie. További tartományvezérlők is konfigurálnia kell juthatnak ebbe a generációba, mert egy egyetlen tartományból álló hello globális katalógus nem szükséges a hello DC bármit tennie kellene.
* [Hello Windows Server AD DS-adatbázis és a SYSVOL helyét](#BKMK_PlaceDB): hozzáadása egy futtató Azure virtuális gépeken, sorrendben toostore hello Windows Server Active Directory-adatbázis, a naplókat, és a SYSVOL adatok lemez tooDCs.
* [Biztonsági mentése és visszaállítása](#BKMK_BUR): határozza meg, ahol toostore rendszerállapot biztonsági mentéseit. Ha szükséges, adjon hozzá egy másik az adatok lemezre toohello DC VM toostore biztonsági.

### <a name="BKMK_CloudOnlyFed"></a>2 AD FS: kiterjesztése a helyszíni jogcímbarát előtér-alkalmazás toohello Internet
![Összevonás az közötti kapcsolatot nyújthassanak](media/active-directory-deploying-ws-ad-guidelines/Federation_xprem.png)
**2. ábra**

#### <a name="description"></a>Leírás
Jogcímbarát alkalmazáshoz, amelyeket már sikeresen telepítve a helyi vállalati felhasználók igényeinek toobecome érhető el közvetlenül a hello Internet használják. hello alkalmazás lesz egy webes előtér tooa SQL adatbázis pedig tárolja az adatokat. hello hello alkalmazás által használt SQL-kiszolgálók is hello a vállalati hálózaton található. Két Windows Server AD FS STSs és a terheléselosztó voltak telepített helyszíni tooprovide hozzáférés toohello vállalati felhasználók. hello most alkalmazásnak továbbá elért hello interneten keresztül közvetlenül a saját vállalati identitásokat üzleti partnerek és a meglévő vállalati felhasználók toobe.

Az elérhető toosimplify és igazodhat hello üzembe helyezési és konfigurálási igényeinek ezt a követelményt, az a döntés született, hogy két további webalkalmazás-frontends és két Windows Server AD FS proxykiszolgálókra telepíthető Azure virtuális gépeken. Négy virtuális gépeinek lesznek közzétéve közvetlenül toohello Internet, és biztosítja a kapcsolatot toohello a helyszíni hálózat az Azure Virtual Network pont-pont VPN funkcióval.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>A forgatókönyv szempontjait és hogyan identitástechnológiai területein alkalmazása toohello forgatókönyv
* [Hálózati topológia](#BKMK_NetworkTopology): hozzon létre egy Azure virtuális hálózatra és [közötti kapcsolatot nyújthassanak konfigurálása](../vpn-gateway/vpn-gateway-site-to-site-create.md).
  
  > [!NOTE]
  > Minden egyes hello Windows Server AD FS-tanúsítványok esetében győződjön meg arról, hogy hello URL-cím meghatározott hello tanúsítványsablon hello létrehozott tanúsítványok elérhető Azure-on futó Windows Server AD FS hello példányával. Ehhez a nyilvános kulcsokra ÉPÜLŐ infrastruktúra a létesítmények közötti kapcsolat tooparts lehet szükség. Például ha hello CRL-végpontot az helyszíni LDAP-alapú és kizárólag üzemeltetett, majd közötti kapcsolatot nyújthassanak lesz szükség. Ha ez nem kívánatos, amelynek a CRL nem érhető el hello interneten keresztül egy hitelesítésszolgáltató által kiállított szükséges toouse tanúsítványok lehet.
  > 
  > 
* [Felhő konfigurálása](#BKMK_CloudSvcConfig): Ellenőrizze, hogy a két felhőszolgáltatások sorrendben meg két terhelésű virtuális IP-címeket. hello első felhőalapú szolgáltatás virtuális IP-cím lesz irányított toohello két Windows Server AD FS proxy virtuális gépek a 80-as és 443-as portot. hello Windows Server AD FS proxy virtuális gépek lesz konfigurálva hello helyszíni terheléselosztót, hogy előlapot hello Windows Server AD FS STSs toopoint toohello IP-címét. hello második felhőalapú szolgáltatás virtuális IP-cím lesz irányított toohello két virtuális gépek hello webes előtér újra futtatni a 80-as és 443-as portot. Egy egyéni mintavétel tooensure hello terheléselosztó csak irányítja a forgalmat toofunctioning Windows Server AD FS proxy- és webes előtér virtuális gépeket.
* [Összevonási kiszolgáló konfigurációja](#BKMK_FedSrvConfig): Windows Server konfigurálása az AD FS összevonási kiszolgáló (STS) toogenerate biztonsági jogkivonatokat hello Windows Server Active Directory-erdő hello felhő létrehozása. Összevonási jogcímeket szolgáltató megbízhatósági kapcsolatban szereplő identitásokat tooaccept kívánja hello különböző partnerekkel, és azt szeretné, hogy toogenerate-jogkivonat hello különböző alkalmazásokkal függő entitás bizalmi kapcsolatok konfigurálása.
  
    A legtöbb esetben Windows Server AD FS proxykiszolgálókra telepített biztonsági okokból az Internet felé néző minőségben közben, mint a Windows Server AD FS összevonási elkülönül közvetlen internetkapcsolattal. Az üzembe helyezési forgatókönyvnek függetlenül konfigurálnia kell az a felhőalapú szolgáltatás, amely egy nyilvánosan elérhető IP-címet és portot, amelyet nem tud tooload-egyenleg két Windows Server AD FS STS-példányok vagy a proxy-példányok virtuális IP-címmel.
* [Windows Server AD FS magas rendelkezésre állású konfiguráció](#BKMK_ADFSHighAvail): ajánlott toodeploy legalább két kiszolgáló a feladatátvétel és terheléselosztás a Windows Server AD FS farm. Például, szeretné, hogy tooconsider hello belső Windows adatbázist (WID) használja a Windows Server AD FS konfiguráció adatait, és az hello belső terheléselosztási funkció Azure toodistribute bejövő kérelmek hello farm hello kiszolgálóján.

További információkért lásd: hello [Active Directory tartományi szolgáltatások telepítési útmutatója](https://technet.microsoft.com/library/cc753963).

### <a name="BKMK_HybridExt"></a>3. Active Directory tartományi szolgáltatások: A Windows Server AD DS-t támogató toohello vállalati hálózati kapcsolatot igénylő alkalmazás központi telepítése
![Active Directory tartományi szolgáltatások telepítési létesítmények közötti](media/active-directory-deploying-ws-ad-guidelines/ADDS_xprem.png)
**3. ábra**

#### <a name="description"></a>Leírás
Egy LDAP-kompatibilis alkalmazás lett telepítve egy Azure virtuális gépen. Windows-hitelesítést támogatja, és használja a tárház Windows Server Active Directory tartományi szolgáltatások konfigurációs és a felhasználói profil adatok. hello cél hello alkalmazás tooleverage hello meglévő vállalati Windows Server Active Directory tartományi Szolgáltatásokban, és adja meg az egyszeri bejelentkezés. hello alkalmazás nincs jogcímbarát. Felhasználók is kell tooaccess hello alkalmazás hello Internet-ről. toooptimize teljesítményének és költséghatékonyságának, az a döntés született, hogy két további tartományvezérlők hello vállalati tartomány részét képezik hello alkalmazás Azure-ral telepíthető.

#### <a name="scenario-considerations-and-how-technology-areas-apply-toohello-scenario"></a>A forgatókönyv szempontjait és hogyan identitástechnológiai területein alkalmazása toohello forgatókönyv
* [Hálózati topológia](#BKMK_NetworkTopology): az Azure virtuális hálózat létrehozása [létesítmények közötti kapcsolat](../vpn-gateway/vpn-gateway-site-to-site-create.md).
* [Telepítési módszer](#BKMK_InstallMethod): hello vállalati Windows Server Active Directory-tartományból replika tartományvezérlők telepítése. Replika tartományvezérlő, a virtuális gép hello telepítheti a Windows Server Active Directory tartományi szolgáltatások, és opcionálisan használata hello telepítse az adathordozóról (IFM) szolgáltatás tooreduce hello adatmennyiséget, amelyet a toobe replikált toohello új tartományvezérlő telepítése során. Az oktatóanyagok esetén lásd: [Active Directory replika tartományvezérlő telepítése Azure](active-directory-install-replica-active-directory-domain-controller.md). Az adathordozóról történő telepítés használata esetén is lehet hatékonyabb toobuild hello virtuális tartományvezérlő-hez, és helyezze át a hello teljes virtuális merevlemez (VHD) toohello felhő ahelyett, hogy a Windows Server AD DS telepítése során. Biztonsági javasolt, hogy törölje hello VHD hello helyszíni hálózatról után másolt tooAzure lett.
* [Windows Server Active Directory-helyek topológiájával](#BKMK_ADSiteTopology): hozzon létre egy új Azure helyet az Active Directory – helyek és szolgáltatások. A Windows Server Active Directory alhálózati objektum toorepresent hello Azure-beli virtuális hálózat létrehozása és hozzáadása hello alhálózati toohello hely. Hozzon létre egy új helykapcsolatot hello új Azure helyet és a mely hello Azure-beli virtuális hálózat VPN-végpont található rendelés toocontrol hello helyet tartalmaz, és optimalizálja a Windows Server Active Directory forgalom tooand az Azure-ból.
* [IP-címzést és a DNS-](#BKMK_IPAddressDNS):
  
  * Állítsa be egy statikus IP-címet hello DC hello Set-AzureStaticVNetIP Azure PowerShell-parancsmag használatával.
  * Telepítse és konfigurálja a Windows Server DNS az Azure x hello tartományvezérlőn zajlik.
  * Hello virtuális hálózati tulajdonságok konfigurálása hello nevű és hello hello tartományvezérlő és DNS-kiszolgáló szerepkört üzemeltető virtuális gép IP-címét.
* [Földrajzilag elosztott tartományvezérlők](#BKMK_DistributedDCs): igény szerint konfigurálhatja a további virtuális hálózatokat. Ha az Active Directory-helyek topológiáját szükséges rendszerű tartományvezérlőkkel, amelyek megfelelnek az toodifferent Azure földrajzi régióban, mint amennyit toocreate Active Directory-helyek ennek megfelelően szeretné.
* [Írásvédett tartományvezérlők](#BKMK_RODC): hello Azure hely az írásvédett tartományvezérlő központi telepítését, attól függően, a követelmények végrehajtásához írási műveletek elleni hello DC és hello kompatibilitási az alkalmazások és szolgáltatások hello telephelyen RODC-k. Alkalmazáskompatibilitás kapcsolatos további információkért lásd: hello [írásvédett tartományvezérlők kompatibilitási útmutatója](https://technet.microsoft.com/library/cc755190).
* [Globális katalógus](#BKMK_GC): globális katalógusok szükséges tooservice bejelentkezési kérelmek többtartományos erdőben. Nem kell telepítenie a hello Azure hely a globális Katalógus, ha, a kimenő forgalom költségek a hitelesítési kérelmek oka lekérdezések juthatnak ebbe a generációba más helyek tájékozódnia. amely a forgalom toominimize, univerzális csoporttagság gyorsítótárazását az hello Azure hely az Active Directory – helyek és szolgáltatások engedélyezéséhez.
  
    Ha a globális Katalógus, helykapcsolatok konfigurálása és helykapcsolatok költségeit, amely az Azure site hello hello GC által előnyben részesített adatforrásként DC más juthatnak ebbe a generációba tooreplicate igénylő hello azonos részleges tartományi partíciókat.
* [Hello Windows Server AD DS-adatbázis és a SYSVOL helyét](#BKMK_PlaceDB): egy Azure virtuális gépeken futó rendelés toostore hello Windows Server Active Directory-adatbázis, a naplókat, és a SYSVOL adatok lemez tooDCs hozzáadása.
* [Biztonsági mentése és visszaállítása](#BKMK_BUR): határozza meg, ahol toostore rendszerállapot biztonsági mentéseit. Ha szükséges, adjon hozzá egy másik az adatok lemezre toohello DC VM toostore biztonsági.

## <a name="deployment-decisions-and-factors"></a>Telepítési döntések és tényezők
Ez a táblázat hello Windows Server Active Directory identitástechnológiai területein forgatókönyvek megelőző hello az érintett és a megfelelő döntések tooconsider, az alábbi hivatkozások toomore részletességgel hello foglalja össze. Néhány technológiai terület nem feltétlenül megfelelő tooevery üzembe helyezési forgatókönyv, és előfordulhat, hogy néhány identitástechnológiai területein kritikus fontosságú tooa környezetben, mint más identitástechnológiai területein.

Például ha telepít egy replika tartományvezérlő virtuális hálózaton, és az erdő egyetlen tartományt tartalmaz, majd kiválasztása toodeploy globáliskatalógus-kiszolgáló ebben az esetben nem lesz kritikus toohello üzembe helyezési forgatókönyv, mert nem hoz létre minden további replikációs követelmények. A hello ugyanakkor, ha több tartomány hello erdőben van, majd hello döntési toodeploy a globális katalógus, a virtuális hálózaton befolyásolhatja a rendelkezésre álló sávszélességet, teljesítmény, hitelesítés, directory-keresések és így tovább.

| Windows Server Active Directory technológiai terület | Döntések | Tényezők |
| --- | --- | --- |
| [Hálózati topológia](#BKMK_NetworkTopology) |Virtuális hálózat létrehozása? |<li>Követelmények tooaccess vállalati erőforrások</li> <li>Authentication</li> <li>Fiókkezelés</li> |
| [Tartományvezérlő telepítési konfiguráció](#BKMK_DeploymentConfig) |<li>Egy másik erdőben, a bizalmi kapcsolatok nélkül telepítéséhez?</li> <li>Az összevonási új erdő telepítéséhez?</li> <li>Windows Server Active Directory erdőszintű megbízhatósággal új erdőt vagy Kerberos telepítése?</li> <li>Corp erdő kiterjesztése a replika tartományvezérlő üzembe helyezésével?</li> <li>Kiterjeszti a Corp erdőben telepít egy új gyermektartomány vagy tartományfa?</li> |<li>Biztonság</li> <li>Megfelelőség</li> <li>Költségek</li> <li>Rugalmasság és a hibatűrés</li> <li>Alkalmazáskompatibilitás</li> |
| [Windows Server Active Directory-helyek topológiájával](#BKMK_ADSiteTopology) |Hogyan, webhelyek, helykapcsolatok és alhálózatok beállítása az Azure Virtual Network toooptimize forgalommal és költségek minimalizálása érdekében? |<li>Alhálózat és a webhely-definíciók</li> <li>A hely tulajdonságai és -módosítási értesítés</li> <li>Replikációs tömörítés</li> |
| [IP-címzést és a DNS-](#BKMK_IPAddressDNS) |Hogyan tooconfigure IP-címek és névfeloldás? |<li>Hello használata hello Set-AzureStaticVNetIP parancsmag tooassign egy statikus IP-cím használata</li> <li>Windows Server DNS-kiszolgáló telepítése és konfigurálása a virtuális hálózati tulajdonságok hello hello névvel és IP-címe hello hello tartományvezérlő és DNS-kiszolgáló szerepkört üzemeltető virtuális gép</li> |
| [Földrajzilag elosztott tartományvezérlők](#BKMK_DistributedDCs) |Hogyan tooreplicate tooDCs a külön virtuális hálózatok? |Ha az Active Directory-helyek topológiáját szükséges rendszerűek, amely megfelel a toodifferent Azure földrajzi régióban, mint amennyit toocreate Active Directory-helyek ennek megfelelően szeretné. [Konfigurálja a virtuális hálózat toovirtual hálózati kapcsolat](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) tooreplicate külön virtuális hálózatok a tartományvezérlők között. |
| [Írásvédett tartományvezérlők](#BKMK_RODC) |Írásvédett vagy írható tartományvezérlők használni? |<li>A HBI vagy PII attribútumok szűrése</li> <li>Szűrő titkos kulcsok</li> <li>Kimenő forgalom korlátot</li> |
| [Globális katalógus](#BKMK_GC) |Globális katalógus telepíteni? |<li>Egyetlen tartományból erdő ellenőrizze az összes tartományvezérlőjén juthatnak ebbe a generációba</li> <li>Többtartományos erdő juthatnak ebbe a generációba szükségesek a hitelesítéshez</li> |
| [Telepítési módszer](#BKMK_InstallMethod) |Hogyan tooinstall DC az Azure-ban? |Vagy: <li>A Windows PowerShell vagy a Dcpromo Active Directory tartományi szolgáltatások telepítése</li> <li>Helyezze át egy helyi virtuális tartományvezérlő virtuális Merevlemezt</li> |
| [Hello Windows Server AD DS-adatbázis és a SYSVOL helyét](#BKMK_PlaceDB) |Ha toostore Windows Server AD DS adatbázis, naplózza, és a SYSVOL? |Módosítsa a Dcpromo.exe alapértékeket. A kritikus Active Directory-fájlok *kell* Azure adatlemezek elhelyezni, amelyek megvalósítják az írási gyorsítótárazást operációsrendszer-lemezek helyett. |
| [Biztonsági mentés és visszaállítás](#BKMK_BUR) |Hogyan toosafeguard és helyreállítás adatokat? |-A rendszerállapot biztonsági mentésének létrehozása |
| [Összevonási kiszolgáló konfigurációja](#BKMK_FedSrvConfig) |<li>Az összevonási hello felhőben új erdő telepítéséhez?</li> <li>Helyszíni AD FS telepítéséhez, és tegye elérhetővé a proxy hello felhőben?</li> |<li>Biztonság</li> <li>Megfelelőség</li> <li>Költségek</li> <li>Hozzáférés tooapplications üzleti partnerek</li> |
| [Felhő konfigurálása](#BKMK_CloudSvcConfig) |Implicit módon történik a felhőszolgáltatás hello első alkalommal hoz létre egy virtuális gépet. Nem kell további felhőszolgáltatások toodeploy? |<li>Egy virtuális Gépet vagy virtuális gépek szüksége van az Internet közvetlen kitettség toohello?</li> <li> Hello szolgáltatást igényel a terheléselosztás?</li> |
| [Összevonási kiszolgáló követelményei a nyilvános és magánhálózati IP-címzési (virtuális IP-cím és a dinamikus IP)](#BKMK_FedReqVIPDIP) |<li>Hello Windows Server AD FS-példányt kell nem érhető el közvetlenül a hello Internet toobe?</li> <li>Igényelnek-e hello alkalmazás üzembe helyezéséhez hello felhőben saját Internet felé néző IP-cím és port?</li> |Minden virtuális IP-címet a telepítés során szükséges egy felhőalapú szolgáltatás létrehozása |
| [Windows Server AD FS magas rendelkezésre állás konfigurálása](#BKMK_ADFSHighAvail) |<li>A Windows Server AD FS kiszolgálófarm hány csomópontok?</li> <li>Hány csomópontok toodeploy a Windows Server AD FS proxy farm?</li> |Rugalmasság és a hibatűrés |

### <a name="BKMK_NetworkTopology"></a>Hálózati topológia
Rendelés toomeet hello IP-címek konzisztenciáját, és a Windows Server Active Directory tartományi szolgáltatások DNS-követelmények, szükség toofirst hozzon létre egy [Azure-beli virtuális hálózat](../virtual-network/virtual-networks-overview.md) , és csatlakoztassa a virtuális gépek tooit. A létrehozás során el kell döntenie, hogy toooptionally kiterjesztése kapcsolat tooyour helyszíni vállalati hálózaton, ami transzparens módon csatlakozik az Azure virtuális gépek tooon helyszíni gépeket – ehhez a hagyományos virtuális Magánhálózati technológiái és megköveteli, hogy a VPN-végponttal elérhetővé tehető a hello hello vállalati hálózat peremén.. Ez azt jelenti, hogy hello VPN Azure toohello vállalati hálózaton, nem viszont kezdeményezni.

Vegye figyelembe, hogy további többletköltségeket hello általános díjait, amelyek érvényesek a virtuális gép tooeach túl virtuális hálózati tooyour a helyszíni hálózat kiterjesztése. Pontosabban nincsenek díjak CPU-idő hello Azure virtuális hálózati átjáró és az összes virtuális Géphez, amely a helyszíni gépeket kommunikál a különböző hello VPN által generált hello kimenő forgalom. További információ a hálózati forgalom díjak: [Azure díjszabása:-a-áttekintő](http://azure.microsoft.com/pricing/).

### <a name="BKMK_DeploymentConfig"></a>Tartományvezérlő telepítési konfiguráció
hello módon hello Service, a hello követelmények függ DC hello konfigurálása Azure toorun szeretné. Például előfordulhat, hogy telepít egy új erdő, nem a saját vállalati erdő, egy-a-koncepció igazolása, egy új alkalmazást vagy valamilyen más rövid távú projekt, amelyhez címtárszolgáltatások, de nem adott hozzáférést toointernal vállalati erőforrásokhoz.

Előnyt, mint egy elkülönített erdő tartományvezérlő nem replikálja a vállalati tartományvezérlők, kevesebb kimenő hálózati forgalom előállítja hello magát, ami közvetlenül a költségek csökkentése. További információ a hálózati forgalom díjak: [Azure díjszabása:-a-áttekintő](http://azure.microsoft.com/pricing/).

Tegyük fel például, egy szolgáltatás adatvédelmi követelményeinek, de az hello szolgáltatás hozzáférési tooyour függ belső Windows Server Active Directory. Hello szolgáltatás hello felhőben toohost adatok engedélyezettek, ha a belső erdőnek Azure új gyermektartomány helyezhetők üzembe. Ebben az esetben is telepíthet a tartományvezérlő új gyermektartomány hello (nélkül hello rendelés toohelp cím adatvédelmi megfontolások a globális katalógussal). Ebben a forgatókönyvben egy replika tartományvezérlő telepítése egy virtuális hálózati kapcsolatot a helyszíni tartományvezérlők igényel.

Ha új erdőt hoz létre, hogy toouse [Active Directory bizalmi kapcsolatok](https://technet.microsoft.com/library/cc771397) vagy [összevonási megbízhatósági kapcsolatok](https://technet.microsoft.com/library/dd807036). Egyensúlyba hello kompatibilitási, biztonsági, megfelelőségi, költség és rugalmasság meghatározni. Például tootake előnyeit [szelektív hitelesítés](https://technet.microsoft.com/library/cc755844) előfordulhat, hogy toodeploy válasszon egy új erdő Azure, és hozzon létre egy Windows Server Active Directory megbízható hello a helyi erdő és a hello felhő erdő között. Ha hello alkalmazás jogcímbarát, azonban előfordulhat, hogy telepít helyett az erdőszintű Megbízhatóságok Active Directory összevonási megbízhatósági kapcsolatok. Egy másik tényező hello költségadatok replikálás tooeither további kiterjeszti a helyszíni Windows Server Active Directory toohello felhő által vagy fogja miatt a hitelesítés és a lekérdezés több kimenő adatforgalmat generálnak.

Rendelkezésre állás és a hibatűrés követelményei is befolyásolják a választott. Például ha hello kapcsolat megszakad, alkalmazások vagy a Kerberos megbízhatósági kapcsolat, vagy egy összevonási megbízhatósági kapcsolat, ami összes valószínűleg teljesen bontottuk kivéve, ha elegendő Azure-infrastruktúra központilag telepített. Például a replika tartományvezérlők alternatív telepítési konfigurációk (írható vagy RODC-k) megnövelik hello valószínűségét, hogy az képes tootolerate hivatkozás kimaradások esetén.

### <a name="BKMK_ADSiteTopology"></a>Windows Server Active Directory-helyek topológiájával
Meg kell toocorrectly határozza meg helyeket, és a sorrend toooptimize helykapcsolatok forgalom, és költségek minimalizálása érdekében. Helyek, helykapcsolatok és alhálózatok hatással hello replikációs topológia tartományvezérlők és a hitelesítési forgalom hello folyamat között. Vegye figyelembe a következő forgalmat díjak hello és központi telepítése, és az üzembe helyezési forgatókönyvnek toohello követelményeinek megfelelően azok a tartományvezérlők konfigurálása:

* Egy névleges díj óránként hello átjáró maga van:
  
  * Is lehet elindul vagy leáll, ahogyan szeretné
  * Ha leállt, Azure virtuális gépek el különítve hello vállalati hálózat
* Bejövő forgalom felszabadul.
* Kimenő forgalom fel van töltve, túl szerint[Azure díjszabása:-a-áttekintő](http://azure.microsoft.com/pricing/). Optimalizálhatja a hely tulajdonságai között a helyszíni és felhőalapú helyek hello az alábbiak szerint:
  
  * Ha több virtuális hálózat használ, adja meg hello helykapcsolatok és költségeik megfelelően rangsorolása hello Azure a Windows Server Active Directory tartományi szolgáltatások tooprevent hely több mint egy másikat, amely tartalmaz hello azonos szolgáltatás díjmentesen. Akkor is érdemes hello híd összes hely (amely alapértelmezés szerint engedélyezve van) hivatkozás (BASL) beállítás letiltása. Ez biztosítja, hogy csak közvetlenül csatlakoztatott helyek egymással replikálni. Azok a tartományvezérlők tranzitív csatlakoztatott helyek nem lesznek képesek tooreplicate közvetlenül egymással, de egy közös webhelyet vagy webhelyeket keresztül kell replikálni. Ha valamilyen okból hello közvetítő helyek nem érhető el, azok a tartományvezérlők tranzitív csatlakoztatott helyek közötti replikáció nem történik meg, akkor is, ha hello helyek közötti kapcsolat. Végezetül a tranzitív replikáció működését szakaszok továbbra is hasznos, ha webhely létrehozása helykapcsolati hidak, amelyek tartalmazzák a hello megfelelő helykapcsolatok és a helyen, például a helyszíni vállalati hálózati helyek.
  * [Konfigurálja a helykapcsolatköltségek](https://technet.microsoft.com/library/cc794882) megfelelően tooavoid nem kívánt forgalmat. Például ha **következő legközelebbi helyen próbálja** beállítás engedélyezve van, ellenőrizze, hogy hello virtuális hálózati vannak a helyek nem hello mellett legközelebbi hello költsége hello helykapcsolati objektum, amely a hello Azure hely hátsó toohello növelésével vállalati hálózat.
  * Konfigurálja a helykapcsolatot [intervallumok](https://technet.microsoft.com/library/cc794878) és [ütemezések](https://technet.microsoft.com/library/cc816906) tooconsistency követelmények és az objektum módosításai arány alapján történik. Az ütemezésben igazodni késés tolerancia. Tartományvezérlők csak hello utolsó állapotának értéket, replikálja, így csökkenő hello replikációs időköztől mentheti a költségek, ha nincs elegendő objektum-változási sebessége.
* Ha a költségek minimalizálása prioritást, ellenőrizze a replikációs ütemezve van, és értesítést nincs engedélyezve. Ez akkor hello alapértelmezett konfiguráció, ha a helyek közti replikálásához. Ez nem fontos, ha telepít írásvédett Tartományvezérlőt egy virtuális hálózat mert hello írásvédett tartományvezérlő nem replikálja a kimenő módosításokat. De ha telepít egy írható Tartományvezérlőt, ellenőrizze, hogy hello helykapcsolatot nincs beállított tooreplicate frissítések szükségtelen gyakorisággal. Ha egy globális katalógus (GC) kiszolgálóra telepíti, győződjön meg arról, hogy minden más olyan webhely, amely tartalmazza a globális Katalógus tartománypartíciók replikálja a forrás tartományvezérlő egy helyen, a helykapcsolatot kapcsolódnak a, vagy helykapcsolatok, amelyek alacsonyabb költségekkel rendelkeznek, mint a GC hello hello Azure site-e.
* Lehetséges toofurther továbbra is hello replikációs tömörítési algoritmusának módosítása a helyek közötti replikálás által létrehozott hálózati forgalom csökkentése érdekében. hello tömörítési algoritmusának hello REG_DWORD beállításjegyzékbeli bejegyzés HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Replicator tömörítési algoritmust határozza meg. hello alapértelmezett érték: 3, amely toohello Xpress Compress algoritmus ad eredményül. Hello érték too2, mely módosításokat hello algoritmus tooMSZip módosíthatja. A legtöbb esetben ez megnöveli a hello tömörítés, akkor viszont igen, a CPU-felhasználás hello költségén. További információkért lásd: [hogyan Active Directory replikációs topológia works](https://technet.microsoft.com/library/cc755994).

### <a name="BKMK_IPAddressDNS"></a>IP-címzést és a DNS-
Az Azure virtuális gépek alapértelmezés szerint "DHCP-bérelt címek" foglal le. Azure-beli virtuális hálózat dinamikus címek hello élettartama hello virtuális gép fennállnak virtuális géphez, mert hello Windows Server Active Directory tartományi szolgáltatások teljesülnek.

Ennek eredményeképpen Azure használjon dinamikus címet, ha meg vannak hatással a statikus IP-cím használatával, mert azt irányítható hello bérleti hello időszakban, továbbá hello címbérlet időtartama hello egyenlő toohello élettartama hello felhőalapú szolgáltatás.

Dinamikus hello-cím felszabadítása azonban ha hello virtuális gép leáll. tooprevent hello IP-címet folyamatban felszabadítása. lehetséges, hogy is [használja a Set-AzureStaticVNetIP tooassign egy statikus IP-cím](http://social.technet.microsoft.com/wiki/contents/articles/23447.how-to-assign-a-private-static-ip-to-an-azure-vm.aspx).

A névfeloldáshoz, telepítheti a saját (vagy kihasználja a meglévő) DNS-kiszolgálói infrastruktúrában; Azure által biztosított DNS-névfeloldási szükségleteit Windows Server Active Directory tartományi szolgáltatások speciális hello nem felel meg. Például hogy nem támogatja a dinamikus SRV-rekordok, és így tovább. Névfeloldás kritikus konfigurációs elem a tartományvezérlők és a tartományhoz csatlakoztatott ügyfelek. Azok a tartományvezérlők erőforrású rekordok regisztrációja és egyéb DC erőforrásrekordok feloldása képesnek kell lennie.
A tartalék tűréshatár és a teljesítmény érdekében optimális tooinstall hello Windows Server DNS-szolgáltatás az Azure-on futó hello tartományvezérlők esetén. Ezután konfigurálja hello Azure-beli virtuális hálózat tulajdonságai hello névvel és IP-cím hello DNS-kiszolgáló. Más virtuális gépek virtuális hálózati hello indul el, ha a DNS-ügyfél feloldási beállítások hello dinamikus IP-címek hozzárendelését részeként DNS-kiszolgáló konfigurálható.

> [!NOTE]
> Nem lehet csatlakozni a helyszíni számítógépek tooa Windows Server Active Directory tartományi szolgáltatások Active Directory tartományi hello interneten keresztül közvetlenül az Azure-on tárolt. az Active Directory és a tartományhoz való csatlakozást művelet hello megjelenítheti praktikus toodirectly teszi közzé a hello szükséges portok és a hatás, egy teljes DC toohello Internet, hello portokra vonatkozó követelmények.
> 
> 

Virtuális gépek regisztrálása a DNS-neve automatikusan indítási vagy egy kiszolgálónév-változás esetén.

Ebben a példában és egy másik példa bemutatja, hogyan tooprovision hello első virtuális gép, és az AD DS telepítéséhez kapcsolatos további információkért lásd: [egy új Active Directory-erdő telepítése a Microsoft Azure](active-directory-new-forest-virtual-machine.md). A Windows PowerShell használatával kapcsolatos további információkért lásd: [Azure PowerShell telepítése](/powershell/azureps-cmdlets-docs) és [Azure parancsmagokat](/powershell/module/azurerm.compute/#virtual_machines).

### <a name="BKMK_DistributedDCs"></a>Földrajzilag elosztott tartományvezérlők
Azure előnyöket kínál különböző virtuális hálózatokon lévő több tartományvezérlők esetén:

* Többhelyes hibatűrési
* Fizikai közelségi kapcsolat toobranch irodák (kisebb késést biztosít)

Virtuális hálózatok közötti közvetlen kapcsolat konfigurálásával kapcsolatos további információkért lásd: [konfigurálja a virtuális hálózati toovirtual hálózati kapcsolat](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

### <a name="BKMK_RODC"></a>Írásvédett tartományvezérlők
Toochoose kell, hogy toodeploy olvasási-csak vagy írható tartományvezérlő. RODC-k ferde toodeploy azért lehet, mert nem kell fizikai szabályozhatják őket, de RODC-k tervezett toobe telepített helyen, ahol a fizikai biztonsági van kitéve, például a fiókirodákban.

Azure nem jelent-e hello fizikai biztonsági kockázatot jelent a fiókiroda, de az RODC-k továbbra is bizonyulhat toobe költséghatékonyabb megoldás, mert hello funkciókat biztosítanak jól alkalmazható toothese környezetek Habár nagyon eltérő okokból. Például RODC-k nem kimenő replikációs rendelkezik, és vannak képes tooselectively feltöltése titkos kulcsokat (jelszó). A hello hátránya, ezeknek a kulcsoknak hello hiánya igény szerinti kimenő forgalom toovalidate lehet szükség, egy felhasználó vagy számítógép hitelesítő őket. De titkokat is szelektív előre feltöltve és gyorsítótárazza.

RODC-k és HBI és PII aggályokat környékén További előny adja meg, mert attribútumok, amelyek tartalmazzák a bizalmas adatok toohello RODC szűrt attribútumkészletet (eszközök) adhat hozzá. hello eszközök olyan attribútumokat, amelyek nem replikált tooRODCs testre szabható készlete. A biztonságos működés érdekében hello eszközök használhatja, abban az esetben nem engedélyezett, vagy nem szeretné a személyhez köthető adatokat és az Azure-on HBI toostore. További információ: [RODC szűrt attribútumkészlet [(https://technet.microsoft.com/library/cc753459)].

Győződjön meg arról, hogy alkalmazás kompatibilis lesz az RODC-k toouse tervezi. Számos, Windows Server Active Directory-kompatibilis alkalmazások működnek jól működő RODC-k, de néhány alkalmazást, hajtsa végre a töredezetté, vagy sikertelen, ha nem rendelkeznek hozzáféréssel tooa írható tartományvezérlő. További információkért lásd: [írásvédett tartományvezérlők kompatibilitási útmutatója](https://technet.microsoft.com/library/cc755190).

### <a name="BKMK_GC"></a>Globális katalógus
E szüksége toochoose tooinstall a globális katalógus (GC). Az erdő egyetlen tartományt úgy kell konfigurálni minden tartományvezérlő globáliskatalógus-kiszolgálóként. Mivel nincs további replikációs forgalom nem megnöveli költségeket.

Többtartományos erdőben a globális katalógusok szükséges tooexpand univerzális csoporttagság hello hitelesítési folyamat során. Ha nem kell telepítenie a globális Katalógus, hitelesítést, a tartományvezérlő az Azure virtuális hálózat hello munkaterhelések közvetve készítése kimenő hitelesítési forgalom tooquery helyszíni juthatnak ebbe a generációba minden hitelesítési kísérlet során.

globális katalógusok társított hello költségek nincsenek kevésbé előre jelezhető, mert azok üzemeltetéséhez, minden tartomány (a-része). Ha hello munkaterhelés egy internetre irányuló szolgáltatást használja, és hitelesíti a felhasználókat a Windows Server AD DS-en, hello költségek lehet teljesen előre nem látható. toohelp GC lekérdezések kívül hello felhő helyvédelmi csökkentheti a hitelesítés során, akkor [engedélyezi az univerzális csoporttagság gyorsítótárazását](https://technet.microsoft.com/library/cc816928).

### <a name="BKMK_InstallMethod"></a>Telepítési módszer
Hogyan kell toochoose tooinstall hello tartományvezérlők hello virtuális hálózaton:

* Új tartományvezérlők előléptetése. További információkért lásd: [egy új Active Directory-erdő telepítése Azure virtuális hálózatra](active-directory-new-forest-virtual-machine.md).
* Helyezze át a virtuális merevlemez egy helyi virtuális tartományvezérlő toohello felhő hello. Ebben az esetben győződjön meg arról, hogy hello a helyi virtuális tartományvezérlő "kerül," nem "másolt" vagy "Klónozott."

Használata csak az Azure virtuális gépek tartományvezérlők (megakadályozását tooAzure "web" vagy "munkavégző" szerepkört virtuális gépeken). Tartós, és a tartós állapot egy tartományvezérlő szükség. Az Azure virtuális gépek tervezett munkaterhelések esetén, mint azok a tartományvezérlők.

Ne használjon a SYSPREP toodeploy vagy tartományvezérlők klónozásához. hello képességét tooclone tartományvezérlők csak szervizcsomagjától kezdve érhetők el a Windows Server 2012-ben. Klónozási szolgáltatásának hello támogatást igényel az VMGenerationID az alapul szolgáló hipervizor hello. Hyper-V a Windows Server 2012 és az Azure virtuális hálózatok egyaránt használható VMGenerationID, harmadik fél virtualizálási szoftvergyártók.

### <a name="BKMK_PlaceDB"></a>Hello Windows Server AD DS-adatbázis és a SYSVOL helyét
Válassza ki, ahol toolocate hello Windows Server AD DS-adatbázis, a naplókat, és a SYSVOL MAPPÁT. Ezek az Azure-adatlemez kell telepíteni.

> [!NOTE]
> Az Azure adatlemezek korlátozott too1 TB.
> 
> 

Adatok lemezmeghajtók alapértelmezés szerint nem gyorsítótár írási műveleteket hajthatja végre. Adatok meghajtók, amelyek a virtuális gép csatlakoztatott tooa használjon író gyorsítótárazást. Írási gyorsítótárazással teszi, hogy hello írási véglegesített toodurable az Azure storage hello tranzakció hello virtuális gép operációs rendszerének hello szempontjából befejezése előtt. Tartósság érdekében némileg lassabban futnak, írások hello költségén biztosít.

Ez azért fontos a Windows Server Active Directory tartományi szolgáltatások, mert a késleltetve visszaírt lemez-gyorsítótárazási érvényteleníti hello tartományvezérlő által tett feltételezéseket. Windows Server Active Directory tartományi szolgáltatások kísérletet toodisable írási gyorsítótárazást, de toohello lemez IO rendszer toohonor fel azt. Hiba toodisable írási gyorsítótárazást, bizonyos körülmények vezethetnek fennmaradó objektumok és az egyéb problémákat eredményező USN-visszaállítást.

Az hello ajánlott eljárásként virtuális tartományvezérlők, a következő:

* Hello állomás gyorsítótár beállítás be hello Azure adatlemez sem. Ez megakadályozza, hogy problémákat írási gyorsítótárazást Active Directory tartományi szolgáltatások műveletekhez.
* Hello vagy ugyanazokat az adatokat lemezre, vagy külön adatlemezek hello adatbázis, a naplókat, és a SYSVOL tárolni. Ez általában a lemezen hello lemez operációs rendszerhez használt hello magát. hello kulcs takeaway hello Windows Server Active Directory tartományi szolgáltatások adatbázis és SYSVOL nem tárolható a egy Azure operációsrendszer-lemez típusra. Alapértelmezés szerint hello AD DS telepítési folyamata telepíti ezeket az összetevőket a % systemroot % mappában, ez nem ajánlott az Azure-bA.

### <a name="BKMK_BUR"></a>Biztonsági mentés és helyreállítás
Ügyeljen arra, mi, és nem támogatja a biztonsági mentési és visszaállítási tartományvezérlő általában, és pontosabban, a virtuális gépen futó. Lásd: [biztonsági mentése és visszaállítása a virtualizált tartományvezérlők szempontjai](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv#backup_and_restore_considerations_for_virtualized_domain_controllers).

Létrehozni a rendszerállapot biztonsági mentése csak kifejezetten ismeri a biztonsági mentés követelményeinek, a Windows Server Active Directory tartományi szolgáltatások, például a Windows Server biztonsági másolat biztonsági mentési szoftver segítségével.

Ne másolja vagy VHD-fájlokat a tartományvezérlők klónozásához helyett rendszeres biztonsági mentéseket. A visszaállítás legalább egyszer kell szükséges, ennek segítségével klónozott vagy másolt VHD-k, a Windows Server 2012 és a támogatott hipervizor nélkül is bevezeti, USN-buborékok során.

### <a name="BKMK_FedSrvConfig"></a>Összevonási kiszolgáló konfigurációja
Windows Server AD FS összevonási kiszolgálójára (STSs) hello konfigurációs függ e hello alkalmazásokat, amelyet az Azure toodeploy kell a helyszíni hálózati erőforrások tooaccess.

Ha hello alkalmazások felel meg a következő feltételek hello, elkülönítési hello alkalmazásokat telepíthet a helyi hálózatról.

* SAML-alapú biztonsági jogkivonatokat elfogadja őket
* -E exposable toohello Internet
* Amikor nem férnek hozzá a helyszíni erőforrások

Ebben az esetben konfigurálása Windows Server AD FS STSs az alábbiak szerint:

1. Egy egyetlen tartományból elkülönített erdő konfigurálása az Azure-on.
2. Összevont hozzáférést toohello erdő biztosítása a Windows Server AD FS összevonási kiszolgálófarm konfigurálásával.
3. Hello a helyi erdőben (összevonási kiszolgálófarm és összevonási kiszolgálóproxy-farmot) Windows Server AD FS konfigurálása
4. Közötti hello a helyszíni és az Azure-példányokon Windows Server Active Directory összevonási szolgáltatások összevonási megbízhatósági kapcsolat létrehozására.

A hello ugyanakkor, hello alkalmazások tooon helyszíni erőforrások eléréséhez szükséges, ha konfigurálhatja a Windows Server AD FS Azure hello alkalmazással az alábbiak szerint:

1. Konfigurálja a helyszíni hálózatokhoz és az Azure közötti kapcsolatot.
2. Konfigurálja a Windows Server AD FS összevonási kiszolgálófarm hello a helyi erdőben.
3. A Windows Server AD FS összevonási kiszolgálóproxy-farmot konfigurálása az Azure-on.

Ez a konfiguráció azt hello előnyt, csökkenti a hello elérhetővé tegyék a helyszíni erőforrások hasonló tooconfiguring Windows Server AD FS alkalmazásokkal a szegélyhálózaton.

Vegye figyelembe, hogy mindkét esetben hozhat létre megbízhatósági kapcsolatok kapcsolatok további identitás-szolgáltatóktól, a vállalatok együttműködés van szükség.

### <a name="BKMK_CloudSvcConfig"></a>Felhő konfigurálása
Cloud services csomag szükség, ha a virtuális gépek tooexpose közvetlen toohello Internet vagy az internetre irányuló tooexpose terheléselosztással rendelkező alkalmazás. Ez azért lehetséges, mert minden felhőalapú szolgáltatás kínál egy egyetlen konfigurálható virtuális IP-címet.

### <a name="BKMK_FedReqVIPDIP"></a>Összevonási kiszolgáló követelményei a nyilvános és magánhálózati IP-címzési (virtuális IP-cím és a dinamikus IP)
Minden Azure virtuális gép egy dinamikus IP-címet kap. A dinamikus IP-cím csak Azure-ban elérhető privát cím. A legtöbb esetben azonban lesz szükséges tooconfigure egy virtuális IP-címet a Windows Server AD FS-telepítések. hello virtuális IP-cím szükséges tooexpose Windows Server AD FS végpontok toohello Internet, és a hitelesítéshez és folyamatos felügyeletét összevont partnerek és az ügyfelek által használandó. Egy virtuális IP-cím egy felhőalapú szolgáltatás, amely tartalmazza az Azure virtuális gépek egy vagy több tulajdonságát. Ha hello Azure és a Windows Server AD FS telepített megjelölt jogcímbarát alkalmazáshoz Internet felé néző és a megosztáshoz közös portokon, mindkét egy virtuális IP-címet saját szükséges, és ezért lesz szükséges toocreate egy felhőalapú szolgáltatás hello alkalmazás és egy második az Windows Server AD FS-hez.

Hello feltételek virtuális IP-cím és a dinamikus IP-cím meghatározása, lásd: [szakkifejezéseket és definíciójukat](#BKMK_Glossary).

### <a name="BKMK_ADFSHighAvail"></a>Windows Server AD FS magas rendelkezésre állás konfigurálása
Bár lehetséges toodeploy önálló Windows Server AD FS összevonási szolgáltatások, a farm legalább két csomóponttal toodeploy ajánlott AD FS STS és proxyk éles környezetben.

Lásd: [AD FS 2.0-s üzemi topológia szempontjai](https://technet.microsoft.com/library/gg982489) a hello [AD FS 2.0 kialakítási útmutató](https://technet.microsoft.com/library/dd807036) toodecide telepítési konfigurációt legjobb lehetőség, a konkrét igényeinek.

> [!NOTE]
> Rendelés tooget terheléselosztás Azure, a Windows Server AD FS-végpontok konfigurálása hello Windows Server AD FS-farm összes tag a hello ugyanaz a felhőalapú szolgáltatás, és használja hello terheléselosztási Azure képességének (alapértelmezett: 80) HTTP és HTTPS-portot (alapértelmezett: 443). További információkért lásd: [Azure terheléselosztó mintavételi](https://msdn.microsoft.com/library/azure/jj151530).
> Windows Server hálózati terheléselosztási (NLB) nem támogatott az Azure-on.
> 
> 

