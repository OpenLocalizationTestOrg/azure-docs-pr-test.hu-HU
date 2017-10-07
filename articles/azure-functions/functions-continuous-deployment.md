---
title: "az Azure Functions aaaContinuous telepítési |} Microsoft Docs"
description: "Az Azure App Service toopublish folyamatos üzembe helyezés létesítményekben az Azure Functions használja."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: 28c44f737dad3feab3cf54f7dd42b6a978d0617e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure Functions – folyamatos üzembe helyezés
Az Azure Functions segítségével könnyen toodeploy az App Service folyamatos integrációt használó függvény alkalmazás. Funkciók integrálható a Bitbucketből, a dropbox-ba, a GitHub és a Visual Studio Team Services (VSTS). Ez lehetővé teszi egy munkafolyamatot, ha frissíti a funkciókódot ezek integrált szolgáltatások eseményindító telepítési tooAzure használatával végrehajtott. Ha új tooAzure funkciók, kezdődnie [Azure Functions áttekintése](functions-overview.md).

A folyamatos üzembe helyezés jó megoldás lehet olyan projektek esetén, amelyeknél többszöri és gyakori közreműködői változtatást kell integrálni. Lehetővé teszi a funkciók kódja verziókezelő karbantartása. a következő központi telepítési források hello jelenleg támogatottak:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox-bA](https://www.dropbox.com/)
* Külső tárház (Git vagy Mercurial)
* [Helyi Git-tárház](../app-service-web/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [Onedrive vállalati verzió](https://onedrive.live.com/)
* [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

Központi telepítések függvény alkalmazás szinten vannak konfigurálva. Folyamatos üzembe helyezés engedélyezése után túl van-e állítva a hozzáférési toofunction kód hello portálon*írásvédett*.

## <a name="continuous-deployment-requirements"></a>Folyamatos üzembe helyezés feltételeit

A konfigurálva telepítési forrás és a funkciók kódot hello központi telepítés forrásának beállítása a folyamatos üzembe helyezés előtt kell rendelkeznie. Egy adott funkció alkalmazások központi telepítésének egyes függvény él, egy elnevezett alkönyvtárra, ahol hello könyvtárnév az hello hello függvény neve.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a>Folyamatos üzembe helyezés beállítása
Ez az eljárás tooconfigure folyamatos üzembe helyezés használjon egy meglévő függvény alkalmazást. Ezeket a lépéseket egy GitHub-tárházban az integráció bemutatásához, de hasonló lépésekkel a Visual Studio Team Services vagy egyéb telepítési érvényesek.

1. Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **központi telepítési beállítások**. 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Ezt a hello **központi telepítések** panelen kattintson **telepítő**.
 
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. A hello **központi telepítés forrásának** panelen kattintson a **forrás választása**, majd adja meg a választott központi telepítési forrás hello adatait, és kattintson a **OK**.
   
    ![Válassza ki a központi telepítés forrása](./media/functions-continuous-deployment/choose-deployment-source.png)

Folyamatos üzembe helyezés beállítása után a központi telepítés forrásának összes fájl módosításának másolt toohello függvény alkalmazást, és akkor váltódik ki, a teljes helyen történő központi telepítés. hello hely van újratelepítése hello fájljai frissítésekor.

## <a name="deployment-options"></a>Üzembe helyezési lehetőségek

hello az alábbiakban néhány tipikus telepítési forgatókönyv:

- [Átmeneti központi telepítés](#staging)
- [Helyezze át a meglévő funkciók toocontinuous telepítés](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Átmeneti központi telepítés

Alkalmazások funkció még nem támogatja a üzembe helyezési pontok. Folyamatos integrációt használatával azonban külön átmeneti és üzemi központi telepítések továbbra is kezelheti.

folyamat tooconfigure hello és az átmeneti központi telepítés használata általában ilyen formátumú:

1. Az előfizetés, hello éles kódot és egy az átmeneti két függvény-alkalmazásai létrehozására. 

2. Hozzon létre olyan telepítési forrás, ha még nem rendelkezik. Ez a példa [GitHub].

3. Az éles függvény alkalmazás előző teljes hello lépései **folyamatos üzembe helyezés beállítása** és set hello telepítési fiókirodai toohello főágába a GitHub-tárházban.
   
    ![Válassza ki a központi telepítés ág](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Átmeneti függvény app hello ismételje meg ezt a lépést, de ehelyett staging ágat a GitHub-tárház hello válassza. Ha a központi telepítési forrás nem támogatja a ugorhat, használjon egy másik mappába.
    
5. Elágazás, illetve mappa átmeneti hello frissítések tooyour kód készítsen, majd győződjön meg arról, hogy ezek a módosítások megjelennek-e telepítési átmeneti hello.

6. Tesztelés után lévő módosítások hello átmeneti fiókirodai hello főágba történő. Az egyesítési eseményindítókat telepítési toohello éles függvény alkalmazás. Ha a központi telepítési forrás nem támogatja a elágazásokat, felülírni a hello fájlokat hello éles mappában átmeneti mappája hello hello fájlokat.

<a name="existing"></a>
### <a name="move-existing-functions-toocontinuous-deployment"></a>Helyezze át a meglévő funkciók toocontinuous telepítés
Ha meglévő függvények, amelyek létrehozott és karbantartott hello portálon toodownload van szüksége van a meglévő működik kódfájlok FTP vagy hello helyi Git-tárház használata előtt állíthat be a folyamatos üzembe helyezés fent leírt módon. Ehhez a hello App Service-beállítások az funkciót alkalmazásához. A fájlok letöltését követően feltöltheti azokat a kiválasztott tooyour folyamatos üzembe helyezés forrás.

> [!NOTE]
> Miután konfigurálta a folyamatos integrációt, már nem lesz képes tooedit a forrásfájlok hello funkciók portálon.

- [Hogyan: üzembe helyezési hitelesítő adatok konfigurálása](#credentials)
- [Útmutató: FTP használata fájlok letöltése](#downftp)
- [Hogyan: Töltse le a fájlokat a hello helyi Git-tárház](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Hogyan: üzembe helyezési hitelesítő adatok konfigurálása
A függvény alkalmazás FTP-vagy helyi Git-tárház letöltheti a fájlokat, konfigurálnia kell a hitelesítő adatok tooaccess hello helyet. Hitelesítő adatok beállítása hello függvény alkalmazási szintű. A következő lépéseket tooset üzembe helyezési hitelesítő adatokat az Azure-portálon hello hello használata:

1. Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **üzembe helyezési hitelesítő adatok**.
   
    ![Helyi telepítési hitelesítő adatok beállítása](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Írja be a felhasználónevet és jelszót, majd kattintson az **mentése**. Most már használhatja a hitelesítő adatok tooaccess az FTP- vagy hello beépített Git-tárház függvény alkalmazását.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Útmutató: FTP használata fájlok letöltése

1. Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **tulajdonságok**, hello értékeit másolja **FTPvagyüzembehelyezőfelhasználó**, **FTP állomásnév**, és **FTPS állomásnév**.  

    **FTP-vagy üzembe helyező felhasználó** hello portálon, beleértve a hello alkalmazás neve, tooprovide megfelelő környezet hello FTP-kiszolgáló jelenik meg kell adni.
   
    ![A központi telepítési információk](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Az FTP-programból tooconnect tooyour app összegyűjtött információkat felhasználva hello kapcsolat, és a függvények hello forrásfájlok letöltéséhez.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Hogyan: Töltse le a fájlokat a helyi Git-tárház

1. Az alkalmazás a hello függvény [Azure-portálon](https://portal.azure.com), kattintson a **Platform funkciói** és **központi telepítési beállítások**. 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Ezt a hello **központi telepítések** panelen kattintson **telepítő**.
 
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. A hello **központi telepítés forrásának** panelen kattintson **helyi Git-tárház** majd **OK**.

3. A **Platform funkciói**, kattintson a **tulajdonságok** és jegyezze fel a Git URL-cím hello értékét. 
   
    ![Folyamatos üzembe helyezés beállítása](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. A Git-kompatibilis parancssor vagy a kedvenc Git használatával, a helyi gépen hello tárház klónozása. hello Git-klón parancs így néz ki:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. A függvény app toohello Klónozás fájlok beolvasni a helyi számítógépen, mint például a következő hello:
   
        git pull origin master
   
    Ha szükséges, adja meg a [üzembe helyezési hitelesítő adatok konfigurált](#credentials).  

[GitHub]: https://github.com/
