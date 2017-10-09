---
title: "a Mobile Services aaaAdding kapcsolódó szolgáltatások a Visual Studio használatával |} Microsoft Docs"
description: "Adja hozzá a Mobile Services hello Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen"
services: visual-studio-online
documentationcenter: na
author: mlhoop
manager: douge
editor: 
ms.assetid: 75c3cb93-88e1-476d-a416-f34caa3608e3
ms.service: visual-studio-online
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: mobile
ms.date: 12/16/2015
ms.author: mlearned
ms.openlocfilehash: c47b6fb63dc99fbc012e1c627c6c7e95249de7a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>A Mobile Services hozzáadása a Visual Studio kapcsolódó szolgáltatások használatával
A Visual Studio 2015-öt, tooAzure Mobile Services használatával hello kapcsolódhatnak **kapcsolódó szolgáltatás hozzáadása** párbeszédpanel. Bármely C# ügyfélalkalmazás, bármely JavaScript-alkalmazást, illetve a platformok közötti Cordova-alkalmazás lehet csatlakoztatni. Miután csatlakozott, létrehozása és adatok eléréséhez, hozzon létre egyéni API-k és ütemezett feladatok, vagy a leküldéses értesítések támogatását.  hello kapcsolódó szolgáltatások művelet hozzáadása megfelelő hivatkozások és a kapcsolat kódot. Is kihasználhatja a hitelesítés népszerű identitáskezelési rendszerek, például Azure AD számos beépített támogatása Facebook, a Twitter és a Microsoft Accounts.

## <a name="supported-project-types"></a>Támogatott projekttípusok
> [!NOTE]
> A Visual Studio 2015-öt Azure Mobile Services tooa univerzális Windows (Windows 10) projektek hello kapcsolódó szolgáltatások hozzáadása párbeszédpanelen hozzáadása nem támogatott. Azure Mobile Services használatával hello NuGet-Csomagkezelőt a projekthez hello megfelelő csomagok telepítésével is hozzáadhat.
> 
> 

Hello kapcsolódó szolgáltatások párbeszédpanelen tooconnect tooAzure Mobile Services a következő projekttípusok hello is használhatja.

* .NET Windows 8.1 Áruházbeli, telefon és univerzális alkalmazás projektek
* JavaScript Windows 8.1 Áruházbeli, telefon és univerzális alkalmazás projektek
* Az Apache Cordova Visual Studio eszközök használatával létrehozott projektek

## <a name="connect-tooazure-mobile-services-using-hello-add-connected-services-dialog"></a>Csatlakozás tooAzure Mobile Services használatával hello kapcsolódó szolgáltatások hozzáadása párbeszédpanel
1. Győződjön meg arról, hogy az Azure-fiók. Ha az Azure-fiók nem rendelkezik, akkor regisztrálhatnak az egy [ingyenes próbaverzió](http://go.microsoft.com/fwlink/?LinkId=518146).
2. Nyissa meg hello **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel megnyitásához.
   
   * .NET-alkalmazások esetén nyissa meg a projektet a Visual Studio felületén nyissa meg hello helyi menüjére hello **hivatkozások** csomópont a Megoldáskezelőben, majd válassza **kapcsolódó szolgáltatás hozzáadása**
     
        ![Csatlakozás a Mobile Service tooAzure](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)
   * Az Apache Cordova-alkalmazás projektet, nyissa meg a projektet a Visual Studio felületén nyissa meg hello helyi menüjére hello projektcsomópontra a Megoldáskezelőben, és válassza a **kapcsolódó szolgáltatás hozzáadása**.
3. A hello **kapcsolódó szolgáltatás hozzáadása** párbeszédpanelen válassza ki **Azure Mobile Services**, majd válassza a hello **konfigurálása** gomb. Az Azure felszólító toolog lehet, ha még nem tette.
   
    ![Egy Azure-mobilszolgáltatás hozzáadása](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)
4. A hello **Azure Mobile Services** párbeszédpanelen válasszon egy meglévő mobilszolgáltatás, ha rendelkezik ilyennel. Ha egy új Azure mobilszolgáltatás toocreate van szüksége, eljárással hello alatt toodo így. Egyéb esetben folytassa a következő lépés toohello.
   
    toocreate egy új mobilszolgáltatás-fiókot:
   
   1. Válassza ki a hello ** szolgáltatás létrehozása ** hivatkozást hello hello párbeszédpanel alsó részén.
       ![Új csatlakoztatott mobilszolgáltatás hozzáadása](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)
   2. A hello **Mobile szolgáltatás létrehozása** párbeszédpanelen választhat egy JavaScript háttérrendszer mobilszolgáltatást, vagy a .NET-háttérrendszer mobilszolgáltatás hello **futásidejű** legördülő listából. 
      
       ![A mobilszolgáltatás létrehozása](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)
      
       A JavaScript háttérrendszer szolgáltatás egyszerű és hatékony. Ha a JavaScript háttérrendszer mobilszolgáltatások létrehozását, hello kiszolgálóoldali JavaScript-kód hello felhőben van tárolva, de server parancsfájlok szerkesztheti a Server Explorer vagy hello Azure felügyeleti portálján. 
      
       A .NET-háttérrendszer mobil szolgáltatás lehetővé teszi az hello teljes hatékonyságát és rugalmasságát Web API és az Entity Framework. Ha egy olyan .NET háttérrendszer mobilszolgáltatások létrehozását, a projekt jön létre, és hozzáadott tooyour megoldás. 
   3. Válassza ki a hello **régió** ahol mobil hello szolgáltatást, és hello kiszolgálót adja meg egy felhasználónevet és jelszót.
   4. Miután minden szükséges hello információt megadott, válassza ki azt a hello **létrehozása** toocreate hello mobilszolgáltatás gombra.
   5. hello új mobilszolgáltatás meg kell jelennie a hello hello szolgáltatás listájában **Azure Mobile Services** párbeszédpanel megnyitásához. Válassza ki az új mobilszolgáltatás hello hello listában, és válassza a hello **Hozzáadás** gomb tooyour tooadd hello szolgáltatásprojektet.
5. Felülvizsgálati hello első lépések oldal, amely akkor jelenik meg, és megtudhatja, hogyan a projekthez lett módosítva. A böngészőben egy első lépések lap jelenik meg, amikor egy összekapcsolt szolgáltatás. Tekintse át javasolt következő lépések és példák hello, vagy váltson toohello Mi történt lap toosee, milyen hivatkozások kerültek tooyour projektet, és hogyan módosultak a kódot és konfigurációs fájljait.
6. Hello mintakódok útmutatóként használva elkezd kód tooaccess a mobilszolgáltatáshoz!

## <a name="how-your-project-is-modified"></a>A projekt módosítása hogyan
Hello projekttípus függ, hogy hogyan a Visual Studio módosítja a projekthez. C# ügyfélalkalmazások, lásd: [Mi történt – C# projektek](http://go.microsoft.com/fwlink/p/?LinkId=513119). JavaScript ügyfélalkalmazások, lásd: [Mi történt – a JavaScript-projektek](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova-alkalmazásokkal, lásd: [Mi történt – a Cordova-projektek](http://go.microsoft.com/fwlink/p/?LinkId=513116).

## <a name="next-steps"></a>Következő lépések
Kérdései vannak, és rátalálhat: 

* [MSDN fórumon: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)
* [Az Azure Mobile Services hello Microsoft Azure-csapat blogja:](https://azure.microsoft.com/blog/topics/mobile/)
* [Azure Mobile Services azure.microsoft.com:](https://azure.microsoft.com/services/mobile-services/)
* [Azure Mobile Services dokumentációját a azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)

