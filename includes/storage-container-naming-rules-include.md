<span data-ttu-id="1dc0c-101">Az Azure Storage összes blobjának egy tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-101">Every blob in Azure storage must reside in a container.</span></span> <span data-ttu-id="1dc0c-102">hello tároló részét képezi hello blob neve.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-102">hello container forms part of hello blob name.</span></span> <span data-ttu-id="1dc0c-103">Például `mycontainer` hello név hello tároló ezekben a BLOB URI-azonosítók:</span><span class="sxs-lookup"><span data-stu-id="1dc0c-103">For example, `mycontainer` is hello name of hello container in these sample blob URIs:</span></span>

    https://storagesample.blob.core.windows.net/mycontainer/blob1.txt
    https://storagesample.blob.core.windows.net/mycontainer/photos/myphoto.jpg

<span data-ttu-id="1dc0c-104">A tároló nevének kell lennie egy érvényes DNS-nevet, a következő elnevezési szabályoknak megfelelő toohello:</span><span class="sxs-lookup"><span data-stu-id="1dc0c-104">A container name must be a valid DNS name, conforming toohello following naming rules:</span></span>

1. <span data-ttu-id="1dc0c-105">A tárolónévnek betűvel vagy számmal kell kezdődnie, és csak betűket, számokat és kötőjelet (-) karakter hello tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-105">Container names must start with a letter or number, and can contain only letters, numbers, and hello dash (-) character.</span></span>
2. <span data-ttu-id="1dc0c-106">Minden kötőjel előtt és után közvetlenül egy betűnek vagy számnak kell állnia. A tárolók nevében nem szerepelhetnek egymást követő kötőjelek.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-106">Every dash (-) character must be immediately preceded and followed by a letter or number; consecutive dashes are not permitted in container names.</span></span>
3. <span data-ttu-id="1dc0c-107">A tároló nevében szereplő összes betűnek kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-107">All letters in a container name must be lowercase.</span></span>
4. <span data-ttu-id="1dc0c-108">A tároló nevének 3–63 karakter hosszúságúnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-108">Container names must be from 3 through 63 characters long.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1dc0c-109">Vegye figyelembe, hogy hello a tároló nevének mindig kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1dc0c-109">Note that hello name of a container must always be lowercase.</span></span> <span data-ttu-id="1dc0c-110">Ha egy nagybetűt szerepeljenek a tároló neve, vagy más módon megsérti a hello tároló elnevezési szabályait, előfordulhat, hogy hibaüzenet 400 (hibás kérés).</span><span class="sxs-lookup"><span data-stu-id="1dc0c-110">If you include an upper-case letter in a container name, or otherwise violate hello container naming rules, you may receive a 400 error (Bad Request).</span></span> 
> 
> 

