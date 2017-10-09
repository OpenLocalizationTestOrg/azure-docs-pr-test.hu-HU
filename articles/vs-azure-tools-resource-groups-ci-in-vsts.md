---
title: "a Visual STUDIO Team Services használatával Azure erőforráscsoport-projektek aaaContinuous integrációs |} Microsoft Docs"
description: "Ismerteti, hogyan tooset folyamatos integrációt a Visual Studio Team Services, Azure-erőforráscsoport központi telepítéssel fel a Visual Studio projekt."
services: visual-studio-online
documentationcenter: na
author: mlearned
manager: erickson-doug
editor: 
ms.assetid: b81c172a-be87-4adc-861e-d20b94be9e38
ms.service: azure-resource-manager
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: mlearned
ms.openlocfilehash: 0fe4a4b8989ee323e8ef2206fa4ebed503025670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>A Visual Studio Team Services használatával Azure erőforráscsoport-telepítési projektek folyamatos integrációt
toodeploy Azure-sablon alapján, feladatokat hajthat végre különböző szakaszaiban: Build, tesztelési, másolása tooAzure (más néven "Tesztelés"), és a sablon telepítése. Nincsenek két különböző módon toodeploy sablonok tooVisual Studio Team Services (Visual STUDIO Team Services). Mindkét módszer hello meg ugyanazokat az eredményeket, tehát a hello, amelyik leginkább megfelel a munkafolyamat.

1. Egyetlen lépésben tooyour build definícióját hello Azure erőforráscsoport-telepítési projekt (telepítés-AzureResourceGroup.ps1) szereplő hello PowerShell parancsfájlt futtató hozzáadása. hello parancsfájl összetevők másolja, és ezután telepíti a hello sablon.
2. Vegyen fel több Visual STUDIO Team Services build lépéseket, egyenként a szakasz feladatok.

Ez a cikk mindkét lehetőséget bemutatja. hello első lehetőség van hello ugyanazt a parancsfájlt használja a fejlesztők a Visual Studio használatával és a hello életciklus egységének hello előnyeit. hello második lehetőség egy tetszőleges alternatív toohello beépített parancsfájl kínál. Mindkét eljárás során feltételezzük, hogy már be van jelölve, a Visual STUDIO Team Services Visual Studio telepítési projekt.

## <a name="copy-artifacts-tooazure"></a>Az összetevők tooAzure másolása
Függetlenül hello forgatókönyvben Ha bármely, a sablon-üzembehelyezés szükséges összetevők meg kell adni Azure Resource Manager hozzáférési toothem. Ezen összetevők például a fájlok közé tartoznak:

* Beágyazott sablonok
* Konfigurációs és DSC parancsfájlok
* Bináris alkalmazásfájlokat

### <a name="nested-templates-and-configuration-scripts"></a>Beágyazott sablonok és konfigurációs parancsfájlokat
Visual Studio által biztosított hello sablonok használatakor (vagy a Visual Studio kódtöredékek építését), PowerShell-parancsfájl hello csak nem készít elő az hello összetevők, azt is parameterizes hello URI hello erőforrások a különböző központi telepítésénél. hello parancsfájl majd hello összetevők tooa biztonságos tároló másolja az Azure-ban létrehoz egy SaS-jogkivonatot, hogy a tároló és ezután továbbítja a toohello sablon-üzembehelyezés kapcsolatos információkat. Lásd: [hozzon létre egy sablon-üzembehelyezés](https://msdn.microsoft.com/library/azure/dn790564.aspx) toolearn arról beágyazott sablonok.  Feladatok használata a Visual STUDIO Team Services, hello megfelelő feladatokat a sablon üzembe helyezéshez választott, és ha szükséges, előkészítési lépés toohello sablon-üzembehelyezés hello átadása paraméterértékeket.

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>A Visual STUDIO Team Services folyamatos üzembe helyezés beállítása
tooupdate szüksége toocall hello PowerShell-parancsfájlt a Visual STUDIO Team Services, a build definícióját. Röviden hello lépései a következők: 

1. Hello build definíciójának szerkesztése.
2. A Visual STUDIO Team Services Azure engedélyezési beállítása.
3. Adjon hozzá egy Azure PowerShell build lépést, amely PowerShell-parancsfájl hello hello Azure erőforráscsoport-telepítési projekt hivatkozik.
4. Állítsa be hello hello *- ArtifactsStagingDirectory* paraméter toowork Visual STUDIO Team Services beépített projekthez.

### <a name="detailed-walkthrough-for-option-1"></a>Részletes bemutató az 1. lehetőség
hello alábbi eljárások végigvezetik Önt hello lépéseket szükséges tooconfigure folyamatos üzembe helyezés a Visual STUDIO Team Services, a projekt hello PowerShell parancsfájlt futtató egy feladat használatával. 

1. Szerkesztés a Visual STUDIO Team Services build definícióját, és adjon hozzá egy Azure PowerShell build lépést. Válasszon hello build definíciót a hello **definíciók Build** kategória majd hello **szerkesztése** hivatkozásra.
   
   ![Build definíciójának szerkesztése][0]
2. Adjon hozzá egy új **Azure PowerShell** összeállítása lépés toohello build definícióját, és válassza a hello **összeállítása lépés hozzáadása...** gombra.
   
   ![Build lépés hozzáadása][1]
3. Válassza ki a hello **telepítés a feladat** kategória, jelölje be hello **Azure PowerShell** feladat, és válassza a **hozzáadása** gombra.
   
   ![Feladatok hozzáadása][2]
4. Válassza ki a hello **Azure PowerShell** összeállítása lépés, és töltse ki az értékeket.
   
   1. Ha már rendelkezik egy Azure szolgáltatás végpontjának hozzáadott tooVS Team Services, válasszon hello előfizetés hello **Azure-előfizetés** legördülő listában, majd a skip toohello a következő szakaszban. 
      
      Ha Azure-szolgáltatás a végpont nem rendelkezik a Visual STUDIO Team Services, egy tooadd kell. Ez a szakasz végigvezeti hello folyamaton. Ha az Azure-fiókjával (például Hotmail) Microsoft-fiókot használ, egy egyszerű hitelesítést a következő lépéseket tooget hello kell tennie.
   2. Válassza ki a hello **kezelése** csatolása a következő toohello **Azure-előfizetés** legördülő listában.
      
      ![Az Azure-előfizetések kezelése][3]
   3. Válasszon **Azure** a hello **új szolgáltatási végpont** legördülő listában.
      
      ![Új szolgáltatási végpont][4]
   4. A hello **Azure-előfizetés hozzáadása** párbeszédpanel megnyitásához, jelölje be hello **egyszerű** lehetőséget.
      
      ![Szolgáltatás egyszerű beállítás][5]
   5. Adja hozzá az Azure-előfizetés információk toohello **Azure-előfizetés hozzáadása** párbeszédpanel megnyitásához. A következő elemek tooprovide hello lesz szüksége:
      
      * Előfizetés-azonosító
      * Előfizetés neve
      * Egyszerű szolgáltatás azonosítója
      * Szolgáltatás egyszerű kulcs
      * Bérlő azonosítója
   6. A választott toohello nevet **előfizetés** neve mező. Ez az érték jelenik meg később hello **Azure-előfizetés** legördülő listából válassza ki a Visual STUDIO Team Services. 
   7. Ha nem ismeri az Azure-előfizetése Azonosítóját, a következő parancsok tooretrieve hello egyikét használhatja azt.
      
      PowerShell-parancsfájlok használata:
      
      `Get-AzureRmSubscription`
      
      Azure CLI esetén használja az alábbi parancsot:
      
      `azure account show`
   8. Szolgáltatás egyszerű kulcs és a bérlő azonosítója, a szolgáltatás egyszerű azonosító tooget a hello eljárással [létrehozása az Active Directory-alkalmazás és szolgáltatás egyszerű portál használatával](resource-group-create-service-principal-portal.md) vagy [a szolgáltatás egyszerű hitelesítés Az Azure Resource Manager](resource-group-authenticate-service-principal.md).
   9. Hozzáadás hello szolgáltatás egyszerű azonosító, a szolgáltatás egyszerű kulcs és a bérlő azonosítója értékek toohello **Azure-előfizetés hozzáadása** párbeszédpanel mezőbe, majd válassza a hello **OK** gombra.
      
      Most már rendelkezik egy érvényes egyszerű szolgáltatásnév toouse toorun hello Azure PowerShell-parancsfájlt.
5. Hello build definíciójának szerkesztése, és válassza ki a hello **Azure PowerShell** összeállítása lépés. Válassza ki a hello előfizetést a hello **Azure-előfizetés** legördülő listában. (Ha hello előfizetés nem jelenik meg, válassza ki azt a hello **frissítése** gomb következő hello **kezelése** hivatkozást.) 
   
   ![Azure PowerShell összeállítási feladat konfigurálása][8]
6. Adjon meg egy elérési utat toohello telepítés-AzureResourceGroup.ps1 PowerShell-parancsfájlt. toodo, válassza ki a következő toohello a hello három ponttal (…) gombra **parancsfájl elérési útján** toohello telepítés-AzureResourceGroup.ps1 PowerShell parancsfájl hello válassza **parancsfájlok** mappa a projekt, válassza ki azt, majd hello **OK** gombra.    
   
   ![Válassza ki az elérési út tooscript][9]
7. Hello parancsfájl kiválasztása után frissítse hello elérési toohello parancsfájl úgy, hogy fut, a hello Build.StagingDirectory (hello ugyanabban a könyvtárban, amely *ArtifactsLocation* értékre van állítva). Ehhez adja hozzá a "$(Build.StagingDirectory)/" toohello elejére hello parancsprogram elérési útját.
   
    ![Elérési út tooscript szerkesztése][10]
8. A hello **parancsfájl argumentumai** mezőbe írja be a következő paramétereket (egy sorba) hello. A Visual Studio hello parancsprogram futtatásakor láthatja, hogyan használja a Visual STUDIO hello hello paramétereiben **kimeneti** ablak. Ezzel kiindulási pontként a hello paraméterértékeinek beállítása a build lépésben.
   
   | Paraméter | Leírás |
   | --- | --- |
   | -ResourceGroupLocation |földrajzihely-értéket, ahol hello erőforráscsoportban, többek között hello **eastus** vagy **"USA keleti régiója"**. (Ha adja hozzá szimpla idézőjelben hello névben szóköz található.) Lásd: [Azure-régiókat](https://azure.microsoft.com/en-us/regions/) további információt. |
   | -ResourceGroupName |Ehhez a központi telepítéshez használt hello erőforráscsoport hello nevét. |
   | -UploadArtifacts |Ezt a paramétert, ha létezik, adja meg, hogy az összetevők toobe igénylő feltöltött tooAzure hello helyi rendszerből. Csak akkor kell tooset ezt a kapcsolót, ha a sablon telepítéséhez szükséges további összetevők, amelyet az toostage (például a parancsfájlokat vagy beágyazott sablonok) hello PowerShell-parancsfájl használatával. |
   | -StorageAccountName |Ehhez a központi telepítéshez használt toostage összetevők hello tárfiók hello neve. Ezt a paramétert csak akkor használja, ha meg vannak átmeneti összetevőket a telepítéshez. Ha meg van adva ez a paraméter, egy új tárfiókot jön létre, ha hello parancsfájl nem létrehozva egy korábbi központi telepítés során. Ha hello paraméter van megadva, a hello tárfiók már léteznie kell. |
   | -StorageAccountResourceGroupName |hello tárfiók társított hello erőforráscsoport hello nevét. Ez a paraméter megadása kötelező, csak akkor, ha megad egy értéket hello StorageAccountName paraméter. |
   | -TemplateFile |hello elérési toohello sablonfájl hello Azure erőforráscsoport-telepítési projekt. tooenhance rugalmas, elérési útvonalat használja a paraméter, amely PowerShell-parancsfájl abszolút elérési út helyett hello relatív toohello helyét. |
   | -TemplateParametersFile |hello elérési toohello paraméterfájl hello Azure erőforráscsoport-telepítési projekt. tooenhance rugalmas, elérési útvonalat használja a paraméter, amely PowerShell-parancsfájl abszolút elérési út helyett hello relatív toohello helyét. |
   | -ArtifactStagingDirectory |Ez a paraméter lehetővé teszi, hogy a hello hello mappa tudja, hova lehet másolni hello projekt bináris fájlok a PowerShell-parancsfájlt. Ez az érték hello PowerShell-parancsfájl által használt hello alapértelmezett érték felülbírálja. A Visual STUDIO Team Services használatához hello értékét állítsa: - ArtifactStagingDirectory $(Build.StagingDirectory) |
   
   Íme egy parancsfájl argumentumai példa (az olvashatóság hibás sor):
   
   ```    
   -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
   -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
   –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)'    
   ```
   
   Amikor végzett, hello **parancsfájl argumentumai** mezőben a következő lista hello kell hasonlítania:
   
   ![Parancsfájl argumentumai][11]
9. Miután felvett összes hello Azure PowerShell összeállítása lépés szükséges elemeket toohello, válassza ki azt a hello **várólista** gomb toobuild hello projekt felépítéséhez. Hello **Build** a képernyőn látható hello hello PowerShell-parancsfájl kimenete.

### <a name="detailed-walkthrough-for-option-2"></a>2. lehetőség részletes bemutató
hello alábbi eljárások végigvezetik Önt hello lépéseket szükséges tooconfigure folyamatos üzembe helyezés a Visual STUDIO Team Services hello beépített feladatainak.

1. Szerkesztés a Visual STUDIO Team Services build definition tooadd két új build lépéseket. Válasszon hello build definíciót a hello **definíciók Build** kategória majd hello **szerkesztése** hivatkozásra.
   
   ![Build attribútumdefiníciós szerkesztése][12]
2. Hozzáadás hello új build lépéseket toohello build definition hello segítségével **összeállítása lépés hozzáadása...** gombra.
   
   ![Build lépés hozzáadása][13]
3. Válasszon hello **telepítés** Feladatkategória, jelölje be hello **Azure fájl másolása** feladat, és válassza a **hozzáadása** gomb.
   
   ![Azure-fájl másolása tevékenység hozzáadása][14]
4. Hello válassza **Azure erőforrás-csoport központi telepítésének** feladat, majd kattintson a **hozzáadása** gombra, majd **Bezárás** hello **feladat katalógus**.
   
   ![Adja hozzá Azure-erőforráscsoport telepítési feladat][15]
5. Válassza ki a hello **Azure fájlmásolással** feladatot, és adja meg az értékeket.
   
   Ha már rendelkezik egy Azure szolgáltatás végpontjának hozzáadott tooVS Team Services, válasszon hello előfizetés hello **Azure-előfizetés** legördülő listában. Ha nem rendelkezik előfizetéssel, lásd: [1. lehetőség](#detailed-walkthrough-for-option-1) a Visual STUDIO Team Services egyik beállításával kapcsolatos utasításokat.
   
   * A forrás - írja be **$(Build.StagingDirectory)**
   * Az Azure kapcsolattípus - válassza **Azure Resource Manager**
   * Az Azure erőforrás-kezelő előfizetési - válassza hello előfizetés azt szeretné, hogy a hello toouse hello tárfiók **Azure-előfizetés** legördülő listában. Ha hello előfizetés nem jelenik meg, válassza ki azt a hello **frissítése** gomb következő hello **kezelése** hivatkozásra.
   * Cél típusának - válassza **Azure-Blobba**
   * Erőforrás-kezelő Tárfiók - válassza hello tárolási fiók meg szeretné toouse összetevők átmeneti
   * Tároló neve – hello nevét adja meg az átmeneti; toouse milyen hello tároló az bármilyen érvényes tároló nevet, hanem egy dedikált toothis build definíciót használatára
   
   Hello kimeneti értékeket:
   
   * Adja meg a tároló URI - **artifactsLocation**
   * Tárolási tároló SAS-jogkivonat - meg **artifactsLocationSasToken**
   
   ![Azure-fájl másolása tevékenység konfigurálása][16]
6. Válassza ki a hello **Azure erőforrás-csoport központi telepítésének** összeállítása lépés, és töltse ki az értékeket.
   
   * Az Azure kapcsolattípus - válassza **Azure Resource Manager**
   * Az Azure erőforrás-kezelő előfizetési - válassza hello előfizetés hello központi **Azure-előfizetés** legördülő listában. Ez általában lesz ugyanahhoz az előfizetéshez használt hello hello előző lépésben
   * Művelet – válassza **vagy frissítés erőforráscsoport létrehozása**
   * Erőforráscsoport - válasszon egy erőforráscsoportot, vagy adjon meg egy új erőforráscsoport nevét hello hello központi telepítés
   * Hely - válassza hello hely hello erőforráscsoport
   * Sablon - adja meg a hello elérési útja és neve hello sablon telepített toobe fertőző **$(Build.StagingDirectory)**, például: **$(Build.StagingDirectory/DSC-CI/azuredeploy.json)**
   * Sablonparaméterek - adja meg a hello elérési útját és nevét használva hello paraméterek toobe fertőző **$(Build.StagingDirectory)**, például: **$(Build.StagingDirectory/DSC-CI/azuredeploy.parameters.json)**
   * Sablonparaméterek felülbírálása – adja meg, másolása és beillesztése hello a következő kódot:
     
     ```    
     -_artifactsLocation $(artifactsLocation) -_artifactsLocationSasToken (ConvertTo-SecureString -String "$(artifactsLocationSasToken)" -AsPlainText -Force)
     ```
     ![Azure erőforráscsoport-telepítési feladat konfigurálása][17]
7. Az összes szükséges hello elemek felvételét, mentés hello build definícióját, és válassza **új build várólistára** hello tetején.

## <a name="next-steps"></a>Következő lépések
Olvasási [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md) toolearn további információk az Azure Resource Manager és az Azure erőforráscsoport-sablonok.

[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
[12]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough13.png
[13]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough14.png
[14]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough15.png
[15]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough16.png
[16]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough17.png
[17]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough18.png
