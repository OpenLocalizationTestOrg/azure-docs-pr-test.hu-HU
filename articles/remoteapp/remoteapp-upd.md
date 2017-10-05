---
title: "Hogyan menteni a Azure RemoteApp a felhasználói adatok és beállítások? | Microsoft Docs"
description: "Ismerje meg, hogyan menti az Azure RemoteApp a felhasználói adatok a felhasználói profil lemez használatával."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: e125af62-5c1a-4186-a238-52f537f7bb34
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 4993bf507536659d7951e1552559cf0a6061d345
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Hogyan menteni a Azure RemoteApp a felhasználói adatok és beállítások?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Az Azure RemoteApp menti a felhasználói azonosító és a testreszabások eszközök és munkamenet között. Felhasználói adatok felhasználónkénti gyűjteményre lemez, a felhasználói profil lemezre (UPD) néven ismert tárolja. A lemez a felhasználó a következő, és biztosítja, hogy a felhasználó rendelkezik-e egy egységes felhasználói élményt, ahol bejelentkeznek az függetlenül.

Felhasználói profillemezek rendszer teljesen átlátható a felhasználó – felhasználói menti az dokumentumok a dokumentumok mappát (Mi a úgy tűnik, hogy egy helyi meghajtó), és módosítsa a alkalmazás beállításait, mint a szokásos. Egy időben az összes személyes beállítások megmaradnak, bármilyen eszközről Azure RemoteApp történő csatlakozás során. A felhasználónál ugyanazon a helyen adataikat.

Minden egyes UPD állandó tároló 50GB rendelkezik, és mindkét felhasználói adatok és alkalmazások beállításait tartalmazza. 

A részletekért a felhasználói profil adatainak olvasás.

> [!NOTE]
> Le kell tiltania a UPD? Megteheti, hogy most - Pavithra által írt blogbejegyzés, kivételét [tiltsa le a felhasználói Profillemezek (felhasználóiprofil) az Azure Remoteappban](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), a részletekért.
> 
> 

## <a name="how-can-an-admin-get-to-the-data"></a>Hogyan férhetnek a rendszergazda az adatokhoz?
Egy a felhasználókat (a vész-helyreállítási, vagy ha a felhasználó elhagyja a céget) adatok eléréséhez szükséges, ha Azure ügyfélszolgálatához, és adja meg az előfizetési adatok gyűjtésére és a felhasználói identitás. Az Azure RemoteApp csapatától biztosítja azokat a virtuális merevlemez URL-CÍMÉT. Töltse le a virtuális merevlemez, és a dokumentumok és fájlok kell beolvasni. Vegye figyelembe, hogy a virtuális merevlemez 50 GB-os, így jelző bit annak letöltését.

## <a name="is-the-data-backed-up"></a>Az adatok biztonsági mentése?
Igen, azt a biztonsági másolatot a felhasználói adatok egy földrajzi helyet. Az adatok csak olvasható, és ugyanolyan módon is elérhetők, a rendszeres adatgyűjtési (lépjen kapcsolatba az Azure RemoteApp Letöltés helye) lenne, ha az elsődleges adatközpont leáll. Az adatokat a biztonsági mentési helyre valós idejű másolja, és azt nem példányaira különböző verziói. Igen adatsérülést, azt nem lesz visszaállítható korábban ismert helyes verzióra, de az elsődleges adatközpont leáll, ha nem kérhető le a felhasználói adatok a másik helyen.

## <a name="how-do-users-see-the-upd-on-the-server-side"></a>Hogyan felhasználók látnak az UPD kiszolgálóoldali?
Minden felhasználó saját kell a kiszolgálón, amely a UPD van leképezve: c:\Users\username.

## <a name="whats-the-best-way-to-use-outlook-and-upd"></a>Mi az a legjobb módszer az Outlook és UPD?
Az Azure RemoteApp menti az Outlook állapotát (postaládák, pst) munkamenetek között. Ennek engedélyezéséhez igazolnia kell a csendes-óceáni TÉLI tárolja a felhasználói profil adatait (c:\users\<felhasználónév >). Ez az alapértelmezett hely a adatok esetén, amennyiben nem módosítja a helyet, az adatok megmaradnak a munkamenetek között.

Javasoljuk továbbá, az Outlook használja a "gyorsítótárazott" és "server/online" üzemmódban használja a keresés.

Tekintse meg [Ez a cikk](remoteapp-outlook.md) vonatkozó további információ az Outlook és az Azure RemoteApp használatával.

## <a name="what-about-redirection"></a>Mi a helyzet átirányítása?
Beállíthatja, hogy a felhasználók férhetnek hozzá a helyi eszközök beállítása az Azure RemoteApp [átirányítás](remoteapp-redirection.md). Helyi eszközök érhessék el az adatokat a UPD lesz.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Használható hálózati megosztásnak a UPD?
Nem. Felhasználóiprofil nem használható egy hálózati megosztásra. Egy UPD metódus csak a felhasználó számára elérhető, ha a felhasználó aktívan kapcsolódik az Azure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Ha egy felhasználó egy gyűjteményből, azok UPD törölve van?
Nem, a felhasználó töröl, a Microsoft automatikusan törli a UPD - ehelyett tároljuk az adatokat mindaddig, amíg a gyűjtemény törlése. a gyűjtemény törlése után 90 nappal azt minden felhasználóiprofil törlése. 

Ha egy UPD törölje gyűjteménye, lépjen kapcsolatba az Azure RemoteApp - kell azt az ügyféloldali UPD törli.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Hozzáférhet a felhasználók felhasználóiprofil (vagy a jelenlegi, vagy a törölt felhasználók)?
Igen, ha kapcsolatba lép a [Azure RemoteApp](mailto:remoteappforum@microsoft.com), azt állíthat be, egy URL-cím, az adatok eléréséhez. Körülbelül 10 órát kell letöltését adatokat és fájlokat a UPD járjon le a hozzáférési.

## <a name="are-upds-available-offline"></a>Elérhetők a felhasználóiprofil offline?
Most azt nem offline hozzáférést nyújtani a felhasználóiprofil, túl a fent leírt 10 óra hozzáférés ablakban. Ez azt jelenti, hogy jelenleg nincs oly módon, hogy hosszú ideig nyújt hozzáférést elég bonyolultabb feladatokat, például víruskereső fut a felhasználóiprofil, vagy a naplózási adatok eléréséhez.

## <a name="do-registry-key-settings-persist"></a>Tegye megőrizni a beállításkulcs-értékei?
Igen, a HKEY_CURRENT_USER írt semmit a UPD részét képezi.

## <a name="can-i-disable-upds-for-a-collection"></a>Képes-e letiltása felhasználóiprofil gyűjtemény?
Igen, megkérheti, hogy letiltja a felhasználóiprofil-előfizetéshez tartozó Azure RemoteApp, de nem adott meg. Ez azt jelenti, hogy felhasználóiprofil le lesz tiltva az előfizetés minden gyűjteményére.

Előfordulhat, hogy le szeretné tiltani az alábbi esetek bármelyikében felhasználóiprofil: 

* El kell végeznie, elérését és irányítását felhasználói adatok (a naplózása, és tekintse át a pénzügyi intézmények például céljából).
* 3. fél felhasználói profil megoldások helyszíni felügyelethez, és továbbra is használja azokat a tartományhoz csatlakoztatott Azure RemoteApp környezetben van. Ehhez a profilhoz ügynök betöltését az arany lemezképbe van szükség. 
* Nincs szükség helyi adattárolóval, vagy minden adatokat tartalmaz a felhő vagy a fájlmegosztásnak, és a vezérlő szeretné helyileg használata az Azure RemoteApp adatok mentése.

Lásd: [tiltsa le a felhasználói Profillemezek (felhasználóiprofil) az Azure Remoteappban](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) további információt.

## <a name="can-i-restrict-users-from-saving-data-to-the-system-drive"></a>Felhasználók tiltása adatok mentése a rendszermeghajtón?
Igen, de kell beállítani, hogy a sablon lemezképe a gyűjtemény létrehozása előtt. Letiltja a hozzáférést a rendszermeghajtó tegye a következőket:

1. Futtatás **gpedit.msc** meg a sablon lemezképe.
2. Navigáljon a **felhasználó konfigurációja > Felügyeleti sablonok > Windows-összetevők > Explorer**.
3. Válassza ki a következő beállításokat:
   * **A megadott meghajtók Sajátgép elrejtése**
   * **A Sajátgép meghajtókhoz való hozzáférés letiltása**

## <a name="can-i-seed-upds-i-want-to-put-some-data-in-the-upd-thats-available-the-first-time-the-user-signs-in"></a>Is feltöltheti felhasználóiprofil? Egyes adatokat be a rendelkezésre álló a felhasználó jelentkezik be először UPD szeretnék.
Igen, a sablon lemezképe létrehozásakor hozzáadhat információk az alapértelmezett profil. Ezt az információt a UPD majd kerül.

## <a name="can-i-change-the-size-of-the-upd-depending-on-how-much-data-i-want-to-store"></a>Módosíthatja a tárolni kívánt adatok mennyiségétől függően UPD mérete?
Nem, minden felhasználóiprofil rendelkezik 50 GB tárhelyet. Ha azt szeretné, különböző mennyiségű adat tárolására, próbálkozzon a következőkkel:

1. Tiltsa le a felhasználóiprofil ahhoz a gyűjteményhez.
2. A felhasználók számára hozzáférést fájlmegosztás beállítása.
3. A fájlmegosztás betölteni egy indítási parancsfájl használatával. További információ alább olvasható az indítási parancsfájlokat az Azure Remoteappban.
4. Közvetlen felhasználó összes adatok mentése a fájlmegosztáshoz.

## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Hogyan futtathatok egy indítási parancsfájl az Azure Remoteappban?
Ha azt szeretné, egy indítási parancsfájl futtatásához, először hozzon létre egy ütemezett feladatot a gyűjteményhez használni kívánt sablon-lemezképben. (Ehhez *előtt* futtatja a Sysprep programot.) 

![A rendszer a feladat létrehozása](./media/remoteapp-upd/upd1.png)

![A rendszer fut, amikor a felhasználó bejelentkezik feladat létrehozása](./media/remoteapp-upd/upd2.png)

Az a **általános** lapra, ügyeljen arra, hogy módosítsa a **felhasználói fiók** alatt a biztonság a "BUILTIN\Users."

![A felhasználói fiók módosítása egy csoporthoz](./media/remoteapp-upd/upd4.png)

Az ütemezett feladat elindul a indítási parancsfájl, a felhasználó hitelesítő adataival. A futtatandó feladat ütemezése minden felhasználó egy alkalommal jelentkezik be.

![Állítsa be megfelelően az eseményindító a feladathoz tartozó, "bejelentkezés"](./media/remoteapp-upd/upd3.png)

Is [csoportházirend-alapú indítási parancsfájlok](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-the-start-menu-would-that-work"></a>Mi a helyzet helyezi el a Start menüben egy indítási parancsfájl? Szeretné, hogy működik a?
Ez azt jelenti I létrehozhat egy config ablak parancsfájlt futtató .bat fájl és mentse a c:\ProgramData\Microsoft\Windows\Start Menu\Programok\Indítópult mappát, és akkor kell, hogy lefusson, amikor a felhasználó elindít egy RemoteApp-munkamenetben parancsfájl?

Nem, amely nem támogatott az Azure RemoteApp, amely RDSH, amely is nem támogatja az indítási parancsfájlokat a Start menüben használja.

## <a name="can-i-use-mstscexe-the-remote-desktop-program-to-configure-logon-scripts"></a>Mstsc.exe (a távoli asztal program) segítségével konfigurálhatja a bejelentkezési parancsfájlok?
Nope nem támogatja az Azure RemoteApp.

## <a name="can-i-store-data-on-the-vm-locally"></a>Tárolhatok adatok a helyi virtuális Gépen?
NEM, a virtuális gép nem a felhasználói profillemezen bármely mappájában tárolt adatok ekkor elvesznek. Nincs a nagy valószínűséggel a felhasználó nem kap az azonos virtuális gép a következő bejelentkezéskor az Azure Remoteappba. Felhasználó-VM adatmegőrzési, nem azt karbantartása, így a felhasználó nem alá fogja írni az azonos virtuális gép, és a tárolt adatok ekkor elvesznek. Továbbá azt frissítenie kell a gyűjteményt, ha a meglévő virtuális gépek váltják fel olyan új készletét virtuális gépek -, amely azt jelenti, hogy a virtuális gépért tárolt összes adatot elvész. Az ajánljuk, hogy adatok tárolása a UPD, például Azure-fájlokban, a virtuális hálózaton belül, vagy a felhő dropboxba felhőalapú tárolási rendszert használ, a fájlkiszolgáló megosztott tároló.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Hogyan csatlakoztassa egy Azure fájlmegosztás a virtuális gép, PowerShell segítségével?
A Net-PSDrive parancsmag segítségével a meghajtó csatlakoztatása az alábbiak szerint:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


A hitelesítő adatok is mentheti a következő futtatásával:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Lehetővé teszi, hagyja ki a - Credential paramétert a New-PSDrive parancsmagban.

