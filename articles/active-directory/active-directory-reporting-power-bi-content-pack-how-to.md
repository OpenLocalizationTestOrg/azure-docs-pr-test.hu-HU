---
title: aaaHow toouse hello Azure Active Directory Power BI tartalomcsomag |} Microsoft Docs
description: Ismerje meg, hogyan toouse hello Azure Active Directory Power BI tartalomcsomag
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d07d678aedbe3089c4ea5f981f72311bdb389a17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-azure-active-directory-power-bi-content-pack"></a>Hogyan toouse hello Azure Active Directory Power BI tartalomcsomag

Rendszergazdaként elengedhetetlen, hogy tudja, hogyan használják felhasználói az Azure Active Directory funkcióit. Ez lehetővé teszi az informatikai infrastruktúra és a kommunikáció tooincrease használati és tooget hello legtöbb kívül AAD szolgáltatások tooplan. A Power BI-tartalmakat csomag az Azure Active Directory által biztosított lehetőséget toofurther hello, elemezheti az adatokat toounderstand, hogyan használhatja a adatok toogather mélyebb betekintést az Azure Active Directoryval a hello jelenít meg különböző képességeket, erősen támaszkodnak.  A Power BI-bA az Azure Active Directory API integrációja hello egyszerűen letöltheti hello előre elkészített tartalomcsomagok és dcu tooall hello tevékenységek az Azure Active Directoryban gazdag képi megjelenítés élményt nyújt a Power BI használatával. Létrehozhatja saját irányítópultját, és könnyedén megoszthatja szervezete bármelyik tagjával. 

Ez a témakör részletes lépéseit hogyan tooinstall és -felhasználási hello tartalom csomagolja a környezetben.

## <a name="installation"></a>Telepítés  

**tooinstall hello Power BI-tartalomcsomag:**

1. Jelentkezzen be [Power BI](https://app.powerbi.com/groups/me/getdata/services) a Power BI-fiókjába (Ez az hello ugyanazt a fiókot az Office 365 vagy Azure AD-fiókot).

2. Alján hello hello bal oldali navigációs panelen, jelölje ki a **adatok beolvasása**.

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. A hello **szolgáltatások** kattintson **beolvasása**.
   
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  Keresse meg az **Azure Active Directoryt**.

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  Amikor a rendszer arra kéri, írja be Azure AD-bérlőazonosítóját, majd kattintson a **Tovább** gombra.

    > [!TIP] 
    > Egy gyorsan tooget hello Bérlőazonosító az Office 365 / Azure AD-bérlő toologin toohello Azure AD-portálhoz, részletekbe menően tárhatják toohello directory, és másolja hello azonosító URL-cím a következő hello: https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ ActiveDirectoryExtension vagykönyvtár/<tenantid>/directoryQuickStart

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  Kattintson a **Bejelentkezés** gombra. 
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  Írja be felhasználónevét és jelszavát, majd kattintson a **Bejelentkezés** gombra.
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  A következő párbeszédpanelen: hello app hozzájárulási, kattintson a **elfogadás**.
 
9.  Miután létrejött az Azure Active Directory-tevékenységnaplók irányítópultja, kattintson rá.
 
    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## <a name="what-can-i-do-with-this-content-pack"></a>Mire használhatom ezt a tartalomcsomagot?

Ahhoz, hogy belevágjon mit tehet a tartalomcsomaggal, az alábbiakban rövid áttekintés hello különböző jelentéseket hello tartalmat a csomag. A jelentés adatainak Visszalépés toohello **utolsó 30 nap**.

### <a name="reports-included-in-this-version-of-azure-active-directory-logs-content-pack"></a>Az Azure Active Directory-naplók tartalomcsomagjának jelen verziójában található jelentések

**Alkalmazás használatának és a Trend jelentés**: hello alkalmazásokba használt nyerhet a szervezet és a munkaterületek használt legtöbb hello és mikor. Ez a jelentés toogather betekintést Ön nemrég megkezdődött a szervezetben az alkalmazások használatának használhatja, vagy megtudhatja, mely alkalmazások állnak népszerű. Ezzel az eljárással használati is javítható, ha megjelenik, ha hello alkalmazás nem használja.

**Hely és a felhasználói bejelentkezéseket**: az összes hello bejelentkezések alapján történik az Azure Identity és hello identitásának hello felhasználók által biztosított betekintést nyerhet. Ennek segítségével részletesebb betekintést nyerhet az egyes bejelentkezésekbe, és választ kaphat többek között a következő kérdésekre:

- Honnan jelentkezett be ez a felhasználó?
- Melyik felhasználó van hello legtöbb bejelentkezéseket és ahol tegye azokat jelentkezzen be a? 
- Hello bejelentkezés sikeres volt?  
 
Megtekintheti a részleteket is egy konkrét dátumra vagy helyre kattintva.

**Egyedi felhasználók alkalmazásonként**: Egy adott alkalmazás összes egyedi felhasználója. Ebbe csak azok a felhasználók számítanak bele, akik „*sikeresen*” bejelentkeztek az adott alkalmazásba.

**Eszköz bejelentkezések**: egy megtekintheti a hello operációsrendszer-típust, és böngészők hello felhasználók többek között részletes tájékoztatást a szervezethez tartozó felhasználók használnak:

- Felhasználónév
- IP-cím
- Hely 
- Bejelentkezési állapot 

Ezzel a jelentéssel megismerheti hello különböző eszközprofilok a szervezetén belül, és határozza meg az eszközökre vonatkozó házirendeket használttól alapján

**SSPR tölcsér**: Megismerheti a szervezete jelszó-visszaállítási folyamatait. Egy betekintés bejutni hány jelszó alaphelyzetbe állítását megkísérelte hello SSPR eszköz segítségével, és ezeknek sikerrel járt-e. Feltárva hello jelszó alaphelyzetbe állítása sikertelen hello SSPR tölcsér használatával, és megismerheti a miért bizonyos hibák történtek-e. Ez a jelentés hello SSPR eszköz használatáról a szervezeten belül, hogy jobb döntéseket hello bemutatják tartalmazza.

## <a name="customizing-azure-ad-activity-content-pack"></a>Az Azure AD-tevékenység tartalomcsomag személyre szabása

**Módosíthatja a képi megjelenítés**: kattintva módosíthatja a képi megjelenítés jelentés **jelentés szerkesztése** válassza ki a kívánt hello képi megjelenítés.
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

**További mezők szerepelhetnek,**: mező toohello jelentés hozzáadása, vagy távolítsa el a hello visual toowhich tooadd/eltávolítása hello mező kiválasztásával. Hello az alábbi példában a "bejelentkezés" mező toohello tábla nézet hozzáadott. 
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

**PIN-kód képi megjelenítések tooyour irányítópult**: testre szabhatja az irányítópultot és a saját képi megjelenítések toohello jelentéssel és toohello irányítópulton rögzítheti. A hello az alábbi példában I "bejelentkezési állapot" nevű új szűrő hozzáadott és hello jelentésben szereplő. I is hello képi megjelenítés változása sávdiagram tooa vonaldiagram, és az új visual toohello irányítópult rögzíthető.

![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


**Az irányítópult megosztása**: Miután létrehozta a kívánt hello tartalmat, megoszthatja hello irányítópult hello felhasználókat a szervezetben. Ne feledje, hogy miután hello jelentés megosztásához láthatják hello jelentésben kijelölt hello mezőt.
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## <a name="scheduling-a-daily-refresh-of-your-power-bi-report"></a>A Power BI-jelentés napi frissítésének ütemezése

túl nyissa meg a Power BI-jelentés napi frissítését tooschedule**adatkészletek > Beállítások > ütemezés frissítése** és állítsa be az alább látható módon.
 
![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## <a name="updating-toonewer-version-of-content-pack"></a>Tartalomcsomag toonewer verziójának frissítése

Ha azt szeretné, hogy tooupdate a tartalom pack tooget egy újabb verzióra:

- Töltse le a hello új csomagot, és állítsa be a cikkben szereplő utasítások szerint.

- Miután állított be, lépjen túl**adatforrás > Beállítások > adatforrás hitelesítő adatait** írja be újra a hitelesítő adatok alább látható módon

    ![Azure Active Directory Power BI-tartalomcsomag](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

Amint hello hello tartalomcsomag új verziója nem működik, eltávolíthatja hello régi verziója, hello alapul szolgáló jelentések és adatkészletek adott tartalomcsomag társított törlésével szükség esetén.

## <a name="still-having-issues"></a>Továbbra is problémákat tapasztal? 

Tekintse meg a [hibaelhárítási útmutatót](active-directory-reporting-troubleshoot-content-pack.md). A Power BI-jal kapcsolatos általános útmutatásért tekintse meg ezeket a [súgótémaköröket](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).
 

## <a name="next-steps"></a>Következő lépések

Jelentéskészítési áttekintését lásd: hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).
