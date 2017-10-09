---
title: "a Machine Learning-munkaterület aaaManage |} Microsoft Docs"
description: "Hozzáférés tooAzure Machine Learning munkaterületek, kezelése és szolgáltatások telepítésére és kezelésére ML API webkiszolgáló"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 7bd378d82aa46f4340ca974c7dc5a612c089cdca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Azure Machine Learning-munkaterület kezelése

> [!NOTE]
> Webszolgáltatások hello Machine Learning webszolgáltatások portálon kezeléséről további információért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).
> 
> 

Gépi tanulás munkaterületeket, vagy hello Azure-portálon kezelheti, vagy a klasszikus Azure portálon hello.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-hello-azure-portal"></a>Hello Azure portál használata

toomanage munkaterületeinek hello Azure-portálon:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) Azure-előfizetéshez rendszergazdai fiók használatával.
2. Hello oldal hello tetején hello a keresési mezőbe, írja be a "számítógép tanulási munkaterületek", és válassza ki **Machine Learning munkaterületek**.
3. Kattintson a kívánt toomanage hello munkaterületen.

Ezenkívül toohello szabványos erőforrás-kezelési információkat és a rendelkezésre álló lehetőségeket, a következőket teheti:

- Nézet **tulajdonságok** - ezen a lapon látható hello munkaterületet, és az erőforrás-információkat, és módosíthatja hello előfizetés és az erőforráscsoport, a munkaterület kapcsolódnak.
- **Tárolási kulcsok újraszinkronizálásra** -hello munkaterület kulcsok toohello tárfiók tart fenn. Ha hello storage-fiók vált kulcsok, ezt követően kattinthat **kulcsok újraszinkronizálásra** toosynchronize hello kulcsok hello munkaterület.

a munkaterülethez tartozó toomanage hello webszolgáltatások hello Machine Learning webszolgáltatások portál használata. Lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md) kapcsolatos részletes információkért.

> [!NOTE]
> toodeploy vagy új webszolgáltatások, akkor hozzá kell rendelni egy munkatárs kezelése vagy a rendszergazda szerepkörhöz a hello előfizetés toowhich hello webszolgáltatás telepítve van. Ha meghívni egy másik felhasználó tooa gépi tanulási munkaterület, hozzá kell rendelnie őket tooa közreműködő vagy a rendszergazda szerepkör hello előfizetés ahhoz, azok központilag telepíthetne vagy kezelhetne webszolgáltatások. 
> 
>A hozzáférési engedélyek beállítása további információkért lásd: [access-hozzárendelések megtekintése a felhasználók és csoportok az Azure portál – nyilvános előzetes hello](../active-directory/role-based-access-control-manage-assignments.md).

## <a name="use-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello használata

Hello klasszikus Azure portál használatával, a Machine Learning-munkaterületekkel képes kezelni:

* Hello munkaterület használatának figyelése
* Hello munkaterület tooallow konfigurálása vagy megtagadja a hozzáférést
* Hello munkaterületen létrehozott webszolgáltatások kezelése
* Hello munkaterület törlése

Emellett a hello irányítópult fület az munkaterület használatának áttekintése és a munkaterület adatok gyors áttekintő biztosítja.  

> [!TIP]
> Az Azure Machine Learning Studio, a hello **WEBSZOLGÁLTATÁSOK** lapon adhat hozzá, frissítés, vagy egy Machine Learning webszolgáltatás-bővítmény törlése.
> 
> 

a klasszikus Azure portálon hello munkaterületeinek toomanage:

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) használatával a Microsoft Azure fiók – hello Azure-előfizetéssel társított hello fiókot használja.
2. Kattintson a Microsoft Azure szolgáltatások panel hello **MACHINE LEARNING**.
3. Kattintson a kívánt toomanage hello munkaterületen.

hello munkaterület lap rendelkezik a három lappal:

* **IRÁNYÍTÓPULT** -lehetővé teszi az tooview munkaterület használati és információ
* **KONFIGURÁLÁS** -lehetővé teszi a toomanage hozzáférés toohello munkaterület
* **WEBSZOLGÁLTATÁSOK** -lehetővé teszi a munkaterületről közzétett toomanage webszolgáltatások

### <a name="toomonitor-how-hello-workspace-is-being-used"></a>hello munkaterület használatának toomonitor
Kattintson a hello **IRÁNYÍTÓPULT** fülre.

Hello irányítópultról összesített használatát a munkaterület tekintheti meg és munkaterület adatok gyors áttekintő beolvasása.

* Hello **számítási** hello számítási erőforrásokat hello munkaterületen használja a diagram ábrázolja. Hello nézet toodisplay relatív vagy abszolút értékek módosíthatja, és módosíthatja a hello időkeretre hello diagram.
* **Használati áttekintése** hello munkaterületen használja az Azure storage jeleníti meg.
* **Gyors áttekintő** munkaterület információkat és hasznos hivatkozásokat összegzését tartalmazza.

> [!NOTE]
> Hello **bejelentkezési tooML Studio** a hivatkozás megnyitja a Machine Learning Studio jelenleg bejelentkezett Microsoft Account hello segítségével. hello Azure klasszikus portál toocreate munkaterület toohello toosign használt Microsoft Account automatikusan nincs engedélye tooopen munkaterületet. a munkaterület tooopen be kell jelentkeznie a Microsoft hello munkaterület hello tulajdonosaként megadott Account toohello, vagy tooreceive hello tulajdonos toojoin hello munkaterületről meghívót van szüksége.
> 
> 

### <a name="toogrant-or-suspend-access-for-users"></a>toogrant vagy felhasználók hozzáférésének felfüggesztése
Kattintson a hello **KONFIGURÁLÁSA** fülre.

Hello konfigurációs lapján a következőket teheti:

* Hozzáférés toohello Machine Learning-munkaterület felfüggesztése MEGTAGADÁS kattintva. Felhasználók többé nem tud tooopen hello munkaterület a Machine Learning Studióban. toorestore, kattintson az engedélyezés férhet hozzá.

további fiókok toomanage rendelkező hozzáférés toohello munkaterület a Machine Learning Studióban, kattintson a **bejelentkezési tooML Studio** a hello **IRÁNYÍTÓPULT** lapon (lásd: hello portáladatbázis előző Megjegyzés vonatkozó  **TooML Studio Sign-in**). Hello munkaterületen megnyílik a Machine Learning Studióban. Itt kattintson a hello **beállítások** fülre, majd **felhasználók**. Kattinthat **több felhasználók MEGHÍVÁSA** toogive felhasználók hozzáférését toohello munkaterületet, vagy válasszon ki egy felhasználót, majd kattintson **eltávolítása**.

### <a name="toomanage-web-services-in-this-workspace"></a>a munkaterület toomanage webszolgáltatások
Kattintson a hello **WEBSZOLGÁLTATÁSOK** fülre.

Ez megjeleníti a munkaterületről közzétett webes szolgáltatások listáját.
egy webszolgáltatás-bővítmény toomanage hello lista tooopen hello webszolgáltatás oldalát hello nevére kattint.

Egy webszolgáltatás-bővítmény definiált egy vagy több végpontot is rendelkezhet.

* További végpontok hozzáadása toohello "Alapértelmezett" végpont is meghatározhatja. tooadd hello végpont, kattintson a **végpontok kezelése** hello irányítópult tooopen hello Azure Machine Learning webszolgáltatások portal hello alján.
* toodelete (hello "Alapértelmezett" végpont nem törölhetők) végpont, jelölje be a hello végpont sor hello elején hello jelölőnégyzetet, majd kattintson **törlése**. Ezzel eltávolítja hello végpont hello webszolgáltatáshoz.
  
  > [!NOTE]
  > Ha egy alkalmazás hello végpont törlése a hello webszolgáltatási végpontot, hello alkalmazás kap egy hiba hello legközelebb megkísérli tooaccess hello szolgáltatást.
  > 
  > 

Kattintson egy webes szolgáltatás végpont tooopen hello nevére azt. 

A webes szolgáltatás teljes használatának hello irányítópult megtekintheti egy meghatározott időtartamra vonatkozóan. Hello időszak tooview hello hello használati diagramok jobb felső sarkában található hello időszak legördülő menüből kiválaszthatja. hello irányítópult a következő információ hello jeleníti meg:

* **Kérelmek keresztül idő** kérések száma hello lépés grafikonja kijelölt időszakban hello jeleníti meg. Segíthet megállapítani, hogy a használati igényeiben jelentkező problémát.
* **Kérés-válasz kérelmek** hello teljes hello szolgáltatás által fogadott hello kijelölt időszakra vonatkozóan, és azok számát nem sikerült a kérés-válasz hívások száma.
* **Átlagos kérés-válasz számítási ideje** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.
* **Kötegelt kérelmek** hello az összes száma kötegelt kérelmekben hello szolgáltatás kapott kijelölt időszakban hello keresztül, és azok számát nem sikerült.
* **Átlagos késés feladat** jeleníti meg, az átlagos hello idő szükséges tooexecute hello fogadott kérések.
* **Hibák** hello bekövetkezett hibák összesített számát jeleníti meg a hívások toohello webes szolgáltatás.
* **Szolgáltatási költségei** hello hello szolgáltatáshoz tartozó számlázási csomag díjai hello jeleníti meg.

Hello konfigurációs lapján, a következő tulajdonságai hello frissítheti:

* **Leírás** lehetővé teszi a tooenter hello webszolgáltatás leírását. Leírás megadása kötelező.
* **Naplózás** lehetővé teszi a hello végponton hibanaplózást tooenable vagy letiltása. A naplózást további információkért lásd: Enable [Machine Learning webszolgáltatások naplózása](machine-learning-web-services-logging.md).
* **Engedélyezze a mintaadatok** tooprovide mintaadatok használható tootest hello kérés-válasz szolgáltatás lehetővé teszi. Ha a Machine Learning Studióban létrehozott hello webszolgáltatás, hello mintaadatok forrása hello adatok a használt tootrain a modell. Ha programozottan létrehozott hello szolgáltatást, hello adatok hello JSON csomag részeként megadott hello példaadatokat származik.

