---
title: "Megbízható szolgáltatások értesítések |} Microsoft Docs"
description: "Service Fabric Reliable Services értesítések fogalmi dokumentációja"
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,vturecek
ms.assetid: cdc918dd-5e81-49c8-a03d-7ddcd12a9a76
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/29/2017
ms.author: mcoskun
ms.openlocfilehash: c6a53d851510ed5e6eec1f3ac0f636ad034a6d4c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="reliable-services-notifications"></a><span data-ttu-id="29fb5-103">Megbízható szolgáltatások értesítések</span><span class="sxs-lookup"><span data-stu-id="29fb5-103">Reliable Services notifications</span></span>
<span data-ttu-id="29fb5-104">Értesítések engedélyezése az ügyfelek nyomon követni a kíváncsiak vagyunk objektum végrehajtott módosításokat.</span><span class="sxs-lookup"><span data-stu-id="29fb5-104">Notifications allow clients to track the changes that are being made to an object that they're interested in.</span></span> <span data-ttu-id="29fb5-105">Kétféle típusú objektumok támogatja értesítéseket: *megbízható állapotkezelője* és *megbízható szótár*.</span><span class="sxs-lookup"><span data-stu-id="29fb5-105">Two types of objects support notifications: *Reliable State Manager* and *Reliable Dictionary*.</span></span>

<span data-ttu-id="29fb5-106">Értesítések használata gyakori okai a következők:</span><span class="sxs-lookup"><span data-stu-id="29fb5-106">Common reasons for using notifications are:</span></span>

* <span data-ttu-id="29fb5-107">Épület materializált nézetek, például a másodlagos indexek vagy szűrt nézeteinek a másodpéldány állapotát összesíti.</span><span class="sxs-lookup"><span data-stu-id="29fb5-107">Building materialized views, such as secondary indexes or aggregated filtered views of the replica's state.</span></span> <span data-ttu-id="29fb5-108">Példa: egy rendezett index összes kulcsok megbízható szótárban.</span><span class="sxs-lookup"><span data-stu-id="29fb5-108">An example is a sorted index of all keys in Reliable Dictionary.</span></span>
* <span data-ttu-id="29fb5-109">Küldő figyelési adatok, például az elmúlt órában hozzáadott felhasználók száma.</span><span class="sxs-lookup"><span data-stu-id="29fb5-109">Sending monitoring data, such as the number of users added in the last hour.</span></span>

<span data-ttu-id="29fb5-110">Értesítések elindulása részeként műveleteket alkalmazása esetén.</span><span class="sxs-lookup"><span data-stu-id="29fb5-110">Notifications are fired as part of applying operations.</span></span> <span data-ttu-id="29fb5-111">Miatt, amely értesítések gyors lehetséges és szinkron események nem tartalmazhatja a költséges műveleteket kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="29fb5-111">Because of that, notifications should be handled as fast as possible, and synchronous events shouldn't include any expensive operations.</span></span>

## <a name="reliable-state-manager-notifications"></a><span data-ttu-id="29fb5-112">Megbízható állapot Manager értesítéseire</span><span class="sxs-lookup"><span data-stu-id="29fb5-112">Reliable State Manager notifications</span></span>
<span data-ttu-id="29fb5-113">Megbízható állapotkezelő a következő események értesítéseket biztosít:</span><span class="sxs-lookup"><span data-stu-id="29fb5-113">Reliable State Manager provides notifications for the following events:</span></span>

* <span data-ttu-id="29fb5-114">Tranzakció</span><span class="sxs-lookup"><span data-stu-id="29fb5-114">Transaction</span></span>
  * <span data-ttu-id="29fb5-115">Véglegesítés</span><span class="sxs-lookup"><span data-stu-id="29fb5-115">Commit</span></span>
* <span data-ttu-id="29fb5-116">Állapot manager</span><span class="sxs-lookup"><span data-stu-id="29fb5-116">State manager</span></span>
  * <span data-ttu-id="29fb5-117">Építse újra</span><span class="sxs-lookup"><span data-stu-id="29fb5-117">Rebuild</span></span>
  * <span data-ttu-id="29fb5-118">A megbízható állapot hozzáadása</span><span class="sxs-lookup"><span data-stu-id="29fb5-118">Addition of a reliable state</span></span>
  * <span data-ttu-id="29fb5-119">A megbízható állapot törlése</span><span class="sxs-lookup"><span data-stu-id="29fb5-119">Removal of a reliable state</span></span>

<span data-ttu-id="29fb5-120">Megbízható állapotkezelője követi nyomon, hogy az aktuális aktív tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="29fb5-120">Reliable State Manager tracks the current inflight transactions.</span></span> <span data-ttu-id="29fb5-121">Mindössze annyi a változás aktiválódik értesítést okozó tranzakció állapotban egy tranzakció véglegesítés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="29fb5-121">The only change in transaction state that causes a notification to be fired is a transaction being committed.</span></span>

<span data-ttu-id="29fb5-122">Megbízható állapotkezelője tart fenn például megbízható szótár és megbízható várólista megbízható állapotok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="29fb5-122">Reliable State Manager maintains a collection of reliable states like Reliable Dictionary and Reliable Queue.</span></span> <span data-ttu-id="29fb5-123">Megbízható állapotkezelője értesítések következik be, amikor megváltozik a gyűjtemény: olyan megbízható állapotban van hozzáadásakor vagy eltávolításakor, vagy a teljes gyűjteményt újraépítésekor.</span><span class="sxs-lookup"><span data-stu-id="29fb5-123">Reliable State Manager fires notifications when this collection changes: a reliable state is added or removed, or the entire collection is rebuilt.</span></span>
<span data-ttu-id="29fb5-124">A megbízható állapotkezelője gyűjtemény újraépítésekor három esetben:</span><span class="sxs-lookup"><span data-stu-id="29fb5-124">The Reliable State Manager collection is rebuilt in three cases:</span></span>

* <span data-ttu-id="29fb5-125">Helyreállítás: Replika indításakor azt anélkül állíthatóak vissza korábbi állapotba a lemezről.</span><span class="sxs-lookup"><span data-stu-id="29fb5-125">Recovery: When a replica starts, it recovers its previous state from the disk.</span></span> <span data-ttu-id="29fb5-126">A helyreállítás végén, használja **NotifyStateManagerChangedEventArgs** az érvényesítést a helyreállított megbízható állapotok készletét tartalmazó esemény.</span><span class="sxs-lookup"><span data-stu-id="29fb5-126">At the end of recovery, it uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of recovered reliable states.</span></span>
* <span data-ttu-id="29fb5-127">Másolás teljes: replika csatlakozhat a konfigurációs készlet, mielőtt rendelkezik kialakítani.</span><span class="sxs-lookup"><span data-stu-id="29fb5-127">Full copy: Before a replica can join the configuration set, it has to be built.</span></span> <span data-ttu-id="29fb5-128">Egyes esetekben ehhez az elsődleges replikából a tétlen másodlagos replikán alkalmazni kívánt megbízható állapotkezelője állapot teljes másolata.</span><span class="sxs-lookup"><span data-stu-id="29fb5-128">Sometimes, this requires a full copy of Reliable State Manager's state from the primary replica to be applied to the idle secondary replica.</span></span> <span data-ttu-id="29fb5-129">Használja a másodlagos replikán megbízható állapotkezelője **NotifyStateManagerChangedEventArgs** az érvényesítést egy eseményt, amely azokat a megbízható állapotok azért szerzett, az elsődleges replikából.</span><span class="sxs-lookup"><span data-stu-id="29fb5-129">Reliable State Manager on the secondary replica uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of reliable states that it acquired from the primary replica.</span></span>
* <span data-ttu-id="29fb5-130">Visszaállítás: A vész-helyreállítási helyzetekben a replika visszaállítható egy biztonsági másolatból keresztül **RestoreAsync**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-130">Restore: In disaster recovery scenarios, the replica's state can be restored from a backup via **RestoreAsync**.</span></span> <span data-ttu-id="29fb5-131">Ilyen esetben az elsődleges replikán megbízható állapotkezelője használ **NotifyStateManagerChangedEventArgs** az érvényesítést, amely azokat a megbízható állapotok azt a biztonsági másolatból visszaállított esemény.</span><span class="sxs-lookup"><span data-stu-id="29fb5-131">In such cases, Reliable State Manager on the primary replica uses **NotifyStateManagerChangedEventArgs** to fire an event that contains the set of reliable states that it restored from the backup.</span></span>

<span data-ttu-id="29fb5-132">Tranzakció értesítések és/vagy az állapot manager értesítéseinek regisztrálásához regisztrálni kell a **TransactionChanged** vagy **StateManagerChanged** megbízható állapotkezelője eseményeit.</span><span class="sxs-lookup"><span data-stu-id="29fb5-132">To register for transaction notifications and/or state manager notifications, you need to register with the **TransactionChanged** or **StateManagerChanged** events on Reliable State Manager.</span></span> <span data-ttu-id="29fb5-133">A közös ezek eseménykezelők regisztrálására helye az állapotalapú szolgáltatás konstruktor.</span><span class="sxs-lookup"><span data-stu-id="29fb5-133">A common place to register with these event handlers is the constructor of your stateful service.</span></span> <span data-ttu-id="29fb5-134">Ha regisztrálja a konstruktora, teljesíti-e nem élettartama során változás által okozott értesítésekhez **IReliableStateManager**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-134">When you register on the constructor, you won't miss any notification that's caused by a change during the lifetime of **IReliableStateManager**.</span></span>

```C#
public MyService(StatefulServiceContext context)
    : base(MyService.EndpointName, context, CreateReliableStateManager(context))
{
    this.StateManager.TransactionChanged += this.OnTransactionChangedHandler;
    this.StateManager.StateManagerChanged += this.OnStateManagerChangedHandler;
}
```

<span data-ttu-id="29fb5-135">A **TransactionChanged** eseménykezelő használ **NotifyTransactionChangedEventArgs** arra, hogy az esemény részleteit.</span><span class="sxs-lookup"><span data-stu-id="29fb5-135">The **TransactionChanged** event handler uses **NotifyTransactionChangedEventArgs** to provide details about the event.</span></span> <span data-ttu-id="29fb5-136">A művelet tulajdonságot tartalmaz (például **NotifyTransactionChangedAction.Commit**), amely megadja, hogy olyan változást.</span><span class="sxs-lookup"><span data-stu-id="29fb5-136">It contains the action property (for example, **NotifyTransactionChangedAction.Commit**) that specifies the type of change.</span></span> <span data-ttu-id="29fb5-137">A transaction tulajdonság, amely módosult a tranzakció egy hivatkozást is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="29fb5-137">It also contains the transaction property that provides a reference to the transaction that changed.</span></span>

> [!NOTE]
> <span data-ttu-id="29fb5-138">Napjainkban **TransactionChanged** események aktiválódnak csak akkor, ha a tranzakció.</span><span class="sxs-lookup"><span data-stu-id="29fb5-138">Today, **TransactionChanged** events are raised only if the transaction is committed.</span></span> <span data-ttu-id="29fb5-139">A művelet megegyezik **NotifyTransactionChangedAction.Commit**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-139">The action is then equal to **NotifyTransactionChangedAction.Commit**.</span></span> <span data-ttu-id="29fb5-140">De a jövőben események előfordulhat, hogy generál tranzakciót állapotváltozások más típusú.</span><span class="sxs-lookup"><span data-stu-id="29fb5-140">But in the future, events might be raised for other types of transaction state changes.</span></span> <span data-ttu-id="29fb5-141">Javasoljuk, hogy a művelet ellenőrzése és az esemény feldolgozása, csak akkor, ha egy, a várt.</span><span class="sxs-lookup"><span data-stu-id="29fb5-141">We recommend checking the action and processing the event only if it's one that you expect.</span></span>
> 
> 

<span data-ttu-id="29fb5-142">Példa **TransactionChanged** eseménykezelő.</span><span class="sxs-lookup"><span data-stu-id="29fb5-142">Following is an example **TransactionChanged** event handler.</span></span>

```C#
private void OnTransactionChangedHandler(object sender, NotifyTransactionChangedEventArgs e)
{
    if (e.Action == NotifyTransactionChangedAction.Commit)
    {
        this.lastCommitLsn = e.Transaction.CommitSequenceNumber;
        this.lastTransactionId = e.Transaction.TransactionId;

        this.lastCommittedTransactionList.Add(e.Transaction.TransactionId);
    }
}
```

<span data-ttu-id="29fb5-143">A **StateManagerChanged** eseménykezelő használ **NotifyStateManagerChangedEventArgs** arra, hogy az esemény részleteit.</span><span class="sxs-lookup"><span data-stu-id="29fb5-143">The **StateManagerChanged** event handler uses **NotifyStateManagerChangedEventArgs** to provide details about the event.</span></span>
<span data-ttu-id="29fb5-144">**NotifyStateManagerChangedEventArgs** két alosztályok rendelkezik: **NotifyStateManagerRebuildEventArgs** és **NotifyStateManagerSingleEntityChangedEventArgs**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-144">**NotifyStateManagerChangedEventArgs** has two subclasses: **NotifyStateManagerRebuildEventArgs** and **NotifyStateManagerSingleEntityChangedEventArgs**.</span></span>
<span data-ttu-id="29fb5-145">A művelet tulajdonsággal a **NotifyStateManagerChangedEventArgs** konvertálni **NotifyStateManagerChangedEventArgs** a helytelen alosztályának létrehozására:</span><span class="sxs-lookup"><span data-stu-id="29fb5-145">You use the action property in **NotifyStateManagerChangedEventArgs** to cast **NotifyStateManagerChangedEventArgs** to the correct subclass:</span></span>

* <span data-ttu-id="29fb5-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-146">**NotifyStateManagerChangedAction.Rebuild**: **NotifyStateManagerRebuildEventArgs**</span></span>
* <span data-ttu-id="29fb5-147">**NotifyStateManagerChangedAction.Add** és **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-147">**NotifyStateManagerChangedAction.Add** and **NotifyStateManagerChangedAction.Remove**: **NotifyStateManagerSingleEntityChangedEventArgs**</span></span>

<span data-ttu-id="29fb5-148">Példa **StateManagerChanged** értesítéskezelő.</span><span class="sxs-lookup"><span data-stu-id="29fb5-148">Following is an example **StateManagerChanged** notification handler.</span></span>

```C#
public void OnStateManagerChangedHandler(object sender, NotifyStateManagerChangedEventArgs e)
{
    if (e.Action == NotifyStateManagerChangedAction.Rebuild)
    {
        this.ProcessStataManagerRebuildNotification(e);

        return;
    }

    this.ProcessStateManagerSingleEntityNotification(e);
}
```

## <a name="reliable-dictionary-notifications"></a><span data-ttu-id="29fb5-149">Megbízható szótár értesítések</span><span class="sxs-lookup"><span data-stu-id="29fb5-149">Reliable Dictionary notifications</span></span>
<span data-ttu-id="29fb5-150">Megbízható szótár a következő események értesítéseket biztosít:</span><span class="sxs-lookup"><span data-stu-id="29fb5-150">Reliable Dictionary provides notifications for the following events:</span></span>

* <span data-ttu-id="29fb5-151">Újraépítés: Meghívva **ReliableDictionary** helyreállt állapotában a helyreállított vagy másolt helyi állapot vagy a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="29fb5-151">Rebuild: Called when **ReliableDictionary** has recovered its state from a recovered or copied local state or backup.</span></span>
* <span data-ttu-id="29fb5-152">Törlés: Meghívva állapotának **ReliableDictionary** keresztül törölve lett a **ClearAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="29fb5-152">Clear: Called when the state of **ReliableDictionary** has been cleared through the **ClearAsync** method.</span></span>
* <span data-ttu-id="29fb5-153">Adja hozzá: Hívható meg, ha egy elem hozzáadva **ReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-153">Add: Called when an item has been added to **ReliableDictionary**.</span></span>
* <span data-ttu-id="29fb5-154">Frissítés: Hívható meg, ha az elem **IReliableDictionary** frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="29fb5-154">Update: Called when an item in **IReliableDictionary** has been updated.</span></span>
* <span data-ttu-id="29fb5-155">Eltávolítás: Hívható meg, ha az elem **IReliableDictionary** törölve lett.</span><span class="sxs-lookup"><span data-stu-id="29fb5-155">Remove: Called when an item in **IReliableDictionary** has been deleted.</span></span>

<span data-ttu-id="29fb5-156">Ahhoz, hogy megbízható szótár értesítéseket, és regisztrálnia kell a **DictionaryChanged** eseménykezelő a **IReliableDictionary**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-156">To get Reliable Dictionary notifications, you need to register with the **DictionaryChanged** event handler on **IReliableDictionary**.</span></span> <span data-ttu-id="29fb5-157">Van olyan közös helyen ezek eseménykezelők regisztrálni a **ReliableStateManager.StateManagerChanged** értesítési hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="29fb5-157">A common place to register with these event handlers is in the **ReliableStateManager.StateManagerChanged** add notification.</span></span>
<span data-ttu-id="29fb5-158">Ha regisztrálja **IReliableDictionary** hozzáadódik **IReliableStateManager** biztosítja, hogy nem hagyott ki belőle értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="29fb5-158">Registering when **IReliableDictionary** is added to **IReliableStateManager** ensures that you won't miss any notifications.</span></span>

```C#
private void ProcessStateManagerSingleEntityNotification(NotifyStateManagerChangedEventArgs e)
{
    var operation = e as NotifyStateManagerSingleEntityChangedEventArgs;

    if (operation.Action == NotifyStateManagerChangedAction.Add)
    {
        if (operation.ReliableState is IReliableDictionary<TKey, TValue>)
        {
            var dictionary = (IReliableDictionary<TKey, TValue>)operation.ReliableState;
            dictionary.RebuildNotificationAsyncCallback = this.OnDictionaryRebuildNotificationHandlerAsync;
            dictionary.DictionaryChanged += this.OnDictionaryChangedHandler;
            }
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="29fb5-159">**ProcessStateManagerSingleEntityNotification** a minta módszer, amely az előző **OnStateManagerChangedHandler** példa hívások.</span><span class="sxs-lookup"><span data-stu-id="29fb5-159">**ProcessStateManagerSingleEntityNotification** is the sample method that the preceding **OnStateManagerChangedHandler** example calls.</span></span>
> 
> 

<span data-ttu-id="29fb5-160">Az előző kód beállítása a **IReliableNotificationAsyncCallback** kommunikáljanak, jelszavat **DictionaryChanged**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-160">The preceding code sets the **IReliableNotificationAsyncCallback** interface, along with **DictionaryChanged**.</span></span> <span data-ttu-id="29fb5-161">Mivel **NotifyDictionaryRebuildEventArgs** tartalmaz egy **IAsyncEnumerable** illesztő – amely aszinkron módon--számba kell belüli értesítések elindulása esetén keresztül **RebuildNotificationAsyncCallback** helyett **OnDictionaryChangedHandler**.</span><span class="sxs-lookup"><span data-stu-id="29fb5-161">Because **NotifyDictionaryRebuildEventArgs** contains an **IAsyncEnumerable** interface--which needs to be enumerated asynchronously--rebuild notifications are fired through **RebuildNotificationAsyncCallback** instead of **OnDictionaryChangedHandler**.</span></span>

```C#
public async Task OnDictionaryRebuildNotificationHandlerAsync(
    IReliableDictionary<TKey, TValue> origin,
    NotifyDictionaryRebuildEventArgs<TKey, TValue> rebuildNotification)
{
    this.secondaryIndex.Clear();

    var enumerator = e.State.GetAsyncEnumerator();
    while (await enumerator.MoveNextAsync(CancellationToken.None))
    {
        this.secondaryIndex.Add(enumerator.Current.Key, enumerator.Current.Value);
    }
}
```

> [!NOTE]
> <span data-ttu-id="29fb5-162">Az előzőekben látható kód rebuild értesítés feldolgozása részeként az először a karbantartott összesített állapota nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="29fb5-162">In the preceding code, as part of processing the rebuild notification, first the maintained aggregated state is cleared.</span></span> <span data-ttu-id="29fb5-163">A megbízható gyűjtemény újraépíti az új állapot, mert az összes korábbi értesítés irrelevánsak.</span><span class="sxs-lookup"><span data-stu-id="29fb5-163">Because the reliable collection is being rebuilt with a new state, all previous notifications are irrelevant.</span></span>
> 
> 

<span data-ttu-id="29fb5-164">A **DictionaryChanged** eseménykezelő használ **NotifyDictionaryChangedEventArgs** arra, hogy az esemény részleteit.</span><span class="sxs-lookup"><span data-stu-id="29fb5-164">The **DictionaryChanged** event handler uses **NotifyDictionaryChangedEventArgs** to provide details about the event.</span></span>
<span data-ttu-id="29fb5-165">**NotifyDictionaryChangedEventArgs** öt alosztályok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="29fb5-165">**NotifyDictionaryChangedEventArgs** has five subclasses.</span></span> <span data-ttu-id="29fb5-166">A művelet tulajdonsággal **NotifyDictionaryChangedEventArgs** konvertálni **NotifyDictionaryChangedEventArgs** a helytelen alosztályának létrehozására:</span><span class="sxs-lookup"><span data-stu-id="29fb5-166">Use the action property in **NotifyDictionaryChangedEventArgs** to cast **NotifyDictionaryChangedEventArgs** to the correct subclass:</span></span>

* <span data-ttu-id="29fb5-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-167">**NotifyDictionaryChangedAction.Rebuild**: **NotifyDictionaryRebuildEventArgs**</span></span>
* <span data-ttu-id="29fb5-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-168">**NotifyDictionaryChangedAction.Clear**: **NotifyDictionaryClearEventArgs**</span></span>
* <span data-ttu-id="29fb5-169">**NotifyDictionaryChangedAction.Add** és **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-169">**NotifyDictionaryChangedAction.Add** and **NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemAddedEventArgs**</span></span>
* <span data-ttu-id="29fb5-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-170">**NotifyDictionaryChangedAction.Update**: **NotifyDictionaryItemUpdatedEventArgs**</span></span>
* <span data-ttu-id="29fb5-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span><span class="sxs-lookup"><span data-stu-id="29fb5-171">**NotifyDictionaryChangedAction.Remove**: **NotifyDictionaryItemRemovedEventArgs**</span></span>

```C#
public void OnDictionaryChangedHandler(object sender, NotifyDictionaryChangedEventArgs<TKey, TValue> e)
{
    switch (e.Action)
    {
        case NotifyDictionaryChangedAction.Clear:
            var clearEvent = e as NotifyDictionaryClearEventArgs<TKey, TValue>;
            this.ProcessClearNotification(clearEvent);
            return;

        case NotifyDictionaryChangedAction.Add:
            var addEvent = e as NotifyDictionaryItemAddedEventArgs<TKey, TValue>;
            this.ProcessAddNotification(addEvent);
            return;

        case NotifyDictionaryChangedAction.Update:
            var updateEvent = e as NotifyDictionaryItemUpdatedEventArgs<TKey, TValue>;
            this.ProcessUpdateNotification(updateEvent);
            return;

        case NotifyDictionaryChangedAction.Remove:
            var deleteEvent = e as NotifyDictionaryItemRemovedEventArgs<TKey, TValue>;
            this.ProcessRemoveNotification(deleteEvent);
            return;

        default:
            break;
    }
}
```

## <a name="recommendations"></a><span data-ttu-id="29fb5-172">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="29fb5-172">Recommendations</span></span>
* <span data-ttu-id="29fb5-173">*Tegye* értesítési események lehető legnagyobb befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="29fb5-173">*Do* complete notification events as fast as possible.</span></span>
* <span data-ttu-id="29fb5-174">*Ne* hajtható végre a költséges műveleteket (például i/o-műveletek) szinkron események részeként.</span><span class="sxs-lookup"><span data-stu-id="29fb5-174">*Do not* execute any expensive operations (for example, I/O operations) as part of synchronous events.</span></span>
* <span data-ttu-id="29fb5-175">*Tegye* a művelet típusának ellenőrzése előtt feldolgozni a eseményt.</span><span class="sxs-lookup"><span data-stu-id="29fb5-175">*Do* check the action type before you process the event.</span></span> <span data-ttu-id="29fb5-176">Új művelettípusok előfordulhat, hogy a jövőben hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="29fb5-176">New action types might be added in the future.</span></span>

<span data-ttu-id="29fb5-177">Az alábbiakban néhány dolgot figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="29fb5-177">Here are some things to keep in mind:</span></span>

* <span data-ttu-id="29fb5-178">Értesítések elindulása esetén egy művelet végrehajtásának részeként.</span><span class="sxs-lookup"><span data-stu-id="29fb5-178">Notifications are fired as part of the execution of an operation.</span></span> <span data-ttu-id="29fb5-179">Például egy visszaállítási értesítést a visszaállítási művelet utolsó lépéseként lép működésbe.</span><span class="sxs-lookup"><span data-stu-id="29fb5-179">For example, a restore notification is fired as the last step of a restore operation.</span></span> <span data-ttu-id="29fb5-180">A visszaállítás nem fejeződik be, amíg a értesítő eseményt dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="29fb5-180">A restore will not finish until the notification event is processed.</span></span>
* <span data-ttu-id="29fb5-181">Értesítések a alkalmazása műveletek részeként elindulása esetén, mert az ügyfelek csak a helyileg véglegesített műveletek értesítések:.</span><span class="sxs-lookup"><span data-stu-id="29fb5-181">Because notifications are fired as part of the applying operations, clients see only notifications for locally committed operations.</span></span> <span data-ttu-id="29fb5-182">Ezért műveletek garantáltan csak helyi véglegesített (más szóval naplózása), előfordulhat, hogy, vagy előfordulhat, hogy nem lehet visszavonni a jövőben.</span><span class="sxs-lookup"><span data-stu-id="29fb5-182">And because operations are guaranteed only to be locally committed (in other words, logged), they might or might not be undone in the future.</span></span>
* <span data-ttu-id="29fb5-183">A visszaállítási útvonalon egyetlen értesítést minden alkalmazott művelethez lép működésbe.</span><span class="sxs-lookup"><span data-stu-id="29fb5-183">On the redo path, a single notification is fired for each applied operation.</span></span> <span data-ttu-id="29fb5-184">Ez azt jelenti, hogy ha tranzakció T1 Create(X) Delete(X) és Create(X) tartalmaz, kap egy értesítési X, egy, a törlésre, és egy újbóli létrehozásához létrehozásához abban a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="29fb5-184">This means that if transaction T1 includes Create(X), Delete(X), and Create(X), you'll get one notification for the creation of X, one for the deletion, and one for the creation again, in that order.</span></span>
* <span data-ttu-id="29fb5-185">Az egyes tranzakciókra vonatkozóan, több műveletet tartalmazó műveletek lesznek alkalmazva a sorrendet, amelyben azok érkezett a felhasználótól az elsődleges replikán.</span><span class="sxs-lookup"><span data-stu-id="29fb5-185">For transactions that contain multiple operations, operations are applied in the order in which they were received on the primary replica from the user.</span></span>
* <span data-ttu-id="29fb5-186">Feldolgozási hamis folyamat részeként néhány művelet lehet, hogy vonható vissza.</span><span class="sxs-lookup"><span data-stu-id="29fb5-186">As part of processing false progress, some operations might be undone.</span></span> <span data-ttu-id="29fb5-187">Értesítések a visszavonási műveletek, a replika állapotának visszaállítása egy stabil ponton aktiválódnak.</span><span class="sxs-lookup"><span data-stu-id="29fb5-187">Notifications are raised for such undo operations, rolling the state of the replica back to a stable point.</span></span> <span data-ttu-id="29fb5-188">Egy fontos visszavonási értesítések különbség, hogy eseményeket, amelyek ismétlődő kulcsok összesítése.</span><span class="sxs-lookup"><span data-stu-id="29fb5-188">One important difference of undo notifications is that events that have duplicate keys are aggregated.</span></span> <span data-ttu-id="29fb5-189">Például ha tranzakció T1 visszavonja, Delete(X) egyetlen értesítést láthatja.</span><span class="sxs-lookup"><span data-stu-id="29fb5-189">For example, if transaction T1 is being undone, you'll see a single notification to Delete(X).</span></span>

## <a name="next-steps"></a><span data-ttu-id="29fb5-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29fb5-190">Next steps</span></span>
* [<span data-ttu-id="29fb5-191">Reliable Collections</span><span class="sxs-lookup"><span data-stu-id="29fb5-191">Reliable Collections</span></span>](service-fabric-work-with-reliable-collections.md)
* [<span data-ttu-id="29fb5-192">Megbízható szolgáltatások – első lépések</span><span class="sxs-lookup"><span data-stu-id="29fb5-192">Reliable Services quick start</span></span>](service-fabric-reliable-services-quick-start.md)
* [<span data-ttu-id="29fb5-193">Megbízható szolgáltatások biztonsági mentése és visszaállítása (katasztrófa utáni helyreállítás)</span><span class="sxs-lookup"><span data-stu-id="29fb5-193">Reliable Services backup and restore (disaster recovery)</span></span>](service-fabric-reliable-services-backup-restore.md)
* [<span data-ttu-id="29fb5-194">Fejlesztői leírás megbízható gyűjtemények</span><span class="sxs-lookup"><span data-stu-id="29fb5-194">Developer reference for Reliable Collections</span></span>](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

