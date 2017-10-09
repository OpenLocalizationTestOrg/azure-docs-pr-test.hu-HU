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
# <a name="working-with-reliable-collections"></a><span data-ttu-id="09578-103">Megbízható gyűjtemények használata</span><span class="sxs-lookup"><span data-stu-id="09578-103">Working with Reliable Collections</span></span>
<span data-ttu-id="09578-104">A Service Fabric egy állapotfüggő programozási modell elérhető too.NET fejlesztők keresztül megbízható gyűjtemények kínál.</span><span class="sxs-lookup"><span data-stu-id="09578-104">Service Fabric offers a stateful programming model available too.NET developers via Reliable Collections.</span></span> <span data-ttu-id="09578-105">Pontosabban a Service Fabric megbízható szótár és megbízható várólista osztályok biztosít.</span><span class="sxs-lookup"><span data-stu-id="09578-105">Specifically, Service Fabric provides reliable dictionary and reliable queue classes.</span></span> <span data-ttu-id="09578-106">Ha használja ezeket az osztályokat, az állapot (méretezhetőségre) particionálva, replikálni (a rendelkezésre állás érdekében), és egy partíciót (ACID szemantikáját) belül.</span><span class="sxs-lookup"><span data-stu-id="09578-106">When you use these classes, your state is partitioned (for scalability), replicated (for availability), and transacted within a partition (for ACID semantics).</span></span> <span data-ttu-id="09578-107">Most egy tipikus használati megbízható dictionary objektum tekintse meg, és tekintse meg, milyen a végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="09578-107">Let’s look at a typical usage of a reliable dictionary object and see what its actually doing.</span></span>

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

<span data-ttu-id="09578-108">Összes művelet (kivéve a ClearAsync pedig nem vonható vissza), a megbízható szótár objektumokon ITransaction objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="09578-108">All operations on reliable dictionary objects (except for ClearAsync which is not undoable), require an ITransaction object.</span></span> <span data-ttu-id="09578-109">Ez az objektum van társítva toomake tooany megbízható szótár és/vagy megbízható várólista objektumok belül egyetlen partícióra végrehajtani kívánt bármely és az összes módosítása.</span><span class="sxs-lookup"><span data-stu-id="09578-109">This object has associated with it any and all changes you’re attempting toomake tooany reliable dictionary and/or reliable queue objects within a single partition.</span></span> <span data-ttu-id="09578-110">Egy ITransaction szerez be objektum hello partíciós függvény meghívásával tartozó StateManager tartozó CreateTransaction metódust.</span><span class="sxs-lookup"><span data-stu-id="09578-110">You acquire an ITransaction object by calling hello partition’s StateManager’s CreateTransaction method.</span></span>

<span data-ttu-id="09578-111">A fenti hello kódban hello ITransaction objektum tooa megbízható szótár AddAsync metódus lett átadva.</span><span class="sxs-lookup"><span data-stu-id="09578-111">In hello code above, hello ITransaction object is passed tooa reliable dictionary’s AddAsync method.</span></span> <span data-ttu-id="09578-112">Belső a szótár módszerek, amely fogad egy kulcs egy olvasási/írási zárolás hello kulcshoz tartozó igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="09578-112">Internally, dictionary methods that accepts a key take a reader/writer lock associated with hello key.</span></span> <span data-ttu-id="09578-113">Hello metódus hello kulcs-érték módosítja, ha hello metódus hello kulcs zár lép, és ha hello metódus csak olvassa be az hello kulcs értékét, majd olvasási zárolás szükséges hello kulcs.</span><span class="sxs-lookup"><span data-stu-id="09578-113">If hello method modifies hello key’s value, hello method takes a write lock on hello key and if hello method only reads from hello key’s value, then a read lock is taken on hello key.</span></span> <span data-ttu-id="09578-114">Mivel AddAsync hello kulcs-érték toohello új módosítja, átadott érték hello kulcs írási zárolás használatban van.</span><span class="sxs-lookup"><span data-stu-id="09578-114">Since AddAsync modifies hello key’s value toohello new, passed-in value, hello key’s write lock is taken.</span></span> <span data-ttu-id="09578-115">Igen, 2 (vagy több) szálak megpróbálnak hello tooadd értékek ugyanaz a kulcs: hello azonos időben, egy szál hello írási zárolás fogják beszerezni és hello más szálak blokkolja.</span><span class="sxs-lookup"><span data-stu-id="09578-115">So, if 2 (or more) threads attempt tooadd values with hello same key at hello same time, one thread will acquire hello write lock and hello other threads will block.</span></span> <span data-ttu-id="09578-116">Alapértelmezés szerint az too4 másodperc tooacquire hello zárat; blokkja módszerek 4 másodperc múlva hello módszerek throw egy TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="09578-116">By default, methods block for up too4 seconds tooacquire hello lock; after 4 seconds, hello methods throw a TimeoutException.</span></span> <span data-ttu-id="09578-117">Módszer túlterhelések található így Ön toopass egy explicit időtúllépési értéket, ha szeretne.</span><span class="sxs-lookup"><span data-stu-id="09578-117">Method overloads exist allowing you toopass an explicit timeout value if you’d prefer.</span></span>

<span data-ttu-id="09578-118">A kód tooreact tooa TimeoutException általában, valamint rögzíti és hello teljes művelet megkísérlése (ahogy a fenti hello kódot) írható.</span><span class="sxs-lookup"><span data-stu-id="09578-118">Usually, you write your code tooreact tooa TimeoutException by catching it and retrying hello entire operation (as shown in hello code above).</span></span> <span data-ttu-id="09578-119">Egyszerű kód Task.Delay átadásakor 100 milliomod másodperc minden alkalommal, amikor csak hívandó.</span><span class="sxs-lookup"><span data-stu-id="09578-119">In my simple code, I’m just calling Task.Delay passing 100 milliseconds each time.</span></span> <span data-ttu-id="09578-120">De a valóságban, valószínűleg jobb, ha valamilyen exponenciális vissza az indító késleltetés használja helyette.</span><span class="sxs-lookup"><span data-stu-id="09578-120">But, in reality, you might be better off using some kind of exponential back-off delay instead.</span></span>

<span data-ttu-id="09578-121">Miután hello zárolási keletkezik, AddAsync hello kulcs hozzáadása, és érték objektum tooan belső ideiglenes szótár hello ITransaction objektumhoz rendelt hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="09578-121">Once hello lock is acquired, AddAsync adds hello key and value object references tooan internal temporary dictionary associated with hello ITransaction object.</span></span> <span data-ttu-id="09578-122">Ebben az esetben tooprovide, olvasási-a-saját-írási műveletek szemantikájú.</span><span class="sxs-lookup"><span data-stu-id="09578-122">This is done tooprovide you with read-your-own-writes semantics.</span></span> <span data-ttu-id="09578-123">Ez azt jelenti, hogy követően meghívja a AddAsync, egy újabb hívás tooTryGetValueAsync (azonos ITransaction objektum használatával hello) hello értéket adja vissza, akkor is, ha még nem véglegesített hello tranzakció.</span><span class="sxs-lookup"><span data-stu-id="09578-123">That is, after you call AddAsync, a later call tooTryGetValueAsync (using hello same ITransaction object) will return hello value even if you have not yet committed hello transaction.</span></span> <span data-ttu-id="09578-124">A következő AddAsync rendezi sorba a kulcsot és értéket toobyte tömbök objektumokat, és hozzáfűzi a byte tömbök tooa naplófájl hello helyi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="09578-124">Next, AddAsync serializes your key and value objects toobyte arrays and appends these byte arrays tooa log file on hello local node.</span></span> <span data-ttu-id="09578-125">Végezetül AddAsync küldi hello bájt tömbök tooall hello másodlagos replika, azonos rendelkezik hello kulcs/érték információkat.</span><span class="sxs-lookup"><span data-stu-id="09578-125">Finally, AddAsync sends hello byte arrays tooall hello secondary replicas so they have hello same key/value information.</span></span> <span data-ttu-id="09578-126">Annak ellenére, hogy a kulcs/érték információk hello tooa naplófájl van írva, hello nem adatai hello szótár részét csak akkor társított hello tranzakció véglegesítése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="09578-126">Even though hello key/value information has been written tooa log file, hello information is not considered part of hello dictionary until hello transaction that they are associated with has been committed.</span></span>

<span data-ttu-id="09578-127">A fenti hello kódban hello hívás tooCommitAsync érvényesítése hello tranzakció műveleteket.</span><span class="sxs-lookup"><span data-stu-id="09578-127">In hello code above, hello call tooCommitAsync commits all of hello transaction’s operations.</span></span> <span data-ttu-id="09578-128">Pontosabban hozzáfűzi a véglegesítési információk toohello naplófájl hello helyi csomóponton, és is elküldi a hello véglegesítési rekord tooall hello másodlagos replikákat.</span><span class="sxs-lookup"><span data-stu-id="09578-128">Specifically, it appends commit information toohello log file on hello local node and also sends hello commit record tooall hello secondary replicas.</span></span> <span data-ttu-id="09578-129">Miután egy kvórum (nagy) hello replikák rendelkezik válaszolt, módosítások minősülnek állandó és bármely, amely nem volt kezelhetők hello ITransaction objektum keresztül kapcsolódó zárolások feloldásáig blokkolva lesz más szálak tranzakciók állíthatók be, minden adatot hello ugyanazokkal a kulcsokkal és az értékekre.</span><span class="sxs-lookup"><span data-stu-id="09578-129">Once a quorum (majority) of hello replicas has replied, all data changes are considered permanent and any locks associated with keys that were manipulated via hello ITransaction object are released so other threads/transactions can manipulate hello same keys and their values.</span></span>

<span data-ttu-id="09578-130">Ha CommitAsync nem neve (általában miatt tooan kivétel lépett fel az éppen), majd hello ITransaction elemet lekérdezi eldobták.</span><span class="sxs-lookup"><span data-stu-id="09578-130">If CommitAsync is not called (usually due tooan exception being thrown), then hello ITransaction object gets disposed.</span></span> <span data-ttu-id="09578-131">Amikor nem véglegesített ITransaction objektum ártalmatlanítása, a Service Fabric hozzáfűzi a megszakítási információk toohello helyi csomópont naplófájl, és semmi sem kell toobe küldött tooany hello a másodlagos replikák.</span><span class="sxs-lookup"><span data-stu-id="09578-131">When disposing an uncommitted ITransaction object, Service Fabric appends abort information toohello local node’s log file and nothing needs toobe sent tooany of hello secondary replicas.</span></span> <span data-ttu-id="09578-132">És ezt követően bármely, amely nem volt kezelhetők hello tranzakció keresztül kapcsolódó zárolások feloldásáig blokkolva lesz.</span><span class="sxs-lookup"><span data-stu-id="09578-132">And then, any locks associated with keys that were manipulated via hello transaction are released.</span></span>

## <a name="common-pitfalls-and-how-tooavoid-them"></a><span data-ttu-id="09578-133">Közös nehézségek és hogyan tooavoid őket</span><span class="sxs-lookup"><span data-stu-id="09578-133">Common pitfalls and how tooavoid them</span></span>
<span data-ttu-id="09578-134">Most, hogy megismerkedett hello megbízható gyűjtemények működése belső, vessen egy pillantást néhány gyakori előreláthatók őket.</span><span class="sxs-lookup"><span data-stu-id="09578-134">Now that you understand how hello reliable collections work internally, let’s take a look at some common misuses of them.</span></span> <span data-ttu-id="09578-135">Tekintse meg az alábbi kód hello:</span><span class="sxs-lookup"><span data-stu-id="09578-135">See hello code below:</span></span>

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

<span data-ttu-id="09578-136">Egy rendszeres .NET könyvtár az használatakor kulcs/érték toohello szótár felvétele, és módosítsa a hello (például LastLogin) tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="09578-136">When working with a regular .NET dictionary, you can add a key/value toohello dictionary and then change hello value of a property (such as LastLogin).</span></span> <span data-ttu-id="09578-137">Azonban ez a kód nem működnek megfelelően megbízható adatkönyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="09578-137">However, this code will not work correctly with a reliable dictionary.</span></span> <span data-ttu-id="09578-138">Ne feledje hello a korábbi vitafórum, tooAddAsync rendezi sorba hello kulcs/érték hello hívás toobyte tömbök objektumokat, és majd menti hello tömbállandó tooa helyi fájlt, és is toohello másodlagos replikák küldése.</span><span class="sxs-lookup"><span data-stu-id="09578-138">Remember from hello earlier discussion, hello call tooAddAsync serializes hello key/value objects toobyte arrays and then saves hello arrays tooa local file and also sends them toohello secondary replicas.</span></span> <span data-ttu-id="09578-139">Ha később megváltoztatja egy tulajdonság, értékre változik hello tulajdonság értékét a memóriában csak; helyi fájl hello vagy toohello replikák küldött hello adatok nem befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="09578-139">If you later change a property, this changes hello property’s value in memory only; it does not impact hello local file or hello data sent toohello replicas.</span></span> <span data-ttu-id="09578-140">Ha hello folyamat leállásából eredő, a memória van érvényteleníteni.</span><span class="sxs-lookup"><span data-stu-id="09578-140">If hello process crashes, what’s in memory is thrown away.</span></span> <span data-ttu-id="09578-141">Új folyamat indításakor, vagy ha egy másik replika válik elsődleges, majd hello régi tulajdonság értéke van mi érhető el.</span><span class="sxs-lookup"><span data-stu-id="09578-141">When a new process starts or if another replica becomes primary, then hello old property value is what is available.</span></span>

<span data-ttu-id="09578-142">I nem emelje ki elég milyen egyszerűen toomake hello típusú hibát fent látható.</span><span class="sxs-lookup"><span data-stu-id="09578-142">I cannot stress enough how easy it is toomake hello kind of mistake shown above.</span></span> <span data-ttu-id="09578-143">És csak megtudhatja, hogyan hello hibát Ha hello folyamat leáll.</span><span class="sxs-lookup"><span data-stu-id="09578-143">And, you will only learn about hello mistake if/when hello process goes down.</span></span> <span data-ttu-id="09578-144">hello megfelelő módon toowrite hello kódja: egyszerűen tooreverse hello két sort:</span><span class="sxs-lookup"><span data-stu-id="09578-144">hello correct way toowrite hello code is simply tooreverse hello two lines:</span></span>


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

<span data-ttu-id="09578-145">Íme egy másik példa a megjelenítő gyakori hiba:</span><span class="sxs-lookup"><span data-stu-id="09578-145">Here is another example showing a common mistake:</span></span>

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

<span data-ttu-id="09578-146">Ebben az esetben rendszeres .NET szótárak, a fenti hello kódot remekül működik, és egy közös minta: hello fejlesztői használja egy kulcs toolook be egy értéket.</span><span class="sxs-lookup"><span data-stu-id="09578-146">Again, with regular .NET dictionaries, hello code above works fine and is a common pattern: hello developer uses a key toolook up a value.</span></span> <span data-ttu-id="09578-147">Ha hello érték már létezik, a hello fejlesztői megváltozik egy tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="09578-147">If hello value exists, hello developer changes a property’s value.</span></span> <span data-ttu-id="09578-148">Megbízható gyűjteményéhez, hogy ez a kód azonban mutat, már tárgyalt probléma hello: **nem módosítania kell az objektum az adott tooa megbízható gyűjtemény után.**</span><span class="sxs-lookup"><span data-stu-id="09578-148">However, with reliable collections, this code exhibits hello same problem as already discussed: **you MUST not modify an object once you have given it tooa reliable collection.**</span></span>

<span data-ttu-id="09578-149">hello megfelelő módon tooupdate egy megbízható gyűjtemény értéket tooget hivatkozás toohello meglévő érték, fontolja meg a hello objektum említett tooby ezt a hivatkozást nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="09578-149">hello correct way tooupdate a value in a reliable collection, is tooget a reference toohello existing value and consider hello object referred tooby this reference immutable.</span></span> <span data-ttu-id="09578-150">Ezután hozzon létre egy új objektumot, amely hello eredeti objektum pontos másolatát.</span><span class="sxs-lookup"><span data-stu-id="09578-150">Then, create a new object which is an exact copy of hello original object.</span></span> <span data-ttu-id="09578-151">Most az új objektum állapota hello módosítja, és írja hello új objektumot hello gyűjteménybe, hogy azt lekérdezi szerializált toobyte tömbállandó, hozzáfűzött toohello helyi fájlt, és elküldi a toohello replikákat.</span><span class="sxs-lookup"><span data-stu-id="09578-151">Now, you can modify hello state of this new object and write hello new object into hello collection so that it gets serialized toobyte arrays, appended toohello local file and sent toohello replicas.</span></span> <span data-ttu-id="09578-152">Miután hello véglegesítése change(s), hello memórián belüli objektumok hello helyi fájlt, és minden hello replikának hello azonos pontos állapota.</span><span class="sxs-lookup"><span data-stu-id="09578-152">After committing hello change(s), hello in-memory objects, hello local file, and all hello replicas have hello same exact state.</span></span> <span data-ttu-id="09578-153">Csak jó!</span><span class="sxs-lookup"><span data-stu-id="09578-153">All is good!</span></span>

<span data-ttu-id="09578-154">az alábbi kód hello gyűjteményben mutatja be hello megfelelő módon tooupdate érték megbízható:</span><span class="sxs-lookup"><span data-stu-id="09578-154">hello code below shows hello correct way tooupdate a value in a reliable collection:</span></span>

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

## <a name="define-immutable-data-types-tooprevent-programmer-error"></a><span data-ttu-id="09578-155">Nem módosítható adatok típusok tooprevent programozói hiba meghatározása</span><span class="sxs-lookup"><span data-stu-id="09578-155">Define immutable data types tooprevent programmer error</span></span>
<span data-ttu-id="09578-156">Szeretnénk ideális esetben hello fordítási tooreport hibákat, ha véletlenül eredményez, hogy meg kellene tooconsider nem módosítható objektum állapotának mutates kódját.</span><span class="sxs-lookup"><span data-stu-id="09578-156">Ideally, we’d like hello compiler tooreport errors when you accidentally produce code that mutates state of an object that you are supposed tooconsider immutable.</span></span> <span data-ttu-id="09578-157">De hello C# fordítóprogram nincs hello képességét toodo ez.</span><span class="sxs-lookup"><span data-stu-id="09578-157">But, hello C# compiler does not have hello ability toodo this.</span></span> <span data-ttu-id="09578-158">Igen, tooavoid lehetséges programozói hibák, erősen ajánlott, hogy megbízható gyűjtemények toobe nem módosítható típusokat használ hello típust határoznak meg.</span><span class="sxs-lookup"><span data-stu-id="09578-158">So, tooavoid potential programmer bugs, we highly recommend that you define hello types you use with reliable collections toobe immutable types.</span></span> <span data-ttu-id="09578-159">Pontosabban Ez azt jelenti, hogy Ön odatapadjon toocore érték típuson (például számok [Int32, UInt64 stb.], dátum és idő, Guid, TimeSpan és hello hasonlóan).</span><span class="sxs-lookup"><span data-stu-id="09578-159">Specifically, this means that you stick toocore value types (such as numbers [Int32, UInt64, etc.], DateTime, Guid, TimeSpan, and hello like).</span></span> <span data-ttu-id="09578-160">És természetesen is karakterlánc használható.</span><span class="sxs-lookup"><span data-stu-id="09578-160">And, of course, you can also use String.</span></span> <span data-ttu-id="09578-161">Ajánlott tooavoid gyűjtemény tulajdonságok szerializálása és deszerializálása azokat is gyakran hátrányosan befolyásolhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="09578-161">It is best tooavoid collection properties as serializing and deserializing them can frequently can hurt performance.</span></span> <span data-ttu-id="09578-162">Azonban ha azt szeretné, hogy toouse gyűjtemény tulajdonságait, erősen ajánlott hello használatát. NET által nem módosítható gyűjteményeket könyvtár ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span><span class="sxs-lookup"><span data-stu-id="09578-162">However, if you want toouse collection properties, we highly recommend hello use of .NET’s immutable collections library ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span></span> <span data-ttu-id="09578-163">Ebben a könyvtárban http://nuget.org letölthető. Javasoljuk továbbá, az osztályok zárolásra és a mezőket csak olvasható amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="09578-163">This library is available for download from http://nuget.org. We also recommend sealing your classes and making fields read-only whenever possible.</span></span>

<span data-ttu-id="09578-164">hello UserInfo típusát az alábbi bemutatja, hogyan írja be a toodefine nem módosítható egy kihasználni a fenti ajánlásokat.</span><span class="sxs-lookup"><span data-stu-id="09578-164">hello UserInfo type below demonstrates how toodefine an immutable type taking advantage of aforementioned recommendations.</span></span>

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

<span data-ttu-id="09578-165">hello ItemId típusa nem is módosítható típus Itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="09578-165">hello ItemId type is also an immutable type as shown here:</span></span>

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

## <a name="schema-versioning-upgrades"></a><span data-ttu-id="09578-166">Séma versioning (frissítés)</span><span class="sxs-lookup"><span data-stu-id="09578-166">Schema versioning (upgrades)</span></span>
<span data-ttu-id="09578-167">Belsőleg megbízható gyűjtemények szerializálni a objektumok használják. NET a DataContractSerializer.</span><span class="sxs-lookup"><span data-stu-id="09578-167">Internally, Reliable Collections serialize your objects using .NET’s DataContractSerializer.</span></span> <span data-ttu-id="09578-168">hello szerializált objektumok megőrzött toohello elsődleges replika helyi lemezek és is átvitt toohello másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="09578-168">hello serialized objects are persisted toohello primary replica’s local disk and are also transmitted toohello secondary replicas.</span></span> <span data-ttu-id="09578-169">A szolgáltatás Miután kiforrottá válik, akkor valószínű toochange hello típusú adatok (séma), a szolgáltatásnak szüksége van szükség.</span><span class="sxs-lookup"><span data-stu-id="09578-169">As your service matures, it’s likely you’ll want toochange hello kind of data (schema) your service requires.</span></span> <span data-ttu-id="09578-170">Nagy gondot az adatok verziószámozásának kell készíthető elő.</span><span class="sxs-lookup"><span data-stu-id="09578-170">You must approach versioning of your data with great care.</span></span> <span data-ttu-id="09578-171">Mindenekelőtt mindig kell tudni toodeserialize régi adatokat.</span><span class="sxs-lookup"><span data-stu-id="09578-171">First and foremost, you must always be able toodeserialize old data.</span></span> <span data-ttu-id="09578-172">Pontosabban, ez azt jelenti, hogy a deszerializálás kódot kell végtelenül visszamenőlegesen kompatibilis: a szolgáltatás kód verziója 333 kell lennie egy megbízható gyűjtemény helyezi el őket a szolgáltatáskód hibáit 1 verziójának 5 éve adatokon képes toooperate.</span><span class="sxs-lookup"><span data-stu-id="09578-172">Specifically, this means your deserialization code must be infinitely backward compatible: Version 333 of your service code must be able toooperate on data placed in a reliable collection by version 1 of your service code 5 years ago.</span></span>

<span data-ttu-id="09578-173">Ezenkívül szolgáltatáskódot egyszerre lehet frissített több frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="09578-173">Furthermore, service code is upgraded one upgrade domain at a time.</span></span> <span data-ttu-id="09578-174">Igen a frissítés során, hogy a szolgáltatáskód hibáit, hiszen egyszerre két különböző verziója.</span><span class="sxs-lookup"><span data-stu-id="09578-174">So, during an upgrade, you have two different versions of your service code running simultaneously.</span></span> <span data-ttu-id="09578-175">Meg kell kerülni az hello új verziójának a szolgáltatáskód hibáit hello új séma használata, mert a szolgáltatáskód hibáit régi verziói esetleg nem tudja toohandle hello új sémára.</span><span class="sxs-lookup"><span data-stu-id="09578-175">You must avoid having hello new version of your service code use hello new schema as old versions of your service code might not be able toohandle hello new schema.</span></span> <span data-ttu-id="09578-176">Ha lehetséges, akkor tervezzen a szolgáltatás toobe inkompatibilis verziói 1 verziójával.</span><span class="sxs-lookup"><span data-stu-id="09578-176">When possible, you should design each version of your service toobe forward compatible by 1 version.</span></span> <span data-ttu-id="09578-177">Pontosabban, ez azt jelenti, hogy a szolgáltatás kód V1 képesnek kell lennie toosimply figyelmen kívül bármely séma elemei nem explicit módon kezeli.</span><span class="sxs-lookup"><span data-stu-id="09578-177">Specifically, this means that V1 of your service code should be able toosimply ignore any schema elements it does not explicitly handle.</span></span> <span data-ttu-id="09578-178">Adatot nem kifejezetten ismernie és nem egyszerűen írási visszalépési szótár kulcs vagy érték frissítésekor képes toosave kell legyen.</span><span class="sxs-lookup"><span data-stu-id="09578-178">However, it must be able toosave any data it doesn’t explicitly know about and simply write it back out when updating a dictionary key or value.</span></span>

> [!WARNING]
> <span data-ttu-id="09578-179">Való hello séma, kulcs módosítása, győződjön meg róla, hogy a kulcs kivonatkód és egyenlő algoritmusok stabil.</span><span class="sxs-lookup"><span data-stu-id="09578-179">While you can modify hello schema of a key, you must ensure that your key’s hash code and equals algorithms are stable.</span></span> <span data-ttu-id="09578-180">Hogyan működnek ezek az algoritmusok valamelyikét módosításakor legalább egyszer ismét nem lesz képes toolook hello kulcsot hello megbízható szótár belül.</span><span class="sxs-lookup"><span data-stu-id="09578-180">If you change how either of these algorithms operate, you will not be able toolook up hello key within hello reliable dictionary ever again.</span></span>
>
>

<span data-ttu-id="09578-181">Másik lehetőségként hajthat végre, mi van általában hivatkozott tooas 2 fázisú frissítése.</span><span class="sxs-lookup"><span data-stu-id="09578-181">Alternatively, you can perform what is typically referred tooas a 2-phase upgrade.</span></span> <span data-ttu-id="09578-182">A 2-fázis frissítést, a szolgáltatás verzióról V1 tooV2: V2 hello kódot tartalmaz, amely tudja, hogyan toodeal hello séma módosítást, de ez a kód nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="09578-182">With a 2-phase upgrade, you upgrade your service from V1 tooV2: V2 contains hello code that knows how toodeal with hello new schema change but this code doesn’t execute.</span></span> <span data-ttu-id="09578-183">Hello V2 kód V1 adatok olvasását, akkor működik, és V1 adatokat.</span><span class="sxs-lookup"><span data-stu-id="09578-183">When hello V2 code reads V1 data, it operates on it and writes V1 data.</span></span> <span data-ttu-id="09578-184">Ezt követően hello frissítés befejezése után minden frissítési tartományban, Ön is valamilyen módon azt toohello futó V2 példányát, hogy hello frissítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="09578-184">Then, after hello upgrade is complete across all upgrade domains, you can somehow signal toohello running V2 instances that hello upgrade is complete.</span></span> <span data-ttu-id="09578-185">(Egyirányú toosignal Ez el egy konfigurációs frissítés tooroll; azt, hogy mi a 2-fázis frissítés miatt ez.) Most hello V2 példányok is V1-adatok olvasása, tooV2 adatok átalakítását, el és írja ki V2 adatként.</span><span class="sxs-lookup"><span data-stu-id="09578-185">(One way toosignal this is tooroll out a configuration upgrade; this is what makes this a 2-phase upgrade.) Now, hello V2 instances can read V1 data, convert it tooV2 data, operate on it, and write it out as V2 data.</span></span> <span data-ttu-id="09578-186">Más esetekben V2-adatok olvasása, ha nincs szükségük tooconvert, ezek csak működik rajta, és kiírni a V2 adatokat.</span><span class="sxs-lookup"><span data-stu-id="09578-186">When other instances read V2 data, they do not need tooconvert it, they just operate on it, and write out V2 data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="09578-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09578-187">Next Steps</span></span>
<span data-ttu-id="09578-188">toolearn kompatibilis adatokat továbbítson a szerződések létrehozásával kapcsolatban lásd: [előre kompatibilis adategyezményeinek](https://msdn.microsoft.com/library/ms731083.aspx).</span><span class="sxs-lookup"><span data-stu-id="09578-188">toolearn about creating forward compatible data contracts, see [Forward-Compatible Data Contracts](https://msdn.microsoft.com/library/ms731083.aspx).</span></span>

<span data-ttu-id="09578-189">Gyakorlati tanácsok toolearn versioning adategyezményeinek, lásd: [adatok szerződés Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span><span class="sxs-lookup"><span data-stu-id="09578-189">toolearn best practices on versioning data contracts, see [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span></span>

<span data-ttu-id="09578-190">toolearn hogyan szerződések tooimplement verzió hibatűrő adatokat, lásd: [verzió hibatűrő szerializálási visszahívások](https://msdn.microsoft.com/library/ms733734.aspx).</span><span class="sxs-lookup"><span data-stu-id="09578-190">toolearn how tooimplement version tolerant data contracts, see [Version-Tolerant Serialization Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).</span></span>

<span data-ttu-id="09578-191">Hogyan tooprovide olyan adatszerkezet, amely képes együttműködni több verziója között webhelyet: toolearn [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="09578-191">toolearn how tooprovide a data structure that can interoperate across multiple versions, see [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span></span>
