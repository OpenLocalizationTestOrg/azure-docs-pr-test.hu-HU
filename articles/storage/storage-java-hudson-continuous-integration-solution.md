---
title: "aaaHow toouse Hudson az Blob storage szolgáltatással |} Microsoft Docs"
description: "Ismerteti, hogyan toouse Hudson az Azure Blob Storage tárolót a build összetevők tára."
services: storage
documentationcenter: java
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 119becdd-72c4-4ade-a439-070233c1e1ac
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 02/28/2017
ms.author: seguler
ms.openlocfilehash: a23581e4b58526d2e9c936d4f75f6abdc407e27f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-storage-with-a-hudson-continuous-integration-solution"></a>Az Azure Storage szolgáltatás használata Hudson folyamatos integrációs megoldással
## <a name="overview"></a>Áttekintés
hello alábbi információkat jeleníti meg a build összetevőihez Hudson folyamatos integráció (CI) megoldás által létrehozott tára, vagy letölthető fájlok toobe forrásaként toouse Blob-tároló használatáról a felépítési folyamat. Egy hello forgatókönyv, ahol Ön is hasznos ez akkor, ha Ön most kódolási egy gyors fejlesztési környezetben (Java vagy más nyelvek) buildek futtat alapú folyamatos integrációt és tára van szüksége a build összetevők, hogy Ön volt , például más szervezet tagjaival, az ügyfelek megosztására vagy archiválhatja karbantartása.  Egy másik helyzet akkor, ha az összeállítási feladat maga van szükség egyéb fájlok, például a függőségek toodownload hello részét build bemenet.

Ebben az oktatóanyagban fog használni hello Azure Storage beépülő modul a Microsoft által elérhetővé tett Hudson konfigurációelemhez.

## <a name="introduction-toohudson"></a>Bevezetés tooHudson
Hudson lehetővé teszi, hogy folyamatos integrációt azáltal, hogy a fejlesztők tooeasily integrálja a kód módosítására, és rendelkezik a szoftver projekt buildek előállított automatikusan és gyakran, növelve a hello fejlesztők hello hatékonyságára. Buildek verzióval ellátva, és build összetevők lehetnek feltöltött toovarious tárházak találhatók. A cikkből megtudhatja hogyan toouse hello tárháza hello Azure Blob Storage tárolót build összetevők. Ebben is bemutatjuk, hogyan toodownload függőségek az Azure Blob storage.

További információ a Hudson található [megfelelnek Hudson](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson).

## <a name="benefits-of-using-hello-blob-service"></a>Hello Blob szolgáltatás használatának előnyei
Hello Blob szolgáltatás toohost használatának előnyei közé tartozik a gyors fejlesztés build összetevők:

* Magas rendelkezésre állás a build összetevők és/vagy letölthető függőségek.
* Ha a Hudson CI megoldás feltölti a build összetevők teljesítményét.
* Ha az ügyfelek és partnerek töltse le a build összetevők teljesítményét.
* Felhasználói hozzáférési házirendeket, a névtelen hozzáférés, lejárati-alapú megosztott hozzáférési aláírást hozzáférés, saját közötti választás felett ellenőrzést hozzáférés, stb.

## <a name="prerequisites"></a>Előfeltételek
Akkor lesz kell hello következő toouse hello Blob szolgáltatás a Hudson CI-megoldás:

* A folyamatos integrációs Hudson megoldás.
  
    Jelenleg nem rendelkezik olyan Hudson CI megoldást, ha egy Hudson CI megoldás hello a következő eljárás használatával futtathatók:
  
  1. A Java-kompatibilis gépen letöltési hello Hudson WAR a <http://hudson-ci.org/>.
  2. Parancsot a parancssorba, amely a megnyitott toohello mappa hello Hudson WAR, futtassa a hello Hudson WAR tartalmaz. Ha például verzió 3.1.2 letöltött:
     
      `java -jar hudson-3.1.2.war`

  3. A böngészőben nyissa meg a `http://localhost:8080/`. Ekkor megnyílik a hello Hudson irányítópult.
  4. Első használata Hudson, végezze el a kezdeti telepítőt hello `http://localhost:8080/`.
  5. Hello kezdeti beállítás elvégzése után hello Hudson WAR-példányt futtató hello megszakításához Hudson WAR hello újra, és nyissa meg újra hello Hudson irányítópult elindításához `http://localhost:8080/`, mely akkor tooinstall használnak, illetve hello Azure Storage beépülő modul konfigurálása.
     
      Egy tipikus Hudson CI megoldás szolgáltatásként toorun volna kell beállítani, amíg hello Hudson war hello parancssorból lesz elegendő ehhez az oktatóanyaghoz.
* Egy Azure-fiók. Azure-fiókot, regisztrálhat <http://www.azure.com>.
* Egy Azure-tárfiók. Ha még nem rendelkezik a storage-fiókra, létrehozhat egy hello készítésével használatával [hozzon létre egy Tárfiókot](storage-create-storage-account.md#create-a-storage-account).
* Hello Hudson CI megoldás ismeretét ajánlott, de nem szükséges, hello következő tartalom fogja használni, egy egyszerű példa tooshow meg hello lépések szükségesek, ha a hello Blob szolgáltatás-t egy tárház Hudson CI build összetevők.

## <a name="how-toouse-hello-blob-service-with-hudson-ci"></a>Hogyan toouse hello Hudson CI Blob-bA
toouse hello Blob szolgáltatást Hudson, szüksége lesz tooinstall hello Azure Storage beépülő modul, a tárfiók hello beépülő modul toouse konfigurálása és hozzon létre egy utáni műveletet, amely feltölti a build összetevők tooyour tárfiók. A következő szakaszok hello ezeket a lépéseket ismerteti.

## <a name="how-tooinstall-hello-azure-storage-plugin"></a>Hogyan tooinstall hello Azure Storage beépülő modul
1. Belül hello Hudson irányítópultot, kattintson a **kezelése Hudson**.
2. A hello **kezelése Hudson** kattintson **kezelése beépülő modulok**.
3. Kattintson a hello **elérhető** fülre.
4. Kattintson a **mások**.
5. A hello **összetevő Uploaders** szakaszban jelölje be **Microsoft Azure Storage beépülő modul**.
6. Kattintson az **Install** (Telepítés) gombra.
7. Hello telepítés befejezése után indítsa újra a Hudson.

## <a name="how-tooconfigure-hello-azure-storage-plugin-toouse-your-storage-account"></a>Hogyan tooconfigure hello Azure Storage beépülő modul toouse a tárfiók
1. Belül hello Hudson irányítópultot, kattintson a **kezelése Hudson**.
2. A hello **kezelése Hudson** kattintson **rendszer konfigurálása**.
3. A hello **Microsoft Azure Storage-fiók konfigurációja** szakasz:
   
    a. Adja meg a tárfiók neve, amely szerezhet be a hello [Azure Portal](https://portal.azure.com).
   
    b. Adja meg a tárfiók kulcsára, történő hello is szerezhető [Azure Portal](https://portal.azure.com).
   
    c. Hello alapértelmezett érték a **Blob-Szolgáltatásvégpont URL-** hello nyilvános Azure felhőalapú rendszer használata esetén. Ha egy másik Azure felhőbe használ, hello végpont használják a hello [Azure Portal](https://portal.azure.com) a tárfiók.
   
    d. Kattintson a **tárolási fiók hitelesítő adatainak érvényesítéséhez** toovalidate a tárfiók.
   
    e. [Választható] Ha a megjeleníteni kívánt készült elérhető tooyour Hudson CI további tárfiókokat, kattintson **adja hozzá a további tárfiókok**.
   
    f. Kattintson a **mentése** toosave a beállításokat.

## <a name="how-toocreate-a-post-build-action-that-uploads-your-build-artifacts-tooyour-storage-account"></a>Hogyan toocreate egy utáni műveletet, amely feltölti a build összetevők tooyour tárfiók
Utasítás okokból először kell toocreate egy feladatot, amely több fájlok létrehozása, és vegye fel hello létrehozás után végrehajtandó művelet tooupload hello fájlok tooyour tárfiókban.

1. Belül hello Hudson irányítópultot, kattintson a **új feladat**.
2. Név hello feladat **MyJob**, kattintson a **egy ingyenes stílusú szoftver feladat létrehozása**, és kattintson a **OK**.
3. A hello **Build** szakasz hello feladat konfiguráció, kattintson **Hozzáadás összeállítása lépés** válassza **hajtható végre Windows kötegelt parancs**.
4. A **parancs**, a következő parancsok hello használata:

    ```   
        md text
        cd text
        echo Hello Azure Storage from Hudson > hello.txt
        date /t > date.txt
        time /t >> date.txt
    ```

5. A hello **utáni műveletek** szakasz hello feladat konfiguráció, kattintson a **töltse fel az összetevők tooMicrosoft Azure Blob Storage tárolóban**.
6. A **Tárfióknév**, válassza ki a tárolási fiók toouse hello.
7. A **Tárolónév**, adja meg a hello tároló neve. (hello tároló jön létre, ha még nem létezik hello build összetevők feltöltése során.) Környezeti változók használata, így ehhez a példához írja be **${JOB_NAME}** hello tároló neveként.
   
    **Tipp**
   
    Alább hello **parancs** egy parancsfájlt megadásánál szakasz **hajtható végre Windows kötegelt parancs** toohello környezeti változók ismeri fel Hudson kapcsolat van. Kattintson a hivatkozásra toolearn hello környezeti változó neve és leírása. Vegye figyelembe, hogy a környezeti változókat, amelyek különleges karaktereket, például hello **BUILD_URL** környezeti változó, a tároló neve vagy a közös virtuális elérési út nem tartalmazhat.
8. Kattintson a **új tároló alapértelmezés szerint nyilvánosságra** ehhez a példához. (Ha azt szeretné, hogy toouse egy személyes tárolót, szüksége lesz egy megosztott hozzáférési aláírást tooallow hozzáférés toocreate. Amely nem, ez a cikk hello terjed. A megosztott hozzáférési aláírásokkal kapcsolatos részletesebb [használatával megosztott hozzáférési aláírásokkal (SAS)](storage-dotnet-shared-access-signature-part-1.md).)
9. [Választható] Kattintson a **feltöltés előtt tiszta tároló** Ha azt szeretné, hogy a hello tároló toobe előtt build összetevők frissíti a rendszer törli az tartalmának (hagyja bejelölve Ha nem szeretné, hogy hello tároló tartalmának tooclean hello).
10. A **listán az összetevők tooupload**, adja meg  **szöveg /*.txt**.
11. A **közös virtuális elérési utat feltöltött összetevők**, adja meg **${BUILD\_azonosító} / ${BUILD\_szám}**.
12. Kattintson a **mentése** toosave a beállításokat.
13. Hello Hudson irányítópultot, kattintson **Build most** toorun **MyJob**. Vizsgálja meg a konzol kimeneti hello állapot. Állapotüzenetek az Azure Storage szerepelni fog a konzol kimeneti hello tooupload build összetevők hello létrehozás után végrehajtandó művelet indításakor.
14. Hello feladat sikeres befejezését követően hello build összetevők hello nyilvános blob megnyitásával ellenőrizheti.
    
    a. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).
    
    b. Kattintson a **tárolási**.
    
    c. Kattintson a hello tárfióknév Hudson hitelesítésszolgáltatónak.
    
    d. Kattintson a **tárolók**.
    
    e. Kattintson nevű hello tárolót **myjob**, vagyis hello hello Hudson feladat létrehozásakor rendelt hello feladatnév kisbetűs verzióját. A tároló nevének és a blob nevének, kisbetű (és kis-és nagybetűket) az Azure Storage. Blobok nevű hello tároló hello listája belül **myjob** kell megjelennie **hello.txt** és **date.txt**. Hello URL-Címének másolása ezek az elemek egyikét, majd nyissa meg a böngészőben. Hello szövegfájl feltöltött, a build összetevő jelenik meg.

Csak egy utáni művelet, amely feltölti az összetevők tooAzure Blob-tároló Feladatonkénti hozhatók létre. Vegye figyelembe, hogy a hello műveletével utáni tooupload összetevők tooAzure a Blob storage (beleértve a helyettesítő karaktereket) különböző fájlok és elérési utak toofiles belül adhat meg **listán az összetevők tooupload** pontosvesszővel kell elválasztani őket egy használatával. Például ha a Hudson előállító JAR és TXT-fájlok a munkaterületen **build** mappa, és szeretné tooupload mindkét tooAzure Blob-tároló, a hello használja hello következőket **listán az összetevők tooupload** érték: **build /\*.jar; build /\*.txt**. Dupla kettőspont szintaxis toospecify egy elérési utat toouse hello blob neve belül is használhatja. Például, ha azt szeretné, hogy hello JAR-fájlok kivételével tooget feltöltött használatával **bináris** hello blob elérési út és a TXT-fájlok tooget hello feltöltött használatával **értesítéseket** hello blob elérési út hello a használja következőket hello  **Az összetevők tooupload listája** érték: **build /\*. jar::binaries; build /\*. txt::notices**.

## <a name="how-toocreate-a-build-step-that-downloads-from-azure-blob-storage"></a>Hogyan toocreate egy összeállítása lépés, amely letölti az Azure Blob storage
hello lépések bemutatják, hogyan tooconfigure build lépés az Azure Blob storage toodownload elemek. Ez akkor hasznos, ha a build, például JAR-fájlok kivételével, amely azt az Azure Blob Storage tárolóban tárolni kívánt tooinclude elemeket.

1. A hello **Build** szakasz hello feladat konfiguráció, kattintson a **Hozzáadás összeállítása lépés** válassza **töltse le az Azure Blob storage**.
2. A **tárfióknév**, válassza ki a tárolási fiók toouse hello.
3. A **Tárolónév**, adja meg a kívánt toodownload hello blobok rendelkező hello tároló hello neve. Környezeti változókat is használhat.
4. A **Blob neve**, adja meg a hello blob neve. Környezeti változókat is használhat. Is használhatja, csillag helyettesítő karakter hello kezdeti betűből hello blob nevének megadása után. Például **projekt\***  kellene megadnia kezdődő összes BLOB **projekt**.
5. [Választható] A **letöltési mappa elérési útját**, hello utat adjon meg hello Hudson gépen, ahol azt szeretné, hogy az Azure Blob storage toodownload fájlok. Környezeti változók is használható. (Ha nem ad meg értéket **letöltési mappa elérési útját**, az Azure Blob storage hello fájlok lesznek a letöltött toohello feladat munkaterület.)

Ha azt szeretné, hogy az Azure Blob storage toodownload további elemek, build további lépéseket is létrehozhat.

Miután lefuttatta a build, ellenőrizheti a hello build előzmények a konzol kimeneti, vagy nézze meg a letöltési hely toosee e hello blobok várt letöltése sikeresen megtörtént.

## <a name="components-used-by-hello-blob-service"></a>Hello Blob szolgáltatás által használt összetevők
hello következő hello Blob szolgáltatás-összetevők áttekintést nyújt.

* **A tárfiók**: összes keresztül férnek hozzá tooAzure tárolási történik egy tárfiókot. Ez a legmagasabb szintű hello hello névtér blobok eléréséhez. Egy fiók tartalmazhat egy korlátlan számú tárolót, mindaddig, amíg a teljes mérete 100 TB alatt.
* **Tároló**: A tárolók blobok blobkészletek csoportosítását biztosítja. Az összes blobnak tárolóban kell lennie. Egy fiók korlátlan számú tárolót tartalmazhat. Egy tároló korlátlan számú blob tárolására használható.
* **A BLOB**: bármilyen típusú és bármekkora méretű fájl. Az Azure Storage tárolható blobok két típusa van: blokkblobok és lapblobok. A legtöbb fájlok blokk blobokat. Egyetlen blokkblob mentése too200 GB méretű lehet. Ez az oktatóanyag a blokkblobokhoz használja. Lapblobokat, egy másik blob típushoz, mentése too1 TB méretét, és a hatékonyabb lehet, ha egy fájlban bájt tartományok gyakran módosítják. Blobok kapcsolatos további információkért lásd: [ismertetése Blokkblobokat, hozzáfűző blobokat és Lapblobokat](http://msdn.microsoft.com/library/azure/ee691964.aspx).
* **URL-formátum**: Blobok olyan megcímezhető hello URL-cím formátuma a következő használatával:
  
    `http://storageaccount.blob.core.windows.net/container_name/blob_name`
  
    (a fenti hello formátum toohello nyilvános Azure-felhőbe vonatkozik. Ha egy másik Azure felhőbe használ, hello végpont belül hello használata [Azure Portal](https://portal.azure.com) toodetermine az URL-végpontjának.)
  
    A fenti hello formátumban `storageaccount` jelöli hello a tárfiók nevére `container_name` jelöli hello a tároló neve és `blob_name` pedig rendre hello a blob neve. Hello Tárolónév esetén belül rendelkezhet több elérési út, szóközzel elválasztva perjellel,  **/** . hello példa tároló neve ebben az oktatóanyagban **MyJob**, és **${BUILD\_azonosító} / ${BUILD\_szám}** lett hello közös virtuális elérési útja, hello blob rendelkező eredményezi egy A következő képernyő hello URL-címe:
  
    `http://example.blob.core.windows.net/myjob/2014-05-01_11-56-22/1/hello.txt`

## <a name="next-steps"></a>Következő lépések
* [Hudson felel meg](http://wiki.eclipse.org/Hudson-ci/Meet_Hudson)
* [Az Azure Storage Java SDK](https://github.com/azure/azure-storage-java)
* [Az Azure Storage ügyfél SDK-dokumentáció](http://dl.windowsazure.com/storage/javadoc/)
* [Az Azure Storage-szolgáltatások REST API-ja](https://msdn.microsoft.com/library/azure/dd179355.aspx)
* [Az Azure Storage csapat blogja](http://blogs.msdn.com/b/windowsazurestorage/)

További információk is: hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).
