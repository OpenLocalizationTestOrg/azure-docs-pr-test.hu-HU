---
title: "egy Azure-Jenkins kiszolgáló aaaCreate"
description: "Jenkins telepíthető egy Azure Linux virtuális gép hello Jenkins megoldás sablonból, és egy minta Java-alkalmazás létrehozása."
author: mlearned
manager: douge
ms.service: multiple
ms.workload: web
ms.devlang: java
ms.topic: hero-article
ms.date: 08/21/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: 82ab2ac52594acba131414b449b608978591d4b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-jenkins-server-on-an-azure-linux-vm-from-hello-azure-portal"></a>Hozzon létre egy Jenkins kiszolgálót egy Azure Linux virtuális gépen a hello Azure-portálon

A gyors üzembe helyezés bemutatja, hogyan tooinstall [Jenkins](https://jenkins.io) az Ubuntu Linux virtuális gép hello eszközök és beépülő modulok konfigurált toowork az Azure-ral. Ha elkészült, rendelkezni fog egy Azure-ban futó Jenkins-kiszolgálóval, amely egy Java-mintaalkalmazást hoz létre a [GitHubról](https://github.com).

## <a name="prerequisites"></a>Előfeltételek

* Azure-előfizetés
* A számítógép parancssorban hozzáférési tooSSH (például hello Bash rendszerhéjat vagy [PuTTY](http://www.putty.org/))

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-hello-jenkins-vm-from-hello-solution-template"></a>Hello megoldás sablonból hello Jenkins virtuális gép létrehozása

Nyissa meg hello [Jenkins a Piactéri lemezképhez](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview) a webböngészőt, és válasszon **most az beszerzése informatikai** hello hello lap bal oldalán. Felülvizsgálati hello díjszabás részleteit, és válassza **Folytatás**, majd jelölje be **létrehozása** tooconfigure hello Jenkins kiszolgáló hello Azure-portálon. 
   
![Azure Portal párbeszédpanel](./media/install-jenkins-solution-template/ap-create.png)

A hello **konfigurálja az alapbeállításokat** lapra, adja meg a következő mezők hello:

![Az alapvető beállítások konfigurálása](./media/install-jenkins-solution-template/ap-basic.png)

* Írja be a **Jenkins** értéket a **Név** mezőbe.
* Írjon be egy **felhasználónevet**. hello felhasználónevet meg kell felelnie [kapcsolatos követelmények](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
* Válassza ki **jelszó** , hello **hitelesítési típus** és adjon meg egy jelszót. hello ügyeljen arra, hogy egy nagybetűt, számot és egy speciális karaktert.
* Használjon **myJenkinsResourceGroup** a hello **erőforráscsoport**.
* Válassza ki a hello **USA keleti régiója** [az Azure-régió](https://azure.microsoft.com/regions/) a hello **hely** legördülő.

Válassza ki **OK** tooproceed toohello **további beállításokat** fülre. Adjon meg egy egyedi tartomány neve tooidentify hello Jenkins kiszolgálót, és válassza ki **OK**.

![További beállítások megadása](./media/install-jenkins-solution-template/ap-addtional.png)  

 Után az ellenőrzés eredménye akkor megfelelő, válassza a **OK** újra a hello **összegzés** fülre. Végül válassza ki **beszerzési** toocreate hello Jenkins virtuális gép. Ha a kiszolgáló készen áll, értesítést kap a hello Azure-portálon:   

![„A Jenkins készen áll” értesítés](./media/install-jenkins-solution-template/jenkins-deploy-notification-ready.png)

## <a name="connect-toojenkins"></a>Csatlakozás tooJenkins

A böngészőben nyissa meg a tooyour virtuális gép (például http://jenkins2517454.eastus.cloudapp.azure.com/). hello Jenkins konzol nem érhető el, nem biztonságos HTTP Protokollon keresztül, így útmutatást a hello lap tooaccess hello Jenkins konzolon biztonságosan a számítógépről SSH-alagúton keresztül.

![Jenkins zárolásának feloldása](./media/install-jenkins-solution-template/jenkins-ssh-instructions.png)

Hello alagút használatával hello beállítása `ssh` parancs hello lapon hello parancssorból cseréje `username` hello virtuális gép rendszergazdai jogú felhasználó hello megoldás sablonból hello virtuális gép beállítása során a korábban kiválasztott hello nevére.

```bash
ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins2517454.eastus.cloudapp.azure.com
```

Miután elindította hello alagút, keresse meg a toohttp://localhost:8080 / a helyi számítógépen. 

Kezdeti jelszó hello lekéréséhez futtassa a következő parancsot a parancssorban hello SSH toohello Jenkins VM keresztül csatlakozik hello.

```bash
`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
```

Feloldási hello Jenkins irányítópult hello az első alkalommal a kezdeti jelszó használatával.

![Jenkins zárolásának feloldása](./media/install-jenkins-solution-template/jenkins-unlock.png)

Válassza ki **javasolt beépülő modulok telepítése** hello a következő lap, és hozzon létre egy Jenkins admin használt felhasználói tooaccess hello Jenkins irányítópultot.

![Jenkins készen áll!](./media/install-jenkins-solution-template/jenkins-welcome.png)

hello Jenkins kiszolgáló mostantól készen áll a toobuild kódot.

## <a name="create-your-first-job"></a>Az első feladat létrehozása

Válassza ki **hozzon létre új feladatokat** hello Jenkins konzolon, majd nevezze el **mySampleApp** válassza **Freestyle projekt**, majd jelölje be **OK**.

![Új feladat létrehozása](./media/install-jenkins-solution-template/jenkins-new-job.png) 

SELECT hello **forrás kód felügyeleti** lapján engedélyezése **Git**, és írja be a következő URL-címet hello **tárház URL-CÍMÉT** mező:`https://github.com/spring-guides/gs-spring-boot.git`

![Hello Git-tárház meghatározása](./media/install-jenkins-solution-template/jenkins-job-git-configuration.png) 

Jelölje be hello **Build** lapra, majd válassza ki **Hozzáadás összeállítása lépés**, **meghívása Gradle parancsfájl**. Válassza a **Use Gradle Wrapper** (Gradle-burkoló használata) lehetőséget, majd a **Wrapper location** (Burkoló helye) mezőbe írja be a `complete`, a **Tasks** (Feladatok) mezőbe pedig a `build` értéket.

![Hello Gradle burkoló toobuild használata](./media/install-jenkins-solution-template/jenkins-job-gradle-config.png) 

Válassza az **Advanced...** (Speciális) lehetőséget, majd adja meg `complete` a hello **legfelső szintű Build script** mező. Kattintson a **Mentés** gombra.

![Hello Gradle burkoló build lépésben speciális beállításainak megadása](./media/install-jenkins-solution-template/jenkins-job-gradle-advances.png) 

## <a name="build-hello-code"></a>Hello kód létrehozása

Válassza ki **Build most** toocompile hello kód és a csomag hello mintaalkalmazást. A build befejezése után válassza ki a hello **munkaterület** hello projekt hivatkozásra.

![Keresse meg a toohello munkaterület tooget hello JAR-fájlra a hello build](./media/install-jenkins-solution-template/jenkins-access-workspace.png) 

Keresse meg a túl`complete/build/libs` , és ellenőrizze a hello `gs-spring-boot-0.1.0.jar` van-e, hogy sikeres volt-e a build tooverify. A kiszolgáló jelenleg Jenkins kész toobuild saját projektek az Azure-ban.

## <a name="next-steps"></a>Következő lépések

> [!div class="nextstepaction"]
> [Azure-beli virtuális gépek hozzáadása Jenkins-ügynökökként](jenkins-azure-vm-agents.md)
