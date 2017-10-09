---
title: Azure Data Encryption nyugalmi aaaMicrosoft |} Microsoft Docs
description: "Ez a cikk áttekintése a Microsoft Azure adattitkosítás nyugalmi, általános képességek és általános szempontok hello."
services: security
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: TomSh
ms.assetid: 9dcb190e-e534-4787-bf82-8ce73bf47dba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: yurid
ms.openlocfilehash: d74d103e4fd7585543b4f039877af7d742a6b2e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-encryption-at-rest"></a>Az Azure Data Encryption nyugalmi
Nincsenek a Microsoft Azure toosafeguard adatok tooyour a vállalat biztonsági és megfelelőségi igényei szerint belül több eszközt. A dokumentum hogyan adatokat a Microsoft Azure között aktívan védett, és ismerteti, amelyek hello hello data protection megvalósítási részt vevő összetevők, ellenőrzi, hogy előnyei és hátrányai hello különböző kulcskezelés védelmi megközelítések összpontosít. 

Aktívan nem adattitkosítás egy közös biztonsági követelménye. A Microsoft Azure előnye, hogy a szervezetek érhető el titkosítását, anélkül, hogy a hello költségét megvalósítása és egyéni kulcskezelés megoldást kezelési és hello kockázatát. A szervezetek rendelkezik hello lehetőségével teljesen kezelheti a aktívan Azure engedélyezése. Emellett a szervezet rendelkezik a különböző beállítások tooclosely titkosítás vagy a titkosítási kulcsok kezeléséhez.

## <a name="what-is-encryption-at-rest"></a>Mi az a titkosítását?
Titkosítását hivatkozik toohello kriptográfiai kódolás (titkosítás) az adatok amikor azt. hello Azure Rest tervek titkosítását szimmetrikus titkosítási tooencrypt és és nagy adatmennyiségek gyors szerint tooa egyszerű fogalmi modell visszafejtéséhez:

- A szimmetrikus titkosítási kulcsot az használt tooencrypt adatai, azt is megőrződjenek 
- ugyanazt a titkosítási kulcs hello van a adatok használt toodecrypt, akkor a rendszer a memóriában readied
- Adatok particionálható, és különböző kulcsok használhatók minden partíció esetében
- Hozzáférés-vezérlési házirendeket korlátozza a hozzáférést toocertain identitások és naplózási kulcshasználat a kulcsokat tárolja biztonságos helyen. Az adattitkosítási kulcsokat gyakran titkosított az aszimmetrikus titkosítási toofurther hozzáférés korlátozása (hello tárgyalt *kulcs hierarchia*, ez a cikk későbbi)

a fenti hello hello általános magas szintű elemeinek titkosítását ismerteti. A gyakorlatban a fő felügyeleti és ellenőrzési forgatókönyvek, valamint a méretezés és a rendelkezésre állási biztosítékok, további szerkezetek igényelnek. A Microsoft Azure titkosítás Rest fogalmakat és az összetevők az alábbiakban található.

## <a name="hello-purpose-of-encryption-at-rest"></a>hello célját aktívan titkosítása
Aktívan nem adattitkosítás tervezett tooprovide adatvédelmet adatokat nyugalmi (a fentiekben ismertetett.) Adatokat nyugalmi elleni támadások lehetnek kísérletek tooobtain fizikai hozzáférés toohello hardver a mely hello tárolja, és ezután a biztonsági sérülés hello adatokat tartalmazott. Az ilyen támadások egy kiszolgáló merevlemezén előfordulhat, hogy rendelkezik lett kezelésük során karbantartási, hogy a támadó tooremove hello merevlemez-meghajtón. Újabb hello támadó hello merevlemez-meghajtóról kellene üzembe egy számítógép, a vezérlő tooattempt tooaccess hello adatai alapján. 

Aktívan nem adattitkosítás tervezett tooprevent hello támadó titkosítatlanul hello adatok hozzáférését az adatok titkosítása a lemezen hello biztosításával. Ha a támadó tooobtain egy merevlemezen, az ilyen titkosított adatok, és nincs hozzáférési toohello titkosítási kulcsokat, hello támadó nem veszélyeztetheti a hello adatok nélkül nehéz. Ilyen esetben a támadónak tooattempt támadások elleni titkosított adatok, amelyek sokkal bonyolultabb, és erőforrás fel mint elérése nem titkosított adatokat a merevlemezen. Emiatt aktívan titkosítása erősen ajánlott, és magas prioritású követelmény a legtöbb szervezet számára. 

Bizonyos esetekben titkosítását is szüksége van egy szervezet szükséges adatok irányítási és a megfelelőségi erőfeszítéseket. Iparági és kormányzati szabványok, például a HIPAA, PCI és FedRAMP és nemzetközi szabályozási követelmények, folyamatok és adatok védelme és titkosítási követelmények vonatkozó házirendek adott óvintézkedéseket elrendezését. Ezek a szabályozások számos titkosítását mérőszáma kötelező a megfelelő adatok kezelése és védelme szükséges. 

Ezenkívül toocompliance és szabályozási követelmények, titkosítását kell kell tekinteni, mint egy védelmi jellegű platform képességei. Microsoft megfelelő platformot kínál a szolgáltatások, alkalmazások és adatok, az átfogó létesítményt a személyes és a fizikai biztonság, adatelérési vezérlő és a naplózás, de napjainkban fontos tooprovide további "átfedő" biztonsági intézkedéseket hello egyik esetben egyéb biztonsági intézkedések meghiúsul. Titkosítását ilyen egy további védelmi mechanizmust biztosít.

Microsoft felhőszolgáltatások és tooprovide ügyfelek kezelhetőségi megfelelő titkosítási kulcsok és a titkosítási kulcsok használata megjelenítő hozzáférés toologs nem véglegesített tooproviding titkosítását rest-beállítások. Ezenkívül a Microsoft felé hello cél az, hogy az összes felhasználói adat titkosítása alapértelmezés szerint működik.

## <a name="azure-encryption-at-rest-components"></a>Az Azure titkosítását Rest-összetevők

Az előzőleg leírtak hello titkosítását célja, hogy a lemezen fennállásának adattitkosítás titkos kulccsal. tooachieve fentieket biztonságos kulcs létrehozása, a tárolás, a hozzáférés-vezérléshez és a felügyeleti hello titkosítási kulcsok meg kell adni. Részletek változhat, bár az Azure szolgáltatások titkosítási Rest-megvalósítások jelentheti alatt majd mutatja be a következő diagram hello fogalmainak hello tekintetében.

![Összetevők](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig1.png)

### <a name="azure-key-vault"></a>Azure Key Vault

hello tárolási hello titkosítási kulcsok és access control toothose helye rest modell központi tooan titkosítását. hello kulcsok a megadott felhasználók és a rendelkezésre álló toospecific szolgáltatások által kezelhető de magas védett toobe kell. Az Azure szolgáltatások esetében az Azure Key Vault hello ajánlott kulcstároló megoldás, és egy közös kezelést biztosít szolgáltatásban. Kulcsok tárolása és kezelése a kulcstárolót, és hozzáférést tooa kulcstároló adható meg toousers vagy szolgáltatások. Az Azure Key Vault ügyfél által felügyelt titkosítási kulcs olyan esetekben támogatja a kulcsok létrehozását ügyfél vagy ügyfél kulcsok importálása.

### <a name="azure-active-directory"></a>Azure Active Directory

Engedélyek toouse hello kulcsok tárolása az Azure Key Vault, toomanage vagy tooaccess őket titkosítási inaktív adatok titkosítása és visszafejtése, adható meg tooAzure Active Directory-fiókok. 

### <a name="key-hierarchy"></a>Kulcs hierarchia

Általában több titkosítási kulcs használatban van egy rest-megvalósítási titkosítását. Aszimmetrikus titkosítási esetén hasznos létrehozó hello megbízhatóság és a hitelesítési kulcs hozzáférési és felügyeleti szükséges. Szimmetrikus titkosítást hatékonyabb tömeges titkosítás és visszafejtés, lehetővé teszi a erősebb titkosítást és jobb teljesítményt. Emellett az egy egyetlen titkosítási kulcs csökken hello kockázata, hogy a kulcs hello lesz, és hello újbóli titkosítása költségét, ha egy kulcsot le kell cserélni hello használatának korlátozását. tooleverage hello aszimmetrikus és szimmetrikus titkosítási és előnyei korlát hello használja, és egy kulcs elérhetővé tegyék, rest-modellek Azure titkosítását áll a következő kulcstípusok hello kulcs hierarchia használatának:

- **Adatok titkosítási kulcs-(adattitkosítási kulcs)** – AES256 szimmetrikus kulcs tooencrypt használt, a partíció vagy az adatblokk.  Előfordulhat, hogy egyetlen sok partíciót és sok az adattitkosítási kulcsokat. Minden adatblokk különböző kulccsal titkosított nehezebbé titkosítási elemzés támadásokat. Hozzáférés tooDEKs hello szolgáltató vagy az alkalmazás erőforráspéldány titkosítása és egy adott adatblokk visszafejtése van szükség. Amikor a adattitkosítási kulcs helyére egy új kulcsot csak hello adatok a társított blokkban kell újra titkosítani hello új kulccsal.
- **Kulcs titkosítási kulcscserekulcs (KEK)** – aszimmetrikus titkosítási kulcs használt tooencrypt hello az adattitkosítási kulcsokat. A fő titkosítási kulcs használatával hello maguk toobe titkosított és a titkosítási kulcsokat. hozzáférés toohello hello entitás KEK eltérhetnek hello entitás, amely hello adattitkosítási kulcsot igényel. Ez lehetővé teszi, hogy egy entitás toobroker hozzáférés toohello adattitkosítási kulcs annak biztosítására, hogy az egyes adattitkosítási kulcs toospecific partíciók korlátozott hozzáférésű hello célra. Mivel hello KEK szükséges toodecrypt hello DEKs, hello KEK hatékonyan egy olyan hibaérzékeny pontot, amellyel DEKs hatékonyan törölheti az hello KEK törlését.

hello az adattitkosítási kulcsokat, titkosított kulcs titkosítási kulcsok hello tárolódnak, és ezzel a kulccsal titkosított csak egy fő titkosítási kulcs bármely az adattitkosítási kulcsokat kérheti le a hozzáférési toohello rendelkező entitás. A kulcs tárolása különböző modellek támogatottak. Egyes modellek hello következő szakasz későbbi részében részletesebben ismertetik.

## <a name="data-encryption-models"></a>Adatok titkosítása modellek

Understanding hello különböző titkosítási modellek, és azok előnyei és hátrányai elengedhetetlen az ismertetése, hogyan hello Azure megvalósítása titkosítását a különböző erőforrás-szolgáltató. Ezek a definíciók összes erőforrás-szolgáltató között megosztott Azure tooensure közös nyelvi és besorolás. 

Kiszolgálóoldali titkosítás három forgatókönyv van:

- Kiszolgálóoldali titkosítás szolgáltatás felügyelt kulcsok használata
    - Az Azure erőforrás-szolgáltatók hello titkosítási és visszafejtési műveleteket végezhet.
    - A Microsoft hello kulcsok kezelése
    - Teljes felhő funkció

- Ügyfél által felügyelt kulcsok használata az Azure Key Vault kiszolgálóoldali titkosítás
    - Az Azure erőforrás-szolgáltatók hello titkosítási és visszafejtési műveleteket végezhet.
    - Felhasználói vezérlők kulcsok Azure Key Vault keresztül
    - Teljes felhő funkció

- Kiszolgálóoldali titkosítás szabályozott ügyfélhardvereken ügyfél által felügyelt kulcsok használata
    - Az Azure erőforrás-szolgáltatók hello titkosítási és visszafejtési műveleteket végezhet.
    - Felhasználói vezérlők kulcsok ügyfél szabályozott hardver
    - Teljes felhő funkció

Ügyféloldali titkosítás vegye figyelembe a következő hello:

- Azure-szolgáltatások visszafejtett adatait nem látja.
- Ügyfelek kezelése és a helyszíni kulcsok tárolására (vagy más biztonságos tárolók). Kulcsokban nincs elérhető tooAzure szolgáltatások
- Csökkentett felhő funkció

az Azure-felosztása két fő csoportok modellek hello támogatott titkosítási: "Titkosítási ügyfél" és "kiszolgálóoldali titkosítás" mint azt korábban említettük. Vegye figyelembe, független a többi modell hello titkosítását használt, az Azure szolgáltatások mindig ajánlott egy biztonságos, például a TLS vagy HTTPS átviteli hello használatát. Ezért titkosításra átvitelt hello átviteli protokoll által kell figyelembe venni, és nem lehet meghatározni, hogy mely rest modell toouse titkosítását egy jelentős tényező.

### <a name="client-encryption-model"></a>Ügyfél titkosítási modell

Ügyfél titkosítási modell hello szolgáltatás, illetve a hívó alkalmazás erőforrás-szolgáltató vagy Azure hello kívül végrehajtott műveletek tooencryption hivatkozik. hello titkosítási hello szolgáltatás alkalmazás az Azure-ban, vagy hello ügyfél adatközpont futó alkalmazáshoz hajtható végre. Mindkét esetben, ha a titkosítás modell kihasználva hello Azure erőforrás-szolgáltató kap egy titkosított blob adatok hello képességét toodecrypt hello adatok nélkül semmilyen módon, vagy rendelkezik hozzáférési toohello titkosítási kulcsokat. Ebben a modellben hello kulcskezelő szolgáltatás vagy alkalmazás hívása hello végezhető el és nem teljesen átlátszó toohello Azure-szolgáltatás.

![Ügyfél](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig2.png)

### <a name="server-side-encryption-model"></a>Kiszolgálóoldali titkosítás modell

Kiszolgálóoldali titkosítás modellek hello Azure-szolgáltatás által végrehajtott műveletek tooencryption hivatkozik. Minta, erőforrás-szolgáltató hello végez hello titkosításához és visszafejtéséhez műveletek. Azure Storage például kapnak adatokat egyszerű szöveges műveletek, és végrehajtják a hello titkosítási és visszafejtési belső. Erőforrás-szolgáltató hello használhatja a Microsoft vagy attól függően, hogy a megadott konfigurációs hello hello ügyfél által felügyelt titkosítási kulcsok. 

![Kiszolgáló](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig3.png)

### <a name="server-side-encryption-key-management-models"></a>Kiszolgálóoldali titkosítás kulcskezelés modellek

Egyes hello kiszolgálóoldali titkosítás a többi modellek azt jelenti, hogy kulcskezelés megkülönböztető jellemzői. Ez magában foglalja a helyét és a titkosítási kulcsok létrehozása, és tárolja, csakúgy, mint hello hozzáférési modellek és kulcs Elforgatás eljárások hello. 

#### <a name="server-side-encryption-using-service-managed-keys"></a>Kiszolgálóoldali titkosítás szolgáltatással kezelt kulcsok

Sok felhasználónál hello alapvető követelmény, amely adatok hello tooensure titkosítva van, ha aktívan. Kiszolgálóoldali titkosítás szolgáltatás felügyelt kulcsok használata lehetővé teszi, hogy a Ez a modell lehetővé teszi az ügyfelek toomark hello megadott erőforrás (Tárfiók, SQL DB stb.) a titkosítás és kilépés a kulcsok kiadásához, elforgatás és biztonsági mentés minden kulcskezelés szempontjai tooMicrosoft. A legtöbb aktívan titkosítása általában támogató Azure-szolgáltatások márkája kiszervezésével hello titkosítási kulcsok tooAzure hello kezelését támogatja. hello Azure erőforrás-szolgáltató hello kulcsokat hoz létre, ki vannak téve a biztonságos, és olvassa be ezeket, ha szükséges. Ez azt jelenti, hogy hello szolgáltatás teljes toohello hívóbetűk és hello szolgáltatás hello hitelesítő adat életciklus-felügyeletének teljes körű vezérléssel rendelkezik.

![Felügyelt](./media/azure-security-encryption-atrest/azure-security-encryption-atrest-fig4.png)

Kiszolgálóoldali titkosítás segítségével felügyelt kulcsai ezért gyorsan címek hello kell toohave titkosítását alacsony általános toohello ügyféllel. Ha elérhető a ügyfél általában hello Azure-portál a hello célként megadott előfizetés- és erőforrás-szolgáltató megnyílik, és ellenőrzi, hogy egy titkosított hello adatok toobe szeretnének mezőben arról. Az egyes erőforrás-kezelők kiszolgálóoldali titkosítás szolgáltatással felügyelt kulcsoknál alapértelmezés szerint van bekapcsolva. 

A Microsoft által felügyelt kulcsok kiszolgálóoldali titkosítás hello szolgáltatás toostore teljes hozzáféréssel rendelkezik, és kezeli a hello kulcsokat utalnak. Egyes ügyfelek érdemes toomanage hello kulcsok, mert azok érzi, hogy akkor is a biztonság növelése érdekében, miközben hello költség, valamint olyan egyéni kulcs tárolási megoldást kockázatának figyelembe kell venni a modell kiértékelése során. Sok esetben egy szervezet dönthet, hogy erőforrás-korlátozások és a kockázatok egy helyszíni megoldás lehet, hogy nagyobb, mint a felhő management rest kulcsok: hello titkosítási hello kockázatát.  Azonban ez a modell lehet, hogy elegendő-e a szervezeteknek, amelyek követelmények toocontrol hello létrehozása vagy hello titkosítási kulcsok vagy toohave másik csoporthoz életciklusát hello szolgáltatás kezelése (azaz, mint az adott szolgáltatás titkosítási kulcsok kezelése a hello kulcskezelés elkülönítése átfogó felügyeleti modellt hello szolgáltatáshoz).

##### <a name="key-access"></a>Hozzáférés a kulcshoz

Szolgáltatás felügyelt kulccsal rendelkező kiszolgálóoldali titkosítás használata esetén hello kulcs létrehozása, tárolás és a szolgáltatás összes által felügyelt hello szolgáltatást. Hello eligazodást az Azure erőforrás-szolgáltatók általában tárolja a hello az adattitkosítási kulcsokat tárolóban, zárja be a toohello adatokat, és gyorsan elérhető-közben hello kulcs titkosítási kulcsok egy biztonságos belső tárolóban vannak tárolva.

**Előnyei**

- Egyszerű telepítés
- A Microsoft kezeli a kulcs elforgatás, a biztonsági mentés és a redundancia
- Ügyfél nincs társítva egy egyéni kulcskezelés séma megvalósítását vagy hello kockázatát hello.

**Hátrányok**

- Nem vevő szabályozhatják hello titkosítási kulcsok (kulcs jellemzője, életciklusát, visszavonás, stb.)
- Nincs lehetősége toosegregate kulcskezelés hello szolgáltatás általános felügyeleti modellből

#### <a name="server-side-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Felügyelt felhasználói kulcsok használata az Azure Key Vault kiszolgálóoldali titkosítás 

Az esetek, amikor hello tooencrypt hello inaktív adatok mérete, így vezérlő hello titkosítási kulcsokat az ügyfelek használhatják a kiszolgálóoldali titkosítás Key Vault ügyfél által felügyelt kulcsok használatát. Egyes szolgáltatások előfordulhat, hogy csak hello gyökér fő titkosítási kulcs tárolása az Azure Key Vault, valamint tároló hello egy belső szorosabb toohello helyadatok adattitkosítási kulcs titkosított. Az, hogy a forgatókönyv ügyfelek átvihetők a saját tooKey tárolóban (BYOK – Bring Your Own Key), a kulcsok vagy újakat létrehozni, és használhatják azokat tooencrypt hello szükséges erőforrásokat. Erőforrás-szolgáltató hello hello titkosítási és visszafejtési műveleteket hajtja végre, amíg azt hello konfigurált kulcs minden titkosítási művelet hello legfelső szintű kulcsot használja. 

##### <a name="key-access"></a>Hozzáférés a kulcshoz

hello Azure Key Vault felügyelt ügyfél kulcsokkal rendelkező kiszolgálóoldali titkosítás modell magában foglalja a hello szolgáltatás elérése során hello kulcsok tooencrypt és szükség szerint visszafejtése. Rest-kulcsok titkosítását elérhető tooa szolgáltatás egy hozzáférés-vezérlési házirend szolgáltatás identitásának hozzáférés tooreceive hello kulcs megadása keresztül történik. Az Azure-szolgáltatások egy társított előfizetés nevében futtató konfigurálható, hogy a szolgáltatás adott előfizetésen belül identitással. hello szolgáltatás Azure Active Directory-hitelesítést végezni, és kap egy hitelesítési jogkivonatot azonosító magát, hogy a szolgáltatás hello előfizetés nevében. A jogkivonat számára bemutatott tooKey tároló tooobtain egy kulcsot kell kapott az elérésére.

A titkosítási kulcsok használatával műveleteket, szolgáltatásidentitás kaphatnak hozzáférést tooany a következő műveletek hello: visszafejtés, a titkosítása, unwrapKey, wrapKey, győződjön meg arról, jelentkezzen, beolvasása, listában, frissítése, létrehozása, importálása, törlése, biztonsági mentése és visszaállítása.

tooobtain kulcs titkosítása vagy visszafejtése adatokat a többi hello szolgáltatásidentitás adott hello erőforrás-kezelő szolgáltatáspéldány fog futni, mert UnwrapKey (visszafejtési tooget hello kulcs) és WrapKey (tooinsert egy kulcsot a kulcstároló, ha új kulcs létrehozása) kell rendelkeznie.


>[!NOTE] 
>További információkhoz juthat a Key Vault engedélyezési tekintse meg a kulcstartót lapján hello biztonságos hello [dokumentáció az Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault). 

**Előnyei**

- Hello kulcsok teljes ellenőrzést használt – a titkosítási kulcsok kezelése az hello ügyfél Key Vault hello felhasználói vezérlése alatt.
- Képes tooencrypt több szolgáltatások tooone fő
- Átfogó felügyeleti modellből hello szolgáltatás kulcskezelést is elkülönítse
- Különböző régiókban a szolgáltatás és a kulcs helyének megadása is

**Hátrányok**

- Ügyfél van a kulcs kezelési vonatkozó teljes felelősség
- Ügyfél van a kulcs életciklusához kapcsolódó felügyeleti vonatkozó teljes felelősség
- A telepítő a & konfigurációs többletterhelést

#### <a name="server-side-encryption-using-service-managed-keys-in-customer-controlled-hardware"></a>Kiszolgálóoldali titkosítás szolgáltatással kezelt szabályozott ügyfélhardvereken kulcsok

Olyan esetekben, ahol hello követelmény inaktív tooencrypt hello adat és a jogvédett tárházban kívül a Microsoft hello kulcsok kezelése bizonyos Azure-szolgáltatások engedélyezése hello állomás a saját Key (HYOK) kulcskezelés modell. Ebben a modellben hello szolgáltatást kell lekérnie hello kulcs, külső webhelyről, és ezért a teljesítmény és rendelkezésre állás garanciák érintettek, és konfigurációs összetettebb. Ezenkívül mivel hello szolgáltatás rendelkezik hozzáférési toohello adattitkosítási kulcs során hello titkosítási és visszafejtési műveletek hello a modell átfogó biztonsági garanciákat olyan hasonló toowhen hello ügyfél felügyelete az Azure Key Vault kulcsai.  Ennek eredményeképpen a modell nincs a legtöbb szervezet számára megfelelő hacsak nem rendelkeznek, így szükségessé teszi az adott kulcskezelés követelmények. Toothese korlátozásai miatt legtöbb Azure-szolgáltatások nem támogatják a kiszolgáló által kezelt kulcsok használata a szabályozott ügyfélhardvereken kiszolgálóoldali titkosítás.

##### <a name="key-access"></a>Hozzáférés a kulcshoz

Felügyelt kulcsai használatával szabályozott ügyfélhardvereken kiszolgálóoldali titkosítás használata esetén hello kulcsok hello ügyfél által konfigurált rendszerek karbantartása. Azure-szolgáltatásokat, amelyek támogatják ezt a modellt biztosítanak egy eszközeit biztonságos kapcsolatot tooa ügyfél megadott kulcstároló.

**Előnyei**

- Hello legfelső szintű kulcs a faxrendszergazdáknak teljes hozzáférésük használt – titkosítási kulcsok egy megadott ügyfél-tároló által felügyelt
- Képes tooencrypt több szolgáltatások tooone fő
- Átfogó felügyeleti modellből hello szolgáltatás kulcskezelést is elkülönítse
- Különböző régiókban a szolgáltatás és a kulcs helyének megadása is

**Hátrányok**

- Kulcstároló, biztonság, teljesítmény és rendelkezésre állás vonatkozó teljes felelősség
- Hozzáférés a kulcshoz felügyeleti vonatkozó teljes felelősség
- Kulcs életciklus-felügyeletének vonatkozó teljes felelősség
- Jelentős telepítése, a konfiguráció és a folyamatos karbantartási költségek
- A hálózat rendelkezésre állásának hello ügyfél adatközpont és az Azure adatközpontjaiban között függőség nagyobb.

## <a name="encryption-at-rest-in-microsoft-cloud-services"></a>A Microsoft más felhőszolgáltatásaival aktívan titkosítása

A Microsoft Cloud services csomag használt összes modellt a felhőinfrastruktúra:, IaaS és PaaS, SaaS. Az alábbi példák ezek hogyan illeszkednek minden modellel rendelkezik:

- Szoftver szolgáltatások, a hivatkozott tooas kiszolgáló vagy SaaS, például az Office 365 hello felhő által biztosított alkalmazás rendelkező szoftver.
- Mely ügyfelek kihasználhatja az alkalmazásokban, hello felhő hello felhő használatával dolgot platformszolgáltatások például a tárolás, az elemzés és a szolgáltatási busz funkciót.
- Infrastruktúra-szolgáltatásokat, vagy infrastruktúrák (IaaS) melyik az ügyfelek központi telepítése az operációs rendszerek és alkalmazások, amelyek hello felhő- és esetleg használhatja más felhőalapú szolgáltatások.

### <a name="encryption-at-rest-for-saas-customers"></a>A Szolgáltatottszoftver-ügyfelek aktívan titkosítása

A szolgáltatott szoftverként (SaaS) ügyfél szoftver általában rendelkeznek titkosítás engedélyezve vagy nem érhető el minden egyes szolgáltatás inaktív. Az Office 365 szolgáltatások program számos ügyfél tooverify vagy engedélyezése titkosítási aktívan. Office 365-szolgáltatásokhoz kapcsolatos információkat lásd: adatok titkosítási technológiák az Office 365.

### <a name="encryption-at-rest-for-paas-customers"></a>A PaaS ügyfelek aktívan titkosítása

Platform (PaaS) szolgáltatás ügyfél adatként általában egy alkalmazás végrehajtási környezetben található, és bármely Azure erőforrás-szolgáltatók használt toostore ügyféladatokat. rest-beállítások elérhető tooyou toosee hello titkosítását vizsgálja meg az alábbi táblázatban hello hello tárolás és alkalmazást platformok, amelyekkel. Támogatott, ha mindegyik erőforrás-szolgáltató célokat szolgálnak hivatkozások tooinstructions engedélyezéséről titkosítását. 

### <a name="encryption-at-rest-for-iaas-customers"></a>Az infrastruktúra-szolgáltatási ügyfelek aktívan titkosítása

Szolgáltatott infrastruktúra (IaaS) szolgáltatás-ügyfelek különböző szolgáltatásokat és alkalmazásokat lehet használatban. IaaS-szolgáltatások engedélyezheti titkosítását az Azure-ban futó virtuális gépek és VHD-k használata az Azure Disk Encryption. 

#### <a name="encrypted-storage"></a>Titkosított tároló

A PaaS, például IaaS-megoldásokat más Azure-szolgáltatásokkal, inaktív adatok tárolására szolgáló használhatják fel. Ezekben az esetekben engedélyezheti hello titkosítási Rest terméktámogatási minden felhasznált Azure szolgáltatás által biztosított. hello az alábbi táblázat felsorolja a hello jelentős tárhely, szolgáltatások és alkalmazások platformok márkája és típusa hello támogatott titkosítását. Támogatott, ha hivatkozásokkal tooinstructions engedélyezéséről titkosítását. 

#### <a name="encrypted-compute"></a>Titkosított számítási

Rest-megoldást egy teljes titkosítás szükséges, hogy hello adatok soha nem megőrzött titkosítatlan formában. A használatát, a kiszolgáló betöltése hello adatok a memóriában, adatok helyben is őrizhető számos lehetőséget kínál, többek között a hello Windows lapozófájlja összeomlási memóriaképet és bármely naplózási hello alkalmazás előfordulhat, hogy hajtsa végre. Ezek az adatok titkosított tooensure aktívan IaaS-alkalmazások Azure Disk Encryption használhatja az Azure infrastruktúra-szolgáltatási virtuális gép (Windows vagy Linux) és a virtuális lemez. 

#### <a name="custom-encryption-at-rest"></a>Egyéni titkosítását

Javasoljuk, hogy amikor csak lehetséges, IaaS alkalmazások tudják hasznosítani Azure Disk Encryption és a titkosítás, felhasznált Azure-szolgáltatások által biztosított Rest-beállítások. Egyes esetekben például a szabálytalan titkosítási követelményeket vagy az Azure-alapú tárolási, a fejlesztők az infrastruktúra-szolgáltatási alkalmazás esetleg tooimplement titkosítását rest-magukat. IaaS-megoldások jobban fejlesztői integrálása az Azure felügyeleti és az ügyfél verziójával kapcsolatos elvárások, ami bizonyos Azure összetevők. Pontosabban segítségével hello Azure Key Vault szolgáltatás tooprovide biztonságos kulcs tárolása, valamint hogy következetes kulcskezelési beállításokat az Azure platform szolgáltatásaiból vele. Emellett egyéni megoldások Azure felügyelt szolgáltatás-identitások tooenable szolgáltatási fiókok tooaccess titkosítási kulcsokat használja. Az Azure Key Vault és a szolgáltatás-identitások felügyelt fejlesztői információkhoz lásd: a megfelelő SDK-k.

## <a name="azure-resource-providers-encryption-model-support"></a>Azure-erőforrás szolgáltatók titkosítás modell támogatása

Microsoft Azure-szolgáltatások minden támogatásához legalább egy rest-modellek hello titkosítását. Bizonyos szolgáltatások esetén azonban egy vagy több hello titkosítási modellek nem vonatkoznak. Emellett a szolgáltatások előfordulhat, hogy kibocsátási különböző ütemezéseket, ezek a forgatókönyvek támogatása. Ez a szakasz ismerteti az egyes hello fő az Azure data tárolószolgáltatások írásának időpontjában hello rest támogatását hello titkosítását.

### <a name="azure-disk-encryption"></a>Az Azure lemeztitkosítás

Minden ügyfél Azure infrastruktúra (IaaS) szolgáltatás használatával szolgáltatások érhető el az infrastruktúra-szolgáltatási virtuális gépeket és a lemezek keresztül Azure Disk Encryption titkosítását. További információ az Azure lemezen titkosítási: hello [Azure Disk Encryption dokumentáció](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

#### <a name="azure-storage"></a>Azure Storage

Az Azure Blob, és a fájl titkosítását támogatja a kiszolgálóoldali titkosított forgatókönyvek, valamint a titkosított adatok (ügyféloldali titkosítás).

- Kiszolgálóoldali: az Azure blob storage használatával az ügyfelek titkosítását az Azure storage erőforrás számlára engedélyezheti. Egyszer engedélyezett kiszolgálóoldali titkosítás átlátható módon toohello alkalmazás történik. Lásd: [Azure Storage szolgáltatás titkosítási inaktív adatok](https://docs.microsoft.com/azure/storage/storage-service-encryption) további információt.
- Ügyféloldali: az Azure BLOB ügyféloldali titkosítás használata támogatott. Ha használja az ügyféloldali titkosítás ügyfelek hello adatok titkosítására és hello adatait, mivel az egy titkosított blob feltöltése. Kulcskezelés hello ügyfél végezhető el. Lásd: [ügyféloldali titkosítás és a Microsoft Azure tárolás az Azure Key Vault](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) további információt.


#### <a name="sql-azure"></a>SQL Azure

Az SQL Azure jelenleg titkosítását a Microsoft által felügyelt szolgáltatás és az ügyféloldali titkosítás forgatókönyvek.

Támogatja a kiszolgáló jelenleg a titkosítást a transzparens adattitkosítás nevű hello SQL funkción keresztül biztosítja. Ha egy SQL Azure-ügyfelünkkel lehetővé teszi, hogy TDE kulcs automatikusan létrehozása és kezelése a számukra. Titkosítását hello adatbázis és a kiszolgáló szintjén engedélyezhető. Től június 2017 [átlátszó Data Encryption (TDE)](https://msdn.microsoft.com/library/bb934049.aspx) alapértelmezés szerint az újonnan létrehozott adatbázisok engedélyezve lesz.

Az SQL Azure adatok ügyféloldali titkosítás keresztül hello támogatott [mindig titkosítja](https://msdn.microsoft.com/library/mt163865.aspx) szolgáltatás. Mindig titkosított kulcsot használ, amely a létrehozott és tárolt hello ügyfél. Az ügyfelek hello főkulcs tárolhat Windows tanúsítványtárolót, az Azure Key Vault vagy helyi hardveres biztonsági modult. SQL Server Management Studio használatával, SQL felhasználók eldönthetik, hogy mi kulcs szeretnének toouse tooencrypt melyik oszlop.

|                                  |                |                     | **Titkosítási modell**             |                              |        |
|----------------------------------|----------------|---------------------|------------------------------|------------------------------|--------|
|                                  |                |                     |                              |                              | **Ügyfél** |
|                                  | **Kulcskezelés** | **Szolgáltatás felügyelt kulcs** | **A Key vaultban felügyelt ügyfél** | **Helyszíni felügyelt ügyfél** |        |
| **Tárolás és adatbázisok**            |                |                     |                              |                              |        |
| Lemez (IaaS)                      |                | -                   | Igen                          | Igen*                         | -      |
| SQL Server (IaaS)                |                | Igen                 | Igen                          | Igen                          | Igen    |
| Az SQL Azure (PaaS)                 |                | Igen                 | Előzetes verzió                      | -                            | Igen    |
| Az Azure Storage (blokk/Lapblobokat) |                | Igen                 | Előzetes verzió                      | -                            | Igen    |
| Az Azure Storage (fájlok)            |                | Igen                 | -                            | -                            | -      |
| Az Azure Storage (táblák, üzenetsorok)   |                | -                   | -                            | -                            | Igen    |
| A cosmos DB (dokumentum DB)          |                | Igen                 | -                            | -                            | -      |
| StorSimple                       |                | Igen                 | -                            | -                            | Igen    |
| Biztonsági mentés                           |                | -                   | -                            | -                            | Igen    |
| **Az eszközintelligencia és az elemzések**       |                |                     |                              |                              |        |
| Azure Data Factory               |                | Igen                 | -                            | -                            | -      |
| Azure Machine Learning           |                | -                   | Előzetes verzió                      | -                            | -      |
| Azure Stream Analytics           |                | Igen                 | -                            | -                            | -      |
| HDInsights (az Azure Blob-tároló)  |                | Igen                 | -                            | -                            | -      |
| HDInsights (Data Lake-tároló)   |                | Igen                 | -                            | -                            | -      |
| Azure Data Lake Store            |                | Igen                 | Igen                          | -                            | -      |
| Azure Data Catalog               |                | Igen                 | -                            | -                            | -      |
| Power BI                         |                | Igen                 | -                            | -                            | -      |
| **IoT-szolgáltatásaival**                     |                |                     |                              |                              |        |
| IoT Hub                          |                | -                   | -                            | -                            | Igen    |
| Service Bus                      |                | -              | -                            | -                            | Igen    |
| Event Hubs                       |                | -             | -                            | -                            | -      |


## <a name="conclusion"></a>Összegzés

Azure Services belül tárolt ügyféladatok védelméről kiemelkedő fontosságú tooMicrosoft verziója. Minden Azure üzemeltetett szolgáltatások véglegesített tooproviding titkosítási, Rest-beállítások. Például az Azure tárolás, az SQL Azure és a kulcs analytics eligazodást szolgáltatásokat és az eszközintelligencia már titkosítást biztosíthat, Rest-beállítások. Ezen szolgáltatások ellenőrzött felhasználói kulcsok és a ügyféloldali titkosítás támogatása, valamint a felügyelt kulcsok és a titkosítási szolgáltatás. Microsoft Azure-szolgáltatások vannak tágabb értelemben véve javítja a titkosítási többi rendelkezésre álló, illetve új beállítások előzetes és általánosan rendelkezésre álló tervezett hello jövőbeli hónapon belül.

