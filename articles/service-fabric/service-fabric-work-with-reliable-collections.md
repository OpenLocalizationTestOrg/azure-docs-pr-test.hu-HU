---
title: "Megbízható gyűjteményekkel aaaWorking |} Microsoft Docs"
description: "Ismerje meg, hogy hello gyakorlati tanácsok a megbízható gyűjtemények használata."
services: service-fabric
documentationcenter: .net
author: rajak
manager: timlt
editor: 
ms.assetid: 39e0cd6b-32c4-4b97-bbcf-33dad93dcad1
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/19/2017
ms.author: rajak
ms.openlocfilehash: 41ba0b257da8493c1fc2e99ad7565593dc7cbcce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-reliable-collections"></a>Megbízható gyűjtemények használata
A Service Fabric egy állapotfüggő programozási modell elérhető too.NET fejlesztők keresztül megbízható gyűjtemények kínál. Pontosabban a Service Fabric megbízható szótár és megbízható várólista osztályok biztosít. Ha használja ezeket az osztályokat, az állapot (méretezhetőségre) particionálva, replikálni (a rendelkezésre állás érdekében), és egy partíciót (ACID szemantikáját) belül. Most egy tipikus használati megbízható dictionary objektum tekintse meg, és tekintse meg, milyen a végrehajtása.

```csharp

///retry:

try {
   // Create a new Transaction object for this partition
   using (ITransaction tx = base.StateManager.CreateTransaction()) {
      // AddAsync takes key's write lock; if >4 secs, TimeoutException
      // Key & value put in temp dictionary (read your own writes),
      // serialized, redo/undo record is logged & sent to
      // secondary replicas
      await m_dic.AddAsync(tx, key, value, cancellationToken);

      // CommitAsync sends Commit record toolog & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record toolog & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

Összes művelet (kivéve a ClearAsync pedig nem vonható vissza), a megbízható szótár objektumokon ITransaction objektum szükséges. Ez az objektum van társítva toomake tooany megbízható szótár és/vagy megbízható várólista objektumok belül egyetlen partícióra végrehajtani kívánt bármely és az összes módosítása. Egy ITransaction szerez be objektum hello partíciós függvény meghívásával tartozó StateManager tartozó CreateTransaction metódust.

A fenti hello kódban hello ITransaction objektum tooa megbízható szótár AddAsync metódus lett átadva. Belső a szótár módszerek, amely fogad egy kulcs egy olvasási/írási zárolás hello kulcshoz tartozó igénybe vehet. Hello metódus hello kulcs-érték módosítja, ha hello metódus hello kulcs zár lép, és ha hello metódus csak olvassa be az hello kulcs értékét, majd olvasási zárolás szükséges hello kulcs. Mivel AddAsync hello kulcs-érték toohello új módosítja, átadott érték hello kulcs írási zárolás használatban van. Igen, 2 (vagy több) szálak megpróbálnak hello tooadd értékek ugyanaz a kulcs: hello azonos időben, egy szál hello írási zárolás fogják beszerezni és hello más szálak blokkolja. Alapértelmezés szerint az too4 másodperc tooacquire hello zárat; blokkja módszerek 4 másodperc múlva hello módszerek throw egy TimeoutException. Módszer túlterhelések található így Ön toopass egy explicit időtúllépési értéket, ha szeretne.

A kód tooreact tooa TimeoutException általában, valamint rögzíti és hello teljes művelet megkísérlése (ahogy a fenti hello kódot) írható. Egyszerű kód Task.Delay átadásakor 100 milliomod másodperc minden alkalommal, amikor csak hívandó. De a valóságban, valószínűleg jobb, ha valamilyen exponenciális vissza az indító késleltetés használja helyette.

Miután hello zárolási keletkezik, AddAsync hello kulcs hozzáadása, és érték objektum tooan belső ideiglenes szótár hello ITransaction objektumhoz rendelt hivatkozik. Ebben az esetben tooprovide, olvasási-a-saját-írási műveletek szemantikájú. Ez azt jelenti, hogy követően meghívja a AddAsync, egy újabb hívás tooTryGetValueAsync (azonos ITransaction objektum használatával hello) hello értéket adja vissza, akkor is, ha még nem véglegesített hello tranzakció. A következő AddAsync rendezi sorba a kulcsot és értéket toobyte tömbök objektumokat, és hozzáfűzi a byte tömbök tooa naplófájl hello helyi csomóponton. Végezetül AddAsync küldi hello bájt tömbök tooall hello másodlagos replika, azonos rendelkezik hello kulcs/érték információkat. Annak ellenére, hogy a kulcs/érték információk hello tooa naplófájl van írva, hello nem adatai hello szótár részét csak akkor társított hello tranzakció véglegesítése megtörtént.

A fenti hello kódban hello hívás tooCommitAsync érvényesítése hello tranzakció műveleteket. Pontosabban hozzáfűzi a véglegesítési információk toohello naplófájl hello helyi csomóponton, és is elküldi a hello véglegesítési rekord tooall hello másodlagos replikákat. Miután egy kvórum (nagy) hello replikák rendelkezik válaszolt, módosítások minősülnek állandó és bármely, amely nem volt kezelhetők hello ITransaction objektum keresztül kapcsolódó zárolások feloldásáig blokkolva lesz más szálak tranzakciók állíthatók be, minden adatot hello ugyanazokkal a kulcsokkal és az értékekre.

Ha CommitAsync nem neve (általában miatt tooan kivétel lépett fel az éppen), majd hello ITransaction elemet lekérdezi eldobták. Amikor nem véglegesített ITransaction objektum ártalmatlanítása, a Service Fabric hozzáfűzi a megszakítási információk toohello helyi csomópont naplófájl, és semmi sem kell toobe küldött tooany hello a másodlagos replikák. És ezt követően bármely, amely nem volt kezelhetők hello tranzakció keresztül kapcsolódó zárolások feloldásáig blokkolva lesz.

## <a name="common-pitfalls-and-how-tooavoid-them"></a>Közös nehézségek és hogyan tooavoid őket
Most, hogy megismerkedett hello megbízható gyűjtemények működése belső, vessen egy pillantást néhány gyakori előreláthatók őket. Tekintse meg az alábbi kód hello:

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes hello name/user, logs hello bytes,
   // & sends hello bytes toohello secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // hello line below updates hello property’s value in memory only; the
   // new value is NOT serialized, logged, & sent toosecondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

Egy rendszeres .NET könyvtár az használatakor kulcs/érték toohello szótár felvétele, és módosítsa a hello (például LastLogin) tulajdonság értéke. Azonban ez a kód nem működnek megfelelően megbízható adatkönyvtárhoz. Ne feledje hello a korábbi vitafórum, tooAddAsync rendezi sorba hello kulcs/érték hello hívás toobyte tömbök objektumokat, és majd menti hello tömbállandó tooa helyi fájlt, és is toohello másodlagos replikák küldése. Ha később megváltoztatja egy tulajdonság, értékre változik hello tulajdonság értékét a memóriában csak; helyi fájl hello vagy toohello replikák küldött hello adatok nem befolyásolja. Ha hello folyamat leállásából eredő, a memória van érvényteleníteni. Új folyamat indításakor, vagy ha egy másik replika válik elsődleges, majd hello régi tulajdonság értéke van mi érhető el.

I nem emelje ki elég milyen egyszerűen toomake hello típusú hibát fent látható. És csak megtudhatja, hogyan hello hibát Ha hello folyamat leáll. hello megfelelő módon toowrite hello kódja: egyszerűen tooreverse hello két sort:


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

Íme egy másik példa a megjelenítő gyakori hiba:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use hello user’s name toolook up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // hello user exists in hello dictionary, update one of their properties.
   if (user.HasValue) {
      // hello line below updates hello property’s value in memory only; the
      // new value is NOT serialized, logged, & sent toosecondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

Ebben az esetben rendszeres .NET szótárak, a fenti hello kódot remekül működik, és egy közös minta: hello fejlesztői használja egy kulcs toolook be egy értéket. Ha hello érték már létezik, a hello fejlesztői megváltozik egy tulajdonság értéke. Megbízható gyűjteményéhez, hogy ez a kód azonban mutat, már tárgyalt probléma hello: **nem módosítania kell az objektum az adott tooa megbízható gyűjtemény után.**

hello megfelelő módon tooupdate egy megbízható gyűjtemény értéket tooget hivatkozás toohello meglévő érték, fontolja meg a hello objektum említett tooby ezt a hivatkozást nem módosítható. Ezután hozzon létre egy új objektumot, amely hello eredeti objektum pontos másolatát. Most az új objektum állapota hello módosítja, és írja hello új objektumot hello gyűjteménybe, hogy azt lekérdezi szerializált toobyte tömbállandó, hozzáfűzött toohello helyi fájlt, és elküldi a toohello replikákat. Miután hello véglegesítése change(s), hello memórián belüli objektumok hello helyi fájlt, és minden hello replikának hello azonos pontos állapota. Csak jó!

az alábbi kód hello gyűjteményben mutatja be hello megfelelő módon tooupdate érték megbízható:

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use hello user’s name toolook up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // hello user exists in hello dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with hello same state as hello current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In hello new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update hello key’s value toohello updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-tooprevent-programmer-error"></a>Nem módosítható adatok típusok tooprevent programozói hiba meghatározása
Szeretnénk ideális esetben hello fordítási tooreport hibákat, ha véletlenül eredményez, hogy meg kellene tooconsider nem módosítható objektum állapotának mutates kódját. De hello C# fordítóprogram nincs hello képességét toodo ez. Igen, tooavoid lehetséges programozói hibák, erősen ajánlott, hogy megbízható gyűjtemények toobe nem módosítható típusokat használ hello típust határoznak meg. Pontosabban Ez azt jelenti, hogy Ön odatapadjon toocore érték típuson (például számok [Int32, UInt64 stb.], dátum és idő, Guid, TimeSpan és hello hasonlóan). És természetesen is karakterlánc használható. Ajánlott tooavoid gyűjtemény tulajdonságok szerializálása és deszerializálása azokat is gyakran hátrányosan befolyásolhatja a teljesítményt. Azonban ha azt szeretné, hogy toouse gyűjtemény tulajdonságait, erősen ajánlott hello használatát. NET által nem módosítható gyűjteményeket könyvtár ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)). Ebben a könyvtárban http://nuget.org letölthető. Javasoljuk továbbá, az osztályok zárolásra és a mezőket csak olvasható amikor csak lehetséges.

hello UserInfo típusát az alábbi bemutatja, hogyan írja be a toodefine nem módosítható egy kihasználni a fenti ajánlásokat.

```csharp

[DataContract]
// If you don’t seal, you must ensure that any derived classes are also immutable
public sealed class UserInfo {
   private static readonly IEnumerable<ItemId> NoBids = ImmutableList<ItemId>.Empty;

   public UserInfo(String email, IEnumerable<ItemId> itemsBidding = null) {
      Email = email;
      ItemsBidding = (itemsBidding == null) ? NoBids : itemsBidding.ToImmutableList();
   }

   [OnDeserialized]
   private void OnDeserialized(StreamingContext context) {
      // Convert hello deserialized collection tooan immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has tooset it. So instead, hello getter is public and hello setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId toohello ItemsBidding
   // collection by creating a new immutable UserInfo object with hello added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

hello ItemId típusa nem is módosítható típus Itt látható módon:

```csharp

[DataContract]
public struct ItemId {

   [DataMember] public readonly String Seller;
   [DataMember] public readonly String ItemName;
   public ItemId(String seller, String itemName) {
      Seller = seller;
      ItemName = itemName;
   }
}
```

## <a name="schema-versioning-upgrades"></a>Séma versioning (frissítés)
Belsőleg megbízható gyűjtemények szerializálni a objektumok használják. NET a DataContractSerializer. hello szerializált objektumok megőrzött toohello elsődleges replika helyi lemezek és is átvitt toohello másodlagos replikákon. A szolgáltatás Miután kiforrottá válik, akkor valószínű toochange hello típusú adatok (séma), a szolgáltatásnak szüksége van szükség. Nagy gondot az adatok verziószámozásának kell készíthető elő. Mindenekelőtt mindig kell tudni toodeserialize régi adatokat. Pontosabban, ez azt jelenti, hogy a deszerializálás kódot kell végtelenül visszamenőlegesen kompatibilis: a szolgáltatás kód verziója 333 kell lennie egy megbízható gyűjtemény helyezi el őket a szolgáltatáskód hibáit 1 verziójának 5 éve adatokon képes toooperate.

Ezenkívül szolgáltatáskódot egyszerre lehet frissített több frissítési tartományt. Igen a frissítés során, hogy a szolgáltatáskód hibáit, hiszen egyszerre két különböző verziója. Meg kell kerülni az hello új verziójának a szolgáltatáskód hibáit hello új séma használata, mert a szolgáltatáskód hibáit régi verziói esetleg nem tudja toohandle hello új sémára. Ha lehetséges, akkor tervezzen a szolgáltatás toobe inkompatibilis verziói 1 verziójával. Pontosabban, ez azt jelenti, hogy a szolgáltatás kód V1 képesnek kell lennie toosimply figyelmen kívül bármely séma elemei nem explicit módon kezeli. Adatot nem kifejezetten ismernie és nem egyszerűen írási visszalépési szótár kulcs vagy érték frissítésekor képes toosave kell legyen.

> [!WARNING]
> Való hello séma, kulcs módosítása, győződjön meg róla, hogy a kulcs kivonatkód és egyenlő algoritmusok stabil. Hogyan működnek ezek az algoritmusok valamelyikét módosításakor legalább egyszer ismét nem lesz képes toolook hello kulcsot hello megbízható szótár belül.
>
>

Másik lehetőségként hajthat végre, mi van általában hivatkozott tooas 2 fázisú frissítése. A 2-fázis frissítést, a szolgáltatás verzióról V1 tooV2: V2 hello kódot tartalmaz, amely tudja, hogyan toodeal hello séma módosítást, de ez a kód nem hajtható végre. Hello V2 kód V1 adatok olvasását, akkor működik, és V1 adatokat. Ezt követően hello frissítés befejezése után minden frissítési tartományban, Ön is valamilyen módon azt toohello futó V2 példányát, hogy hello frissítés befejeződött. (Egyirányú toosignal Ez el egy konfigurációs frissítés tooroll; azt, hogy mi a 2-fázis frissítés miatt ez.) Most hello V2 példányok is V1-adatok olvasása, tooV2 adatok átalakítását, el és írja ki V2 adatként. Más esetekben V2-adatok olvasása, ha nincs szükségük tooconvert, ezek csak működik rajta, és kiírni a V2 adatokat.

## <a name="next-steps"></a>Következő lépések
toolearn kompatibilis adatokat továbbítson a szerződések létrehozásával kapcsolatban lásd: [előre kompatibilis adategyezményeinek](https://msdn.microsoft.com/library/ms731083.aspx).

Gyakorlati tanácsok toolearn versioning adategyezményeinek, lásd: [adatok szerződés Versioning](https://msdn.microsoft.com/library/ms731138.aspx).

toolearn hogyan szerződések tooimplement verzió hibatűrő adatokat, lásd: [verzió hibatűrő szerializálási visszahívások](https://msdn.microsoft.com/library/ms733734.aspx).

Hogyan tooprovide olyan adatszerkezet, amely képes együttműködni több verziója között webhelyet: toolearn [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).
