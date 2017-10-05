---
title: "Megbízható gyűjtemények használata |} Microsoft Docs"
description: "Ismerje meg az ajánlott eljárások megbízható gyűjtemények használata."
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
ms.openlocfilehash: f53f13e4fb83b1cd370ec673e86e5311cd93055f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-reliable-collections"></a><span data-ttu-id="d7d40-103">Megbízható gyűjtemények használata</span><span class="sxs-lookup"><span data-stu-id="d7d40-103">Working with Reliable Collections</span></span>
<span data-ttu-id="d7d40-104">A Service Fabric a .NET-fejlesztők számára megbízható gyűjtemények keresztül elérhető állapot-nyilvántartó programozási modellt biztosít.</span><span class="sxs-lookup"><span data-stu-id="d7d40-104">Service Fabric offers a stateful programming model available to .NET developers via Reliable Collections.</span></span> <span data-ttu-id="d7d40-105">Pontosabban a Service Fabric megbízható szótár és megbízható várólista osztályok biztosít.</span><span class="sxs-lookup"><span data-stu-id="d7d40-105">Specifically, Service Fabric provides reliable dictionary and reliable queue classes.</span></span> <span data-ttu-id="d7d40-106">Ha használja ezeket az osztályokat, az állapot (méretezhetőségre) particionálva, replikálni (a rendelkezésre állás érdekében), és egy partíciót (ACID szemantikáját) belül.</span><span class="sxs-lookup"><span data-stu-id="d7d40-106">When you use these classes, your state is partitioned (for scalability), replicated (for availability), and transacted within a partition (for ACID semantics).</span></span> <span data-ttu-id="d7d40-107">Most egy tipikus használati megbízható dictionary objektum tekintse meg, és tekintse meg, milyen a végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="d7d40-107">Let’s look at a typical usage of a reliable dictionary object and see what its actually doing.</span></span>

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

      // CommitAsync sends Commit record to log & secondary replicas
      // After quorum responds, all locks released
      await tx.CommitAsync();
   }
   // If CommitAsync not called, Dispose sends Abort
   // record to log & all locks released
}
catch (TimeoutException) {
   await Task.Delay(100, cancellationToken); goto retry;
}
```

<span data-ttu-id="d7d40-108">Összes művelet (kivéve a ClearAsync pedig nem vonható vissza), a megbízható szótár objektumokon ITransaction objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="d7d40-108">All operations on reliable dictionary objects (except for ClearAsync which is not undoable), require an ITransaction object.</span></span> <span data-ttu-id="d7d40-109">Ez az objektum van társítva a, és minden bármely megbízható szótár és megbízható végrehajtja a végrehajtani kívánt módosítás várólistára objektumok belül egyetlen partícióra.</span><span class="sxs-lookup"><span data-stu-id="d7d40-109">This object has associated with it any and all changes you’re attempting to make to any reliable dictionary and/or reliable queue objects within a single partition.</span></span> <span data-ttu-id="d7d40-110">Egy ITransaction szerez be a partíció meghívásával objektum által StateManager tartozó CreateTransaction metódust.</span><span class="sxs-lookup"><span data-stu-id="d7d40-110">You acquire an ITransaction object by calling the partition’s StateManager’s CreateTransaction method.</span></span>

<span data-ttu-id="d7d40-111">A fenti kódot, a megbízható dictionary AddAsync metódusnak átadott a ITransaction objektum.</span><span class="sxs-lookup"><span data-stu-id="d7d40-111">In the code above, the ITransaction object is passed to a reliable dictionary’s AddAsync method.</span></span> <span data-ttu-id="d7d40-112">Belső a szótár módszerek, amely fogad egy kulcs egy kulcshoz hozzárendelt olvasási/írási zárolás igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="d7d40-112">Internally, dictionary methods that accepts a key take a reader/writer lock associated with the key.</span></span> <span data-ttu-id="d7d40-113">A metódus módosítja a kulcs-érték, ha a módszer egy írási zárolás veszi a kulcsot, és ha a metódus csak beolvassa a kulcs értékét, majd olvasási zárolás szükséges a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d7d40-113">If the method modifies the key’s value, the method takes a write lock on the key and if the method only reads from the key’s value, then a read lock is taken on the key.</span></span> <span data-ttu-id="d7d40-114">Mivel AddAsync a kulcs-érték az új, az átadott értékre módosítja, a kulcs írási zárolás lesz végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="d7d40-114">Since AddAsync modifies the key’s value to the new, passed-in value, the key’s write lock is taken.</span></span> <span data-ttu-id="d7d40-115">Igen 2 (vagy több) szálak megpróbálnak értékek hozzáadásához ugyanazzal a kulccsal egyszerre, egy szál arányban fogják beszerezni az írási zárolás, és a más szálak blokkolja.</span><span class="sxs-lookup"><span data-stu-id="d7d40-115">So, if 2 (or more) threads attempt to add values with the same key at the same time, one thread will acquire the write lock and the other threads will block.</span></span> <span data-ttu-id="d7d40-116">Alapértelmezés szerint 4 másodpercet a zárolás; módszerek letiltása 4 másodpercen belül a módszerek throw egy TimeoutException.</span><span class="sxs-lookup"><span data-stu-id="d7d40-116">By default, methods block for up to 4 seconds to acquire the lock; after 4 seconds, the methods throw a TimeoutException.</span></span> <span data-ttu-id="d7d40-117">Módszer túlterhelések létezik, hogy lehetővé teszi a adjon át egy explicit időtúllépési értéket, ha inkább.</span><span class="sxs-lookup"><span data-stu-id="d7d40-117">Method overloads exist allowing you to pass an explicit timeout value if you’d prefer.</span></span>

<span data-ttu-id="d7d40-118">Általában, hogy a kód írása reagálni a TimeoutException alatt, és újra próbálkozik a teljes műveletet (ahogy a fenti kódot)</span><span class="sxs-lookup"><span data-stu-id="d7d40-118">Usually, you write your code to react to a TimeoutException by catching it and retrying the entire operation (as shown in the code above).</span></span> <span data-ttu-id="d7d40-119">Egyszerű kód Task.Delay átadásakor 100 milliomod másodperc minden alkalommal, amikor csak hívandó.</span><span class="sxs-lookup"><span data-stu-id="d7d40-119">In my simple code, I’m just calling Task.Delay passing 100 milliseconds each time.</span></span> <span data-ttu-id="d7d40-120">De a valóságban, valószínűleg jobb, ha valamilyen exponenciális vissza az indító késleltetés használja helyette.</span><span class="sxs-lookup"><span data-stu-id="d7d40-120">But, in reality, you might be better off using some kind of exponential back-off delay instead.</span></span>

<span data-ttu-id="d7d40-121">A zárolás keletkezik, ha a AddAsync hozzáadja egy belső ideiglenes könyvtár az ITransaction objektumhoz rendelt kulcs-érték objektumra hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d7d40-121">Once the lock is acquired, AddAsync adds the key and value object references to an internal temporary dictionary associated with the ITransaction object.</span></span> <span data-ttu-id="d7d40-122">Ez biztosítja, hogy olvasási-a-saját-írási műveletek szemantikájú történik.</span><span class="sxs-lookup"><span data-stu-id="d7d40-122">This is done to provide you with read-your-own-writes semantics.</span></span> <span data-ttu-id="d7d40-123">Ez azt jelenti, hogy után hívható AddAsync, (az azonos ITransaction objektum használatával) TryGetValueAsync újabb hívásakor visszatér az érték akkor is, ha még nem véglegesített tranzakció.</span><span class="sxs-lookup"><span data-stu-id="d7d40-123">That is, after you call AddAsync, a later call to TryGetValueAsync (using the same ITransaction object) will return the value even if you have not yet committed the transaction.</span></span> <span data-ttu-id="d7d40-124">A következő AddAsync rendezi sorba a kulcsot és értéket bájt tömbök objektumok és ezek bájt tömbök hozzáfűz egy naplófájlba, a helyi csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d7d40-124">Next, AddAsync serializes your key and value objects to byte arrays and appends these byte arrays to a log file on the local node.</span></span> <span data-ttu-id="d7d40-125">Végezetül AddAsync küld a byte tömbök a másodlagos replikák úgy, hogy a kulcs/érték ugyanazokat az információkat.</span><span class="sxs-lookup"><span data-stu-id="d7d40-125">Finally, AddAsync sends the byte arrays to all the secondary replicas so they have the same key/value information.</span></span> <span data-ttu-id="d7d40-126">Annak ellenére, hogy a kulcs/érték adatokat naplófájlba van írva, az adatokat nem részének számít a szótár csak a velük társított tranzakció véglegesítése után.</span><span class="sxs-lookup"><span data-stu-id="d7d40-126">Even though the key/value information has been written to a log file, the information is not considered part of the dictionary until the transaction that they are associated with has been committed.</span></span>

<span data-ttu-id="d7d40-127">A fenti kódot CommitAsync hívása véglegesíti a tranzakció műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d7d40-127">In the code above, the call to CommitAsync commits all of the transaction’s operations.</span></span> <span data-ttu-id="d7d40-128">Pontosabban hozzáfűzi a véglegesítési információt a naplófájl a helyi csomóponton, és a végrehajtási rekord is küld a másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="d7d40-128">Specifically, it appends commit information to the log file on the local node and also sends the commit record to all the secondary replicas.</span></span> <span data-ttu-id="d7d40-129">Miután a replikák másodlagosak (nagy) rendelkezik válaszolt, minden adatmódosítást állandó minősülnek, és bármely, amely nem volt kezelhetők a ITransaction objektum keresztül kapcsolódó zárolások feloldásáig blokkolva lesz, így más szálak tranzakciók állíthatók be ugyanazokkal a kulcsokkal és értékekkel.</span><span class="sxs-lookup"><span data-stu-id="d7d40-129">Once a quorum (majority) of the replicas has replied, all data changes are considered permanent and any locks associated with keys that were manipulated via the ITransaction object are released so other threads/transactions can manipulate the same keys and their values.</span></span>

<span data-ttu-id="d7d40-130">Ha CommitAsync nem neve (általában miatt kivételt folyamatban), majd a ITransaction elemet lekérdezi eldobták.</span><span class="sxs-lookup"><span data-stu-id="d7d40-130">If CommitAsync is not called (usually due to an exception being thrown), then the ITransaction object gets disposed.</span></span> <span data-ttu-id="d7d40-131">Nem véglegesített ITransaction objektum ártalmatlanítása, amikor a Service Fabric hozzáfűzi a megszakítási információt a helyi csomópont naplófájl, és semmi nem kell a másodlagos replikák bármelyike elküldését.</span><span class="sxs-lookup"><span data-stu-id="d7d40-131">When disposing an uncommitted ITransaction object, Service Fabric appends abort information to the local node’s log file and nothing needs to be sent to any of the secondary replicas.</span></span> <span data-ttu-id="d7d40-132">És ezt követően bármely, amely nem volt kezelhetők a tranzakció keresztül kapcsolódó zárolások feloldásáig blokkolva lesz.</span><span class="sxs-lookup"><span data-stu-id="d7d40-132">And then, any locks associated with keys that were manipulated via the transaction are released.</span></span>

## <a name="common-pitfalls-and-how-to-avoid-them"></a><span data-ttu-id="d7d40-133">Közös nehézségek és azok elkerülésének</span><span class="sxs-lookup"><span data-stu-id="d7d40-133">Common pitfalls and how to avoid them</span></span>
<span data-ttu-id="d7d40-134">Most, hogy megismerte a megbízható gyűjtemények működése belső, vessen egy pillantást néhány gyakori előreláthatók őket.</span><span class="sxs-lookup"><span data-stu-id="d7d40-134">Now that you understand how the reliable collections work internally, let’s take a look at some common misuses of them.</span></span> <span data-ttu-id="d7d40-135">Tekintse meg az alábbi kódot:</span><span class="sxs-lookup"><span data-stu-id="d7d40-135">See the code below:</span></span>

```csharp
using (ITransaction tx = StateManager.CreateTransaction()) {
   // AddAsync serializes the name/user, logs the bytes,
   // & sends the bytes to the secondary replicas.
   await m_dic.AddAsync(tx, name, user);

   // The line below updates the property’s value in memory only; the
   // new value is NOT serialized, logged, & sent to secondary replicas.
   user.LastLogin = DateTime.UtcNow;  // Corruption!

   await tx.CommitAsync();
}
```

<span data-ttu-id="d7d40-136">Egy rendszeres .NET könyvtár az használatakor egy kulcs/érték hozzáadása a szótár, és módosítsa a tulajdonság (például LastLogin).</span><span class="sxs-lookup"><span data-stu-id="d7d40-136">When working with a regular .NET dictionary, you can add a key/value to the dictionary and then change the value of a property (such as LastLogin).</span></span> <span data-ttu-id="d7d40-137">Azonban ez a kód nem működnek megfelelően megbízható adatkönyvtárhoz.</span><span class="sxs-lookup"><span data-stu-id="d7d40-137">However, this code will not work correctly with a reliable dictionary.</span></span> <span data-ttu-id="d7d40-138">Ne felejtse el a korábbi vitafórum a AddAsync hívása rendezi sorba a kulcs/érték objektumok bájt tömbök és a helyi fájl majd menti a tömbök és is elküldi azokat a másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="d7d40-138">Remember from the earlier discussion, the call to AddAsync serializes the key/value objects to byte arrays and then saves the arrays to a local file and also sends them to the secondary replicas.</span></span> <span data-ttu-id="d7d40-139">Ha később megváltoztatja egy tulajdonság, a tulajdonság értéke csak; memóriában változik a helyi fájl vagy a replikák küldött adatok nem befolyásolja.</span><span class="sxs-lookup"><span data-stu-id="d7d40-139">If you later change a property, this changes the property’s value in memory only; it does not impact the local file or the data sent to the replicas.</span></span> <span data-ttu-id="d7d40-140">Ha a folyamat leállásából eredő, a memória van érvényteleníteni.</span><span class="sxs-lookup"><span data-stu-id="d7d40-140">If the process crashes, what’s in memory is thrown away.</span></span> <span data-ttu-id="d7d40-141">Új folyamat indításakor, vagy ha egy másik replika elsődleges válik, majd a régi tulajdonság értéke nem elérhető.</span><span class="sxs-lookup"><span data-stu-id="d7d40-141">When a new process starts or if another replica becomes primary, then the old property value is what is available.</span></span>

<span data-ttu-id="d7d40-142">I nem emelje ki elég milyen egyszerűen azt, hogy milyen típusú hibát fent látható.</span><span class="sxs-lookup"><span data-stu-id="d7d40-142">I cannot stress enough how easy it is to make the kind of mistake shown above.</span></span> <span data-ttu-id="d7d40-143">És csak megtudhatja, hogyan a hibát. Ha a folyamat leáll.</span><span class="sxs-lookup"><span data-stu-id="d7d40-143">And, you will only learn about the mistake if/when the process goes down.</span></span> <span data-ttu-id="d7d40-144">A kód írása a megfelelő módon egyszerűen a két sort fordított:</span><span class="sxs-lookup"><span data-stu-id="d7d40-144">The correct way to write the code is simply to reverse the two lines:</span></span>


```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   user.LastLogin = DateTime.UtcNow;  // Do this BEFORE calling AddAsync
   await m_dic.AddAsync(tx, name, user);
   await tx.CommitAsync();
}
```

<span data-ttu-id="d7d40-145">Íme egy másik példa a megjelenítő gyakori hiba:</span><span class="sxs-lookup"><span data-stu-id="d7d40-145">Here is another example showing a common mistake:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> user =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (user.HasValue) {
      // The line below updates the property’s value in memory only; the
      // new value is NOT serialized, logged, & sent to secondary replicas.
      user.Value.LastLogin = DateTime.UtcNow; // Corruption!
      await tx.CommitAsync();
   }
}
```

<span data-ttu-id="d7d40-146">Ebben az esetben rendszeres .NET szótárak, a fenti kódot remekül működik, és egy közös minta: a fejlesztői kulcsot használ kereshet meg egy értéket.</span><span class="sxs-lookup"><span data-stu-id="d7d40-146">Again, with regular .NET dictionaries, the code above works fine and is a common pattern: the developer uses a key to look up a value.</span></span> <span data-ttu-id="d7d40-147">A meglévő értéket, a fejlesztői módosítja egy tulajdonság értékét.</span><span class="sxs-lookup"><span data-stu-id="d7d40-147">If the value exists, the developer changes a property’s value.</span></span> <span data-ttu-id="d7d40-148">Megbízható gyűjteményéhez, hogy ez a kód azonban mutat a szerint már tárgyalt probléma: **ne módosítson egy objektum után adott, megbízható gyűjteményhez.**</span><span class="sxs-lookup"><span data-stu-id="d7d40-148">However, with reliable collections, this code exhibits the same problem as already discussed: **you MUST not modify an object once you have given it to a reliable collection.**</span></span>

<span data-ttu-id="d7d40-149">A megfelelő értéket egy megbízható gyűjtemény frissítése módja egy hivatkozást a meglévő értéket, és fontolja meg többé ez az útmutató által hivatkozott objektum.</span><span class="sxs-lookup"><span data-stu-id="d7d40-149">The correct way to update a value in a reliable collection, is to get a reference to the existing value and consider the object referred to by this reference immutable.</span></span> <span data-ttu-id="d7d40-150">Ezután hozzon létre egy új objektumot, amely az eredeti objektum pontos másolatát.</span><span class="sxs-lookup"><span data-stu-id="d7d40-150">Then, create a new object which is an exact copy of the original object.</span></span> <span data-ttu-id="d7d40-151">Most ezt az új objektumot állapotának módosítása, és írja a gyűjteményhez az új objektumot, hogy azt bájt tömbök, szerializálható lekérdezi a hozzáfűzi a helyi fájlhoz, és elküldve a.</span><span class="sxs-lookup"><span data-stu-id="d7d40-151">Now, you can modify the state of this new object and write the new object into the collection so that it gets serialized to byte arrays, appended to the local file and sent to the replicas.</span></span> <span data-ttu-id="d7d40-152">A módosítás(ok) véglegesítés után a memóriában lévő objektumok, a helyi fájl és a replikák a azonos pontos állapottal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="d7d40-152">After committing the change(s), the in-memory objects, the local file, and all the replicas have the same exact state.</span></span> <span data-ttu-id="d7d40-153">Csak jó!</span><span class="sxs-lookup"><span data-stu-id="d7d40-153">All is good!</span></span>

<span data-ttu-id="d7d40-154">Az alábbi kódot a megfelelő módon frissíteni egy megbízható gyűjtemény értéket jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="d7d40-154">The code below shows the correct way to update a value in a reliable collection:</span></span>

```csharp

using (ITransaction tx = StateManager.CreateTransaction()) {
   // Use the user’s name to look up their data
   ConditionalValue<User> currentUser =
      await m_dic.TryGetValueAsync(tx, name);

   // The user exists in the dictionary, update one of their properties.
   if (currentUser.HasValue) {
      // Create new user object with the same state as the current user object.
      // NOTE: This must be a deep copy; not a shallow copy. Specifically, only
      // immutable state can be shared by currentUser & updatedUser object graphs.
      User updatedUser = new User(currentUser);

      // In the new object, modify any properties you desire
      updatedUser.LastLogin = DateTime.UtcNow;

      // Update the key’s value to the updateUser info
      await m_dic.SetValue(tx, name, updatedUser);

      await tx.CommitAsync();
   }
}
```

## <a name="define-immutable-data-types-to-prevent-programmer-error"></a><span data-ttu-id="d7d40-155">Programozói hiba megelőzése érdekében megváltoztathatatlan adattípusok definiálása</span><span class="sxs-lookup"><span data-stu-id="d7d40-155">Define immutable data types to prevent programmer error</span></span>
<span data-ttu-id="d7d40-156">Szeretnénk ideális esetben a fordítási hibákat, ha véletlenül eredményez, amelyeknek figyelembe kell venni az nem módosítható objektum állapotának mutates kódot.</span><span class="sxs-lookup"><span data-stu-id="d7d40-156">Ideally, we’d like the compiler to report errors when you accidentally produce code that mutates state of an object that you are supposed to consider immutable.</span></span> <span data-ttu-id="d7d40-157">De a C# fordítóprogram nincs ehhez lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d7d40-157">But, the C# compiler does not have the ability to do this.</span></span> <span data-ttu-id="d7d40-158">Igen, lehetséges programozói hibák elkerülése érdekében határozottan ajánlott, hogy a típust határoznak meg kell megváltoztathatatlan típusokra megbízható gyűjteményeket használ.</span><span class="sxs-lookup"><span data-stu-id="d7d40-158">So, to avoid potential programmer bugs, we highly recommend that you define the types you use with reliable collections to be immutable types.</span></span> <span data-ttu-id="d7d40-159">Pontosabban Ez azt jelenti, hogy anyagot core értéktípusok (például számok [Int32, UInt64 stb.] dátum és idő, Guid, TimeSpan érték vagy hasonló).</span><span class="sxs-lookup"><span data-stu-id="d7d40-159">Specifically, this means that you stick to core value types (such as numbers [Int32, UInt64, etc.], DateTime, Guid, TimeSpan, and the like).</span></span> <span data-ttu-id="d7d40-160">És természetesen is karakterlánc használható.</span><span class="sxs-lookup"><span data-stu-id="d7d40-160">And, of course, you can also use String.</span></span> <span data-ttu-id="d7d40-161">Legjobb gyűjtemény tulajdonságok szerializálása során elkerülése érdekében, és azokat deszerializálása is gyakran hátrányosan befolyásolhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="d7d40-161">It is best to avoid collection properties as serializing and deserializing them can frequently can hurt performance.</span></span> <span data-ttu-id="d7d40-162">Azonban ha gyűjteménytulajdonságokkal használni kívánt, erősen ajánlott a használata. NET által nem módosítható gyűjteményeket könyvtár ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span><span class="sxs-lookup"><span data-stu-id="d7d40-162">However, if you want to use collection properties, we highly recommend the use of .NET’s immutable collections library ([System.Collections.Immutable](https://www.nuget.org/packages/System.Collections.Immutable/)).</span></span> <span data-ttu-id="d7d40-163">Ebben a könyvtárban http://nuget.org letölthető.</span><span class="sxs-lookup"><span data-stu-id="d7d40-163">This library is available for download from http://nuget.org.</span></span> <span data-ttu-id="d7d40-164">Javasoljuk továbbá, az osztályok zárolásra és a mezőket csak olvasható amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="d7d40-164">We also recommend sealing your classes and making fields read-only whenever possible.</span></span>

<span data-ttu-id="d7d40-165">Az alábbi UserInfo típus bemutatja, hogyan kell egy nem módosítható típus kihasználja a fenti ajánlásokat.</span><span class="sxs-lookup"><span data-stu-id="d7d40-165">The UserInfo type below demonstrates how to define an immutable type taking advantage of aforementioned recommendations.</span></span>

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
      // Convert the deserialized collection to an immutable collection
      ItemsBidding = ItemsBidding.ToImmutableList();
   }

   [DataMember]
   public readonly String Email;

   // Ideally, this would be a readonly field but it can't be because OnDeserialized
   // has to set it. So instead, the getter is public and the setter is private.
   [DataMember]
   public IEnumerable<ItemId> ItemsBidding { get; private set; }

   // Since each UserInfo object is immutable, we add a new ItemId to the ItemsBidding
   // collection by creating a new immutable UserInfo object with the added ItemId.
   public UserInfo AddItemBidding(ItemId itemId) {
      return new UserInfo(Email, ((ImmutableList<ItemId>)ItemsBidding).Add(itemId));
   }
}
```

<span data-ttu-id="d7d40-166">A ItemId típusa nem is módosítható típus Itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="d7d40-166">The ItemId type is also an immutable type as shown here:</span></span>

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

## <a name="schema-versioning-upgrades"></a><span data-ttu-id="d7d40-167">Séma versioning (frissítés)</span><span class="sxs-lookup"><span data-stu-id="d7d40-167">Schema versioning (upgrades)</span></span>
<span data-ttu-id="d7d40-168">Belsőleg megbízható gyűjtemények szerializálni a objektumok használják. NET a DataContractSerializer.</span><span class="sxs-lookup"><span data-stu-id="d7d40-168">Internally, Reliable Collections serialize your objects using .NET’s DataContractSerializer.</span></span> <span data-ttu-id="d7d40-169">A szerializált objektumok őrződnek meg az elsődleges másodpéldány helyi lemezre, és a rendszer szintén továbbíthatja a Microsoftnak a másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="d7d40-169">The serialized objects are persisted to the primary replica’s local disk and are also transmitted to the secondary replicas.</span></span> <span data-ttu-id="d7d40-170">A szolgáltatás Miután kiforrottá válik, valószínű, milyen típusú adatok (séma), a szolgáltatásnak szüksége van módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="d7d40-170">As your service matures, it’s likely you’ll want to change the kind of data (schema) your service requires.</span></span> <span data-ttu-id="d7d40-171">Nagy gondot az adatok verziószámozásának kell készíthető elő.</span><span class="sxs-lookup"><span data-stu-id="d7d40-171">You must approach versioning of your data with great care.</span></span> <span data-ttu-id="d7d40-172">Mindenekelőtt mindig kell tudni régi adatokat.</span><span class="sxs-lookup"><span data-stu-id="d7d40-172">First and foremost, you must always be able to deserialize old data.</span></span> <span data-ttu-id="d7d40-173">Pontosabban, ez azt jelenti, hogy a deszerializálás kódot kell végtelenül visszamenőlegesen kompatibilis: a szolgáltatás kód verziója 333 egy megbízható gyűjtemény helyezi el őket a szolgáltatáskód hibáit 1 verziójának 5 éve adatok alapján képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d7d40-173">Specifically, this means your deserialization code must be infinitely backward compatible: Version 333 of your service code must be able to operate on data placed in a reliable collection by version 1 of your service code 5 years ago.</span></span>

<span data-ttu-id="d7d40-174">Ezenkívül szolgáltatáskódot egyszerre lehet frissített több frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="d7d40-174">Furthermore, service code is upgraded one upgrade domain at a time.</span></span> <span data-ttu-id="d7d40-175">Igen a frissítés során, hogy a szolgáltatáskód hibáit, hiszen egyszerre két különböző verziója.</span><span class="sxs-lookup"><span data-stu-id="d7d40-175">So, during an upgrade, you have two different versions of your service code running simultaneously.</span></span> <span data-ttu-id="d7d40-176">A szolgáltatáskód hibáit új verzióját használja az új sémával, mert a szolgáltatáskód hibáit régi verziói esetleg nem tudja kezelni az új sémával rendelkező kell elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="d7d40-176">You must avoid having the new version of your service code use the new schema as old versions of your service code might not be able to handle the new schema.</span></span> <span data-ttu-id="d7d40-177">Ha lehetséges, akkor tervezzen előre kompatibilis 1 verziójával kell a szolgáltatás minden verziója.</span><span class="sxs-lookup"><span data-stu-id="d7d40-177">When possible, you should design each version of your service to be forward compatible by 1 version.</span></span> <span data-ttu-id="d7d40-178">Pontosabban Ez azt jelenti, hogy 1-es verzió, a szolgáltatás kód egyszerűen figyelmen kívül bármely séma elemei nem explicit módon kezeli képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d7d40-178">Specifically, this means that V1 of your service code should be able to simply ignore any schema elements it does not explicitly handle.</span></span> <span data-ttu-id="d7d40-179">Azonban képesnek kell lennie minden explicit módon kapcsolatos nem tud adatot és egyszerűen vissza kimenő szótár kulcs vagy érték frissítésekor írási mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d7d40-179">However, it must be able to save any data it doesn’t explicitly know about and simply write it back out when updating a dictionary key or value.</span></span>

> [!WARNING]
> <span data-ttu-id="d7d40-180">Amíg a séma, kulcs módosíthatja, bizonyosodjon meg, hogy a kulcs kivonatkód és egyenlő algoritmusok stabil.</span><span class="sxs-lookup"><span data-stu-id="d7d40-180">While you can modify the schema of a key, you must ensure that your key’s hash code and equals algorithms are stable.</span></span> <span data-ttu-id="d7d40-181">Ha módosítja, hogyan működnek ezek az algoritmusok valamelyikét, csak akkor képes a kulcsot a megbízható szótár belül minden eddiginél újra.</span><span class="sxs-lookup"><span data-stu-id="d7d40-181">If you change how either of these algorithms operate, you will not be able to look up the key within the reliable dictionary ever again.</span></span>
>
>

<span data-ttu-id="d7d40-182">Másik lehetőségként hajthat végre, milyen gyakran emlegetik úgy, a 2-fázis frissítés.</span><span class="sxs-lookup"><span data-stu-id="d7d40-182">Alternatively, you can perform what is typically referred to as a 2-phase upgrade.</span></span> <span data-ttu-id="d7d40-183">A 2-fázis frissítést, a szolgáltatás V1-es rendszerről frissít V2: V2 a kódot tartalmaz, amely meg tudja az új sémaváltozás foglalkozik, de ez a kód nem hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="d7d40-183">With a 2-phase upgrade, you upgrade your service from V1 to V2: V2 contains the code that knows how to deal with the new schema change but this code doesn’t execute.</span></span> <span data-ttu-id="d7d40-184">A V2 kód V1 adatok olvasását, akkor működik, és V1 adatokat.</span><span class="sxs-lookup"><span data-stu-id="d7d40-184">When the V2 code reads V1 data, it operates on it and writes V1 data.</span></span> <span data-ttu-id="d7d40-185">Majd a frissítés befejezése után minden frissítési tartományok között, akkor is valamilyen módon azt a futó V2-példányokban kívánja, hogy a frissítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="d7d40-185">Then, after the upgrade is complete across all upgrade domains, you can somehow signal to the running V2 instances that the upgrade is complete.</span></span> <span data-ttu-id="d7d40-186">(Jel egyik módja azt a konfigurációs frissítés fokozatosan; ez hasznossá ezt a 2-fázis frissítésének.) Most a V2 példányok is V1-adatok olvasása, V2 adatokat átalakíthatja, el és kiírni V2 adatként.</span><span class="sxs-lookup"><span data-stu-id="d7d40-186">(One way to signal this is to roll out a configuration upgrade; this is what makes this a 2-phase upgrade.) Now, the V2 instances can read V1 data, convert it to V2 data, operate on it, and write it out as V2 data.</span></span> <span data-ttu-id="d7d40-187">Ha más esetekben V2-adatok olvasása, nem kell konvertálni, csak működik rajta, és kiírni a V2-adatok.</span><span class="sxs-lookup"><span data-stu-id="d7d40-187">When other instances read V2 data, they do not need to convert it, they just operate on it, and write out V2 data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7d40-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d7d40-188">Next Steps</span></span>
<span data-ttu-id="d7d40-189">Kompatibilis adatokat továbbítson a szerződések létrehozásával kapcsolatos további tudnivalókért lásd: [előre kompatibilis adategyezményeinek](https://msdn.microsoft.com/library/ms731083.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7d40-189">To learn about creating forward compatible data contracts, see [Forward-Compatible Data Contracts](https://msdn.microsoft.com/library/ms731083.aspx).</span></span>

<span data-ttu-id="d7d40-190">Gyakorlati tanácsok a versioning adategyezményeinek kapcsolatban [adatok szerződés Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7d40-190">To learn best practices on versioning data contracts, see [Data Contract Versioning](https://msdn.microsoft.com/library/ms731138.aspx).</span></span>

<span data-ttu-id="d7d40-191">Verzió hibatűrő adategyezményeinek végrehajtására, lásd: [verzió hibatűrő szerializálási visszahívások](https://msdn.microsoft.com/library/ms733734.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7d40-191">To learn how to implement version tolerant data contracts, see [Version-Tolerant Serialization Callbacks](https://msdn.microsoft.com/library/ms733734.aspx).</span></span>

<span data-ttu-id="d7d40-192">Adjon meg olyan adatszerkezet, amely képes együttműködni több verziója között lásd: [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span><span class="sxs-lookup"><span data-stu-id="d7d40-192">To learn how to provide a data structure that can interoperate across multiple versions, see [IExtensibleDataObject](https://msdn.microsoft.com/library/system.runtime.serialization.iextensibledataobject.aspx).</span></span>
