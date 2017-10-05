---
title: "Az Azure Role szerepkör tulajdonságai"
description: "Megtudhatja, hogyan használja az Eclipse Azure eszköztára Azure szerepkör beállításainak konfigurálásához."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 5c0ec412-5702-465a-8f47-87a8ce99a267
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: cd734c64ba6d1394cb261bace92dee9dd579dd08
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-role-properties"></a>Az Azure Role szerepkör tulajdonságai
Az Azure szerepkör különböző konfigurációs beállításainak Eclipse Azure eszköztára belül állítható be.

## <a name="configuring-azure-role-properties"></a>Az Azure szerepkör tulajdonságainak konfigurálása
A feldolgozói szerepkör esetében a tulajdonság párbeszédpanelek segítségével történik, az Azure szerepkör tulajdonságainak konfigurálása. Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, és válassza ki a **Azure** almenü. (Ha nem látja az Eclipse Project Explorer szerepét, bontsa ki az Azure projektre a Project Explorer.)

![][ic789599]

A különböző tulajdonságokról érték között állítható be a **tulajdonságok** Ez a témakör ismerteti a párbeszédpanelek. Vegye figyelembe, hogy számos tulajdonság automatikusan kitölti az Azure-telepítés új projekt létrehozásakor.

A következő tulajdonságlapjain Azure szerepkörök érhetők el.

* [Virtuális gép tulajdonságai](#virtual_machine_properties)
* [Gyorsítótár tulajdonságai](#caching_properties)
* [Tanúsítványok tulajdonságai](#certificates_properties)
* [Összetevő tulajdonságai](#components_properties)
<!-- * [Debugging properties](#debugging_properties) -->
* [Végpontok tulajdonságai](#endpoints_properties)
* [Környezeti változók tulajdonságai](#environment_variables_properties)
* [Terheléselosztás / munkamenet affinitás (más néven "állandóságát munkamenetek") tulajdonságai](#session_affinity_properties)
* [Helyi tárolási tulajdonságai](#local_storage_properties)
* [Kiszolgáló konfigurációs tulajdonságai](#server_configuration_properties)
* [SSL-feladatkiszervezést tulajdonságai](#ssl_offloading_properties)

<a name="virtual_machine_properties"></a>

### <a name="virtual-machine-properties"></a>Virtuális gép tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **tulajdonságok**, és lesznek képesek a virtuális gép méretének módosítása, és is módosíthatja a példányok száma, a következő ábrán látható módon.

![][ic719499]

> [!NOTE]
> Csak Windows: a példányok száma értékre 1-nél nagyobb és az alkalmazáskiszolgáló is konfigurál, az eszközkészlet lehetővé teszi az emulátorban, ettől a beállítástól függetlenül futtatásához csak 1 szerepkörpéldányt. Ez a port kötés közötti ütközések elkerülésével a különböző kiszolgálópéldányok (például minden próbált kötni a 8080-as porton) futtatásakor. az ugyanazon a számítógépen. A beállítás kívánt példányszám megmarad, de kerül érvénybe csak akkor, ha telepíti a felhőbe.
> 
> 

<a name="caching_properties"></a> 

### <a name="caching-properties"></a>Gyorsítótár tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **gyorsítótárazását**. Ezen a párbeszédpanelen belül engedélyezheti elnevezett közös elhelyezésű memcache-kompatibilis gyorsítótárak, lehetővé teszi a webalkalmazások felgyorsítása érdekében.

![][ic719483]

Belül a **gyorsítótárazását** tulajdonságlapját, megadhatja az alábbi globális beállításokat:

* közös elhelyezésű gyorsítótárazás engedélyezve van-e.
* a gyorsítótár méretét, a memória százalékában.
* a tárfiók nevét a gyorsítótár állapotának mentése, amikor az alkalmazás egy felhőalapú szolgáltatás, vagy egyik sem fut, ha nem szeretné menteni a gyorsítótár állapotot. (A tárfiók neve nem használatos a compute emulator az alkalmazás futtatásakor.) Megadásával a tárfiók neve **(automatikus)** (ez utóbbi érték az alapértelmezett), a gyorsítótár konfigurációs automatikusan használja ugyanazt a tárfiókot megegyezik a lehetőséget választja a **Azure közzététel** párbeszédpanel.

> [!NOTE]
> A **(automatikus)** beállítás lesz a kívánt hatása, csak akkor, ha a központi telepítés az Eclipse eszközkészlet segítségével közzéteszi közzététele varázsló. Ha ehelyett közzéteszi az .cspkg fájlt manuálisan külső mechanizmust, mint például a [Azure felügyeleti portálon][Azure Management Portal], a telepítés nem fog megfelelően működni.
> 
> 

A következő párbeszédpanel a gyorsítótár tulajdonságai jeleníti meg.

![][ic719501]

* **Name:** a közös elhelyezésű gyorsítótár nevét.
* **Port száma:** a gyorsítótárhoz használni a port számát.
* **Elévülési szabályzatának:** , amely meghatározza, hogy amikor egy kulcsot a gyorsítótárban érvényessége lejár a következő értékek egyikét.
  * **Abszolút:** a kulcs jár le, ha a megadott idő szerint **élő perc** éri el.
  * **NeverExpires:** a kulcs nem rendelkezik a lejárati idő.
  * **SlidingWindow:** a kulcs évül el, ha azt nem fért által meghatározott időtartamra **élő perc**; minden alkalommal, amikor érhető el, a lejárat óra lesz visszaállítva.
* **Élő perc:** percek függvényében a lejárati házirendet élő memcached kulcsok maximális számát.
* **A különböző szerepkörpéldányokat replikált biztonsági mentések a magas rendelkezésre állású:** engedélyezve van, ha a segít különböző szerepkörpéldányt beállítani a biztonsági mentések magas rendelkezésre állású használatával replikálja. Vegye figyelembe, hogy legalább két szerepkörpéldányokat érvényben kell lennie a funkció használatához a központi telepítést.

Új gyorsítótár hozzáadásához kattintson a **Hozzáadás** gombra a **gyorsítótárazását** tulajdonságlapját, és egy **nevű gyorsítótár konfigurálása** párbeszédpanel nyílik. Adja meg a tulajdonságait, amely a fent ismertetett.

Egy nevesített gyorsítótár módosításához jelölje ki a gyorsítótárban, és kattintson a **szerkesztése** gombra a **gyorsítótárazását** tulajdonságlapján. A párbeszédpanel nyílik meg, így módosíthatja a gyorsítótár tulajdonságai. Nyomja le az **OK** a gyorsítótár értékeket menteni.

A gyorsítótár törléséhez jelölje ki a gyorsítótárban, és kattintson a **eltávolítása** gombra a **gyorsítótárazását** tulajdonságlapját, és kattintson **Igen** gombra a törlés jóváhagyásához.

Gyorsítótárazás használatáról további információk: [hogyan használja a Co-located gyorsítótárazás][How to Use Co-located Caching].

<a name="certificates_properties"></a> 

### <a name="certificates-properties"></a>Tanúsítványok tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **tanúsítványok**.

![][ic710964]

Ezen a párbeszédpanelen belül adja hozzá, vagy távolítsa el az Eclipse-projekt által hivatkozott tanúsítványok. Vegye figyelembe, hogy az itt felsorolt tanúsítványok a Java keystore belül automatikusan nem tárolja, és ezért nem automatikusan elérhetők a Java-alkalmazások belül. Ezek csak regisztrált az Azure-ral, hogy azok lehet tölteni előzetesen tanúsítvány tárolja a virtuális gépek futnak a telepítés, és ezt követően használja a Windows más Windows szoftver. Jelenleg csak a szolgáltatás, amely a tanúsítványokat használ az eszközkészlet hivatkozott ezzel a módszerrel a **tanúsítványok** párbeszédpanel [SSL-Feladatkiszervezést][SSL Offloading], hogy az Internet Information Services (IIS) és alkalmazás alkalmazáskérelmek útválasztása (ARR), amelyeket az ilyen módon elérhetővé kell tenni a megfelelő tanúsítványt igénylő igényel.

A Közzététel varázsló használatával a projekt telepítésekor kérni fogja a személyes információcseréhez kapcsolódó (PFX) fájlokat ahhoz, hogy automatikusan feltöltheti ezeket az Azure szolgáltatásban, de csak akkor, ha azok nem lettek feltöltött nincs korábban ezeket a tanúsítványokat a jelszavuk megfelelő mutasson.

<a name="components_properties"></a> 

### <a name="components-properties"></a>Összetevő tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **összetevők**. Ezen a párbeszédpanelen belül, hogy a számítógép hozzáadása, módosítása, vagy a szerepkör-összetevők eltávolítása, valamint módosíthatja feldolgozásra kerülnek.

![][ic719502]

Az összetevők funkció lehetővé teszi az Azure-telepítés a projekthez, például a végrehajtható fájl parancssori utasításokat a telepítés során szükséges, a Java alkalmazások és a speciális fájlok függőségek.

Az egyes összetevők adhatja meg:

* A lépésre van szükség, ha az összetevő importálása az Azure-telepítés projektjéhez, ha be van építve.
* A lépésre van szükség, ha az adott Azure felhőben összetevőjének telepítése.

> [!NOTE]
> Összetevő-fájlok vagy parancssorok megadásakor vegye figyelembe, hogy a központi telepítés közzéteendő Windows rendszerű virtuális gép, ezért az egyéni lépéseket egy Windows-alapú operációs rendszer érvényesnek kell lennie. 
> 
> 

Összetevők a következő jellemzőkkel rendelkezik:

* **Importálás:** módszerrel, amely azt jelzi, hogyan összetevő importálja a a projekt, amikor a projekt épül. A következő értékek egyike lehet:
  * **másolás:** az összetevő által megadott helyi elérési út átmásolva a **a** tulajdonság a szerepkörhöz **approot** könyvtár.
  * **EAR:** tartozó összetevő állapota a Java enterprise archív (EAR) által megadott helyi elérési úton vállalati alkalmazás projekt importálása a **a** tulajdonság. (Ez automatikusan észlelt által az adott helyen a projekt jellege alapján eszközkészlet).
  * **JAR:** az összetevő egy Java-archívum (JAR), és importálja a Java-projektet által megadott helyi elérési úton a **a** tulajdonság. (Ez automatikusan észlelt által az adott helyen a projekt jellege alapján eszközkészlet).
  * **Nincs:** nem műveletet a komponens importálásához. Ez akkor alkalmazható, ha az összetevő már megtalálható a szerepkör feltételezett **approot** könyvtár, vagy ha az összetevő nem csupán egy végrehajtható fájl parancssori utasítás, ahogy a a **,** tulajdonság amikor a **telepítés** módszer **exec**.
  * **WAR:** egy Java webalkalmazások archívumából (WAR) az összetevő és a dinamikus webes projekt által megadott helyi elérési úton ről importálni lehet az **a** tulajdonság. (Ez automatikusan észlelt által az adott helyen a projekt jellege alapján eszközkészlet).
  * **zip:** az összetevő egy zip-fájlt, és importálja a megadott könyvtár vagy fájl által tömörítés a **a** tulajdonság.
* **Forrás:** forrás elérési útja a mappa vagy fájl, amely jelöli az elem (ek) importálhatja a központi telepítés a helyi számítógépen. Ez a tulajdonság Windows környezeti változók is használható. Az összes importálható összetevő importálja a szerepkör a **approot** könyvtár, amikor a projekt épül.
  
    Ne feledje, hogy lehetővé teszi egy letöltéssel összetevő telepítését, a felhő (nem a compute emulator) való üzembe helyezés esetén. Kapcsolódó információk alább egy összetevő hozzáadásáról.    
* **Mivel:** fájl nevét, amely alatt az összetevő importálja a a szerepkör **approot** könyvtárhoz, és végső soron telepített Azure felhőben. Ez a tulajdonság üresen tartsa a neve megegyezik, mert a helyi számítógépen. (Végrehajtható összetevők, azaz ezek alkalmazáscsoportokból **telepítés** módszer **exec**, ez lehet egy tetszőleges Windows parancssori utasítás.)
  
  > [!IMPORTANT]
  > Karaktereket használja ezt az értéket, ha azok eltérően kezelni a központi telepítés módszertől függően. Ha a telepítés módja **exec**, szóközök értelmezi parancssori argumentum szerepel, nem pedig a fájlnév részét. Bármely más módszereket, a szóközöket a fájlnév részét képező értelmezi.
  > 
  > 
* **Telepítés:** módszerrel, amely azt jelzi, a művelet az összetevő érvényesíthetők, ha a telepítés nincs elindítva. A következő értékek egyike lehet:
  
  * **másolás:** az összetevő másolja a megadott célhely elérési útja a **való** tulajdonság.
  * **Exec:** a összetevője egy végrehajtható Windows parancssori utasítás által megadott elérési út környezetében hajtotta végre a **való** tulajdonság, amikor elindul a központi telepítés.
  * **Nincs:** intézkedés összetevőnek akkor érvényes, ha a telepítés megkezdése.
  * **zip:** tartozó összetevő állapota tartalmazzák a megadott célhely elérési útja a **való** tulajdonság. Ez a módszer elérhető csak akkor, ha a **importálási** tulajdonság **zip**.
* **Ide:** elérési utat célként a virtuális gépen, hova szeretné telepíteni az összetevőt. Windows környezeti változók használhatók ehhez a tulajdonsághoz, és Fájlelérési utak relatív **approot**.

Új összetevőt hozzáadni, kattintson a **Hozzáadás** gombra a **összetevők** tulajdonságlapját, és egy **Azure szerepkör összetevő** párbeszédpanel nyílik. Adja meg a tulajdonságait, amely a fent ismertetett. 

A következő új WAR összetevő felvételéhez példáját mutatja be.

![][ic719503]

Ha a letöltés, az összetevőt telepíteni szeretné a felhőbe (nem a compute emulator), telepítésekor ügyeljen arra, hogy **felhő helyett a csomagban, beleértve a központi telepítése a** be van jelölve. Ha szeretné letölteni az Azure-tárfiókot, válassza ki a tárfiókot, a a **tárfiók** legördülő listából válassza ki (kattintson a **fiókok** hivatkozásra a lista a módosításához), amely részlegesen tölti ki a **URL-cím** mezőben, és töltse ki az URL-CÍMÉT a részét. Ha nem szeretné, hogy az Azure storage használatához, jelölje be **(nincs)** a a **tárfiók** legördülő listában, és adja meg az URL-címet az összetevőt a **URL-cím** mező. Adja meg a következő módszerek egyikét:

* **másolás:** a letöltési összetevő másolja a megadott célhely elérési útja a **való Directory** elérési útja.
* **azonos:** ugyanezt a módszert használja **központi telepítése a letöltési** megegyezik a **csomagból telepítés**.
* **zip:** a letölthető összetevője tartalmazzák a megadott célhely elérési útja a **való Directory** elérési útja.

Ha módosítani szeretné egy összetevő, válassza ki az összetevőt, majd kattintson a **szerkesztése** gombra a **összetevők** tulajdonságlapján. A párbeszédpanel nyílik meg, lehetővé téve a komponens tulajdonságainak módosítása. Nyomja le az **OK** az összetevő értékeket menteni.

Összetevő törléséhez válassza ki az összetevőt, majd kattintson a **eltávolítása** gombra a **összetevők** tulajdonságlapját, és kattintson **Igen** gombra a törlés jóváhagyásához.

Összetevők feldolgozása a megadott sorrendben. Használja a **fel** és **le** gombok sorrendjének meghatározásához.

> [!NOTE]
> A kiszolgálói konfigurációs szolgáltatás, valamint az összetevők támaszkodik. Ezek az összetevők nem távolítható el és nem szerkeszthető, a megfelelő kiszolgálókonfiguráció eltávolítása nélkül. Kérni fogja, amely kapcsolatos módosításokat szeretne ilyen összetevők megkísérlése során.
> 
> 

<!-- <a name="debugging_properties"></a> -->

<!-- ### Debugging properties -->
<!-- Open the context menu for the role in Eclipse's Project Explorer pane, click **Azure**, and then click **Debugging**. Within this dialog, you have the ability to enable or disable remote debugging, as well as create debug configurations, as shown in the following image. -->

<!-- ![][ic719504] -->

<!-- For related information about debugging, see [Debugging Azure Applications in Eclipse][Debugging Azure Applications in Eclipse]. -->

<a name="endpoints_properties"></a> 

### <a name="endpoints-properties"></a>Végpontok tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **végpontok**. Ezen a párbeszédpanelen belül, hogy a számítógép hozzon létre egy végpontot, valamint szerkesztheti vagy végpont, távolítsa el a következő ábrán látható módon.

![][ic719505]

A végpont hozzáadásához kattintson a **Hozzáadás** gombra a **végpontok** tulajdonságlapját, és egy **végpont hozzáadása** párbeszédpanel nyílik.

![][ic710897]

Adja meg a végpont nevét, válassza ki a típust (vagy **bemeneti**, **belső**, vagy **InstanceInput**), és adja meg a nyilvános és magánhálózati portot. Nyomja le az **OK** az új végpont értékeket menteni.

Attól függően, hogy milyen típusú végpont az alábbiak szerint lehetséges, hogy használjon porttartományokat:

* A példány bemeneti végpont, a nyilvános port portok tartománya is lehet (például **2000-2010**) és a magánhálózati port megadása rögzített érték.
* Belső végpont a nyilvános portot nem használja, és a magánhálózati port lehet egy tartományt, vagy üresen hagyott vagy adja meg a csillag karakter jelzi azt automatikusan beállítja az Azure-ban.
* A bemeneti végpontok a nyilvános port rögzített érték csak lehet, és a magánhálózati port egy rögzített érték, vagy üresen kell vagy meg csillagot ez automatikusan beállítja az Azure-ban is.

Széles helyett egy portszámot használni kívánt, ha hagyja üresen ezt a tartományt a végén a szövegmezőben.

A beállított automatikus, ha meg kell határoznia, melyik portot használja ténylegesen futásidőben, az alkalmazás használhat az Azure-szolgáltatás futásidejű API, amely ismerteti a [com.microsoft.windowsazure.serviceruntime csomag összefoglaló][com.microsoft.windowsazure.serviceruntime package summary].

<!-- To see how instance input endpoints can be used to help with debugging a multi-instance deployment, see [Debugging a specific role instance in a multi-instance deployment][Debugging a specific role instance in a multi-instance deployment]. -->

A végpont módosításához jelölje ki a végpontot, majd kattintson a **szerkesztése** gombra a **végpontok** tulajdonságlapján. A párbeszédpanel nyílik meg, így módosíthatja a végpont neve, típusa és nyilvános vagy privát portokkal. Nyomja le az **OK** menteni a módosított végpont értékeket.

A végpont törléséhez válassza ki a végpontot, majd kattintson a **eltávolítása** gombra a **végpontok** tulajdonságlapját, és kattintson **Igen** gombra a törlés jóváhagyásához.

Ahhoz, hogy megfelelően konfigurálni egyes (például a gyorsítótárazáshoz, munkamenet-affinitás vagy SSL-feladatkiszervezést) a felhasználói szerepkör által biztosított szolgáltatásokat, az eszközkészlet automatikus konfigurálása együtt felhasználói végpontok szereplő különleges végpontok. Az eszközkészlet megakadályozza, hogy a felhasználó szerkesztése vagy törlése például automatikusan generált végpontok, mindaddig, amíg a társított szolgáltatás engedélyezve van.

<a name="environment_variables_properties"></a> 

### <a name="environment-variables-properties"></a>Környezeti változók tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **környezeti változók**. Ezen a párbeszédpanelen belül, hogy a számítógép egy környezeti változó létrehozása, valamint módosítsa vagy távolítsa el a környezeti változó, a következő ábrán látható módon.

![][ic719506]

Környezeti változók használhatók az indítási parancsfájl a szerepkör indításakor.

> [!NOTE]
> Környezeti változók meghatározásakor vegye figyelembe, hogy a központi telepítés lesz közzétéve a Windows rendszerű virtuális gép, ezért érvényes Windows-alapú operációs rendszerhez a környezeti változók meg kell adni.
> 
> 

Például alatt elérhető, a szerepkör indulásakor környezeti változó, hozzon létre egy új környezeti változót kattintva a **Hozzáadás** gombra. A következő példa nevű környezeti változó **MyRoleVersion** alatt létrehozva. hozzárendeltük a érték **1.0**.

![][ic659268]

A jsp kód meg lehetett jelenjenek meg az értéket a `System.getenv` módszert:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Az alkalmazás futásakor, ami azt eredményezi, a kimenetet:

![][ic552233]

Egy környezeti változó módosításához jelölje ki a környezeti változó, és kattintson a **szerkesztése** gombra a **környezeti változók** tulajdonságlapján. Egy párbeszédpanel nyílik meg, hogy lehetővé teszi a környezeti változó tulajdonságainak módosítása. Nyomja le az **OK** a környezeti változó mentéséhez.

Egy környezeti változó törléséhez jelölje ki a környezeti változó, és kattintson a **eltávolítása** gombra a **környezeti változók** tulajdonságlapját, és kattintson **Igen** gombra a törlés jóváhagyásához.

Ahhoz, hogy megfelelően konfigurálni néhány funkciója (például a kiszolgálókonfiguráció távoli hibakeresés vagy helyi tároló) a felhasználói szerepkör által engedélyezett, az eszközkészlet automatikus konfigurálása különleges környezeti változókat, amelyek a felhasználó által definiált környezeti változók mellett megjelenik. Az eszközkészlet megakadályozza, hogy a felhasználó szerkesztése vagy törlése például automatikusan generált környezeti változókat, mindaddig, amíg a társított szolgáltatás engedélyezve van.

<a name="session_affinity_properties"></a> 

### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Terheléselosztás / munkamenet affinitás (más néven "állandóságát munkamenetek") tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **terheléselosztás**. Ezen a párbeszédpanelen belül, hogy a számítógép engedélyezheti vagy tilthatja le a munkamenet-kapcsolatot, a következő ábrán látható módon.

![][ic719492]

Kapcsolódó tudnivalókért lásd: [munkamenet affinitás][Session Affinity]. Megjegyzés: Ez a funkció viselkedését a környezetében SSL történő kiszervezésével a részben ismertetett módon [SSL-Feladatkiszervezést][SSL Offloading].

<a name="local_storage_properties"></a> 

### <a name="local-storage-properties"></a>Helyi tárolási tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **helyi tároló**. Ezen a párbeszédpanelen belül, hogy a számítógép létre, módosíthat vagy távolíthat el a virtuális gépen, amelyen fut az alkalmazás ideiglenes helyi tárhelyet. Adott értékekre, valamint hogy a tartalmak megmaradnak, amikor a szerepkör újraindul, az alábbi ábrán látható módon állítható be a helyi tárterület mérete.

![][ic719508]

Opcionálisan megadhat egy környezeti változó, amely megfelel a helyi tárterület.

Alapértelmezés szerint minden Azure telepített elhelyezett (és a unzipped) az a **approot** mappában található a szerepkör példánya. A legtöbb egyszerű telepítés után is kicsomagolás nincs elfér, amíg a terület számára lefoglalt-e a **approot** directory korlátozva, és jól meghatározott (kevesebb mint 1 GB-os ésszerű tapasztalatok). Ezért Azure biztosításához foglal le, elegendő szabad lemezterület, esetleg nem férnek el a nagyobb üzemelő példányok esetében a **approot** mappa, állítson be egy helyi tárolóhoz erőforrás használata a **helyi tároló** párbeszédpanel. Ehhez egyszerűen, lásd: [nagy méretű telepítéséhez telepítése][Deploying Large Deployments].

Az indítási parancsfájlok könnyen megtalálja a tárolási erőforrások (például a **startup.cmd**) használatával a környezeti változó automatikusan társított az Eclipse eszközkészlet által az erőforráshoz, ahogy az a **helyi tároló** párbeszédpanel. Környezeti változó tartalmazza a teljes elérési útját a helyi erőforrás úgy konfigurálta, az indítási parancsfájl végrehajtásakor. 

A helyi tároló egyik erőforrásához módosításához jelölje ki a helyi tároló-erőforrás, majd kattintson a **szerkesztése** gombra a **helyi tároló** tulajdonságlapján. Egy párbeszédpanel nyílik meg, hogy lehetővé teszi a helyi tároló-erőforrás tulajdonságainak módosítása. Nyomja le az **OK** a helyi tároló-erőforrás értékeket menteni.

A helyi tároló-erőforrás törléséhez jelölje ki a helyi tároló-erőforrás, majd kattintson a **eltávolítása** gombra a **helyi tároló** tulajdonságlapját, és kattintson **Igen** gombra a törlés jóváhagyásához.

<a name="server_configuration_properties"></a> 

### <a name="server-configuration-properties"></a>Kiszolgáló konfigurációs tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **kiszolgálókonfiguráció**. Ezen a párbeszédpanelen belül meg, hogy a számítógép hozzáadása, eltávolítása és módosíthatja a JDK és a Java-kiszolgáló a telepítés során használt, valamint hozzáadása vagy eltávolítása (például WAR, JAR vagy EAR fájlokat) a telepítés során használt alkalmazások.

### <a name="jdk-configuration"></a>JDK-konfiguráció
Ezen a párbeszédpanelen adja meg a telepítéshez használni a JDK-csomag teszi lehetővé. Eclipse használatakor a Windows megadhatja, hogy a JDK-csomag helyi Azure emulátorban készítésére használ, és lehetősége van a helyi telepítés telepítése az Azure-bA. Nem Windows operációs rendszereken az emulátor JDK beállítás nem alkalmazható, és a helyileg telepített JDK nem telepíthető, mert nem kompatibilis a Windows. Azonban az operációs rendszer által használt, függetlenül mindig közül választhat a 3. fél JDK csomagok telepítse az Azure, vagy a saját Windows-kompatibilis JDK-csomag egy másik letöltési helyről mutat.

A következő egy példa hogyan adhat meg a JDK Windows:

![][ic780647]

Használja az eclipse-ben Windows szolgáltatást, ha is megadhat egy JDK használata a compute emulator; Ehhez az szükséges, győződjön meg arról **használja ezt az elérési utat a JDK helyi tesztelés céljából** beadása a **emulátor telepítési** szakasz. Ezt követően adja meg a JDK; helyi elérési útja tallózással is kikeresheti azt másik JDK Ha szeretne használni egy nem engedélyezi automatikusan. Lehetősége is van a telepítése a JDK az Azure cloud service; Ehhez az szükséges, válassza ki a **központi telepítése a helyi JDK (automatikus feltöltés felhőalapú tárhelyre)** beállítást a **felhőalapú üzemelő példány** szakasz.

Megjegyzés: A nem Windows operációs rendszerek esetében a **emulátor telepítési** beállítások és a **központi telepítése a helyi JDK** beállítás nem érhetők el. Az alábbi példában látható, Mac vagy más támogatott-Windows operációs rendszert a JDK megadása:

![][ic789643]

Az operációs rendszer van, függetlenül a következő két rendelkezik **felhőalapú üzemelő példány** beállítások a forrás és a JDK-csomag típusa:

* **3. fél JDK csomag elérhető, az Azure telepítéséhez** 
* **Egyéni letöltéssel telepítése** 

Ha használja a **telepítsen központilag egy 3. fél JDK csomagot érhetők el az Azure** lehetőséget:

1. Jelölje be a jelölőnégyzetet, nevű **telepítsen központilag egy 3. fél JDK csomagot érhetők el az Azure**.
2. A legördülő listából válassza ki a 3. fél JDK Azure elérhető.
3. A **JDK** lapon fog kinézni a Windows a következőhöz hasonló: ![][ic780648] és a Mac OS az alábbihoz hasonló lesz, vagy nem Windows operációs rendszereken támogatott:![][ic789643]
4. Kattintson az **OK** gombra a módosítások mentéséhez.
5. Amikor a rendszer kéri, fogadja el a licencszerződést a 3. fél JDK csomag szolgáltató által, tekintse át a licencfeltételeket. Feltéve, hogy elfogadja a feltételeket, kattintson a **Igen** bezárásához a **licencszerződésének elfogadása** párbeszédpanel.
    Vegye figyelembe, hogy mely elemek az alapul szolgáló logikai az legördülő listában jelennek meg a **telepítsen központilag egy 3. fél JDK csomagot érhetők el az Azure** beállítás testre szabható. Az elemek testreszabása a a **JDK** párbeszédpanel, kattintson a **Testreszabás** hivatkozásra. Ez a művelet megszünteti az **JDK** tulajdonságlapját, és nyissa meg a **componentsets.xml** fájlt az eclipse-ben, amely ezt követően módosíthatja igény szerint. Dokumentációja **componentsets.xml** megtalálható a **componentsets.xml** magában.

Ha használja a **központi telepítése egy egyéni letöltéssel JDK** lehetőséget:

1. Hozzon létre egy ZIP a JDK telepítési könyvtára, amely biztosítja, hogy a könyvtár a csomópont önmaga a gyermek a ZIP-struktúra és a tartalma nem. Jegyezze fel a annak a könyvtárnak a nevét fogja később szüksége, és vegye figyelembe a JDK-telepítés a Windows rendszerű virtuális gép telepítve lesz.
2. Töltse fel az Azure-tárfiókot ZIP blob-ként. Ez egy eszközzel kívülről elérhető a blobok feltöltése az Azure storage teheti meg. Javasoljuk, hogy személyes blob használja. Jegyezze fel a ZIP tartalma blob URL-CÍMÉT.
3. Jelölje be a jelölőnégyzetet, nevű **központi telepítése egy egyéni letöltéssel JDK**.
    Ha szeretné letölteni az Azure-tárfiókot, válassza ki a tárfiókot, a a **tárfiók** legördülő listából válassza ki (kattintson a **fiókok** hivatkozásra a lista a módosításához), amely részlegesen tölti ki a **URL-cím** mezőben, és töltse ki az URL-CÍMÉT a részét. Ha nem szeretné, hogy az Azure storage használatához, jelölje be **(nincs)** a a **tárfiók** legördülő listában, és adja meg az URL-címet a JDK letölthető a **URL-cím** mező. Az Azure storage használata esetén az URL-címben a blob nevének kisbetűnek kell lennie.
4. Győződjön meg arról, hogy a **JAVA_HOME** szövegmező melyik könyvtár nevére hivatkozik. Alapértelmezés szerint a JDK könyvtár neve megegyezik a értéket választja, a helyi használatra ra hivatkozik. De ha a ZIP szerepel a könyvtár egy másik nevet (például, mert egy másik verzió használatával), frissítse a könyvtár neve a **JAVA_HOME** szövegmező ennek megfelelően, mivel ez a beállítás lesz a felhő (nem a compute emulator) használható.
5. Kattintson az **OK** gombra a módosítások mentéséhez.

Ennyi az egész. Most a felhő építésekor megfigyelheti a csomag mérete sokkal kisebb lesz, a felépítési folyamat általában rövidebb idő alatt elvégezhető, és a központi telepítést, maga a felhőben való közzétételekor is rövidebb idő alatt elvégezhető. Vegye figyelembe, hogy a **központi telepítése a helyi JDK (automatikus feltöltés felhőalapú tárhelyre)** vagy **központi telepítése egy egyéni letöltéssel JDK** beállítások érvényben, csak ha az alkalmazás központi telepítése a felhőben. Nincs hatással a compute emulator felhasználói élmény; rendelkeznek az összetevők verziója helyi a compute emulator telepítésekor továbbra is használható. 

### <a name="server-configuration"></a>Kiszolgálókonfiguráció
Hogyan kiszolgálót is megadhat egy alkalmazás például a következő:

![][ic796926]

Ellenőrizze, hogy a **ilyen típusú kiszolgáló központi telepítése** jelölőnégyzet van kiválasztva, és ezután válassza ki a használni kívánt kiszolgáló.

A kiszolgáló, a felhő telepítéshez meghatározásához, kihasználhatja az alábbiak közül:

1. **Elérhető az Azure-3. fél kiszolgáló központi telepítése** – különösen a fejlesztési és tesztelési célú forgatókönyvekben, ahol telepítési hatékonyságát és egyszerűségre prioritást és a kiszolgálók nem igényelnek egyéni konfiguráció alkalmazható. Vagy ha ezek a kiszolgálók egyikét kiindulási pontjaként használni kívánt, de felvehet megfelelő kiszolgálói testreszabási lépései a központi telepítés ügyfélindítási logikája.
2. **Egyéni letöltéssel telepítése** – Ez akkor különösen akkor alkalmazható, éles helyzetekben, amikor a felhőben használni kívánt kifejezetten előkészített és konfigurált kiszolgáló.
3. **Központi telepítése a helyi kiszolgáló telepítési** -különösen alkalmazható a Ha a helyi kiszolgáló telepítés már egyéni konfigurálva a használathoz. Ha ezt a lehetőséget választja, meg kell adni a helyi kiszolgáló elérési utat a **helyi kiszolgáló elérési útja** alábbi mezőbe.

Ha használja a **elérhető Azure 3. fél kiszolgáló központi telepítése** lehetőséget:

1. Jelölje be a jelölőnégyzetet, nevű **elérhető Azure 3. fél kiszolgáló központi telepítése**.
2. A legördülő menüből válassza ki a kívánt kiszolgáló szoftver használata a központi telepítés a felhőben. Megjegyzés: Ha Ön már meg van adva egy korábbi használni kívánt kiszolgálót típusú, akkor lesz korlátozva csak olyan felhőalapú kiszolgálót, amely a kiszolgáló típusa azonos termékcsalád kiválasztása. De ha egy kiszolgáló típusa nem választotta, választhat a kiszolgálók, amelyek jelenleg az Azure és a kiszolgáló típusa automatikusan be lesz az Ön.
3. Kattintson az **OK** gombra a módosítások mentéséhez.

Ha használja a **egyéni letöltéssel telepítés** lehetőséget:

1. Győződjön meg arról, hogy a kiszolgáló típusa alapján az előző lépések már ki van-e. Ez alapján a beépülő modul telepíti a kiszolgálót az egyéni le azt kell lennie, mint a kijelölt kiszolgáló típus ugyanazon operációs.
2. Jelölje be a jelölőnégyzetet, nevű **egyéni letöltéssel telepítés**.
    Ha szeretné letölteni az Azure-tárfiókot, válassza ki a tárfiókot, a a **tárfiók** legördülő listából válassza ki (kattintson a **fiókok** hivatkozásra a lista a módosításához), amely részlegesen tölti ki a **URL-cím** mező, majd töltse ki a kiszolgáló URL-CÍMÉT a részét letöltése a ZIP (az Azure-tárolót, az URL-címben a blob nevének kisbetűnek kell lennie). Ha nem szeretné, hogy az Azure storage használatához, jelölje be **(nincs)** a a **tárfiók** legördülő listában, és adja meg az URL-címet a kiszolgáló töltse le a ZIP a **URL-cím** mező. A ZIP tartalmazná egy gyermekmappába képviselő az alkalmazás server telepítési könyvtárába. Például egy zip használatakor az Apache Tomcat 7.0.35 belül a zip lenne az alárendelt mappát a telepítési könyvtárban, például a jelölő **apache-tomcat-7.0.35**. 
3. Adja meg a kezdőkönyvtár környezeti változó értékét. Az alapértelmezés szerint az értéket, a helyi alkalmazáskiszolgáló, ha vannak ilyenek, de egy másik értéket adhat meg, ha a felhő alkalmazáskiszolgáló eltér a helyi kiszolgáló. Azonban ügyeljen arra, hogy a felhő alkalmazáskiszolgáló ugyanabba a családba tartozó kiszolgálóként kell korábban kiválasztott típus.
    Ha a jövőben frissíti a felhő application server zip, manuálisan módosíthatja a kezdőkönyvtár beállítást, vagy pedig újra egyezik a helyi beállítás (Ha túl módosította a helyi kiszolgáló).
4. Kattintson az **OK** gombra a módosítások mentéséhez.

Az alapul szolgáló logikai, amelynek elemek jelennek meg a **Server** lapján a **kiszolgálókonfiguráció** tulajdonságok oldalán testre szabható. Ez az egy speciális funkció, ha az igényeinek túlnyúlnak az alapértelmezett értékeket, vagy vegyen fel további kiszolgálókat szeretne szükség lehet. A logikai testreszabása a a **Server** párbeszédpanel, kattintson a **Testreszabás** hivatkozás. Ez a művelet megszünteti az **kiszolgálókonfiguráció** tulajdonságlapját, és nyissa meg a **componentsets.xml** fájlt az eclipse-ben, amely ezt követően módosíthatja a kiszolgáló konfigurációs sablon kiterjeszteni igény szerint. Dokumentációja **componentsets.xml** megtalálható a **componentsets.xml** magában.

Ha használja a **központi telepítése a helyi kiszolgáló (automatikus feltöltés felhőalapú tárhelyre)** lehetőséget:

1. Jelölje be a jelölőnégyzetet, nevű **központi telepítése a helyi kiszolgáló (automatikus feltöltés felhőalapú tárhelyre)**.
2. Használja a **tárfiók** legördülő listában válassza **(automatikus)**. Ha megad **(automatikus)** itt, az Eclipse eszközkészlet fogja használni a tárfiókon a kiszolgáló választja, a központi telepítést, mint a **Azure közzététel** párbeszédpanel.
3. Kattintson az **OK** gombra a módosítások mentéséhez.

Válasszon ki egy kiszolgáló telepítési útvonalat a számítógépén a **helyi kiszolgáló elérési útja** szövegmezőben, a következő feltételek teljesülése esetén:

* Szeretné a központi telepítés tesztelése az emulátorban (Ez kizárólag Windows).
* Azt szeretné, a helyileg telepített kiszolgáló telepíti a felhőbe.
* Szeretné használni a saját, a felhőben, amelyben eset letölteni egy egyéni kiszolgáló, arról is győződjön meg a **központi telepítése a helyi kiszolgáló (automatikus feltöltés felhőalapú tárhelyre)** beállítást fent.

Ha az előző beállítások egyike alkalmazni a helyzet, a helyi kiszolgáló beállítás nem kötelező.

### <a name="applications-configuration"></a>Alkalmazások konfigurálása
Hogyan megadhatja az alkalmazás például a következő:

![][ic719512]

Kattintson a **Hozzáadás** egy másik alkalmazás hozzáadása vagy **eltávolítása** alkalmazás eltávolítása. Hatékonyságát célokra, ha szeretne egy letölthető egy alkalmazás forrását telepítéshez használandó a felhőbe, használja a [összetevők tulajdonságok](#components_properties) egy URL-cím tárfiókot, stb. 

A 2014. április a kiadástól kezdve az alkalmazások automatikusan frissíti a rendszer ugyanazt a tárfiókot az (alatt a **eclipsedeploy** tároló) megegyezik a a központi telepítéshez kiválasztott. A telepítés indítása logikát a lépést, amely először letölti a tárolási fiók ezeket az alkalmazásokat tartalmazza. Ez azt jelenti, hogy előfordulhat, hogy frissíti az alkalmazások a környezetben anélkül, hogy építse újra, és telepítse újra a teljes csomag manuális feltöltésével közvetlenül figyelembe, hogy storage (az Azure portál használatával például) az alkalmazás újabb verziója által az eszközkészlet eredetileg feltöltött cseréje a WAR-fájl nincs. Majd most indítsa el az összes szerepkör logikailemez Azure felügyeleti portál használatával újra, vagy a parancssori segédeszközök keresztül újrahasznosítását. (Kiváltó szerepkörök újrahasznosítása közvetlenül az Eclipse eszközkészlet belül jelenleg nem támogatott.)

### <a name="notes-about-server-configuration"></a>Kiszolgáló konfigurációjával kapcsolatos megjegyzések
Által végrehajtott módosítások a **kiszolgálókonfiguráció** tulajdonságok lapon megjelennek a `<component>` package.xml fájl elemek.

Ha a **automatikus feltöltése...**  vagy **le telepítése...**  beállítások a JDK vagy alkalmazáskiszolgálót, és Ön által létrehozott, a felhőben (nem a compute emulator), és csatlakozik a hálózathoz, azt tapasztalhatja, például a következő, a konzol kimeneti üzenetek létrehozása, a telepítsenek jelentéskészítő ellenőrzi a letöltési rendelkezésre állási:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Ha bejelölte a **le telepítése...**  beállítás, a következő figyelmeztetést is látható, de továbbra is a build:

`[windowsazurepackage] warning: Failed to confirm blob availability! Make sure the URL and/or the access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Ez a figyelmeztetés csak jelzi, hogy a letöltési rendelkezésre állási még nem nincs ellenőrizve. Ezért ha a központi telepítés a felhő valamilyen okból nem sikerül, ellenőrizze, hogy ha ezt a figyelmeztetést kapott.

Ha azt szeretné, hogy tiltsa le a letöltés ellenőrzése (például, ha úgy érzi, hogy azt feleslegesen lassítja a build), állítsa a `verifydownloads` attribútumot `false` a a `<windowsazurepackage>` package.xml eleme: 

`<windowsazurepackage verifydownloads="false" ...>` 

Ha bejelölte a **automatikus feltöltése...**  lehetőséget, majd a konzolablakban, látni fogja a build üzenetek reporting a feltöltés 5 másodpercentként, amikor szükség egy feltöltés előrehaladását.

<a name="ssl_offloading_properties"></a> 

### <a name="ssl-offloading-properties"></a>SSL-feladatkiszervezést tulajdonságai
Nyissa meg a helyi menüt a szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában kattintson **Azure**, és kattintson a **SSL-Feladatkiszervezést**. 

![][ic719481]

Ezen a párbeszédpanelen belül engedélyezheti SSL történő kiszervezésével, amellyel könnyen engedélyezheti a Hypertext Transfer Protocol Secure (HTTPS) támogatása az Azure, a Java környezetben anélkül, hogy a Java-alkalmazáskiszolgáló az SSL konfigurálását. További információkért lásd: [SSL-Feladatkiszervezést] [ SSL Offloading] és [hogyan használja az SSL-Feladatkiszervezést][How to Use SSL Offloading].

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Azure-projekt tulajdonságai][Azure Project Properties]

[Az Azure Storage-fiók listája][Azure Storage Account List]

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Project Properties]: http://go.microsoft.com/fwlink/?LinkID=699524
[Azure Storage Account List]: http://go.microsoft.com/fwlink/?LinkID=699528
[com.microsoft.windowsazure.serviceruntime package summary]: http://azure.github.io/azure-sdk-for-java/com/microsoft/windowsazure/serviceruntime/package-summary.html
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Debugging a specific role instance in a multi-instance deployment]: http://go.microsoft.com/fwlink/?LinkID=699535#debugging_specific_role_instance
[Debugging Azure Applications in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699535
[Deploying Large Deployments]: http://go.microsoft.com/fwlink/?LinkID=699536
[How to Use Co-located Caching]: http://go.microsoft.com/fwlink/?LinkID=699542
[How to Use SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699549

<!-- IMG List -->

[ic789599]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789599.png
[ic719499]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719499.png
[ic719483]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719483.png
[ic719501]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719501.png
[ic710964]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710964.png
[ic719502]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719502.png
[ic719503]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719503.png
[ic719504]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719504.png
[ic719505]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719505.png
[ic710897]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic710897.png
[ic719506]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719506.png
[ic659268]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic659268.png
[ic552233]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic552233.png
[ic719492]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719492.png
[ic719508]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719508.png
[ic780647]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780647.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic780648]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic780648.png
[ic789643]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic789643.png
[ic796926]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic796926.png
[ic719512]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719512.png
[ic719481]: ./media/azure-toolkit-for-eclipse-azure-role-properties/ic719481.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690945.aspx -->
