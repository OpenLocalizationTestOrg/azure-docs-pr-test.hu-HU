---
title: "aaaBuild és az Azure App Service egy Java API-alkalmazás telepítése"
description: "Megtudhatja, hogyan toocreate egy Java API-alkalmazás csomagot, majd központilag telepíti az App Service tooAzure."
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 04/25/2017
ms.author: rachelap;robmcm
ms.openlocfilehash: a4056fec870b1c4bed8ee14bb0e748b3ee89b9e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a><span data-ttu-id="d5dd1-103">Java API-alkalmazás buildjének elkészítése és telepítése az Azure App Service platformon</span><span class="sxs-lookup"><span data-stu-id="d5dd1-103">Build and deploy a Java API app in Azure App Service</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="d5dd1-104">Ez az oktatóanyag bemutatja, hogyan toocreate egy Java-alkalmazást, és telepítse azt tooAzure App Service API Apps segítségével [Git].</span><span class="sxs-lookup"><span data-stu-id="d5dd1-104">This tutorial shows how toocreate a Java application and deploy it tooAzure App Service API Apps using [Git].</span></span> <span data-ttu-id="d5dd1-105">hello az oktatóanyag utasításai követhetők bármely operációs rendszeren, amely képes Java.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-105">hello instructions in this tutorial can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="d5dd1-106">Ebben az oktatóanyagban hello kód-t [Maven].</span><span class="sxs-lookup"><span data-stu-id="d5dd1-106">hello code in this tutorial is built using [Maven].</span></span> <span data-ttu-id="d5dd1-107">[Jax-RS] használt toocreate hello RESTful szolgáltatás, és jön létre hello alapján [Swagger] hello segítségével metaadat-specifikáció [Swagger Editor].</span><span class="sxs-lookup"><span data-stu-id="d5dd1-107">[Jax-RS] is used toocreate hello RESTful Service, and is generated based on hello [Swagger] metadata specification using hello [Swagger Editor].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d5dd1-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d5dd1-108">Prerequisites</span></span>
1. <span data-ttu-id="d5dd1-109">[Java Developer's Kit 8]\(vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="d5dd1-109">[Java Developer's Kit 8] \(or later)</span></span>
2. <span data-ttu-id="d5dd1-110">A fejlesztői gépen telepítve van a [Maven]</span><span class="sxs-lookup"><span data-stu-id="d5dd1-110">[Maven] installed on your development machine</span></span>
3. <span data-ttu-id="d5dd1-111">A fejlesztő gépen telepítve van a [Git]</span><span class="sxs-lookup"><span data-stu-id="d5dd1-111">[Git] installed on your development machine</span></span>
4. <span data-ttu-id="d5dd1-112">Fizetett vagy [ingyenes próbaverzió] előfizetés túl[Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="d5dd1-112">A paid or [free trial] subscription too[Microsoft Azure]</span></span>
5. <span data-ttu-id="d5dd1-113">HTTP-tesztalkalmazás, például [Postman]</span><span class="sxs-lookup"><span data-stu-id="d5dd1-113">An HTTP test application like [Postman]</span></span>

## <a name="scaffold-hello-api-using-swaggerio"></a><span data-ttu-id="d5dd1-114">Scaffold hello API Swagger.IO használatával</span><span class="sxs-lookup"><span data-stu-id="d5dd1-114">Scaffold hello API using Swagger.IO</span></span>
<span data-ttu-id="d5dd1-115">Hello swagger.io online szerkesztő segítségével adhatja meg az API struktúráját hello képviselő Swagger JSON- vagy YAM-kóddal.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-115">Using hello swagger.io online editor, you can enter Swagger JSON or YAML code representing hello structure of your API.</span></span> <span data-ttu-id="d5dd1-116">Miután hello API felület tervezték, exportálhatja a különböző platformokon és keretrendszerek kódját.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-116">Once you have hello API surface area designed, you can export code for a variety of platforms and frameworks.</span></span> <span data-ttu-id="d5dd1-117">A következő szakaszban hello hello generált kód lesz módosított tooinclude a funkciók utánzatait.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-117">In hello next section, hello scaffolded code will be modified tooinclude mock functionality.</span></span> 

<span data-ttu-id="d5dd1-118">Ebben a bemutatóban kezdődik egy Swagger JSON-törzsére, amely akkor lesz illessze be hello swagger.io szerkesztő, amely majd használt toogenerate kód végez egy REST API-végpont JAX-RS tooaccess használatát.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-118">This demonstration will begin with a Swagger JSON body that you will paste into hello swagger.io editor, which will then be used toogenerate code making use of JAX-RS tooaccess a REST API endpoint.</span></span> <span data-ttu-id="d5dd1-119">Ezt követően kell szerkeszteni hello generált kód tooreturn próbaadatokat, szimulálva egy adatperzisztencia-mechanizmus REST API.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-119">Then, you'll edit hello scaffolded code tooreturn mock data, simulating a REST API built atop a data persistence mechanism.</span></span>  

1. <span data-ttu-id="d5dd1-120">A következő Swagger JSON-kód tooyour vágólapra másolás hello:</span><span class="sxs-lookup"><span data-stu-id="d5dd1-120">Copy hello following Swagger JSON code tooyour clipboard:</span></span>
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. <span data-ttu-id="d5dd1-121">Keresse meg a toohello [Online Swagger Editor].</span><span class="sxs-lookup"><span data-stu-id="d5dd1-121">Navigate toohello [Online Swagger Editor].</span></span> <span data-ttu-id="d5dd1-122">Ezután kattintson hello **fájl -> JSON beillesztése** menüpont.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-122">Once there, click hello **File -> Paste JSON** menu item.</span></span>
   
    ![A JSON beillesztése menüpont][paste-json]
3. <span data-ttu-id="d5dd1-124">Illessze be a névjegyek lista API Swagger JSON korábban kimásolt hello.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-124">Paste in hello Contacts List API Swagger JSON you copied earlier.</span></span> 
   
    ![JSON-kód beillesztése a Swaggerbe][pasted-swagger]
4. <span data-ttu-id="d5dd1-126">Nézet hello dokumentációs oldalakat és API-összefoglalót hello szerkesztőben megjelenítve.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-126">View hello documentation pages and API summary rendered in hello editor.</span></span> 
   
    ![A Swagger által generált dokumentumok megtekintése][view-swagger-generated-docs]
5. <span data-ttu-id="d5dd1-128">Jelölje be hello **kiszolgáló generálása -> JAX-RS** menü beállítás tooscaffold hello kiszolgálóoldali kód amelyet szerkeszteni fog újabb tooadd utánzatait végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-128">Select hello **Generate Server -> JAX-RS** menu option tooscaffold hello server-side code you'll edit later tooadd mock implementation.</span></span> 
   
    ![A Kód generálása menüpont][generate-code-menu-item]
   
    <span data-ttu-id="d5dd1-130">Miután hello kód jön létre, lesz egy ZIP-fájl toodownload megadni.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-130">Once hello code is generated, you'll be provided a ZIP file toodownload.</span></span> <span data-ttu-id="d5dd1-131">Ez a fájl hello Swagger kódgeneráló által generált hello kódot tartalmaz, és összes kapcsolódó fordítási parancsprogramot.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-131">This file contains hello code scaffolded by hello Swagger code generator and all associated build scripts.</span></span> <span data-ttu-id="d5dd1-132">Bontsa ki a hello teljes tooa könyvtárat a fejlesztő munkaállomás.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-132">Unzip hello entire library tooa directory on your development workstation.</span></span> 

## <a name="edit-hello-code-tooadd-api-implementation"></a><span data-ttu-id="d5dd1-133">Hello kód tooadd API-implementáció szerkesztése</span><span class="sxs-lookup"><span data-stu-id="d5dd1-133">Edit hello Code tooadd API Implementation</span></span>
<span data-ttu-id="d5dd1-134">Ebben a szakaszban cseréli le hello Swagger által generált kód kiszolgálóoldali implementációját a saját kódjára.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-134">In this section, you'll replace hello Swagger-generated code's server-side implementation with your custom code.</span></span> <span data-ttu-id="d5dd1-135">hello új kódot ad vissza egy ArrayList Contact entitásokat toohello hívó ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-135">hello new code will return an ArrayList of Contact entities toohello calling client.</span></span> 

1. <span data-ttu-id="d5dd1-136">Nyissa meg hello *Contact.java* modellfájl, amely hello *src/gen/java/io/swagger/model* mappa, használatával [Visual Studio Code] vagy kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-136">Open hello *Contact.java* model file, which is located in hello *src/gen/java/io/swagger/model* folder, using [Visual Studio Code] or your favorite text editor.</span></span> 
   
    ![A Contact modellfájl megnyitása][open-contact-model-file]
2. <span data-ttu-id="d5dd1-138">Adja hozzá a következő belül hello konstruktor hello **forduljon** osztály.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-138">Add hello following constructor within hello **Contact** class.</span></span> 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. <span data-ttu-id="d5dd1-139">Nyissa meg hello *mappában* fájlt, amely hello *src/main/java/io/swagger/api/impl* mappa, használatával [Visual Studio Code]vagy kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-139">Open hello *ContactsApiServiceImpl.java* service implementation file, which is located in hello *src/main/java/io/swagger/api/impl* folder, using [Visual Studio Code] or your favorite text editor.</span></span>
   
    ![A Contact szolgáltatásimplementációs fájl megnyitása][open-contact-service-code-file]
4. <span data-ttu-id="d5dd1-141">Hello kód hello fájl felülírása az új kódot tooadd utánzatait megvalósítási toohello szolgáltatás kódot.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-141">Overwrite hello code in hello file with this new code tooadd a mock implementation toohello service code.</span></span> 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. <span data-ttu-id="d5dd1-142">Nyisson meg egy parancssort, és módosítsa a könyvtárat toohello az alkalmazás gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-142">Open a command prompt and change directory toohello root folder of your application.</span></span>
6. <span data-ttu-id="d5dd1-143">Hajtható végre a következő Maven parancs toobuild hello kód hello, és futtassa helyileg a Jetty alkalmazáskiszolgálóval hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-143">Execute hello following Maven command toobuild hello code and run it using hello Jetty app server locally.</span></span> 
   
        mvn package jetty:run
7. <span data-ttu-id="d5dd1-144">Meg kell jelennie hello parancsablakot tükrözi, hogy Jetty elindította a kódot a 8080-as porton.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-144">You should see hello command window reflect that Jetty has started your code on port 8080.</span></span> 
   
    ![A Contact szolgáltatásimplementációs fájl megnyitása][run-jetty-war]
8. <span data-ttu-id="d5dd1-146">Használjon [Postman] toomake kérelem toohello "get all contacts" API-metódus: 8080/api/Contacts címen.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-146">Use [Postman] toomake a request toohello "get all contacts" API method at http://localhost:8080/api/contacts.</span></span>
   
    ![Hello névjegyek API hívása][calling-contacts-api]
9. <span data-ttu-id="d5dd1-148">Használjon [Postman] toomake kérelem toohello "get specific contact" API-módszer helye: 8080/api/contacts/2.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-148">Use [Postman] toomake a request toohello "get specific contact" API method located at http://localhost:8080/api/contacts/2.</span></span>
   
    ![Hello névjegyek API hívása][calling-specific-contact-api]
10. <span data-ttu-id="d5dd1-150">Végezetül fordítsa le hello Java WAR (Web ARchive) fájlt a következő Maven-parancsot a konzolon hello végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-150">Finally, build hello Java WAR (Web ARchive) file by executing hello following Maven command in your console.</span></span> 
    
         mvn package war:war
11. <span data-ttu-id="d5dd1-151">Miután összeállította hello WAR-fájlt, akkor bekerülnek hello **cél** mappát.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-151">Once hello WAR file is built, it will be placed into hello **target** folder.</span></span> <span data-ttu-id="d5dd1-152">Hello navigálni **cél** mappa, és nevezze át hello WAR-fájl túl**ROOT.war**.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-152">Navigate into hello **target** folder and rename hello WAR file too**ROOT.war**.</span></span> <span data-ttu-id="d5dd1-153">(Győződjön meg arról, hogy hello és nagybetűkre).</span><span class="sxs-lookup"><span data-stu-id="d5dd1-153">(Make sure hello capitalization matches this format).</span></span>
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. <span data-ttu-id="d5dd1-154">Végezetül parancsok követően az alkalmazás toocreate hello gyökérmappájában hello egy **telepítése** mappa toouse toodeploy hello WAR-fájl tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-154">Finally, execute hello following commands from hello root folder of your application toocreate a **deploy** folder toouse toodeploy hello WAR file tooAzure.</span></span> 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-hello-output-tooazure-app-service"></a><span data-ttu-id="d5dd1-155">Hello kimeneti tooAzure App Service közzététele</span><span class="sxs-lookup"><span data-stu-id="d5dd1-155">Publish hello output tooAzure App Service</span></span>
<span data-ttu-id="d5dd1-156">Ebben a szakaszban megtudhatja, hogyan toocreate hello segítségével új API-alkalmazást az Azure portál, adott API-alkalmazások előkészítése a Java-alkalmazások és hello telepítése újonnan létrehozott WAR fájlt az új API-alkalmazás az App Service toorun tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-156">In this section you'll learn how toocreate a new API App using hello Azure Portal, prepare that API App for hosting Java applications, and deploy hello newly-created WAR file tooAzure App Service toorun your new API App.</span></span> 

1. <span data-ttu-id="d5dd1-157">Hozzon létre egy új API-alkalmazás hello [Azure-portálon], hello kattintva **új -> Web + mobil -> API-alkalmazás** menüpont, írja be az adatokat, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-157">Create a new API app in hello [Azure portal], by clicking hello **New -> Web + Mobile -> API app** menu item, entering your app details, and then clicking **Create**.</span></span>
   
    ![Új API-alkalmazás létrehozása][create-api-app]
2. <span data-ttu-id="d5dd1-159">Az API-alkalmazás létrehozása után, nyissa meg az alkalmazás **beállítások** panelen, majd kattintson a hello **Alkalmazásbeállítások** menüpont.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-159">Once your API app has been created, open your app's **Settings** blade, and then click hello **Application settings** menu item.</span></span> <span data-ttu-id="d5dd1-160">Válassza ki hello hello rendelkezésre álló lehetőségeket, a legújabb Java-verziót, majd válassza ki a legújabb tomcat-verziót a hello hello **webes tároló** menüben, majd kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-160">Select hello latest Java versions from hello available options, then select hello latest Tomcat from hello **Web container** menu, and then click **Save**.</span></span>
   
    ![Java API-alkalmazás paneljének hello beállítása][set-up-java]
3. <span data-ttu-id="d5dd1-162">Kattintson a hello **üzembe helyezési hitelesítő adatok** menüpontra, és írja be egy felhasználónevet és jelszót fájlok tooyour API-alkalmazás-közzététel toouse kívánja.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-162">Click hello **Deployment credentials** settings menu item, and provide a username and password you wish toouse for publishing files tooyour API App.</span></span> 
   
    ![Telepítési hitelesítő adatok beállítása][deployment-credentials]
4. <span data-ttu-id="d5dd1-164">Kattintson a hello **központi telepítés forrásának** menüpontra.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-164">Click hello **Deployment source** settings menu item.</span></span> <span data-ttu-id="d5dd1-165">Ezután kattintson hello **forrás választása** gombra, jelölje be hello **helyi Git-tárház** lehetőséget, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-165">Once there, click hello **Choose source** button, select hello **Local Git Repository** option, and then click **OK**.</span></span> <span data-ttu-id="d5dd1-166">Ezzel létrehoz egy Git-tárházat, amely az Azure-ban fut és az Ön API-alkalmazásához van társítva.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-166">This will create a Git repository running in Azure, that has an association with your API App.</span></span> <span data-ttu-id="d5dd1-167">Minden alkalommal, amikor véglegesíti kód toohello *fő* fiókirodai a Git-tárház, a kódot közzéteszi a környezet az élő futó API-alkalmazáspéldányban.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-167">Each time you commit code toohello *master* branch of your Git repository, your code will be published into your live running API App instance.</span></span> 
   
    ![Új helyi Git-tárház beállítása][select-git-repo]
5. <span data-ttu-id="d5dd1-169">Hello új Git-tárház URL-cím tooyour vágólapra másolása.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-169">Copy hello new Git repository's URL tooyour clipboard.</span></span> <span data-ttu-id="d5dd1-170">Mentse, mert hamarosan szüksége lesz rá.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-170">Save this as it will be important in a moment.</span></span> 
   
    ![Új Git-tárház beállítása az alkalmazáshoz][copy-git-repo-url]
6. <span data-ttu-id="d5dd1-172">Git push hello WAR fájl toohello online tárházba.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-172">Git push hello WAR file toohello online repository.</span></span> <span data-ttu-id="d5dd1-173">toodo, hello navigálni **telepítése** mappa a korábban létrehozott, hogy könnyen véglegesíthesse hello kód fel az App Service-ben futó toohello tárházba.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-173">toodo this, navigate into hello **deploy** folder you created earlier so that you can easily commit hello code up toohello repository running in your App Service.</span></span> <span data-ttu-id="d5dd1-174">Egyszer Ön hello konzolablakban, és belépett hello mappáját hello webapps mappát, adja ki a Git parancsok toolaunch hello folyamatot követve hello és a telepítés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-174">Once you're in hello console window and navigated into hello folder where hello webapps folder is located, issue hello following Git commands toolaunch hello process and fire off a deployment.</span></span> 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    <span data-ttu-id="d5dd1-175">Hello kiadása után **leküldéses** kérelem, meg kell adnia a hello hello telepítési hitelesítő adatokhoz korábban létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-175">Once you issue hello **push** request, you'll be asked for hello password you created for hello deployment credential earlier.</span></span> <span data-ttu-id="d5dd1-176">Miután megadta a hitelesítő adatait, meg kell jelennie, a portál megjelenítése, frissítés hello tették elérhetővé telepítésre.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-176">After you enter your credentials, you should see your portal display that hello update was deployed.</span></span>
7. <span data-ttu-id="d5dd1-177">Még egyszer használatakor Postman toohit hello újonnan telepített API-alkalmazás fusson az Azure App Service-ben, látni fogja, hogy hello viselkedése konzisztens, és most adja vissza kapcsolattartási adatokat várt módon, és egyszerű kód módosítások toohello Swagger.io használatával generált Java-kódot.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-177">If you once again use Postman toohit hello newly-deployed API App running in Azure App Service, you'll see that hello behavior is consistent and that now it is returning contact data as expected, and using simple code changes toohello Swagger.io scaffolded Java code.</span></span> 
   
    ![A Java Contacts REST API használata élőben az Azure-ban][postman-calling-azure-contacts]

## <a name="next-steps"></a><span data-ttu-id="d5dd1-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d5dd1-179">Next steps</span></span>
<span data-ttu-id="d5dd1-180">Ebben a cikkben egy Swagger JSON-fájlt, és néhány generált Java-kóddal indultunk el hello Swagger.io szerkesztő képes toostart volt.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-180">In this article, you were able toostart with a Swagger JSON file and some scaffolded Java code obtained from hello Swagger.io editor.</span></span> <span data-ttu-id="d5dd1-181">Ezekből egyszerű módosításokkal és a Git telepítési folyamatának eredményeként egy működő, Java nyelven írt API-alkalmazást kaptunk.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-181">From there, your simple changes and a Git deploy process resulted in having a functional API app written in Java.</span></span> <span data-ttu-id="d5dd1-182">hello tovább az oktatóanyag bemutatja, hogyan túl[JavaScript-ügyfelekkel a CORS segítségével API-alkalmazásokat felhasználni][App Service API CORS].</span><span class="sxs-lookup"><span data-stu-id="d5dd1-182">hello next tutorial shows how too[consume API apps from JavaScript clients, using CORS][App Service API CORS].</span></span> <span data-ttu-id="d5dd1-183">Későbbi részei hello adatsor megjelenítése hogyan tooimplement hitelesítéshez és engedélyezéshez.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-183">Later tutorials in hello series show how tooimplement authentication and authorization.</span></span>

<span data-ttu-id="d5dd1-184">erre a példára toobuild, akkor tudhat meg többet hello [Storage SDK for Java] toopersist hello JSON-blobok.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-184">toobuild on this sample, you can learn more about hello [Storage SDK for Java] toopersist hello JSON blobs.</span></span> <span data-ttu-id="d5dd1-185">Vagy használhat hello [Document DB Java SDK] toosave a kapcsolattartó adatok tooAzure Document DB rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="d5dd1-185">Or, you could use hello [Document DB Java SDK] toosave your Contact data tooAzure Document DB.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="d5dd1-186">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d5dd1-186">See Also</span></span>
<span data-ttu-id="d5dd1-187">További információk az Azure és a Java együttes használatáról lásd: [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="d5dd1-187">For more information about using Azure with Java, visit [Azure for Java developers](/java/azure).</span></span>

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure-portálon]: https://portal.azure.com/
[Document DB Java SDK]: ../documentdb/documentdb-java-application.md
[ingyenes próbaverzió]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor2.swagger.io/
[Postman]: https://www.getpostman.com/
[Storage SDK for Java]:../storage/blobs/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger Editor]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
