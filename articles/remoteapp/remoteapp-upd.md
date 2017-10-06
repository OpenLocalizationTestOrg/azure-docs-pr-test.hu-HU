---
title: "aaaHow Azure RemoteApp felhasználói adatok és beállítások mentési használ? | Microsoft Docs"
description: "Ismerje meg, hogyan menti az Azure RemoteApp a felhasználói adatok hello felhasználói profillemeze használatával."
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
ms.openlocfilehash: dccebc7861e8a0d87cb3ee174f399a2df7fe023c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Hogyan menteni a Azure RemoteApp a felhasználói adatok és beállítások?
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp menti a felhasználói azonosító és a testreszabások eszközök és munkamenet között. Felhasználói adatok felhasználónkénti gyűjteményre lemez, a felhasználói profil lemezre (UPD) néven ismert tárolja. hello lemez hello felhasználói követi, és biztosítja a hello felhasználó rendelkezik-e egy egységes felhasználói élményt, függetlenül attól, ahol bejelentkeznek az.

Felhasználói profillemezek teljes mértékben transzparens toohello felhasználókként szerepelnek – felhasználók dokumentumok tootheir dokumentumok mappa mentése (a megjelenő toobe egy helyi meghajtó), és módosítsa a alkalmazás beállításait, mint a szokásos. At hello azonos belül, mind a személyes beállítások megmaradnak, bármilyen eszközről tooAzure RemoteApp kapcsolódáskor. Minden hello felhasználónál információikat a hello ugyanazon a helyen.

Minden egyes UPD állandó tároló 50GB rendelkezik, és mindkét felhasználói adatok és alkalmazások beállításait tartalmazza. 

A részletekért a felhasználói profil adatainak olvasás.

> [!NOTE]
> Szükséges toodisable hello UPD? Megteheti, hogy most - Pavithra által írt blogbejegyzés, kivételét [tiltsa le a felhasználói Profillemezek (felhasználóiprofil) az Azure Remoteappban](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), a részletekért.
> 
> 

## <a name="how-can-an-admin-get-toohello-data"></a>Hogyan férhetnek a rendszergazda toohello adatokat?
Ha tooaccess hello adatokat kell a felhasználókat (a vész-helyreállítási, vagy ha hello felhasználó elhagyja-e a hello vállalati) egyike, lépjen kapcsolatba az Azure támogatási szolgálatához, és adjon meg hello előfizetési adatok hello adatgyűjtési és -hello felhasználói azonosító. hello Azure RemoteApp csapatától biztosítja azokat egy URL-cím toohello VHD-t. Töltse le a virtuális merevlemez, és a dokumentumok és fájlok kell beolvasni. Vegye figyelembe, hogy hello VHD 50 GB-os, így a bit toodownload azt.

## <a name="is-hello-data-backed-up"></a>Hello adatairól készít biztonsági mentést?
Igen, azt a biztonsági mentéshez hello felhasználói adatok egy földrajzi helyet. hello adatok csak olvasható, és lehet hozzáférni hello ugyanaz, mint hello rendszeres adatai (lépjen kapcsolatba az Azure RemoteApp tooget azt), ha az elsődleges adatközpont hello nem működik. hello adatok másolt valós idejű toohello biztonsági mentési helyre, és azt nem példányaira különböző verziói. Így adatsérülés, azt nem lesz képes toorestore azt korábban a helyes verziót, de ha hello elsődleges adatközpont leáll, a képes tooget felhasználó adatait fogja ismert tooa hello más helyre.

## <a name="how-do-users-see-hello-upd-on-hello-server-side"></a>Hogyan felhasználók látnak hello UPD hello kiszolgálóoldalon?
Minden felhasználó saját rendelkezik, amely leképezhető tootheir UPD hello kiszolgálón: c:\Users\username.

## <a name="whats-hello-best-way-toouse-outlook-and-upd"></a>Mi az az hello legjobb módja toouse Outlook és UPD?
Az Azure RemoteApp hello Outlook állapota (postaládák, pst) menti a munkamenetek között. tooenable, igazolnia kell hello csendes-óceáni TÉLI toobe hello felhasználói profil adatait tárolja (c:\users\<felhasználónév >). Ez az alapértelmezett hely hello hello adatok esetén, amennyiben nem módosítja hello helyét, hello adatok megmaradnak a munkamenetek között.

Javasoljuk továbbá, az Outlook használja a "gyorsítótárazott" és "server/online" üzemmódban használja a keresés.

Tekintse meg [Ez a cikk](remoteapp-outlook.md) vonatkozó további információ az Outlook és az Azure RemoteApp használatával.

## <a name="what-about-redirection"></a>Mi a helyzet átirányítása?
Konfigurálhatja az Azure RemoteApp toolet felhasználók férhetnek hozzá a helyi eszközök beállításával [átirányítás](remoteapp-redirection.md). Helyi eszközök képes tooaccess hello adatok hello UPD lesz.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Használható hálózati megosztásnak a UPD?
Nem. Felhasználóiprofil nem használható egy hálózati megosztásra. A rendszer egy UPD csak elérhető toohello felhasználói Ha hello felhasználó aktívan csatlakozó tooAzure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Ha egy felhasználó egy gyűjteményből, azok UPD törölve van?
Nem, ha a felhasználó töröl, azt nem automatikusan delete hello UPD - ehelyett tároljuk hello adatok hello gyűjtemény törléséig. hello gyűjtemény törlése után 90 nappal azt minden felhasználóiprofil törlése. 

Ha egy olyan gyűjteményből UPD toodelete van szüksége, lépjen kapcsolatba az Azure RemoteApp - UPD az ügyféloldali törölheti azt.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Hozzáférhet a felhasználók felhasználóiprofil (vagy a jelenlegi, vagy a törölt felhasználók)?
Igen, ha kapcsolatba lép a [Azure RemoteApp](mailto:remoteappforum@microsoft.com), azt állíthat be, az egy URL-cím tooaccess hello adatokat. Összekapcsolta 10 óra toodownload kapcsolatos adatokat és fájlokat hello UPD hello hozzáférés lejárata előtt.

## <a name="are-upds-available-offline"></a>Elérhetők a felhasználóiprofil offline?
Most nem nyújtunk offline hozzáférés tooUPDs túl a fent leírt hello 10 óra hozzáférés ablakban. Ez azt jelenti, hogy jelenleg nincs olyan módon tooprovide való hozzáférési elég hosszú több bonyolult toocomplete feladatok, például víruskereső szoftver futó hello felhasználóiprofil vagy a naplózási adatok elérése.

## <a name="do-registry-key-settings-persist"></a>Tegye megőrizni a beállításkulcs-értékei?
Igen, tooHKEY_Current_User írt semmit a hello UPD részét képezi.

## <a name="can-i-disable-upds-for-a-collection"></a>Képes-e letiltása felhasználóiprofil gyűjtemény?
Igen, kérje meg az Azure RemoteApp toodisable felhasználóiprofil-előfizetéssel, de nem adott meg. Ez azt jelenti, hogy felhasználóiprofil le lesz tiltva hello előfizetés minden gyűjteményére.

Érdemes lehet a következő helyzetekben hello valamelyikében toodisable felhasználóiprofil: 

* El kell végeznie, elérését és irányítását felhasználói adatok (a naplózása, és tekintse át a pénzügyi intézmények például céljából).
* 3. fél felhasználói profil megoldások helyszíni felügyelethez, és szeretné, hogy a tartományhoz csatlakoztatott Azure RemoteApp környezetben használatuk toocontinue rendelkezik. Ehhez hello profil ügynök toobe hello arany lemezképpel betöltött van szükség. 
* Nincs szükség helyi adattárolóval vagy hello felhő vagy a fájlmegosztásnak összes adatokat tartalmaz, és szeretné toocontrol mentése az adatok helyileg az Azure RemoteApp használatával.

Lásd: [tiltsa le a felhasználói Profillemezek (felhasználóiprofil) az Azure Remoteappban](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) további információt.

## <a name="can-i-restrict-users-from-saving-data-toohello-system-drive"></a>Felhasználók tiltása mentése adatok toohello rendszermeghajtón?
Igen, de tooset, amely másolatot hello sablon rendszerképet hello gyűjtemény létrehozása előtt szüksége lesz. A következő lépéseket tooblock hozzáférés toohello rendszermeghajtó hello használata:

1. Futtatás **gpedit.msc** hello sablon rendszerképre.
2. Keresse meg a túl**felhasználó konfigurációja > Felügyeleti sablonok > Windows-összetevők > Explorer**.
3. Válassza ki az alábbi beállítások hello:
   * **A megadott meghajtók Sajátgép elrejtése**
   * **Hozzáférés toodrives saját számítógépről megakadályozása**

## <a name="can-i-seed-upds-i-want-tooput-some-data-in-hello-upd-thats-available-hello-first-time-hello-user-signs-in"></a>Is feltöltheti felhasználóiprofil? Szeretném tooput adatokat tartalmaz, amely elérhető hello első alkalommal hello felhasználói UPD hello jelentkezik be.
Igen, hello sablonlemezkép létrehozásakor hozzáadhat információk toohello alapértelmezett profilt. Ez az információ toohello UPD kerül.

## <a name="can-i-change-hello-size-of-hello-upd-depending-on-how-much-data-i-want-toostore"></a>Módosítható hello UPD hello mérete attól függően, hogy mennyi adatot toostore kívánt?
Nem, minden felhasználóiprofil rendelkezik 50 GB tárhelyet. Ha azt szeretné, hogy a különféle adatmennyiségek toostore, próbálkozzon a hello következő:

1. Tiltsa le a felhasználóiprofil hello gyűjtemény.
2. Felhasználók tooaccess fájlmegosztás beállítása.
3. Hello-fájlmegosztás terhelés indítási parancsfájl használatával. További információ alább olvasható az indítási parancsfájlokat az Azure Remoteappban.
4. Közvetlen felhasználók toosave összes adatok toohello fájlmegosztást.

## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Hogyan futtathatok egy indítási parancsfájl az Azure Remoteappban?
Ha azt szeretné, hogy egy indítási parancsfájl toorun, indítsa el az ütemezett feladat létrehozásával hello sablon rendszerképet kívánja toouse hello gyűjtemény. (Ehhez *előtt* futtatja a Sysprep programot.) 

![A rendszer a feladat létrehozása](./media/remoteapp-upd/upd1.png)

![A rendszer fut, amikor a felhasználó bejelentkezik feladat létrehozása](./media/remoteapp-upd/upd2.png)

A hello **általános** lapra, lehet, hogy toochange hello **felhasználói fiók** biztonsági csoportban túl "BUILTIN\Users."

![Hello felhasználói fiók tooa csoport módosítása](./media/remoteapp-upd/upd4.png)

hello ütemezett feladat elindul a indítási parancsfájl hello felhasználói hitelesítő adatok használatával. Hello feladat toorun ütemezése minden felhasználó egy alkalommal jelentkezik be.

![Set hello eseményindítója a következőnek: "A napló a" hello tevékenység](./media/remoteapp-upd/upd3.png)

Is [csoportházirend-alapú indítási parancsfájlok](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-hello-start-menu-would-that-work"></a>Mi a helyzet helyezi el egy indítási parancsfájl hello Start menü? Szeretné, hogy működik a?
Más szóval I hozzon létre egy .bat fájlt egy config ablak parancsfájlt futtató és mentheti azt toohello c:\ProgramData\Microsoft\Windows\Start Menu\Programok\Indítópult mappát, és akkor kell, hogy lefusson, amikor a felhasználó elindít egy RemoteApp-munkamenetben parancsfájl?

Nem, amely nem támogatott az Azure RemoteApp, amely RDSH, amely még nem támogatja a indítási parancsfájlok hello Start menüben használja.

## <a name="can-i-use-mstscexe-hello-remote-desktop-program-tooconfigure-logon-scripts"></a>Mstsc.exe (hello távoli asztal program) tooconfigure bejelentkezési parancsfájlok használata
Nope nem támogatja az Azure RemoteApp.

## <a name="can-i-store-data-on-hello-vm-locally"></a>Tárolhatok adatok a helyi virtuális gép hello?
NEM, hello UPD a virtuális gép nem hello bármely mappájában tárolt adatok ekkor elvesznek. Van egy nagy valószínűséggel hello felhasználó nem kap hello ugyanazon VM hello, amikor legközelebb bejelentkeznek az Azure Remoteappba. Jelenleg nem tartanak felhasználói-VM adatmegőrzési, így hello felhasználói nem alá fogja írni a hello azonos virtuális gép, és hello tárolt adatok ekkor elvesznek. Emellett hello gyűjtemény frissítjük, meglévő virtuális gépek olyan új készletét virtuális gépek -, amely azt jelenti, hogy a virtuális gépért hello tárolt összes adatot cserélése hello elvész. hello ajánljuk, hello UPD, például Azure-fájlokban, a VNETEN belül, vagy hello felhő dropboxba felhőalapú tárolási rendszert használ, a fájlkiszolgáló megosztott tároló toostore adatokat.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Hogyan csatlakoztassa egy Azure fájlmegosztás a virtuális gép, PowerShell segítségével?
Hello Net-PSDrive parancsmag toomount hello meghajtó, az alábbiak szerint használható:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


A hitelesítő adatok is mentheti hello következő futtatásával:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Amely lehetővé teszi kihagyása hello - Credential paramétert a New-PSDrive hello parancsmagban.

