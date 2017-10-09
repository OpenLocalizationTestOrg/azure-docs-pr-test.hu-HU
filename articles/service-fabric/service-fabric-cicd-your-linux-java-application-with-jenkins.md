---
title: "aaaContinuous build és az integráció az Azure Service Fabric Linux Java-alkalmazás Jenkins használatával |} Microsoft Docs"
description: "A linuxos Java-alkalmazás folyamatos felépítése és integrálása a Jenkins használatával"
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: 15da2cb8c759233219369ea889550da93748129f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-jenkins-toobuild-and-deploy-your-linux-java-application"></a>Jenkins toobuild használja, és a Linux-Java-alkalmazás központi telepítése
A Jenkins egy népszerű eszköz az alkalmazások folyamatos integrációjához és üzembe helyezéséhez. Itt hogyan toobuild és az Azure Service Fabric-alkalmazás telepítését Jenkins.

## <a name="general-prerequisites"></a>Általános előfeltételek
- A Git legyen telepítve a helyi számítógépen. Telepíthet hello megfelelő Git verziót a [hello Git letölti lap](https://git-scm.com/downloads)az operációs rendszer alapján. Ha új tooGit, további információ a hello [Git dokumentáció](https://git-scm.com/docs).
- Service Fabric Jenkins beépülő modul hello rendelkezik lesz szüksége. Ezt a [Service Fabric letöltési](https://servicefabricdownloads.blob.core.windows.net/jenkins/serviceFabric.hpi) oldaláról töltheti le.

## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>A Jenkins beállítása egy Service Fabric-fürtben

A Jenkinst egy Service Fabric-fürtben vagy azon kívül is beállíthatja. hello alábbi szakaszokban megjelenítése hogyan tooset az be Azure-tárolóra használatakor a fürtben található derül toosave hello hello tároló példány állapotát.

### <a name="prerequisites"></a>Előfeltételek
1. Szükséges egy kész linuxos Service Fabric-fürt. A Service Fabric-fürt már hello Azure-portálon létrehozott Docker telepítve van. Ha helyileg futtat hello fürt, Docker meglétének ellenőrzése hello parancs használatával ``docker info``. Ha nincs telepítve, telepítse ennek megfelelően az alábbi parancsok hello:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```
2. Hello Service Fabric-tároló alkalmazás hello lépések használatával hello fürtön telepítve rendelkezik:

  ```sh
git clone https://github.com/Azure-Samples/service-fabric-java-getting-started.git
cd service-fabric-java-getting-started/Services/JenkinsDocker/
```

3. Hello kell toopersist hello állapot hello Jenkins tároló példány, ahová az Azure storage-fájlmegosztás, hello beállítás részleteit csatlakozzon. Ha a hello hello Microsoft Azure-portált használja azonos, adjon hello lépésekkel,-Azure-tárfiók létrehozása, mondja ki ``sfjenkinsstorage1``. Hozzon létre egy **fájlmegosztás** mondja ki az adott tárolási fiók ``sfjenkins``. Kattintson a **Connect** hello fájlmegosztási és megjegyzés hello értékek azt alatt megjelenik a **Linux csatlakozó**, mondja ki a következőhöz hasonló módon -
```sh
sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
```

4. Frissítse a hello hello helyőrző értékeket ```setupentrypoint.sh``` parancsfájl megfelelő azure-storage-adatokkal.
```sh
vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
```
Cserélje le ``[REMOTE_FILE_SHARE_LOCATION]`` hello értékű ``//sfjenkinsstorage1.file.core.windows.net/sfjenkins`` csatlakozás hello hello kimenetét a fenti 3.
Cserélje le ``[FILE_SHARE_CONNECT_OPTIONS_STRING]`` hello értékű ``vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777`` a fenti 3.

5. Csatlakoztassa toohello fürtöt, és hello tároló alkalmazás telepítéséhez.
```azurecli
sfctl cluster select --endpoint http://PublicIPorFQDN:19080   # cluster connect command
bash Scripts/install.sh
```
Ez telepíti egy Jenkins tároló hello fürtön, és hello Service Fabric Explorer használatával figyelhetők.

### <a name="steps"></a>Lépések
1. A böngészőben nyissa meg túl``http://PublicIPorFQDN:8081``. Hello elérési útja hello kezdeti rendszergazdai jelszó szükséges toosign a biztosít. Rendszergazda felhasználóként toouse Jenkins tovább. Vagy hozhat létre és hello felhasználó módosítása után hello kezdeti rendszergazdai fiókkal jelentkezik.

   > [!NOTE]
   > Győződjön meg arról, hogy hello fürt létrehozása közben hello 8081 port van megadva hello végpont portja.
   >

2. Hello tároló Példányazonosító segítségével könnyebben nyerhet ``docker ps -a``.
3. Secure Shell (SSH) bejelentkezési toohello tárolóban, és illessze be a hello elérési út volt látható hello Jenkins portálon. Például, ha a hello portál hello elérési útját jeleníti meg `PATH_TO_INITIAL_ADMIN_PASSWORD`, futtassa a következő hello:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
  cat PATH_TO_INITIAL_ADMIN_PASSWORD
  ```

4. Az említett hello módon Jenkins, a GitHub toowork beállítása [új SSH-kulcs létrehozása és hozzáadná toohello SSH-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
    * Használja a Githubon toogenerate hello SSH-kulccsal és tooadd hello SSH kulcs toohello GitHub-fiók, amely üzemelteti a tárház hello utasításait.
    * Futtassa a hello parancsokat hello megelőző hivatkozás hello Jenkins Docker rendszerhéj (és nem a gazdagépen) szerepel.
    * a toohello Jenkins rendszerhéj a gazdagépről, a következő parancs használata hello toosign:

  ```sh
  docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
  ```

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>A Jenkins beállítása Service Fabric-fürtön kívül

A Jenkinst egy Service Fabric-fürtben vagy azon kívül is beállíthatja. a következő szakaszok megjelenítése hogyan hello tooset a fürtön kívüli azt be.

### <a name="prerequisites"></a>Előfeltételek
Toohave Docker telepítve van szüksége. a következő parancsok hello lehet használt tooinstall Docker a Terminálszolgáltatások hello:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

Most futtatásakor ``docker info`` a Terminálszolgáltatások hello kell megjelennie hello kimenet adott hello szolgáltatás fut. Docker.

### <a name="steps"></a>Lépések
  1. Lekéréses hello Service Fabric Jenkins tároló lemezképet:``docker pull raunakpandya/jenkins:v1``
  2. Futtassa a hello tároló lemezképet:``docker run -itd -p 8080:8080 raunakpandya/jenkins:v1``
  3. Hello tároló kép példány hello azonosító beszerzése. Az összes hello Docker tároló hello paranccsal listázhatja``docker ps –a``
  4. Jelentkezzen be toohello Jenkins portal használatával hello a következő lépéseket:

    * ```sh
    docker exec [first-four-digits-of-container-ID] cat /var/jenkins_home/secrets/initialAdminPassword
    ```
    Ha a tároló azonosítója 2d24a73b5964, használja a 2d24 értéket.
    * Meg kell adni az aláíráshoz toohello Jenkins irányítópulton portálról, amely a jelszót``http://<HOST-IP>:8080``
    * A bejelentkezést a hello először, a saját felhasználói fiók létrehozása és a jövőbeli célokra használni, amely vagy toouse hello rendszergazdai fiók továbbra is. A felhasználó létrehozása után kell toocontinue vele.
  5. Az említett hello módon Jenkins, a GitHub toowork beállítása [új SSH-kulcs létrehozása és hozzáadná toohello SSH-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
        * Használja a Githubon toogenerate hello SSH-kulccsal és tooadd hello SSH kulcs toohello GitHub-fiók, amelyen az hello tárház hello utasításait.
        * Futtassa a hello parancsokat hello megelőző hivatkozás hello Jenkins Docker rendszerhéj (és nem a gazdagépen) szerepel.
      * a toohello Jenkins rendszerhéj a gazdagépről, a következő parancsok használata hello toosign:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Győződjön meg arról, hogy hello fürt vagy a gép, ahol Jenkins tároló lemezkép hello rendelkezik egy nyilvánosan elérhető IP-cím. Ez lehetővé teszi a hello Jenkins példány tooreceive értesítések a Githubról.

## <a name="install-hello-service-fabric-jenkins-plug-in-from-hello-portal"></a>Hello portálról hello Service Fabric Jenkins beépülő modul telepítése

1. Nyissa meg túl``http://PublicIPorFQDN:8081``
2. Hello Jenkins irányítópultot, válassza ki **kezelése Jenkins** > **kezelése beépülő modulok** > **speciális**.
Itt feltöltheti a beépülő modult. Válassza ki **fájl kiválasztása**, majd válassza ki a hello **serviceFabric.hpi** Előfeltételek a letöltött fájlt. Ha bejelöli **feltöltése**, Jenkins automatikusan telepíti a beépülő modul hello. Ha a rendszer kéri, engedélyezze az újraindítást.

## <a name="create-and-configure-a-jenkins-job"></a>Jenkins-feladatok létrehozása és konfigurálása

1. Hozzon létre **új elemet** az irányítópultról.
2. Adjon nevet az elemnek (pl. **MyJob**). Válassza a **free-style project** (szabad projekt) lehetőséget, majd kattintson az **OK** gombra.
3. Nyissa meg hello feladat lapot, és kattintson a **konfigurálása**.

   a. Hello általános területen alatt **GitHub-projekt**, adja meg a GitHub-projekt URL-címe. Az URL-cím állomások hello szolgáltatás háló Java-alkalmazás, amelyet a hello Jenkins folyamatos integrációt toointegrate, a folyamatos üzembe helyezés (CI/CD) flow (például ``https://github.com/sayantancs/SFJenkins``).

   b. A hello **forrás kód felügyeleti** szakaszban jelölje be **Git**. Adja meg a hello tárház URL-CÍMÉT, amelyen hello Service Fabric Java-alkalmazás, amelyet a hello Jenkins CI/CD folyamat toointegrate (például ``https://github.com/sayantancs/SFJenkins.git``). Emellett itt megadhatja mely fiókirodai toobuild (például **/fő**).
4. Konfigurálja a *GitHub* (amely üzemeltető hello tárház), hogy az képes tootalk tooJenkins. A lépéseket követve hello használata:

   a. Nyissa meg a GitHub-tárház tooyour oldalon. Nyissa meg túl**beállítások** > **integrációja és a szolgáltatások**.

   b. Válassza ki **hozzáadása szolgáltatás**, típus **Jenkins**, és jelölje be hello **Jenkins-GitHub beépülő modul**.

   c. Adja meg a Jenkins-webhook URL-címét (alapértelmezés szerint ez a következő: ``http://<PublicIPorFQDN>:8081/github-webhook/``). Kattintson az **add/update service** (Szolgáltatás hozzáadása/frissítése) elemre.

   d. A vizsgált esemény érkezik tooyour Jenkins példány. Megjelenik egy zöld pipa által hello webhook a Githubon, és a projekt fog létrehozni.

   e. A hello **Build eseményindítók** területen válassza ki, amely összeállítása a kívánt beállítást. Ebben a példában a kívánt tootrigger build minden egyes leküldéses toohello tárház történik. Így a kiválasztandó lehetőség a következő: **GitHub hook trigger for GITScm polling** (GitHub beavatkozási pont eseményindító GITScm lekérdezés esetén). (Korábban, ez a beállítás lett meghívva **létrehozása, amikor változás fejlesztőre tooGitHub**.)

   f. A hello **szakasz Build**, a hello legördülő **Hozzáadás összeállítása lépés**, hello beállítást **meghívása Gradle parancsfájl**. Hello widget előre, adja meg hello elérési út túl**legfelső szintű build script** az alkalmazáshoz. Felveszi a megadott hello elérési útról build.gradle, és ennek megfelelően működik. Ha nevű projekt létrehozása ``MyActor`` hello legfelső szintű build script kell tartalmaznia (hello Eclipse beépülő modul vagy Yeoman generátor használatával), ``${WORKSPACE}/MyActor``. Tekintse meg a következő képernyőkép néz Ez például egy hello:

    ![Service Fabric, Jenkins felépítési művelet][build-step]

   g. A hello **utáni műveletek** legördülő listából válassza **Fabric-projekt telepítése**. Itt tooprovide fürt, ahol hello Jenkins összeállított Service Fabric-alkalmazás részletei kellene telepíteni kell. Az alkalmazás további részletei használt toodeploy hello alkalmazást is megadhatja. Tekintse meg a következő képernyőkép néz Ez például egy hello:

    ![Service Fabric, Jenkins felépítési művelet][post-build-step]

   > [!NOTE]
   > Itt hello fürt lehet ugyanaz, mint a hello egy üzemeltetési hello Jenkins tároló alkalmazás, abban az esetben, ha a Service Fabric toodeploy hello Jenkins tároló lemezképet használ.
   >

## <a name="next-steps"></a>Következő lépések
A GitHub és a Jenkins beállítása kész. Néhány változás a minta tervezhet a ``MyActor`` hello tárház példában projekt https://github.com/sayantancs/SFJenkins. A módosítások tooa távoli leküldéses ``master`` ág (vagy bármely toowork a konfigurált fiókja). Ez elindítja a hello Jenkins feladat, ``MyJob``, konfigurált. Hello módosítások beolvassa a Githubból, épít fel őket, és telepíti a hello alkalmazás toohello a fürt végpontja utáni műveletek a megadott.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/build-step.png
  [post-build-step]: ./media/service-fabric-cicd-your-linux-java-application-with-jenkins/post-build-step.png
