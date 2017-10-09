---
title: "aaaManage adatbázis-szerepkörök és a felhasználók az Azure Analysis Services |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage adatbázis-szerepkörök és a felhasználók az Azure Analysis Services kiszolgálón."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 2ad069a6bcce11bc43347625cb32ec400d48af18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-database-roles-and-users"></a>Adatbázis-szerepkörök és a felhasználók kezelése

Hello modell adatbázis szintjén a minden felhasználó tooa szerepkörhöz kell tartoznia. Szerepkörök definiálása hello modellű adatbázisnál adott engedéllyel rendelkező felhasználók. Minden felhasználót vagy biztonsági csoportot hozzáadni tooa szerepkörnek rendelkeznie kell egy fiókot az Azure AD-bérlő hello hello kiszolgálóként ugyanahhoz az előfizetéshez.

Szerepkörök definiálása hogyan hello eszköz, amellyel másik attól függően, de hello hatása van hello azonos.

Szerepkör-engedélyek a következők:
*  **Rendszergazda** -felhasználók hello adatbázis teljes körű engedélyekkel rendelkezik. Adatbázis-szerepkörök rendszergazdai jogosultságokkal rendelkező rendszergazdáinak eltérnek.
*  **Folyamat** – a felhasználók kapcsolódhatnak tooand hello adatbázis folyamat műveleteket, és elemezni a modell adatbázis adatai.
*  **Olvasási** -felhasználók használhatják az ügyfél alkalmazás tooconnect tooand modell adatbázis elemzéséhez.

Amikor létrehoz egy táblázatos modell projektet, szerepkörök létrehozása és felhasználók vagy csoportok toothose szerepkörök hozzáadása a szerepkörkezelővel SSDT-ben. Amikor telepített tooa kiszolgáló, az SSMS, [Analysis Services PowerShell-parancsmagok](https://msdn.microsoft.com/library/hh758425.aspx), vagy [táblázatos modell Scripting Language](https://msdn.microsoft.com/library/mt614797.aspx) (TMSL) tooadd, vagy távolítsa el a szerepkörök és a felhasználói tagjai.

## <a name="tooadd-or-manage-roles-and-users-in-ssdt"></a>tooadd vagy szerepkörök és az SSDT felhasználók kezelése  
  
1.  Az SSDT > **táblázatos modell Explorer**, kattintson a jobb gombbal **szerepkörök**.  
  
2.  A **Szerepkörkezelőben** kattintson az **Új** lehetőségre.  
  
3.  Írja be a hello szerepkör nevét.  
  
     Alapértelmezés szerint hello alapértelmezett szerepkör nevét hello Növekményesen számozott minden új szerepkörhöz. Javasoljuk, adjon meg egy nevet, amely egyértelműen azonosítja a hello tag típusa, például a pénzügyi menedzserek vagy az emberi erőforrások szakemberei.  
  
4.  Válasszon egyet az alábbi engedélyek használata hello:  
  
    |Engedély|Leírás|  
    |----------------|-----------------|  
    |**Egyik sem**|A tagok hello modellsémát nem módosítható, és nem tudja lekérdezni az adatokat.|  
    |**Olvasás**|Tagok lekérdezheti adatokat (sorszűrőket alapján), de nem módosíthatja a hello modellsémát.|  
    |**Olvasás és a folyamat**|Tagok lekérdezheti adatokat (a sorszintű szűrők) és futtatási folyamat és a folyamat minden műveleteket, de nem módosítható a hello modellsémát.|  
    |**Folyamat**|Tagok folyamat és a folyamat minden műveletek is futtatható. Hello modellsémát nem módosítható és nem tudja lekérdezni az adatokat.|  
    |**Rendszergazda**|Tagok hello modellsémát módosíthatja, és minden adat lekérdezése.|   
  
5.  Ha hello szerepkör létrehozása rendelkezik olvasási vagy olvasási és a folyamat engedélyt adhat hozzá sorszűrőket DAX-képlet használatával. Kattintson a hello **sorszűrőket** lapra, majd egy táblázatban, majd kattintson a hello **DAX-szűrő** mezőben, és írja be egy DAX-képletet.
  
6.  Kattintson a **tagok** > **vegye fel a külső**.  
  
8.  A **külső tag hozzáadása**, adjon meg felhasználókat vagy csoportokat az Azure AD-bérlőben e-mail címet. Kattintson az OK gombra, és zárja be a szerepkör-kezelőt, szerepkörök és a szerepkör tagjai jelennek meg a táblázatos modell Explorer. 
 
     ![Szerepkörök és a táblázatos modell Explorer felhasználók](./media/analysis-services-database-users/aas-roles-tmexplorer.png)

9. Tooyour Azure Analysis Services-kiszolgáló telepítése.


## <a name="tooadd-or-manage-roles-and-users-in-ssms"></a>tooadd vagy szerepkörök és a szolgáltatáshoz az ssms felhasználók kezelése
szerepkörök és a felhasználók tooa tooadd telepített modellű adatbázisnál, kiszolgálói rendszergazdaként vagy a rendszergazdai jogosultságokkal rendelkező adatbázis-szerepkör már csatlakoztatott toohello kiszolgálón kell lennie.

1. Az objektum Exporer, kattintson a jobb gombbal **szerepkörök** > **új szerepkör**.

2. A **szerepkör létrehozása**, írja be a szerepkör nevét és leírását.

3. Jelöljön ki egy engedélyt.
   |Engedély|Leírás|  
   |----------------|-----------------|  
   |**Teljes hozzáférés (rendszergazda)**|Tagjai módosíthatják hello modellsémát, feldolgozásához, és lekérdezheti az összes adatot.| 
   |**Folyamat adatbázis**|Tagok folyamat és a folyamat minden műveletek is futtatható. Hello modellsémát nem módosítható és nem tudja lekérdezni az adatokat.|  
   |**Olvasás**|Tagok lekérdezheti adatokat (sorszűrőket alapján), de nem módosíthatja a hello modellsémát.|  
  
4. Kattintson a **tagsági**, majd írjon be egy felhasználót vagy csoportot az Azure AD bérlőhöz e-mail cím alapján.

     ![Felhasználó hozzáadása](./media/analysis-services-database-users/aas-roles-adduser-ssms.png)

5. Ha hello szerepkör létrehozásakor olvasási engedéllyel rendelkezik, egy DAX-képlet használatával adhat hozzá a sor szűrők. Kattintson a **sorszűrőket**, válasszon ki egy táblát, és írja be egy DAX-képlet hello **DAX-szűrő** mező. 

## <a name="tooadd-roles-and-users-by-using-a-tmsl-script"></a>tooadd szerepkörök és a felhasználók TMSL parancsfájl használatával
Egy TMSL parancsprogrammal hello XMLA ablakban szolgáltatáshoz az ssms vagy a PowerShell használatával. Használjon hello [CreateOrReplace](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/createorreplace-command-tmsl) parancs és hello [szerepkörök](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-objects/roles-object-tmsl) objektum.

**Mintaparancsfájl TMSL**

Ez a példa egy B2B külső felhasználó vagy csoport kerülnek toohello elemző szerepkör hello SalesBI adatbázis olvasási engedélyekkel. Mindkét hello külső felhasználó, és az Azure AD ugyanannak a bérlőnek kell lennie.

```
{
  "createOrReplace": {
    "object": {
      "database": "SalesBI",
      "role": "Analyst"
    },
    "role": {
      "name": "Users",
      "description": "All allowed users tooquery hello model",
      "modelPermission": "read",
      "members": [
        {
          "memberName": "user1@contoso.com",
          "identityProvider": "AzureAD"
        },
        {
          "memberName": "group1@adventureworks.com",
          "identityProvider": "AzureAD"
        }
      ]
    }
  }
}
```

## <a name="tooadd-roles-and-users-by-using-powershell"></a>tooadd szerepkörök és a felhasználók a PowerShell használatával
Hello [SqlServer](https://msdn.microsoft.com/library/hh758425.aspx) modul adja meg a tevékenység-specifikus adatbázis felügyeleti parancsmagok és hello általános célú Invoke-ASCmd parancsmag, amely egy táblázatos modell Scripting Language (TMSL) lekérdezés vagy parancsfájl fogadja. a következő parancsmagok hello használt adatbázis-szerepkörök és felhasználók kezeléséhez.
  
|Parancsmag|Leírás|
|------------|-----------------| 
|[Adja hozzá RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Egy tag tooa adatbázis-szerepkör hozzáadása.| 
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Tag eltávolítása egy adatbázis-szerepkör.|   
|[Invoke-ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Hajtsa végre a TMSL parancsfájlt.|

## <a name="row-filters"></a>Sorszűrőket  
Sorszűrőket határozza meg, mely egy tábla sorainak lekérdezhetők, egy adott szerepkör tagjai. Sorszűrőket DAX-képlet használatával definiáltak a modellben minden táblához.  
  
Sorszűrőket csak rendelkező olvasási és olvasási szerepkörökhöz adható meg és folyamat-engedélyeket. Alapértelmezés szerint ha egy adott tábla nincs definiálva a Sorszűrő tagok lekérdezheti hello tábla összes sorát kivéve, ha egy másik táblából keresztszűrési vonatkozik.
  
 Sorszűrőket kell a DAX-képlet, amely ki kell értékelnie minden tooa igaz vagy hamis értéket, toodefine hello azon sorait, amelyek az adott adott szerepkör tagjai kérdezhetők le. Hello DAX-képlet nem szereplő sorok nem kérdezhető le. Például hello ügyfelek tábla a következő szűrők sorkifejezésen hello *ügyfelek [Ország] = "USA" =*, hello értékesítési szerepkör tagjai csak ügyfelek hello USA tekintheti meg.  
  
Sorszűrőket alkalmazása toohello megadott sorok és a kapcsolódó sorok. Ha egy táblában több kapcsolatnak is tagja, szűrők hello kapcsolat aktív biztonsági lesznek alkalmazva. Sorszűrőket az egyéb kapcsolódó tábla, például meghatározott sor szûrõkkel vannak átfedő:  
  
|Tábla|DAX-kifejezés|  
|-----------|--------------------|  
|Régió|= Régió [Ország] = "USA"|  
|ProductCategory|= ProductCategory [Name] = "Kerékpárt"|  
|Tranzakciók|= Tranzakciók [év] = 2016|  
  
 hello nettó hatása tagok sornyi adatot, ahol hello ügyfél hello USA, hello termékkategória kerékpárt, és hello év 2016 lekérdezheti. Felhasználók kívül hello USA tranzakciók, amelyek nincsenek kerékpárt vagy tranzakciók nem 2016 kivéve, ha egy másik szerepkör, amely engedélyt ad a tranzakciók nem tudja lekérdezni.
  
 Hello szűrővel *=FALSE()*, toodeny hozzáférés tooall sorainak teljes táblát.

## <a name="next-steps"></a>Következő lépések
  [Kiszolgáló-rendszergazdák kezelése](analysis-services-server-admins.md)   
  [A PowerShell segítségével az Azure Analysis Services kezelése](analysis-services-powershell.md)  
  [Táblázatos modell Scripting (TMSL) nyelvi referencia](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference)

