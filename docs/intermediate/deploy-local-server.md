# Set up and Deploy a Local Test Server

Welcome to this introduction to the Pact local Test server!

The goal of this tutorial is to help you run and deploy a working Pact test server locally.


**Topics covered in this tutorial**

* Install Dependencies
* Package.json
* YAML API Request
* Endpoint Types
* Exercise: Write a YAML file

In this tutorial you will go into depth on the **pact-lang-api** including topics like yaml API requests and endpoint types. Aside from letting you explore even more of pact’s functionality, it puts you in a good position to complete our next tutorial, Pact and Javascript. In that tutorial you bring all the pieces together and get your first look at developing your first full stack blockchain application with pact.

___

## **Set up and Deploy a Local Test Server Tutorial**

<iframe width="720" height="405" src="https://www.youtube.com/embed/fbZpsQAqz48" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Subscribe to our <a href="https://www.youtube.com/channel/UCB6-MaxD2hlcGLL70ukHotA" target="_blank">YouTube channel </a> to access the latest Pact tutorials.

___

## **Install Dependencies**

Pact and the pact-lang-api are required to successfully complete this tutorial. If you haven’t already installed these, follow the steps below to install each of them on your device.

### **Install Pact**

First, install Pact on your computer using homebrew by running the following code in your terminal.

``` terminal
brew install kadena-io/pact/pact
```

!!! Note
      For more information on installing Pact, you can view the <a href="https://github.com/kadena-io/pact" target="_blank">Pact GitHub page</a>. You can also view <a href="./beginner/pact-on-atom-sdk.html" target="_blank">8. Atom SDK </a>from the Pact Beginner tutorial series for more information.

### **Install pact-lang-API**

Install the pact-lang-api by running the following code in your terminal.

``` terminal
npm install pact-lang-api
```

!!! Note
      You can view more about installing the Pact Lang API <a href="https://github.com/kadena-io/pact#quick-start-pact-rest-api-server" target="_blank">here</a>. Further details about the pact-lang-api are <a href="https://www.npmjs.com/package/pact-lang-api" target="_blank">here</a>.

___

## **Project Overview**

To get started, here is a brief overview of the project you will build. The goal is to run a Pact server locally that you can communicate with using various pact-lang-api commands from your terminal.

![1-overview](../assets/intermediate-tutorials/deploy-local-server/1-overview.png)

Starting in your terminal, you will create a project directory along with an **example.yaml** and a **config.yaml** file to store your code. The **example.yaml** file will include the program you are trying to run, and the **config.yaml** file will specify things such as the port, which will be on **8080**, and the path to the log folder that will log the data.

Next, you will populate each of the yaml files, create the **log** folder, and run the **Pact server**.

After the server is running you will need a way to send the code in the example.yaml file to the pact server. This is done by first converting the code into JSON, then by sending this json to the server using a variety of commands including send, local, listen, or poll depending on your needs.

I’ll show you each of these commands in more detail as we go through the tutorial. For now, this hopefully helped you organize some of your thoughts around what we are planning to build.

Let’s start building!

___

## **Project Demonstration**

To begin your project, open your terminal and navigate to your preferred project directory.

Within that directory, create a new directory named deploy-tutorial to house your project files.

``` terminal
mkdir deploy-tutorial
```
### **Create Project files**

Navigate into the folder and create both a **config.yaml** file and an **example.yaml** file.

``` terminal
cd deploy-tutorial
```
``` terminal
touch config.yaml
```
``` terminal
touch example.yaml
```

### **Populate Project Files**

The config.yaml file follows a standard format, and you can find the code for this <a href="https://github.com/kadena-io/pact/blob/master/config.yaml" target="_blank">here</a>. It is also included below.

Copy the code below into your **config.yaml** and save the file.

``` terminal
# Config file for pact http server. Launch with `pact -s config.yaml`

# HTTP server port
port: 8080

# directory for HTTP logs
logDir: log

# persistence directory
persistDir: log

# SQLite pragmas for pact back-end
pragmas: []

# verbose: provide log output
verbose: True

```

Next, populate the **example.yaml** file with the example provided below. This code is also included in the Pact language documentation <a href="https://pact-language.readthedocs.io/en/latest/pact-reference.html#api-request-formatter" target="_blank">here</a>.

Copy this text, paste it into your **example.yaml** file, and save the file.

``` terminal
code: "(+ 1 2)"
data:
  name: Stuart
  language: Pact
keyPairs:
  - public: ba54b224d1924dd98403f5c751abdd10de6cd81b0121800bf7bdbdcfaec7388d
    secret: 8693e641ae2bbe9ea802c736f42027b03f86afe63cae315e7169c9c496c17332
```

### **Config.yaml Details**

Navigate back to the config.yaml file to view the details of this code.

Here you can see 5 fields.

| HTTP Server       | The HTTP server port specifies 8080. This can change depending on which port you need but we often use 8080 in our examples so you can leave this as it is for now.                  |
|-------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Logdir/PersistDir | The directory and persistence directory for http logs states where the applications data will be stored. This specifies a folder location that you will create for yourself shortly. |
| Sqlite Pragmas    | The SQLite pragmas.                                                                                                                                                                  |
| Verbose           | Specifies whether or not you want to provide log output using true or false.                                                                                                         |

### **Example.yaml Details**

Navigate back to the **example.yaml** file to view the details of this code.

Here you can see the code that will be running on the Pact server. In this case it includes the code `1 + 2`, the name `Stuart`, and the language `Pact`. You can also view the keyPairs for this code.

This code is formatted using the **Request YAML file format** specified in the Pact language documentation <a href="https://pact-language.readthedocs.io/en/latest/pact-reference.html#request-yaml-file-format" target="_blank">here</a>.

``` terminal
code: Transaction code
codeFile: Transaction code file
data: JSON transaction data
dataFile: JSON transaction data file
keyPairs: list of key pairs for signing (use pact -g to generate): [
  public: base 16 public key
  secret: base 16 secret key
  ]
nonce: optional request nonce, will use current time if not provided
from: entity name for addressing private messages
to: entity names for addressing private messages
```

Here you’ll see that the format includes keys such as code, codefile, data, datafile, keypairs, nonce, from, and to.

Each of these have their own use case but only the **code** and **keypairs** are required. Looking at your example.yaml file, you can see that these 2 keys are included.


!!! Note
      Some things to note are that if you use the data key it defaults to an empty object, and nonce defaults to the current date and time. Other details about each of these keys can be found in the <a href="https://pact-language.readthedocs.io/en/latest/pact-reference.html#request-yaml-file-format" target="_blank">documentation</a>.

___

## **Create Log folder**

Having seen the code, you can now focus on working from the terminal.

To start, you’ll need to create the log folder that was specified in the **config.yaml** file as the log directory.

From your terminal, create a log folder in your project directory.

``` terminal
mkdir log
```
Next, check the directory to make sure you’re all set up with your log and yaml files.

``` terminal
ls
```

Finally, run pact -s (-s stands for serve) and provide it the **config.yaml** file.

``` terminal
pact -s config.yaml
```

After running this you can see that you initialized a pact server and that it’s currently running on port 8080.

You can now run API calls on the server.

___

## **Endpoint types**

The Pact server contains multiple endpoint types that you can access using the pact-lang-api.

| Send   | /send takes in cmd object and returns tx hash                                                                                                               |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Listen | /listen takes in a hash and returns tx result.                                                                                                              |
| Poll   | /poll is similar to listen but works with multiple hashes and returns multiple tx results.                                                                  |
| Local  | /local takes in cmd object with code that queries from blockchain. It only runs in local server and does not impact the blockchain - and returns tx result. |

To get more familiar with each of these endpoints, we can run each of them from our terminal now.

___

### **/send demo**

Now that the Pact server is running, you use your terminal and the pact-lang-api to /send code to the Pact endpoint. /send takes in cmd object and returns tx hash. To run this command you need to do 2 things. First, you will format the existing **example.yaml** file into JSON. Next, you will send it to the Pact end point.

#### **JSON Format**

In a new terminal window within the same directory, run the following code.

``` terminal
pact -a example.yaml
```

This line formats the example file into type JSON.

After running this command, you should see the JSON data appear in the terminal.

Looking at the data you’ll see the hash, the signature, the payload and a few other details. Something you may recognize is that within the payload you’ll see the data including name `Stuart`, language `Pact`, and the code ``(+ 1 2)``.

!!! Note
      By default -a formats the YAML file into API requests for the /send endpoint. Adding the -l flag after the command formats the api request for the local endpoint.

Now that you have the data in the format you need you can send it to the REST endpoint.

#### **Use /send**

To send data to the REST endpoint, you need to use a curl command with a header of content type application json along with server and endpoint.

Run the following command in your terminal to /send data to the REST endpoint.

``` terminal
pact -a example.yaml | curl -H "Content-Type: application/json" -d @- http://localhost:8080/api/v1/send
```

This command starts with the code you ran previously and adds on a few important details.

First, it states the content type as `application/json`. It then sets the destination of the API call. In this case it sends an API call to the `localhost` on port `8080/`.

!!! Note
      By running this full command you’re both converting the file to JSON and making an API request call to the /send endpoint.

After running this command, you will receive request key as the response. This is the hash value of the command you sent to the REST endpoint.

``` terminal
{"requestKeys":["5Fu1m0crUk7MKL0Yyjg3gMPGcKuC0U4dx5Z5kBEQUz8"]}
```

Note: The hash value shown above is different than the one you will receive.

___

### **/poll demo**

Using the pact-lang-api, you can also run code and return the result of running that code.

This is done using another command, known as **/poll**.

**/poll** is similar to listen but works with multiple hashes and returns multiple tx results.

Run the following command in your terminal. Note that you will need to include the requestKeys by pasting the key you received when running /send into the space labeled <REQUEST-KEY>.

``` terminal
curl -H "Content-Type: application/json" -d '{"requestKeys":["<REQUEST-KEY>"]}' -X POST
http://localhost:8080/api/v1/poll
```

The code you run should look end up looking similar to the command shown below.

``` terminal
curl -H "Content-Type: application/json" -d '{"requestKeys":["18_CToCa_JXDvOPIHF-CceDw9gCpEMbpJKgLmot7I9M"]}' -X POST
http://localhost:8080/api/v1/poll
```

!!! Note
      As you may have noticed, you don’t need to convert the yaml file into JSON as you did when using /send. This is because you are sending the request key which is already in JSON format.

This command will return the result of the request made in the /send demo as shown below.

``` terminal
{"5Fu1m0crUk7MKL0Yyjg3gMPGcKuC0U4dx5Z5kBEQUz8":{"gas":0,"result":{"status":"success","data":3},"reqKey":"5Fu1m0crUk7MKL0Yyjg3gMPGcKuC0U4dx5Z5kBEQUz8","logs":"wsATyGqckuIvlm89hhd2j4t6RMkCrcwJe_oeCYr7Th8","metaData":null,"continuation":null,"txId":1}}
```

___

### **/listen demo**

Listen is similar to poll in that it can request the result of a transaction.

**/listen** takes in a hash and returns tx result.

The difference between these commands is subtle but important. You would use /listen when working with a single hash to return the result, and /poll when working with multiple hashes to return multiple transaction results.

The format of a /listen command looks like this.

``` terminal
curl -H "Content-Type: application/json" -d '{"listen":"pdjiSC6IN_enbolWiuLVynXYVv1os8E9YfptltXjckU
"}' -X POST http://localhost:8080/api/v1/listen
```

This command returns the result of the request made in the **/send** demo.

``` terminal
{"gas":0,"result":{"status":"success","data":3},"reqKey":"5Fu1m0crUk7MKL0Yyjg3gMPGcKuC0U4dx5Z5kBEQUz8","logs":"wsATyGqckuIvlm89hhd2j4t6RMkCrcwJe_oeCYr7Th8","metaData":null,"continuation":null,"txId":1}
```

#### **Why /listen?**

As you may have noticed, **/listen** is similar **/poll**.

The difference here is that you are listening to the hash value and that you are making an API request to the listen endpoint.

What you’ll get back, like with poll, is the result of running the code, which is again 3.

!!! Note
      /listen and /poll are very similar in this case but use /listen in cases that you only need to take in the result of a single hash and return a single transaction result. Use /poll if you need multiple hash values and multiple results.

___

### **/local demo**

The final command you can use to access the Pact REST endpoint is /local.

/local takes in cmd object with code that queries from blockchain - only runs in local server and does not impact the blockchain - and returns tx result.

The format of a /local command looks like this.

``` terminal
pact -a example.yaml -l | curl -H "Content-Type: application/json" -d @- http://localhost:8080/api/v1/local
```

This command is structured very similar to send. It converts the exampl.yaml file to JSON then makes an API request to the local endpoint. When running this, you’ll see that it returns data along with the request key.

#### **Why /local?**

/local takes in the cmd object with code that queries from blockchain. It only runs in the local server and does not impact the blockchain when returning tx results.

This is helpful to use when running a function that doesn’t need to touch the blockchain. This is different than **/send**, which you would use when changing data in the blockchain, or when deploying contracts. Local is a great choice when you are working with simple functions or fetching data from the blockchain.

___

## **EXERCISE: Write a YAML File**

At this point you hopefully have a local Pact server running and have some practice with running commands to access the variety of endpoints available to you.

We’ve set up a challenge to help you get even more familiar with setting up YAML files for your local pact server deployments.

On the Kadena GitHub page, there is a repo named pact-lang.org-code found <a href="https://github.com/kadena-io/pact-lang.org-code" target="_blank">here</a> that includes the code samples and challenges used throughout our tutorials.

You can clone this folder to get started with this challenge.

``` terminal
git clone https://github.com/kadena-io/pact-lang.org-code.git
```

From there navigate into the folder **intermediate > set-up-and-deploy > challenge**.

Your challenge is to acclimate yourself to the project structure and use what you’ve learned to both deploy the smart contract and call the **helloWorld** function.

___

## **Review**

Here’s a quick recap of what you accomplished throughout this tutorial.

![1-overview](../assets/intermediate-tutorials/deploy-local-server/1-overview.png)

Starting in your terminal, you created a project directory along with an **example.yaml** and a **config.yaml** file to store your code. You then populated each of these files with their code. You then created a **log** folder that holds the data from the Pact server and then ran the server on port **8080**.

After the server was running you converted the **example.yaml** file into JSON, then used curl commands to send it to the Pact server; either to store the data or to read the result of running it’s code.

And that’s everything you need to know to set up and deploy your local test server.

Good luck with your challenge, and when you’re ready, I’ll see you in the next tutorial.
