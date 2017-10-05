---
title: "Java API-alkalmazás buildjének elkészítése és telepítése az Azure App Service platformon"
description: "Megtudhatja, hogyan hozhat létre egy Java API-alkalmazáscsomagot, és hogyan telepítheti az Azure App Service platformra."
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
ms.openlocfilehash: e38c540071cb49b0177e79178566d72ecb5f8886
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a><span data-ttu-id="a509b-103">Java API-alkalmazás buildjének elkészítése és telepítése az Azure App Service platformon</span><span class="sxs-lookup"><span data-stu-id="a509b-103">Build and deploy a Java API app in Azure App Service</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="a509b-104">Ez az oktatóanyag bemutatja, hogyan hozhat létre egy Java-alkalmazást, és hogyan telepítheti az Azure App Service API Apps szolgáltatásba a [Git] használatával.</span><span class="sxs-lookup"><span data-stu-id="a509b-104">This tutorial shows how to create a Java application and deploy it to Azure App Service API Apps using [Git].</span></span> <span data-ttu-id="a509b-105">Az oktatóanyagban lévő utasítások bármilyen, Java-programok futtatására alkalmas operációs rendszeren végrehajthatók.</span><span class="sxs-lookup"><span data-stu-id="a509b-105">The instructions in this tutorial can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="a509b-106">Az oktatóanyagban szereplő kód a [Maven] használatával lett lefordítva.</span><span class="sxs-lookup"><span data-stu-id="a509b-106">The code in this tutorial is built using [Maven].</span></span> <span data-ttu-id="a509b-107">A RESTful szolgáltatás létrehozásához a [Jax-RS] API-t használjuk, és a szolgáltatás generálása [Swagger] metaadat-specifikáció alapján történik, amelyet a [Swagger Editor] programmal készítünk el.</span><span class="sxs-lookup"><span data-stu-id="a509b-107">[Jax-RS] is used to create the RESTful Service, and is generated based on the [Swagger] metadata specification using the [Swagger Editor].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a509b-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a509b-108">Prerequisites</span></span>
1. <span data-ttu-id="a509b-109">[Java Developer's Kit 8] \(vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="a509b-109">[Java Developer's Kit 8] \(or later)</span></span>
2. <span data-ttu-id="a509b-110">A fejlesztői gépen telepítve van a [Maven]</span><span class="sxs-lookup"><span data-stu-id="a509b-110">[Maven] installed on your development machine</span></span>
3. <span data-ttu-id="a509b-111">A fejlesztő gépen telepítve van a [Git]</span><span class="sxs-lookup"><span data-stu-id="a509b-111">[Git] installed on your development machine</span></span>
4. <span data-ttu-id="a509b-112">[Ingyenes] vagy fizetős [Microsoft Azure]-előfizetés</span><span class="sxs-lookup"><span data-stu-id="a509b-112">A paid or [free trial] subscription to [Microsoft Azure]</span></span>
5. <span data-ttu-id="a509b-113">HTTP-tesztalkalmazás, például [Postman]</span><span class="sxs-lookup"><span data-stu-id="a509b-113">An HTTP test application like [Postman]</span></span>

## <a name="scaffold-the-api-using-swaggerio"></a><span data-ttu-id="a509b-114">Az API strukturálása a Swagger.IO használatával</span><span class="sxs-lookup"><span data-stu-id="a509b-114">Scaffold the API using Swagger.IO</span></span>
<span data-ttu-id="a509b-115">A swagger.io online szerkesztő segítségével az API struktúráját leírhatja Swagger JSON- vagy YAM- kóddal.</span><span class="sxs-lookup"><span data-stu-id="a509b-115">Using the swagger.io online editor, you can enter Swagger JSON or YAML code representing the structure of your API.</span></span> <span data-ttu-id="a509b-116">Az API felületének megtervezése után a kódot exportálhatja különböző platformokra és keretrendszerekre.</span><span class="sxs-lookup"><span data-stu-id="a509b-116">Once you have the API surface area designed, you can export code for a variety of platforms and frameworks.</span></span> <span data-ttu-id="a509b-117">A következő szakaszban a generált kódot úgy módosítjuk, hogy tartalmazza a funkciók utánzatait.</span><span class="sxs-lookup"><span data-stu-id="a509b-117">In the next section, the scaffolded code will be modified to include mock functionality.</span></span> 

<span data-ttu-id="a509b-118">A demonstráció elején a swagger.io online szerkesztőbe Swagger JSON-kódot illesztünk be, amelyből a JAX-RS segítségével egy REST API-végpont elérésére alkalmas kódot generálunk.</span><span class="sxs-lookup"><span data-stu-id="a509b-118">This demonstration will begin with a Swagger JSON body that you will paste into the swagger.io editor, which will then be used to generate code making use of JAX-RS to access a REST API endpoint.</span></span> <span data-ttu-id="a509b-119">Ezt követően a generált kódot úgy szerkesztjük, hogy próbaadatokat adjon vissza, ezáltal egy adatperzisztencia-mechanizmus feletti REST API-t szimulálva.</span><span class="sxs-lookup"><span data-stu-id="a509b-119">Then, you'll edit the scaffolded code to return mock data, simulating a REST API built atop a data persistence mechanism.</span></span>  

1. <span data-ttu-id="a509b-120">Másolja a vágólapra a következő Swagger JSON-kódot:</span><span class="sxs-lookup"><span data-stu-id="a509b-120">Copy the following Swagger JSON code to your clipboard:</span></span>
   
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
2. <span data-ttu-id="a509b-121">Nyissa meg az [Online Swagger Editort].</span><span class="sxs-lookup"><span data-stu-id="a509b-121">Navigate to the [Online Swagger Editor].</span></span> <span data-ttu-id="a509b-122">A szerkesztőben kattintson a **File -> Paste JSON**(Fájl -> JSON-kód beillesztése)  menüpontra.</span><span class="sxs-lookup"><span data-stu-id="a509b-122">Once there, click the **File -> Paste JSON** menu item.</span></span>
   
    ![A JSON beillesztése menüpont][paste-json]
3. <span data-ttu-id="a509b-124">Illessze be a Contacts List API korábban vágólapra másolt Swagger JSON-kódját.</span><span class="sxs-lookup"><span data-stu-id="a509b-124">Paste in the Contacts List API Swagger JSON you copied earlier.</span></span> 
   
    ![JSON-kód beillesztése a Swaggerbe][pasted-swagger]
4. <span data-ttu-id="a509b-126">Olvassa el a szerkesztőben renderelt dokumentációs oldalakat és API-összefoglalót.</span><span class="sxs-lookup"><span data-stu-id="a509b-126">View the documentation pages and API summary rendered in the editor.</span></span> 
   
    ![A Swagger által generált dokumentumok megtekintése][view-swagger-generated-docs]
5. <span data-ttu-id="a509b-128">A **Generate Server -> JAX-RS** (Kiszolgáló generálása -> JAX-RS) menüpontot választva generálja le a kiszolgálóoldali kódot, amelyet szerkeszteni fog, és próbaimplementációval fog bővíteni.</span><span class="sxs-lookup"><span data-stu-id="a509b-128">Select the **Generate Server -> JAX-RS** menu option to scaffold the server-side code you'll edit later to add mock implementation.</span></span> 
   
    ![A Kód generálása menüpont][generate-code-menu-item]
   
    <span data-ttu-id="a509b-130">A kódgenerálás után egy ZIP-fájlt tölthet le.</span><span class="sxs-lookup"><span data-stu-id="a509b-130">Once the code is generated, you'll be provided a ZIP file to download.</span></span> <span data-ttu-id="a509b-131">Ez a fájl tartalmazza a Swagger kódgeneráló által generált kódot és az összes kapcsolódó fordítási parancsprogramot.</span><span class="sxs-lookup"><span data-stu-id="a509b-131">This file contains the code scaffolded by the Swagger code generator and all associated build scripts.</span></span> <span data-ttu-id="a509b-132">Bontsa ki a fájl teljes tartalmát a fejlesztő munkaállomás valamelyik könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="a509b-132">Unzip the entire library to a directory on your development workstation.</span></span> 

## <a name="edit-the-code-to-add-api-implementation"></a><span data-ttu-id="a509b-133">A kód szerkesztése API-implementáció hozzáadásához</span><span class="sxs-lookup"><span data-stu-id="a509b-133">Edit the Code to add API Implementation</span></span>
<span data-ttu-id="a509b-134">Ebben a szakaszban a Swagger által generált kód kiszolgálóoldali implementációját a saját kódjára cseréli le.</span><span class="sxs-lookup"><span data-stu-id="a509b-134">In this section, you'll replace the Swagger-generated code's server-side implementation with your custom code.</span></span> <span data-ttu-id="a509b-135">Az új kód egy Contact entitásokat tartalmazó ArrayListet fog visszaadni a hívó ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="a509b-135">The new code will return an ArrayList of Contact entities to the calling client.</span></span> 

1. <span data-ttu-id="a509b-136">Nyissa meg az *src/gen/java/io/swagger/model* mappában található  *Contact.java* modellfájlt a [Visual Studio Code]-dal vagy kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="a509b-136">Open the *Contact.java* model file, which is located in the *src/gen/java/io/swagger/model* folder, using [Visual Studio Code] or your favorite text editor.</span></span> 
   
    ![A Contact modellfájl megnyitása][open-contact-model-file]
2. <span data-ttu-id="a509b-138">Adja hozzá az alábbi konstruktort a **Contact** osztályon belül.</span><span class="sxs-lookup"><span data-stu-id="a509b-138">Add the following constructor within the **Contact** class.</span></span> 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. <span data-ttu-id="a509b-139">Nyissa meg az *src/main/java/io/swagger/api/impl* mappában található *ContactsApiServiceImpl.java* szolgáltatásimplementációs fájlt a [Visual Studio Code]-dal vagy kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="a509b-139">Open the *ContactsApiServiceImpl.java* service implementation file, which is located in the *src/main/java/io/swagger/api/impl* folder, using [Visual Studio Code] or your favorite text editor.</span></span>
   
    ![A Contact szolgáltatásimplementációs fájl megnyitása][open-contact-service-code-file]
4. <span data-ttu-id="a509b-141">Írja felül a fájlban lévő kódot az alábbi új kóddal, amely a szolgáltatáskód próbaimplementációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a509b-141">Overwrite the code in the file with this new code to add a mock implementation to the service code.</span></span> 
   
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
5. <span data-ttu-id="a509b-142">Nyisson meg egy parancssort, és váltson az alkalmazás gyökérkönyvtárára.</span><span class="sxs-lookup"><span data-stu-id="a509b-142">Open a command prompt and change directory to the root folder of your application.</span></span>
6. <span data-ttu-id="a509b-143">Hajtsa végre a következő Maven-parancsot a kód lefordításához és a Jetty alkalmazáskiszolgálóval való helyi futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a509b-143">Execute the following Maven command to build the code and run it using the Jetty app server locally.</span></span> 
   
        mvn package jetty:run
7. <span data-ttu-id="a509b-144">A parancssori ablakban azt kell látnia, hogy a Jetty elindította a kódot a 8080-as porton.</span><span class="sxs-lookup"><span data-stu-id="a509b-144">You should see the command window reflect that Jetty has started your code on port 8080.</span></span> 
   
    ![A Contact szolgáltatásimplementációs fájl megnyitása][run-jetty-war]
8. <span data-ttu-id="a509b-146">A [Postman] használatával indítson egy lekérdezést a „get all contacts” API-metódusra a http://localhost:8080/api/contacts címen.</span><span class="sxs-lookup"><span data-stu-id="a509b-146">Use [Postman] to make a request to the "get all contacts" API method at http://localhost:8080/api/contacts.</span></span>
   
    ![A Contacts API meghívása][calling-contacts-api]
9. <span data-ttu-id="a509b-148">A [Postman] használatával indítson egy lekérdezést a „get specific contact” API-metódusra a http://localhost:8080/api/contacts/2 címen.</span><span class="sxs-lookup"><span data-stu-id="a509b-148">Use [Postman] to make a request to the "get specific contact" API method located at http://localhost:8080/api/contacts/2.</span></span>
   
    ![A Contacts API meghívása][calling-specific-contact-api]
10. <span data-ttu-id="a509b-150">Végezetül fordítsa le a Java WAR (Web ARchive) fájlt a következő Maven-parancsnak a konzolban való futtatásával.</span><span class="sxs-lookup"><span data-stu-id="a509b-150">Finally, build the Java WAR (Web ARchive) file by executing the following Maven command in your console.</span></span> 
    
         mvn package war:war
11. <span data-ttu-id="a509b-151">A WAR-fájl fordítás után bekerül a **target** mappába.</span><span class="sxs-lookup"><span data-stu-id="a509b-151">Once the WAR file is built, it will be placed into the **target** folder.</span></span> <span data-ttu-id="a509b-152">Nyissa meg a **target** mappát, és nevezze át a WAR-fájlt **ROOT.war**-ra.</span><span class="sxs-lookup"><span data-stu-id="a509b-152">Navigate into the **target** folder and rename the WAR file to **ROOT.war**.</span></span> <span data-ttu-id="a509b-153">(Ügyeljen a kis- és nagybetűkre).</span><span class="sxs-lookup"><span data-stu-id="a509b-153">(Make sure the capitalization matches this format).</span></span>
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. <span data-ttu-id="a509b-154">Végezetül a következő parancsokat az alkalmazás gyökérmappájában végrehajtva hozza létre a **deploy** mappát a WAR-fájlnak az Azure platformra való telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a509b-154">Finally, execute the following commands from the root folder of your application to create a **deploy** folder to use to deploy the WAR file to Azure.</span></span> 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-the-output-to-azure-app-service"></a><span data-ttu-id="a509b-155">A kimenet közzététele az Azure App Service platformon</span><span class="sxs-lookup"><span data-stu-id="a509b-155">Publish the output to Azure App Service</span></span>
<span data-ttu-id="a509b-156">Ebben a szakaszban megtudhatja, hogyan hozhat létre új API-alkalmazást az Azure portálon, hogyan készítheti elő az API-alkalmazást Java-alkalmazások futtatására, valamint hogy hogyan telepítheti az újonnan létrehozott WAR-fájlt az Azure App Service platformra az új API-alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a509b-156">In this section you'll learn how to create a new API App using the Azure Portal, prepare that API App for hosting Java applications, and deploy the newly-created WAR file to Azure App Service to run your new API App.</span></span> 

1. <span data-ttu-id="a509b-157">Hozzon létre egy új API-alkalmazást az [Azure Portal]. Ehhez kattintson a **Új -> Web + mobil -> API-alkalmazás** menüpontra, írja be az adatokat, majd kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a509b-157">Create a new API app in the [Azure portal], by clicking the **New -> Web + Mobile -> API app** menu item, entering your app details, and then clicking **Create**.</span></span>
   
    ![Új API-alkalmazás létrehozása][create-api-app]
2. <span data-ttu-id="a509b-159">Az API-alkalmazás létrehozása után nyissa meg az alkalmazás **Beállítások**  paneljét, majd kattintson az **Alkalmazás beállításai** menüpontra.</span><span class="sxs-lookup"><span data-stu-id="a509b-159">Once your API app has been created, open your app's **Settings** blade, and then click the **Application settings** menu item.</span></span> <span data-ttu-id="a509b-160">Az elérhető lehetőségek közül válassza a legújabb Java-verziót, majd a legújabb Tomcat-verziót a **Webes tároló** menüből, és kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a509b-160">Select the latest Java versions from the available options, then select the latest Tomcat from the **Web container** menu, and then click **Save**.</span></span>
   
    ![A Java beállítása az API-alkalmazás paneljén][set-up-java]
3. <span data-ttu-id="a509b-162">Kattintson a **Telepítési hitelesítő adatok** menüpontra, és írja be azt a felhasználónevet és jelszót, amelyet a fájloknak az API-alkalmazásban való közzétételéhez szeretne használni.</span><span class="sxs-lookup"><span data-stu-id="a509b-162">Click the **Deployment credentials** settings menu item, and provide a username and password you wish to use for publishing files to your API App.</span></span> 
   
    ![Telepítési hitelesítő adatok beállítása][deployment-credentials]
4. <span data-ttu-id="a509b-164">Kattintson a **Központi telepítés forrása** menüpontra.</span><span class="sxs-lookup"><span data-stu-id="a509b-164">Click the **Deployment source** settings menu item.</span></span> <span data-ttu-id="a509b-165">Ezután kattintson a **Forrás választása** gombra, válassza a **Helyi git-tárház** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a509b-165">Once there, click the **Choose source** button, select the **Local Git Repository** option, and then click **OK**.</span></span> <span data-ttu-id="a509b-166">Ezzel létrehoz egy Git-tárházat, amely az Azure-ban fut és az Ön API-alkalmazásához van társítva.</span><span class="sxs-lookup"><span data-stu-id="a509b-166">This will create a Git repository running in Azure, that has an association with your API App.</span></span> <span data-ttu-id="a509b-167">Minden alkalommal, amikor kódot véglegesít a Git-tárház *master* ágán, a kódot közzéteszi a környezet az élő futó API-alkalmazáspéldányban.</span><span class="sxs-lookup"><span data-stu-id="a509b-167">Each time you commit code to the *master* branch of your Git repository, your code will be published into your live running API App instance.</span></span> 
   
    ![Új helyi Git-tárház beállítása][select-git-repo]
5. <span data-ttu-id="a509b-169">Másolja az új Git-tárház URL-címét a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="a509b-169">Copy the new Git repository's URL to your clipboard.</span></span> <span data-ttu-id="a509b-170">Mentse, mert hamarosan szüksége lesz rá.</span><span class="sxs-lookup"><span data-stu-id="a509b-170">Save this as it will be important in a moment.</span></span> 
   
    ![Új Git-tárház beállítása az alkalmazáshoz][copy-git-repo-url]
6. <span data-ttu-id="a509b-172">A Git push parancsával küldje el a WAR-fájlt az online tárházba.</span><span class="sxs-lookup"><span data-stu-id="a509b-172">Git push the WAR file to the online repository.</span></span> <span data-ttu-id="a509b-173">Ehhez nyissa meg a korábban létrehozott **deploy** mappát, hogy könnyen véglegesíthesse a kódot az App Service-ben futó tárházba.</span><span class="sxs-lookup"><span data-stu-id="a509b-173">To do this, navigate into the **deploy** folder you created earlier so that you can easily commit the code up to the repository running in your App Service.</span></span> <span data-ttu-id="a509b-174">Ha már a konzolablakban van, és belépett a webapps mappát tartalmazó mappába, adja ki a következő Git-parancsokat a folyamat elindításához és a telepítés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="a509b-174">Once you're in the console window and navigated into the folder where the webapps folder is located, issue the following Git commands to launch the process and fire off a deployment.</span></span> 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    <span data-ttu-id="a509b-175">A **push** parancs kiadása után meg kell adnia azt a jelszót, amelyet a telepítési hitelesítő adatokhoz korábban megadott.</span><span class="sxs-lookup"><span data-stu-id="a509b-175">Once you issue the **push** request, you'll be asked for the password you created for the deployment credential earlier.</span></span> <span data-ttu-id="a509b-176">Miután megadta a hitelesítő adatait, a portál értesíti arról, hogy a frissítés telepítése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="a509b-176">After you enter your credentials, you should see your portal display that the update was deployed.</span></span>
7. <span data-ttu-id="a509b-177">Ha a Postmannel ismét eléri az Azure App Service-ben futó, újonnan telepített  API-alkalmazást, azt fogja tapasztalni, hogy az API viselkedése konzisztens, és most már a várt módon adja vissza a névjegyadatokat, és a Swagger.io által generált Java-kódot használja.</span><span class="sxs-lookup"><span data-stu-id="a509b-177">If you once again use Postman to hit the newly-deployed API App running in Azure App Service, you'll see that the behavior is consistent and that now it is returning contact data as expected, and using simple code changes to the Swagger.io scaffolded Java code.</span></span> 
   
    ![A Java Contacts REST API használata élőben az Azure-ban][postman-calling-azure-contacts]

## <a name="next-steps"></a><span data-ttu-id="a509b-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a509b-179">Next steps</span></span>
<span data-ttu-id="a509b-180">Ebben a cikkben egy Swagger JSON-fájllal és a Swagger.io szerkesztő által generált Java-kóddal indultunk el.</span><span class="sxs-lookup"><span data-stu-id="a509b-180">In this article, you were able to start with a Swagger JSON file and some scaffolded Java code obtained from the Swagger.io editor.</span></span> <span data-ttu-id="a509b-181">Ezekből egyszerű módosításokkal és a Git telepítési folyamatának eredményeként egy működő, Java nyelven írt API-alkalmazást kaptunk.</span><span class="sxs-lookup"><span data-stu-id="a509b-181">From there, your simple changes and a Git deploy process resulted in having a functional API app written in Java.</span></span> <span data-ttu-id="a509b-182">A következő oktatóanyag azt mutatja be, hogyan [használhat API-alkalmazásokat JavaScript-ügyfelekkel a CORS segítségével][App Service API CORS].</span><span class="sxs-lookup"><span data-stu-id="a509b-182">The next tutorial shows how to [consume API apps from JavaScript clients, using CORS][App Service API CORS].</span></span> <span data-ttu-id="a509b-183">A sorozat későbbi részei bemutatják, hogyan implementálhatja a hitelesítést és az engedélyezést.</span><span class="sxs-lookup"><span data-stu-id="a509b-183">Later tutorials in the series show how to implement authentication and authorization.</span></span>

<span data-ttu-id="a509b-184">Erre a példára építve részletesen megismerheti, hogyan használhatja a [Storage SDK for Java] csomagot a JSON-blobok megtartásához.</span><span class="sxs-lookup"><span data-stu-id="a509b-184">To build on this sample, you can learn more about the [Storage SDK for Java] to persist the JSON blobs.</span></span> <span data-ttu-id="a509b-185">Vagy a [Document DB Java SDK] használatával mentheti a névjegyadatokat az Azure Document DB rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="a509b-185">Or, you could use the [Document DB Java SDK] to save your Contact data to Azure Document DB.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="a509b-186">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a509b-186">See Also</span></span>
<span data-ttu-id="a509b-187">További információk az Azure és a Java együttes használatáról lásd: [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="a509b-187">For more information about using Azure with Java, visit [Azure for Java developers](/java/azure).</span></span>

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure Portal]: https://portal.azure.com/
[Document DB Java SDK]: ../documentdb/documentdb-java-application.md
[Ingyenes]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editort]: http://editor2.swagger.io/
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
