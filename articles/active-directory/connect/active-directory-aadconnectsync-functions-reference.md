---
title: "Azure AD Connect szinkronizálása: funkciók referencia |} Microsoft Docs"
description: "Az Azure AD Connect szinkronizálási szolgáltatás deklaratív kiépítés kifejezéseinek hivatkozását."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 4f525ca0-be0e-4a2e-8da1-09b6b567ed5f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: fbe0df856ca2efda965650fb85c7e831a0be32c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect szinkronizálása: funkciók referencia
Az Azure AD Connectben funkciók használt toomanipulate egy attribútum értékét a szinkronizálás során.  
hello hello funkciók szintaxisa a következő formátumban hello kifejezett:  
`<output type> FunctionName(<input type> <position name>, ..)`

Egy függvény túl van terhelve, és több szintaxissal fogad el, ha az összes érvényes szintaxissal vannak felsorolva.  
hello funkciók erős típusmegadású vannak, és azok győződjön meg arról, hogy hello típust kapott találatok dokumentált hello típus.  
Hello típusa nem egyezik, ha hiba fordul elő.

hello típusok szerint van megadva, a szintaxis a következő hello:

* **Bin** – bináris
* **logikai** – logikai
* **DT** – UTC dátum és idő
* **Enum** – az ismert állandók számbavétele
* **Exp** – kifejezés, amely várt tooevaluate tooa logikai érték
* **mvbin** – többértékű bináris
* **mvstr** – többértékű karakterlánc
* **mvref** – többértékű referencia
* **NUM** – numerikus
* **REF** – referencia
* **Str** – karakterlánc
* **var** – (szinte) bármilyen más típusú variant
* **"void"** – nem ad visszatérési értéket

hello típusú funkciók hello **mvbin**, **mvstr**, és **mvref** többértékű attribútumok csak használható. A Functions **bin**, **str**, és **ref** munkahelyi egyaránt egyértékű és többértékű jellemzőit.

## <a name="functions-reference"></a>Functions – referencia
| Függvények listája |  |  |  |  |
| --- | --- | --- | --- | --- | --- |
| **Tanúsítvány** | | | | |
| [CertExtensionOids](#certextensionoids) |[CertFormat](#certformat) |[CertFriendlyName](#certfriendlyname) |[CertHashString](#certhashstring) | |
| [CertIssuer](#certissuer) |[CertIssuerDN](#certissuerdn) |[CertIssuerOid](#certissueroid) |[CertKeyAlgorithm](#certkeyalgorithm) | |
| [CertKeyAlgorithmParams](#certkeyalgorithmparams) |[CertNameInfo](#certnameinfo) |[CertNotAfter](#certnotafter) |[CertNotBefore](#certnotbefore) | |
| [CertPublicKeyOid](#certpublickeyoid) |[CertPublicKeyParametersOid](#certpublickeyparametersoid) |[CertSerialNumber](#certserialnumber) |[CertSignatureAlgorithmOid](#certsignaturealgorithmoid) | |
| [CertSubject](#certsubject) |[CertSubjectNameDN](#certsubjectnamedn) |[CertSubjectNameOid](#certsubjectnameoid) |[CertThumbprint](#certthumbprint) | |
[CertVersion](#certversion) |[IsCert](#iscert) | | | |
| **Átalakítás** | | | | |
| [CBool](#cbool) |[CDate](#cdate) |[CGuid](#cguid) |[ConvertFromBase64](#convertfrombase64) | |
| [ConvertToBase64](#converttobase64) |[ConvertFromUTF8Hex](#convertfromutf8hex) |[ConvertToUTF8Hex](#converttoutf8hex) |[CNum](#cnum) | |
| [CRef](#cref) |[CStr](#cstr) |[StringFromGuid](#StringFromGuid) |[StringFromSid](#stringfromsid) | |
| **Dátum és idő** | | | | |
| [DateAdd](#dateadd) |[DateFromNum](#datefromnum) |[FormatDateTime](#formatdatetime) |[Most](#now) | |
| [NumFromDate](#numfromdate) | | | | |
| **Könyvtár** | | | | |
| [DNComponent](#dncomponent) |[DNComponentRev](#dncomponentrev) |[EscapeDNComponent](#escapedncomponent) | | |
| **Értékelés** | | | | |
| [IsBitSet](#isbitset) |[IsDate](#isdate) |[Vagyis IsEmpty](#isempty) |[IsGuid](#isguid) | |
| [IsNull](#isnull) |[IsNullOrEmpty](#isnullorempty) |[IsNumeric](#isnumeric) |[IsPresent](#ispresent) | |
| [IsString](#isstring) | | | | |
| **Matematikai** | | | | |
| [BitAnd](#bitand) |[BitOr](#bitor) |[RandomNum](#randomnum) | | |
| **Többértékű** | | | | |
| [Tartalmazza](#contains) |[Száma](#count) |[Elem](#item) |[ItemOrNull](#itemornull) | |
| [Csatlakozás](#join) |[RemoveDuplicates](#removeduplicates) |[Vegyes](#split) | | |
| **Program folyamata** | | | | |
| [Hiba történt](#error) |[AZ IIF](#iif) |[Kiválasztás](#select) |[Kapcsoló](#switch) | |
| [Ha](#where) |[A](#with) | | | |
| **Szöveg** | | | | |
| [GUID](#guid) |[InStr](#instr) |[InStrRev](#instrrev) |[LCase](#lcase) | |
| [Balra](#left) |[Hossz](#len) |[LTrim](#ltrim) |[Mid](#mid) | |
| [PadLeft](#padleft) |[PadRight](#padright) |[PCase](#pcase) |[Cserélje le](#replace) | |
| [ReplaceChars](#replacechars) |[Jobbra](#right) |[RTrim](#rtrim) |[Trim](#trim) | |
| [UCase](#ucase) |[Word](#word) | | | |

- - -
### <a name="bitand"></a>BitAnd
**Leírás:**  
hello BitAnd függvény megadott bits értéket állítja be.

**Szintaxis:**  
`num BitAnd(num value1, num value2)`

* Érték1, érték2: numerikus érték, amely együtt kell-e érték

**Megjegyzés:**  
Ez a funkció mindkét paraméterek toohello bináris megjelenítése alakítja át, és beállítja egy kicsit:

* 0 – Ha az egyik vagy mindkét hello megfelelő bit *maszk* és *jelző* : 0
* 1 – Ha hello megfelelő bits mindkettő az 1.

Ez azt jelenti akkor adja vissza 0 minden esetben, kivéve ha hello megfelelő bitek száma mindkét paraméter 1.

**Példa**  
`BitAnd(&HF, &HF7)`  
7 adja vissza, mivel a hexadecimális "F" és "F7" kiértékelése toothis érték.

- - -
### <a name="bitor"></a>BitOr
**Leírás:**  
hello BitOr függvény megadott bits értéket állítja be.

**Szintaxis:**  
`num BitOr(num value1, num value2)`

* Érték1, érték2: numerikus érték, amely együtt kell-e köztük

**Megjegyzés:**  
Ez a funkció mindkét paraméterek toohello bináris megjelenítése konvertálja, és beállítja egy bit too1, ha az egyik vagy mindkét hello megfelelő bit maszk és jelző: 1 és too0 Ha mindkét hello megfelelő bits 0. Ez azt jelenti az 1 értékkel tér vissza minden esetben kivéve ahol hello megfelelő mindkét paraméter bitje 0.

- - -
### <a name="cbool"></a>CBool
**Leírás:**  
hello CBool függvényt ad vissza, olyan logikai érték alapján értékeli ki hello kifejezés

**Szintaxis:**  
`bool CBool(exp Expression)`

**Megjegyzés:**  
Ha hello kifejezés tooa nullától eltérő értéket, majd CBool igaz értéket ad vissza, ellenkező esetben a visszatérési hamis.

**Példa**  
`CBool([attrib1] = [attrib2])`  

Értéke True, ha mindkét attribútumok beolvasása hello ugyanazt az értéket.

- - -
### <a name="cdate"></a>CDate
**Leírás:**  
hello CDate függvény UTC DateTime egy karakterláncot ad vissza. Dátum és idő nincs szinkronban natív attribútumtípus, de egyes funkciókat használják.

**Szintaxis:**  
`dt CDate(str value)`

* Érték: A karakterlánc dátum, idő, és opcionálisan időzóna

**Megjegyzés:**  
hello visszaadott karakterlánc mindig UTC formátumban.

**Példa**  
`CDate([employeeStartTime])`  
Visszaadja egy dátum és idő alapján hello alkalmazott kezdési ideje

`CDate("2013-01-10 4:00 PM -8")`  
Visszaadja egy jelölő dátum és idő "2013-01-11 12:00-kor"








- - -
### <a name="certextensionoids"></a>CertExtensionOids
**Leírás:**  
Vissza az összes hello kritikus bővítmény tanúsítvány objektum Oid értékek hello.

**Szintaxis:**  
`mvstr CertExtensionOids(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certformat"></a>CertFormat
**Leírás:**  
Beolvasása hello hello formátum a X.509v3 tanúsítvány nevét.

**Szintaxis:**  
`str CertFormat(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certfriendlyname"></a>CertFriendlyName
**Leírás:**  
Tanúsítvány alias társított hello adja vissza.

**Szintaxis:**  
`str CertFriendlyName(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certhashstring"></a>CertHashString
**Leírás:**  
Beolvasása hello X.509v3 tanúsítványt SHA1-kivonatot hello hexadecimális karakterlánc.

**Szintaxis:**  
`str CertHashString(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certissuer"></a>CertIssuer
**Leírás:**  
Beolvasása hello hello X.509v3 tanúsítványt kiállító hitelesítésszolgáltató hello nevét.

**Szintaxis:**  
`str CertIssuer(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certissuerdn"></a>CertIssuerDN
**Leírás:**  
Beolvasása hello hello tanúsítvány kiállítójának megkülönböztető neve.

**Szintaxis:**  
`str CertIssuerDN(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certissueroid"></a>CertIssuerOid
**Leírás:**  
Beolvasása hello Oid hello tanúsítványt kibocsátó.

**Szintaxis:**  
`str CertIssuerOid(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certkeyalgorithm"></a>CertKeyAlgorithm
**Leírás:**  
A string típusúként a X.509v3 tanúsítványt hello nyilvánoskulcs-képzési algoritmus információt adhatja vissza.

**Szintaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certkeyalgorithmparams"></a>CertKeyAlgorithmParams
**Leírás:**  
Hello nyilvánoskulcs-képzési algoritmus paramétereinek hello X.509v3 tanúsítványt Hexadecimális karakterláncként adja vissza.

**Szintaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certnameinfo"></a>CertNameInfo
**Leírás:**  
Ad vissza hello tulajdonos és a kiállító nevét a tanúsítványt.

**Szintaxis:**  
`str CertNameInfo(binary certificateRawData, str x509NameType, bool includesIssuerName)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.
*   X509NameType: hello hello tulajdonos X509NameType értékét.
*   includesIssuerName: true tooinclude hello kibocsátó neve; Ellenkező esetben hamis.

- - -
### <a name="certnotafter"></a>CertNotAfter
**Leírás:**  
Hello dátumot adja vissza, amely után a tanúsítvány már nem érvényes helyi idő.

**Szintaxis:**  
`dt CertNotAfter(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certnotbefore"></a>CertNotBefore
**Leírás:**  
A tanúsítvány hatályba lép, a helyi idő hello dátumot adja vissza.

**Szintaxis:**  
`dt CertNotBefore(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certpublickeyoid"></a>CertPublicKeyOid
**Leírás:**  
Beolvasása hello Oid hello hello X.509v3 tanúsítványhoz tartozó nyilvános kulcs.

**Szintaxis:**  
`str CertKeyAlgorithm(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certpublickeyparametersoid"></a>CertPublicKeyParametersOid
**Leírás:**  
Vissza a nyilvános kulcs paraméterek hello hello X.509v3 tanúsítványt Oid hello.

**Szintaxis:**  
`str CertPublicKeyParametersOid(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certserialnumber"></a>CertSerialNumber
**Leírás:**  
Hello hello X.509v3 tanúsítvány sorozatszámát adja vissza.

**Szintaxis:**  
`str CertSerialNumber(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certsignaturealgorithmoid"></a>CertSignatureAlgorithmOid
**Leírás:**  
Beolvasása hello hello algoritmus Oid toocreate hello aláírási tanúsítványt használja.

**Szintaxis:**  
`str CertSignatureAlgorithmOid(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certsubject"></a>CertSubject
**Leírás:**  
Lekérdezi hello tulajdonos tanúsítványon megjelölt megkülönböztető neve.

**Szintaxis:**  
`str CertSubject(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certsubjectnamedn"></a>CertSubjectNameDN
**Leírás:**  
Beolvasása hello tulajdonos tanúsítványon megjelölt megkülönböztető neve.

**Szintaxis:**  
`str CertSubjectNameDN(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certsubjectnameoid"></a>CertSubjectNameOid
**Leírás:**  
Vissza a tanúsítvány tulajdonos neve hello Oid hello.

**Szintaxis:**  
`str CertSubjectNameOid(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certthumbprint"></a>CertThumbprint
**Leírás:**  
Egy tanúsítvány hello ujjlenyomata értéket ad vissza.

**Szintaxis:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="certversion"></a>CertVersion
**Leírás:**  
Vissza a tanúsítvány X.509 formátumú verziója hello.

**Szintaxis:**  
`str CertThumbprint(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.

- - -
### <a name="cguid"></a>CGuid
**Leírás:**  
hello CGuid függvény alakítja át a GUID tooits bináris megjelenítése a hello karakterláncos ábrázolása.

**Szintaxis:**  
`bin CGuid(str GUID)`

* Ebben a mintában formátumú karakterlánc: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

- - -
### <a name="contains"></a>Contains
**Leírás:**  
hello Contains keresése belüli egy többértékű karakterlánc

**Szintaxis:**  
`num Contains (mvstring attribute, str search)`-nagybetűk  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`-nagybetűk

* attribútum: hello többértékű attribútum toosearch.
* Keresés: hello attribútum toofind karakterlánc.
* Casetype: CaseInsensitive vagy CaseSensitive.

Ahol hello karakterlánc található hello többértékű attribútumban index értéket ad vissza. 0 eredményül, ha a hello karakterlánc nem található.

**Megjegyzés:**  
Attribútumok a többértékű karakterlánc hello kereséséhez karakterláncrészletek hello értékeket.  
Hivatkozás attribútumok hello keresett karakterlánc pontosan egyeznie kell hello érték toobe egyezés számít.

**Példa**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Ha hello proxyAddresses attribútum egy elsődleges e-mail címet (nagybetűs által jelzett "SMTP:"), majd térjen vissza a hello proxyAddress attribútum, ellenkező esetben egy hibaüzenetet.

- - -
### <a name="convertfrombase64"></a>ConvertFromBase64
**Leírás:**  
hello ConvertFromBase64 függvény konvertálja hello megadott base64-kódolású érték tooa rendszeres karakterlánc.

**Szintaxis:**  
`str ConvertFromBase64(str source)`-feltételezi, hogy a Unicode kódolási  
`str ConvertFromBase64(str source, enum Encoding)`

* Forrás: Base64 kódolású karakterlánc  
* Kódolást: Unicode, ASCII, UTF8

**Példa**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Mindkét példák vissza "*Hello world!*"

- - -
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex
**Leírás:**  
hello ConvertFromUTF8Hex függvény konvertálja hello megadott kódolású UTF8 hexadecimális érték tooa karakterlánc.

**Szintaxis:**  
`str ConvertFromUTF8Hex(str source)`

* Forrás: UTF8 2 bájtos kódolású karakterlánc

**Megjegyzés:**  
hello a függvény és ConvertFromBase64([],UTF8) közötti hello eredményező különbség rövid hello megkülönböztető név attribútum.  
Ezt a formátumot használja Azure Active Directory megkülönböztető neve.

**Példa**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Értéket ad vissza "*Hello world!*"

- - -
### <a name="converttobase64"></a>ConvertToBase64
**Leírás:**  
hello ConvertToBase64 függvény egy karakterlánc tooa Unicode Base64 kódolású karakterlánc alakítja át.  
Egész számok tooits egyenértékű karakterlánc-ábrázolása számjegyükkel base-64 kódolású tömbje hello értékének alakítja.

**Szintaxis:**  
`str ConvertToBase64(str source)`

**Példa**  
`ConvertToBase64("Hello world!")`  
Visszaadja a "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

- - -
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex
**Leírás:**  
hello ConvertToUTF8Hex függvény egy karakterlánc tooa kódolású UTF8 hexadecimális érték alakítja át.

**Szintaxis:**  
`str ConvertToUTF8Hex(str source)`

**Megjegyzés:**  
hello kimeneti formátum függvény lesz az Azure Active Directory megkülönböztető név attribútum formátuma.

**Példa**  
`ConvertToUTF8Hex("Hello world!")`  
48656C6C6F20776F726C6421 beolvasása

- - -
### <a name="count"></a>Darabszám
**Leírás:**  
hello Count függvénnyel többértékű attribútum hello számú elemet ad vissza

**Szintaxis:**  
`num Count(mvstr attribute)`

- - -
### <a name="cnum"></a>CNum
**Leírás:**  
hello CNum függvény lép egy karakterláncot, és adja vissza egy numerikus adattípusú.

**Szintaxis:**  
`num CNum(str value)`

- - -
### <a name="cref"></a>CRef
**Leírás:**  
Egy karakterlánc tooa hivatkozási attribútum konvertálja

**Szintaxis:**  
`ref CRef(str value)`

**Példa**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

- - -
### <a name="cstr"></a>CStr
**Leírás:**  
hello CStr függvény tooa karakterlánc adattípusra alakítja át.

**Szintaxis:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

* érték: egy numerikus értéket, a hivatkozási attribútum, vagy a logikai érték lehet.

**Példa**  
`CStr([dn])`  
Visszaadhatja "cn = Joe, dc = contoso, dc = com"

- - -
### <a name="dateadd"></a>DateAdd
**Leírás:**  
A megadott időtartam hozzáadását dátum toowhich tartalmazó egy dátumot adja vissza.

**Szintaxis:**  
`dt DateAdd(str interval, num value, dt date)`

* időköz: karakterlánc-kifejezés, amely azt szeretné, hogy tooadd hello időközt. hello karakterlánc hello a következő értékek egyikével kell rendelkeznie:
  * ÉÉÉÉ év
  * q negyedév
  * m hónap
  * y év napja
  * d nap
  * w hét napja
  * hh hét
  * h óra
  * n perc
  * s második
* érték: hello szám egységek tooadd szeretné. Ez lehet a pozitív (hello jövőbeli dátumok tooget) vagy negatív (elmúlt hello tooget dátumok).
* dátum: DateTime képviselő toowhich hello dátumtartományt kerül.

**Példa**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
3 hónapos hozzáadja, és egy dátum és idő "2001-04-01" jelző adja vissza.

- - -
### <a name="datefromnum"></a>DateFromNum
**Leírás:**  
hello DateFromNum függvény alakít egy értéket a AD meg dátum formátum tooa dátum/idő típusú.

**Szintaxis:**  
`dt DateFromNum(num value)`

**Példa**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Visszaadja a 2012-01-01 jelölő dátum és idő 23:00:00

- - -
### <a name="dncomponent"></a>DNComponent
**Leírás:**  
hello DNComponent funkció egy megadott DN összetevő balról fog hello értékét adja vissza.

**Szintaxis:**  
`str DNComponent(ref dn, num ComponentNumber)`

* megkülönböztető név: hello hivatkozási attribútum toointerpret
* ComponentNumber: a hello DN tooreturn hello összetevője

**Példa**  
`DNComponent([dn],1)`  
Megkülönböztető név esetén "cn = Joe, ou =...," Joe adja vissza

- - -
### <a name="dncomponentrev"></a>DNComponentRev
**Leírás:**  
hello DNComponentRev függvény jobb (hello célból) üzembe helyezésről meghatározott DN összetevője hello értékét adja vissza.

**Szintaxis:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

* megkülönböztető név: hello hivatkozási attribútum toointerpret
* ComponentNumber - hello DN tooreturn hello összetevő
* Beállítások: DC – figyelmen kívül az összes összetevő "dc ="

**Példa**  
Megkülönböztető név esetén "cn Joe, ou = Atlanta –, ou = GA, ou = = US, dc = contoso, dc = com" majd  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Mindkét vissza VELÜNK.

- - -
### <a name="error"></a>Hiba
**Leírás:**  
hello hiba függvény használt tooreturn egyéni hiba.

**Szintaxis:**  
`void Error(str ErrorMessage)`

**Példa**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Hello attribútum accountName nincs jelen, ha a hiba throw hello objektumon.

- - -
### <a name="escapedncomponent"></a>EscapeDNComponent
**Leírás:**  
hello EscapeDNComponent függvény időt vesz igénybe a DN egy összetevőt, és így ábrázolhatók az LDAP-kiszolgálón lehet kilépni.

**Szintaxis:**  
`str EscapeDNComponent(str value)`

**Példa**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Ellenőrzi, hogy hello objektum egy LDAP-címtár is létrehozható, akkor is, ha hello displayName attribútum karakterek, amelyek az LDAP-kiszolgálón kell megjelölni.

- - -
### <a name="formatdatetime"></a>FormatDateTime
**Leírás:**  
hello FormatDateTime függvény használt tooformat megadott formátumú dátum és idő tooa karakterláncot

**Szintaxis:**  
`str FormatDateTime(dt value, str format)`

* érték: hello dátum és idő formátumú érték
* formátum: hello formátum tooconvert való képviselő karakterláncot.

**Megjegyzés:**  
a lehetséges értékek hello a hello formátum itt találhatók: [felhasználói dátum és idő formátumú (Format függvény)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Példa**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
"2007-12-25" eredményez.

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
"20140905081453.0Z" eredményezhet

- - -
### <a name="guid"></a>GUID
**Leírás:**  
hello függvény GUID hoz létre egy új véletlenszerű GUID-ja

**Szintaxis:**  
`str GUID()`

- - -
### <a name="iif"></a>AZ IIF
**Leírás:**  
hello IIF függvény a megadott feltétel alapján lehetséges értékek egyikét adja vissza.

**Szintaxis:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

* feltétel: bármely érték vagy kifejezés, amely lehet értékelni tootrue vagy HAMIS eredményt ad.
* valueIfTrue: hello feltétel tootrue, ha hello értéket adott vissza.
* valueIfFalse: hello feltétel toofalse, ha hello értéket adott vissza.

**Példa**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Az internalizálási hello felhasználó esetén ad vissza, hello alias a felhasználó a "t-" hozzáadott toohello elejére, más hello felhasználói alias, adja vissza.

- - -
### <a name="instr"></a>InStr
**Leírás:**  
hello InStr függvény hello első előfordulásának a substring talál egy karakterláncban.

**Szintaxis:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

* stringcheck: a keresés toobe karakterlánc
* stringmatch: található toobe karakterlánc
* Start: pozíció toofind hello substring indítása
* Hasonlítsa össze: vbTextCompare vagy vbBinaryCompare

**Megjegyzés:**  
Hol található a hello substring értéket ad vissza hello pozíciója vagy 0, ha nem található.

**Példa**  
`InStr("hello quick brown fox","quick")`  
Evalues too5

`InStr("repEated","e",3,vbBinaryCompare)`  
Kiértékeli too7

- - -
### <a name="instrrev"></a>InStrRev
**Leírás:**  
hello InStrRev függvény karakterláncrész utolsó előfordulásának hello talál egy karakterláncban.

**Szintaxis:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

* stringcheck: a keresés toobe karakterlánc
* stringmatch: található toobe karakterlánc
* Start: pozíció toofind hello substring indítása
* Hasonlítsa össze: vbTextCompare vagy vbBinaryCompare

**Megjegyzés:**  
Hol található a hello substring értéket ad vissza hello pozíciója vagy 0, ha nem található.

**Példa**  
`InStrRev("abbcdbbbef","bb")`  
7 beolvasása

- - -
### <a name="isbitset"></a>IsBitSet
**Leírás:**  
hello függvény IsBitSet teszteket, ha egy kicsit van beállítva, vagy sem

**Szintaxis:**  
`bool IsBitSet(num value, num flag)`

* érték: egy numerikus értéket, amely evaluated.flag: egy numerikus érték, amely rendelkezik hello bit toobe kiértékelése

**Példa**  
`IsBitSet(&HF,4)`  
Igaz értéket ad vissza, mert a bit "4" hello hexadecimális érték: "F" be van állítva

- - -
### <a name="isdate"></a>IsDate
**Leírás:**  
Ha hello kifejezés lehet majd hello IsDate függvény kiértékeli tooTrue kiértékeli egy dátum/idő típusként.

**Szintaxis:**  
`bool IsDate(var Expression)`

**Megjegyzés:**  
Toodetermine használja, ha CDate() sikeres lehet.

- - -
### <a name="iscert"></a>IsCert
**Leírás:**  
Igaz értéket ad eredményül, ha a nyers adatok hello .NET X509Certificate2 tanúsítványobjektum lehet szerializálni.

**Szintaxis:**  
`bool CertThumbprint(binary certificateRawData)`  
*   certificateRawData: egy X.509 tanúsítvány bájt tömb ábrázolását. hello bájttömb (DER) kódolású bináris vagy Base64 kódolású X.509 adatok lehetnek.
- - -
### <a name="isempty"></a>Vagyis IsEmpty
**Leírás:**  
Ha hello attribútum hello CS vagy MV jelen, de üres karakterlánc tooan kiértékeli, vagyis IsEmpty függvény hello tooTrue értékeli ki.

**Szintaxis:**  
`bool IsEmpty(var Expression)`

- - -
### <a name="isguid"></a>IsGuid
**Leírás:**  
Ha hello karakterlánc lehet konvertált tooa GUID, hello IsGuid függvény tootrue értékeli ki.

**Szintaxis:**  
`bool IsGuid(str GUID)`

**Megjegyzés:**  
GUID egy ezeket a mintákat a következő karakterláncként van definiálva: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx vagy {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Toodetermine használja, ha CGuid() sikeres lehet.

**Példa**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Ha hello StrAttribute GUID formátuma, térjen vissza a bináris megjelenítése, ellenkező esetben adhat vissza Null.

- - -
### <a name="isnull"></a>IsNull
**Leírás:**  
Ha a kifejezés hello tooNull, majd hello IsNull függvény igaz értéket ad vissza.

**Szintaxis:**  
`bool IsNull(var Expression)`

**Megjegyzés:**  
Az attribútum egy Null hello hiányában hello attribútum által van kifejezve.

**Példa**  
`IsNull([displayName])`  
Igaz értéket ad vissza, ha nincs jelen hello CS vagy MV hello attribútum.

- - -
### <a name="isnullorempty"></a>IsNullOrEmpty
**Leírás:**  
Ha hello kifejezés null vagy üres karakterlánc, majd hello IsNullOrEmpty függvény igaz értéket ad vissza.

**Szintaxis:**  
`bool IsNullOrEmpty(var Expression)`

**Megjegyzés:**  
Az attribútum a értékelné ki tooTrue, ha hello attribútum hiányzik, vagy megtalálható, de üres karakterlánc.  
Ez a funkció hello inverzét IsPresent neve.

**Példa**  
`IsNullOrEmpty([displayName])`  
Igaz értéket ad vissza, ha hello attribútum nincs jelen vagy hello CS vagy MV üres karakterlánc.

- - -
### <a name="isnumeric"></a>IsNumeric
**Leírás:**  
hello IsNumeric függvény jelzi, hogy egy kifejezés kiértékelése számú típusként logikai értéket adja vissza.

**Szintaxis:**  
`bool IsNumeric(var Expression)`

**Megjegyzés:**  
Toodetermine használja, ha CNum() lehet sikeres tooparse hello kifejezés.

- - -
### <a name="isstring"></a>IsString
**Leírás:**  
Ha hello kifejezés is lehet kiértékelt tooa karakterlánc típusúnak, majd hello IsString függvény tooTrue értékelődik ki.

**Szintaxis:**  
`bool IsString(var expression)`

**Megjegyzés:**  
Toodetermine használja, ha CStr() lehet sikeres tooparse hello kifejezés.

- - -
### <a name="ispresent"></a>IsPresent
**Leírás:**  
Ha hello kifejezés értéke nem Null, és nem üres tooa karakterláncot, majd hello IsPresent függvény igaz értéket ad vissza.

**Szintaxis:**  
`bool IsPresent(var expression)`

**Megjegyzés:**  
Ez a funkció hello inverzét IsNullOrEmpty neve.

**Példa**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

- - -
### <a name="item"></a>Elem
**Leírás:**  
hello Item függvény egy elemet egy többértékű karakterlánc attribútum ad vissza.

**Szintaxis:**  
`var Item(mvstr attribute, num index)`

* attribútum: többértékű attribútum
* index: hello többértékű karakterlánc tooan elemének indexét.

**Megjegyzés:**  
hello Item függvény akkor hasznos, hello Contains függvény együtt, mivel ez utóbbi függvény hello hello többértékű attribútum hello index tooan elemét adja vissza.

Hibát jelez, ha az index az értéktartományon kívül esik.

**Példa**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Beolvasása hello elsődleges e-mail címét.

- - -
### <a name="itemornull"></a>ItemOrNull
**Leírás:**  
hello ItemOrNull függvény egy elemet egy többértékű karakterlánc attribútum ad vissza.

**Szintaxis:**  
`var ItemOrNull(mvstr attribute, num index)`

* attribútum: többértékű attribútum
* index: hello többértékű karakterlánc tooan elemének indexét.

**Megjegyzés:**  
hello ItemOrNull függvény akkor hasznos, hello Contains függvény együtt, mivel ez utóbbi függvény hello hello többértékű attribútum hello index tooan elemét adja vissza.

Ha az index az értéktartományon kívül esik, majd adja vissza Null értéket.

- - -
### <a name="join"></a>Csatlakozás
**Leírás:**  
hello illesztési függvényhez időt vesz igénybe a többértékű karakterlánc, és az egyes elemek között megadott elválasztó egyértékű karakterláncot ad vissza.

**Szintaxis:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

* attribútum: csatlakoztatott toobe többértékű attribútumot tartalmazó karakterláncok.
* elválasztó karakter: A karakterlánc, a használt tooseparate hello karakterláncrészletet tartalmazza a karakterláncot adott vissza hello. Ha nincs megadva, hello szóköz karakter ("") használatos. Ha elválasztó karakter egy nulla hosszúságú karakterlánc (""), illetve semmi sem, hello lista összes elemének nélkül határolójelek van kibővítve.

**Megjegyzések**  
Paritás hello illesztési és felosztott funkciók között van. hello illesztési függvényhez karakterláncok vesz igénybe, és csatlakoztatja őket egy elválasztó karakterlánccal, tooreturn egyetlen karakterlánc. hello vegyes függvény lép egy karakterláncot, és a hello elválasztó, tooreturn karakterláncok elválasztja azt. A fő különbség azonban, hogy csatlakozási karakterlánc bármely határoló karakterláncot is összefűzésére, vegyes csak elkülönítheti karakterláncok egy egyetlen karaktert elválasztó használatával.

**Példa**  
`Join([proxyAddresses],",")`  
Visszaadhatja: "SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

- - -
### <a name="lcase"></a>LCase
**Leírás:**  
hello LCase függvény alakítja az összes karaktert karakterlánc toolower esetben.

**Szintaxis:**  
`str LCase(str value)`

**Példa**  
`LCase("TeSt")`  
"Test" adja vissza.

- - -
### <a name="left"></a>balra
**Leírás:**  
hello bal függvény egy karakterlánc hello balról adja vissza a megadott számú karaktert.

**Szintaxis:**  
`str Left(str string, num NumChars)`

* karakterlánc: hello karakterlánc tooreturn karaktert tartalmaz
* NumChars: egy szám karakterek tooreturn elejétől hello (balra) karakterlánc hello száma

**Megjegyzés:**  
Hello első numChars karakterláncon belül karaktereket tartalmazó karakterláncot:

* Ha numChars = 0, térjen vissza az üres karakterlánc.
* Ha numChars < 0, térjen vissza a bemeneti karakterlánc.
* Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.

Ha karakterlánc numChars megadott hello számnál kevesebb karaktert tartalmaz, a karakterlánc azonos toostring (amely, 1. paraméter levő összes karakter) ad vissza.

**Példa**  
`Left("John Doe", 3)`  
"Joh" adja vissza.

- - -
### <a name="len"></a>Hossz
**Leírás:**  
hello Len függvény egy karakterlánc karakterek számát adja vissza.

**Szintaxis:**  
`num Len(str value)`

**Példa**  
`Len("John Doe")`  
8 beolvasása

- - -
### <a name="ltrim"></a>LTrim
**Leírás:**  
LTrim függvény hello kezdő szóközöket eltávolít egy karakterláncból.

**Szintaxis:**  
`str LTrim(str value)`

**Példa**  
`LTrim(" Test ")`  
"Test" értéket ad vissza

- - -
### <a name="mid"></a>Mid
**Leírás:**  
hello Mid függvény egy karakterlánc megadott helyéről a megadott számú karaktert adja vissza.

**Szintaxis:**  
`str Mid(str string, num start, num NumChars)`

* karakterlánc: hello karakterlánc tooreturn karaktert tartalmaz
* Indítsa el: egy karakterlánc tooreturn karaktert tartalmaz a kezdőpozíció hello azonosító szám
* NumChars: egy szám karakterek tooreturn karakterlánc helyéről hello száma

**Megjegyzés:**  
Indítsa el a pozíció visszatérési numChars karakterek karakterlánc.  
Pozíció indítás karakterláncban numChars karaktereket tartalmazó karakterláncot:

* Ha numChars = 0, térjen vissza az üres karakterlánc.
* Ha numChars < 0, térjen vissza a bemeneti karakterlánc.
* Ha start > hello karakterlánc hossza, térjen vissza a bemeneti karakterlánc.
* Ha start < = 0, térjen vissza a bemeneti karakterlánc.
* Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.

Ha nem numChar karakterek fennmaradó karakterlánc pozíció indítás, amit a lehető legtöbb karaktert ad vissza.

**Példa**  
`Mid("John Doe", 3, 5)`  
"Hn Do" adja vissza.

`Mid("John Doe", 6, 999)`  
Visszaadja a "Doe"

- - -
### <a name="now"></a>Most
**Leírás:**  
hello most függvény adja vissza egy dátum és idő megadása az aktuális dátum és idő, hello tooyour számítógép rendszer dátum és idő alapján történik.

**Szintaxis:**  
`dt Now()`

- - -
### <a name="numfromdate"></a>NumFromDate
**Leírás:**  
hello NumFromDate függvény dátumformátum AD meg egy dátumot adja vissza.

**Szintaxis:**  
`num NumFromDate(dt value)`

**Példa**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
129699324000000000 beolvasása

- - -
### <a name="padleft"></a>PadLeft
**Leírás:**  
hello PadLeft balra-kézi működik a megadott karakterlánc tooa használatával egy megadott Kitöltő karakter hosszúságú.

**Szintaxis:**  
`str PadLeft(str string, num length, str padCharacter)`

* karakterlánc: hello karakterlánc toopad.
* hossz: hello jelző egész számot szükségeskonfiguráció-karakterlánc hossza.
* padCharacter: egy egyetlen karaktert toouse hello kitöltő karakterként karakterlánc

**Megjegyzés:**

* Hello hosszúságú karakterlánc hossza kisebb, majd padCharacter akkor ismételten hozzáfűzött toohello kezdete (balra) karakterlánc amíg rá nem kényszerül hosszának egyenlőnek toolength.
* padCharacter lehet szóköz karakter, de nem lehet null értékű.
* Ha hello karakterlánc hossza nagyobb, mint egyenlő tooor, karakterlánc ad változatlan vissza.
* Ha a karakterlánc hossza nagyobb, mint vagy egyenlő toolength rendelkezik, egy karakterlánc azonos toostring ad vissza.
* Ha hello hosszúságú karakterlánc hossza kisebb, hello új karakterlánc szükséges, feltöltve egy padCharacter karakterláncot tartalmazó hosszát adja vissza.
* Karakterlánc értéke null, ha a hello függvény egy üres karakterláncot ad vissza.

**Példa**  
`PadLeft("User", 10, "0")`  
"000000User" adja vissza.

- - -
### <a name="padright"></a>PadRight
**Leírás:**  
hello PadRight jobb-kézi működik a megadott karakterlánc tooa használatával egy megadott Kitöltő karakter hosszúságú.

**Szintaxis:**  
`str PadRight(str string, num length, str padCharacter)`

* karakterlánc: hello karakterlánc toopad.
* hossz: hello jelző egész számot szükségeskonfiguráció-karakterlánc hossza.
* padCharacter: egy egyetlen karaktert toouse hello kitöltő karakterként karakterlánc

**Megjegyzés:**

* Hello hosszúságú karakterlánc hossza kisebb, majd padCharacter akkor ismételten hozzáfűzött toohello végét (jobb oldali) karakterlánc amíg rá nem kényszerül hosszának egyenlőnek toolength.
* padCharacter lehet szóköz karakter, de nem lehet null értékű.
* Ha hello karakterlánc hossza nagyobb, mint egyenlő tooor, karakterlánc ad változatlan vissza.
* Ha a karakterlánc hossza nagyobb, mint vagy egyenlő toolength rendelkezik, egy karakterlánc azonos toostring ad vissza.
* Ha hello hosszúságú karakterlánc hossza kisebb, hello új karakterlánc szükséges, feltöltve egy padCharacter karakterláncot tartalmazó hosszát adja vissza.
* Karakterlánc értéke null, ha a hello függvény egy üres karakterláncot ad vissza.

**Példa**  
`PadRight("User", 10, "0")`  
"User000000" adja vissza.

- - -
### <a name="pcase"></a>PCase
**Leírás:**  
hello PCase függvény konvertálja hello karakterlánc tooupper esetben minden szóközzel elválasztott szó első karaktere, és további karakterekkel alakítja a rendszer toolower eset.

**Szintaxis:**  
`String PCase(string)`

**Megjegyzés:**

* Ez a funkció jelenleg nem biztosít megfelelő kis-és tooconvert teljesen nagybetű, például az angol szót.

**Példa**  
`PCase("TEsT")`  
"Test" adja vissza.

`PCase(LCase("TEST"))`  
"Test" értéket ad vissza

- - -
### <a name="randomnum"></a>RandomNum
**Leírás:**  
hello RandomNum függvény között egy megadott időszakban egy véletlenszerű számot ad vissza.

**Szintaxis:**  
`num RandomNum(num start, num end)`

* Indítsa el: egy szám azonosító hello határértéke hello véletlenszerű értéket toogenerate
* vége: egy szám azonosító hello felső korlátja hello véletlenszerű értéket toogenerate

**Példa**  
`Random(100,999)`  
734 adhat vissza.

- - -
### <a name="removeduplicates"></a>RemoveDuplicates
**Leírás:**  
hello RemoveDuplicates függvény időt vesz igénybe a többértékű karakterlánc, és győződjön meg arról, hogy egyedi.

**Szintaxis:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Példa**  
`RemoveDuplicates([proxyAddresses])`  
Adja vissza egy megtisztított proxyAddress attribútumot, ahol minden ismétlődő értékek el lettek távolítva.

- - -
### <a name="replace"></a>Csere
**Leírás:**  
hello Replace függvény egy karakterlánc tooanother karakterlánc összes előfordulását lecseréli.

**Szintaxis:**  
`str Replace(str string, str OldValue, str NewValue)`

* karakterlánc: egy karakterlánc tooreplace értékei.
* OldValue: hello karakterlánc toosearch az és tooreplace.
* Új érték: hello karakterlánc tooreplace számára.

**Megjegyzés:**  
hello függvény felismeri a következő különleges monikerek hello:

* \n – új sor
* \r – kocsivissza
* \t – lap

**Példa**  
`Replace([address],"\r\n",", ")`  
CRLF lecseréli egy vesszőt és területet, és túl vezethet "Egy Microsoft módja, Redmond, WA, USA"

- - -
### <a name="replacechars"></a>ReplaceChars
**Leírás:**  
hello ReplaceChars függvény hello ReplacePattern karakterláncban található karakterek összes előfordulását lecseréli.

**Szintaxis:**  
`str ReplaceChars(str string, str ReplacePattern)`

* karakterlánc: egy karakterlánc tooreplace karakternél.
* ReplacePattern: egy szótár karakterek tooreplace tartalmazó karakterlánc.

hello formátuma {source1}: {target1}, {source2}: {target2}, {sourceN}, {targetN} hello karakter toofind és a cél hello karakterlánc tooreplace a forrás esetén.

**Megjegyzés:**

* hello függvény minden egyes előfordulásakor meghatározott források veszi és azok hello célokat.
* hello (unicode) pontosan egy karaktert kell lennie.
* hello forrás nem lehet üres vagy hosszabb, mint egy karakter (feldolgozási hiba).
* hello cél több karakter, például ö:oe, β:ss lehet.
* lehet, hogy hello tároló üres, amely azt jelzi, hogy hello karakter el kell távolítani.
* hello forrás kis-és nagybetűket, és pontosan egyeznie kell.
* Hello, (vessző) és: (kettőspont) fenntartott karakterek, és ez a funkció nem helyettesíthető.
* Tárolóhelyek és egyéb fehér karakterek hello ReplacePattern karakterlánc figyelmen kívül lesznek hagyva.

**Példa**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Raksmorgas adja vissza

`ReplaceChars("O’Neil",%ReplaceString%)`  
Vissza "ONeil" hello egyetlen osztásjelek definiált toobe eltávolítva.

- - -
### <a name="right"></a>Jobbra
**Leírás:**  
hello jobb függvény a megadott számú karaktert hello jobb (záró) karakterlánc távolságát adja vissza.

**Szintaxis:**  
`str Right(str string, num NumChars)`

* karakterlánc: hello karakterlánc tooreturn karaktert tartalmaz
* NumChars: egy szám karakterek tooreturn oldalától hello (jobb oldali) karakterlánc hello száma

**Megjegyzés:**  
NumChars karaktereket a rendszer karakterlánc hello utolsó pozícióját adja vissza.

Hello utolsó numChars karakterláncon belül karaktereket tartalmazó karakterláncot:

* Ha numChars = 0, térjen vissza az üres karakterlánc.
* Ha numChars < 0, térjen vissza a bemeneti karakterlánc.
* Ha karakterlánc null értékű, térjen vissza az üres karakterlánc.

Ha a karakterlánc karakternél kevesebb, mint a megadott számú NumChars hello, egy karakterlánc azonos toostring ad vissza.

**Példa**  
`Right("John Doe", 3)`  
"Doe" adja vissza.

- - -
### <a name="rtrim"></a>RTrim
**Leírás:**  
RTrim függvény hello záró szóközt eltávolít egy karakterláncból.

**Szintaxis:**  
`str RTrim(str value)`

**Példa**  
`RTrim(" Test ")`  
"Test" adja vissza.

- - -
### <a name="select"></a>Válassza ezt:
**Leírás:**  
A folyamat egy többértékű attribútum (vagy egy kifejezés eredményének) szereplő összes érték a megadott függvény alapján.

**Szintaxis:**  
`mvattr Select(variable item, mvattr attribute, func function)`  
`mvattr Select(variable item, exp expression, func function)`

* cikk: hello többértékű attribútumban olyan elemet
* attribútum: hello többértékű attribútum
* kifejezés: értékek gyűjteményét visszaadó kifejezés
* feltétel: semmilyen feladatot, amely képes a hello attribútum elem

**Példák:**  
`Select($item,[otherPhone],Replace($item,“-”,“”))`  
Összes hello érték visszaadása hello többértékű attribútum otherPhone után a kötőjeleket (-) el lett távolítva.

- - -
### <a name="split"></a>Vegyes
**Leírás:**  
hello vegyes függvény elválasztóval elválasztott karakterlánc vesz igénybe, és lehetővé teszi a többértékű karakterlánc.

**Szintaxis:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

* érték: karakterlánc, és egy elválasztó karakter tooseparate hello.
* elválasztó karakter: használt hello elválasztó karakter toobe egyetlen.
* korlát: értékeket adhat vissza maximális számát.

**Példa**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
2 elemek többértékű karakterláncot ad vissza hello proxyAddress attribútum használható.

- - -
### <a name="stringfromguid"></a>StringFromGuid
**Leírás:**  
hello StringFromGuid függvény bináris GUID tart, és konvertálja azt tooa karakterlánc

**Szintaxis:**  
`str StringFromGuid(bin GUID)`

- - -
### <a name="stringfromsid"></a>StringFromSid
**Leírás:**  
hello StringFromSid függvény egy karakterlánc-egy biztonsági azonosítót tooa tartalmazó bájttömb alakítja át.

**Szintaxis:**  
`str StringFromSid(bin ObjectSID)`  

- - -
### <a name="switch"></a>Kapcsoló
**Leírás:**  
hello kapcsoló függvény használt tooreturn értékelt feltételek alapján egyetlen értéket.

**Szintaxis:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

* kifejezés: tooevaluate kívánt Variant kifejezést.
* érték: érték toobe visszaadott hello megfelelő kifejezés értéke igaz, ha.

**Megjegyzés:**  
hello kapcsoló függvényargumentum lista kifejezések és érték párokból áll. a bal oldali tooright hello kifejezések kiértékelése és hello első kifejezés tooevaluate tooTrue társított hello értéket adja vissza. Ha nincsenek megfelelően párosítva a hello részeit, egy futásidejű hiba történik.

Például Kif1 értéke igaz, ha a kapcsoló érték1 adja vissza. Ha kifejezés-1 érték hamis, de kifejezés-2 értéke igaz, kapcsoló érték-2, és így tovább adja vissza.

Kapcsoló adja vissza egy Nothing ha:

* Hello kifejezések egyike sem igaz.
* hello első igaz kifejezés a megfelelő értékkel rendelkezik, amely null értékű.

Kapcsoló kiértékeli az összes kifejezést, annak ellenére, hogy csak az egyiket adja vissza. Emiatt érdemes figyelemmel a nemkívánatos hatásai. Például egy nullával való osztást bármely kifejezés kiértékelése hello eredményez, ha hiba lép fel.

Érték hello hiba függvénynek, amely akkor adja vissza egyéni is lehet.

**Példa**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Néhány főbb városában szóbeli hello nyelvét adja eredményül, ellenkező esetben a hibát ad vissza.

- - -
### <a name="trim"></a>Trim
**Leírás:**  
hello Trim függvény eltávolítja a kezdő és záró szóközök karakterláncból.

**Szintaxis:**  
`str Trim(str value)`  

**Példa**  
`Trim(" Test ")`  
"Test" adja vissza.

`Trim([proxyAddresses])`  
Eltávolítja a kezdő és záró szóközök hello proxyAddress attribútum szereplő összes értékhez.

- - -
### <a name="ucase"></a>UCase
**Leírás:**  
hello UCase függvény alakítja az összes karaktert karakterlánc tooupper esetben.

**Szintaxis:**  
`str UCase(str string)`

**Példa**  
`UCase("TeSt")`  
"TEST" adja vissza.

- - -
### <a name="where"></a>Ha

**Leírás:**  
A megadott feltétel alapján többértékű attribútum (vagy egy kifejezés eredményének) értékek egy alhalmazát adja vissza.

**Szintaxis:**  
`mvattr Where(variable item, mvattr attribute, exp condition)`  
`mvattr Where(variable item, exp expression, exp condition)`  
* cikk: hello többértékű attribútumban olyan elemet
* attribútum: hello többértékű attribútum
* feltétel: bármely, amely kiértékeli tootrue vagy HAMIS eredményt ad
* kifejezés: értékek gyűjteményét visszaadó kifejezés

**Példa**  
`Where($item,[userCertificate],CertNotAfter($item)>Now())`  
Hello többértékű attribútum userCertificate nem lejárt hello tanúsítvány értékeket adja vissza.

- - -
### <a name="with"></a>a
**Leírás:**  
hello funkcióval tartalmaz egy módja toosimplify összetett kifejezést egy változó toorepresent egy alkifejezés, amely akkor jelenik meg egy vagy több alkalommal hello összetett kifejezésben.

**Szintaxis:**
`With(var variable, exp subExpression, exp complexExpression)`  
* változó: jelöli hello alkifejezés.
* alkifejezés: változó által képviselt alkifejezés.
* complexExpression: egy összetett kifejezés.

**Példa**  
`With($unExpiredCerts,Where($item,[userCertificate],CertNotAfter($item)>Now()),IIF(Count($unExpiredCerts)>0,$unExpiredCerts,NULL))`  
Mint:  
`IIF (Count(Where($item,[userCertificate],CertNotAfter($item)>Now()))>0, Where($item,[userCertificate],CertNotAfter($item)>Now()),NULL)`  
Amely hello userCertificate attribútum csak leküldéshez tanúsítvány értékeket adja vissza.


- - -
### <a name="word"></a>Word
**Leírás:**  
hello Word függvény egy karakterlánc, hello határolójelek toouse és hello word számú tooreturn leíró paraméterek alapján belül található szót adja vissza.

**Szintaxis:**  
`str Word(str string, num WordNumber, str delimiters)`

* karakterlánc: hello karakterlánc tooreturn a szót.
* WordNumber: egy szám mely word számot kell visszaadnia.
* elválasztó karaktert: egy karakterlánc, amely hello delimiter(s), hogy által használt tooidentify szavakat

**Megjegyzés:**  
A határoló karakter hello egyik hello elválasztott karakterlánc minden karakterlánc szavak azonosítja:

* Ha < 1-es számú, értéket ad vissza üres karakterlánc.
* Ha a karakterlánc értéke null, üres karakterláncot ad vissza.

Karakterlánc nagyobb számú karaktert tartalmaz, vagy karakterlánca nem tartalmazza az elválasztó karaktert által azonosított szavak, ha üres karakterláncot ad vissza.

**Példa**  
`Word("hello quick brown fox",3," ")`  
Visszaadja a "nem"

`Word("This,string!has&many separators",3,",!&#")`  
Alakítanák vissza "tartalmaz"

## <a name="additional-resources"></a>További források
* [Deklaratív kiépítés kifejezéseinek ismertetése](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
