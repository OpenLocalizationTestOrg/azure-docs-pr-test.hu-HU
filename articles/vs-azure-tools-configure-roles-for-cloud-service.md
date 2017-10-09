---
title: "aaaConfigure hello szerepkörök az Azure felhőalapú szolgáltatás, a Visual Studio |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset össze, és az Azure felhőszolgáltatások Visual Studio használatával szerepkörök konfigurálásához."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: d397ef87-64e5-401a-aad5-7f83f1022e16
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: d3c62eb57040ebe987787e73b17b468bb82122bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-cloud-service-roles-with-visual-studio"></a>A Visual Studio Azure cloud service szerepkörök konfigurálása
Azure-felhőszolgáltatás rendelkezhet egy vagy több munkavégző vagy a webes szerepkörök. Az egyes szerepkörökhöz kell, hogy a szerepkör beállítására toodefine és is konfigurálhatja, hogyan fut a szerepkörhöz. További információ a felhőalapú szolgáltatások szerepkörök toolearn lásd: hello videó [bemutatása tooAzure Felhőszolgáltatások](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). 

a felhőszolgáltatás hello adatokat következő fájlok hello tárolja:

- **ServiceDefinition.csdef** -hello szolgáltatásdefiníciós fájlt a milyen szerepkörre szükség, beleértve a felhőalapú szolgáltatás, a végpontok és a virtuálisgép-méret hello futásidejű beállításokat határoz meg. Nincs a hello adataihoz `ServiceDefinition.csdef` módosítható, ha a szerepkör fut..
- **ServiceConfiguration.cscfg** - hello szolgáltatás konfigurációs fájlja konfigurálja, hogy hány példánya egy futnak, és az egy szerepkörhöz definiált hello hello beállítások értékeit. a tárolt adatok hello `ServiceConfiguration.cscfg` módosíthatja a szerepkör futása közben.

egy szerepkör működésével vezérlőhöz hello-beállítások értékei különböző toostore, meghatározhatja, hogy több szolgáltatáskonfiguráció. Minden környezet különböző szolgáltatáskonfiguráció használható. Például állítsa be a tárolási fiók kapcsolati karakterlánc toouse hello helyi az Azure storage emulator egy helyi szolgáltatás konfigurációjában, és hozzon létre egy másik szolgáltatás konfigurációs toouse az Azure storage hello felhőben.

Azure-felhőszolgáltatás létrehozása a Visual Studióban, ha a két szolgáltatáskonfiguráció automatikusan jönnek létre, és a hozzáadott Azure-projekt tooyour:

- `ServiceConfiguration.Cloud.cscfg`
- `ServiceConfiguration.Local.cscfg`

## <a name="configure-an-azure-cloud-service"></a>Azure-felhőszolgáltatás konfigurálása
A Visual Studio megoldáskezelőjében egy Azure felhőszolgáltatást konfigurálhatja, ahogy az alábbi lépésekkel hello:

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt hello és, hello helyi menüből válassza ki a **tulajdonságok**.
   
    ![Solution Explorer projekt helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-project-context-menu.png)

1. Hello projekt Tulajdonságok lapján válassza hello **fejlesztési** fülre. 

    ![Projekt párbeszédpanel - fejlesztési lap](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-development-tab.png)

1. A hello **szolgáltatáskonfiguráció** listán, válassza hello nevét, amelyet az tooedit hello szolgáltatás konfigurációját. (Ha toomake változik tooall hello szolgáltatáskonfiguráció ehhez a szerepkörhöz, jelölje be **összes konfiguráció**.)
   
    > [!IMPORTANT]
    > Ha úgy dönt, hogy egy adott szolgáltatás konfigurációs, néhány tulajdonságát le vannak tiltva, mivel azok csak az összes konfiguráció állítható be. tooedit ezeket a tulajdonságokat, ki kell választania **összes konfiguráció**.
    > 
    > 
   
    ![Azure-felhőszolgáltatás szolgáltatáskonfiguráció listája](./media/vs-azure-tools-configure-roles-for-cloud-service/cloud-service-service-configuration-property.png)

## <a name="change-hello-number-of-role-instances"></a>A szerepkörpéldányok számát hello módosítása
a felhőalapú szolgáltatás tooimprove hello teljesítményétől, módosíthatja hello hány példánya egy szerepkört futtató, felhasználók vagy egy adott szerepkör várt hello betöltése hello száma alapján. Egy különálló virtuális gép szerepkör minden egyes példányánál létrejön, amikor hello felhőalapú szolgáltatás fut az Azure-ban. Ez hatással van a hello számlázási hello telepítését a felhőalapú szolgáltatás. Számlázással kapcsolatos további információkért lásd: [a számlázási megérteni a Microsoft Azure](billing/billing-understand-your-bill.md).

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára. A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Jelölje be hello **konfigurációs** fülre.

    ![Konfiguráció lap](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page.png)

1. A hello **szolgáltatáskonfiguráció** listában, jelölje be hello szolgáltatás konfigurációja, amelyet az tooupdate.
   
    ![Konfigurációs szolgáltatás listája](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-select-configuration.png)

1. A hello **száma példány** szöveg mezőbe írja be a kívánt toostart a szerepkör-példányok hello száma. Minden példány egy különálló virtuális gép futó, hello cloud service tooAzure közzétételekor.

    ![A példányok száma hello frissítése](./media/vs-azure-tools-configure-roles-for-cloud-service/role-configuration-properties-page-instance-count.png)

1. A Visual Studio hello eszköztárában kattintson **mentése**.

## <a name="manage-connection-strings-for-storage-accounts"></a>Kapcsolati karakterláncok storage-fiókok kezelése
Hozzáadhat, távolítsa el, vagy módosítsa a szolgáltatáskonfiguráció-kapcsolati karakterláncok. Például érdemes egy helyi kapcsolati karakterláncot egy helyi szolgáltatás konfigurációjához, amely értéke `UseDevelopmentStorage=true`. Érdemes lehet tooconfigure egy felhőalapú szolgáltatás konfigurációja, amely egy tárfiókot használja az Azure-ban.

> [!WARNING]
> Hello az Azure storage fiókhoz tartozó nyilvánoskulcs-adatokat egy tárolási fiók kapcsolati karakterláncot adja meg, ezeket az információkat tárolja helyileg hello szolgáltatás konfigurációs fájljában. Azonban ez az információ nem tárolt titkosított szövegként.
> 
> 

Minden szolgáltatás konfigurációját egy másik értéket használ, nem toouse eltérő kapcsolódási karakterláncokkal rendelkezik a felhőalapú szolgáltatás, vagy nem módosíthatja a kódot, a cloud service tooAzure közzétételekor. Használhatja ugyanazt a nevet a kódot és hello érték hello kapcsolati karakterlánc nem egyezik a felhőalapú szolgáltatás építésekor, vagy amikor közzétesszük kiválasztott hello szolgáltatás konfigurációja alapján hello.

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára. A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Jelölje be hello **beállítások** fülre.

    ![Beállítások lap](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. A hello **szolgáltatáskonfiguráció** listában, jelölje be hello szolgáltatás konfigurációja, amelyet az tooupdate.

    ![Szolgáltatás konfigurációja](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. egy kapcsolati karakterláncot tooadd kiválasztása **beállítás hozzáadása**.

    ![Kapcsolati karakterlánc hozzáadása](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Új beállítás hello toohello lista hozzá lett adva, frissíti hello listában hello sort hello szükséges információkat.

    ![Új kapcsolati karakterlánc](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Név** -adja meg, amelyet az toouse hello kapcsolati karakterlánc hello nevét.
    - **Típus** – Itt adhatja meg **kapcsolati karakterlánc** hello legördülő listából.
    - **Érték** -megadhat a hello kapcsolati karakterlánc közvetlenül a hello **érték** cellára, vagy válassza hello három ponttal (…) toowork a hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanel.  

1. A hello **tárolási kapcsolati karakterlánc létrehozása** párbeszédpanelen válasszon egy beállítást **protokoll használatával kapcsolódó levelezőprogramokkal**. Kövesse a hello beállítástól hello utasításokat:

    - **A Microsoft Azure storage emulator** -ezt a beállítást, ha az hello fennmaradó beállításokat a következő párbeszédpanelen: hello le vannak tiltva csak tooAzure vonatkoznak. Kattintson az **OK** gombra.
    - **Az előfizetés** – Ha válassza ki a beállítást, használja tooeither hello legördülő listában válassza ki, és jelentkezzen be Microsoft-fiókba, vagy adja hozzá a Microsoft-fiókkal. Válassza ki az Azure-előfizetés és a tárolási fiók. Kattintson az **OK** gombra.
    - **Manuálisan kell megadni a hitelesítő adatok** – hello tárfiók nevét adja meg, és vagy hello második vagy elsődleges kulcs. Válassza ki a **kapcsolat** (HTTPS ajánlott a legtöbb esetben.) Kattintson az **OK** gombra.

1. toodelete egy kapcsolati karakterláncot hello kapcsolati karakterláncot, majd válassza ki és **eltávolítása beállítás**.

1. A Visual Studio hello eszköztárában kattintson **mentése**.

## <a name="programmatically-access-a-connection-string"></a>A kapcsolati karakterlánc programon keresztüli eléréséhez

hello következő lépések bemutatják, hogyan férhetnek hozzá a tooprogrammatically egy kapcsolati karakterláncot, amely C#.

1. Adja hozzá a következő hello toouse hello beállítás hová irányelvek tooa C#-fájl használatával:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. hello alábbi kód bemutatja, hogyan például tooaccess egy kapcsolati karakterláncot. Cserélje le a hello &lt;ConnectionStringName > helyőrzőt hello megfelelő értékre. 

    ```csharp
    // Setup hello connection tooAzure Storage
    var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("<ConnectionStringName>"));
    ```

## <a name="add-custom-settings-toouse-in-your-azure-cloud-service"></a>Egyéni beállítások toouse adja hozzá az Azure-felhőszolgáltatás
Hello szolgáltatás konfigurációs fájljában egyéni beállítások segítségével úgy adja hozzá egy nevet és értéket egy adott szolgáltatás-konfigurációt. Ez a beállítás tooconfigure hello értékének olvasásával a felhőszolgáltatásban található egy szolgáltatás és az érték toocontrol hello logika használatához a kódban hello toouse célszerű használni. Ezeket a szolgáltatás konfigurációs értékeket módosíthatja, anélkül, hogy toorebuild a szolgáltatáscsomag vagy a felhőalapú szolgáltatás futása közben. A kódot is keresése az értesítések, amikor a beállítások módosítása. Lásd: [RoleEnvironment.Changing esemény](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Is hozzáadhat, távolítsa el, vagy módosítsa a szolgáltatáskonfiguráció egyéni beállításait. Ezek a különböző szolgáltatáskonfiguráció karakterláncok érdemes eltérő értékek tartoznak.

Minden szolgáltatás konfigurációját egy másik értéket használ, nem rendelkezik toouse különböző karakterláncokkal a felhőszolgáltatásban vagy módosítsa a kódot, a cloud service tooAzure közzétételekor. Használhatja ugyanazt a nevet a kódot és hello értékének hello karakterlánc nem egyezik a felhőalapú szolgáltatás építésekor, vagy amikor közzétesszük kiválasztott hello szolgáltatás konfigurációja alapján hello.

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára. A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Jelölje be hello **beállítások** fülre.

    ![Beállítások lap](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab.png)

1. A hello **szolgáltatáskonfiguráció** listában, jelölje be hello szolgáltatás konfigurációja, amelyet az tooupdate.

    ![Konfigurációs szolgáltatás listája](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-select-configuration.png)

1. tooadd egy egyéni beállítás kiválasztása **beállítás hozzáadása**.

    ![Egyéni Alkalmazásbeállítás hozzáadása](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting.png)

1. Új beállítás hello toohello lista hozzá lett adva, frissíti hello listában hello sort hello szükséges információkat.

    ![Új egyéni beállítása](./media/vs-azure-tools-configure-roles-for-cloud-service/project-properties-settings-tab-add-setting-new-setting.png)

    - **Név** -hello beállítás hello nevét adja meg.
    - **Típus** – Itt adhatja meg **karakterlánc** hello legördülő listából.
    - **Érték** -hello hello beállítás értékét adja meg. Megadhat a hello érték közvetlenül a hello **érték** cella, vagy válassza hello három ponttal (…) tooenter hello értéket hello **karakterlánc szerkesztése** párbeszédpanel.  

1. toodelete egy egyéni beállítás hello beállítás, majd válassza ki és **eltávolítása beállítás**.

1. A Visual Studio hello eszköztárában kattintson **mentése**.

## <a name="programmatically-access-a-custom-settings-value"></a>Programon keresztüli eléréséhez az egyéni beállítás értéke
 
hello következő lépések bemutatják, hogyan férhetnek hozzá a tooprogrammatically egy C# segítségével egyéni beállítást.

1. Adja hozzá a következő hello toouse hello beállítás hová irányelvek tooa C#-fájl használatával:

    ```csharp
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. hello alábbi kód bemutatja, hogyan lehet például egy egyéni beállítás tooaccess. Cserélje le a hello &lt;SettingName > helyőrzőt hello megfelelő értékre. 
    
    ```csharp
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("<SettingName>");
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Minden egyes szerepkörpéldányhoz helyi tárterület kezelése
Egy szerepkör minden egyes példányánál helyi fájl rendszerhez szükséges tárhelyet adhat hozzá. hello található adatokhoz tároló nem érhető el más példánya hello mely hello vonatkozó adatokat tárolja, vagy más szerepköröket.  

1. Hozzon létre, vagy a Visual Studio Azure cloud service projekt megnyitása.

1. A **Megoldáskezelőben**, bontsa ki a hello projekt csomópontjára. A hello **szerepkörök** csomópontot, kattintson a jobb gombbal hello szerepkör tooupdate, és, hello helyi menüből jelöljük ki **tulajdonságok**.

    ![Solution Explorer Azure szerepkör helyi menü](./media/vs-azure-tools-configure-roles-for-cloud-service/solution-explorer-azure-role-context-menu.png)

1. Jelölje be hello **helyi tároló** fülre.

    ![Helyi tárolás lap](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab.png)

1. A hello **szolgáltatáskonfiguráció** listában, ügyeljen arra, hogy **összes konfiguráció** van jelölve, hogy hello helyi tárolási beállítások tooall szolgáltatáskonfiguráció alkalmazása. Semmilyen más érték eredményez az összes hello beviteli mezők hello lapon le van tiltva. 

    ![Konfigurációs szolgáltatás listája](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-service-configuration.png)

1. a helyi tároló bejegyzés tooadd válassza **hozzáadása helyi tároló**.

    ![Adja hozzá a helyi tároló](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-add-local-storage.png)

1. Hello új helyi tároló bejegyzés toohello lista hozzáadva, frissíti hello listában hello sort hello szükséges információkat.

    ![Új helyi tárterület-bejegyzés](./media/vs-azure-tools-configure-roles-for-cloud-service/role-local-storage-tab-new-local-storage.png)

    - **Név** -adja meg, amelyet az toouse a hello új helyi tárolóhoz tartozó hello nevét.
    - **Méret (MB)** -adja meg a hello méretét, amelyekre szüksége van a hello új helyi tárolóhoz tartozó MB-ban.
    - **Tiszta a szerepkör újrahasznosítást** – Ez a beállítás tooremove hello adatok hello új helyi tároló válassza ki, amikor hello szerepkör hello virtuális gép újraindul.

1. toodelete egy helyi tárolóhoz bejegyzés hello bejegyzést, majd válassza ki és **helyi tárhely eltávolítása**.

1. A Visual Studio hello eszköztárában kattintson **mentése**.

## <a name="programmatically-accessing-local-storage"></a>Programozott módon a helyi tároló elérése

Ez a szakasz bemutatja, miként férhetnek hozzá a tooprogrammatically helyi tárolóhely-C# teszt szövegfájlba írásával `MyLocalStorageTest.txt`.  

### <a name="write-a-text-file-toolocal-storage"></a>A szöveges fájl toolocal tárolási írása

a következő kód hello azt szemlélteti, hogyan toowrite a szöveges fájl toolocal tároló. Cserélje le a hello &lt;LocalStorageName > helyőrzőt hello megfelelő értékre. 

    ```csharp
    // Retrieve an object that points toohello local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("<LocalStorageName>");
    
    //Define hello file name and path
    string[] paths = { localResource.RootPath, "MyLocalStorageTest.txt" };
    String filePath = Path.Combine(paths);
    
    using (FileStream writeStream = File.Create(filePath))
    {
        Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
        writeStream.Write(textToWrite, 0, textToWrite.Length);
    }

    ```

### <a name="find-a-file-written-toolocal-storage"></a>Toolocal tárolási írt fájl keresése

hello kóddal hello előző szakaszban létrehozott tooview hello fájl kövesse az alábbi lépéseket:
    
1.  A Windows tálca értesítési területén hello, kattintson a jobb gombbal a hello Azure ikon és, hello helyi menüből válassza ki a **Show Compute Emulator felhasználói felületén**. 

    ![Az Azure compute emulator megjelenítése](./media/vs-azure-tools-configure-roles-for-cloud-service/show-compute-emulator.png)

1. Válassza ki a hello webes szerepkör.

    ![Az Azure compute emulator](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator.png)

1. A hello **Microsoft Azure Compute Emulator** menü **eszközök** > **nyissa meg a helyi tárolójába**.

    ![Nyissa meg a helyi tárolójába menüpont](./media/vs-azure-tools-configure-roles-for-cloud-service/compute-emulator-open-local-store-menu.png)

1. Amikor megnyílik a hello Windows Explorer ablakot, írja be "MyLocalStorageTest.txt" hello be **keresési** szövegmezőbe, majd válassza ki **adjon meg** toostart hello keresési. 

## <a name="next-steps"></a>Következő lépések
További tudnivalók a Visual Studio Azure projektek olvasásával [konfigurálása az Azure-projekt](vs-azure-tools-configuring-an-azure-project.md). További tudnivalók hello cloud service séma olvasásával [Sémareferenciája](https://msdn.microsoft.com/library/azure/dd179398).

