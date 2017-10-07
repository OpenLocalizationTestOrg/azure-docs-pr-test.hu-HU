---
title: "aaaAuthenticating és a Power BI Embedded engedélyezése"
description: "Authenticating and authorizing with Power BI Embedded (Hitelesítés és engedélyezés a Power BI Embedded használatával)"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 1c1369ea-7dfd-4b6e-978b-8f78908fd6f6
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: 483ca0499e8d03584e1151d3d278c0179d4b8fbe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticating-and-authorizing-with-power-bi-embedded"></a>Authenticating and authorizing with Power BI Embedded (Hitelesítés és engedélyezés a Power BI Embedded használatával)

használja a Power BI Embedded szolgáltatás hello **kulcsok** és **alkalmazási jogkivonatok** a hitelesítéshez és engedélyezéshez, explicit végfelhasználói hitelesítés helyett. Ebben a modellben az alkalmazás kezeli a hitelesítési és engedélyezési a végfelhasználók számára. Ha szükséges, az alkalmazás hoz létre, és arról, hogy a szolgáltatás toorender hello alkalmazási jogkivonatok hello kért jelentést küld. Ez a kialakítás nincs szükség az alkalmazás toouse Azure Active Directory felhasználói hitelesítési és engedélyezési, bár továbbra is.

## <a name="two-ways-tooauthenticate"></a>Két módon tooauthenticate

**Kulcs** -kulcsok az összes Power BI Embedded REST API-hívásokhoz használható. hello kulcsok hello található **Azure-portálon** kattintva **összes beállítás** , majd **hívóbetűk**. A kulcs mindig kezelje, mintha egy jelszót. Ezek a kulcsok engedélyek toomake REST API-k hívható meg egy adott munkaterület-csoport rendelkezik.

a kulcsot egy REST-hívást toouse hello engedélyezési fejléc a következő hozzáadása:            

    Authorization: AppKey {your key}

**Alkalmazási jogkivonatának** -alkalmazási jogkivonatok használt összes beágyazási kérelem. Úgy is korlátozott futtatni tervezett toobe ügyféloldali fontosságúak tooa egyetlen jelentés és az ajánlott eljárás tooset lejárati időt.

Alkalmazás jogkivonatok jwt-t (JSON Web Token), amely alá van írva a kulcsok egyikével.

Az alkalmazás jogkivonatában tartalmazhat hello jogcímek a következő:

| Jogcím | Leírás |
| --- | --- |
| **ver** |hello alkalmazási jogkivonatának hello verzióját. 0.2.0 hello aktuális verziója van. |
| **és** |hello szánt címzett hello jogkivonat. A Power BI Embedded használható: "https://analysis.windows.net/powerbi/api". |
| **iss** |Egy karakterlánc, amely hello alkalmazás hello jogkivonatot kibocsátó. |
| **típusa** |alkalmazási jogkivonatának létrehozott hello típusa. Aktuális hello csak támogatott típus **beágyazása**. |
| **Windows azonnali csatlakozás** |Munkaterület gyűjtemény neve hello token küldése történik. |
| **belső Windows-adatbázis** |A munkaterület azonosítója hello token küldése történik. |
| **a relatív azonosítók** |Jelentés azonosító hello token küldése történik. |
| **felhasználónév** (nem kötelező) |RLS használt, ez egy olyan karakterlánc, amely segítségével azonosítható hello felhasználói RLS szabályok alkalmazásakor. |
| **szerepkörök** (nem kötelező) |Egy karakterlánc hello szerepkörök tooselect sorszintű biztonság szabályok alkalmazásakor. Ha több szerepkör, át kell őket, egy karakterlánc-tömbben. |
| **Szolgáltatáskapcsolódási pont** (nem kötelező) |Hello engedélyek hatókörök tartalmazó karakterlánc. Ha több szerepkör, át kell őket, egy karakterlánc-tömbben. |
| **Exp** (nem kötelező) |Azt jelzi, hogy mely hello a jogkivonat lejár hello idő. Ezek kell adhatók át a Unix időbélyegzőket. |
| **NBF** (nem kötelező) |Azt jelzi, hogy hello indításakor mely hello a jogkivonat érvényes alatt. Ezek kell adhatók át a Unix időbélyegzőket. |

Egy minta app tokent fog kinézni:

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ2ZXIiOiIwLjIuMCIsInR5cGUiOiJlbWJlZCIsIndjbiI6Ikd1eUluQUN1YmUiLCJ3aWQiOiJkNGZlMWViMS0yNzEwLTRhNDctODQ3Yy0xNzZhOTU0NWRhZDgiLCJyaWQiOiIyNWMwZDQwYi1kZTY1LTQxZDItOTMyYy0wZjE2ODc2ZTNiOWQiLCJzY3AiOiJSZXBvcnQuUmVhZCIsImlzcyI6IlBvd2VyQklTREsiLCJhdWQiOiJodHRwczovL2FuYWx5c2lzLndpbmRvd3MubmV0L3Bvd2VyYmkvYXBpIiwiZXhwIjoxNDg4NTAyNDM2LCJuYmYiOjE0ODg0OTg4MzZ9.v1znUaXMrD1AdMz6YjywhJQGY7MWjdCR3SmUSwWwIiI
```

Amikor dekódolni, fog megjelenni ehhez hasonló:

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

Nincsenek elérhető belül hello SDK-k, amelyek egyszerűbbé teszik apptokens létrehozása metódusok. Például a .NET-hez vessen egy pillantást hello [Microsoft.PowerBI.Security.PowerBIToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken) osztály és hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) módszerek.

A .NET SDK hello, olvassa el a túl[hatókörök](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.scopes).

## <a name="scopes"></a>Hatókörök

Beágyazási jogkivonatok használata esetén érdemes lehet toorestrict használatát hello erőforrásokhoz való hozzáférést. Emiatt a hatókörbe tartozó engedélyek jogkivonatot is létrehozhat.

Az alábbiakban hello hello elérhető hatókörök Power BI Embedded.

|Hatókör|Leírás|
|---|---|
|Dataset.Read|Engedélyt biztosít tooread hello megadott adatkészlet.|
|Dataset.Write|Itt megadott engedély toowrite toohello adatkészlet.|
|Dataset.ReadWrite|Engedély tooread és írási toohello újrafordítja dataset biztosít.|
|Report.Read|Engedélyt biztosít tooview hello megadott jelentés.|
|Report.ReadWrite|Itt engedély tooview és Szerkesztés hello megadott jelentés.|
|Workspace.Report.Create|Engedély toocreate biztosít egy új jelentés belül megadott hello munkaterületen.|
|Workspace.Report.Copy|Itt engedély tooclone belül megadott hello meglévő jelentés munkaterületen.|

Ekkor megadhatja, hogy több hatókör hello hatókörök hasonló hello között szóközt használatával.

```
string scopes = "Dataset.Read Workspace.Report.Create";
```

**Szükséges jogcímeket - hatókörök**

Szolgáltatáskapcsolódási pont: {scopesClaim} scopesClaim lehet egy karakterláncot vagy karakterláncok megjegyezni hello engedélyezett engedélyek tooworkspace erőforrások (jelentés, adatkészlet, stb.)

A dekódolt jogkivonatot, hatókörök meghatározása az hasonló toohello következő lenne.

```
Header

{
    typ: "JWT",
    alg: "HS256:
}

Body

{
  "ver": "0.2.0",
  "wcn": "SupportDemo",
  "wid": "ca675b19-6c3c-4003-8808-1c7ddc6bd809",
  "rid": "96241f0f-abae-4ea9-a065-93b428eddb17",
  "scp": "Report.Read",
  "iss": "PowerBISDK",
  "aud": "https://analysis.windows.net/powerbi/api",
  "exp": 1360047056,
  "nbf": 1360043456
}

```

### <a name="operations-and-scopes"></a>Műveletek és hatókörök

|Művelet|Tároló-erőforrás|Token engedélyek|
|---|---|---|
|Adatkészlet alapján új jelentés létrehozása (a memóriában).|Adatkészlet|Dataset.Read|
|(A memóriában) adatkészlet alapján új jelentés létrehozása és hello jelentés mentéséhez.|Adatkészlet|* Dataset.Read<br>* Workspace.Report.Create|
|Megtekintheti és (a memória) egy meglévő jelentés felfedezése és szerkesztése. Report.Read Dataset.Read jelenti. Ez nem lehetővé teszi a engedélyek toosave módosításokat.|Jelentés|Report.Read|
|Módosítsa és mentse egy meglévő jelentést.|Jelentés|Report.ReadWrite|
|A jelentés (Mentés másként) másolatának mentése.|Jelentés|* Report.Read<br>* Workspace.Report.Copy|

## <a name="heres-how-hello-flow-works"></a>Íme a hello folyamat működése
1. Másolja a hello API kulcsok tooyour alkalmazást. Hello kulcsok szerezhet be **Azure Portal**.
   
    ![](media/powerbi-embedded-get-started-sample/azure-portal.png)
2. Token állításokat egy jogcímet, és rendelkezik a lejárati idő.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-2.png)
3. Token lekérdezi az API elérési kulcsainak aláírva.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-3.png)
4. A felhasználó kéri tooview jelentést.
   
    ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-4.png)
5. Egy API-hozzáférési kulccsal rendelkező token érvényességét.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-5.png)
6. A Power BI Embedded küld egy jelentés toouser.
   
   ![](media/powerbi-embedded-get-started-sample/power-bi-embedded-token-6.png)

Után **Power BI Embedded** jelentésfelhasználó toohello küldi hello felhasználó megtekintheti a hello jelentést az egyéni alkalmazásban. Például, ha az importált hello [elemzése forgalmi adatok PBIX minta](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix), hello minta webalkalmazás néz ki:

![](media/powerbi-embedded-get-started-sample/sample-web-app.png)

## <a name="see-also"></a>Lásd még:

[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[Ismerkedés a Microsoft Power BI Embedded mintaverziójával](power-bi-embedded-get-started-sample.md)  
[Microsoft Power BI Embedded gyakori helyzetek](power-bi-embedded-scenarios.md)  
[Ismerkedés a Microsoft Power BI Embedded](power-bi-embedded-get-started.md)  
[A csharp nyelvű Power bi Git-tárház](https://github.com/Microsoft/PowerBI-CSharp)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)

