---
title: "Hibrid kapcsolatok aaaAzure protokoll útmutató |} Microsoft Docs"
description: "Az Azure hibrid kapcsolatok protokoll útmutató."
services: service-bus-relay
documentationcenter: na
author: clemensv
manager: timlt
editor: 
ms.assetid: 149f980c-3702-4805-8069-5321275bc3e8
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: sethm;clemensv
ms.openlocfilehash: 2d145d919d606ae4722b063e1baf39fb845a600a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Az Azure hibrid kapcsolatok protokoll
Azure továbbítási hello Azure Service Bus platform hello képesség kulcsfontosságú oszlopok egyike. új hello *hibrid kapcsolatok* továbbító egy biztonságos, nyílt-protokoll alakulása a HTTP és a websocket elemek alapján. Az előző változat hello volt, ugyanilyen nevű *BizTalk szolgáltatások* funkciója, amely a saját fejlesztésű protokollja alaprendszert lett létrehozva. hello integrálása a hibrid kapcsolatok Azure App Service szolgáltatások továbbra is mint toofunction-van.

Hibrid kapcsolatok lehetővé teszi, hogy kétirányú, bináris adatfolyam közötti kommunikáció során, ami esetleg mindkét félnek elhelyezkedhetnek NAT vagy tűzfal mögé két hálózati alkalmazások. Ez a cikk ismerteti a hello ügyféloldali interakció hello hibrid kapcsolatok továbbítási csatlakozó ügyfeleken figyelő küldő szerepkörök és hogyan figyelői új kapcsolatok fogadásához.

## Interakció modell
hello hibrid kapcsolatok továbbító csatlakozik a két fél hello Azure felhőben, amelyek mind a felek felderíteni, és a csatlakozás toofrom a saját hálózati szempontból egy szinkronizálási pont megadásával. A szinkronizálási pont "Hibrid kapcsolat" Ebben és egyéb dokumentációt neve hello API-k, valamint hello Azure-portálon. hello szolgáltatás végpontjának értéke hibrid kapcsolatok említett tooas hello "szolgáltatás" hello többi cikkben. hello interakció modell leans a sok más hálózati API-k által létrehozott hello elnevezéseket.

Egy figyelő, amely először azt jelzi, hogy készültségi toohandle bejövő kapcsolatokat, és ezt követően fogadja el őket, akkor van. A másik oldalon hello, egy kapcsolódó ügyféloldali, amely a hello figyelő vár, hogy fogadja el a kétirányú kommunikáció elérési létrehozó kapcsolat toobe felé.
"Csatlakozás," "Figyelés," és "Elfogadása" vannak hello azonos feltételek megtalálta a legtöbb szoftvercsatorna API-k.

A továbbítón keresztüli kommunikáció modellnek vagy fél kimenő kapcsolatok felé egy végpontot, amely lehetővé teszi hello "figyelő" is "ügyfél" köznyelvi használja, és más terminológiát túlterhelések okozhat. ezért használjuk a hibrid kapcsolatok hello pontos terminológia a következőképpen történik:

a kapcsolat mindkét oldalán hello programok nevezzük "ügyfél", mert azok az ügyfelek toohello szolgáltatás. hello ügyfél, amely megvárja, és elfogadja a kapcsolatok "a figyelő", vagy említett toobe szerepkörben hello"figyelőt." hello szolgáltatáson keresztül figyelő felé új kapcsolatot kezdeményez hello ügyfél elnevezése "feladó" hello, vagy a "feladó szerepkör."

### Figyelő interakciók
hello figyelő rendelkezik hello szolgáltatás; négy interakciók a cikk későbbi részében hello hivatkozás szakaszban összes átviteli részletes ismerteti.

#### Figyelés
tooindicate készültségi toohello szolgáltatás, hogy egy figyelő kész tooaccept kapcsolatok, létrehoz egy kimenő WebSocket-kapcsolat. hello kapcsolati kézfogás hordoz magában, ha egy hibrid kapcsolat konfigurálja a hello továbbítási névteret, és egy biztonsági jogkivonatot, amely közvetlenül a ruház hello "Figyelés" nevet hello nevére.
Hello WebSocket hello szolgáltatás által elfogadható, ha hello regisztrálása sikeresen befejeződött, és hello létrehozott WebSocket tartják életben hello "vezérlőcsatorna" minden későbbi kapcsolatok engedélyezésére, webes. hello szolgáltatás lehetővé teszi, hogy egy hibrid kapcsolat egyidejű figyelőinek too25 fel. Ha két vagy több aktív figyelők, a bejövő kapcsolatok elosztását mindegyik véletlenszerű sorrendben; igazságos elosztási nem garantált.

#### Fogadja el
Egy küldő hello szolgáltatásban egy új kapcsolat megnyitása után hello szolgáltatást választ, és értesítést küld egy hibrid kapcsolat hello aktív figyelőinek hello. Az értesítés átküldött toohello figyelő hello nyitott vezérlőcsatorna tartalmazó hello URL-CÍMÉT, amely figyelő hello hello WebSocket végpont kapcsolódnia kell a hello kapcsolat elfogadása toofor JSON üzenetben.

hello URL-CÍMÉT is, és közvetlenül hello figyelő extra munka nélkül kell használniuk.
hello kódolt információ csak a hello küldő rövid idő alatt, lényegében ideig érvényes hajlandó toowait hello kapcsolat létrejött toobe-végpontok, de másolatot tooa legfeljebb 30 másodpercig. hello URL-cím csak egy sikeres kapcsolódási kísérlet portbesorolása. Amint hello hello szinkronizálási URL-cím jön létre, minden további tevékenység a WebSocket-kapcsolatot továbbítása a WebSocket és toohello küldő hello szolgáltatás értelmezése és beavatkozás nélkül.

#### Frissítés
hello biztonsági jogkivonatban foglalt tooregister hello figyelő kell és karbantartása a vezérlőcsatorna is lejár, amíg aktív hello figyelő. hello jogkivonat lejáratának nem befolyásolja a folyamatban lévő kapcsolatok, de ennek következtében a hello vezérlési csatorna toobe dobja el, illetve hamarosan lejáró hello pillanattól után hello szolgáltatást. hello "megújítása" művelet, egy JSON-üzenetet, amely figyelő hello küldhet tooreplace hello token társított hello vezérlőcsatorna, így hello vezérlőcsatorna tarthatjuk fenn hosszabb ideig.

#### Ping
Ha hello vezérlőcsatorna tétlen marad hosszú ideig futott, közvetítők hello módon, a betöltés például terheléselosztó vagy NAT dobhatja hello TCP-kapcsolatot. hello "ping" művelet ezzel elkerülheti, amelyiknek hello csatornán, és figyelmeztet mindenki hello hálózati útvonal kisebb mennyiségű adatot küldött a hello kapcsolatra életben toobe, és egy "élő" tesztet hello figyelője is működik. Hello pingelés nem sikerül, ha hello vezérlőcsatorna érdemes figyelembe venni használható, és hello figyelő elméletileg újracsatlakozik.

### Küldő interakció
csak akkor van hello szolgáltatás egyetlen interakcióba hello küldő: csatlakozik.

#### Kapcsolódás
hello "Csatlakozás" művelet megnyitja hello szolgáltatásban, a hibrid kapcsolat hello és (ha szükséges, de alapértelmezés szerint) olyan hello neve WebSocket hello lekérdezési karakterlánc a "Send" engedélyt biztosító biztonsági jogkivonatot. hello szolgáltatás majd hello figyelője eltérő módon az előzőekben leírt hello kommunikál, és hello figyelő kapcsolatot hoz létre szinkronizálási, amely a WebSocket kapcsolódik. Hello WebSocket elfogadását követően az adott WebSocket minden további interakciók vannak egy csatlakoztatott figyelő.

### Interakció összefoglaló
hello a interakció modell eredménye hello küldő ügyfél kívül a "tiszta" WebSocket, amelyek csatlakoztatott tooa figyelő, és, hogy nincs további preambulumok vagy előkészítési kézfogást származik. Ez a modell úgy, hogy megadja a megfelelően összeállított URL-címet a WebSocket ügyfél rétegbe lehetővé teszi gyakorlatilag az összes meglévő WebSocket ügyfél megvalósítása tooreadily előnyeit hello hibrid kapcsolatok szolgáltatás.

hello szinkronizálási kapcsolat figyelő hello WebSocket beolvassa a elfogadás interakció keresztül is tiszta és tooany meglévő WebSocket server végrehajtása néhány minimális extra absztrakciós, amelyik megkülönbözteti az "elfogadás" között lehet adni a keretrendszer helyi hálózati figyelők és a hibrid kapcsolatok távoli műveletek "elfogadása" műveletek.

## Protokoll referenciája

Ez a szakasz ismerteti a fentiekben ismertetett hello protokoll kapcsolati hello részleteit.

Az összes WebSocket-kapcsolat készült a 443-as porton frissítéseként HTTPS 1.1, amelyeket gyakran néhány WebSocket keretrendszer vagy API-val. A leírását tartják megvalósítási semleges, anélkül, hogy egy adott keretrendszert javasolhat.

### Figyelő protokoll
hello figyelőprotokoll két kapcsolat kézmozdulatok és három üzenetművelet áll.

#### Figyelő vezérlő adatcsatorna-kapcsolatot
hello vezérlőcsatorna, ahol a WebSocket-kapcsolat létrehozása:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...[&sb-hc-id=...]&sb-hc-token=...
```

Hello `namespace-address` hello teljesen minősített tartományneve, hogy a gazdagépek a hibrid kapcsolat, általában az űrlap hello hello hello Azure továbbítási névtér `{myname}.servicebus.windows.net`.

hello lekérdezési karakterlánc paraméter lehetőségek a következők:

| Paraméter | Szükséges | Leírás |
| --- | --- | --- |
| `sb-hc-action` |Igen |Hello figyelő szerepkör hello paraméternek kell lennie **sb-hc-művelet figyelési =** |
| `{path}` |Igen |hello URL-kódolású névtér elérési útja hello előre konfigurált a hibrid kapcsolat tooregister Ez a figyelő a. Ebben a kifejezésben rögzített hozzáfűzött toohello `$hc/` elérési út részével. |
| `sb-hc-token` |igen\* |hello figyelő adjon meg egy érvényes, az URL-kódolású Service Bus megosztott hozzáférési jogkivonat hello névtér vagy a hibrid kapcsolat, amely hello ruház **figyelésére** jobb. |
| `sb-hc-id` |Nem |Az ügyfél által megadott választható azonosítója lehetővé teszi, hogy a végpont a diagnosztikai nyomkövetés. |

Hello WebSocket-kapcsolat toohello a hibrid kapcsolat elérési út nem regisztrált, vagy egy érvénytelen vagy hiányzó jogkivonatot, vagy valamilyen egyéb hiba miatt nem sikerül, ha hello hiba visszajelzés érkezett hello rendszeres HTTP 1.1 állapot visszajelzés modell használatával. Az állapot leírása hiba követési-azonosítót tartalmaz, amely az Azure támogatási személyzet tájékoztatni lehet:

| Kód | Hiba | Leírás |
| --- | --- | --- |
| 404 |Nem található |hello hibrid kapcsolat elérési út érvénytelen, vagy hello alap URL-cím formátuma hibás. |
| 401 |Nem engedélyezett |hello biztonsági token hiányzik vagy helytelen formátumú vagy érvénytelen. |
| 403 |Tiltott |a művelet ehhez az elérési úthoz hello biztonsági jogkivonat érvénytelen. |
| 500 |Belső hiba |Valami hiba történt a hello szolgáltatásban. |

Ha a WebSocket-kapcsolat hello szándékosan állítja le hello szolgáltatás után kezdetben hozták létre, így a információcserére kerül sor, használja a megfelelő WebSocket protokoll hibakódot együtt egy részletes hibaüzenet, amely is nyomon hello oka AZONOSÍTÓJÁT. hello szolgáltatás nem nélkül hibaállapotot észlelt a vezérlőcsatorna leáll. A tiszta Leállítás utáni a felügyelt ügyfél.

| WS-állapot | Leírás |
| --- | --- |
| 1001 |hello hibrid kapcsolati útvonal törölve lett vagy le van tiltva. |
| 1008 |hello biztonsági jogkivonat érvényessége lejárt, ezért hello engedélyezési házirend sérül. |
| 1011 |Valami hiba történt a hello szolgáltatásban. |

### Fogadja el a kézfogás
hello "elfogadása" értesítésküldés hello szolgáltatás toohello figyelő a korábban létrehozott vezérlő csatornán WebSocket szöveg keretben JSON üzenetben. Nincs válasz toothis üzenet.

üdvözlőüzenetére JSON objektumot tartalmaz "elfogadás" nevű meghatározó hello jelenleg következő tulajdonságai:

* **cím** – hello WebSocket toothe szolgáltatás tooaccept bejövő kapcsolat létrehozásához használt URL-cím karakterlánc toobe hello.
* **azonosító** – hello egyedi azonosítót ehhez a kapcsolathoz. Hello azonosító hello küldő ügyfél által biztosított, ha hello küldő megadott érték, különben a rendszer által létrehozott értéket.
* **connectHeaders** – volt HTTP-fejlécek megadott toohello továbbítási végpont hello feladó, amely magában foglalja a hello mp-WebSocket-protokoll és az mp-WebSocket-bővítmények fejlécekkel együtt.

#### Üzenet elfogadása

```json
{                                                           
    "accept" : {
        "address" : "wss://168.61.148.205:443/$hc/{path}?..."    
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736",  
        "connectHeaders" : {                                         
            "Host" : "...",                                                
            "Sec-WebSocket-Protocol" : "...",                              
            "Sec-WebSocket-Extensions" : "..."                             
        }                                                            
     }                                                         
}
```

hello cím URL-cím megadott hello a JSON-üzenet hello figyelő létrehozásához használt WebSocket hello elfogadása vagy elutasítása hello küldő szoftvercsatorna.

#### Hello szoftvercsatorna elfogadásakor
tooaccept, hello figyelő WebSocket-kapcsolat toohello megadott címnek hoz létre.

Ha hello "elfogadása" üzenet rendelkezik egy `Sec-WebSocket-Protocol` fejléc, várható, hogy hello figyelő csak fogad hello WebSocket, ha az támogatja-e a protokoll. Továbbá azt állítja be hello fejléc hello WebSocket jön létre.

hello Ugyanez vonatkozik toohello `Sec-WebSocket-Extensions` fejléc. Ha hello keretrendszer támogatja a kiterjesztéssel, hello fejléc toohello kiszolgálóoldali válasz szükséges hello kell beállítva `Sec-WebSocket-Extensions` kézfogás hello bővítmény.

hello URL-címet kell használni, mint-hello létrehozó a szoftvercsatorna elfogadja, de tartalmazza a következő paramétereket:

| Paraméter | Szükséges | Leírás |
| --- | --- | --- |
| `sb-hc-action` |Igen |A szoftvercsatorna elfogadásakor, hello paraméternek kell lennie`sb-hc-action=accept` |
| `{path}` |Igen |(lásd a következő bekezdés hello) |
| `sb-hc-id` |Nem |Tekintse meg az előző leírása **azonosító**. |

`{path}`az URL-kódolású névtér elérési hello hello előre konfigurált mely tooregister a hibrid kapcsolat ennél a figyelőnél. Ebben a kifejezésben rögzített hozzáfűzött toothe `$hc/` elérési út részével. 

Hello `path` kifejezés ki, amelynek utótag és hello regisztrált név elválasztó perjellel után a következő lekérdezési karakterlánc-kifejezés. Ez lehetővé teszi az hello küldő ügyfél toopass feladó argumentumok toohello átvevő figyelő, ha az nem lehetséges tooinclude HTTP-fejlécek. hello általános gyakorlat, hogy hello figyelő keretrendszer elemez hello rögzített elérési út részével és a regisztrált név hello elérési úton található, és teszi hello fennmaradó, valószínűleg által meghatározott lekérdezési karakterlánc argumentum nélkül `sb-`, rendelkezésre álló toohello alkalmazás arról dönt, hogy tooaccept hello kapcsolat.

További információkért lásd: "Küldő protokoll" szakaszt a következő hello.

Ha nem sikerül, hello szolgáltatás válaszolhatnak az alábbiak szerint:

| Kód | Hiba | Leírás |
| --- | --- | --- |
| 403 |Tiltott |hello URL-cím érvénytelen. |
| 500 |Belső hiba |Hiba történt a hello szolgáltatásban |

Hello kapcsolat létrejötte után hello kiszolgáló leáll hello WebSocket, amikor hello küldő WebSocket kikapcsolják, vagy az állapot a következő hello:

| WS-állapot | Leírás |
| --- | --- |
| 1001 |hello küldő ügyfél hello kapcsolat leáll. |
| 1001 |hello hibrid kapcsolati útvonal törölve lett vagy le van tiltva. |
| 1008 |hello biztonsági jogkivonat érvényessége lejárt, ezért hello engedélyezési házirend sérül. |
| 1011 |Valami hiba történt a hello szolgáltatásban. |

#### Hello szoftvercsatorna elutasítása
Hello szoftvercsatorna visszautasítja a ellenőrző "elfogadása" üdvözlőüzenetére szükséges egy hasonló kézfogás, amelyen hello állapotkódot és a kommunikáció hello elutasítás oka áramolhasson az állapot leírása toohello küldő után.

hello protokoll tervezési szempontokat Itt a WebSocket-kézfogás (Ez egy meghatározott hibaállapotban tervezett tooend) toouse figyelő ügyfélmegvalósítások toorely folytatódhat a WebSocket ügyfél, és nem alkalmaz egy további, a HTTP-alapú operációs rendszer szükséges.

tooreject hello szoftvercsatorna hello ügyfél URI-cím hello hello "elfogadása" üzenetének vesz igénybe, és hozzáfűzi a két lekérdezési karakterlánc paraméter tooit, az alábbiak szerint:

| Param | Szükséges | Leírás |
| --- | --- | --- |
| statusCode |Igen |Numerikus HTTP-állapotkódot. |
| StatusDescription |Igen |Hello elutasítás emberi olvasható oka. |

majd az eredményül kapott URI hello tooestablish WebSocket-kapcsolat használni.

Megfelelően befejezése a kézfogás szándékosan sikertelen lesz, hibakódú HTTP 410, mivel nem WebSocket létrehozása. Ha valamilyen hiba, a következő kód hello hello hiba mutatják be:

| Kód | Hiba | Leírás |
| --- | --- | --- |
| 403 |Tiltott |hello URL-cím érvénytelen. |
| 500 |Belső hiba |Valami hiba történt a hello szolgáltatásban. |

### Figyelő jogkivonat megújítási
Ha hello figyelő token tooexpire kapcsolatos, az lecserélheti keret üzenet toohello szolgáltatás létrehozott hello vezérlő csatornán keresztül küldött. Az üzenet tartalmaz egy JSON-objektum neve `renewToken`, amely megadja, hogy hello tulajdonságot jelenleg a következő:

* **token** – egy érvényes, az URL-kódolású Service Bus megosztott hozzáférési jogkivonat a névtér vagy a hibrid kapcsolat, amely hello ruház **figyelésére** jobb.

#### renewToken üzenet

```json
{                                                                                                                                                                        
    "renewToken" : {                                                                                                                                                      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fhyco%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"  
    }                                                                                                                                                                     
}
```

Ha hello jogkivonatok érvényesség-ellenőrzése sikertelen, a hozzáférés megtagadva, és hello felhőalapú szolgáltatás bezárja hello vezérlőcsatorna WebSocket hiba történt. Ellenkező esetben nincs válasz.

| WS-állapot | Leírás |
| --- | --- |
| 1008 |hello biztonsági jogkivonat érvényessége lejárt, ezért hello engedélyezési házirend sérül. |

## Küldő protokoll
hello küldő protokoll módja a gyakorlatilag azonos toohello létrejön egy figyelő.
hello célja hello-végpontok közötti maximális átlátszóságának WebSocket. hello cím hello figyelő, de a "hello"action"ugyanúgy eltér toois hello és a token csatlakozni egy másik engedélyre van szüksége:

```
wss://{namespace-address}/$hc/{path}?sb-hc-action=...&sb-hc-id=...&sbc-hc-token=...
```

Hello *névtér-cím* hello teljesen minősített tartományneve, hogy a gazdagépek a hibrid kapcsolat, általában az űrlap hello hello hello Azure továbbítási névtér `{myname}.servicebus.windows.net`.

hello kérelem tartalmazhat tetszőleges extra HTTP-fejlécek található, beleértve az alkalmazás által meghatározott néhányat a meglévők közül. Az összes megadott fejlécek folyamat toohello figyelő és hello található `connectHeader` hello objektum **elfogadása** felügyeletének üzenetében.

hello lekérdezési karakterlánc paraméter lehetőségek a következők:

| Param | Kötelező? | Leírás |
| --- | --- | --- |
| `sb-hc-action` |Igen |Hello küldő szerepkörhöz, hello paramétert kell `action=connect`. |
| `{path}` |Igen |(lásd a következő bekezdés hello) |
| `sb-hc-token` |igen\* |hello figyelő adjon meg egy érvényes, az URL-kódolású Service Bus megosztott hozzáférési jogkivonat hello névtér vagy a hibrid kapcsolat, amely hello ruház **küldése** jobb. |
| `sb-hc-id` |Nem |Egy nem kötelező Azonosítót, amely lehetővé teszi, hogy a végpont a diagnosztikai nyomkövetés és történik a rendelkezésre álló toohello figyelő során hello kézfogás fogadja el. |

Hello `{path}` hello URL-kódolású névtér elérési útja hello előre konfigurált mely tooregister a hibrid kapcsolat ennél a figyelőnél. Hello `path` kifejezés egy utótagot és a lekérdezési karakterlánc kifejezés toocommunicate tovább bővíthető. Ha a hibrid kapcsolat hello regisztrálva van hello elérési úton található `hyco`, hello `path` kifejezés lehet `hyco/suffix?param=value&...` hello lekérdezési karakterlánc paramétereket az itt megadott követ. Egy teljes kifejezést majd a következő lehet:

```
wss://{namespace-address}/$hc/hyco/suffix?param=value&sb-hc-action=...[&sb-hc-id=...&]sbc-hc-token=...
```

Hello `path` hello címben szereplő "elfogadása" vezérlő üdvözlőüzenetére URI toohello figyelő továbbítja a kifejezésben.

Hello WebSocket-kapcsolat toohello a hibrid kapcsolat elérési út nem regisztrált, egy érvénytelen vagy hiányzó tokent vagy valamilyen egyéb hiba miatt nem sikerül, ha hello hiba visszajelzés érkezett hello rendszeres HTTP 1.1 állapot visszajelzés modell használatával. Az állapot leírása tartalmazza nyomon követését a továbbíthatók azonosító az Azure támogatási személyzet hiba:

| Kód | Hiba | Leírás |
| --- | --- | --- |
| 404 |Nem található |hello hibrid kapcsolat elérési út érvénytelen, vagy hello alap URL-cím formátuma hibás. |
| 401 |Nem engedélyezett |hello biztonsági token hiányzik vagy helytelen formátumú vagy érvénytelen. |
| 403 |Tiltott |hello biztonsági token érvénytelen ehhez az elérési úthoz, és ehhez a művelethez. |
| 500 |Belső hiba |Valami hiba történt a hello szolgáltatásban. |

Ha hello WebSocket-kapcsolat szándékosan hello szolgáltatás azt eredetileg beállítása után állítsa le, ezzel hello okát úgy információcserére kerül sor, használatával egy megfelelő WebSocket protokoll hiba kódja együtt egy részletes hibaüzenet, amely a is egy nyomkövetési azonosítót.

| WS-állapot | Leírás |
| --- | --- |
| 1000 |hello figyelő hello szoftvercsatorna leállítása. |
| 1001 |hello hibrid kapcsolati útvonal törölve lett vagy le van tiltva. |
| 1008 |hello biztonsági jogkivonat érvényessége lejárt, ezért hello engedélyezési házirend sérül. |
| 1011 |Valami hiba történt a hello szolgáltatásban. |

## Következő lépések
* [Relay – gyakori kérdések](relay-faq.md)
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés a .NET-tel](relay-hybrid-connections-dotnet-get-started.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

