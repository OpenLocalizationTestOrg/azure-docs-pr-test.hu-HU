---
title: "aaaDebugging az Azure cloud service vagy a virtuális gép a Visual Studio |} Microsoft Docs"
description: "Egy felhőalapú szolgáltatás, vagy a virtuális gép a Visual Studio hibakereső"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 945e06e0-2100-41af-b218-72347367ddab
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 32a326430021ba2ea9317a6a71fa005d4b87c273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Egy Azure-felhőszolgáltatásban vagy a virtuális gép a Visual Studio hibakereső
A Visual Studio hibakeresési Azure felhőszolgáltatások és virtuális gépek különböző lehetőséget biztosít.

## <a name="debug-your-cloud-service-on-your-local-computer"></a>A felhőalapú szolgáltatás a helyi számítógépen hibakeresése
Mentheti időt és pénzt hello Azure compute emulator toodebug használatával a felhőalapú szolgáltatás a helyi számítógépen. Által a szolgáltatás telepítése előtt helyileg hibakeresés, akkor is megbízhatóságának és teljesítményének javítása számítási alkalommal fizető nélkül. Azonban néhány hiba is felléphet csak futtatásakor egy felhőalapú szolgáltatás az Azure-ban magát. Ezek a hibák megoldhassuk, ha engedélyezi a távoli hibakeresés, majd csatolja a hello hibakereső tooa szerepkör példánya és a szolgáltatást teszik közzé.

hello emulátor szimulál hello Azure számítási szolgáltatás, és a helyi környezetben fut, így tesztelése és hibakeresése a felhőalapú szolgáltatás telepítése előtt. hello emulátor leírók hello a szerepkörpéldányok életciklusát, és hozzáférést biztosít a toosimulated erőforrások, például a helyi tároló. Hibakeresési, illetve a szolgáltatás futtatásához a Visual Studio automatikusan elindul az emulátor hello a háttér-alkalmazásként, és ezután telepíti a szolgáltatás toohello emulátor. Használhat hello emulátor tooview a szolgáltatás helyi környezetben hello futtatásakor. Hello teljes vagy hello hello emulator express verziója is futtathatja. (Azure 2.3 verziótól kezdődően hello expressz hello emulator verziószáma hello alapértelmezett.) Lásd: [tooRun Emulator Express használatával és egy felhőalapú szolgáltatás helyileg hibakeresési](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="toodebug-your-cloud-service-on-your-local-computer"></a>a felhőalapú szolgáltatás a helyi számítógépen toodebug
1. Hello menüsávban válassza **Debug**, **Start Debugging** toorun az Azure-felhőszolgáltatás-projekt. Alternatív megoldásként nyomja le az F5 billentyűt. Megjelenik egy üzenet, miszerint a Compute Emulator hello indul. Hello emulátor indításakor hello tálcaikonjára megerősíti, hogy azt.

    ![A tálcán hello Azure emulátorban](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)
2. Hello felhasználói felület megjelenítése hello compute Emulator hello értesítési területen hello helyi menüje hello Azure ikon megnyitásával, majd válassza ki **Show Compute Emulator felhasználói felületén**.

    hello bal oldali ablaktáblájában hello felhasználói felület látható hello szolgáltatások, amelyek jelenleg telepített toohello compute emulator és hello szerepkörpéldányokat fut-e minden szolgáltatás. Választhat hello szolgáltatást vagy szerepkörök toodisplay életciklusát, naplózási és diagnosztikai információk hello jobb oldali ablaktáblán. Ha a felső margó hello egy befoglalt ablak hello fókusz be, bővíti toofill hello jobb oldali ablaktáblán.
3. Hello található parancsokkal hello alkalmazáson keresztül lépés **Debug** menü és beállítása a töréspontokat a kódban. Hello hibakereső alkalmazást hello lépéseit, mert hello ablak hello alkalmazás hello az aktuális állapota is frissül. Ha kikapcsolja a hibakeresést, hello alkalmazástelepítés törlődik. Ha az alkalmazás tartalmaz a webes szerepkör, és beállította a hello indítási műveletet tulajdonság toostart hello webböngésző, Visual Studio elindítja a webes alkalmazás hello böngészőben. Ha megváltoztatja hello hello szolgáltatás konfigurációját a szerepkör-példányok száma, kell a felhőalapú szolgáltatás leállítása és majd indítsa újra a hibakeresést, hogy ezek a új példányok hello szerepkör is hibakeresése.

    **Megjegyzés:** fut, vagy amikor a hibakeresést a service hello helyi a compute emulator és a storage emulator nem leállt. Le kell állítani azokat explicit módon a hello értesítési területen.

## <a name="debug-a-cloud-service-in-azure"></a>Hibakeresése az Azure-felhőszolgáltatás
toodebug egy távoli számítógépről egy felhőalapú szolgáltatás, engedélyeznie kell, hogy a funkció explicit módon, hogy a szükséges szolgáltatások (például msvsmon.exe) telepítve legyen a szerepkörpéldányok futó virtuális gépek hello a felhőalapú szolgáltatás telepítésekor. Ha nem engedélyezi a távoli hibakeresés közzé a hello szolgáltatást, hogy toorepublish hello szolgáltatás távoli hibakeresés engedélyezésével.

Ha engedélyezi a távoli hibakeresés egy felhőalapú szolgáltatás, ez nem csökkent teljesítményt mutat, vagy további költségek. Ne használja termelési szolgáltatás, a távoli hibakeresés mert hello szolgáltatást használó ügyfeleket negatív hatással lehet.

> [!NOTE]
> Ha közzéteszi a Visual Studio felhőszolgáltatás, engedélyezheti a **IntelliTrace** szerepköröket, amelyek a szolgáltatás a cél hello .NET-keretrendszer 4 vagy hello .NET-keretrendszer 4.5. A **IntelliTrace**, vizsgálja meg az eseményeket, amelyek a következő fordult elő az elmúlt hello olyan szerepkörpéldányt, és Reprodukálja hello környezetben kezdve. Lásd: [egy közzétett felhőszolgáltatás IntelliTrace és a Visual Studio hibakeresési](http://go.microsoft.com/fwlink/?LinkID=623016) és [használatával IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx).
>
>

### <a name="tooenable-remote-debugging-for-a-cloud-service"></a>távoli hibakeresés egy felhőszolgáltatásra tooenable
1. Hello hello Azure-projekt helyi menüjének megnyitásához, majd válassza ki **közzététel**.
2. Jelölje be hello **átmeneti** környezet és hello **Debug** konfigurációs.

    Ez az útmutató csak. Dönthet úgy is toorun a tesztkörnyezetben az éles környezetben. Azonban ez káros hatással lehet felhasználók hello éles környezetben a távoli hibakeresés engedélyezésével. Kiválaszthatja a hello kiadás konfigurációs, de hello hibakeresési konfiguráció esetén könnyebb hibakeresést.

    ![Hello hibakeresési konfiguráció](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)
3. Hajtsa végre az hello szokásos lépéseket, de válassza hello **távoli hibakereső engedélyezése az összes szerepkör** hello jelölőnégyzetet **speciális beállítások** lapon.

    ![Konfigurációs hibakeresése](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="tooattach-hello-debugger-tooa-cloud-service-in-azure"></a>tooattach hello hibakereső tooa felhőalapú szolgáltatás az Azure-ban
1. A Server Explorer eszközben bontsa ki a felhőalapú szolgáltatás hello csomópont.
2. Nyissa meg hello helyi menü hello szerepkör vagy a szerepkör példánya toowhich tooattach szeretne, és válassza a **csatolása hibakereső**.

    Ha hibakeresési szerepkör, hello Visual Studio hibakereső csatol az adott szerepkör tooeach példány. hello hibakereső megszakítja a hello első szerepkörpéldányt, amely futtatja a sort, és azokat a feltételeket, hogy az megfelel a töréspont. Ha hibakeresési egy példányát, hello hibakereső csatol, hogy a példány és a töréspont csak akkor, ha az adott példány fut a sort, és megfelel-e oldaltörések hello töréspont tartozó feltételek tooonly.

    ![Hibakereső csatlakoztatása](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)
3. Miután hello hibakereső tooan példányhoz csatolja, hibakeresési a szokásos módon. hello hibakereső automatikusan csatolja a toohello megfelelő gazdafolyamat a szerepkörhöz. Attól függően, hogy milyen hello szerepköre, hello hibakereső rendeli toow3wp.exe, WaWorkerHost.exe vagy WaIISHost.exe. tooverify hello folyamat toowhich hello hibakereső van csatolva, bontsa ki a Server Explorer hello példány csomópontot. Lásd: [Azure szerepkör architektúra](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Azure folyamatokkal kapcsolatos további információkat.

    ![Válassza ki a kód párbeszédpanel](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
4. tooidentify hello folyamatok toowhich hello hibakereső van csatlakoztatva, hello folyamatok párbeszédpanel megnyitásához, hello menüsoron, hibakeresési, a Windows, a folyamatok kiválasztása. (Billentyűzet: Ctrl + Alt + Z) toodetach megadott folyamatáról, nyissa meg a helyi menüt, és válassza **leválasztani folyamat**. Vagy, hello példány csomópont található a Server Explorer, hello folyamat található, a helyi menü megnyitásához, és válassza **leválasztani folyamat**.

    ![Folyamatok hibakeresése](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

> [!WARNING]
> Hosszú leáll, amikor távoli töréspontok elkerülése hibakeresést. Azure értékként kezelje-e a folyamat, amely válaszol néhány percnél hosszabb ideig leáll, és leállítja a forgalom toothat példány küldésekor. Ha túl sokáig megszakítja, msvsmon.exe leválik hello folyamat.
>
>

a példány vagy szerepkör, nyissa meg hello helyi menü hello szerepkör vagy a példány hibakeresése, és válassza ki az összes folyamatok toodetach hello hibakereső **leválasztani hibakereső**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Korlátozások a távoli hibakeresés az Azure-ban
Az Azure SDK 2.3 távoli hibakeresés rendelkezik hello a következő korlátozások vonatkoznak.

* Távoli hibakeresés engedélyezésével, egy felhőalapú szolgáltatás, amelyben minden olyan szerepkört 25-nél több példánya van nem tehető közzé.
* hello hibakereső portok 30400 too30424, 31400 too31424 és 32400 too32424 használja. Ha toouse valamelyik portot, akkor a szolgáltatáshoz, és a következő hibaüzenetek hello közül fog megjelenni hello tevékenységnapló Azure képes toopublish:

  * Hiba történt a hello .cscfg fájl .csdef fájl hello ellenőrzése.
    hello porttartomány "tartomány" szerepkör "szerepkör" Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector átfedésben van egy már definiált portot vagy porttartományt végpont fenntartva.
  * A lefoglalás sikertelen. Próbálkozzon újra később, próbálja meg csökkenteni a hello Virtuálisgép-méretet vagy a szerepkörpéldányok számát, vagy próbálja tooa más régióban üzembe helyezni.

## <a name="debugging-azure-virtual-machines"></a>Az Azure virtuális gépek hibakeresés
A Visual Studio Server Explorer használatával az Azure virtuális gépeken futó programok megoldhassuk. Ha engedélyezi az Azure virtuális géphez távoli hibakeresés, az Azure hello távoli hibakeresési bővítmény telepítését hello virtuális gépen. Majd amellyel tooprocesses hello virtuális gépen, és hibakeresése a szokásos módon.

> [!NOTE]
> Hello Azure resource manager-készletben használatával létrehozott virtuális gépeket a Visual Studio 2015 Cloud Explorer használatával távolról indítja is. További információkért lásd: [Azure-erőforrások kezelése Cloud Explorer](http://go.microsoft.com/fwlink/?LinkId=623031).
>
>

### <a name="toodebug-an-azure-virtual-machine"></a>egy Azure virtuális gép toodebug
1. A Server Explorer eszközben bontsa ki a hello virtuális gépek csomópontnak, illetve hello virtuális gép, amelyet az toodebug válassza hello csomópontnak.
2. Hello helyi menü megnyitásához, és válassza ki **hibakeresés engedélyezése**. Amikor a rendszer kéri, ha megbizonyosodott arról, hogy, ha azt szeretné, hogy a tooenable hello virtuális gépet, jelölje be a hibakeresés **Igen**.

    Azure hello távoli hibakeresési bővítmény telepítése hello virtuális gép tooenable hibakeresést.

    ![Virtuális gép hibakeresési parancs engedélyezése](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure tevékenységnapló](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
3. Hello távoli hibakeresési bővítmény befejezése telepítése után nyissa meg a hello virtuális gép helyi menüt, majd válassza **csatolása hibakereső...**

    Azure hello folyamatok listájának lekérése hello virtuális gépen, és azt is hello Attach tooProcess párbeszédpanelen.

    ![A hibakereső parancs csatolása](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
4. A hello **tooProcess csatolása** párbeszédpanelen jelölje ki **kiválasztása** toolimit hello eredmények Típuslista tooshow csak hello kód toodebug kívánt. 32 vagy 64-bites felügyelt kódot, natív kóddal vagy mindkettő megoldhassuk.

    ![Válassza ki a kód párbeszédpanel](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
5. Válassza ki a kívánt toodebug hello virtuális gépen, és válassza ki a hello folyamatok **Attach**. Például hello w3wp.exe folyamat célszerű használni, ha a keresett toodebug a webes alkalmazás hello virtuális gépen. Lásd: [Debug egy vagy több folyamatot, a Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) és [Azure szerepkör architektúra](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) további információt.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Webes projektet és a hibakereséshez a virtuális gép létrehozása
Az Azure-projekt közzététele, előfordulhat, hogy hasznos tootest tartalmazott környezetben, amely támogatja a Hibakeresés és tesztelési forgatókönyvek, és ahol tesztelése és programok figyelése telepítheti azt. Egyirányú toodo Ez az tooremotely alkalmazás hibakeresése a virtuális gépen.

A Visual Studio ASP.NET projektek ajánlatot egy beállítás toocreate alkalmazás teszteléséhez használható virtuális gép lesz szüksége. hello virtuális gép végpontok általában szükség például PowerShell, a távoli asztal és a WebDeploy tartalmazza.

### <a name="toocreate-a-web-project-and-a-virtual-machine-for-debugging"></a>toocreate webes projektet és a hibakereséshez a virtuális gép
1. A Visual Studióban hozzon létre egy új ASP.NET Webalkalmazásként való kezelése.
2. Hello új ASP.NET projekt párbeszédpanelje, hello Azure területen válassza a **virtuális gép** hello legördülő listában. Hagyja hello **távoli erőforrások létrehozása** jelölőnégyzet be van jelölve. Válassza ki **OK** tooproceed.

    Hello **virtuális gép létrehozása az Azure** párbeszédpanel jelenik meg.

    ![ASP.NET webes projekt párbeszédpanel létrehozása](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Megjegyzés:** meg kell adnia az Azure-fiók tooyour toosign ha van már nincs bejelentkezve.

1. Válassza ki a hello hello virtuális gép különböző beállításait, és válassza ki **OK**. Lásd: [virtuális gépek](http://go.microsoft.com/fwlink/?LinkId=623033) további információt.

    hello név DNS-név lesz hello hello virtuális gép nevét.

    ![Virtuális gép létrehozása Azure párbeszédpanel](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure hello virtuális gépet, majd a rendelkezések hoz létre, és konfigurálja a hello végpontok, például a távoli asztal és a Web Deploy
2. Hello virtuális gép teljes konfigurálása után válassza ki a Server Explorer hello virtuális gép csomópontját.
3. Hello helyi menü megnyitásához, és válassza ki **hibakeresés engedélyezése**. Amikor a rendszer kéri, ha megbizonyosodott arról, hogy, ha azt szeretné, hogy a tooenable hello virtuális gépet, jelölje be a hibakeresés **Igen**.

    Azure telepíti hello távoli hibakeresési bővítmény toohello virtuális gép tooenable hibakeresést.

    ![Virtuális gép hibakeresési parancs engedélyezése](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure tevékenységnapló](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)
4. A projekt közzététele a [Útmutató: a webes projekt használ egy kattintással közzététele a Visual Studio telepítése](https://msdn.microsoft.com/library/dd465337.aspx). Mivel toodebug hello virtuális gépen, a hello **beállítások** hello oldalán **webhely közzététele** varázslóban válassza **Debug** hello beállításként. Ez biztosítja, hogy kód szimbólumok hibakeresés során rendelkezésre álló is.

    ![Közzétételi beállítások](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)
5. A hello **Fájlközzétételi beállítás**, jelölje be **célhelyen további fájlok eltávolítása** Ha hello projekt korábban már telepítve lett.
6. Miután közzéteszi hello projekt hello virtuális gép helyi menüben a Server Explorer eszközben válassza **csatolása hibakereső...**

    Azure hello folyamatok listájának lekérése hello virtuális gépen, és azt is hello Attach tooProcess párbeszédpanelen.

    ![A hibakereső parancs csatolása](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)
7. A hello **tooProcess csatolása** párbeszédpanelen jelölje ki **kiválasztása** toolimit hello eredmények Típuslista tooshow csak hello kód toodebug kívánt. 32 vagy 64-bites felügyelt kódot, natív kóddal vagy mindkettő megoldhassuk.

    ![Válassza ki a kód párbeszédpanel](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)
8. Válassza ki a kívánt toodebug hello virtuális gépen, és válassza ki a hello folyamatok **Attach**. Például hello w3wp.exe folyamat célszerű használni, ha a keresett toodebug a webes alkalmazás hello virtuális gépen. Lásd: [Debug egy vagy több folyamatot, a Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) további információt.

## <a name="next-steps"></a>Következő lépések
* Használjon **Intellitrace** toocollect hívások és a kiadási kiszolgálótól érkező események naplózása. Lásd: [egy közzétett Felhőszolgáltatás IntelliTrace és a Visual Studio hibakeresési](http://go.microsoft.com/fwlink/?LinkID=623016).
* Használjon **Azure Diagnostics** toolog részletes kód futását szerepköröket, az adatokat, hogy hello szerepkörök hello fejlesztői környezetben vagy az Azure-ban futtatja. Lásd: [naplózási adatok gyűjtése az Azure Diagnostics használatával](http://go.microsoft.com/fwlink/p/?LinkId=400450).
