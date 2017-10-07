---
title: "aaaAzure szerepkör tulajdonságai"
description: "Ismerje meg, hogyan toouse hello Azure eszközkészlet Eclipse tooconfigure Azure felhasználóiszerep-beállítások."
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
ms.openlocfilehash: d111b4b9e4f12e49f38755bf6c9acc1a1de17a50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-role-properties"></a>Az Azure Role szerepkör tulajdonságai
Az Azure szerepkör különböző konfigurációs beállításainak hello Azure eszköztára Eclipse belül állítható be.

## <a name="configuring-azure-role-properties"></a>Az Azure szerepkör tulajdonságainak konfigurálása
A feldolgozói szerepkör esetében hello tulajdonság párbeszédpanelek segítségével történik, az Azure szerepkör tulajdonságainak konfigurálása. Helyi menü megnyitása hello hello szerepkörhöz a tartozó Eclipse Project Explorer ablaktáblában válassza hello **Azure** almenü. (Ha nem látja az Eclipse Project Explorer hello hello szerepkör, bontsa ki az Azure projektre a Project Explorer.)

![][ic789599]

különböző tulajdonságok is megadhatók a hello hello **tulajdonságok** Ez a témakör ismerteti a párbeszédpanelek. Vegye figyelembe, hogy számos tulajdonság automatikusan kitölti az Azure-telepítés új projekt létrehozásakor.

a következő tulajdonságlapjain hello Azure szerepkörök érhetők el.

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
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **tulajdonságok**, és fog rendelkezik hello képességét toochange hello virtuálisgép-mérethez, is módosíthatja példányok, ahogy az a következő kép hello hello száma.

![][ic719499]

> [!NOTE]
> Csak Windows: hello több példányban tooa érték 1-nél nagyobb és az alkalmazáskiszolgáló konfigurálnia, hello eszközkészlet engedélyezi, hogy szerepkör példánya csak 1 toorun hello emulátor ettől a beállítástól függetlenül. Ez a tooavoid port kötés ütközések hello különböző kiszolgálópéldányok (például az összes közben toobind tooport 8080) közötti amikor hello számítógépen futnak ugyanazon a számítógépen. A beállítás kívánt példányszám megmarad, de kerül érvénybe csak toohello felhő telepítésekor.
> 
> 

<a name="caching_properties"></a> 

### <a name="caching-properties"></a>Gyorsítótár tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **gyorsítótárazását**. Ezen a párbeszédpanelen belül engedélyezheti elnevezett közös elhelyezésű memcache-kompatibilis gyorsítótárak, lehetővé teszi a webalkalmazások mentése toohelp sebessége.

![][ic719483]

Hello belül **gyorsítótárazását** tulajdonságlapját, megadhatja a következő hello globális beállításait:

* közös elhelyezésű gyorsítótárazás engedélyezve van-e.
* hello gyorsítótárméret memória százalékában.
* hello tárfióknév hello gyorsítótár állapot mentéséhez, amikor az alkalmazás egy felhőalapú szolgáltatás, vagy egyik sem fut, ha nem szeretné, hogy toosave hello gyorsítótár állapotát. (hello tárfióknév nem használatos a hello compute emulator az alkalmazás futtatásakor.) Ha hello tárfiók neve túl**(automatikus)** (ez utóbbi érték hello alapértelmezett), a gyorsítótár konfigurációs automatikusan használja ugyanazt a tárfiókot hello hello egy hello lehetőséget választja, **tooAzureközzététele**párbeszédpanel.

> [!NOTE]
> Hello **(automatikus)** beállítás lesz szükség hello hatása, csak akkor, ha a központi telepítés hello Eclipse eszközkészlet segítségével közzéteszi közzététele varázsló. Ha inkább hello .cspkg fájl segítségével manuálisan külső mechanizmust, például a hello közzéteszi [Azure felügyeleti portálon][Azure Management Portal], hello központi telepítés nem fog megfelelően működni.
> 
> 

a következő párbeszédpanel hello hello tulajdonságok a gyorsítótár láthatók.

![][ic719501]

* **Name:** hello hello neve ugyanazon gyorsítótár.
* **Port száma:** hello port száma toouse hello gyorsítótár.
* **Elévülési szabályzatának:** hello a következő értékeket, amely meghatározza, hogy a kulcs hello gyorsítótárában lejáratának egyikét.
  * **Abszolút:** hello kulcs lejár, ha a megadott hello idő **toolive perc** éri el.
  * **NeverExpires:** hello kulcs nem rendelkezik a lejárati idő.
  * **SlidingWindow:** hello kulcs évül el, ha azt nem fért által megadott hello időtartamig **toolive perc**; minden alkalommal, amikor érhető el, hello lejárati óra lesz visszaállítva.
* **Perc toolive:** legfeljebb hány perc toolive, memcached kulcsok tulajdonos toohello elévülési szabályzatának.
* **A különböző szerepkörpéldányokat replikált biztonsági mentések a magas rendelkezésre állású:** engedélyezve van, ha a segít különböző szerepkörpéldányt beállítani a biztonsági mentések magas rendelkezésre állású használatával replikálja. Vegye figyelembe, hogy legalább két szerepkörpéldányokat érvényben kell lennie a szolgáltatás toowork a központi telepítést.

tooadd új gyorsítótárat, kattintson a hello **Hozzáadás** hello gombjára **gyorsítótárazását** tulajdonságlapját, és egy **nevű gyorsítótár konfigurálása** párbeszédpanel nyílik. Hello tulajdonságait, amely a fent ismertetett adja meg.

toomodify egy elnevezett gyorsítótárral, válassza ki a hello gyorsítótár, majd kattintson a hello **szerkesztése** hello gombjára **gyorsítótárazását** tulajdonságlapján. A párbeszédpanel nyílik meg így Ön toomodify hello gyorsítótár tulajdonságai. Nyomja le az **OK** toosave hello gyorsítótár értékeket.

toodelete gyorsítótárat, válassza ki a hello gyorsítótár, majd kattintson a hello **eltávolítása** hello gombjára **gyorsítótárazását** tulajdonságlapját, és kattintson **Igen** tooconfirm hello törlését.

További információt a toouse gyorsítótárazás, lásd: [hogyan tooUse közös elhelyezésű gyorsítótárazását][How tooUse Co-located Caching].

<a name="certificates_properties"></a> 

### <a name="certificates-properties"></a>Tanúsítványok tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **tanúsítványok**.

![][ic710964]

Ezen a párbeszédpanelen belül adja hozzá, vagy távolítsa el az Eclipse-projekt által hivatkozott tanúsítványok. Vegye figyelembe, hogy az itt felsorolt hello tanúsítványok automatikusan nem tárolja a Java keystore belül, és ezért nem automatikusan elérhetők a Java-alkalmazások belül. Ezek csak regisztrált az Azure-ral, hogy azok is kell előre feltölti a tanúsítványt a telepítést futtató hello virtuális gépeken tárolja, és ezt követően használja a Windows hello más Windows szoftver. Csak a szolgáltatás által használt tanúsítványok hello hello eszközkészlet hivatkozott ezzel a módszerrel hello jelenleg hello **tanúsítványok** párbeszédpanel [SSL-Feladatkiszervezést][SSL Offloading], határidő tooits Internet Information Services (IIS) és alkalmazás alkalmazáskérelmek útválasztása (ARR), amely igényli igényel hello megfelelő tanúsítványt toobe elérhetővé tenni az ilyen módon.

Központi telepítésekor a projekt tooAzure hello közzététele varázsló segítségével, a személyes információcseréhez kapcsolódó (PFX) fájlok toothese tanúsítványok jelszavukat, valamint megfelelő sorrendben tooautomatically feltöltésükhöz toohello hello felszólító toopoint lesz-e Az Azure szolgáltatást, de csak akkor, ha azok nem lettek feltöltött nincs korábban.

<a name="components_properties"></a> 

### <a name="components-properties"></a>Összetevő tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **összetevők**. Ezen a párbeszédpanelen belül meg hello képességét tooadd rendelkezik, módosít, vagy hello összetevők a szerepkör eltávolítása, valamint feldolgozásra kerülnek hello sorrendjének módosítása.

![][ic719502]

hello összetevők funkciójával tooadd függőségek tooyour az Azure-telepítés projekt, például a Java-alkalmazások, különleges fájlok és végrehajtható fájl parancssori utasításokat a telepítés során szükséges.

Az egyes összetevők adhatja meg:

* hello lépés toobe hajt végre, amikor hello összetevő importálása az Azure-telepítés projektjéhez, ha be van építve.
* hello lépés toobe hajt végre, amikor üzembe helyezése az Azure-felhőbe hello összetevőt.

> [!NOTE]
> Összetevő-fájlok vagy parancssorok megadásakor vegye figyelembe, hogy a központi telepítés lesz-e közzétett tooa Windows rendszerű virtuális gép, így az egyéni lépéseket egy Windows-alapú operációs rendszer érvényesnek kell lennie. 
> 
> 

Összetevők hello alábbi tulajdonságokkal rendelkeznek:

* **Importálás:** módszerrel, amely azt jelzi, hogyan hello összetevő importálja a hello projekt amikor hello projekt épül. Ez hello a következő értékek egyike lehet:
  * **másolás:** hello összetevő átmásolva hello hello által megadott helyi elérési **a** hello szerepkörbe tulajdonság **approot** könyvtár.
  * **EAR:** hello összetevője egy Java enterprise archív (EAR) hello hello által megadott helyi elérési úton vállalati alkalmazás projekt importálása **a** tulajdonság. (Ez automatikusan észlelt által hello eszközkészlet hello jellege hello projekt helye alapján).
  * **JAR:** hello összetevő egy Java-archívum (JAR), és importálja a Java-projektet hello hello által megadott helyi elérési úton **a** tulajdonság. (Ez automatikusan észlelt által hello eszközkészlet hello jellege hello projekt helye alapján).
  * **Nincs:** tooimport hello összetevő történik semmi. Ez akkor alkalmazható, ha hello összetevő feltételezett tooalready megtalálható hello szerepkör **approot** könyvtár, vagy ha a hello eleme csak egy végrehajtható fájl parancssori utasítás, mint a megadott hello **,**tulajdonságot, ha hello **telepítés** módszer **exec**.
  * **WAR:** hello összetevő egy Java webalkalmazások archívumából (WAR), és importálja a dinamikus webes projekt hello hello által megadott helyi elérési úton **a** tulajdonság. (Ez automatikusan észlelt által hello eszközkészlet hello jellege hello projekt helye alapján).
  * **zip:** hello összetevő egy zip-fájlt, és például .zip fájlba hello könyvtár vagy hello által megadott által importált **a** tulajdonság.
* **Forrás:** forrás elérési útja a helyi számítógép toohello mappa vagy fájl, amely hello elem(ek) tooimport tooyour telepítését jelenti. Ez a tulajdonság Windows környezeti változók is használható. Az összes importálható összetevő importálja a hello szerepkör **approot** könyvtárából hello projekt épül.
  
    Ne feledje, hogy Ön hello képességét toodeploy letöltéssel összetevő toohello felhő (nem hello compute emulator) telepítésekor. Kapcsolódó információk alább egy összetevő hozzáadásáról.    
* **Mivel:** fájlnév alapján mely hello összetevő importálja a hello szerepkör **approot** könyvtárhoz, és végső soron telepített hello Azure felhőben található. Ez a tulajdonság üres tookeep hello azonos, mert a helyi számítógépen hello van neve hello hagyja. (Végrehajtható összetevők, azaz ezek alkalmazáscsoportokból **telepítés** mód értéke túl**exec**, ez lehet egy tetszőleges Windows parancssori utasítás.)
  
  > [!IMPORTANT]
  > Karaktereket használja ezt az értéket, ha azok kezelése másképp attól függően, hogy hello metódus telepítése. Ha hello telepítése metódus **exec**, szóközök értelmezi parancssori argumentum szerepel, nem pedig hello fájlnév részét. Bármely más módszereket, a tárolóhelyek hello fájlnév részeként értelmezi.
  > 
  > 
* **Telepítés:** módszerrel, amely azt jelzi, hello művelet toohello összetevő lépnek érvénybe, ha hello telepítés nincs elindítva. Ez hello a következő értékek egyike lehet:
  
  * **másolás:** hello összetevője hello által megadott másolt toohello célútvonalat **való** tulajdonság.
  * **Exec:** hello összetevője egy végrehajtható Windows parancssori utasítás végrehajtása hello környezetében hello által megadott hello elérési **való** tulajdonság hello amikor hello telepítése elindul.
  * **Nincs:** semmilyen műveletet nem alkalmazott toohello összetevő hello telepítési indításakor.
  * **zip:** hello összetevője hello által megadott tömörítetlen toohello célútvonalat **való** tulajdonság. Ez a módszer áll rendelkezésre, csak ha hello **importálási** tulajdonság **zip**.
* **Ide:** cél elérési útja, hova szeretné telepíteni hello összetevő hello virtuális gépen. Windows környezeti változók használhatók ehhez a tulajdonsághoz, és Fájlelérési utak relatív túl**approot**.

tooadd új összetevőt, kattintson a hello **Hozzáadás** hello gombjára **összetevők** tulajdonságlapját, és egy **Azure szerepkör összetevő** párbeszédpanel nyílik. Hello tulajdonságait, amely a fent ismertetett adja meg. 

hello következő hozzáadásához egy új WAR-összetevő példáját mutatja be.

![][ic719503]

Ha telepítése toohello felhő (nem hello compute emulator), ha azt szeretné, hogy toodeploy hello összetevő letöltéssel, győződjön meg arról, hogy **felhő helyett hello csomagban, beleértve a központi telepítése a** be van jelölve. Ha toodownload az Azure-tárfiókot, válassza hello tárfiókot hello **tárfiók** legördülő listából válassza ki (hello kattintva **fiókok** hivatkozás toomodify a hello lista), ami részlegesen kitölti hello **URL-cím** mezőben, majd töltse ki a hello hello URL-cím része maradt. Ha nem szeretné, hogy az Azure storage toouse, válassza ki a **(nincs)** a hello **tárfiók** legördülő listában, és adja meg az URL-cím hello tooyour összetevő hello **URL-cím** mező. Meg kell adni, hello a következő módszerek egyikét:

* **másolás:** hello letölthető összetevője hello által megadott másolt toohello célútvonalat **tooDirectory** elérési útja.
* **azonos:** hello ugyanezt a módszert használja **központi telepítése a letöltési** megegyezik a **csomagból telepítés**.
* **zip:** hello letölthető összetevője hello által megadott tömörítetlen toohello célútvonalat **tooDirectory** elérési útja.

a komponens, jelölje be hello összetevő, és kattintson hello toomodify **szerkesztése** hello gombjára **összetevők** tulajdonságlapján. A párbeszédpanel nyílik meg így Ön toomodify hello összetevő tulajdonságai. Nyomja le az **OK** toosave hello összetevő értékeket.

a komponens, jelölje be hello összetevő, és kattintson hello toodelete **eltávolítása** hello gombjára **összetevők** tulajdonságlapját, és kattintson **Igen** tooconfirm hello törlése.

Összetevők dolgoznak fel a hello sorrendben. Használjon hello **fel** és **le** gombok tooarrange hello sorrendje.

> [!NOTE]
> hello kiszolgálói konfigurációs szolgáltatás, valamint az összetevők támaszkodik. Ezek az összetevők nem távolítható el és nem szerkeszthető, hello megfelelő kiszolgálókonfiguráció eltávolítása nélkül. Kérni fogja, amely kapcsolatos toomake módosítások toosuch összetevők megkísérlése során.
> 
> 

<!-- <a name="debugging_properties"></a> -->

<!-- ### Debugging properties -->
<!-- Open hello context menu for hello role in Eclipse's Project Explorer pane, click **Azure**, and then click **Debugging**. Within this dialog, you have hello ability tooenable or disable remote debugging, as well as create debug configurations, as shown in hello following image. -->

<!-- ![][ic719504] -->

<!-- For related information about debugging, see [Debugging Azure Applications in Eclipse][Debugging Azure Applications in Eclipse]. -->

<a name="endpoints_properties"></a> 

### <a name="endpoints-properties"></a>Végpontok tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **végpontok**. Ezen a párbeszédpanelen belül el hello képességét toocreate végpont, valamint szerkesztése, vagy távolítsa el a végpont, ahogy az a következő kép hello.

![][ic719505]

egy végpont, tooadd kattintson hello **Hozzáadás** hello gombjára **végpontok** tulajdonságlapját, és egy **végpont hozzáadása** párbeszédpanel nyílik.

![][ic710897]

Adjon hello végpont nevét, válassza ki a hello típusa (vagy **bemeneti**, **belső**, vagy **InstanceInput**), és adja meg hello nyilvános és magánhálózati portot. Nyomja le az **OK** toosave hello új végpont értékeket.

Végpont hello típusától függően előfordulhat, hogy használhatja porttartományok az alábbiak szerint:

* A példány bemeneti végpont hello nyilvános port lehet portok tartománya (például **2000-2010**) és magánhálózati portot hello rögzített érték.
* Belső végpont hello nyilvános portot nem használja, és a tartományt, vagy üresen vagy set tooan csillag tooindicate ez automatikusan beállítja az Azure-ban hello magánhálózati port lehet.
* A bemeneti végpontok hello nyilvános port csak egy rögzített érték lehet, és hello magánhálózati port lehet egy rögzített érték, vagy üresen vagy set tooan csillag tooindicate ez automatikusan beállítja az Azure-ban.

Ha azt szeretné, hogy toouse széles helyett egy portszámot, hagyja üresen hello szövegmező hello hello tartomány végéig.

A portok beállítása tooautomatic, ha toodetermine kell futásidőben ténylegesen használt portot, az alkalmazás használhat hello szereplő hello Azure szolgáltatás futásidejű API [com.microsoft.windowsazure.serviceruntime csomag összegző][com.microsoft.windowsazure.serviceruntime package summary].

<!-- toosee how instance input endpoints can be used toohelp with debugging a multi-instance deployment, see [Debugging a specific role instance in a multi-instance deployment][Debugging a specific role instance in a multi-instance deployment]. -->

toomodify egy végpontot, válassza ki a hello végpont, majd kattintson a hello **szerkesztése** hello gombjára **végpontok** tulajdonságlapján. A párbeszédpanel nyílik meg, így toomodify hello végpont nevét, típusát és nyilvános vagy privát portokkal. Nyomja le az **OK** toosave hello módosított végpont értékeket.

toodelete egy végpontot, válassza ki a hello végpont, majd kattintson a hello **eltávolítása** hello gombjára **végpontok** tulajdonságlapját, és kattintson **Igen** tooconfirm hello törlését.

A sorrend tooproperly néhány hello szolgáltatást (például a gyorsítótárazáshoz, munkamenet-affinitás vagy SSL-feladatkiszervezést) hello felhasználói szerepkör által engedélyezett beállításához hello eszközkészlet előfordulhat, hogy automatikusan konfigurálhatja a speciális végpontok, amelyek a felhasználó által definiált végpontok együtt jelenik. hello eszközkészlet megakadályozza, hogy a hello felhasználó szerkesztése vagy törlése ilyen automatikusan generált végpontok mindaddig, amíg hello társított szolgáltatás engedélyezve van.

<a name="environment_variables_properties"></a> 

### <a name="environment-variables-properties"></a>Környezeti változók tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **környezeti változók**. Ezen a párbeszédpanelen belül hello képességét toocreate rendelkezik egy környezeti változó, valamint módosítása vagy törlése egy környezeti változó hello kép a következő ábrán.

![][ic719506]

Környezeti változók a rendelkezésre álló tooyour indítási parancsfájl hello szerepkör indításakor.

> [!NOTE]
> Környezeti változók meghatározásakor vegye figyelembe, hogy a központi telepítés lesz közzétett tooa Windows rendszerű virtuális gép, ezért érvényes Windows-alapú operációs rendszerhez a környezeti változók meg kell adni.
> 
> 

Például alatt elérhető hello szerepkör indulásakor környezeti változó, hozzon létre egy új környezeti változót hello kattintva **Hozzáadás** gombra. hello következő mutatja nevű környezeti változó **MyRoleVersion** létrehozott és a hello értéket hozzárendelni **1.0**.

![][ic659268]

A jsp kód hello értékét hello segítségével lehetett megjeleníteni `System.getenv` módszert:

    <body>
      <b> Hello World!</b>
      <p>Running role version: <%= System.getenv("MyRoleVersion") %></p>
    </body>

Az alkalmazás futásakor, ami azt eredményezi, a kimenetet:

![][ic552233]

egy környezeti változó toomodify jelölje ki a hello környezeti változót, majd kattintson a hello **szerkesztése** hello gombjára **környezeti változók** tulajdonságlapján. A párbeszédpanel nyílik meg így Ön toomodify hello környezeti változó tulajdonságai. Nyomja le az **OK** toosave hello környezeti változók értékeit.

egy környezeti változó toodelete jelölje ki a hello környezeti változót, majd kattintson a hello **eltávolítása** hello gombjára **környezeti változók** tulajdonságlapját, és kattintson **Igen**tooconfirm hello törlését.

A sorrend tooproperly konfigurálásához, néhány hello szolgáltatást (például a kiszolgálókonfiguráció távoli hibakeresés vagy helyi tároló) hello felhasználói szerepkör által engedélyezett, a hello eszközkészlet automatikusan előfordulhat, hogy konfigurálásához különleges környezeti változókat, amelyek együtt jelenik felhasználó által definiált környezeti változókat. hello eszközkészlet megakadályozza, hogy a hello felhasználó szerkesztése vagy törlése ilyen automatikusan generált környezeti változókat, mindaddig, amíg hello társított szolgáltatás engedélyezve van.

<a name="session_affinity_properties"></a> 

### <a name="load-balancing--session-affinity-aka-sticky-sessions-properties"></a>Terheléselosztás / munkamenet affinitás (más néven "állandóságát munkamenetek") tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **terheléselosztás**. Ezen a párbeszédpanelen belül hello képességét tooenable rendelkezik, vagy tiltsa le azt a munkamenet, ahogy az a következő kép hello.

![][ic719492]

Kapcsolódó tudnivalókért lásd: [munkamenet affinitás][Session Affinity]. Megjegyzés: Ez a funkció viselkedését hello környezetében SSL történő kiszervezésével a részben ismertetett módon [SSL-Feladatkiszervezést][SSL Offloading].

<a name="local_storage_properties"></a> 

### <a name="local-storage-properties"></a>Helyi tárolási tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **helyi tároló**. Ezen a párbeszédpanelen belül hello képességét toocreate rendelkezik, módosítása vagy törlése hello virtuális géphez, hogy az alkalmazás fut ideiglenes helyi tárterület. Adott értékekre, valamint hogy hello tartalmak megmaradnak, amikor hello szerepkör újraindul, ahogy az a következő kép hello hello helyi tárhelyre – hello méretére állítható be.

![][ic719508]

Opcionálisan megadhat egy környezeti változó, amely megfelel a toohello helyi tárterület.

Alapértelmezés szerint minden telepített Azure van helyezve (és unzipped) hello **approot** hello szerepkörpéldányt mappájában. Amíg a legtöbb egyszerű telepítés után is kicsomagolás, hello számára lefoglalt hello terület nincs elfér **approot** directory korlátozva, és jól meghatározott (kevesebb mint 1 GB-os ésszerű tapasztalatok). Ezért tooensure Azure foglal le elegendő szabad lemezterület, esetleg nem férnek el a hello nagyobb telepítések **approot** mappa, állítson be egy helyi tárolóhoz erőforrást hello **helyi tároló** párbeszédpanel. Az egy egyszerűen toodo a, lásd: [nagy méretű telepítéséhez telepítése][Deploying Large Deployments].

Indítási parancsfájlok alapján könnyen megtalálja hello tárolási erőforrások (például a **startup.cmd**) automatikusan társított hello Eclipse eszközkészlet által hello erőforrás hello környezeti változó használatával látható módon hello  **Helyi tároló** párbeszédpanel. Környezeti változó hello helyi erőforrás hello időben az indítási parancsfájl végrehajtása konfigurálását hello elérési útját fogja tartalmazni. 

toomodify a helyi tároló egyik erőforrásához, jelölje ki a helyi tároló-erőforrás hello, majd kattintson a hello **szerkesztése** hello gombjára **helyi tároló** tulajdonságlapján. A párbeszédpanel nyílik meg így Ön toomodify hello helyi storage erőforrás-tulajdonságok. Nyomja le az **OK** toosave hello helyi storage erőforrás-értékek.

toodelete a helyi tároló egyik erőforrásához, jelölje ki a helyi tároló-erőforrás hello, majd kattintson a hello **eltávolítása** hello gombjára **helyi tároló** tulajdonságlapját, és kattintson **Igen** tooconfirm hello törlését.

<a name="server_configuration_properties"></a> 

### <a name="server-configuration-properties"></a>Kiszolgáló konfigurációs tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **kiszolgálókonfiguráció**. Ezen a párbeszédpanelen belül hello képességét tooadd, távolítsa el, és hello JDK és a telepítés során használt Java-alkalmazáskiszolgáló módosítása, valamint hozzáadása vagy eltávolítása hello alkalmazások (például WAR, JAR vagy EAR fájlokat) a telepítés során használt rendelkezik.

### <a name="jdk-configuration"></a>JDK-konfiguráció
Ezen a párbeszédpanelen lehetővé teszi toospecify hello JDK csomag toouse az üzembe helyezéshez. Eclipse használatakor a Windows hello JDK csomag toouse helyileg a jelentés futtatásakor a hello Azure emulátorban, és a helyi telepítési tooAzure rendelkezik hello beállítás toodeploy megadhat. Nem Windows operációs rendszereken hello emulátor JDK beállítás nem alkalmazható, és nem telepíthet központilag hello helyileg telepített JDK, mert nem kompatibilis a Windows. Azonban Ön által használt operációs rendszer hello, függetlenül mindig közül választhat 3. fél JDK csomagok toodeploy tooAzure hello, vagy a saját Windows-kompatibilis JDK-csomag egy másik letöltési helyről mutat.

hello az alábbiakban olvashat, hogyan adhat meg a JDK Windows példát:

![][ic780647]

Ha használja az eclipse-ben Windows szolgáltatást, megadhatja, a JDK toouse hello a számítási emulátor; toodo, ezért győződjön meg arról **használata hello JDK-útvonalról a helyi tesztelés céljából** hello beadása **emulátor telepítési** szakasz. Ezt követően adja a hello helyi elérési út tooyour JDK; Ha hello egy toouse nincs kiválasztva automatikusan megkeresheti toodifferent JDK. Azt is, hogy hello beállítás toodeploy a JDK tooyour Azure-felhőszolgáltatásban; toodo Igen, válassza ki a hello **központi telepítése a helyi JDK (automatikus – feltöltés toocloud tároló)** hello beállítása **felhőalapú üzemelő példány** szakasz.

Megjegyzés: A nem Windows operációs rendszereken, hello **emulátor telepítési** beállítások és hello **központi telepítése a helyi JDK** beállítás nem érhetők el. hello alábbi példában látható módon adja meg a JDK Mac számítógépen, vagy más támogatott a nem Windows operációs rendszer:

![][ic789643]

Függetlenül a hello operációs rendszer rendelkezik a következő két hello **felhőalapú üzemelő példány** hello forrás, és írja be a JDK-csomag beállításai:

* **3. fél JDK csomag elérhető, az Azure telepítéséhez** 
* **Egyéni letöltéssel telepítése** 

Hello használata **telepítsen központilag egy 3. fél JDK csomagot érhetők el az Azure** lehetőséget:

1. Nevű hello jelölőnégyzetet **telepítsen központilag egy 3. fél JDK csomagot érhetők el az Azure**.
2. Hello legördülő listából válassza ki a hello 3. fél JDK csomagot, amely elérhető az Azure-on.
3. A **JDK** lapon hasonló toohello következőket a Windows fog kinézni: ![][ic780648] és a Mac OS hasonló toohello következő fog kinézni, vagy nem Windows operációs rendszereken támogatott:![][ic789643]
4. Kattintson a **OK** toosave a módosításokat.
5. Amikor a kért tooaccept hello licencszerződése hello 3. fél JDK csomag szolgáltató, hello licencfeltételek áttekintéséhez. Feltéve, hogy elfogadja hello feltételeket, kattintson a **Igen** tooclose hello **licencszerződésének elfogadása** párbeszédpanel.
    Vegye figyelembe, hogy az alapul szolgáló logikát, amelynek elemek jelennek meg a legördülő listában hello hello hello **telepítsen központilag egy 3. fél JDK csomagot érhetők el az Azure** beállítás testre szabható. toocustomize hello elemek, hello **JDK** párbeszédpanel, kattintson a hello **Testreszabás** hivatkozásra. Ez a művelet megszünteti hello **JDK** tulajdonságlapját, és a nyitott hello **componentsets.xml** fájlt az eclipse-ben, amely ezt követően módosíthatja igény szerint. Dokumentációja **componentsets.xml** hello megtalálható **componentsets.xml** magában.

Hello használata **központi telepítése egy egyéni letöltéssel JDK** lehetőséget:

1. Hozzon létre egy ZIP a JDK telepítőkönyvtár maga hello directory csomópont nem gyermeke hello hello ZIP struktúra és a tartalma nem biztosítására. Jegyezze fel a hello directory hello neve lesz később szüksége, és vegye figyelembe a JDK telepítési telepített tooa Windows rendszerű virtuális gép lesz.
2. Hello ZIP be az Azure storage-fiókjába feltöltése a blob-ként. Ehhez egy kívülről elérhető eszközzel tooAzure tárolási BLOB feltöltése. Az ajánlott toouse titkos blob. Jegyezze fel a hello ZIP tartalmának hello blob URL-CÍMÉT.
3. Nevű hello jelölőnégyzetet **központi telepítése egy egyéni letöltéssel JDK**.
    Ha toodownload az Azure-tárfiókot, válassza hello tárfiókot hello **tárfiók** legördülő listából válassza ki (hello kattintva **fiókok** hivatkozás toomodify a hello lista), ami részlegesen kitölti hello **URL-cím** mezőben, majd töltse ki a hello hello URL-cím része maradt. Ha nem szeretné, hogy az Azure storage toouse, válassza ki a **(nincs)** a hello **tárfiók** legördülő listában, és adja meg a hello URL-cím tooyour JDK töltse le a hello **URL-cím** mező. Az Azure storage használata esetén a blob nevének hello URL-címben kisbetűnek kell lennie.
4. Győződjön meg arról, hogy hello **JAVA_HOME** szövegmező toohello megfelelő könyvtárnév hivatkozik. Alapértelmezés szerint ra hivatkozik hello JDK könyvtár neve megegyezik a helyi használatra választott hello érték. De ha hello directory hello ZIP szerepel egy másik nevet (például esedékes toousing egy másik verzió), frissítés hello könyvtárnév hello **JAVA_HOME** szövegmező ennek megfelelően, mivel ez a beállítás lesz a hello cloud () nem található hello compute emulator).
5. Kattintson a **OK** toosave a módosításokat.

Ennyi az egész. Most összeállításakor hello felhő megfigyelheti hello csomag mérete sokkal kisebb lesz, hello felépítési folyamat általában rövidebb idő alatt elvégezhető, és maga toohello felhő közzétételekor hello telepítési is rövidebb idő alatt elvégezhető. Vegye figyelembe, hogy hello **központi telepítése a helyi JDK (automatikus – feltöltés toocloud tároló)** vagy **központi telepítése egy egyéni letöltéssel JDK** beállítások érvényben, csak ha az alkalmazás központi telepítése hello felhőben. Nincs hatással a compute emulator felhasználói élmény; rendelkeznek helyi verziója hello hello összetevők továbbra is használható, toohello compute emulator központi telepítésekor. 

### <a name="server-configuration"></a>Kiszolgálókonfiguráció
hello hogyan kiszolgálót is megadhat egy alkalmazás egy példa látható.

![][ic796926]

Győződjön meg arról, hogy hello **ilyen típusú kiszolgáló központi telepítése** jelölőnégyzet van kiválasztva, és válassza a hello típus az alkalmazáskiszolgáló toouse szeretné.

A kiszolgáló toouse felhő üzembe helyezése a meghatározásához, kihasználhatja az alábbi beállítások hello:

1. **Elérhető az Azure-3. fél kiszolgáló központi telepítése** – különösen a fejlesztési és tesztelési célú forgatókönyvekben, ahol telepítési hatékonyságát és egyszerűségre prioritást és hello kiszolgálók nem igényelnek egyéni konfiguráció alkalmazható. Vagy ha kiindulási pontjaként hello kívánt toouse, ezek a kiszolgálók egyikét, de felvehet megfelelő kiszolgálói testreszabási lépéseit a központi telepítés ügyfélindítási logikája.
2. **Egyéni letöltéssel telepítése** – Ez akkor éles forgatókönyvekben különösen akkor alkalmazható, ha a megjeleníteni kívánt hello felhőben toouse kifejezetten előkészített és konfigurált kiszolgáló.
3. **Központi telepítése a helyi kiszolgáló telepítési** -különösen alkalmazható a Ha a helyi kiszolgáló telepítés már egyéni konfigurálva a használathoz. Ha ezt a lehetőséget választja, meg kell adnia a helyi kiszolgáló elérési útja hello a **helyi kiszolgáló elérési útja** alábbi mezőbe.

Hello használata **elérhető Azure 3. fél kiszolgáló központi telepítése** lehetőséget:

1. Nevű hello jelölőnégyzetet **elérhető Azure 3. fél kiszolgáló központi telepítése**.
2. Hello legördülő menüben válassza ki az hello szükséges server szoftver toouse a telepítés hello felhőben. Vegye figyelembe, ha Ön már meg van adva egy korábbi kiszolgáló toouse típusú, korlátozott toochoosing csak olyan felhőalapú kiszolgálót, amely a hello fogja, hogy a kiszolgáló típusa azonos termékcsalád. Ha azonban nem adta meg a kiszolgáló típusa, választhat, amelyek jelenleg az Azure-on hello kiszolgálók egyikén hello kiszolgálótípus automatikusan be lesz az Ön.
3. Kattintson a **OK** toosave a módosításokat.

Hello használata **egyéni letöltéssel telepítés** lehetőséget:

1. Győződjön meg arról, hogy a kiszolgáló típusa szerint az előző lépésekben toohello már ki van-e. Ez alapján hello beépülő modul hogyan toodeploy hello kiszolgálón, mert az egyéni le kell hello ugyanazon család a kijelölt kiszolgáló típusa.
2. Nevű hello jelölőnégyzetet **egyéni letöltéssel telepítés**.
    Ha toodownload az Azure-tárfiókot, válassza hello tárfiókot hello **tárfiók** legördülő listából válassza ki (hello kattintva **fiókok** hivatkozás toomodify a hello lista), ami részlegesen kitölti hello **URL-cím** mezőben, és ezután adja meg a fennmaradó hello URL-cím tooyour server részének hello letöltése a ZIP (az Azure-tárolót, hello URL-címben a blob nevének kisbetűnek kell lennie). Ha nem szeretné, hogy az Azure storage toouse, válassza ki a **(nincs)** a hello **tárfiók** legördülő listában, és adja meg a hello URL-cím tooyour kiszolgáló letöltési ZIP hello **URL-cím** mező. hello ZIP tartalmazná egy gyermekmappába képviselő az alkalmazás server telepítési könyvtárába. Például egy zip használatakor az Apache Tomcat 7.0.35 belül hello zip lenne hello gyermek mappa képviselő hello telepítési könyvtárat, például a **apache-tomcat-7.0.35**. 
3. Adja meg a hello hello kezdőkönyvtár környezeti változó értékét. Az alapértelmezés szerint toohello értékét a helyi kiszolgáló, ha vannak ilyenek, de egy másik értéket adhat meg, ha a felhő alkalmazáskiszolgáló eltér a helyi kiszolgáló. Azonban meg kell toomake meg arról, hogy a felhő alkalmazáskiszolgáló hello azonos termékcsalád korábban kiválasztott hello server típusként.
    A felhő application server zip a jövőbeli hello frissítésekor hello kezdőkönyvtár beállítást, vagy azt újra felel meg a helyi beállítás (Ha a helyi kiszolgáló túl módosította) toohave manuálisan módosíthatja.
4. Kattintson a **OK** toosave a módosításokat.

az alapul szolgáló logikai, amelynek elemek jelennek meg hello hello **Server** hello lapján **kiszolgálókonfiguráció** tulajdonságok oldalán testre szabható. Ez az egy speciális funkció, ha igényeinek túlnyúlnak hello alapértelmezett értékeket, vagy ha azt szeretné tooadd más kiszolgálók szükség lehet. toocustomize hello logikája, hello **Server** párbeszédpanel, kattintson hello **Testreszabás** hivatkozásra. Ez bezárul hello **kiszolgálókonfiguráció** tulajdonságlapját, és a nyitott hello **componentsets.xml** fájlt az eclipse-ben, amely ezt követően módosíthatja szükséges tooextend hello kiszolgáló konfigurációs sablont. Dokumentációja **componentsets.xml** hello megtalálható **componentsets.xml** magában.

Hello használata **központi telepítése a helyi kiszolgáló (automatikus – feltöltés toocloud tároló)** lehetőséget:

1. Nevű hello jelölőnégyzetet **központi telepítése a helyi kiszolgáló (automatikus – feltöltés toocloud tároló)**.
2. Hello segítségével **tárfiók** legördülő listában válassza **(automatikus)**. Ha megad **(automatikus)** itt hello eszközkészlet használandó Eclipse hello ugyanazt a tárfiókot a kiszolgáló hello, egyet a hello az üzembe helyezéshez választott **tooAzure közzététele** párbeszédpanel.
3. Kattintson a **OK** toosave a módosításokat.

Válassza ki a kiszolgáló telepítési elérési útvonalat a számítógépen a hello **helyi kiszolgáló elérési útja** szövegmező bármelyik hello a következő feltételek teljesülése esetén:

* Érdemes tootest a központi telepítés (csak tooWindows vonatkozik) hello-emulátoron.
* Érdemes toodeploy a helyileg telepített kiszolgálói toohello felhő.
* Szeretné, hogy toouse, ebben az esetben a saját hello felhő egyéni kiszolgáló letöltési csomagját, gondoskodjon arról is hello **központi telepítése a helyi kiszolgáló (automatikus – feltöltés toocloud tároló)** beállítást fent.

Ha nincs hello megelőző beállítások alkalmazása tooyour helyzet, hello helyi kiszolgáló beállítás nem kötelező.

### <a name="applications-configuration"></a>Alkalmazások konfigurálása
hello az alábbiakban látható egy példa hogyan megadhatja az alkalmazás.

![][ic719512]

Kattintson a **Hozzáadás** tooadd egy másik alkalmazás vagy **eltávolítása** tooremove kérelmet. Hatékonysága érdekében toouse egy letölthető alkalmazás hello forrás toohello felhő telepítésekor, használja hello [összetevők tulajdonságok](#components_properties) toospecify URL-cím, a tárfiók, stb. 

2014. április kiadás hello kezdve, az alkalmazások automatikusan feltöltése hello be ugyanazt a tárfiókot (alatt hello **eclipsedeploy** tároló), a telepítésre kiválasztott egy hello. a telepítés hello ügyfélindítási logikája a lépést, amely először letölti a ezeket az alkalmazásokat a tárolási fiók tartalmazza. Ez azt jelenti, hogy előfordulhat, hogy az alkalmazások frissítése a környezetben anélkül, hogy toorebuild, és telepítse újra a teljes csomag hello, közvetlenül az adott (például használata hello Azure-portál) storage-fiók hello alkalmazás újabb verzióját manuálisan feltöltésével , hello eszközkészlet által eredetileg feltöltött hello WAR fájlok felülírása van. Ezt követően csak kezdeményeznek hello újrahasznosítása összes szerepkör logikailemez Azure felügyeleti portál használatával újra, vagy a parancssori segédeszközök keresztül. (Kiváltó szerepkör újrahasznosítási közvetlenül a hello Eclipse eszközkészlet belül jelenleg nem támogatott.)

### <a name="notes-about-server-configuration"></a>Kiszolgáló konfigurációjával kapcsolatos megjegyzések
Hello által végrehajtott módosítások **kiszolgálókonfiguráció** tulajdonságlapján megjelennek hello `<component>` hello package.xml fájljának elemeit.

Amikor az hello **automatikus feltöltése...**  vagy **le telepítése...**  beállítások hello JDK vagy alkalmazáskiszolgáló, és Ön által létrehozott hello felhő (nem hello compute emulator), és a csatlakoztatott toohello hálózati, azt tapasztalhatja, például hello következő hello a konzol kimeneti, az üzenetek build, telepítsenek hello Jelentéskészítő hello letöltési rendelkezésre állását ellenőrzi:

`[windowsazurepackage] Verifying blob availability (https://example.blob.core.windows.net/temp/tomcat6.zip)...` 

Ha bejelölte a hello **le telepítése...**  beállítás, figyelmeztetés a következő hello is látható, de hello build továbbra is:

`[windowsazurepackage] warning: Failed tooconfirm blob availability! Make sure hello URL and/or hello access key is correct (https://example.blob.core.windows.net/temp/tomcat6.zip).` 

Ez a figyelmeztetés hello csak jelzi, hogy a letöltési tartozó rendelkezésre állási hello még nem lett ellenőrizve. Ezért ha a központi telepítés hello felhő valamilyen okból nem sikerül, ellenőrizze toosee Ha ezt a figyelmeztetést kapott.

Ha azt szeretné, toodisable hello letöltés ellenőrzése (például, ha úgy érzi, hogy azt feleslegesen lelassítja hello build), beállíthatja hello `verifydownloads` túl attribútum`false` a hello `<windowsazurepackage>` package.xml eleme: 

`<windowsazurepackage verifydownloads="false" ...>` 

Ha a kiválasztott hello **automatikus feltöltése...**  lehetőséget, majd a konzolablakban hello látni fogja build üzenetek hello feltöltés 5 másodpercentként, amikor szükség egy feltöltés előrehaladását jelentéskészítési hello.

<a name="ssl_offloading_properties"></a> 

### <a name="ssl-offloading-properties"></a>SSL-feladatkiszervezést tulajdonságai
Nyissa meg a helyi menü hello hello szerepkörhöz tartozó Eclipse Project Explorer ablaktáblában, kattintson **Azure**, és kattintson a **SSL-Feladatkiszervezést**. 

![][ic719481]

Ezen a párbeszédpanelen belül engedélyezheti az SSL-feladatkiszervezést, így tooeasily engedélyezése nélkül tooconfigure SSL a Java-alkalmazáskiszolgáló az Azure, a Java-telepítésben támogatja Hypertext Transfer Protocol Secure (HTTPS). További információkért lásd: [SSL-Feladatkiszervezést] [ SSL Offloading] és [hogyan SSL-Feladatkiszervezést tooUse][How tooUse SSL Offloading].

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Azure-projekt tulajdonságai][Azure Project Properties]

[Az Azure Storage-fiók listája][Azure Storage Account List]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

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
[How tooUse Co-located Caching]: http://go.microsoft.com/fwlink/?LinkID=699542
[How tooUse SSL Offloading]: http://go.microsoft.com/fwlink/?LinkID=699545
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
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
