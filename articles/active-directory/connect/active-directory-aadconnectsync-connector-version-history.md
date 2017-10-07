---
title: "aaaConnector Verziókiadások |} Microsoft Docs"
description: "Ez a témakör hello összekötők összes kiadása a Forefront Identity Manager (FIM) és a Microsoft Identity Manager (MIM)"
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.openlocfilehash: 3522f17c30e46542eaa367ecdefdfd2fc47f71a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connector-version-release-history"></a>Összekötő verziókiadásai
a Forefront Identity Manager (FIM) és a Microsoft Identity Manager (MIM) hello összekötők gyakran frissül.

> [!NOTE]
> Ez a témakör csak a FIM és a MIM rendszer. Az összekötők nem támogatottak az Azure AD Connect telepítése. Az AADConnect kiadott összekötők előtelepített toospecified Build frissítésekor.

Ez a témakör hello összekötők kiadott összes verziójának felsorolása.

Kapcsolódó hivatkozások:

* [Töltse le a legfrissebb összekötők](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Általános LDAP-összekötő](active-directory-aadconnectsync-connector-genericldap.md) dokumentáció
* [Általános SQL-összekötő](active-directory-aadconnectsync-connector-genericsql.md) dokumentáció
* [Webalkalmazás-Services-összekötő](http://go.microsoft.com/fwlink/?LinkID=226245) dokumentáció
* [PowerShell-összekötő](active-directory-aadconnectsync-connector-powershell.md) dokumentáció
* [Lotus Domino-összekötő](active-directory-aadconnectsync-connector-domino.md) dokumentáció


## <a name="116040-aadconnect-pending-release"></a>1.1.604.0 (AADConnect függőben lévő kiadás)


### <a name="fixed-issues"></a>Javított problémák:

* Általános webszolgáltatások:
  * Megtörtént egy probléma javítása meggátolja, hogy a SOAP-projekt jöjjenek létre, amikor két vagy több végpontot történt.
* Általános SQL:
  * Az importálás hello működésének hello GSQL lett nem konvertálása idő megfelelően, mentésekor tooconnector terület. hello hello GSQL összekötő lemezterület az alapértelmezett dátum és idő formátuma megváltozott too'yyyy-hh-nn "éééé-hh-nn hh:mm:ssZ" HH:mm:ssZ ".

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Javított problémák:

* Általános webszolgáltatások:
  * hello Wsconfig eszköz nem megfelelő átalakítani "minta kérelem" hello REST szolgáltatás metódus hello Json-tömb. A Json-tömb hello REST kérelem ennek oka a szerializálás problémákat.
  * Web Service Connector konfigurációs eszköz nem támogatja a hely szimbólumok használata a JSON-attribútum neve 
    * A behelyettesítések felveheti manuálisan toohello WSConfigTool.exe.config fájl, pl.```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```

* Lotus Notes:
  * Ha a beállítás hello **lehetővé teszik egyéni képesítést adók engedélyezése a szervezet vagy szervezeti egység** le van tiltva, akkor a következők exportált tooDomino hello összekötő sikertelen (frissítés) után hello exportálási flow attribútumainak exportálás során, de hello helyreállításkor Exportálás egy KeyNotFoundException tooSync adja vissza. 
    * Ennek oka hello átnevezése művelet sikertelen lesz, amikor toochange megkülönböztető név (felhasználónév attribútum) módosításával az alábbi hello attribútumok egyikét:  
      - Vezetéknév
      - Utónév
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - szervezeti egység
      - altcommonname

  * Ha **lehetővé teszik egyéni képesítést adók engedélyezése a szervezet vagy szervezeti egység** engedélyezve van, de szükséges képesítést adók engedélyezése továbbra is üres, akkor KeyNotFoundException következik be.

### <a name="enhancements"></a>Fejlesztései:

* Általános SQL:
  * **Forgatókönyv: újratervezett megvalósítva:** "*" funkció
  * **Megoldás leírása:** megközelítés megváltozott [többértékű hivatkozási attribútum kezelési](active-directory-aadconnectsync-connector-genericsql.md).


### <a name="fixed-issues"></a>Javított problémák:

* Általános webszolgáltatások:
  * Kiszolgálókonfiguráció nem importálható, ha az megtalálható a WebService összekötő
  * Több webkiszolgáló szolgáltatással nem működik a WebService összekötő

* Általános SQL:
  * Egyetlen értéket hivatkozott attribútum nem objektumtípusok találhatók
  * Különbözeti importálás objektumon változások követése stratégia törlések érték többértékű táblából eltávolításakor
  * Az AS a DB2 GSQL Connector OverflowException / 400

Lotus:
  * Keresés a szervezeti egységek GlobalParameters lap megnyitása előtt hozzáadott beállítás tooenable\disable

## <a name="114430"></a>1.1.443.0

Kiadás dátuma: 2017. március

### <a name="enhancements"></a>Fejlesztések

* Általános SQL:</br>
  **A forgatókönyvben a jelenség:** az SQL-összekötő, ahol azt csak egy hivatkozás tooone objektumtípus és engedélyezése szükséges tagokkal kereszthivatkozás hello egy jól ismert korlátozás. </br>
  **Megoldás leírása:** hello feldolgozási lépésben hivatkozásainak volt "*" lehetőséget választja, minden kombinációi objektumtípusok visszaadott hátsó toohello szinkronizálási motor.

>[!Important]
- Ezzel létrehoz sok helyőrzők
- Szükséges toomake meg arról, hogy hello elnevezési közötti objektumtípusok egyedi legyen.


* Általános LDAP:</br>
 **Forgatókönyv:** Ha csak néhány tárolók adott partíció van kijelölve, majd hello keresési továbbra is megtörténik a teljes partíció. Specifikus szűri a Synchronization szolgáltatás által, de nem MA, ami problémát okozhat teljesítménycsökkenést. </br>

 **Megoldás leírása:** megváltozott GLDAP összekötő kód toomake lehetőség halad át az összes tároló és azok helyett a hello teljes partíció keresése objektumok kereséséhez.


* Lotus Domino:

  **Forgatókönyv:** Domino mail törlés támogatása egy személy eltávolítása az exportálás során. </br>
  **Megoldás:** konfigurálható mail törlés támogatása egy személy eltávolítása az exportálás során.

### <a name="fixed-issues"></a>Javított problémák:
* Általános webszolgáltatások:
 * Az alapértelmezett hello szolgáltatás URL-címe módosításakor SAP wsconfig projektek WebService konfigurációs eszköz hello a következő hiba történik, akkor: hello elérési út egy része nem található

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Általános LDAP:
 * GLDAP Connector nem látja az AD LDS összes attribútum
 * Varázsló oldaltörések nem UPN attribútum hello LDAP directory séma észlelésekor
 * Különbözeti importálása sikertelen teljes importálás során, ha nincs bejelölve a "objectclass" attribútum nem található felderítési hibákkal
 * Egy "Partíciók és hierarchiák konfigurálása" konfigurációs lapján mely típus egyenlő toohello partíció hello általános új kiszolgálókhoz tartozó objektumok nem jeleníti meg  
LDAP MA. Ezek kimutatták RootDSE partíció csak objektumokat.


* Általános SQL:
 * Javítsa ki általános SQL vízjel különbözeti importálás többértékű attribútum nem importált hiba
 * Többértékű attribútum deleted\added értékek exportálásakor nincsenek deleted\added adatforrás.  


* Lotus Notes:
 * Egy adott mező "Teljes neve" megjelenik-e a hello metaverse megfelelően azonban amikor exportáló tooNotes hello értéke hello attribútum értéke Null vagy üres.
 * Hárítsa el az ismétlődő Certifier hiba
 * Hello objektum adatok nélkül van jelölve, a más objektumokkal Lotus Domino-összekötő hello majd azt hibaüzenet hello felderítési teljes-importálás végrehajtása közben.
 * Különbözeti importálás esetén a folyamatban futó hello Lotus Domino-összekötő, hogy futási, hello hello végén Microsoft.IdentityManagement.MA.LotusDomino.Service.exe szolgáltatás néha alkalmazás hibát ad vissza.
 * A csoporttagságot általános szolgáltatás megfelelően működik, és megmarad, azonban a felhasználó hello exportálás tootry tooremove futtatásakor tagság a sikeres frissítéshez látható, de hello felhasználó nem lesz beolvasása eltávolítva Lotus Notes tagjának kell lennie.
 * Egy lehetőség toochoose módban "Append elemmel alul" Exportálás hozzá lett adva a konfigurációs Lotus grafikus felhasználói Felülettel MA tooappend új cikkek alján többértékű attribútumok hello exportálás során.
 * Összekötő hello szükséges logika toodelete hello fájlt az E-mail mappa hello és azonosító tároló adja hozzá.
 * Törölje a tagság közötti NAB tag nem működik.
 * Értékek sikeresen törlődjenek többértékű attribútum

## <a name="111170"></a>1.1.117.0
Kiadás dátuma: 2016. március

**Új összekötő**  
A kezdeti hello kiadása [általános SQL-összekötő](active-directory-aadconnectsync-connector-genericsql.md).

**Új szolgáltatások:**

* Általános LDAP-összekötőhöz:
  * Különbözeti importálás Isode való támogatása.
* Web Services-összekötővel:
  * Frissített hello csEntryChangeResult tevékenység és setImportErrorCode tevékenység tooallow objektum szintű hibák toobe visszaadott hátsó toohello szinkronizálási motor.
  * Frissített hello SAP6 és SAP6User sablonok toouse hello új objektum szint hiba funkcióit.
* Lotus Domino-összekötő:
  * Exportálás van szüksége egy certifier címjegyzék száma. Most használja az azonos hello is összes képesítést adók engedélyezése toomake hello felügyeleti könnyebb jelszavát.

**Javított problémák:**

* Általános LDAP-összekötőhöz:
  * Az IBM Tivoli DS, néhány hivatkozási attribútum a rendszer nem észlelt megfelelően.
  * Nyissa meg az LDAP a különbözeti importálás során, a rendszer csonkolt hello elején és végén karakterláncok szóközöket.
  * Novell és NetIQ, objektum áthelyezése, szervezeti egységek/tárolók közötti és hello exportálás azonos időben átnevezett hello objektum sikertelen.
* Web Services-összekötővel:
  * Ha hello webszolgáltatás ugyanezt a kötést a több végpontok közé, majd hello Connector nem megfelelően találta ezeket a végpontokat.
* Lotus Domino-összekötő:
  * Hello fullName attribútum tooa mail-adatbázisban exportálása sikertelen volt.
  * Egy exportálás, amely egyaránt felvétele, illetve tag eltávolítása egy csoportból, csak exportált hello hozzáadott tagok.
  * Ha egy megjegyzések dokumentum érvénytelen (hello attribútum isValid toofalse beállítása), majd hello összekötő sikertelen.

## <a name="older-releases"></a>Régebbi kiadásai
2016. március, mielőtt hello összekötők kiadott támogatási témakörök szerint.

**Általános LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, 2015. szeptember
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015. március
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, 2015. január
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, 2014. szeptember
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014. március

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, 2014. szeptember

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, 2014. szeptember

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, 2015. szeptember
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015. március
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, 2014. augusztus
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, 2014. február  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, 2013. október
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, 2013. augusztus

## <a name="next-steps"></a>Következő lépések
További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.

További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
