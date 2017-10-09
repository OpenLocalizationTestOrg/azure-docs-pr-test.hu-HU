---
title: "aaaHow toouse hello Azure alárendelt Hudson folyamatos integrációt beépülő modul |} Microsoft Docs"
description: "Ismerteti, hogyan toouse hello Azure alárendelt Hudson folyamatos integrációt beépülő modul."
services: virtual-machines-linux
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: b2083d1c-4de8-4a19-a615-ccc9d9b6e1d9
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: cd6e67ad71c208aa56746aa8b70ba507da20bee9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-slave-plug-in-with-hudson-continuous-integration"></a>Hogyan toouse hello Azure alárendelt Hudson folyamatos integrációt beépülő modul
hello Azure alárendelt Hudson beépülő modul lehetővé teszi tooprovision alárendelt csomópontok Azure amikor fut elosztott alkot.

## <a name="install-hello-azure-slave-plug-in"></a>Hello Azure alárendelt beépülő modul telepítése
1. Hello Hudson irányítópultot, kattintson **kezelése Hudson**.
2. A hello **kezelése Hudson** lapján kattintson a **kezelése beépülő modulok**.
3. Kattintson a hello **elérhető** fülre.
4. Kattintson a **keresési** és típus **Azure** toolimit hello lista toorelevant beépülő modulok.
   
    Ha úgy dönt, hogy tooscroll keresztül elérhető beépülő modulok hello listája, megtalálja hello Azure alárendelt hello a beépülő modul **kiszolgálófürt-felügyelet és az elosztott Build** hello című **mások** lapon.
5. Válassza ki a hello jelölőnégyzetét **Azure alárendelt beépülő modul**.
6. Kattintson az **Install** (Telepítés) gombra.
7. Indítsa újra a Hudson.

Most, hogy hello beépülő modul telepítve van, akkor hello lépések tooconfigure hello beépülő modul, az Azure-előfizetés profil és a toocreate hello VM hello az alárendelt csomópont létrehozásakor használt sablon lenne.

## <a name="configure-hello-azure-slave-plug-in-with-your-subscription-profile"></a>Az előfizetés profillal hello Azure alárendelt beépülő modul konfigurálása
Egy előfizetés profilt is hivatkozott tooas közzétételi beállítások, egy XML-fájl, amely tartalmazza a biztonságos hitelesítő adatok, és néhány további információ a fejlesztési környezetet az Azure-ral toowork lesz szüksége. tooconfigure hello Azure alárendelt beépülő modult, lesz szüksége:

* Az előfizetés-azonosítóval
* Az előfizetéshez tartozó felügyeleti tanúsítvány

Ezek itt található: a [előfizetés profil]. Az alábbiakban az előfizetés profil egy példája.

    <?xml version="1.0" encoding="utf-8"?>

        <PublishData>

          <PublishProfile SchemaVersion="2.0" PublishMethod="AzureServiceManagementAPI">

        <Subscription

              ServiceManagementUrl="https://management.core.windows.net"

              Id="<Subscription ID>"

              Name="Pay-As-You-Go"
            ManagementCertificate="<Management certificate value>" />

          </PublishProfile>

    </PublishData>

Miután az előfizetés profil, hajtsa végre az ezen lépések tooconfigure hello Azure alárendelt beépülő modul.

1. Hello Hudson irányítópultot, kattintson **kezelése Hudson**.
2. Kattintson a **rendszer konfigurálása**.
3. Görgessen lefelé hello lap toofind hello **felhő** szakasz.
4. Kattintson a **adja hozzá az új felhő > Microsoft Azure**.
   
    ![új felhőalapú hozzáadása][add new cloud]
   
    Ez azt mutatja majd hello mezők tooenter kell az előfizetés részletei.
   
    ![profil konfigurálása][configure profile]
5. Hello előfizetési azonosító és felügyeleti tanúsítvány a előfizetés profilból másolással illessze be őket a hello megfelelő mezőket.
   
    Hello előfizetési azonosító és felügyeleti tanúsítvány, másolásakor **nem** hello idézőjelek között, tegye a hello értékeket tartalmazza.
6. Kattintson a **ellenőrizze konfigurációs**.
7. Hello konfiguráció sikeres ellenőrzését követően kattintson **mentése**.

## <a name="set-up-a-virtual-machine-template-for-hello-azure-slave-plug-in"></a>Beépülő modul hello Azure alárendelt virtuálisgép-sablon beállítása
A virtuális gép sablon meghatározza hello hello beépülő modul egy alárendelt csomópont toocreate fogja használni az Azure-on. Az alábbi lépésekkel hello azt fogja kell sablon létrehozása egy Ubuntu virtuális gép számára.

1. Hello Hudson irányítópultot, kattintson **kezelése Hudson**.
2. Kattintson a **rendszer konfigurálása**.
3. Görgessen lefelé hello lap toofind hello **felhő** szakasz.
4. Hello belül **felhő** területen található **Azure virtuálisgép-sablon hozzáadása** hello kattintson **Hozzáadás** gombra.
   
    ![Virtuálisgép-sablon hozzáadása][add vm template]
5. Adja meg a felhőszolgáltatás neve hello **neve** mező. Ha hello nevét adja meg, hogy a meglévő felhőalapú szolgáltatás tooan utal, hello VM lesznek üzembe helyezve, hogy a szolgáltatásban. Ellenkező esetben az Azure létrehoz egy új.
6. A hello **leírás** mezőbe írja be a létrehozandó hello sablon leírását. Ezek az információk csak dokumentációs célokat szolgál, és nem szerepel a virtuális gép kiépítése.
7. A hello **címkék** mezőbe írja be **linux**. Ez a címke használt tooidentify hello sablon létrehozásakor és ezt követően használt tooreference hello sablon Hudson feladat létrehozásakor.
8. Válasszon ki egy régiót, ahol a virtuális gép hello létrejön.
9. Válassza ki a megfelelő hello Virtuálisgép-méretet.
10. Adjon meg egy tárfiókot, ahol a virtuális gép hello létrejön. Győződjön meg arról, hogy hello ugyanabban a régióban hello felhőalapú szolgáltatást fogja használni. Ha azt szeretné, hogy a létrehozott új tárolási toobe, akkor ezt a mezőt üresen hagyhatja.
11. Megőrzési idő megadja perc elteltével Hudson törli az inaktív alárendelt hello számát. Adja meg ezt az alapértelmezett értéket hello 60.
12. A **használati**, válassza ki hello megfelelő feltétel, ha ez az alárendelt csomópont fogja használni. Most, válassza ki a **használata a lehető legnagyobb mértékben csomópont**.
    
     Ezen a ponton az űrlap némileg hasonló toothis jelenne meg:
    
     ![sablon config][template config]
13. A **kép termékcsalád vagy azonosító** rendelkezik toospecify milyen rendszerkép lesz telepítve a virtuális Gépet. Válassza ki a lemezkép termékcsaládok listáját, vagy adjon meg egyéni lemezképet.
    
     Ha azt szeretné, hogy a lemezkép-családok listája tooselect, adja meg a hello első karakterének (kis-és nagybetűket) hello kép csomagcsalád nevét. Például írja be **U** Ubuntu Server családok listája megjelenik. Miután hello listából válassza ki a Jenkins fogja használni hello legújabb verzióját, hogy az operációs rendszer lemezkép, amikor a virtuális gép kiépítésétől.
    
     ![Az operációs rendszer termékcsalád listája][OS family list]
    
     Ha rendelkezik, amelyet toouse helyette egyéni kép, adja meg az egyéni lemezkép hello nevét. Egyéni rendszerkép neve nem láthatók listáját, hogy rendelkezik, amelyek neve hello tooensure megfelelően van-e megadva.    
    
     Ebben az oktatóanyagban írja be a következőt **U** Ubuntu lemezképeihez, és válassza ki listája toobring **Ubuntu Server 14.04 LTS**.
14. A **indítsa el a metódus**, jelölje be **SSH**.
15. Az alábbi parancsfájl hello másolja és illessze be a hello **Init parancsfájl** mező.
    
         # Install Java
    
         sudo apt-get -y update
    
         sudo apt-get install -y openjdk-7-jdk
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y openjdk-7-jdk
    
         # Install git
    
         sudo apt-get install -y git
    
         #Install ant
    
         sudo apt-get install -y ant
    
         sudo apt-get -y update --fix-missing
    
         sudo apt-get install -y ant
    
     Hello **Init parancsfájl** hello virtuális gép létrehozása után a rendszer hajtja majd végre. Ebben a példában a hello parancsfájl telepíti a Java, a git és az telepítsenek.
16. A hello **felhasználónév** és **jelszó** mezőkben adja meg a kívánt értékeket hello rendszergazdai fiók, amely a virtuális gép létrejön.
17. Kattintson a **sablon ellenőrzése** toocheck, ha a megadott hello paraméterek érvényesek.
18. Kattintson a **Mentés** gombra.

## <a name="create-a-hudson-job-that-runs-on-a-slave-node-on-azure"></a>Egy alárendelt csomópont az Azure-on futó Hudson feladat létrehozása
Ebben a szakaszban egy alárendelt csomópont az Azure-on futó Hudson feladat csak létrehozunk.

1. Hello Hudson irányítópultot, kattintson **új feladat**.
2. Adja meg a létrehozandó hello feladat nevét.
3. Hello feladattípus, válassza a **egy ingyenes stílusú szoftver feladat létrehozása**.
4. Kattintson az **OK** gombra.
5. Hello feladat konfigurálása lapon válassza ki a **korlátozása, amelyben ez a projekt futtathatók**.
6. Válassza ki **csomópont és a felirat menü** válassza **linux** (azt meg ezt a címkét az előző szakaszban hello hello virtuálisgép-sablon létrehozásakor).
7. A hello **Build** területen kattintson **Hozzáadás összeállítása lépés** válassza **hajtható végre a rendszerhéj**.
8. A következő parancsfájlt, hogy hello szerkesztése **{a githubon fióknevet}**, **{a projekt neve}**, és **{projektkönyvtárban}** a megfelelő értékeket, és illessze be a hello parancsfájl hello szöveg területen megjelenő szerkeszteni.
   
        # Clone from git repo
   
        currentDir="$PWD"
   
        if [ -e {your project directory} ]; then
   
              cd {your project directory}
   
              git pull origin master
   
        else
   
              git clone https://github.com/{your github account name}/{your project name}.git
   
        fi
   
        # change directory tooproject
   
        cd $currentDir/{your project directory}
   
        #Execute build task
   
        ant
9. Kattintson a **Mentés** gombra.
10. A hello Hudson irányítópult, újonnan létrehozott hello feladat keresése, majd kattintson a hello **build ütemezése** ikonra.

Hudson majd egy alárendelt csomópont hello előző szakaszban létrehozott hello sablonnal létrehozása, és ehhez a feladathoz hello build lépésben megadott hello parancsfájlt.

## <a name="next-steps"></a>Következő lépések
Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].

<!-- URL List -->

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[előfizetés profil]: http://go.microsoft.com/fwlink/?LinkID=396395

<!-- IMG List -->

[add new cloud]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addcloud.png
[configure profile]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-configureprofile.png
[add vm template]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-addnewvmtemplate.png
[template config]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-setup-templateconfig1-withdata.png
[OS family list]: ./media/virtual-machines-azure-slave-plugin-for-hudson/hudson-oslist.png

