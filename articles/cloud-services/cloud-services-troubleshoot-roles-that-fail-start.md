---
title: "aaaTroubleshoot szerepkörök toostart teljesítő |} Microsoft Docs"
description: "Az alábbiakban néhány gyakori okáról, miért egy felhőalapú szolgáltatás szerepkör toostart sikertelenek lehetnek. Megoldások toothese problémák is biztosítja."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e2fbecb08a10984add79dfc74e73de6869bb314f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-toostart"></a>Sikertelen toostart Felhőszolgáltatási szerepkörök hibaelhárítása
Az alábbiakban néhány gyakori problémák és megoldások kapcsolódó tooAzure Felhőszolgáltatások szerepkörök toostart sikertelen.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Hiányzó DLL-ek vagy függőségek
Nem válaszoló szerepkörök és a szerepköröket, amelyek között vannak Váltás **inicializálás**, **foglalt**, és **leállítása** állapotok hiányzik a dll-fájl vagy szerelvény okozhatja.

Hiányzik a dll-fájl vagy szerelvény tünetei lehet:

* A szerepkör példánya van Váltás **inicializálás**, **foglalt**, és **leállítása** állapotok.
* A szerepkör példánya túl áthelyezte**készen** tooyour webalkalmazás keresse meg, ha hello lap nem jelenik meg.

Többféleképpen ajánlott vizsgálata ezeket a problémákat.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>A webes szerepkör hiányzó DLL problémák diagnosztizálása
Navigálás tooa webhely, amely a webes szerepkör telepítve van, és hello böngészőben megjelenik egy kiszolgáló hiba hasonló toohello következő, azt jelezheti, hogy a dll-fájl hiányzik.

![Kiszolgálóhiba a "/" alkalmazás.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Az egyéni hibák kikapcsolásával eseményadatokat
Bővebb hibainformáció hello web.config a hello webes szerepkör tooset hello egyéni hiba mód tooOff konfigurálásával és a fájl újbóli terjesztése hello szolgáltatást is megtekinthetők.

tooview hibák több végezze el a távoli asztal használata nélkül:

1. Nyissa meg a hello megoldást a Microsoft Visual Studio.
2. A hello **Megoldáskezelőben**, keresse meg hello web.config fájlt, és nyissa meg.
3. Hello web.config fájlban keresse meg a hello system.web szakaszt, és adja hozzá a következő sor hello:

    ```xml
    <customErrors mode="Off" />
    ```
4. Hello fájl mentéséhez.
5. Csomagolja újra, és telepítse újra hello szolgáltatást.

Miután hello szolgáltatást újra kell telepíteni, megjelenik a hibaüzenet hello hello hiányzik a szerelvény vagy dll-fájl neve.

## <a name="diagnose-issues-by-viewing-hello-error-remotely"></a>Hello hiba távolról megtekintésével eseményadatokat
A távoli asztal tooaccess hello szerepkört használjon, és távolról tekintheti meg bővebb hibainformáció. Írja be a következő lépéseket tooview hello hibák távoli asztal használatával hello használata:

1. Győződjön meg arról, hogy telepítve van-e az Azure SDK 1.3-as vagy újabb.
2. Hello megoldás üzembe helyezése közben hello Visual Studio használatával, válassza a túl "... konfigurálása távoli asztali kapcsolatok". Hello távoli asztali kapcsolat konfigurálásával kapcsolatos további információkért lásd: [a távoli asztal Azure szerepkörök](../vs-azure-tools-remote-desktop-roles.md).
3. Hello Microsoft klasszikus Azure portál, miután hello példány egy állapotát jeleníti meg a **készen**, kattintson az egyik hello szerepkörpéldányokat.
4. Hello kattintson **Connect** hello ikonra **távelérési** hello menüszalag területén.
5. Jelentkezzen be toohello virtuális gép hello hello távoli asztal konfigurálása során megadott hitelesítő adatok használatával.
6. Nyisson meg egy parancsablakot.
7. Gépelje be: `IPconfig`.
8. Jegyezze fel a hello IPV4-cím értékét.
9. Nyissa meg az Internet Explorert.
10. Írja be a hello cím és hello webalkalmazás hello nevét. Például: `http://<IPV4 Address>/default.aspx`.

Navigálás toohello webhely most visszaadható egyértelműbben hibaüzenetek:

* Kiszolgálóhiba a "/" alkalmazás.
* Leírás: Nem kezelt kivétel történt hello hello aktuális webes kérelem végrehajtása során. Tekintse át a hello Veremkivonat hello hibával kapcsolatos további tudnivalókért és az azt okozó kódrészletre hello kódban.
* A kivétel részletei: System.IO.FIleNotFoundException: nem sikerült betölteni a fájl vagy szerelvény "Microsoft.WindowsAzure.StorageClient, verzió: 1.1.0.0-s, Culture = neutral, PublicKeyToken = = 31bf856ad364e35" vagy annak valamelyik függősége. hello rendszer nem találja a megadott hello fájlt.

Példa:

![A "/" alkalmazás explicit kiszolgálóhiba](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-hello-compute-emulator"></a>Eseményadatokat hello compute emulator használatával
Hello Microsoft Azure compute emulator toodiagnose használja, és a Hiányzó függőségek és web.config hibák elhárítása.

A legjobb eredmény érdekében ezzel a módszerrel a diagnosztikai egy számítógép vagy virtuális géphez, amelyen a Windows tiszta telepítését kell használnia. toobest hello Azure környezetbe szimulálja, Windows Server 2008 R2 x64 használja.

1. Hello hello önálló verziójának telepítése [Azure SDK](https://azure.microsoft.com/downloads/).
2. Hello fejlesztői gépen hello felhőszolgáltatás-projekt létrehozása.
3. A Windows Intézőben keresse meg a toohello bin\debug hello felhőszolgáltatás-projekt mappájából.
4. Másolja a hello .csx mappát és a .cscfg fájl toohello számítógép toodebug hello problémák használt.
5. Hello tiszta gépi, nyissa meg az Azure SDK parancssori ablakot, és írja be `csrun.exe /devstore:start`.
6. Írja be a parancssorba hello `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.
7. Hello szerepkör indulásakor látni fogja a hibával kapcsolatos részletes információk az Internet Explorerben. Is használhatja a szabványos Windows hibaelhárító eszközök toofurther hello probléma diagnosztizálásához.

## <a name="diagnose-issues-by-using-intellitrace"></a>Eseményadatokat IntelliTrace használatával
A munkavégző és a .NET-keretrendszer 4 webes szerepkörök is használhatja [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), elérhető a Microsoft Visual Studio Enterprise.

Kövesse ezeket lépéseket toodeploy hello szolgáltatás intellitrace engedélyezve:

1. Győződjön meg arról, hogy telepítve van-e az Azure SDK 1.3-as vagy újabb.
2. Hello megoldás telepítése a Visual Studio használatával. Ellenőrizze a telepítés során a hello **.NET 4-szerepkörök engedélyezése IntelliTrace** jelölőnégyzetet.
3. Miután hello példány elindítja, nyissa meg a hello **Server Explorer**.
4. Bontsa ki a hello **Azure\\Felhőszolgáltatások** csomópont, és keresse meg hello központi telepítését.
5. Bontsa ki hello telepítési, amíg megjelenik a szerepkörpéldányok hello. Kattintson a jobb gombbal az egyik hello példányok.
6. Válasszon **nézet IntelliTrace-naplók**. Hello **IntelliTrace összegzés** csak akkor nyílik meg.
7. Keresse meg az összefoglaló hello hello kivételek szakasza. Ha nincsenek kivételek, hello szakasz neve **Kivételadat**.
8. Bontsa ki a hello **Kivételadat** , és keressen **System.IO.FileNotFoundException** hibák hasonló toohello következő:

![Kivételadat, hiányzó fájl vagy szerelvény](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Hiányzó dll-EK és szerelvények
Hiányzik a dll-fájl- és szerelvényfájlokat hibák tooaddress kövesse az alábbi lépéseket:

1. Nyissa meg a hello megoldást a Visual Studio.
2. A **Megoldáskezelőben**, nyissa meg hello **hivatkozások** mappát.
3. Kattintson a hello hiba azonosított hello szerelvény.
4. A hello **tulajdonságok** ablaktáblán keresse meg **másolása helyi tulajdonság** és hello érték túl**igaz**.
5. Telepítse újra a hello felhőalapú szolgáltatást.

Miután ellenőrizte, hogy az összes hiba javítva lett, hello szolgáltatás hello ellenőrzése nélkül telepítheti **.NET 4-szerepkörök engedélyezése IntelliTrace** jelölőnégyzetet.

## <a name="next-steps"></a>Következő lépések
További [cikkek hibaelhárítási](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) felhőszolgáltatásai számára.

Hogyan tootroubleshoot felhő szerepkör-szolgáltatás Azure PaaS számítógép diagnosztikai adatok használatával állít toolearn lásd [Kevin Williamson blog adatsorozat](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
