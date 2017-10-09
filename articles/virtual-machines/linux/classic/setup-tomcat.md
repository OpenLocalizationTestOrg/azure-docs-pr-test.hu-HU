---
title: "a Linux virtuális gép mentése Apache Tomcat aaaSet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooset be Apache Tomcat7 Linux operációs rendszert futtató Azure virtuális gépek használatával."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b837a73e91fcb25d5459d993a0e93ceef1a1fc8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>A Linux virtuális gépek Azure-ral Tomcat7 beállítása
Apache Tomcat (vagy egyszerűen Tomcat is korábban Dzsakarta Tomcat) egy nyílt forráskódú webkiszolgáló és a servlet tároló hello Apache szoftver Foundation (ASP) által kidolgozott. Tomcat hello Java Servlet és hello JavaServer lapok (JSP) specifikációk a Sun Microsystems valósítja meg. Tomcat mely toorun Java-kódot a tiszta Java HTTP-web server környezetet biztosít. Hello legegyszerűbb konfiguráció, a Tomcat egyetlen operációs rendszer folyamatban fut. Ez a folyamat fut, a Java virtuális gép (JVM). Minden HTTP-kérelem a egy böngésző tooTomcat hello Tomcat folyamat külön szálban végzi a rendszer.  

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk ismerteti, hogyan toouse hello klasszikus üzembe helyezési modellben. Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja. a Resource Manager sablon toodeploy nyitott JDK és Tomcat Ubuntu virtuális gép toouse lásd [Ez a cikk](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).

Ez a cikk Tomcat7 telepítése egy Linux-lemezképet, és telepítse az Azure-ban.  

Az oktatóanyagban érintett témák köre:  

* Hogyan toocreate egy virtuális gép az Azure-ban.
* Hogyan tooprepare Tomcat7 hello virtuális gépet.
* Hogyan tooinstall Tomcat7.

Feltételezzük, hogy már rendelkezik Azure-előfizetéssel.  Ha nem, regisztrálhat egy ingyenes próbaverziót: [hello Azure-webhelyen](https://azure.microsoft.com/). Ha MSDN-előfizetéssel rendelkezik, tekintse meg [Microsoft Azure különleges árképzési: MSDN, az MPN és a BizSpark előnyök](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). További információ az Azure-ban toolearn lásd [Mi az Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Ez a cikk feltételezi, hogy rendelkezik-e Tomcat- és Linux alapszintű ismeretét.  

## <a name="phase-1-create-an-image"></a>1. fázis: Lemezkép létrehozása
Ebben a fázisban egy virtuális gépet hoz létre egy Linux-lemezképet használja az Azure-ban.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>1. lépés: Az SSH hitelesítési kulcs létrehozása
Az SSH egy rendszergazdák számára fontos eszköze. HR-részleg által meghatározott jelszót alapuló hozzáférés biztonsági beállításainak megadása azonban nem ajánlott. Rosszindulatú felhasználók is felosztása a rendszer a felhasználónév és a gyenge jelszót alapján.

hello jó hírünk, hogy nincs-e olyan módon tooleave távelérési megnyitásához, és nem kell foglalkoznia jelszavak. Ez a módszer a hitelesítés az aszimmetrikus titkosítási áll. hello felhasználó a titkos kulcs egy, amely engedélyezi a hello hitelesítési hello. Hello felhasználói fiók még akkor is zárolhatja toonot jelszó-hitelesítés engedélyezése.

Egy másik ezt a módszert előnye, hogy nem kell különböző jelszóból toosign toodifferent-kiszolgálókon. Hello személyes titkos kulcs használatával az összes kiszolgálón megakadályozza, hogy jelszavainak tooremember több hitelesítheti.



Hajtsa végre az alábbi lépéseket toogenerate hello SSH hitelesítési kulcs.

1. Töltse le és telepítse a következő helyen hello PuTTYgen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Futtassa a Puttygen.exe.
3. Kattintson a **Generate** toogenerate hello kulcsok. Hello folyamat során is fokozható annak véletlenszerű áthelyezése hello egér által hello üres területet hello ablakban.  
   ![PuTTY kulcs generátor képernyőkép hello létrehozás új kulcs gomb][1]
4. Miután hello folyamat létrehozása, Puttygen.exe jeleníti meg az új nyilvános kulcshoz.  
   ![PuTTY kulcs generátor képernyőkép hello új nyilvános és titkos kulcs gomb mentés hello][2]
5. Válassza ki és hello nyilvános kulcsának az átmásolása, és mentse a munkafüzetet egy publicKey.pem nevű fájlt. Ne kattintson **mentés nyilvános kulcs**, mert hello mentett nyilvános kulcs fájl formátuma nem azonos a hello azt szeretnénk, ha nyilvános kulcs.
6. Kattintson a **mentés titkos kulcs**, és mentse a munkafüzetet egy privateKey.ppk nevű fájlt.

### <a name="step-2-create-hello-image-in-hello-azure-portal"></a>2. lépés: Hello lemezképének létrehozásához a hello Azure-portálon
1. A hello [portal](https://portal.azure.com/), kattintson a **új** hello a feladat toocreate látható kép. Válassza ki a hello Linux képet, amely alapul, az igényeknek megfelelően. hello alábbi példa hello Ubuntu 14.04 lemezképet használja.
![Képernyőfelvétel a hello portál, amely hello új gomb][3]

2. A **állomásnév**, adja meg, hogy Ön és az internetes ügyfeleket használja tooaccess a virtuális gép hello URL hello nevét. Adja meg a hello hello DNS-nevét, például tomcatdemo utolsó része. Azure ekkor létrehozza, tomcatdemo.cloudapp.net hello URL-CÍMÉT.  

3. A **SSH hitelesítési kulcs**, hello kulcsérték másolása hello publicKey.pem fájl, amely tartalmazza a PuTTYgen által létrehozott hello nyilvános kulcs.  
![SSH hitelesítési kulcs hello portal párbeszédpanel][4]

4. Igény szerint konfigurálhatja az egyéb beállításokat, és kattintson a **létrehozása**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>2. fázis: A Tomcat7 a virtuális gép előkészítése
Ebben a fázisban a Tomcat-forgalmat a végpont konfigurálása, és csatlakoztassa az új virtuális gép tooyour.

### <a name="step-1-open-hello-http-port-tooallow-web-access"></a>1. lépés: Nyissa meg a hello HTTP port tooallow webes elérés
Az Azure-végpontok közé tartozik a TCP vagy UDP protokoll, valamint nyilvános és magánhálózati portot. hello a magánhálózati port megadása, hogy hello szolgáltatás figyeli-e tooon hello virtuális gép hello port. hello nyilvános port hello Azure-felhőszolgáltatásban hello portot figyeli a bejövő, az internetes forgalmat tooexternally.  

8080-as TCP-port hello alapértelmezett portszámot, hogy a Tomcat toolisten használja. Ha ezt a portot, ahol az Azure-végpont, és a más internetes ügyfelek hozzáférhet Tomcat lapok.  

1. Hello portálon kattintson **Tallózás** > **virtuális gépek**, majd kattintson a létrehozott hello virtuális gépre.  
   ![Képernyőfelvétel a hello Virtual machines könyvtár][5]
2. tooadd egy végpont tooyour virtuális gépet, kattintson a hello **végpontok** mezőbe.
   ![Képernyőkép a hello végpontok mezőbe][6]
3. Kattintson az **Add** (Hozzáadás) parancsra.  

   1. Hello végpont, adjon meg egy nevet a hello végpont **végpont**, és írja be a 80-as **nyilvános Port**.  

      Ha a megadott érték too80, nem kell tooinclude hello portszámot, amely használt tooaccess Tomcat hello URL-címben. Például http://tomcatdemo.cloudapp.net.    

      Ha azt tooanother érték, például a 81-es, tooadd hello port száma toohello URL-cím tooaccess Tomcat kell. Például http://tomcatdemo.cloudapp.net:81 /.
   2. Adja meg a 8080-as **magánhálózati Port**. Alapértelmezés szerint a Tomcat a 8080-as TCP-portot figyeli. Ha módosította hello alapértelmezett figyelési Tomcat port, frissítenie kell **magánhálózati Port** toobe hello megegyeznek a Tomcat figyelési portja hello.  
      ![Képernyőfelvétel a felhasználói felület Hozzáadás parancs, a nyilvános Port és a magánhálózati Port jeleníti meg][7]
4. Kattintson a **OK** tooadd hello végpont tooyour virtuális gépet.

### <a name="step-2-connect-toohello-image-you-created"></a>2. lépés: Csatlakozás létrehozott toohello kép
Kiválaszthatja a SSH eszköz tooconnect tooyour virtuális gépek. A jelen példában használjuk a PuTTY.  

1. A virtuális gép hello DNS-neve beszerzése hello portálról.
    1. Kattintson a **Tallózás** > **virtuális gépek**.
    2. Válassza ki a virtuális gép hello nevét, és kattintson **tulajdonságok**.
    3. A hello **tulajdonságok** csempéjén kattintson a jobb hello hely **tartománynév** mezőben tooget hello DNS-nevét.  

2. Az SSH-kapcsolatokhoz a hello hello portszám beolvasása **SSH** mezőbe.  
![Képernyőkép a hello SSH-kapcsolat portszám][8]

3. Töltse le [PuTTY](http://www.putty.org/).  

4. A letöltés után kattintson a hello végrehajtható fájl Putty.exe. A PuTTY konfigurációs beállításokat adhat meg hello alapszintű hello állomásnévvel és származik a virtuális gép tulajdonságainak hello portszámát.   
![Képernyőkép a hello PuTTY konfigurációs állomás neve és portszáma beállítások][9]

5. Hello bal oldali ablaktáblában kattintson **kapcsolat** > **SSH** > **Auth**, és kattintson a **Tallózás** toospecify hello hello privateKey.ppk fájl helyét. hello privateKey.ppk fájl tartalmazza a titkos kulcs hello hello a korábbi PuTTYgen által létrehozott "1. fázis: lemezkép létrehozása" című szakaszát.  
![Képernyőkép a hello kapcsolat könyvtár-hierarchia és a böngészés gombra.][10]

6. Kattintson a **nyitott**. Így előfordulhat, hogy riasztást, által egy üzenet mezőbe. Ha a konfigurált hello DNS-nevét és portszámát megfelelően, kattintson a **Igen**.
![Képernyőkép a hello értesítés][11]

7. Meg vannak felszólító tooenter a felhasználónév.  
![Képernyőkép a where tooenter felhasználónév][12]

8. Adja meg a toocreate hello virtuális gép szerepel hello hello felhasználónév "1. fázis: lemezkép létrehozása" című rész ebben a cikkben. Hello következő hasonlót fog látni:  
![Képernyőkép a hello hitelesítési megerősítése][13]

## <a name="phase-3-install-software"></a>3. fázis: A szoftver telepítése
Ebben a fázisban telepíti a hello Java-futtatókörnyezet, Tomcat7 és egyéb Tomcat7 összetevőket.  

### <a name="java-runtime-environment"></a>Java-futtatókörnyezet
Tomcat Java nyelven van megírva. Java fejlesztői csomagok (JDKs), OpenJDK és Oracle JDK két típusú léteznek. Kiválaszthatja a kívánt hello.  

> [!NOTE]
> Mind JDKs majdnem megegyezik a hello osztályokhoz hello Java API-kód hello rendelkezik, de hello virtuális gép hello kód nem egyezik. OpenJDK általában toouse nyitott szalagtárak, amíg Oracle JDK általában toouse lezárt azokat. Oracle JDK van több osztályok és néhány rögzített hibák, és Oracle JDK stabilabb, mint OpenJDK.

#### <a name="install-openjdk"></a>OpenJDK telepítése  

A következő parancs toodownload OpenJDK hello használata.   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* a könyvtár toocontain toocreate hello JDK-fájlok:  

        sudo mkdir /usr/lib/jvm  
* tooextract hello JDK fájlok hello/usr/lib/jvm/directory:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a>Oracle JDK telepítése


Parancs toodownload Oracle JDK következő hello Oracle webhelyről hello használata.  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* a könyvtár toocontain toocreate hello JDK-fájlok:  

        sudo mkdir /usr/lib/jvm  
* tooextract hello JDK fájlok hello/usr/lib/jvm/directory:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* Oracle JDK, alapértelmezett Java virtuális gép hello tooset:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a>Győződjön meg arról, hogy Java telepítése sikeres
Ha hello Java-futtatókörnyezet helyesen van-e telepítve a következő tootest hello hasonló parancsot is használhatja:  

    java -version  

Ha OpenJDK telepítette, megjelenik egy üzenet hasonló hello: ![sikeres OpenJDK telepítési üzenet][14]

Ha Oracle JDK telepítette, megjelenik egy üzenet hasonló hello: ![sikeres Oracle JDK telepítés üzenet][15]

### <a name="install-tomcat7"></a>Tomcat7 telepítése
A következő parancs tooinstall Tomcat7 hello használata.  

    sudo apt-get install tomcat7  

Ha nem használ Tomcat7, használja a hello megfelelő változatát, a parancs.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Győződjön meg arról, hogy Tomcat7 telepítése sikeres
toocheck Ha Tomcat7 telepítése sikerült, keresse meg a tooyour Tomcat kiszolgáló DNS-neve. Ebben a cikkben a hello példa URL-címe: http://tomcatexample.cloudapp.net/. Ha megjelenik egy üzenet hello hasonló, Tomcat7 megfelelően van telepítve.
![Sikeres Tomcat7 telepítése üzenet][16]

### <a name="install-other-tomcat7-components"></a>Más Tomcat7 összetevőinek telepítése
Nincsenek más Tomcat összetevőket is telepítheti.  

Használjon hello **sudo apt-gyorsítótár keresési tomcat7** parancs toosee hello elérhető összetevőket. A következő parancsok tooinstall hello néhány hasznos összetevőket használnak.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools toocreate user instances  

## <a name="phase-4-configure-tomcat7"></a>4. fázis: A Tomcat7 konfigurálása
Ebben a fázisban a Tomcat felügyeletéhez.

### <a name="start-and-stop-tomcat7"></a>Elindítása és leállítása Tomcat7
hello Tomcat7 kiszolgáló automatikusan elindul, amikor a telepítés. Is elindíthatja a hello a következő parancsot:   

    sudo /etc/init.d/tomcat7 start

toostop Tomcat7:

    sudo /etc/init.d/tomcat7 stop

Tomcat7 tooview hello állapota:

    sudo /etc/init.d/tomcat7 status

toorestart Tomcat szolgáltatások: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7 felügyeleti
Szerkesztheti a hello Tomcat felhasználói konfigurációs fájl tooset be a rendszergazdai hitelesítő adataival. A következő parancs hello használata:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Például:  
![Képernyőkép a hello sudo vi parancs kimenete][17]  

> [!NOTE]
> Hozzon létre egy erős jelszót hello rendszergazda felhasználóneve.  

Ez a fájl szerkesztése után újra kell indítania Tomcat7 szolgáltatások rendelkező hello a következő parancs tooensure hello módosítások érvénybe:  

    sudo /etc/init.d/tomcat7 restart  

Nyissa meg a böngészőt, és írja be **http://<your tomcat server DNS name>/kezelő/html** , hello URL-CÍMÉT. Ebben a cikkben hello például hello URL-cím http://tomcatexample.cloudapp.net/manager/html.  

A csatlakozás után valami hasonló toohello következő kell megjelennie:  
![Képernyőfelvétel a hello Tomcat webalkalmazás-kezelő][18]

## <a name="common-issues"></a>Gyakori problémák
### <a name="cant-access-hello-virtual-machine-with-tomcat-and-moodle-from-hello-internet"></a>Nem érhető el virtuális gép hello a Moodle és Tomcat hello Internet
#### <a name="symptom"></a>Jelenség  
  Tomcat fut, de a böngésző hello Tomcat alapértelmezett lap nem látható.
#### <a name="possible-root-cause"></a>Probléma lehetséges kiváltó okai   

  * hello Tomcat figyelési portja nem hello ugyanaz, mint a Tomcat-forgalmat a virtuális gép végpontjának hello a magánhálózati port.  

     Ellenőrizze a nyilvános port és a titkos végpont portbeállításokat, és győződjön meg arról, hogy hello magánhálózati port van hello megegyeznek a hello Tomcat port figyelésére. Lásd: "1. fázis: lemezkép létrehozása" című szakaszát, a virtuális gép végpontjai konfigurálásával kapcsolatban.  

     toodetermine hello Tomcat port figyelésére, nyissa meg a /etc/httpd/conf/httpd.conf (Red Hat kiadás), vagy /etc/tomcat7/server.xml (Debian kiadás). Alapértelmezés szerint a Tomcat figyelési portja hello a 8080-as. Például:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Ha például a Debian és Ubuntu és azt szeretné, toochange hello alapértelmezett port a Tomcat figyelésére (például 8081) használ a virtuális gép, is meg kell nyitnia hello portot hello operációs rendszerhez. Első lépésként nyissa meg a hello-profil:  

        sudo vi /etc/default/tomcat7  

     Ezután állítsa vissza a hello utolsó sort, és módosítsa "nem" túl "yes".  

        AUTHBIND=yes
  2. hello tűzfal hello figyelési port a Tomcat letiltotta.

     Hogy hello Tomcat alapértelmezett oldal hello helyi állomásról. hello probléma a legvalószínűbb, hogy hello portot, amely figyelt tooby Tomcat, hello tűzfal által blokkolva van. Hello w3m eszköz toobrowse hello weblap használható. hello következő parancsok w3m telepítse, és keresse meg toohello Tomcat alapértelmezett oldal:  


        sudo yum telepítés w3m w3m-img


        w3m 8080  
#### <a name="solution"></a>Megoldás

  * Hello Tomcat figyelési portja van nem hello ugyanaz, mint hello magánhálózati port hello végpont forgalom toohello virtuális gép, ha módosítania kell a magánhálózati port hello toobe hello megegyeznek a Tomcat figyelési portja hello.   
  2. Hello tűzfal/iptables okozza, ha adja hozzá a következő sorokat túl/etc/sysconfig/iptables hello. hello második sor csak akkor szükséges, a https-forgalmat:  

      -A -p tcp -m tcp--dport 80 – j ELFOGADÁS bemeneti

      -A -p tcp -m tcp--dport 443 -j ELFOGADÁS bemeneti  

     > [!IMPORTANT]
     > Ellenőrizze, hogy hello előző sorok legyenek elhelyezve fent, amelyek globálisan szeretné korlátozni a hozzáférést, például hello következő sorokat: - A bemeneti -j ELUTASÍTÁS – elutasítás-az icmp-állomás által tiltott



tooreload hello iptables, futtassa a következő parancs hello:

    service iptables restart

Ez a CentOS 6.3 tesztelték.

### <a name="permission-denied-when-you-upload-project-files-toovarlibtomcat7webapps"></a>Engedély megtagadva a projekt feltöltésekor fájlok túl/var/lib/tomcat7/webapps /
#### <a name="symptom"></a>Jelenség
  Ha egy SFTP ügyfélről (például FileZilla) tooconnect tooyour virtuális gép, és keresse meg a túl/var/lib/tomcat7/webapps/toopublish webhelyét, kap egy hiba üzenet hasonló toohello következő:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Probléma lehetséges kiváltó okai
  Nincs engedélyek tooaccess hello /var/lib/tomcat7/webapps mappa rendelkezik.  
#### <a name="solution"></a>Megoldás  
  A rendszergazdafiók hello tooget engedéllyel kell rendelkezni. Legfelső szintű toohello felhasználónév hello gép létesített használt hello tulajdonjogát, hogy a mappa módosítható. Íme egy példa a hello azureuser fióknévvel:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Hello -R beállítás tooapply hello engedélyeket használ az összes fájl egy könyvtár belül túl.  

  Ez a parancs könyvtárak is működik. hello -R beállítás módosítások hello engedélyeit a fájlok és könyvtárak hello könyvtárán belül. Például:  

     sudo chown -R username:group directory  

  Ez a parancs (felhasználó és csoport) változik a fájlok és könyvtárak, amelyek hello könyvtárán belül.  

  hello következő parancs csak akkor változik meg hello mappa directory hello engedéllyel. hello fájlok és mappák hello könyvtárán belül nem változnak.  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
