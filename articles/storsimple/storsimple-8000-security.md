---
title: "aaaStorSimple 8000 sorozat biztonsági |} Microsoft Docs"
description: "Hello biztonsági és adatvédelmi funkciói, amely a StorSimple szolgáltatás, eszköz, és az adatok védelme a helyszíni és felhőben hello ismerteti."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 48dd449d2908c21fe05d0ed37a4dc6f3e306e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-security-and-data-protection"></a>StorSimple-biztonság és adatvédelem

## <a name="overview"></a>Áttekintés

A biztonság az bárki, aki bevezetni új technológia, különösen akkor, ha a bizalmas vagy szellemi tulajdont képező adatokat hello technológia használt fő szempont. Szerint értékeli a különböző technológiákkal, át kell gondolnia, nagyobb kockázattal és a data protection költségeket. Microsoft Azure StorSimple nyújt egy biztonsági és adatvédelmi megoldás a data protection tooensure segíti:

* **Bizalmas** – csak az arra jogosult személyek is megtekintheti az adatokat.
* **Integritás** – csak a jogosult személyek módosíthatja vagy törölheti az adatokat.

a Microsoft Azure StorSimple megoldáshoz hello kapcsolatba egymással négy fő összetevőből áll:

* **A Microsoft Azure StorSimple Device Manager szolgáltatott** – hello felügyeleti szolgáltatás tooconfigure és a kiépítés használatát hello StorSimple eszközt.
* **A StorSimple eszköz** – az adatközpontban telepített fizikai eszköz. Összes gazdagép és az ügyfelek, amely adatokat generál csatlakozás toohello StorSimple eszközt, és hello eszköz hello kezeli, és áthelyezi toohello Azure felhőben szükség szerint.
* **Ügyfelek/állomások toohello eszköz csatlakoztatva** – hello toohello StorSimple eszköz csatlakozó ügyfelek az infrastruktúrában, és hozhat létre, amelyet a védett toobe adatokat.
* **Felhőbeli tárhely** – hello hello Azure felhőben adatokat tároló helyét.

hello következő részek a hello StorSimple biztonsági szolgáltatások ezen összetevők és a rajtuk tárolt hello adatok védelme érdekében. Azt is, lehetséges, hogy a Microsoft Azure StorSimple biztonsági és hello válaszokról kapcsolatos kérdések listáját.

## <a name="storsimple-device-manager-service-protection"></a>StorSimple Device Manager szolgáltatás védelme

hello StorSimple Device Manager szolgáltatás a felügyeleti szolgáltatás a Microsoft Azure-ban üzemeltetett és toomanage használt összes, a szervezet közvetített StorSimple eszközre. Hello StorSimple Device Manager szolgáltatás a szervezeti hitelesítő adatok toolog toohello webböngészőn keresztül az Azure portál használatával végezheti el.

Hozzáférés toohello StorSimple Device Manager szolgáltatás megköveteli, hogy a szervezet Azure-előfizetéssel, amely tartalmazza a StorSimple. Az előfizetés hello Azure-portálon keresztül elérhető hello funkciókra szabályozza. Ha a szervezet nem rendelkezik Azure-előfizetéssel, és azt szeretné, hogy a velük kapcsolatos további toolearn, [regisztráció az Azure-bA szervezetként](../active-directory/sign-up-organization.md).

Mivel hello StorSimple Device Manager szolgáltatás az Azure-ban üzemel védi hello Azure biztonsági funkciók. A Microsoft Azure által biztosított hello biztonsági funkciókkal kapcsolatos további információkért lásd a toohello [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).

## <a name="storsimple-device-protection"></a>A StorSimple eszköz védelme

hello StorSimple eszköz azonban egy helyszíni hibrid tárolóeszköz, amely tartalmazza (SSD) és a merevlemezes (HDD) meghajtók, redundáns vezérlők és az automatikus feladatátvételi lehetőségeket. hello tartományvezérlők kezelése a tároló rétegezésével, helyezését jelenleg (vagy a használt kiemelt) adatokat (a StorSimple eszköz hello vagy a helyszíni kiszolgálók), a helyi tároló ritkábban használt adatok toohello felhő áthelyezés közben.

Kizárólag engedélyezett StorSimple eszközök engedélyezettek toojoin hello az Azure-előfizetéshez létrehozott StorSimple Device Manager szolgáltatást. tooauthorize egy eszközt, regisztrálnia kell azt a StorSimple eszköz Manager szolgáltatás hello hello Szolgáltatásregisztrációs kulcs megadásával. hello Szolgáltatásregisztrációs kulcs Azure-portálon hello hozott létre a 128 bites véletlenszerű kulcsot.

![Szolgáltatásregisztrációs kulcs](./media/storsimple-security/ServiceRegistrationKey.png)

toolearn hogyan elérni, a szolgáltatás regisztrációs kulcs, nyissa meg túl[2. lépés: hello Szolgáltatásregisztrációs kulcs lekérése](storsimple-8000-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).

hello szolgáltatás regisztrációs kulcsának, mint egy hosszú kulcs 100 + karaktereket tartalmaz. Hello kulcs másolása, és mentse a fájlt biztonságos helyen, így használhatja tooauthorize további eszközöket szükség szerint. Hello szolgáltatás regisztrációs kulcsának elvész, az első eszköz regisztrálása után, ha a StorSimple eszköz Manager szolgáltatás hello hozhat létre egy új kulcsot. Ez nem érinti a meglévő eszközök hello működését.

Eszköz regisztrálása után jogkivonatok toocommunicate használja a Microsoft Azure-ban. hello Szolgáltatásregisztrációs kulcs nem használatos eszköz regisztrálása után.

> [!NOTE]
> Azt javasoljuk, hogy minden használata után hello Szolgáltatásregisztrációs kulcs újragenerálása.


## <a name="protect-your-storsimple-solution-via-passwords"></a>A StorSimple megoldás a jelszavak védelme

Jelszavak sokan használnak, és olyan fontos szempontja, hogy a számítógép biztonsági hello StorSimple megoldásban toohelp győződjön meg arról, hogy az adatok csak az elérhető tooauthorized felhasználók. StorSimple lehetővé teszi a jelszavak a következő tooconfigure hello:

* A StorSimple eszköz rendszergazdai jelszava
* Ellenőrző kérdés kezdeményező és a cél jelszavak Handshake Authentication Protocol (CHAP)
* StorSimple Snapshot Manager jelszava

### <a name="windows-powershell-for-storsimple-and-hello-storsimple-device-administrator-password"></a>Windows PowerShell a StorSimple és hello StorSimple eszköz rendszergazdai jelszava

A Windows PowerShell-lel használható toomanage hello StorSimple eszközt a parancssorból. A Windows PowerShell-lel, amelyek lehetővé teszik tooregister lehetőséggel rendelkezik az eszköz, hálózati illesztő hello konfigurálása az eszközön, bizonyos típusú frissítések telepítése, az eszköz hibaelhárításához hello támogatási munkamenet elérésével és hello eszköz állapotának módosítása . A StorSimple Windows PowerShell csatlakozó toohello soros konzol hello eszköz vagy Windows PowerShell távoli eljáráshívás segítségével érheti el.

PowerShell távvezérlése HTTPS vagy HTTP keresztül végezhető. Ha engedélyezve van a Rendszerfelügyeleti webszolgáltatások HTTPS protokollon, szüksége lesz a toodownload hello távfelügyeleti tanúsítvány hello eszközről, és telepíteni hello távoli ügyfélhez. A PowerShell-távelérést kapcsolatos további információkért lásd az túl[távoli csatlakozás a StorSimple eszköz tooyour](storsimple-8000-remote-connect.md).

Miután tooconnect toohello eszközét a Windows PowerShell, szüksége lesz a tooprovide hello eszköz rendszergazdai jelszava toolog toohello eszközön.

![Az eszköz rendszergazdai jelszava](./media/storsimple-security/DeviceAdminPW.png)

Tartsa szem előtt ajánlott eljárások a következő hello:

* Távfelügyelet alapértelmezés szerint ki van kapcsolva. Hello StorSimple Device Manager szolgáltatás tooenable használhatja azt. Biztonsági szempontból ajánlott távelérési hello ténylegesen igényelt időszak során csak engedélyezni kell.
* Hello jelszó módosításakor kell meg arról, hogy toonotify összes távoli felhasználók úgy, hogy azok nem tapasztalnak egy váratlan kapcsolatok megszakadását.
* hello StorSimple Device Manager szolgáltatás nem tudja beolvasni a meglévő jelszavak: Ez csak alaphelyzetbe állíthatja őket. Azt javasoljuk, hogy minden jelszavak biztonságos helyen tárolja el, így nincs tooreset jelszó Ha elfelejti azt. Ha tooreset jelszót kell, csak meg arról, hogy toonotify minden felhasználó alaphelyzetbe.

Windows PowerShell-felületet hello egy soros kapcsolat toohello eszközzel végezheti el. Emellett azt távolról HTTP vagy HTTPS-t, amely további biztonsági használatával. HTTPS vagy egy soros, vagy a HTTP-kapcsolat-nál magasabb szintű biztonságot nyújt. Azonban toouse HTTPS, először telepítenie kell egy tanúsítványt, akik hozzáférhetnek a hello eszköz hello ügyfélszámítógépen. Hello eszköz konfigurációs lapján, a StorSimple eszköz Manager szolgáltatás hello hello távelérési tanúsítvány is letölthető. Távelérési hello tanúsítványa nem vesztek el, ha kell egy új tanúsítvány letöltése és terjesztése az engedélyezett toouse Távfelügyelet tooall-ügyfelek.

### <a name="challenge-handshake-authentication-protocol-chap-initiator-and-target-passwords"></a>Ellenőrző kérdés kezdeményező és a cél jelszavak Handshake Authentication Protocol (CHAP)

A CHAP egy hitelesítési séma hello StorSimple eszköz toovalidate hello identitásának távoli ügyfelek által használt protokoll. hello ellenőrzési megosztott jelszó alapul. Lehet, hogy a CHAP egyirányú (egyirányú) vagy a kölcsönös (kétirányú). Az egyirányú CHAP hello cél (hello StorSimple eszköz) hitelesíti a egy kezdeményező (gazda). Kölcsönös vagy fordított CHAP a hello cél hello kezdeményező hitelesítéséhez, és hello kezdeményező hitelesítse hello cél használatához. A StorSimple lehet konfigurált toouse mindkét módszer.

Vegye figyelembe a következőket hello CHAP konfigurálásakor:

* hello CHAP-felhasználónév kevesebb mint 233 karaktert kell tartalmaznia.
* hello CHAP-jelszó 12 és 16 karakter hosszúságú lehet. Kísérlet toouse hosszabb felhasználónevet vagy jelszót eredményezi egy sikertelen hitelesítési kísérlet, a Windows hello-állomás.
* Hello nem használhatja ugyanazt a jelszót a hello CHAP-kezdeményező és a hello CHAP céljának.
* Hello jelszó beállítása után ez módosítható, de az nem olvasható. Hello jelszót módosítják, ha kell, hogy toonotify minden távelérési felhasználók, hogy sikeresen csatlakozhassanak toohello StorSimple eszköz.

További információ a CHAP, és hogyan tooconfigure azt a StorSimple megoldásban túl[CHAP konfigurálása a StorSimple eszköz](storsimple-8000-configure-chap.md).

### <a name="storsimple-snapshot-manager-password"></a>StorSimple Snapshot Manager jelszava

StorSimple Snapshot Manager egy Microsoft Management Console (MMC) beépülő modulja által használt kötet csoportok és hello Windows a kötet árnyékmásolata szolgáltatás toogenerate alkalmazáskonzisztens biztonsági mentéseket. Ezenkívül használja a StorSimple Snapshot Manager toocreate biztonsági mentési ütemezés és a Klónozás vagy kötetek visszaállítása.

Egy eszköz StorSimple Snapshot Manager toouse konfigurálásakor fogja szükséges tooprovide hello StorSimple Snapshot Manager jelszavát. Ez a jelszó először állítja be a Windows PowerShell StorSimple a regisztráció során. hello jelszó is beállítása és hello StorSimple Device Manager szolgáltatás megváltozott. Ez a jelszó hello eszköz StorSimple Snapshot Manager hitelesíti.

![StorSimple Snapshot Manager jelszava](./media/storsimple-security/SnapshotMgrPassword.png)

hello StorSimple Snapshot Manager jelszava 14 too15 karakterből kell állnia, és 3 vagy több nagybetű, nagybetűk, numerikus és speciális karakterek kombinációjából kell tartalmaznia. Hello StorSimple Snapshot Manager jelszavának beállítása után ez módosítható, de az nem olvasható. Hello jelszó módosítása, lehet, hogy toonotify összes távoli felhasználót.

StorSimple Snapshot Manager kapcsolatos további információkért lépjen túl[Mi az StorSimple Snapshot Manager?](storsimple-what-is-snapshot-manager.md)

### <a name="password-best-practices"></a>Jelszó gyakorlati tanácsok

Azt javasoljuk, hogy használja-e hello következő iránymutatásokat toohelp gondoskodjon arról, hogy a StorSimple-jelszavak erős és jól védett:

* A jelszavak módosítása háromhavonta. Hello jelszavak módosítása a évente érvényesül.
* Erős jelszavakat használjon. További információ: túl[erősebb jelszavak létrehozása és védelmük](http://blogs.microsoft.com/cybertrust/2014/08/25/create-stronger-passwords-and-protect-them/).
* Mindig használjon különböző jelszót eltérő hozzáférési mechanizmusok; minden egyes megadott hello jelszavak egyedinek kell lennie.
* Ne ossza meg jelszavak bárki, aki nem jogosult tooaccess hello StorSimple eszközt.
* Beszéd kapcsolatos elé más jelszót, vagy ne hello formátum jelszó mutatót.
* Ha azt gyanítja, hogy egy fiók vagy jelszó feltörték, jelentést hello incidens tooyour információk biztonsági részleg.
* Kezelje a jelszó-és nagybetűket, bizalmas információkat. 

## <a name="storsimple-data-protection"></a>StorSimple-adatok védelme

Ez a szakasz hello StorSimple biztonsági szolgáltatásait ismerteti, az átvitel során az adatok és a tárolt adatok védelme.

További fejezeteiben bővebben ismertetjük, jelszavak használt tooauthorize és ahhoz, hogy hozzáférési tooyour StorSimple megoldás a felhasználók hitelesítésére. Egy másik biztonsági szempont védi az adatokat a jogosulatlan felhasználóktól tárolási rendszerek között, és közben az adatok tárolása alatt átvitel közben. hello következő részek a adatbiztonsági funkciók hello StorSimple biztosított.

> [!NOTE]
> A deduplikáció és a Microsoft Azure Storage hello StorSimple eszközön tárolt adatok további védelmet biztosít. Ha a deduplikált adatok, hello adatobjektumainak külön-külön tárolódnak a hello használt metaadatok toomap, és a hozzáférésüket: nincsenek elérhető tárhely szintű környezet tooreconstruct hello adatok kötetek szerkezete, a fájlrendszer vagy a fájl neve alapján.


## <a name="protect-data-flowing-through-hello-service"></a>Hello szolgáltatáson keresztül áramló adatok védelme

hello elsődleges célja, hogy a StorSimple Device Manager szolgáltatás hello toomanage és hello StorSimple eszköz konfigurálása. hello StorSimple Device Manager szolgáltatás fut a Microsoft Azure-ban. Hello Azure portál tooenter eszköz konfigurációs adatokat használ, majd a Microsoft Azure hello StorSimple Device Manager szolgáltatás toosend hello adatok toohello eszközt használ. StorSimple használ aszimmetrikus kulcspárokat toohelp rendszert győződjön meg arról, hogy hello Azure szolgáltatás-biztonság sérülését nem eredményez-biztonság sérülését tárolt adatokat.

![Felé továbbított adatok titkosítása](./media/storsimple-security/DataEncryption.png)

hello aszimmetrikus kulcs rendszer megvédi hello adatok áthaladó hello szolgáltatást az alábbiak szerint:

1. Egy aszimmetrikus nyilvános és titkos kulcsból álló kulcspárt használó adattitkosítási tanúsítványának hello eszközön jön létre, és a használt tooprotect hello adat. hello kulcsok jönnek létre, amikor regisztrál hello első eszközt.
2. hello adattitkosítási tanúsítványt kulcsokat hello szolgáltatásadat-titkosítási kulcs, amely hello első eszköz regisztrálása során véletlenszerűen létrehozott erős 128 bites kulccsal védett személyes információcsere (.pfx) fájlba exportált.
3. hello hello tanúsítvány nyilvános kulcsa biztonságosan történik meg a rendelkezésre álló toohello StorSimple Device Manager szolgáltatás, és titkos kulcs hello hello eszköz marad.
4. Belépés hello szolgáltatás használatával titkosított adatok hello nyilvános kulcsot, és visszafejteni a hello hello eszközön tárolt titkos kulcsot, győződjön meg arról, hogy hello Azure-szolgáltatás nem tudja visszafejteni toohello eszköz áramló hello adatokat.

szolgáltatásadat-titkosítási kulcs hello eszközön csak hello első hello szolgáltatásban regisztrált jön létre. Hello szolgáltatásban regisztrált összes további eszközöket kell használnia a hello azonos szolgáltatásadat-titkosítási kulcs.

> [!IMPORTANT]
> Ez nagyon fontos toomake hello szolgáltatásadat-titkosítási kulcs másolatát, és mentse egy biztonságos helyre. Hello szolgáltatásadat-titkosítási kulcs biztonsági másolatának oly módon, hogy elérhetők, meghatalmazott, és lehet könnyen közölt toohello eszköz-rendszergazdai kell tárolni.
> 
> Hello szolgáltatásadat-titkosítási kulcs nem vesztek el, ha a Microsoft támogatási szolgálatnak segítségével tooretrieve, feltéve, hogy rendelkezik-e legalább egy eszköz online állapotban. Ajánlott hello szolgáltatásadat-titkosítási kulcs módosítása után lekéri azt. Útmutatásért lépjen túl[módosítás hello szolgáltatásadat-titkosítási kulcs](storsimple-service-dashboard.md#change-the-service-data-encryption-key).

Hello választásával módosíthatja hello szolgáltatásadat-titkosítási kulcs és hello megfelelő adattitkosítási tanúsítványának **módosítás szolgáltatásadat-titkosítási kulcs** hello szolgáltatás irányítópulton lehetőséget. adatok biztonsági ne legyenek veszélyben tooensure, a fizikai StorSimple eszköz toochange hello szolgáltatásadat-titkosítási kulcs kell használnia. Hello titkosítási kulcsok módosításához szükséges, hogy minden eszköz frissíteni kell-e hello új kulccsal. Ezért azt javasoljuk, hogy módosítsa hello kulcs, minden eszköz online állapotba kerülnek. Kapcsolat nélküli eszközök esetén a kulcsok egy másik időpontban lehet megváltoztatni. elavult kulcsok hello eszközök továbbra is képes toorun biztonsági mentések, de nem fogja tudni toorestore adatok amíg hello kulcsa nem frissül. További információ: túl[használata hello StorSimple Device Manager szolgáltatás irányítópultját](storsimple-8000-service-dashboard.md).

szolgáltatásadat-titkosítási kulcs hello és hello adatok titkosítási tanúsítvány nem jár le. Javasoljuk azonban, hogy módosítsa a hello szolgáltatásadat-titkosítási kulcs évente toohelp megakadályozása kulcs biztonsági sérülése.

## <a name="protect-data-at-rest"></a>Adatok inaktív védelme

hello StorSimple eszközön tárolja őket rétegek helyileg és hello felhőben, attól függően, hogy a használat gyakorisága kezeli az adatokat. Az összes gazdagép gépeket, amelyek csatlakoztatott toohello eszköz send data toohello eszközét, ezzel adatok toohello felhő, szükség szerint helyezi. Adatátvitel hello eszköz toohello felhőből biztonságosan hello interneten keresztül. Minden eszközhöz tartozik egy iSCSI-tároló, amely megjeleníti az összes megosztott kötetek azokon az eszközökön. Összes adata titkosításra kerül toocloud tárolási elküldés előtt. 

![Felhőalapú tárolás titkosítási kulcsa](./media/storsimple-security/CloudStorageEncryption.png)

toohelp hello biztonsági és az adatok sértetlenségének toohello felhő áthelyezése érdekében a StorSimple lehetővé teszi a toodefine felhőalapú tárolás titkosítási kulcsokat az alábbiak szerint:

* A kötettároló létrehozásakor megadhatja hello felhőalapú tárolás titkosítási kulcsát. hello kulcs nem módosítható, vagy később.
* Egy kötet tároló megosztáson található összes kötetet hello megegyező titkosítási kulccsal. Ha azt szeretné, hogy egy másik formája, amelyet egy adott kötet titkosítási, azt javasoljuk, hogy hozzon létre egy új kötetet tároló toohost, hogy a köteten.
* Amikor a StorSimple eszköz Manager szolgáltatás hello hello felhőalapú tárolás titkosítási kulcsa, hello hello szolgáltatásadat-titkosítási kulcs nyilvános részének használatával, és elküldheti a toohello eszköz hello kulcs titkosított.
* hello felhőalapú tárolás titkosítási kulcsa nem tárolódik a hello szolgáltatásban, és csak toohello eszköz ismert.
* A felhőalapú tárolás titkosítási kulcsát megadása nem kötelező megadni. Hello állomás toohello eszközön titkosított adatokat küldhet.

### <a name="additional-security-best-practices"></a>Ajánlott biztonsági eljárások

* Ossza fel a forgalom: különítse el a iSCSI SAN egy vállalati helyi hálózaton felhasználói forgalomnak a telepítése egy teljesen elkülönített hálózatot és VLAN-ok használatával, ahol fizikai elkülönítési lehetőség nem érhető el. Az iSCSI-tárolóhoz egy dedikált hálózaton hello biztonságát és az üzleti szempontból kritikus fontosságú adatok teljesítményt garantálja. Tárolás és a felhasználói forgalom keverése egy vállalati helyi hálózaton keresztül nem ajánlott és is növelheti a késés és hálózati hibákhoz vezethet.
* Gazdagép-oldali hálózati biztonság érdekében a TCP/IP-kiszervezés motor (TOE) támogató hálózati felületek használatával. TOE csökkenti a CPU-terhelés TCP feldolgozásával hello hálózati adapteren.

## <a name="protect-data-via-storage-accounts"></a>Storage-fiókok keresztül az adatok védelme

Minden Microsoft Azure-előfizetés egy vagy több storage-fiókokat hozhat létre. (A storage-fiók egy egyedi névteret biztosít hello Azure felhőben tárolt adatok.) Hozzáférés tooa tárfiók hello előfizetés és a hozzáférési kulcsok tárolási fiókhoz hozzárendelt vezérli.

Amikor létrehoz egy tárfiókot, a Microsoft Azure két 512 bites tárelérési kulcsot, amelyek közül az egyik hitelesítéshez használt amikor hello StorSimple eszköz hello tárfiók ér el állít elő. Vegye figyelembe, hogy ezek a kulcsok csak az egyik használatban van. hello más kulcs használatban van-tartalék értékét, akkor toorotate hello kulcsok rendszeres időközönként engedélyezése. hello aktív másodlagos kulcsát, majd törölje hello elsődleges kulcs elvégezte toorotate kulcsok. Ezután létrehozhat új kulcs hello következő Elforgatás során. (Biztonsági okokból sok adatközpontokban van szükség kulcs elforgatás.)

Azt javasoljuk, hogy kövesse az alábbi gyakorlati tanácsok a kulcs elforgatás:

* Tárfiókkulcsok rendszeresen toohelp győződjön meg arról, hogy a tárfiók jogosulatlan felhasználók nem férhetnek hozzá kell elforgatása.
* Rendszeres időközönként az Azure rendszergazdai kell módosítani, vagy hello elsődleges vagy másodlagos kulcs újragenerálása hello tárolási szakasza hello Azure portál toodirectly hozzáférés hello storage-fiók használatával.

## <a name="protect-data-via-encryption"></a>Titkosítási keresztül az adatok védelme

StorSimple használja a következő tooprotect adatokat tárolja a titkosítási algoritmusok, vagy a StorSimple megoldásban hello összetevői közötti utazás hello.

| Algoritmus | Kulcshossz | Protokollok/applications/megjegyzések |
| --- | --- | --- |
| RSA |2048 |Hello Azure portál tooencrypt konfigurációs adatokat küldött toohello eszköz RSA 1-PKCS 1.5-ös verzióját használja: például a tárolási fiók hitelesítő adatait, a StorSimple eszköz konfigurációs, és a felhőalapú tárolás titkosítási kulcsokat. |
| AES |256 |A CBC AES használt tooencrypt hello nyilvános részében hello szolgáltatásadat-titkosítási kulcs, toohello Azure-portálon hello StorSimple eszközön történő továbbítás előtt. Azt is használják hello StorSimple eszköz tooencrypt adatok toohello a felhőalapú társzolgáltatás fiókja hello adatok elküldése előtt. |

## <a name="storsimple-cloud-appliance-security"></a>StorSimple felhő készülék biztonsági

[!INCLUDE [storsimple Cloud Appliance security](../../includes/storsimple-virtual-device-security.md)]

## <a name="frequently-asked-questions-faq"></a>Gyakori kérdések (GYIK)

hello az alábbiakban néhány kapcsolatos kérdések és válaszok biztonsági és a Microsoft Azure StorSimple.

**K:** a szolgáltatás biztonsága sérül. Milyen kell összeállításának következő lépései?

**V:** hello szolgáltatásadat-titkosítási kulcs és hello tárfiókkulcsok rétegezési adatok használt tárfiók hello azonnal meg kell változtatni. Útmutatásért tekintse:

* [Szolgáltatásadat-titkosítási kulcs hello módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
* [Kulcs elforgatási szögét storage-fiókok](storsimple-8000-manage-storage-accounts.md#key-rotation-of-storage-accounts)

**K:** új StorSimple eszköz, amely a kért hello szolgáltatás regisztrációs kulcsának. Hogyan azt lekérése?

**V:** ezt a kulcsot jött létre, amikor először létrehozott hello StorSimple Device Manager szolgáltatást. Ha hello StorSimple Device Manager szolgáltatás tooconnect toohello eszközzel, hello szolgáltatás – első lépések lap tooview vagy Szolgáltatásregisztrációs kulcs újragenerálása hello is használhatja. Egy új szolgáltatás regisztrációs kulcs hello meglévő regisztrált eszközöket nem érinti. Útmutatásért tekintse:

* [Megtekintése vagy hello Szolgáltatásregisztrációs kulcs újragenerálása](storsimple-8000-manage-service.md##regenerate-the-service-registration-key)

**K:** a szolgáltatásadat-titkosítási kulcs elvész. Mit tegyek?

**V:** forduljon a Microsoft támogatási szolgálatához. Bejelentkezés tooa támogatási munkamenet az eszközön és a Súgó hello kulcs beolvasása az (feltéve, hogy legalább egy eszköz online állapotban). Miután beszerezte hello szolgáltatásadat-titkosítási kulcs, akkor kell megváltoztatnia tooensure azonnal hello új kulcs csak tooyou ismert. Útmutatásért tekintse:

* [Szolgáltatásadat-titkosítási kulcs hello módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key)

**K:** szeretnék a szolgáltatás titkosítási kulcs változását eszköz engedélyezett, de hello kulcsváltozás folyamat nem indult el. Mit tegyek?

**V:** hello időkorlát lejárt, ha lesz szüksége tooreauthorize hello eszköz hello szolgáltatás titkosítási kulcs változását és hello folyamat elindításához ismét.

**K:** módosítottam hello szolgáltatásadat-titkosítási kulcs, de nem sikerült tooupdate más eszközök hello 4 órán belül. Van toostart újra?

**V:** hello 4 órás időszak az az csak a kezdeményező hello módosítása. A hello frissítési folyamat megkezdése után hello StorSimple eszköz engedélyezett, hello engedélyezési érvényes, amíg minden eszköz nem frissítették.

**K:** a StorSimple rendszergazda hello vállalati kilépett. Mit tegyek?

**V:** és -visszaállítás hello engedélyezése hozzáférés toohello StorSimple eszközön, és módosítsa a hello szolgáltatás adatok titkosítási kulcs tooensure, hogy hello új adatokat nem ismert toounauthorized személyzet jelszavakat. Útmutatásért tekintse:

* [Hello StorSimple Device Manager szolgáltatás toochange a storsimple-jelszavak használata](storsimple-8000-change-passwords.md)
* [Szolgáltatásadat-titkosítási kulcs hello módosítása](storsimple-service-dashboard.md#change-the-service-data-encryption-key)
* [A CHAP konfigurálása a StorSimple eszköz](storsimple-8000-configure-chap.md)

**K:** szeretném tooprovide hello StorSimple Snapshot Manager jelszavát tooa gazdagépre, amely toohello StorSimple eszköz kapcsolódik, de hello jelszó nem érhető el. Mi a teendő?

**V:** Ha elfelejtette hello jelszavát, akkor hozzon létre egy új. Ezt követően lehet meg arról, hogy az összes meglévő felhasználót, hogy a jelszó hello megváltozott tooinform és, hogy azok frissítenie kell az ügyfelek toouse hello új jelszót. Útmutatásért tekintse:

* [Hello StorSimple Snapshot Manager jelszavának módosítása](storsimple-8000-change-passwords.md#set-the-storsimple-snapshot-manager-password)
* [Egy eszköz hitelesítéséhez](storsimple-snapshot-manager-manage-devices.md#authenticate-a-device)

**K:** távelérési toohello Windows PowerShell-lel hello tanúsítványa megváltozott hello eszközön. Hogyan frissíthetők a távelérési ügyfelek?

**V:** hello új tanúsítvány letöltését hello StorSimple Device Manager szolgáltatás, és telepítve hello tanúsítványtárolójába a távoli ügyfelek toobe biztosítanak. Útmutatásért tekintse:

* [Tanúsítvány importálása parancsmag](https://technet.microsoft.com/library/hh848630.aspx)

**K:** az adatok, ha hello StorSimple Device Manager szolgáltatás sérült védett?

**V:** szolgáltatás konfigurációs mindig adattitkosítás a nyilvános kulccsal webböngészőben megtekintésekor. Hello szolgáltatás nem rendelkezik hozzáférési toohello titkos kulcsot, mert a hello szolgáltatás nem lesz képes toosee adatot kell. Ha hello StorSimple Device Manager szolgáltatás biztonsága sérül, akkor ennek nincs hatása, mivel nincsenek hello StorSimple Device Manager szolgáltatás tárolt kulcsok.

**K:** valaki kap hozzáférést toohello adatok titkosítási tanúsítványt, ha rendszer adataimat megsértik?

**V:** Microsoft Azure titkosított formátumban tárolja a hello ügyfél adattitkosítási kulcsot (.pfx fájlt). Hello .pfx fájl titkosítva van, és hello StorSimple szolgáltatás nem rendelkezik hello szolgáltatás adatok titkosítási kulcs toodecrypt hello .pfx fájlt, mert egyszerűen első access toohello .pfx fájlt fog nem teszi közzé a titkos kulcsok.

**K:** mi történik, ha egy kormányzati entitás Microsoft kéri az adataimat?

**V:** hello adatok titkosított hello szolgáltatásban, mert a titkos kulcs hello tartják hello eszközzel hello kormányzati entitás hello adatok hello ügyfél kell kérnie.

## <a name="next-steps"></a>Következő lépések

[A StorSimple eszköz üzembe helyezése](storsimple-8000-deployment-walkthrough-u2.md).

