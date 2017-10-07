---
title: "aaaPrepare toopublish vagy a Visual Studio az Azure-alkalmazások telepítése |} Microsoft Docs"
description: "Hello eljárások tooset felhő- és tárolási fiók szolgáltatások be további, és állítsa be az Azure alkalmazását."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 92ee2f9e-ec49-4c7a-900d-620abe5e9d8a
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: b5231d400e2ad9e20c3f21bad48a77c328b1f7a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-toopublish-or-deploy-an-azure-application-from-visual-studio"></a>TooPublish előkészítéséhez, illetve az Azure-alkalmazás, a Visual Studio telepítése
## <a name="overview"></a>Áttekintés
Egy felhőszolgáltatás-projekt közzététele előtt be kell állítania a következő szolgáltatások hello:

* A **felhőalapú szolgáltatás** toorun a szerepkörök az Azure-alapú környezetben hello
* A **tárfiók** , amely hozzáférést biztosít toohello Blob, Queue és Table szolgáltatások.

A következő eljárások tooset be ezeket a szolgáltatásokat hello használja, és állítsa be az alkalmazását

## <a name="create-a-cloud-service"></a>Felhőszolgáltatás létrehozása
egy felhőalapú szolgáltatás tooAzure toopublish, akkor először létre kell hoznia egy felhőalapú szolgáltatás, amely futtat a szerepkörök hello Azure környezetben. Létrehozhat egy felhőalapú szolgáltatás hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885)hello szakaszban leírtak szerint **toocreate egy felhőalapú szolgáltatás, a klasszikus Azure portálon hello**, ez a témakör későbbi részében. Hello közzététele varázsló segítségével a Visual Studio is létrehozható egy felhőalapú szolgáltatás.

### <a name="toocreate-a-cloud-service-by-using-visual-studio"></a>toocreate egy felhőalapú szolgáltatás, a Visual Studio használatával
1. Hello hello Azure-projekt helyi menüjének megnyitásához, és válassza a **közzététel**.

    ![VST_PublishMenu](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/vst-publish-menu.png)
2. Ha még nem jelentkezett be, jelentkezzen be a felhasználónevét és jelszavát hello Microsoft-fiókkal vagy szervezeti fiókkal az Azure-előfizetéshez társított.
3. Válassza ki a hello **következő** gomb tooadvance toohello **beállítások** lap.

    ![Közzétételi varázsló általános beállítások](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/publish-settings-page.png)
4. A hello **Felhőszolgáltatások** menüben válassza ki **hozzon létre új**. Hello **létrehozása Azure-szolgáltatások** párbeszédpanel jelenik meg.
5. Adja meg a felhőalapú szolgáltatás hello nevét. hello neve hello URL-címet, a szolgáltatás részét képezi, és ezért egyedinek kell lenniük. hello név nincs kis-és nagybetűket.

### <a name="toocreate-a-cloud-service-by-using-hello-azure-classic-portal"></a>toocreate egy felhőalapú szolgáltatás hello klasszikus Azure portál használatával
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkId=253103) hello Microsoft webhelyen.
2. (választható) toodisplay felhőalapú szolgáltatások, amelyek a már létrehozott, válassza a hello Felhőszolgáltatások hivatkozás hello bal oldalán található hello.
3. Válassza ki a hello  **+**  hello bal alsó sarokban, és válassza a **Felhőszolgáltatás** hello menüben megjelenő. Két lehetőség közül választhat, egy másik képernyő **Gyorslétrehozás** és **egyéni létrehozás**, akkor jelenik meg. Ha úgy dönt, **Gyorslétrehozás**, létrehozhat egy felhőalapú szolgáltatás csak az URL-cím és hello régió, ahol fizikailag tárolható megadásával. Ha úgy dönt, **egyéni létrehozás**, azonnal közzététele egy felhőszolgáltatás egy csomagot (.cspkg fájl), a konfigurációs (.cscfg) fájljában és tanúsítvány megadásával. Egyéni létrehozás nem szükséges, ha azt tervezi, toopublish a felhőalapú szolgáltatás hello segítségével **közzététel** Azure-projekt parancsot. Hello **közzététel** parancs az Azure-projekt helyi menüjének hello érhető el.
4. Válasszon **Gyorslétrehozás** toolater közzététele a felhőalapú szolgáltatás Visual Studio használatával.
5. Adjon meg egy nevet a felhőalapú szolgáltatás. hello teljes URL-CÍMÉT a következő toohello neve jelenik meg.
6. Hello listában válassza ki a hello régió, ahol a felhasználók a legtöbb találhatók.
7. Hello ablak hello alján válassza a hello **felhőalapú szolgáltatás létrehozása** hivatkozásra.

## <a name="create-a-storage-account"></a>Create a storage account
A tárfiók a Blob, Queue és Table szolgáltatások toohello hozzáférést biztosít. Visual Studio vagy hello segítségével létrehozhat egy tárfiókot [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkId=253103).

### <a name="toocreate-a-storage-account-by-using-visual-studio"></a>toocreate egy tárfiókot, a Visual Studio használatával
1. A **Megoldáskezelőben**, nyissa meg hello helyi menüje hello **tárolási** csomópont, és válassza a **Storage-fiók létrehozása**.

    ![Új Azure storage-fiók létrehozása](./media/vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio/IC744166.png)
2. Válassza ki vagy adja meg a következő információ hello hello az új tárfiók hello **Storage-fiók létrehozása** párbeszédpanel megnyitásához.

   * hello Azure-előfizetés toowhich tooadd hello tárfiókot szeretne.
   * hello nevet toouse hello új tárfiók.
   * hello régiót vagy affinitáscsoportot (például az USA nyugati régiója vagy Kelet-Ázsia).
   * hello típus replikációs hello tárfiók, például a Georedundáns kívánt toouse.
3. Amikor elkészült, válassza ki a **létrehozása**.hello új tárfiók megjelenik hello **tárolási** listájában **Server Explorer**.

### <a name="toocreate-a-storage-account-by-using-hello-azure-classic-portal"></a>a tárfiók a klasszikus Azure portálon hello toocreate
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkId=253103) hello Microsoft webhelyen.
2. (Választható) tooview a storage-fiókok válasszon hello **tárolási** hello panel bal oldalán található hello hello a hivatkozásra.
3. Hello lap hello bal alsó sarkában, válassza ki a hello  **+**  ikonra.
4. Hello a helyi menüben válassza a **tárolási**, és válassza a **Gyorslétrehozás**.
5. Adjon meg olyan nevet, egy egyedi URL-címet fog eredményezni hello tárfiók.
6. Nevezze el a felhőalapú szolgáltatás. hello teljes URL-CÍMÉT a következő toohello neve jelenik meg.
7. Régiók hello listában válasszon egy régiót, ahol a felhasználók a legtöbb találhatók.
8. Adja meg, hogy tooenable a georeplikáció. Georeplikáció engedélyezéséhez, ha az adatok több fizikai hellyel tooreduce hello esélye adatvesztés, a rendszer menti. A szolgáltatás révén tárolási drágább, de hello költsége helyett hello szolgáltatás később hello storage-fiók létrehozásakor, mivel lehetővé teszi, hogy földrajzi helyhez csökkentése érdekében. További információkért lásd: [georeplikáció](http://go.microsoft.com/fwlink/?LinkId=253108).
9. Hello ablak hello alján válassza a hello **Storage-fiók létrehozása** hivatkozásra.

A tárfiók létrehozása után hello URL-címeket is használja tooaccess erőforrások minden hello Azure storage szolgáltatások, és a fiók elsődleges és másodlagos kulcsok is hello jelenik meg. Használja a kulcsok tooauthenticate kérelmek hello tárolószolgáltatások ellen.

> [!NOTE]
> hello másodlagos elérési kulcsát hello azonos tooyour tárfiók hello elsődleges elérési kulcs eléréséhez, és jön létre biztonsági az elsődleges elérési kulcsát megsértik biztosít. Emellett ajánlott, hogy a tárelérési kulcsok rendszeresen újragenerálja. Módosíthatja a kapcsolati karakterlánc beállítási toouse hello másodlagos kulcs közben újragenerálja hello elsődleges kulcsot, majd módosíthatja toouse újragenerálása hello elsődleges kulcs közben hello másodlagos kulcs újragenerálása.
>
>

## <a name="configure-your-app-toouse-services-provided-by-hello-storage-account"></a>A tárfiók hello által biztosított alkalmazás toouse szolgáltatások konfigurálása
Olyan szerepkört, amely hozzáfér a tárolási szolgáltatások toouse hello Azure storage szolgáltatások létrehozott kell konfigurálnia. toodo, több szolgáltatáskonfiguráció használható az Azure-projekt. Alapértelmezés szerint két jönnek létre az Azure-projekt. Több szolgáltatáskonfiguráció használatával hello ugyanazt a kapcsolatot a kódban karakterlánc, de más értéket a kapcsolati karakterlánc szerepel minden szolgáltatás konfigurációját is használhatja. Például egy szolgáltatás konfigurációs toorun használja, és az alkalmazás helyileg használatával hello az Azure storage emulator és egy másik szolgáltatás konfigurációs toopublish az alkalmazás tooAzure debug. Szolgáltatás-konfigurációkkal kapcsolatos további információkért lásd: [konfigurálása az Azure projekt használatával több szolgáltatáskonfiguráció](vs-azure-tools-multiple-services-project-configurations.md).

### <a name="tooconfigure-your-application-toouse-services-that-hello-storage-account-provides"></a>tooconfigure a toouse alkalmazásszolgáltatások, amely a tárfiók hello biztosít
1. A Visual Studióban nyissa meg az Azure megoldást. A Megoldáskezelőben, nyissa meg az Azure-projekt hello tárolószolgáltatások hozzáférő hello helyi menü az egyes szerepkörökhöz, és válassza ki **tulajdonságok**. Hello Visual Studio-szerkesztőben hello szerepkör hello nevű oldal jelenik meg. hello lap megjeleníti a hello hello mezők **konfigurációs** fülre.
2. Hello szerepkör hello tulajdonságlapokat, válassza a **beállítások**.
3. A hello **szolgáltatáskonfiguráció** menüben válassza ki hello szolgáltatás konfigurációja hello nevét, amelyet az tooedit. Ha azt szeretné, hogy toomake módosítások tooall hello szolgáltatás konfigurációk ezt a szerepkört, kiválaszthatja **összes konfiguráció**.  Hogyan tooupdate szolgáltatás konfigurációk kapcsolatos további információkért lásd: hello szakasz **Storage-fiókok kezelése kapcsolati karakterláncok** hello témakör [a Visual Studio Azure Cloud Service hello szerepkörök konfigurálása ](vs-azure-tools-configure-roles-for-cloud-service.md).
4. toomodify bármely kapcsolódási karakterlánc beállításainak kiválasztása hello **...** következő toohello beállítás gombra. Hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanel jelenik meg.
5. A **protokoll használatával kapcsolódó levelezőprogramokkal**, válassza ki a hello **az előfizetés** lehetőséget.
6. A hello **előfizetés** menüben válassza ki az előfizetését. Ha hello előfizetések listája nem tartalmazza a hello egy használni kívánt, válassza ki azt a hello **közzétételi beállítások letöltése** hivatkozásra.
7. A hello **fióknév** menüben válassza ki a tárfiók nevére. Azure-eszközök jut hozzá a tárfiók hitelesítő adatainak automatikusan hello .publishsettings fájl használatával. a tárfiók hitelesítő adatokat kézzel, toospecify válasszon hello **manuálisan kell megadni a hitelesítő adatok** lehetőséget, majd folytassa az eljárás a. A tárfiók nevét és az elsődleges kulcs lekérheti hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/p/?LinkID=213885). Ha nem szeretné, hogy toospecify a tárolási fiók beállításainak manuális, válassza ki a hello **OK** gomb tooclose hello párbeszédpanel.
8. Válassza ki a hello **adja meg a tárfiók** hitelesítő adatok hivatkozásra.
9. A hello **fióknév** mezőbe írja be a tárfiók hello nevét.

   > [!NOTE]
   > Jelentkezzen be a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), majd válassza a hello **tárolási** gombra. hello portal storage-fiókok listáját tartalmazza. Ha úgy dönt, hogy a fiók, az oldal jelenik meg. Ezen a lapon hello tárfiók hello nevét másolhatja. Klasszikus portál hello korábbi verziójának használatakor a tárfiók neve hello megjelenik-e a hello **Tárfiókok** nézet. toocopy ez nevével, kiemelés hello **tulajdonságok** ablakban a megtekintése, és válassza a Ctrl-C hello kulcsok. Visual Studio toopaste hello nevű válasszon hello **fióknév** szöveg mezőbe, majd kattintson a hello Ctrl + V kulcsok.
   >
   >
10. A hello **fiókkulcs** mezőben, adja meg az elsődleges kulcsot, vagy másolja és illessze be a hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885).
     a kulcs toocopy:

    1. Alján hello hello hello megfelelő tárfiók, válassza ki a hello **kulcsok kezelése** gombra.
    2. A hello **kulcsok hozzáférés kezelése** lapon, válassza ki a hello elsődleges elérési kulcsát hello szöveget, és válassza a hello Ctrl + C kulcsok.
    3. Az Azure-eszközök, illessze be hello kulcs hello **fiókkulcs** mezőbe.
    4. Válasszon egyet a következő beállítások toodetermine hello hello szolgáltatás miként férhetnek hozzá a tárfiók hello:

       * **A HTTP használata**. Ez a lehetőség hello szabványos. Például: `http://<account name>.blob.core.windows.net`.
       * **A HTTPS PROTOKOLLT használja** a biztonságos kapcsolat. Például: `https://<accountname>.blob.core.windows.net`.
       * **Egyéni végpontokat határoz meg** hello három szolgáltatások mindegyikéhez. Majd hello adott szolgáltatás hello mezőjébe írja be ezeket a végpontokat.

         > [!NOTE]
         > Ha egyéni végpontokat hoz létre, létrehozhat egy összetettebb kapcsolati karakterláncot. Ez a karakterlánc-formátum használata esetén a tárfiók a Blob szolgáltatás hello regisztrált egyéni tartománynevet tartalmazó tároló Szolgáltatásvégpontok is megadhat. Is hozzáférést biztosíthat a közös hozzáférésű jogosultságkód keresztül egyetlen tárolót csak tooblob erőforrásokat. További információ toocreate egyéni végpontokat, lásd: [konfigurálása Azure Storage kapcsolati karakterláncok](storage/common/storage-configure-connection-string.md).
         >
         >
11. toosave kapcsolati karakterlánc módosítások válasszon hello **OK** gombra, majd kattintson a hello **mentése** hello gombjára. A módosítások mentése után kaphat hello Ez a kapcsolati karakterlánc értékét a kódban használatával [GetConfigurationSettingValue](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.getconfigurationsettingvalue.aspx). Az alkalmazás tooAzure közzétételekor válasszon hello szolgáltatás konfigurációja, amely hello Azure storage-fiók hello kapcsolati karakterláncot tartalmaz. Az alkalmazás közzététele után győződjön meg arról, hogy hello alkalmazás megfelelően működik-e hello Azure storage szolgáltatások ellen

## <a name="next-steps"></a>Következő lépések
toolearn közzététele a Visual Studio eszközből alkalmazások tooAzure kapcsolatos további információkért lásd: [közzététele a felhőalapú szolgáltatás hello Azure eszközökkel](vs-azure-tools-publishing-a-cloud-service.md).
