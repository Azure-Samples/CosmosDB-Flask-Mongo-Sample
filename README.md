---
services: cosmos-db
platforms: java
author: heatherbshapiro
---

# Build a Flask app using Azure Cosmos DB for MongoDB API
Azure Cosmos DB is a fully managed globally distributed, multi-model database service, transparently replicating your data across any number of Azure regions. You can elastically scale throughput and storage, and take advantage of fast, single-digit-millisecond data access using the API of your choice backed by 99.999 SLA. This sample shows you how to use the Azure Cosmos DB for MongoDB API to store and access data from a Flask application.

## Prerequisites

- Download the [Azure Cosmos DB Emulator](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator). The emulator is currently only supported on Windows. The sample shows how to use the sample with a production key from Azure, which can be done on any platform.

- If you don’t already have Visual Studio Code installed, you can quickly install [VS Code](https://code.visualstudio.com/Download) for your platform (Windows, Mac, Linux).

- Be sure to add Python Language support by installing one of the popular Python extensions.
    1. Select an extension.
    2. Install the extension by typing `ext install` into the Command Palette `Ctrl+Shift+P`.

    The examples in this document use Don Jayamanne's popular and full featured [Python Extension](https://marketplace.visualstudio.com/items?itemName=donjayamanne.python).

## Clone the sample application

Now let's clone the app, set the connection string, and run it.

1. Open a git terminal window, such as git bash, and `cd` to a working directory.
2. Clone this sample repository
3. Run the following command to install the python modules.
    ```bash
    pip install -r .\requirements.txt
    ```
4. Open the folder in Visual Studio Code or your IDE of choice.

## Review the code

Let's take a quick review of what's happening in the app. Open the **app.py** file under the root directory and you find that these lines of code create the Azure Cosmos DB connection. The following code uses the connection string for the local Azure Cosmos DB Emulator. The password needs to be split up as seen below to accommodate for the forward slashes that cannot be parsed otherwise.

* Initialize the MongoDB client, retrieve the database, and authenticate.

    ```python
    client = MongoClient("mongodb://127.0.0.1:10250/?ssl=true") #host uri
    db = client.test    #Select the database
    db.authenticate(name="localhost",password='C2y6yDjf5' + r'/R' + '+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw' + r'/Jw==')
    ```

* Retrieve the collection or create it if it does not already exist.

    ```python
    todos = db.todo #Select the collection
    ```

* Create the app

    ```Python
    app = Flask(__name__)
    title = "TODO with Flask"
    heading = "ToDo Reminder"
    ```
    
## Run the web app

1. Make sure the Azure Cosmos DB Emulator is running.

2. Open a terminal window and `cd` to the directory that the app is saved in.

3. Then set the environment variable for the Flask app with `set FLASK_APP=app.py` or `export FLASK_APP=app.py` if you are using a Mac.

4. Run the app with `flask run` and browse to [http://127.0.0.1:5000/](http://127.0.0.1:5000/).

5. Add and remove tasks and see them added and changed in the collection.

## Deploy to Azure

To deploy this app, you can create a new web app in Azure and enable continuous deployment with a fork of this github repo. Follow this [tutorial](https://docs.microsoft.com/en-us/azure/app-service-web/app-service-continuous-deployment) to set up continuous deployment with Github in Azure.

When deploying to Azure, you should remove your application keys and make sure the section below is not commented out:

```python
    client = MongoClient(os.getenv("MONGOURL"))
    db = client.test    #Select the database
    db.authenticate(name=os.getenv("MONGO_USERNAME"),password=os.getenv("MONGO_PASSWORD"))
```

You then need to add your MONGOURL, MONGO_PASSWORD, and MONGO_USERNAME to the application settings. You can follow this [tutorial](https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-configure#application-settings) to learn more about Application Settings in Azure Web Apps.

If you don't want to create a fork of this repo, you can also click the deploy to Azure button below. You should then go into Azure and set up the application settings with your Cosmos DB account info.

<a href="https://deploy.azure.com/?repository=https://github.com/heatherbshapiro/To-Do-List---Flask-MongoDB-Example" target="_blank">
<img src="http://azuredeploy.net/deploybutton.png"/>
</a>

[!NOTE]
If you plan to store your code in Github or other source control options, please be sure to remove your connection strings from the code. They can be set with application settings for the web app instead.
