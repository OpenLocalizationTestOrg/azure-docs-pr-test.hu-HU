<span data-ttu-id="df320-101">Az Azure Storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="df320-101">Every blob in Azure storage must reside in a container.</span></span> <span data-ttu-id="df320-102">A blob nevének egy részét a tároló adja meg.</span><span class="sxs-lookup"><span data-stu-id="df320-102">The container forms part of the blob name.</span></span> <span data-ttu-id="df320-103">Például ezekben a blob URI-mintákban a `mycontainer` a tároló neve:</span><span class="sxs-lookup"><span data-stu-id="df320-103">For example, `mycontainer` is the name of the container in these sample blob URIs:</span></span>

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

<span data-ttu-id="df320-104">A tároló nevének egy érvényes DNS-névnek kell lennie, amely megfelel az alábbi elnevezési szabályoknak:</span><span class="sxs-lookup"><span data-stu-id="df320-104">A container name must be a valid DNS name, conforming to the following naming rules:</span></span>

1. <span data-ttu-id="df320-105">A tároló nevének betűvel vagy számmal kell kezdődnie, és csak betűket, számokat és kötőjelet (-) tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="df320-105">Container names must start with a letter or number, and can contain only letters, numbers, and the dash (-) character.</span></span>
2. <span data-ttu-id="df320-106">Minden kötőjel előtt és után közvetlenül egy betűnek vagy számnak kell állnia. A tárolók nevében nem szerepelhetnek egymást követő kötőjelek.</span><span class="sxs-lookup"><span data-stu-id="df320-106">Every dash (-) character must be immediately preceded and followed by a letter or number; consecutive dashes are not permitted in container names.</span></span>
3. <span data-ttu-id="df320-107">A tároló nevében szereplő összes betűnek kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="df320-107">All letters in a container name must be lowercase.</span></span>
4. <span data-ttu-id="df320-108">A tároló nevének 3–63 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="df320-108">Container names must be from 3 through 63 characters long.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="df320-109">Ne feledje, hogy a tároló nevének minden esetben kisbetűsnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="df320-109">Note that the name of a container must always be lowercase.</span></span> <span data-ttu-id="df320-110">Ha a tárolónévbe nagybetűt ír, vagy más módon megsérti a tároló elnevezési szabályait, akkor a rendszer 400-as hibát adhat vissza (Hibás kérés).</span><span class="sxs-lookup"><span data-stu-id="df320-110">If you include an upper-case letter in a container name, or otherwise violate the container naming rules, you may receive a 400 error (Bad Request).</span></span> 
> 
> 

