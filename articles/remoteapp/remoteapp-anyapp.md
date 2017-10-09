---
title: "aaaRun bármely Windows-alkalmazások bármely eszközön és az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan tooshare bármelyik Windows-alkalmazást a felhasználóival az Azure RemoteApp segítségével."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: 961d40ca-9673-4977-aa54-d6b22fc61ce1
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 750f3b881e0cb671ff6e8f3e851bccdf2262d156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-any-windows-app-on-any-device-with-azure-remoteapp"></a>Windows-alkalmazások futtatása bármely Azure RemoteAppet használó eszközön
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Bárhol, bármilyen eszközön, akár azonnal futtathat Windows-alkalmazásokat – egyszerűen az Azure RemoteApp használatával. Egy 10 éve írt egyéni alkalmazásról, vagy akár egy Office-alkalmazást, a felhasználók többé nem kell kötött toobe tooa adott operációs rendszerrel (például Windows XP) néhány alkalmazás.

Az Azure RemoteApp segítségével a felhasználók a saját Android vagy is és az Apple-eszközök hello ugyanazt a felhasználói élményt élvezhetik, mint Windows rendszeren (vagy Windows Phone-telefonokon). Ehhez a Windows-alkalmazását Windows rendszerű virtuális gépeken tároljuk az Azure-ban, így a felhasználók bárhonnan elérhetik ezeket, ha rendelkeznek internetkapcsolattal. 

Olvassa el a példa pontosan hogyan toodo ez.

Ebben a cikkben tooshare Access programot fogjuk az összes felhasználónkkal. Azonban ehelyett BÁRMELYIK alkalmazás használható. Mindaddig, amíg az alkalmazás a Windows Server 2012 R2 számítógépen is telepíthető, megoszthatja azt az alábbi hello lépéseket használhatja. Tekintse át hello [alkalmazáskövetelmények](remoteapp-appreqs.md) toomake meg arról, hogy működni fog.

Vegye figyelembe, hogy hozzáférést egy adatbázist, és szeretnénk, hogy adatbázis toobe hasznos, mert mindaddig kell végre néhány további lépés toolet felhasználók adjon hello Access adatmegosztásához eléréséhez. Ha az alkalmazása nem adatbázis, vagy nincs szükség a felhasználók toobe képes tooaccess egy fájlmegosztást, ezeket a lépéseket kihagyhatja az oktatóanyag

> [!NOTE]
> <a name="note"></a>Ez az oktatóanyag kell egy Azure-fiók toocomplete:
> 
> * Is [nyissa meg az Azure-fiók szabad](https://azure.microsoft.com/free/?WT.mc_id=A261C142F): jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például webhelyekhez. A hitelkártya soha nem lesz felszámítva, kivéve, ha explicit módon a beállítások módosításához és kérje meg a toobe számítjuk fel.
> * [Aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): Az MSDN-előfizetés minden hónapban biztosít Önnek krediteket, amelyekkel fizetős Azure-szolgáltatásokat használhat.
> 
> 

## <a name="create-a-collection-in-remoteapp"></a>Katalógus létrehozása a RemoteAppben
Első lépésként hozzon létre egy gyűjteményt. az alkalmazások és felhasználók tárolója hello gyűjtemény lesz. Minden gyűjtemény egy rendszerképen alapul – létrehozhatja a saját rendszerképét, vagy használhatja azt, amelyet az előfizetéséhez kapott. Ebben az oktatóanyagban használjuk-próba kép hello Office 2013 – hello alkalmazást, hogy szeretnénk tooshare tartalmaz.

1. Az Azure-portálon hello görgessen le a hello bal oldali navigációs fában amíg megjelenik a RemoteApp. Nyissa meg azt az oldalt.
2. Kattintson a **RemoteApp-gyűjtemény létrehozása** lehetőségre.
3. Kattintson a **Gyors létrehozás** gombra, és adjon meg egy nevet a gyűjtemény számára.
4. Válassza ki a gyűjtemény toouse toocreate kívánt hello régiót. Hello legjobb élmény érdekében régiót hello földrajzilag legközelebb van a felhasználói hozzáférésének helyétől hello app toohello helyét. Például ebben az oktatóanyagban hello felhasználók találhatók Redmond, Washington. hello legközelebbi Azure-régió az **USA nyugati régiója**.
5. Válassza ki a kívánt toouse hello számlázási csomagot. hello alapszintű számlázási csomag 16 felhasználót helyez egy nagyméretű Azure virtuális Gépet, miközben hello standard számlázási csomag esetében 10 felhasználó található egy nagyméretű Azure virtuális Gépen. Általános példaként alapszintű hello esetében működik jól adatok adatbevitel típusú munkafolyamatok. Egy termelékenységi alkalmazás, például az Office célszerű hello standard csomag.
6. Végül válassza ki a hello Office 2013 Professional rendszerképet. Ez a rendszerkép tartalmazza Office 2013-alkalmazásokat. Emlékeztető: ez a rendszerkép csak próbaverziójú gyűjteményekhez és POC-khez használható, éles gyűjteményekben nem.
7. Most kattintson a **RemoteApp-gyűjtemény létrehozása** lehetőségre.

![Felhőalapú gyűjtemény létrehozása a RemoteAppben](./media/remoteapp-anyapp/ra-anyappcreatecollection.png)

Ekkor elindul a gyűjtemény létrehozása, de tooan óra is eltarthat.

Így már készen áll a tooadd még a felhasználók.

## <a name="share-hello-app-with-users"></a>Felhasználókkal való megosztás hello alkalmazás
Ha a gyűjtemény sikeresen létrejött, idő toopublish hozzáférés toousers és hozzáférési tooit rendelkező hello felhasználók hozzáadása.

Ha lépjen másik lapra hello Azure RemoteApp csomóponttól hello gyűjtemény létrehozása közben, kezdje azáltal, hogy a biztonsági tooit hello Azure kezdőlapról módon.

1. Kattintson a további beállítások létrehozott korábbi tooaccess hello gyűjteményre, és konfiguráljon hello gyűjteményt.
   ![Új felhőalapú RemoteApp-gyűjtemény](./media/remoteapp-anyapp/ra-anyappcollection.png)
2. A hello **közzétételi** lapra, majd **közzététel** üdvözlő képernyőt, és kattintson a hello alján **Start menü programjainak közzététele**.
   ![RemoteApp-program közzététele](./media/remoteapp-anyapp/ra-anyapppublish.png)
3. Válassza ki a kívánt hello listából toopublish hello alkalmazásokat. Ebben az oktatóanyagban az Accesst választjuk. Kattintson a **Befejezés** gombra. Várjon, amíg hello alkalmazások toofinish közzététel.
   ![Az Access közzététele a RemoteAppben](./media/remoteapp-anyapp/ra-anyapppublishaccess.png)
4. Hello alkalmazás közzététele befejeződött, miután látogasson toohello **felhasználói hozzáférés** lapon tooadd hello tooyour alkalmazások eléréséhez szükséges összes felhasználó. Adja meg a felhasználók felhasználónevét (e-mail-címét), majd kattintson a **Mentés** gombra.

![Felhasználók tooRemoteApp hozzáadása](./media/remoteapp-anyapp/ra-anyappaddusers.png)

1. Idő tootell, a felhasználókat arról, hogy új alkalmazásokról, és hogyan tooaccess őket. toodo, küldjön egy e-mailt toohello távoli asztali ügyfél letöltési URL-CÍMRE mutat őket.
   ![RemoteApp hello ügyfél letöltési URL-címe](./media/remoteapp-anyapp/ra-anyappurl.png)

## <a name="configure-access-tooaccess"></a>Hozzáférés tooAccess konfigurálása
Egyes alkalmazások további konfigurálást igényelnek azután, hogy üzembe helyezte őket a RemoteAppen keresztül. Különösen hozzáféréshez, a rendszer minden olyan felhasználó hozzáférő Azure fájlmegosztás folyamatos toocreate. (Ha nem szeretné, hogy toodo, létrehozhat egy [hibrid gyűjtemény](remoteapp-create-hybrid-deployment.md) [helyett a felhőalapú gyűjteményünk, amely lehetővé teszi, hogy a felhasználók férhessenek hozzá fájlokhoz és információk a helyi hálózaton.) Ezt követően kell tootell a felhasználók toomap egy helyi meghajtó a számítógépen toohello Azure-fájlrendszerre.

hello első része hello rendszergazdaként teheti meg. Ezután néhány lépést a felhasználóinak kell elvégeznie.

1. Első lépésként közzétételi hello parancssori felület (cmd.exe). A hello **közzétételi** lapon jelölje be **cmd**, és kattintson a **közzététel > program közzététele elérési út használatával**.
2. Adja meg a hello app és hello elérési hello nevét. A célra használják "Fájlkezelő" hello nevét és "% SYSTEMDRIVE%\windows\explorer.exe" hello elérési útjával.
   ![Hello cmd.exe fájl közzététele.](./media/remoteapp-anyapp/ra-publishcmd.png)
3. Most egy Azure toocreate [tárfiók](../storage/common/storage-create-storage-account.md). Azt is "accessstorage," nevű, adjon meg egy nevet, amely jelentéssel bíró tooyou. (toomisquote Highlander, lehetnek, hogy csak egy "accessstorage.") ![Az Azure storage-fiók](./media/remoteapp-anyapp/ra-anyappazurestorage.png)
4. Most lépjen vissza tooyour irányítópult érheti hello elérési tooyour storage (végponthelyét). Erre később még szükség lesz, ezért másolja ki valahová.
   ![hello tárfiók elérési útja](./media/remoteapp-anyapp/ra-anyappstoragelocation.png)
5. A következő hello storage-fiók létrehozása után meg kell hello elsődleges elérési kulcsát. Kattintson a **elérési kulcsok kezelése**, és ezután másolási hello elsődleges elérési kulcsát.
6. Most hello tárfiók hello környezet beállítása, és Új fájlmegosztás létrehozása a hozzáférés. Futtassa a következő parancsmagokat egy emelt szintű Windows PowerShell-ablakban hello:
   
        $ctx=New-AzureStorageContext <account name> <account key>
        $s = New-AzureStorageShare <share name> -Context $ctx
   
    Ezért a megosztás, az alábbiak hello parancsmagokat futtatjuk le:
   
        $ctx=New-AzureStorageContext accessstorage <key>
        $s = New-AzureStorageShare <share name> -Context $ctx

Most akkor a hello felhasználókon a sor. Először kérje meg a felhasználóit, hogy telepítsenek egy [RemoteApp-ügyfelet](remoteapp-clients.md). A következő hello felhasználók egy meghajtót a fiók toothat Azure fájl megosztása létrehozott toomap kell, és adja hozzá a fájlok elérése. Ezt a következőképpen tehetik meg:

1. Hello RemoteApp-ügyfelet, az access hello közzétett alkalmazást. Indítsa el a hello cmd.exe programot.
2. Futtassa a következő parancs toomap hello egy meghajtót a számítógép toohello fájlmegosztásból:
   
        net use z: \\<accountname>.file.core.windows.net\<share name> /u:<user name> <account key>
   
    Ha hello **/ persistent** paraméter tooyes hello csatlakoztatott meghajtó minden munkamenetben megmarad.
3. Ezután indítsa el a hello fájlkezelő alkalmazást a Remoteappból. Hello megosztott app toohello fájlmegosztásban toouse kívánt Access-fájlokat másolni.
   ![Access-fájlok elhelyezése Azure-megosztásban](./media/remoteapp-anyapp/ra-anyappuseraccess.png)
4. Végül nyissa meg az Accesst, és majd nyissa meg az imént megosztott hello adatbázis. Az adatok, az Access hello felhőből kell megjelennie.
   ![Egy igazi Access-adatbázis hello felhőből fut](./media/remoteapp-anyapp/ra-anyapprunningaccess.png)

Most már bármelyik eszközön használhatja az Accesst, ha telepít hozzá egy RemoteApp-ügyfelet.

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
Most, hogy elsajátította gyűjtemények létrehozását, próbáljon meg létrehozni egy [Office 365-öt használó gyűjteményt](remoteapp-tutorial-o365anywhere.md). Vagy hozzon létre egy [hibrid gyűjteményt](remoteapp-create-hybrid-deployment.md), amely hozzáfér a helyi hálózatához.

<!--Image references-->

