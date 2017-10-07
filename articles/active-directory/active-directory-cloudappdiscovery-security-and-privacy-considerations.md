---
title: "aaaCloud App Discovery biztonsági és adatvédelmi megfontolások |} Microsoft Docs"
description: "Ez a témakör ismerteti a hello biztonsági és adatvédelmi megfontolások kapcsolódó tooCloud App Discovery szolgáltatásra."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 2fce5c82-d3de-4097-808f-40214768df9e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 33659e85bd2cf4294e443512e69a85401f7c53f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Cloud App Discovery biztonsági és adatvédelmi megfontolások
A Microsoft véglegesített tooprotecting az adatok védelmére és biztonságossá tétele az adatokat, miközben nagy szoftverek és -szolgáltatások, amelyek segítségével kezelheti a szervezet hello biztonsági.  
Felismertük, hogy megbízhat az adatok tooothers, adott megbízhatósági megköveteli szigorú biztonsági mérnöki beruházások és szakértői tooback azt.
A Microsoft megfelelő toostrict megfelelőségi és biztonsági előírásait a biztonságos szoftver fejlesztési életciklus eljárások toooperating szolgáltatás.  
Biztonságossá tétele és az adatok védelmének a Microsoft a legnagyobb prioritással.

Ez a témakör azt ismerteti, hogyan adatok gyűjtése, feldolgozása, és Azure Active Directory a Cloud App Discovery belül védett

## <a name="overview"></a>Áttekintés
A cloud App Discovery szolgáltatás az Azure AD és az Microsoft Azure-ban.  
hello a Cloud App Discovery endpoint agent használt toocollect alkalmazás felderítési adatokat informatikai felügyelt gépek jelzi.  
hello összegyűjtött adatok továbbítása biztonságosan egy titkosított csatornán toohello Azure AD Cloud App Discovery szolgáltatásba.  
a Cloud App Discovery-adatok szervezet hello majd is elérhetővé válik a hello Azure-portálon. 

![A Cloud App Discovery működése](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png) 

hello alábbiakban hello információáramlás kövesse és ismertetik, hogyan titkosítása a szervezetben toohello Cloud App Discovery szolgáltatásba, és végső soron toohello a Cloud App Discovery portal átvitel során.

## <a name="collecting-data-from-your-organization"></a>A szervezet adatainak begyűjtése
Rendelés toouse Azure Active Directory Cloud App discovery szolgáltatás tooget betekintést hello alkalmazások a szervezet alkalmazottai által használt, toofirst kell hello Azure AD Cloud App Discovery endpoint agent toomachines a szervezetében telepíteni.

Azure Active Directory-bérlő hello (vagy a delegált) rendszergazdák hello agent telepítési csomagja letölthető hello Azure-portálon. hello ügynök vagy manuálisan telepíthető vagy hello szervezet SCCM vagy csoportházirend segítségével több számítógépen telepítve.

A központi telepítési beállítások további utasításokért lásd: [Cloud App Discovery csoport házirend telepítési útmutató](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).


### <a name="data-collected-by-hello-agent"></a>Hello ügynök által gyűjtött adatok
az alábbi listában hello meghatározott hello információkat gyűjti a program hello ügynök által a kapcsolat tooa webalkalmazás. hello csak összegyűjtött információkról az alkalmazásokhoz, hogy hello rendszergazda felderítési van beállítva.  
Szerkesztheti a felhőalkalmazások, amelyeknek az ügynök figyelők hello keresztül hello Microsoft Cloud App Discovery paneljén hello hello listája [Azure-portálon](https://portal.azure.com/)a **beállítások**->**adatok Gyűjtemény**->**Alkalmazáskatalógus lista**. További részletekért lásd: [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


**Információk kategória**: felhasználói adatok  
**Leírás**:  
hello Windows rendszerbeli felhasználónevet egy kérelem toohello cél webalkalmazás végrehajtott hello folyamat (pl.: tartomány\felhasználónév) valamint a Windows biztonsági azonosítók (SID) hello felhasználó hello.

**Információk kategória**: feldolgozni adatait  
**Leírás**:  
hello folyamat hello webes kérelem toohello célalkalmazás végrehajtott hello nevét (pl.: "iexplore.exe")

**Információk kategória**: számítógép-információk  
**Leírás**:  
hello számítógép NetBIOS-neve mely hello ügynök telepítve van.

**Információk kategória**: alkalmazás forgalmi információk  
**Leírás**: 

a következő kapcsolati adatokat hello:

* hello forrás (helyi számítógép) és a cél IP-címek és a portszámok
* hello nyilvános IP-cím hello szervezet keresztül mely hello kérelem kerül ki.
* hello kérelem hello ideje
* küldött és fogadott forgalom mennyiségét hello
* hello vagy IP-verziót (4 6)
* A TLS-kapcsolatok csak: hello cél állomásnevet hello kiszolgálónév jelzése bővítmény vagy hello kiszolgálói tanúsítványt.

a következő HTTP-információk hello:

* Metódus (GET, POST, stb.)
* Protokoll (HTTP/1.1, stb.)
* Felhasználói ügynök karakterlánca
* Gazdanév
* Cél URI (kivéve a lekérdezési karakterlánc)
* Tartalom típussal kapcsolatos információk
* Hivatkozó URL-cím adatait (kivéve a lekérdezési karakterlánc)

> [!NOTE]
> hello fenti HTTP-adatokat gyűjti össze az összes nem titkosított kapcsolat.
> A TLS-kapcsolatok esetén ezt az információt csak rögzített bekapcsolásakor hello "Mélyreható vizsgálat" beállítás hello portálon. hello értéke "ON" alapértelmezés szerint.
> További részletekért lásd az alábbi, és [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 

Ezenkívül toohello adatok adott hello ügynök gyűjti a hello hálózati tevékenységre vonatkozó, azt is hello hardver- és konfigurációs, jelentéseket és hello ügynök használatáról információt névtelen adatokat gyűjt.


### <a name="how-hello-agent-works"></a>Hello ügynök működése
hello az ügynök telepítése a két összetevőből áll:

* A felhasználói módú összetevő
* Kernelmódú illesztőprogram-összetevő (a Windows szűrőplatform-illesztőprogramok)

Hello ügynök első telepítésekor a számítógép-specifikus megbízható tanúsítvány tárol hello gépen amelyeket majd használ tooestablish biztonságos kapcsolatot hello Cloud App Discovery szolgáltatásba.  
hello ügynök rendszeres időközönként lekérdezi házirend konfigurációs hello Cloud App Discovery szolgáltatásba a biztonságos kapcsolaton keresztül.  
hello házirendet is tartalmaz, mely felhőalapú alkalmazások toomonitor kapcsolatos információkat, és hogy automatikus frissítési engedélyezni kell, többek között.

Webes forgalom küldése és fogadása hello a számítógépen az Internet Explorer és a Chrome, hello a Cloud App Discovery-ügynök hello forgalmát elemzi és kivonatok hello vonatkozó metaadatok (lásd: hello **hello ügynök által gyűjtött adatok** szakasz a fenti).  
Minden percben hello ügynök gyűjtött hello metaadatok toohello Cloud App Discovery szolgáltatásba feltölti a egy titkosított csatornán keresztül.

hello illesztőprogram összetevő elfogja azokat a titkosított forgalom hello és beépülve hello titkosított adatfolyamba. További részletek a hello **elfogja a titkosított kapcsolatokat (mélyreható vizsgálat) adatait** az alábbi szakasz.

### <a name="respecting-user-privacy"></a>Felhasználói adatok tiszteletben
Célunk tooprovide rendszergazdák hello eszközök tooset hello egyenleg az alkalmazás használatát és a felhasználói adatvédelem a szervezet megfelelő részletes optika között. toothat célból nyújtunk hello forgatógombját hello hello portál beállítások lapján a következő:

* **Adatgyűjtés**: a rendszergazdák kiválaszthatják toospecify mely alkalmazások vagy tooget felderítési adatok meg szeretnék alkalmazáskategóriákat.
* **Mélyreható vizsgálat**: rendszergazdák toospecify kiválaszthatja, ha hello ügynök gyűjti az SSL/TLS kapcsolatok HTTP-forgalom (más néven **"Mélyreható vizsgálat"**). Bővebben a következő szakaszban hello.
* **Hozzájárulási beállítások**: a rendszergazdák használhatják a Cloud App Discovery portál toochoose hello e toonotify felhasználók hello adatgyűjtési hello ügynök, és hogy toorequire felhasználói hozzájárulás hello ügynök felhasználói adatgyűjtés megkezdése előtt.

hello a Cloud App Discovery endpoint agent csak hello leírt hello adatokat gyűjt **hello ügynök által gyűjtött adatok** fenti szakaszban.

### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Adatok elfogja a titkosított kapcsolatokat (mélyreható vizsgálat)
A korábban említett azt, a rendszergazdák konfigurálhatják hello ügynök toomonitor adatok a titkosított kapcsolatokat (a "mélyreható vizsgálat"). A TLS ([Transport Layer Security](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) egyike hello hello Internet használt leggyakoribb protokollok ma. Kommunikáció a TLS titkosításával ügyfél létesíthet-e egy biztonságos és személyes kommunikációs csatorna webkiszolgáló; A TLS az hitelesítő adatok továbbításához alapvető védelmet biztosít, és hello bizalmas információk közzétételének megakadályozása.

Hello végpontok közötti biztonságos titkosított csatornán TLS által biztosított fontos biztonsági és adatvédelmi védelem lehetővé teszi, akkor hello protokoll gyakran visszaélt, rosszindulatú vagy nefarious célokra. Adjon annyi Igen valójában TLS gyakran az említett tooas hello "univerzális tűzfal-megkerülési protokoll". hello hello probléma gyökere, hogy a legtöbb tűzfalon nincsenek-e a nem tooinspect TLS kommunikációs, mert hello alkalmazásszintű adatok titkosítása SSL használatával. Ennek tudatában, a támadók gyakran használja ki a TLS toodeliver rosszindulatú hasznos adat található tooa felhasználói abban, hogy még hello legtöbb intelligens alkalmazásréteg-tűzfalak teljesen vak tooTLS, és egyszerűen kell továbbítani a TLS kommunikációs gazdagépek között. A végfelhasználók gyakran használja ki a TLS toobypass hozzáférés-vezérlés vállalati tűzfalak és proxykiszolgálók tooconnect toopublic proxyk használja, és a TLS protokollok, ellenkező esetben előfordulhat, hogy a házirend által blokkolt hello tűzfalon bújtatásra lépnek érvénybe.

Mélyreható vizsgálat lehetővé teszi, hogy a Cloud App Discovery-ügynök tooact hello szerint egy megbízható man-az-átjárójának. Amikor egy ügyfél kérelmet tooaccess egy HTTPS védett erőforrás, hello Endpoint-ügynök illesztőprogram elfogja hello kapcsolatot hoz létre egy új kapcsolat toohello cél kiszolgáló tooretrieves az SSL-tanúsítvány hello ügyfél nevében. hello ügynök majd ellenőrzi, hogy hello tanúsítványt is megbízhatóként (ellenőrzése, hogy azt nem visszavonva, és más tanúsítvány ellenőrzések végrehajtása), és ezeket adja át, ha hello Endpoint-ügynök, majd másolja át hello kiszolgálótanúsítvány hello információkat és hoz létre a saját kiszolgálói tanúsítvány--néven egy hozzáférés-tanúsítvány – ezt az információt. hello hozzáférés tanúsítványok aláírt az azonnali hello endpoint-ügynök a legfelső szintű tanúsítvánnyal, hello Windows megbízható tanúsítványok tárába telepített is. A önaláírt legfelső szintű tanúsítvány nem exportálható van megjelölve, és hozzáférés-vezérlési lista tooadministrators volna. Mert ez tervezett toonever-hagyja hello gép, amelyen létrehozták. Amikor hello végfelhasználó ügyfélalkalmazás hello hozzáférés tanúsítványt kap, a azt bízó is sikeresen érvényesíteni hello tanúsítványlánc összes hello módon toohello legfelső szintű tanúsítványt is. Ez a folyamat alapvetően a végfelhasználó szempontjából az alább ismertetett néhány kikötésekkel átlátszó.

Mélyreható vizsgálat engedélyezésével hello Cloud App Discovery Endpoint Agent visszafejthetik és TLS titkosított kommunikáció, így hello szolgáltatás tooreduce zaj vizsgálhatja és titkosított hello felhőalkalmazások hello használata áttekintést adnak.

#### <a name="a-word-of-caution"></a>A figyelmeztetés szót
Mélyreható vizsgálat bekapcsolása, előtt erősen ajánlott a céljaira tooyour jogi és HR-részleg kommunikálnak, és szerezze be a hozzájárulásukat. Tanulmányozza a végfelhasználó személyes titkosított kommunikációt nyilvánvaló oka egy bizalmas tulajdonos lehet. Mielőtt egy éles kiépítése mélyreható vizsgálat, gondoskodjon arról, hogy a vállalati biztonsági és használati házirendek frissítve lett, titkosított kommunikációt tooindicate ellenőrizni kell. Felhasználói értesítés és a kivételt a helyek tekinteni bizalmas (pl. banki és orvosi helyek) is szükséges, ha a Cloud App Discovery toomonitor konfigurálja azokat. Fent említett rendszergazdák használhatják a Cloud App Discovery portál toochoose hello e toonotify felhasználók hello adatgyűjtési hello ügynök, és hogy toorequire felhasználói hozzájárulás hello ügynök felhasználói adatgyűjtés megkezdése előtt.

### <a name="known-issues-and-drawbacks"></a>Ismert problémák és a hátrányai
Van néhány olyan esetekben, ahol a TLS hozzáférés hello végfelhasználói élmény befolyásolhatja:

* Bővített érvényesítési (Bővített) tanúsítványok hello címmezőjébe hello webes böngésző zöld tooact jelennek meg, hogy egy megbízható webhely meglátogat egy visual clue. TLS-ellenőrző nem lehet ugyanaz a EV hello tanúsítvány kapcsolatos toohello ügyfél, így Bővített tanúsítványok használó webhelyek megszokott módon fognak működni, de hello címsor nem jelenik meg a zöld.  
* Nyilvános kulcs rögzítési (más néven tanúsítvány rögzítését) tervezett toohelp-átjárójának felhasználók megvédje és tanúsítványszolgáltatók támadó. Ha rögzített hely hello legfelső szintű tanúsítvány nem felel meg egyik ismert helyes hitelesítésszolgáltató hello, hello böngésző elutasítja hello kapcsolódási hiba történt. Mivel a TLS-hozzáférés, valójában egy man-az-átjárójának, ezek a kapcsolatok sikertelen lesz.
* Ha a felhasználók hello lakat ikon látható hello böngésző cím sáv böngésző tooinspect hello helyinformációkat gombra, akkor nem fog látni hello hitelesítésszolgáltató végződése lánc használt toosign hello webhelytanúsítvány, de ehelyett a végződő tanúsítványlánc Windows hello megbízható tanúsítványok tárolójába.

Ezeket a problémákat tooreduce hello előfordulását nyomon követjük, felhőszolgáltatások és toouse ismert ügyfélalkalmazások bővített, az érvényesítés vagy a nyilvános kulcs rögzítését, és utasítsa a hello Endpoint-ügynök tooavoid érintett kapcsolatok elfogja. Még ezekben az esetekben azonban továbbra is az ezekbe a felhőalkalmazásokba hello használata, és az átvitt adatok mennyiségét hello jelentések fog kapni, de mivel azok nincsenek mély megvizsgálni, módjáról hello alkalmazások volt részletek nem lesz elérhető.

## <a name="sending-data-toocloud-app-discovery"></a>Sending data tooCloud App Discovery szolgáltatásra
Miután a metaadatok hello ügynök által gyűjtött, vannak gyorsítótárazva mentése tooone perc hello gépen található, vagy hello csak gyorsítótárazott adatokat mérete eléri a 5 MB-nál. Majd tömörített, és egy biztonságos kapcsolatot toohello Cloud App Discovery szolgáltatásba keresztül.

A Cloud App Discovery szolgáltatásba bármilyen okból hello nem toocommunicate hello ügynök esetén hello összegyűjtött metaadatokból tárolódik a helyi gyorsítótárban csak megfelelő jogosultsággal rendelkező felhasználók (például a Rendszergazdák csoport hello) hello gépen hozzáfér.  
hello ügynök automatikusan kísérletek tooresend hello gyorsítótárazott metaadatok mindaddig, amíg sikeresen érkezett hello a Cloud App Discovery szolgáltatás által.

## <a name="receiving-hello-data-at-hello-service-end"></a>Adatfogadás hello hello szolgáltatás végén
hello ügynökök hitelesítéséhez toohello a Cloud App Discovery szolgáltatás hello gép adott ügyfél-hitelesítési tanúsítványt használ a fentiekben említett, és egy titkosított csatornán keresztül továbbítja az adatokat.  
hello Cloud App Discovery szolgáltatásba elemzési folyamatok elvégzésére folyamatok metaadatok minden ügyfél esetében külön-külön által logikailag particionálás hello analytics folyamat minden szakaszában.
hello elemzése metaadatok meghajtók hello hello portálon különböző jelentéseket.

hello feldolgozatlan metaadatok, és elemzése hello metaadatok mentése too180 nap tárolódnak. Emellett az ügyfelek is ki lehet választani toocapture elemzett hello metaadatok igénye egy Azure blob storage-fiók.
Ez akkor hasznos, a kapcsolat nélküli elemzéshez metaadatok és a hosszabb megőrzési hello adatok.

## <a name="accessing-hello-data-using-hello-azure-portal"></a>Hello adatok hello Azure-portál elérése
Egy elérhető tookeep hello metaadatok gyűjtött biztonságos, alapértelmezés szerint csak globális rendszergazdák hello bérlő rendelkezik hozzáférési toohello a Cloud App Discovery szolgáltatás hello Azure-portálon a.  
Azonban a rendszergazdák kiválaszthatják toodelegate a hozzáférés tooother felhasználókat vagy csoportokat.

> [!NOTE]
> További részletekért lásd: [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
> 
> 


Felhasználói hozzáférés hello adatokat hello portálon, az Azure AD Premium licenccel rendelkező licenccel kell rendelkezniük.

## <a name="additional-resources"></a>További források
* [Hogyan lehet használt, jóvá nem hagyott felhőalkalmazások felderítése a szervezeten belül](active-directory-cloudappdiscovery-whatis.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

