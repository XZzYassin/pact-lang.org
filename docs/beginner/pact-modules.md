# Pact Modules

This tutorial introduces an essential feature in Pact known as modules. We’ll discuss what modules are, why they’re important, and how they relate to smart contracts.

**Tutorial Overview**

* Introduction to Pact Modules
* Create a Module
* Example Modules

!!! Summary "Key Takeaway"
      A Pact Module contains the logic necessary to create your application. It is where all code is written to for a smart contract. All API and data definitions exist within the Module.
___

## **Pact Modules Tutorial**

<iframe width="720" height="405" src="https://www.youtube.com/embed/MjOglMjxYT4" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Subscribe to our <a href="https://www.youtube.com/channel/UCB6-MaxD2hlcGLL70ukHotA" target="_blank">YouTube channel </a> to access the latest Pact tutorials.

## **Introduction to Pact Modules**

A Pact <a href="https://pact-language.readthedocs.io/en/latest/pact-reference.html#module-declaration" target="_blank">Module</a> contains the logic necessary to create your application. It is where all code is written to for a smart contract. All API and data definitions exist within the Module.

**Modules contain the following code**

* Schema Definitions
* Table Definitions
* Functions Definitions
* Pact Special Functions
* Const Values

**Modules do not contain the following code**

While modules contain the code needed to create a smart contract, some things are not included in a module.

* Keysets
* Table Creation
* Function Calls

For more information on keysets, see our Pact Keysets Tutorial.

___

### **Module vs Smart Contract**

When starting with Pact, you’ll often create single modules that contain the full functionality of your smart contract.

**This raises an important question...**

Why aren’t modules called smart contracts?

The reason for this becomes clear once you begin creating more complicated smart contracts. These smart contracts will often contain many modules that work together to create your application. The goal of each module should be to create a focused set of functionality and clear organization within your files.

While one module can make one smart contract, many modules can also make one smart contract. For that reason, we refer to the logic within a Pact file as a module.

For more information see <a href="https://pact-language.readthedocs.io/en/latest/pact-reference.html#module-declaration" target="_blank">modules</a> in the developer documentation.

___

### **Create a Module**

To create a module, you write the module keyword and name followed by the keyset that has access to call this module.

Here is a module named **example** that gives access to an **admin-keyset**.

``` clojure
;; define and read keysets

(module example 'admin-keyset
    ;; module code goes here
)

;; function calls

```

The entire module is written within these parentheses. The top level code, like keysets, are defined outside of these parentheses.

The basic idea of modules is simple, but the structure of the code and the actual logic within the module can get pretty complicated. For that reason, it’s helpful to start by focusing on the basic syntax and structure of existing modules.

___


### **Example Modules**

You can find examples using the Module Explorer in the <a href="https://pact.kadena.io/" target="_blank">Pact Online Editor</a>.




#### **Hello World**

From the Module Explorer, open the Hello World Smart Contract. If you’re interested, this smart contract is explained in depth in the <a href="./beginner/hello-world-with-pact.html" target="_blank">Hello World with Pact</a>.


Notice that the pattern of this smart contract is similar to the outline described above.

``` clojure
;; define and read keysets
(define-keyset 'admin-keyset (read-keyset "admin-keyset"))

(module helloWorld 'admin-keyset
   ;; module code goes here
  (defun hello (name)
    (format "Hello {}!" [name]))
)

;; function calls
(hello "world")

```

In this case, the module code only contains a simple function named **hello**. Later modules will include much more complexity.

#### **Simple Payment**

Next, take some time to look through the simple payment module. From the Module Explorer, select **open** on **Simple Payment** to open this smart contract in the online editor.

Once again, you’ll notice the same pattern. This time, the smart contract includes a few other features.

**Included in the Simple Payment Smart Contract**

* Schema Definitions
* Table Definitions
* Table Creation
* Functions
* Function Calls

View the summary of this contract below and take some time to investigate the actual code provided in the **Simple Payment Module**.

``` clojure
;; define and read keysets
(define-keyset 'admin-keyset (read-keyset "admin-keyset"))

(module payments 'admin-keyset
   ;; module code goes here

   ;; define schemas
   ;;    ex. defschema payments...

   ;; define tables
   ;;    ex. deftable payments-table:{payments})

   ;; define functions
   ;;    ex. defun create-account...
   ;;    ex. defun get-balance...
   ;;    ex. defun pay...

)
;; create tables
    ;; ex. create-table payments-table

;; function calls
    ;; ex. pay
    ;; ex. get-balance
```

While you may not fully understand each line of code quite yet, there are a few important things to note here.

First, schemas and tables are defined inside of modules, but tables are created outside of modules. Table and schema definitions include built in functions including defschema, deftable, and create-table. You’ll learn more about each of these functions in the **Pact Schemas and Tables tutorial**.

Next, there are some new Pact built in functions that you may have not seen before.

**Contract Built-in functions include:**

* <a href="https://pact-language.readthedocs.io/en/latest/pact-functions.html#enforce" target="_blank">enforce</a>
* <a href="https://pact-language.readthedocs.io/en/latest/pact-functions.html#insert" target="_blank">insert</a>
* <a href="https://pact-language.readthedocs.io/en/latest/pact-functions.html#with-read" target="_blank">with-read</a>


We’ll also go over each of these in more detail in later tutorials. You can explore the <a href="https://pact-language.readthedocs.io/en/latest/pact-functions.html" target="_blank">Pact Language Reference built-in functions</a> to learn more about each of them now if you’d like.

___

### **Explore Other Modules**
Other modules include the same basic framework as those included in these simple smart contracts. Take some time now to explore each of the other Smart Contract examples to get more familiar with how they work.

**Example Modules**

* International Payment
* Verification
* Accounts

Focus on understanding the structure of each module. Gain some familiarity with the different techniques used to create each smart contract.

___

## **Review**
That wraps up this tutorial on **Pact Modules**.

In this tutorial you were introduced to modules, one of the core features of the Pact Programming Language. You learned what modules are, why they’re important, and went over a few examples.

The goal for this tutorial wasn’t to help you learn all of the code that can possibly go into creating a module. That’s what most of the entire Pact programming language is built to accomplish. You’ll learn much more about this in later tutorials.

For now, the important takeaway is to understand what a module is, as well as to recognize the key elements that belong inside and outside of Pact modules.

Take some time now to continue exploring the examples provided in the Pact Online Editor. When you’re ready, you can learn more about the Pact programming language in our next tutorial.


