---
title: "Azure Active Directory Connect Health – gyakori kérdések - aaaAzure |} Microsoft Docs"
description: "Ez a GYIK az Azure AD Connect Health kapcsolatos kérdésekre ad választ. Ez a GYIK hello szolgáltatást, beleértve a számlázási modell, képességek, korlátozások és támogatás hello használatával kapcsolatos kérdéseket fedi le."
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 3f15d9e9b557b09ef74ceff85806579dd51f2412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Azure AD Connect Health – gyakori kérdések
A cikk a Microsoft Azure Active Directory (Azure AD) Connect Health kapcsolatban felmerülő kérdések (GYIK) válaszok toofrequently tartalmaz. Ezek gyakran ismételt kérdéseket fedik le a hogyan toouse hello szolgáltatást, amely magában foglalja a számlázási modell, képességek, korlátozások és támogatás hello kapcsolatos kérdésekre.

## <a name="general-questions"></a>Általános kérdések
**K: kezelhető több Azure AD-címtártól. Egy Azure Active Directory Premium rendelkező toohello átváltása?**

különböző közötti tooswitch az Azure AD bérlők, jelölje be hello jelenleg bejelentkezett **felhasználónév** a hello jobb felső sarkában, és kattintson a megfelelő fiók hello. Ha hello fiók itt nem látható, válassza ki a **Kijelentkezés**, és majd hello globális rendszergazdai hitelesítő adatok használata, amely rendelkezik az Azure Active Directory Premium hello könyvtár engedélyezve van a toosign.

**K: milyen identitás szerepkörök az Azure AD Connect Health által támogatott verzióját?**

hello következő táblázat hello szerepkörök és támogatott operációs rendszer verziója.

|Szerepkör| Operációs rendszer / verziója|
|--|--|
|Active Directory összevonási szolgáltatások (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | 1.0.9125 verzió vagy újabb|
|Active Directory tartományi szolgáltatások (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Vegye figyelembe, hogy hello szolgáltatás által biztosított hello funkciók eltérőek lehetnek a hello szerepkör és hello operációs rendszer alapján. Ez azt jelenti minden hello szolgáltatás nem érhetők el minden operációs rendszerekhez. Tekintse meg a részleteket hello a szolgáltatások leírása.

**K: hogyan hány licencet tegye a infrastruktúra kell toomonitor?**

* hello Connect Health-ügynök első legalább egy Azure AD Premium licenc szükséges.
* Minden további regisztrált ügynök 25 további Azure AD Premium licenc szükséges.
* Ügynökök száma az ügynökök közötti összes figyelt szerepkör (AD FS, az Azure AD Connect, és/vagy az AD DS) regisztrált egyenértékű toohello teljes száma.

Licencelési információkat is megtalálható hello [az Azure AD árazás lap](https://aka.ms/aadpricing).

Példa:

| Regisztrált ügynökök | Szükséges licencek | Példa figyelési konfiguráció |
| ------ | --------------- | --- |
| 1 | 1 | 1 az azure AD Connect-kiszolgáló |
| 2 | 26| Kiszolgáló. az azure AD Connect 1. és 1 tartományvezérlő |
| 3 | 51 | 1 active Directory összevonási szolgáltatások (AD FS) kiszolgálót, 1 AD FS proxy és 1 tartományvezérlő |
| 4 | 76 | 1 AD FS-kiszolgáló, az 1 AD FS proxy és a 2 tartományvezérlők |
| 5 | 101 | 1 az azure AD Connect-kiszolgáló, 1 AD FS-kiszolgáló, 1 AD FS proxy és 2 tartományvezérlők |


## <a name="installation-questions"></a>Telepítési kérdések

**K: Mi az a hello hatása hello Azure AD Connect Health-ügynök telepítése az egyes kiszolgálókon?**

hello Microsoft Azure AD Connect Health-ügynök, az AD FS, webalkalmazás-proxy kiszolgálók, az Azure AD Connect (sync) kiszolgálók, a tartományvezérlők telepítése hello hatása a tekintetben toohello Processzor, memória, hálózati sávszélességet és tárolóhelyet minimális.

a következő számokat hello közelítés:

* CPU-felhasználás: ~ 1-5 %-os növelését.
* Memória-felhasználás: hello teljes rendszermemória too10 %-át.

> [!NOTE]
> Ha hello ügynök nem tud kommunikálni az Azure-ral, hello ügynök eltárolja hello helyileg egy meghatározott maximális korlátot. hello ügynök hello "gyorsítótárazott" data "legrégebben kiszolgált" alapon felülírja.
>
>

* Helyi puffer tárhely az Azure AD Connect Health-ügynököket: ~ 20 MB-ot.
* Az AD FS-kiszolgáló azt javasoljuk, hogy mértékben hello AD FS naplózása csatornához tartozó Azure AD Connect Health-ügynököket tooprocess 1024 MB (1 GB) szabad lemezterület minden hello naplózási adatok előtt a rendszer felülírja azt.

**K: el kell tooreboot a kiszolgálók hello Azure AD Connect Health-ügynököket hello telepítése során?**

Nem. hello telepítési hello ügynökök nem követeli meg tooreboot hello kiszolgáló. Azonban néhány előfeltételként szükséges lépéseket a telepítés hello kiszolgáló újraindítás lehet szükség.

A Windows Server 2008 R2, például a .NET-keretrendszer 4.5-ös verziójának telepítése egy kiszolgáló újraindítása szükséges.

**K: az Azure AD Connect Health munkahelyi csatlakoztatott HTTP proxyn keresztül?**

Igen. A folyamatban lévő műveletek konfigurálhatja hello rendszerállapot-ügynöke toouse egy HTTP proxy tooforward kimenő HTTP-kérelmekre.
Tudjon meg többet az [Health-ügynököket HTTP-Proxy konfigurálása](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Ha a proxy tooconfigure az ügynök regisztrálása során, szükség lehet toomodify Internet Explorer proxybeállításainak előre.

1. Nyissa meg az Internet Explorert > **beállítások** > **Internetbeállítások** > **kapcsolatok** > **LAN-beállítások**.
2. Válassza ki **proxykiszolgálót használni a helyi hálózaton**.
3. Válassza ki **speciális** Ha különböző proxyhoz HTTP és HTTPS/Secure rendelkeznek.

**K: az Azure AD Connect Health támogatási egyszerű hitelesítés tooHTTP proxyk kapcsolódáskor?**

Nem. A mechanizmus toospecify egy tetszőleges felhasználónevet és jelszót az egyszerű hitelesítés jelenleg nem támogatott.

**K: tűzfalportok mire tooopen hello Azure AD Connect Health Agent toowork van szükség?**

Lásd: hello [követelményeket ismertető részben](active-directory-aadconnect-health-agent-install.md#requirements) tűzfalportokat és egyéb hálózati kapcsolati követelményeinek hello listáját.

**K: Miért látom azt hello hello Azure AD Connect Health portálon neve megegyezik a két kiszolgáló?**

Ha az ügynök eltávolítása a kiszolgálóról, hello kiszolgáló nem törlődik automatikusan hello Azure AD Connect Health portálon. Ha manuálisan az ügynök eltávolítása a kiszolgálóról, vagy távolítsa el a hello kiszolgálójának nevével, akkor toomanually delete hello server bejegyzés hello Azure AD Connect Health portálon.

Előfordulhat, hogy újból lemezképet létrehozni egy kiszolgálót, vagy egy új kiszolgáló létrehozása hello azonos részletei (például a számítógép nevét). Ha nem távolította el hello már regisztrált kiszolgáló hello Azure AD Connect Health portálon, és telepítette hello ügynök hello új kiszolgálón, két bejegyzés a hello láthatja ugyanazzal a névvel.

Ebben az esetben manuálisan törölje hello toohello régebbi verziójú kiszolgálón tartozik. a kiszolgáló hello adatok elavult kell lennie.

## <a name="health-agent-registration-and-data-freshness"></a>Health ügynök regisztrálása és az adatok frissesség

**K: milyen hello rendszerállapot-ügynöke eszközregisztrációs hibák leggyakoribb oka, és hogyan problémák elhárításához?**

hello rendszerállapot-ügynöke sikertelen lehet tooregister miatt toohello következő lehetséges okok:

* hello ügynök nem tud kommunikálni hello szükséges végpontok, mert egy tűzfal blokkolja a forgalmat. Ez a különösen közös a webalkalmazás-proxy kiszolgálók. Győződjön meg arról, hogy engedélyezte-e kimenő kommunikáció toohello szükséges végpontok és portok. Lásd: hello [követelményeket ismertető részben](active-directory-aadconnect-health-agent-install.md#requirements) részleteiről.
* Kimenő kommunikáció SSL-ellenőrzést végeztek tooan hello hálózati réteg. Ennek hatására a hello tanúsítvány hello ügynök hello hálózatfelügyeleti kiszolgáló/entitás helyébe toobe használja, és hello lépéseket toocomplete hello az ügynök regisztrálása sikertelen.
* hello felhasználónak nincs hozzáférési tooperform hello regisztrációs hello ügynök. Globális rendszergazdák alapértelmezés szerint rendelkezik hozzáféréssel. Használhat [szerepköralapú hozzáférés-vezérlés](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) toodelegate hozzáférés tooother felhasználók.

**K: I vagyok első arra figyelmezteti, hogy "Állapotfigyelő szolgáltatás adatokat nincs toodate be." Hogyan hello probléma elhárítása?**

Az Azure AD Connect Health hello riasztást állít elő, ha azt nem minden hello adatpontok kiszolgálóról fogadott hello hello az elmúlt két órában. Ez a riasztás több oka is lehet.

* hello ügynök nem tud kommunikálni hello szükséges végpontok, mert egy tűzfal blokkolja a forgalmat. Ez a különösen közös a webalkalmazás-proxy kiszolgálók. Győződjön meg arról, hogy engedélyezte-e kimenő kommunikáció toohello szükséges végpontok és portok. Lásd: hello [követelményeket ismertető részben](active-directory-aadconnect-health-agent-install.md#requirements) részleteiről.
* Kimenő kommunikáció SSL-ellenőrzést végeztek tooan hello hálózati réteg. Ennek hatására a hello ügynök hello hálózatfelügyeleti kiszolgáló/entitás helyébe toobe használja, és hello folyamat sikertelen lesz az Azure AD Connect Health toohello tooupload adatszolgáltatás hello tanúsítványt.
* Paranccsal hello kapcsolat hello ügynök beépített. [További információk](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service).
* hello ügynökök is támogatja a kimenő kapcsolat egy nem hitelesített HTTP-proxyn keresztül. [További információk](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy).

## <a name="operations-questions"></a>Műveletek kérdések
**K: kell naplózása a webalkalmazás-proxy kiszolgálók hello tooenable?**

Naplózás nem, nem kell engedélyezve a webalkalmazás-proxy kiszolgálók hello toobe.

**K: hogyan tegye beolvasása feloldani az Azure AD Connect Health-riasztások?**

Az Azure AD Connect Health-riasztások lekérése Megoldva a Kifogástalan állapot. Az Azure AD Connect Health-ügynököket észleli, és rendszeres időközönként jelentést hello sikerességi feltételek toohello szolgáltatás. Néhány riasztások hello tiltási időalapú. Ez azt jelenti Ha hello azonos hiba feltételek nem teljesülnek a riasztás előállítás 72 órán belül, az hello riasztás automatikusan megoldódik.

**K: I vagyok első arra figyelmezteti, hogy "vizsgálati hitelesítési kérelmet (szintetikus tranzakciónak) nem sikerült jogkivonatot tooobtain." Hogyan hello probléma elhárítása?**

Az Azure AD Connect Health AD FS ezt a riasztást állít elő, ha hello Health-ügynök telepítése az AD FS-kiszolgáló nem tud tooobtain jogkivonat hello rendszerállapot-ügynöke által indított szintetikus tranzakció részeként. hello rendszerállapot-ügynöke hello helyi rendszer környezetében használ, és megpróbál tooget jogkivonat self függő entitás számára. Ez az egy általános teszt tooensure, amely az AD FS jogkivonatok kiállításának állapotban van.

Általában a teszt sikertelen, mert hello rendszerállapot-ügynöke nem tooresolve hello AD FS farm name. Ez akkor fordulhat elő, ha egy hálózati egy terheléselosztó mögött hello AD FS-kiszolgálók és hello kérelem lekérdezi kezdeményezése csomópontot (ügyfélként megakadályozását tooa rendszeres, amely előtt hello terheléselosztó) hello terheléselosztó mögött van. Ez is kijavítva, hogy hello "gazdagép" fájl "C:\Windows\System32\drivers\etc" tooinclude hello kiszolgáló IP-címe az AD FS hello visszacsatolási IP- címének (127.0.0.1) alatt hello AD FS farm neve (például sts.contoso.com). Hozzáadását hello állomás fájlt fog testzárlat hello hálózati hívás, így a hello rendszerállapot-ügynöke tooget hello jogkivonat.

**K: jelent meg arról, hogy a gép nincs telepítve hello legutóbbi ransomeware támadások elleni e-mailt. Miért kapta meg az e-mailt?**

Az Azure AD Connect Health szolgáltatással tooensure hello szükséges javítások figyeli gépek telepített összes hello beolvasott. Ha legalább egy számítógép nem rendelkezett hello kritikus javításokat hello e-mail elküldése toohello a bérlői rendszergazdák. következő logika hello használt toomake lett ez a döntés.
1. Minden hello gyorsjavítás telepítve hello számítógépen található.
2. Ellenőrizze, hogy a hello gyorsjavítások hello közül legalább egy meghatározott listában megtalálható.
3. Ha igen, hello gép védett. Ha nem, hello gép hello támadás veszélyének.

Használhatja a következő PowerShell-parancsfájl tooperform hello ezt az ellenőrzést manuálisan. Hello fent logika valósítja meg.

```
Function CheckForMS17-010 ()
{
    $hotfixes = "KB3205409", "KB3210720", "KB3210721", "KB3212646", "KB3213986", "KB4012212", "KB4012213", "KB4012214", "KB4012215", "KB4012216", "KB4012217", "KB4012218", "KB4012220", "KB4012598", "KB4012606", "KB4013198", "KB4013389", "KB4013429", "KB4015217", "KB4015438", "KB4015546", "KB4015547", "KB4015548", "KB4015549", "KB4015550", "KB4015551", "KB4015552", "KB4015553", "KB4015554", "KB4016635", "KB4019213", "KB4019214", "KB4019215", "KB4019216", "KB4019263", "KB4019264", "KB4019472", "KB4015221", "KB4019474", "KB4015219", "KB4019473"

    #checks hello computer it's run on if any of hello listed hotfixes are present
    $hotfix = Get-HotFix -ComputerName $env:computername | Where-Object {$hotfixes -contains $_.HotfixID} | Select-Object -property "HotFixID"

    #confirms whether hotfix is found or not
    if (Get-HotFix | Where-Object {$hotfixes -contains $_.HotfixID})
    {
        "Found HotFix: " + $hotfix.HotFixID
    } else {
        "Didn't Find HotFix"
    }
}

CheckForMS17-010

```



## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Az Azure AD Connect Health-ügynök telepítése](active-directory-aadconnect-health-agent-install.md)
* [Az Azure AD Connect Health műveletei](active-directory-aadconnect-health-operations.md)
* [Az Azure AD Connect Health használata az AD FS szolgáltatással](active-directory-aadconnect-health-adfs.md)
* [Az Azure AD Connect Health szinkronizálási szolgáltatás használata](active-directory-aadconnect-health-sync.md)
* [Az Azure AD Connect Health használata az AD DS szolgáltatással](active-directory-aadconnect-health-adds.md)
* [Az Azure AD Connect Health verzióelőzményei](active-directory-aadconnect-health-version-history.md)
