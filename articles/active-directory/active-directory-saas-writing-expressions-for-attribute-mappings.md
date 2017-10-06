---
title: "az Azure Active Directoryban attribútum-leképezésekhez kifejezések aaaWriting |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse kifejezés hozzárendelések tootransform attribútum értékei egy elfogadható formátumba SaaS app objektumok Azure Active Directoryban automatikus kiépítése során."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: b13c51cd-1bea-4e5e-9791-5d951a518943
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: markvi
ms.openlocfilehash: caa0dd8144f6e5279a869e015ed75bd24169d585
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="writing-expressions-for-attribute-mappings-in-azure-active-directory"></a>Attribútum-leképezésekhez kifejezések írása az Azure Active Directoryban
Üzembe helyezési tooa SaaS-alkalmazás konfigurálásakor megadható attribútum-leképezésekhez hello típusú egyik egy kifejezés leképezése. Ezeknél kell írnia egy parancsfájl-szerű kifejezés, amely lehetővé teszi tootransform a felhasználói adatok több biztosítható hello SaaS-alkalmazás formátumokba.

## <a name="syntax-overview"></a>Szintaxis áttekintése
Attribútum-leképezésekhez kifejezések hello szintaxisa a Visual Basic Applications (VBA) funkciók reminiscent.

* hello teljes kifejezés függvények, amelyek a nevét, majd argumentumait zárójelek áll kell meghatározni: <br>
  *Függvénynév (<< argumentumának 1 >>, <<argument N>>)*
* Előfordulhat, hogy ágyazhatók be funkciók belül egymással. Példa: <br> *FunctionOne (FunctionTwo (<<argument1>>))*
* Három különböző típusú argumentumok függvényekké átadhatók:
  
  1. Attribútumok, amelyek négyzetes szögletes zárójelbe kell foglalni. Például: [attributeName]
  2. A karakterlánckonstansokat, amelyek dupla idézőjelek közé kell. Például: "Az Amerikai Egyesült Államok"
  3. Egyéb funkciót. Például: FunctionOne (<<argument1>>, FunctionTwo (<<argument2>>))
* A karakterlánckonstansokat Ha egy fordított perjel (\) vagy az idézőjel (") hello karakterláncból a szükséges azt kell megjelölni hello fordított perjel (\) szimbólum. Például: "vállalatnév: \"Contoso\""

## <a name="list-of-functions"></a>Függvények listája
[Hozzáfűzendő](#append) &nbsp; &nbsp; &nbsp; &nbsp; [FormatDateTime](#formatdatetime) &nbsp; &nbsp; &nbsp; &nbsp; [Csatlakozás](#join) &nbsp; &nbsp; &nbsp; &nbsp; [Mid](#mid) &nbsp; &nbsp; &nbsp; &nbsp; [nem](#not) &nbsp; &nbsp; &nbsp; &nbsp; [Cserélje le](#replace) &nbsp; &nbsp; &nbsp; &nbsp; [StripSpaces](#stripspaces) &nbsp; &nbsp; &nbsp; &nbsp; [Kapcsoló](#switch)

- - -
### <a name="append"></a>Hozzáfűzés
**Függvény:**<br> Append(Source, suffix)

**Leírás:**<br> A forrás karakterlánc értéket vesz, és hozzáfűzi hello utótag toohello vége.

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Karakterlánc |Általában hello attribútumot hello adatforrás-objektum neve |
| **utótag** |Szükséges |Karakterlánc |hello karakterlánc, amelyet az adatforrás-értéke hello tooappend toohello végét. |

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Függvény:**<br> FormatDateTime (forrás, inputFormat, outputFormat)

**Leírás:**<br> Egy adott formátumból egy dátumkarakterlánc vesz igénybe, és egy másik formátumba konvertálja.

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Karakterlánc |Általában a hello attribútum a hello adatforrás-objektum neve. |
| **inputFormat** |Szükséges |Karakterlánc |Adatforrás-értéke hello formátumúak. A támogatott formátumok, lásd: [http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx](http://msdn.microsoft.com/library/8kb3ddd4%28v=vs.110%29.aspx). |
| **outputFormat** |Szükséges |Karakterlánc |A kimeneti dátum hello formátuma. |

- - -
### <a name="join"></a>Csatlakozás
**Függvény:**<br> Csatlakozás (elválasztó, source1, source2,...)

**Leírás:**<br> JOIN() hasonló tooAppend(), azzal a különbséggel, hogy kombinálhatja a több **forrás** karakterlánc az egyetlen karakterlánc értéket, majd minden egyes érték fogja elválasztani a **elválasztó** karakterlánc.

Ha egy többértékű attribútum hello forrás értékek egyike minden Ez az attribútum értékét együtt illesztett, elkülönített hello elválasztó érték lesz.

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **elválasztó** |Szükséges |Karakterlánc |Karakterláncot tooseparate forrás értékek használják, amikor azok a halmaz zónanevének egyetlen karakterlánccá egyesít. Lehet "" Ha nem elválasztó szükség. |
| ** source1... sourceN ** |Szükség esetén a változó-szám, ahányszor |Karakterlánc |Karakterlánc-értékek toobe csatlakoztatott együtt. |

- - -
### <a name="mid"></a>Mid
**Függvény:**<br> Mid (forrás, kezdet, hossz)

**Leírás:**<br> Hello adatforrás-értéke egy részét adja vissza. Egy karakterláncrészletet: csak néhány hello forráskarakterlánc hello karaktert tartalmazó karakterlánc.

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Karakterlánc |Általában hello attribútum neve. |
| **start** |Szükséges |egész szám |Index hello **forrás** karakterlánc, ahol a substring kell kezdődnie. Hello karakterlánc első karaktere az 1 indexe lesz, a második karakter 2 mutatója, és így tovább. |
| **hossza** |Szükséges |egész szám |Hello substring hosszát. Hossz lejártát külső hello **forrás** karakterlánc, a függvény karakterláncrészletet ad vissza **start** végét indextől **forrás** karakterlánc. |

- - -
### <a name="not"></a>nem
**Függvény:**<br> Not(Source)

**Leírás:**<br> Átfordítás hello hello logikai értéke **forrás**. Ha **forrás** értéke "*igaz*", adja vissza "*hamis*". Ellenkező esetben adja vissza "*igaz*".

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Logikai típusú karakterlánc |Várt **forrás** értékek: "True" vagy "False". |

- - -
### <a name="replace"></a>Csere
**Függvény:**<br> ObsoleteReplace (forrás, oldValue, regexPattern, regexGroupName, helyettesítő értéke, replacementAttributeName, sablon)

**Leírás:**<br>
Lecseréli az értékeket karakterláncként. Attól függően, hogy a megadott paraméterek hello másképp működik:

* Ha **oldValue** és **helyettesítő értéke** itt találhatók:
  
  * A forrásban: hello oldValue összes előfordulását lecseréli helyettesítő értéke
* Ha **oldValue** és **sablon** itt találhatók:
  
  * Hello összes előfordulását lecseréli **oldValue** a hello **sablon** a hello **forrás** érték
* Ha **oldValueRegexPattern**, **oldValueRegexGroupName**, **helyettesítő értéke** itt találhatók:
  
  * Lecseréli az összes oldValueRegexPattern hello forrás karakterlánc helyettesítő értéke a megfelelő értékeket
* Ha **oldValueRegexPattern**, **oldValueRegexGroupName**, **replacementPropertyName** itt találhatók:
  
  * Ha **forrás** értéke van, de **forrás** adja vissza
  * Ha **forrás** nem rendelkezik értékkel, használja a **oldValueRegexPattern** és **oldValueRegexGroupName** tooextract helyettesítő értéket hello tulajdonság  **replacementPropertyName**. Csereértékre hello eredményt adja vissza a rendszer

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Karakterlánc |Általában a hello attribútum a hello adatforrás-objektum neve. |
| **oldValue** |Optional |Karakterlánc |Lecseréli a toobe érték **forrás** vagy **sablon**. |
| **regexPattern** |Optional |Karakterlánc |Regex mintát hello érték toobe a váltják le **forrás**. Vagy a replacementPropertyName használatakor mintát tooextract helyettesítő tulajdonság értékét. |
| **regexGroupName** |Optional |Karakterlánc |Hello csoport neve **regexPattern**. Csak akkor, ha replacementPropertyName használata esetén azt ki a csoport értékét, helyettesítő helyettesítő tulajdonság értéke. |
| **helyettesítő értéke** |Optional |Karakterlánc |Az új érték tooreplace régiétől. |
| **replacementAttributeName** |Optional |Karakterlánc |Hello attribútum toobe használt értékét, ha az adatforrás nem ad meg értéket neve. |
| **sablon** |Optional |Karakterlánc |Ha **sablon** érték van megadva, fog keresni **oldValue** belül hello sablont, és cserélje le az adatforrás-értéke. |

- - -
### <a name="stripspaces"></a>StripSpaces
**Függvény:**<br> StripSpaces(source)

**Leírás:**<br> Eltávolítja az összes szóközt ("") karaktert hello forrás karakterláncot.

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Karakterlánc |**forrás** érték tooupdate. |

- - -
### <a name="switch"></a>Kapcsoló
**Függvény:**<br> A kapcsoló (forrás, defaultValue, key1, érték1, key2, érték2,...)

**Leírás:**<br> Ha **forrás** értéke megegyezik egy **kulcs**, adja vissza **érték** az adott **kulcs**. Ha **forrás** érték nem felel meg a kulcsokat, értéket ad vissza **defaultValue**.  **Kulcs** és **érték** paraméterek mindig párban kell származniuk. hello mindig függvényhez paraméterek páros számú.

**Paraméterek:**<br> 

| Név | Kötelező / ismétlődő | Típus | Megjegyzések |
| --- | --- | --- | --- |
| **forrás** |Szükséges |Karakterlánc |**Forrás** érték tooupdate. |
| **DefaultValue érték** |Optional |Karakterlánc |Alapértelmezett érték toobe használható, ha a forrás nem felel meg a kulcsokat. Üres karakterlánc lehet (""). |
| **kulcs** |Szükséges |Karakterlánc |**Kulcs** toocompare **forrás** az értéket. |
| **érték** |Szükséges |Karakterlánc |A hello csereértékre **forrás** megfelelő hello kulcs. |

## <a name="examples"></a>Példák
### <a name="strip-known-domain-name"></a>Sáv ismert tartománynév
Egy felhasználó e-mailek tooobtain egy felhasználónevet egy ismert tartománynévnek toostrip van szüksége. <br>
Például ha hello tartomány "contoso.com", majd használhatja hello kifejezés a következő:

**Kifejezés:** <br>
`Replace([mail], "@contoso.com", , ,"", ,)`

**A minta bemeneti / kimeneti:** <br>

* **BEMENETI** (mail): "john.doe@contoso.com"
* **KIMENETI**: "john.doe"

### <a name="append-constant-suffix-toouser-name"></a>Állandó utótagnevet toouser hozzáfűzése
Ha Salesforce védőfalat használ, szükség lehet egy további utótagok tooall tooappend a felhasználónevek őket szinkronizálása előtt.

**Kifejezés:** <br>
`Append([userPrincipalName], ".test"))`

**Minta bemeneti/kimeneti:** <br>

* **BEMENETI**: (userPrincipalName): "John.Doe@contoso.com"
* **KIMENETI**: "John.Doe@contoso.com.test"

### <a name="generate-user-alias-by-concatenating-parts-of-first-and-last-name"></a>Utónév és Vezetéknév hozzáfűzésével részei felhasználói alias létrehozása
A felhasználó utónevét először 3 betű vagy a felhasználó vezetéknevét első 5 betűket megtételével toogenerate felhasználói alias van szüksége.

**Kifejezés:** <br>
`Append(Mid([givenName], 1, 3), Mid([surname], 1, 5))`

**Minta bemeneti/kimeneti:** <br>

* **BEMENETI** (givenName): "John"
* **BEMENETI** (Vezetéknév): "Doe"
* **KIMENETI**: "JohDoe"

### <a name="output-date-as-a-string-in-a-certain-format"></a>Bizonyos formátumban karakterláncként kimeneti dátum
Azt szeretné, hogy toosend dátumok tooa SaaS-alkalmazás egy bizonyos formátumban. <br>
Például tooformat dátumok kívánt ServiceNow.

**Kifejezés:** <br>

`FormatDateTime([extensionAttribute1], "yyyyMMddHHmmss.fZ", "yyyy-MM-dd")`

**Minta bemeneti/kimeneti:**

* **BEMENETI** (extensionAttribute1): "20150123105347.1Z"
* **KIMENETI**: "2015-01-23"

### <a name="replace-a-value-based-on-predefined-set-of-options"></a>Cserélje le egy előre megadott beállítások alapján értékét
Az Azure ad-ben tárolt hello kódja alapján hello felhasználói toodefine hello időzónájának van szüksége. <br>
Ha hello állapot kód nem egyezik az előre megadott hello-beállítások, használja a "Ausztrália/Sydney" alapértelmezett értékét.

**Kifejezés:** <br>

`Switch([state], "Australia/Sydney", "NSW", "Australia/Sydney","QLD", "Australia/Brisbane", "SA", "Australia/Adelaide")`

**Minta bemeneti/kimeneti:**

* **BEMENETI** (állapot): "QLD"
* **KIMENETI**: "Ausztrália/Brisbane"

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Felhasználói kiépítési/megszüntetés tooSaaS alkalmazások automatizálása](active-directory-saas-app-provisioning.md)
* [A felhasználók átadása attribútum-leképezésekhez testreszabása](active-directory-saas-customizing-attribute-mappings.md)
* [Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez](active-directory-saas-scoping-filters.md)
* [SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával](active-directory-scim-provisioning.md)
* [Alkalmazás-kiépítési értesítések](active-directory-saas-account-provisioning-notifications.md)
* [Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz](active-directory-saas-tutorial-list.md)

