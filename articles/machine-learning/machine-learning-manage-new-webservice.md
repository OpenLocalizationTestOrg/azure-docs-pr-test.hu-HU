---
title: "aaaUse hello Azure Machine Learning webszolgáltatások portál |} Microsoft Docs"
description: "Hozzáférés tooAzure Machine Learning munkaterületek, kezelése és szolgáltatások telepítésére és kezelésére ML API webkiszolgáló"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: jhubbard
editor: cgronlun
ms.assetid: b62cf2ca-dd2a-4a83-bb54-469f948fb026
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: v-donglo
ms.openlocfilehash: 04b49027fc0ab227382b320310088bb66aafacc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-service-using-hello-azure-machine-learning-web-services-portal"></a>Kezelheti egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portál használatával
A Machine Learning új és a klasszikus webes szolgáltatások hello Microsoft Azure Machine Learning webszolgáltatások portál használatával kezelheti. Klasszikus webszolgáltatások és új webes szolgáltatások különböző alapul szolgáló technológiák alapján, mert némileg eltérő felügyeleti képességek állnak a rendelkezésére az egyes őket.

Hello Machine Learning webszolgáltatások portál a következőket teheti:

* Hello webszolgáltatás használatának figyelése
* Hello leírás beállításához frissítse hello hello webes szolgáltatás (csak új), frissítse a tárolási fiók kulcs (csak új), engedélyezése, naplózása és adatátvitel engedélyezése vagy letiltása a minta.
* Hello webszolgáltatás törlése.
* Létrehozás, törlés vagy frissítés számlázási tervek (csak új).
* Hozzáadni és törölni végpontjai (csak klasszikus)

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="permissions-toomanage-new-resources-manager-based-web-services"></a>Új erőforrás-kezelő engedélyei toomanage alapú webszolgáltatások

Új webes szolgáltatások, Azure-erőforrások vannak telepítve. Ilyen kell hello megfelelő engedélyek toodeploy rendelkezik, és új webes szolgáltatások kezelése.  toodeploy vagy új webszolgáltatások, akkor hozzá kell rendelni egy munkatárs kezelése vagy a rendszergazda szerepkörhöz a hello előfizetés toowhich hello webszolgáltatás telepítve van. Ha meghívni egy másik felhasználó tooa gépi tanulási munkaterület, hozzá kell rendelnie őket tooa közreműködő vagy a rendszergazda szerepkör hello előfizetés ahhoz, azok központilag telepíthetne vagy kezelhetne webszolgáltatások. 

Ha hello felhasználó nem rendelkezik hello javítsa ki az engedélyek tooaccess erőforrások hello Azure Machine Learning webszolgáltatások portálon, kapnak egy webszolgáltatás-bővítmény toodeploy közben a következő hiba hello:

*A webes szolgáltatás telepítése sikertelen volt. Ez a fiók nem rendelkezik elegendő hozzáférési toohello Azure-előfizetéssel, amely tartalmazza a hello munkaterület. Ahhoz, egy webszolgáltatás tooAzure, ugyanazt a fiókot kell hello toodeploy meghívott toohello munkaterület, és adott hozzáférés toohello Azure-előfizetéssel kell hello munkaterület tartalmazza.*

A munkaterület létrehozása további információkért lásd: [létrehozása és a megosztáshoz egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).

A hozzáférési engedélyek beállítása további információkért lásd: [access-hozzárendelések megtekintése a felhasználók és csoportok az Azure portál – nyilvános előzetes hello](../active-directory/role-based-access-control-manage-assignments.md).


## <a name="manage-new-web-services"></a>Új webes szolgáltatások kezelése
toomanage az új webes szolgáltatások:

1. Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portálra a Microsoft Azure-fiókjával - társított hello fiók használata hello Azure-előfizetés.
2. Hello menüjének **webszolgáltatások**.

Ez megjeleníti az előfizetéshez tartozó telepített webes szolgáltatás. 

toomanage egy webszolgáltatás-bővítmény, kattintson a webes szolgáltatás. Hello webszolgáltatások oldalról a következőket teheti:

* Kattintson a hello web service toomanage azt.
* Kattintson a hello web service tooupdate számlázási megtervezése hello azt.
* Egy webszolgáltatás-bővítmény törlése.
* Másolja át a webes szolgáltatás, és telepítse azt tooanother régióban.

Ha egy webszolgáltatás-bővítmény gombra kattint, hello szolgáltatás gyors üzembe helyezés nyílik meg. gyors üzembe helyezés szolgáltatás hello weblap rendelkezik, amelyek lehetővé teszik toomanage két menüpont a webszolgáltatás:

* **IRÁNYÍTÓPULT** -tooview webes szolgáltatás használata lehetővé teszi.
* **KONFIGURÁLÁS** – lehetővé teszi tooadd leíró szöveg, frissítési hello kulcs hello tárfiók társított hello webes szolgáltatás, és engedélyezze vagy tiltsa le a mintaadatokat.

### <a name="monitoring-how-hello-web-service-is-being-used"></a>Hello webszolgáltatás használatának figyelése
Kattintson a hello **IRÁNYÍTÓPULT** fülre.

A webes szolgáltatás teljes használatának hello irányítópult megtekintheti egy meghatározott időtartamra vonatkozóan. Hello időszak tooview hello hello használati diagramok jobb felső sarkában található hello időszak legördülő menüből kiválaszthatja. hello irányítópult a következő információ hello jeleníti meg:

* **Kérelmek keresztül idő** kérések száma hello lépés grafikonja kijelölt időszakban hello jeleníti meg. Segíthet megállapítani, hogy a használati igényeiben jelentkező problémát.
* **Kérés-válasz kérelmek** hello teljes hello szolgáltatás által fogadott hello kijelölt időszakra vonatkozóan, és azok számát nem sikerült a kérés-válasz hívások száma.
* **Átlagos kérés-válasz számítási ideje** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.
* **Kötegelt kérelmek** hello az összes száma kötegelt kérelmekben hello szolgáltatás kapott kijelölt időszakban hello keresztül, és azok számát nem sikerült.
* **Átlagos késés feladat** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.
* **Hibák** hello bekövetkezett hibák összesített számát jeleníti meg a hívások toohello webes szolgáltatás.
* **Szolgáltatási költségei** hello hello szolgáltatáshoz tartozó számlázási csomag díjai hello jeleníti meg.

### <a name="configuring-hello-web-service"></a>Hello webszolgáltatás konfigurálása
Kattintson a hello **KONFIGURÁLÁSA** menüjét.

A következő tulajdonságok hello frissítheti:

* **Leírás** lehetővé teszi a tooenter hello webszolgáltatás leírását.
* **Cím** lehetővé teszi a tooenter hello webszolgáltatás címét
* **Kulcsok** toorotate lehetővé teszi az elsődleges és másodlagos API-kulcsokat.
* **Tárfiók kulcsa** lehetővé teszi a tooupdate hello kulcsának hello társított hello webes szolgáltatás módosításait. 
* **Engedélyezze a mintaadatok** tooprovide mintaadatok használható tootest hello kérés-válasz szolgáltatás lehetővé teszi. Ha a Machine Learning Studióban létrehozott hello webszolgáltatás, hello mintaadatok forrása hello adatok a használt tootrain a modell. Ha programozottan létrehozott hello szolgáltatást, hello adatok hello JSON csomag részeként megadott hello példaadatokat származik.

### <a name="managing-billing-plans"></a>Számlázási tervek kezelése
Kattintson a hello **tervek** menüpont hello web services gyors üzembe helyezés oldalról. Tervezze meg adott Web service toomanage társított hello terv is kattinthat.

* **Új** lehetővé teszi a toocreate egy új tervet.
* **Hozzáadása terv példány** lehetővé teszi a túl "horizontális felskálázás" egy meglévő terv tooadd kapacitás.
* **Frissítés/alacsonyabb szintre való visszalépést** lehetővé teszi a túl "vertikális felskálázás" egy meglévő terv tooadd kapacitás.
* **Törlés** lehetővé teszi a toodelete egy tervet.

A terv tooview kattintson az irányítópulton. hello irányítópult lehetővé teszi az pillanatképét vagy a terv használata egy kijelölt időtartamra vonatkozóan. tooselect hello idő időszak tooview, kattintson a hello **időszak** legördülő irányítópult hello felső sarokban. 

hello terv irányítópultról hello a következő információkat:

* **Terv leírása** hello költségeket és a kapacitás hello terv társított információit jeleníti meg.
* **Tervezze meg használati** tranzakciók és számítási órák, amelyek elleni hello terv felszámítani hello számát mutatja.
* **Webszolgáltatások** Web services által használt a terv hello számát mutatja.
* **Webes szolgáltatás által hívások felső** hello első négy webalapú alkalmazásokhoz, a rendszer terhére hello terv hívások létesített jeleníti meg.
* **Webszolgáltatások TOP számítási kezdéssel alapján** hello első négy Web services által használt számítási erőforrásokat, amelyek van szó, hello terv alapján jeleníti meg.

## <a name="manage-classic-web-services"></a>Klasszikus webszolgáltatások kezelése
> [!NOTE]
> hello ebben a szakaszban bemutatott eljárások a megfelelő toomanaging klasszikus webes szolgáltatások hello Azure Machine Learning webszolgáltatások portálon keresztül. Klasszikus webes kezelésével kapcsolatos tudnivalók a szolgáltatások kezelésében a Machine Learning Studio hello és hello a klasszikus Azure portál, lásd a [kezelése az Azure Machine Learning-munkaterület](machine-learning-manage-workspace.md).
> 
> 

toomanage a klasszikus webszolgáltatások:

1. Jelentkezzen be toohello [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/quickstart) portálra a Microsoft Azure-fiókjával - társított hello fiók használata hello Azure-előfizetés.
2. Hello menüjének **klasszikus webszolgáltatások**.

Kattintson egy klasszikus webszolgáltatás toomanage **klasszikus webszolgáltatások**. Hello klasszikus webszolgáltatások oldalról a következőket teheti:

* Kattintson a hello webszolgáltatási tooview hello kapcsolódó végpontok.
* Egy webszolgáltatás-bővítmény törlése.

Ha Ön kezeli a klasszikus webes szolgáltatás, kezelheti egyes hello végpontok külön-külön. Egy webszolgáltatás-bővítmény hello webszolgáltatások lapon gombra kattint, hello szolgáltatáshoz tartozó végpontok hello listáját nyitja meg. 

Az oldalon hello klasszikus webszolgáltatás végpont hozzáadhat és hello szolgáltatás végpontok törlése. Végpontok hozzáadásával kapcsolatos további információkért lásd: [végpontok létrehozása](machine-learning-create-endpoint.md).

Kattintson az egyik hello végpontok tooopen hello webes szolgáltatás gyors üzembe helyezés lap. Az oldalon hello gyors üzembe helyezés két módon menü, amelyek lehetővé teszik toomanage a webszolgáltatás:

* **IRÁNYÍTÓPULT** -tooview webes szolgáltatás használata lehetővé teszi.
* **KONFIGURÁLÁS** -lehetővé teszi a tooadd leíró szöveg, kapcsolja be a hiba bejelentkezés és kijelentkezés, frissítési hello kulcs hello tárfiók társított hello webes szolgáltatás, és engedélyezése, és tiltsa le a mintaadatokat.

### <a name="monitoring-how-hello-web-service-is-being-used"></a>Hello webszolgáltatás használatának figyelése
Kattintson a hello **IRÁNYÍTÓPULT** fülre.

A webes szolgáltatás teljes használatának hello irányítópult megtekintheti egy meghatározott időtartamra vonatkozóan. Hello időszak tooview hello hello használati diagramok jobb felső sarkában található hello időszak legördülő menüből kiválaszthatja. hello irányítópult a következő információ hello jeleníti meg:

* **Kérelmek keresztül idő** kérések száma hello lépés grafikonja kijelölt időszakban hello jeleníti meg. Segíthet megállapítani, hogy a használati igényeiben jelentkező problémát.
* **Kérés-válasz kérelmek** hello teljes hello szolgáltatás által fogadott hello kijelölt időszakra vonatkozóan, és azok számát nem sikerült a kérés-válasz hívások száma.
* **Átlagos kérés-válasz számítási ideje** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.
* **Kötegelt kérelmek** hello az összes száma kötegelt kérelmekben hello szolgáltatás kapott kijelölt időszakban hello keresztül, és azok számát nem sikerült.
* **Átlagos késés feladat** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.
* **Hibák** hello bekövetkezett hibák összesített számát jeleníti meg a hívások toohello webes szolgáltatás.
* **Szolgáltatási költségei** hello hello szolgáltatáshoz tartozó számlázási csomag díjai hello jeleníti meg.

### <a name="configuring-hello-web-service"></a>Hello webszolgáltatás konfigurálása
Kattintson a hello **KONFIGURÁLÁSA** menüjét.

A következő tulajdonságok hello frissítheti:

* **Leírás** lehetővé teszi a tooenter hello webszolgáltatás leírását. Leírás megadása kötelező.
* **Naplózás** lehetővé teszi a hello végponton hibanaplózást tooenable vagy letiltása. A naplózást további információkért lásd: Enable [Machine Learning webszolgáltatások naplózása](machine-learning-web-services-logging.md).
* **Engedélyezze a mintaadatok** tooprovide mintaadatok használható tootest hello kérés-válasz szolgáltatás lehetővé teszi. Ha a Machine Learning Studióban létrehozott hello webszolgáltatás, hello mintaadatok forrása hello adatok a használt tootrain a modell. Ha programozottan létrehozott hello szolgáltatást, hello adatok hello JSON csomag részeként megadott hello példaadatokat származik.

## <a name="grant-or-suspend-access-tooweb-services-for-users-in-hello-portal"></a>Adja meg, vagy a hozzáférés tooWeb szolgáltatások felhasználók felfüggesztése hello portálon
Hello klasszikus Azure portál használatával, engedélyezi vagy megtagadja toospecific felhasználók.

### <a name="access-for-users-of-new-web-services"></a>Hozzáférés a felhasználók új webes szolgáltatások
tooenable más felhasználók toowork hello Azure Machine Learning webszolgáltatások portálon a webszolgáltatással, hozzá kell őket, ko-rendszergazdák az Azure-előfizetése.

Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) használatával a Microsoft Azure fiók – hello Azure-előfizetéssel társított hello fiókot használja.

1. Hello navigációs ablaktábláján kattintson **beállítások**, majd kattintson a **rendszergazdák**.
2. Hello ablak hello alul kattintson **Hozzáadás**. 
3. Hello A közös rendszergazda hozzáadása párbeszédpanelen, a típus hello hello felvenni kívánt személy e-mail tooadd közös rendszergazdaként, és válassza ki, hogy szeretné-e hello társadminisztrátoraként tooaccess hello előfizetés.
4. Kattintson a **Save** (Mentés) gombra.

### <a name="access-for-users-of-classic-web-services"></a>Klasszikus webszolgáltatások felhasználók hozzáférését
a munkaterület toomanage:

Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) használatával a Microsoft Azure fiók – hello Azure-előfizetéssel társított hello fiókot használja.

1. Kattintson a Microsoft Azure szolgáltatások panel hello **MACHINE LEARNING**.
2. Kattintson a kívánt toomanage hello munkaterületen.
3. Kattintson a hello **KONFIGURÁLÁSA** fülre.

Hello konfigurációs lapján kattintva is függesztheti hozzáférés toohello Machine Learning-munkaterület **MEGTAGADÁS**. Felhasználók többé nem tud tooopen hello munkaterület a Machine Learning Studióban. toorestore hozzáférés, kattintson a **engedélyezése**.

toospecific felhasználók:

további fiókok toomanage rendelkező hozzáférés toohello munkaterület a Machine Learning Studióban, kattintson a **bejelentkezési tooML Studio** a hello **IRÁNYÍTÓPULT** fülre. Hello munkaterületen megnyílik a Machine Learning Studióban. Itt kattintson a hello **beállítások** fülre, majd **felhasználók**. Kattinthat **több felhasználók MEGHÍVÁSA** toogive felhasználók hozzáférését toohello munkaterületet, vagy válasszon ki egy felhasználót, majd kattintson **eltávolítása**.

> [!NOTE]
> Hello **bejelentkezési tooML Studio** a hivatkozás megnyitja a Machine Learning Studio jelenleg bejelentkezett Microsoft Account hello segítségével. hello Azure klasszikus portál toocreate munkaterület toohello toosign használt Microsoft Account automatikusan nincs engedélye tooopen munkaterületet. a munkaterület tooopen be kell jelentkeznie a Microsoft hello munkaterület hello tulajdonosaként megadott Account toohello, vagy tooreceive hello tulajdonos toojoin hello munkaterületről meghívót van szüksége.
> 
> 

