---
lab:
  title: Erstellen von Azure Cosmos DB-Ressourcen für NoSQL mithilfe von .NET
  description: 'Erfahren Sie, wie Sie Datenbank- und Container-Ressourcen in Azure Cosmos DB mit dem Microsoft .NET SDK v3 erstellen.'
---

# Erstellen von Azure Cosmos DB-Ressourcen für NoSQL mithilfe von .NET

In dieser Übung erstellen Sie eine Konsolen-App, die einen Container, eine Datenbank und ein Element in Azure Cosmos DB erstellt.

In dieser Übung ausgeführte Aufgaben:

* Erstellen eines Azure Cosmos DB-Kontos
* Schreiben der Konsolen-App
* Ausführen der Konsolen-App und Überprüfen der Ergebnisse

Diese Übung dauert ca. **30** Minuten.

## Vor der Installation

Stellen Sie vorab sicher, dass die folgenden Anforderungen erfüllt sind:

* Ein Azure-Abonnement. Wenn Sie noch keines besitzen, können Sie sich dafür [registrieren](https://azure.microsoft.com/).

## Erstellen eines Azure Cosmos DB-Kontos

In diesem Abschnitt der Übung erstellen Sie eine Ressourcengruppe und ein Azure Cosmos DB-Konto. Sie notieren auch den Endpunkt und den Zugriffsschlüssel für das Konto.

1. Navigieren Sie in Ihrem Browser zum Azure-Portal [https://portal.azure.com](https://portal.azure.com) und melden Sie sich mit Ihren Azure-Anmeldeinformationen an, wenn Sie dazu aufgefordert werden.

1. Verwenden Sie die Schaltfläche **[\>_]** rechts neben der Suchleiste oben auf der Seite, um eine neue Cloud Shell im Azure-Portal zu erstellen, und wählen Sie eine ***Bash***-Umgebung aus. Die Cloud Shell bietet eine Befehlszeilenschnittstelle in einem Bereich am unteren Rand des Azure-Portals.

    > **Hinweis**: Wenn Sie zuvor eine Cloud Shell erstellt haben, die eine *PowerShell*-Umgebung verwendet, wechseln Sie diese zu ***Bash***.

1. Wählen Sie in der Cloud Shell-Symbolleiste im Menü **Einstellungen** das Menüelement **Zur klassischen Version wechseln** aus (dies ist für die Verwendung des Code-Editors erforderlich).

1. Erstellen Sie eine Ressourcengruppe für die Ressourcen, die für diese Übung benötigt werden. Ersetzen Sie **\<myResourceGroup>** durch einen Namen, den Sie für die Ressourcengruppe verwenden möchten. Bei Bedarf können Sie **useast** durch eine Region in Ihrer Nähe ersetzen. 

    ```
    az group create --location useast --name <myResourceGroup>
    ```

1. Führen Sie den folgenden Befehl aus, um das Azure Cosmos DB-Konto zu erstellen. Ersetzen Sie **\<myCosmosDBacct>** durch einen *eindeutigen* Namen, um Ihr Azure Cosmos DB-Konto zu identifizieren. Ersetzen Sie **\<myResourceGroup>** durch den zuvor gewählten Namen.

    >**Hinweis:** Der Kontoname darf nur Kleinbuchstaben, Zahlen und das Bindestrich-Zeichen (-) enthalten. Sie muss zwischen drei und 31 Zeichen lang sein. *Die Ausführung dieses Befehls dauert einige Minuten.*

    ```
    az cosmosdb create -n <myCosmosDBacct> -g <myResourceGroup>
    ```

1.  Führen Sie den folgenden Befehl aus, um den **documentEndpoint** für das Azure Cosmos DB-Konto abzurufen. Notieren Sie sich den Endpunkt aus den Befehlsergebnissen, er wird später in der Übung benötigt. Ersetzen Sie **\<myCosmosDBacct>** und **\<myResourceGroup>** durch die von Ihnen erstellten Namen.

    ```bash
    az cosmosdb show -n <myCosmosDBacct> -g <myResourceGroup> --query "documentEndpoint" --output tsv
    ```

1. Rufen Sie mithilfe des folgenden Befehls den Primärschlüssel für das Konto ab. Notieren Sie sich den Wert **primaryMasterKey** aus den Befehlsergebnissen für die Verwendung im Code. Ersetzen Sie **\<myCosmosDBacct>** und **\<myResourceGroup>** durch die von Ihnen erstellten Namen.

     ```
    # Retrieve the primary key
    az cosmosdb keys list -n <myCosmosDBacct> -g <myResourceGroup>
    ```

## Erstellen der Konsolenanwendung

Nachdem die erforderlichen Ressourcen nun in Azure bereitgestellt wurden, besteht der nächste Schritt im Einrichten der Konsolenanwendung mithilfe desselben Terminalfensters in Visual Studio Code.

1. Erstellen Sie einen Ordner für das Projekt, und öffnen Sie diesen.

    ```bash
    mkdir cosmosdb
    cd cosmosdb
    ```

1. Erstellen Sie die .NET-Konsolen-App.

    ```dotnetcli
    dotnet new console --framework net8.0
    ```

1. Führen Sie die folgenden Befehle aus, um das Paket **Microsoft.Azure.Cosmos** zum Projekt hinzuzufügen, sowie das unterstützende Paket **Newtonsoft.Json**.

    ```dotnetcli
    dotnet add package Microsoft.Azure.Cosmos --version 3.*
    dotnet add package Newtonsoft.Json --version 13.*
    ```

Nun ist es an der Zeit, den Vorlagencode in der Datei **Program.cs** mithilfe des Editors in der Cloud Shell zu ersetzen.

### Hinzufügen des Startcodes für das Projekt

1. Führen Sie den folgenden Befehl in der Cloud-Shell aus, um mit der Bearbeitung der Anwendung zu beginnen.

    ```bash
    code Program.cs
    ```

1. Ersetzen Sie den vorhandenen Code durch den folgenden Codeschnipsel nach den **using**-Anweisungen. Achten Sie darauf, dass Sie die Platzhalterwerte für **\<documentEndpoint>** und **\<primaryKey>** gemäß den Anweisungen in den Codekommentaren ersetzen.

    Der Code stellt die Gesamtstruktur der App und einige erforderliche Elemente bereit. Überprüfen Sie die Kommentare im Code, und fügen Sie Code in Bereichen hinzu, die in den Anweisungen angegeben sind. 

    ```csharp
    using Microsoft.Azure.Cosmos;
    
    namespace CosmosExercise
    {
        // This class represents a product in the Cosmos DB container
        public class Product
        {
            public string? id { get; set; }
            public string? name { get; set; }
            public string? description { get; set; }
        }
    
        public class Program
        {
            // Cosmos DB account URL - replace with your actual Cosmos DB account URL
            static string cosmosDbAccountUrl = "<documentEndpoint>";
    
            // Cosmos DB account key - replace with your actual Cosmos DB account key
            static string accountKey = "<primaryKey>";
    
            // Name of the database to create or use
            static string databaseName = "myDatabase";
    
            // Name of the container to create or use
            static string containerName = "myContainer";
    
            public static async Task Main(string[] args)
            {
                // Create the Cosmos DB client using the account URL and key
    
    
                try
                {
                    // Create a database if it doesn't already exist
    
    
                    // Create a container with a specified partition key
    
    
                    // Define a typed item (Product) to add to the container
    
    
                    // Add the item to the container
                    // The partition key ensures the item is stored in the correct partition
    
    
                }
                catch (CosmosException ex)
                {
                    // Handle Cosmos DB-specific exceptions
                    // Log the status code and error message for debugging
                    Console.WriteLine($"Cosmos DB Error: {ex.StatusCode} - {ex.Message}");
                }
                catch (Exception ex)
                {
                    // Handle general exceptions
                    // Log the error message for debugging
                    Console.WriteLine($"Error: {ex.Message}");
                }
            }
        }
    }
    ```

### Hinzufügen von Code zum Erstellen des Clients und Ausführen von Vorgängen 

In diesem Abschnitt der Übung fügen Sie Code in bestimmte Bereiche der Projekte ein, um den Client, die Datenbank und den Container zu erstellen und ein Beispielelement zum Container hinzuzufügen.

1. Fügen Sie den folgenden Code in den Bereich nach dem Kommentar **// Create the Cosmos DB client using the account URL and key** ein. Dieser Code definiert den Client, der zum Herstellen einer Verbindung mit Ihrem Azure Cosmos DB-Konto verwendet wird.

    ```csharp
    CosmosClient client = new(
        accountEndpoint: cosmosDbAccountUrl,
        authKeyOrResourceToken: accountKey
    );
    ```

    >Hinweis: Es empfiehlt sich, **DefaultAzureCredential** aus der *Azure Identity-Bibliothek* zu verwenden. Je nachdem, wie Ihr Abonnement eingerichtet ist, sind hierfür möglicherweise einige zusätzliche Konfigurationen in Azure erforderlich. 

1. Fügen Sie den folgenden Code in den Bereich nach dem Kommentar **// Create a database if it doesn't already exist** ein. 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. Fügen Sie den folgenden Code in den Bereich nach dem Kommentar **// Create a container with a specified partition key** ein. 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. Fügen Sie den folgenden Code in den Bereich nach dem Kommentar **// Define a typed item (Product) to add to the container** ein. Dadurch wird das Element definiert, das dem Container hinzugefügt wird.

    ```csharp
    Product newItem = new Product
    {
        id = Guid.NewGuid().ToString(), // Generate a unique ID for the product
        name = "Sample Item",
        description = "This is a sample item in my Azure Cosmos DB exercise."
    };
    ```

1. Fügen Sie den folgenden Code in den Bereich nach dem Kommentar **// Add the item to the container** ein. 

    ```csharp
    ItemResponse<Product> createResponse = await container.CreateItemAsync(
        item: newItem,
        partitionKey: new PartitionKey(newItem.id)
    );
    ```

1. Nachdem der Code vollständig ist, speichern Sie Ihre Arbeit mit **STRG + S** und schließen Sie den Editor mit **STRG + Q**.

1. Führen Sie den folgenden Befehl in der Cloudshell aus, um auf Fehler im Projekt zu testen. Sollten Sie Fehler feststellen, öffnen Sie die Datei *Program.cs* im Editor und überprüfen Sie diese auf fehlenden Code oder Fehler beim Einfügen.

    ```bash
    dotnet build
    ```

Nachdem das Projekt fertig ist, ist es an der Zeit, die Anwendung auszuführen und die Ergebnisse im Azure-Portal zu überprüfen.

## Ausführen der Anwendung und Überprüfen der Ergebnisse

1. Führen Sie den Befehl `dotnet run` aus, wenn Sie sich in der Cloud Shell befinden. Die Ausgabe sollte in etwa dem folgenden Beispiel entsprechen.

    ```
    Created or retrieved database: myDatabase
    Created or retrieved container: myContainer
    Created item: c549c3fa-054d-40db-a42b-c05deabbc4a6
    Request charge: 6.29 RUs
    ```

1. Navigieren Sie im Azure-Portal zu der zuvor erstellten Azure Cosmos DB-Ressource. Wählen Sie in der linken Navigation **Daten-Explorer** aus. Wählen Sie im **Daten-Explorer** **myDatabase** und erweitern Sie dann **myContainer**. Sie können den von Ihnen erstellten Artikel anzeigen, indem Sie **Artikel** auswählen.

    ![Screenshot, der die Position der Elemente im Daten-Explorer anzeigt.](./media/09/cosmos-data-explorer.png)

## Bereinigen von Ressourcen

Nachdem Sie die Übung beendet haben, sollten Sie die von Ihnen erstellten Cloud-Ressourcen löschen, um eine unnötige Ressourcennutzung zu vermeiden.

1. Navigieren Sie zu der Ressourcengruppe, die Sie erstellt haben, und zeigen Sie den Inhalt der in dieser Übung verwendeten Ressourcen an.
1. Wählen Sie auf der Symbolleiste die Option **Ressourcengruppe löschen** aus.
1. Geben Sie den Namen der Ressourcengruppe ein, und bestätigen Sie, dass Sie sie löschen möchten.
