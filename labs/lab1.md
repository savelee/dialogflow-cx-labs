<!--
    Copyright 2021 Google LLC

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
-->
# Build a retail virtual agent with Dialogflow CX

[Download & Import Intermediate CX Agent - Reusable Parts (Flows, Entities, Intents)](https://github.com/savelee/dialogflow-cx-labs/blob/master/agents/lab1-intermediate.blob) | [Download & Import Finalized CX Agent](https://github.com/savelee/dialogflow-cx-labs/blob/master/agents/lab1-complete.blob)


## Before you begin

Duration: 1:00

In this codelab, you'll learn how to build a retail chatbot with <a href="https://cloud.google.com/dialogflow?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-"><strong>Dialogflow CX</strong></a>, a Conversational AI Platform (CAIP) for building conversational UIs.
Dialogflow CX can implement virtual agents, like: chatbots, voice bots, phone gateways and can support multiple channels in over 50 different languages.

This codelab will guide you how to build a website chatbot for a retail. The fictional business we are building the chatbot for is called: <strong>G-Records</strong>.
G-Records is a rock record label, based in California. The label has 4 rock bands signed; <strong>Alice Googler</strong>, <strong>G's N' Roses</strong>, <strong>The Goo Fighters</strong> and <strong>The Google Dolls</strong>.
G-Records is selling band merchandise to all rock fans.

At the end of this codelab, you can use the chatbot, to order shirts or music or you can ask about your order.

![Final Result](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/result1.png?raw=true)

### What you'll learn

You will learn the benefits of Dialogflow CX compared to Dialogflow ES by doing! It includes the following concepts:

+   How to create a Dialogflow CX virtual agent within Google Cloud
+   Learn how to create flows
+   Learn how to create entities
+   Learn how to create intents
+   Learn how to create pages and transition pages with state handlers
+   Learn how to transition pages with intent routes
+   Learn how to transition pages with parameters & condition routes
+   Learn how to return conditional responses with system functions
+   Learn how to create fallback messages
+   Learn how to use the simulator
+   Learn how to create test cases & test coverage

The final Dialogflow CX agent design will look like this:

![Final Result](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/dialogflow-interface.png?raw=true)

>**Note:** We will approach this lab as building a real world virtual agent! We will show you a conversation design (prototype/flow chart) of the dialogue tree. Typically conversational designers create these types of prototypes. It's based on data (conversation analytics, the most common questions that have been asked). We will also give you a UML diagram that shows the relation of the entities (such as Artists or Merchandise product types). Typically you would get this information from a database, as that will also be the place where you store the user input. You will start this lab by first creating the reusable parts. (Flows, Entities and Intents). Afterwards, you will configure the flows by creating Pages, Transitions,
and Fulfillments. Enjoy!

You can breakdown the workshop in the following 4 parts:

* Prerequisites - Everything you need to set up a Dialogflow CX environment.
* Reusable parts - All the parts that are reusable, such as intents, entities and flows.
* State Machine, this is the Dialogflow CX magic, and the most important part of this lab.
* Finalizing the agent - Finalize to get a full working end-to-end chatbot.

>**Note:** Download the agent blob exports: [Lab 1 Intermediate Export](https://github.com/savelee/dialogflow-cx-labs/blob/master/agents/lab1-intermediate.blob) or [Lab 1 Final Export](https://github.com/savelee/dialogflow-cx-labs/blob/master/agents/lab1-complete.blob) to import/restore the agent, in case you want to skip the manual steps. To do this, go to [https://dialogflow.cloud.google.com/cx/projects](https://dialogflow.cloud.google.com/cx/projects), select your Google Cloud project, select or create an agent in *us-central1*. Click the options icon in the table (this is the icon with the 3 bullets) in the (view agents / locations overview). Click *Restore* > *Upload*, and select one of the blobs. Click *Restore*. See the below screenshots.

![Restore an agent, first select: View Agents](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/restore-options0.png?raw=true)
![Restore an agent, click: Restore](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/restore-options1.png?raw=true)
![Restore an agent, select the blob](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/restore-options2.png?raw=true)


### What you'll need

+   You'll need a Google Identity / Gmail address to create a Dialogflow CX agent.
+   Access to Google Cloud.


>**Note:** Each new Dialogflow CX customer will receive a $600 credit for a free trial of Dialogflow CX. This credit is automatically activated upon using Dialogflow CX for the first time and expires after 12 months. 
This is a Dialogflow-specific extension of the Google Cloud free trial. For more information about pricing see the [pricing overview](https://cloud.google.com/dialogflow/pricing?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-).


## Environment Setup

Duration: 9:00

### Create a Google Cloud project

Since Dialogflow CX runs in Google Cloud, you must **create a Google Cloud project**. A project organizes all your Google Cloud resources. It consists of a set of collaborators, enabled APIs (and other resources), monitoring tools, billing information, and authentication and access controls.

<button>[Open Google Cloud Console](https://console.cloud.google.com/projectselector2/home/dashboard?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-)</button>
When you create a new project, you will need to enter a **Project Name**. And you will have to link it to an existing Billing Account and Organization.

A billing account is used to define who pays for a given set of resources, and it can be linked to one or more projects. Project usage is charged to the linked billing account. In most cases, you configure billing when you create a project. For more information, see the [Billing documentation](https://cloud.google.com/billing/docs?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-). Make sure that billing is enabled for your Cloud project.

![Create a new project](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/create-project.png?raw=true)

### Enable the Dialogflow API

In order to make use of Dialogflow, you will have to enable the Dialogflow API for your project.

1. <button>[Enable the Dialogflow API](https://console.cloud.google.com/flows/enableapi?apiid=dialogflow.googleapis.com?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-)</button>
2. Select the project you want to enable the API for, and click **Continue**.

3. Collapse the menu of APIs & Services and click on **Credentials**

4. Click **Application Data**

5. Say **No, I am not using them** as you are not using Kubernetes Engine, App Engine or Cloud Functions for now.

6. Click **Done**

![Setup Credentials](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/credentials.png?raw=true)

### Create a new Dialogflow CX agent

To create a new Dialogflow CX agent, first open the Dialogflow CX Console:

<button>[Dialogflow CX Console](https://dialogflow.cloud.google.com/cx/projects?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-)</button>
1. Choose the previously created Google Cloud project.
2. Click **Create agent**.

Complete the form for basic agent settings:

* You can choose any display name.
* As location choose: **us-central1**
* Select your preferred time zone.
* Select **en - English** as default language

>**Note:** In Dialogflow CX you can create multiple agents per project. When you set up an agent in Dialogflow CX, you will have to tie it to a location explicitly.

Click **Create**.

![Create Agent](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/create-agent.png?raw=true)

Alright, we are all set. We finally can get started with modeling our virtual agent.

## Flows

Duration: 10:00

Complex dialogs often involve multiple conversation topics. In the case of the chatbot we are building for G-Records, for selling band merchandise,
we would have dialogs about the product catalog, payment, order status, and customer care questions. We could split these conversation topics into flows.

![Retail Flow](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/flows-start.png?raw=true)

[Flows](https://cloud.google.com/dialogflow/cx/docs/concept/flow?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) allow teams to work on individual conversation paths. A good practice would be to simplify the flow, so it fits easily. on a screen
and it's more modular.

Flows are a new concept to Dialogflow CX. Dialogflow Essentials has the concept Mega Agents, that are somehow similar to Flows. However, you would
use Flows much more often.

Later in this lab, we will use *state handlers* that can end a flow (so it will jump back to a next or previous flow), or you can end the full agent session.

Let's go and create some flows.

### Creating Flows

1. In [Dialogflow CX](https://dialogflow.cloud.google.com/cx/?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-), click on the **+** icon > **Create flow**.
2. Specify the name: `Catalog` and hit enter.

![Create a flow](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/create-flow.png?raw=true)

Your first flow *Catalog* has been created. Now create the other flows:

* `Order Process`
* `My Order`
* `Customer Care`

![Flows](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/flows3.png?raw=true)

Later in this lab we will set page state handlers, this will make sure that eventually the visualization will look like this:

![Flows](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/flows-visualization.png?raw=true)

### Simulator

On the right side of Dialogflow CX Console you can test the virtual agent, with the built-in simulator.
You can test the conversation from the beginning of the conversation, or from a particular flow.

1. Click on the **Test Agent** button, in the top right of your screen.
2. In the talk to agent field write: `Hello`
The virtual agent will respond with a default welcome text: *Greetings! How can I assist?*

![Simulator](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/simulator.png?raw=true)

Let's modify this default welcome text.

### Default Start Flow

Let's start by creating an *Intent Route* that will be triggered once you greet the virtual agent.

1. In the left *Build > Flows* sidebar, click on **Default Start Flow**, and select the **Start** tree node.

This will open the *Start* page. It automatically selected the Start page, in the *Build > Pages* sidebar section.

2. In *Start > Routes* click on the *Default Welcome Intent*.

An intent categorizes an end user’s intention for one conversation turn.
In Dialogflow CX, intents can be part of a state handler to route the next active page or fulfillment

3. Remove all the *Agent says* entries, and add this new text:

`Welcome, I am the virtual agent of G-Records, a fictional rock label. You can order artists merchandise, ask questions about your order or shipping, and I can tell you more which artists are currently signed with us. How can I help?`

To streamline the conversation, we will also need some quick reply buttons / suggestion chips.

4. Click on **Add dialogue option > Custom payload** and use the below code snippet.

5. Use the below code snippet as a Custom payload, and hit **Save**.

To read more about Custom payloads have a look into the [documentation](https://cloud.google.com/dialogflow/cx/docs/concept/integration/dialogflow-messenger#suggestion_chip_response_type?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-).

```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Which artists?"
            },
            {
              "text": "Which products?"
            },
            {
              "text": "About my order..."
            }
          ]
        }
      ]
    ]
  }
```

![Default Welcome Intent](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/welcome-intent2.png?raw=true)

5. Go ahead and test the welcome intent in the simulator.

You are probably wondering why you can't see any rich content. This is because rich content such as suggestion chips are dependent on an integration.

6. In the left sidebar, click on **Manage > Integrations**.

7. Choose **Dialogflow Messenger** and click on **Connect**.

8. In the popup click **Enable**.

![Integration Enable](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/integration-enable.png?raw=true)

Another popup will be shown, this time with integration JavaScript code that you can paste in your website to integrate the Dialogflow Messenger component on your website.
Since we don't have a website yet, we will test the virtual agent directly in the tool.

![Dialogflow Messenger Try Now](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/try-now.png?raw=true)

9. Click on the **Try Now** link.

10. Click on the bottom right chatbot icon, to open the chat window. Write `Hello` to kick-off the conversation.

![Dialogflow Messenger Try Now](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/df-messenger2.png?raw=true)

For now, when you click on the suggestion chips, the virtual agent won't understand what you mean. This is because our virtual agent is not switching between states yet. We can do this in Dialogflow CX with **Pages**. Let's continue the lab, we will first create some **Entities** and **Intents**.

## Entity Types

Duration: 5:00

[Entity types](https://cloud.google.com/dialogflow/cx/docs/concept/entity?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) are used to control how data from end-user input is extracted. Dialogflow CX entity types are very similar to Dialogflow ES entity types.
Dialogflow provides predefined system entities that can match many common types of data. For example, there are system entities for matching dates, times, colors, email addresses, and so on. 
You can also create your own custom entities for matching custom data.

Let's start by preparing all the custom entities before we can design the pages in a flow.
We will create the following entities:

![Dialogflow Entities](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/entities-uml2.png?raw=true)

### Creating Entities

Let's create an *Artist* entity.

1. Click **Manage > Entity Types**

2. Click **Create**

* Display Name: `Artist`
* Entities:
- `The Google Dolls` (with synonym: `Google Dolls`)
- `The Goo Fighters` (with synonym: `Goo Fighters`)
- `G's N' Roses` (with synonym: `Gs and Roses`)
- `Alice Googler`

* Click Advanced options and check **Fuzzy Matching**. (If you spell the band name wrong, it might still match it to the right entity.)
* In Advanced options also check **Redact in log**. (If you spell the band name wrong, it will correct the name in the log.)

3. Click **Save**

We will also need an entity for the *Merch* item:

4. Click **Manage > Entity Types**

5. Click **Create New**

* Display Name: `Merch`
* Entities:
- `T-shirt`
- `Longsleeve` (with synonym: `Longsleeve shirt`)
- `Tour Movie`
- `Digital Album` (with synonym: `MP3 Album`, 'MP3')
- `CD` (with synonyms `Disc`, `Physical CD`)

6. Click **Save**

We will also need an entity for the *Album*:

7. Click **Manage > Entity Types**

8. Click **Create New**

* Display Name: `Album`
* Entities:
- `Live`
- `Greatest Hits` (with synonym: `Hits`)

9. Click **Save**

We will also need an entity for clothing *sizes*:

10. Click **Manage > Entity Types**

11. Click **Create New**

* Display Name: `ShirtSize`
* Entities:
- `XS` (with synonym: `Extra Small`)
- `S` (with synonym: `Small`)
- `M` (with synonym: `Medium`)
- `L` (with synonym: `Large`)
- `XL` (with synonym: `Extra Large`)
- `2XL` (with synonym: `Extra Extra Large`)
- `3XL`

12. Click **Save**

And an entity for *order numbers*, that are typically 4 alphanumeric and 3 numbers. (like ABCD123)

13. Click **Manage > Entity Types**

14. Click **Create New**

* Display Name: `OrderNumber`
* Regexp entities
* Entity: [A-Z]{4}[0-9]{3}

15. Click **Save**



Your entity configuration should look similar to the following:

@Artist:
![@Artist Entity Type](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/artist-entity.png?raw=true)

@Merch:
![@Merch Entity Type](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/merch-entity.png?raw=true)

@Album:
![@Album Entity Type](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/album-entity.png?raw=true)

@ShirtSize:
![@ShirtSize Entity Type](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/shirtsize-entity.png?raw=true)

@OrderNumber:
![@OrderNumber Entity Type](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/ordernumber-entity.png?raw=true)


Once, the custom entities are prepared, we can prepare the intents. Let's continue the lab.

## Intents

Duration: 15:00

An [Intent](https://cloud.google.com/dialogflow/cx/docs/concept/intent?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) categorizes an end user’s intention for one conversation turn. They have been dramatically simplified in Dialogflow CX, It’s no longer a building block for conversational control. Dialogflow
CX only uses intents to match what the users are saying. In Dialogflow ES, you had to tie everything to an intent (parameters, events, fulfillment, etc.). Intents in Dialogflow CX only contain
training phrases and therefore are reusable. It no longer controls the conversation. So the process of creating intents will be straight forward:

The training phrases in intents can make use of **Entities** to extract 'variable' input, this is why it's a good practice, to create your entity types in advance,
which is what we did in the previous page of lab steps.

### Creating Intents

Let's start by preparing all the intents before we can design the pages in a flow.

1. Click the **Manage > Intents**.

2. Click on **+ Create**

Use the following details:

* Display name `redirect.artists.overview`
* Description `Artists overview: The bands supported by the label`

>**Note:** As a best practice, we will use the following naming convention for pages & intents:
all characters are lowercase, use dots (.) instead of spaces. Intents use the following prefixes:
**redirect** for intents that use NLU to fetch a page, **confirm**/**decline** for intents that confirm or decline choices ('yes' or 'no' training phrases)
and **supp** in case it's a supplemental question, which can come back at any moment in the flow.

![New Intent](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/new-intent.png?raw=true)

Scroll down and create the following **training phrases**:

* `Which bands are signed?`
* `Which bands`
* `Which artists`
* `Which artists are part of the record label`
* `Who is part of the label`
* `From which bands can I buy merchandise`
* `Band merchandise`
* `Which music do you have?`
* `I would like to know who are signed to the label`
* `Who are supported by the label`
* `From who can I buy shirts`*
* `What music can I order`
* `Can I get an overview of all the artists`

![Training phrases](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/trainingphrases.png?raw=true)

4. Click **Save**.

>**Note:** For what is worth, it is also possible to create intents directly from a page, in the **Routes** section.

5. Now let's continue and create all the other intents. Use your own imagination to come up with more training phrases.
A best practice would be to have at least 10 training phrases per intent to cover the different ways a user might trigger thant intent. For the purpose of this lab, having less should be fine too.

A couple of things to look for:
* Note that as you enter your training phrase, Dialogflow CX will automatically annotate your entities. If it doesn't do so, you may need to update your entity (by adding a synonym) or by manually annotating the training phrase yourself.
* Shorter training phrases: Dialogflow's NLU system can also work with shorter training phrases and we have provided a couple of examples here.
* Over-training: Too many training phrases for an intent may cause over-training and a less desirable result. It is best practice to use iterative and incremental testing and add in training phrases in the case that there isn't an intent matched.

| **Display name** | **Training phrases** |
|-|-|
| `redirect.product.overview` | `"Which products do you sell?", "What merchandise items do you have?", "What are you selling?", "What are the items?", "Which products?" "What merchandise?", "Please tell me what you have"` |
| `confirm.artists.overview` | `"Yeah, let me buy merchandise", "Yes, I want to purchase something", "Yes, I would like to order merchandise from Alice Googler"` *(Note: Alice Googler, should be recognized as an @Artist entity!)*, `"Ok, let's buy stuff."` |
| `redirect.price` | `"How much does a t-shirt cost?", "What's the price for the tour movie?", "The album is how much?", "I want to know the price of a longsleeve shirt", "What's the price difference?", "What does each product costs?", "What does it cost?", "What is the price?"` |
| `redirect.product` | `"Tour movie", "I am interested in a t-shirt", "Can I buy a digital album?", "I want the CD", "I want to buy something", "Can I purchase a record?", "I want to buy a t-shirt size M of The Google Dolls", "Can I purchase the Alice Googler digital album?"` |
| `redirect.product.of.artist` | `"Yeah, let's shop", "Give me merch of Alice Googler", "Shirts of The Google Dolls that would be nice.", "Yes", "I want The Goo Fighters stuff", "Yes, I want to order merchandise", "Yep, give me items of G's N' Roses", "Go for it", "Anything Alice Googler", "I am a G's N' Roses fan!", "Google Dolls", "Yes of The Google Dolls"` |
| `redirect.shirts` | `"Shirts", "I want to buy shirts", "I am interested in shirts", "I want a shirt", "Shirts of G's N' Roses please", "Give me shirts of the Google Dolls", "I want to buy shirts of Alice Googler"` |
| `redirect.music` | `"Music", "I want to buy music", "I am interested in music", "Give me music of the Goo Fighters", "Music of Goo Fighters please", "Interested in buying the Alice Googler album", "Purchase Alice Googler music"`|
| `redirect.album` | `"Hits", "Live Album", "I want the Greatest Hits Digital Album", "Give me the Greatest Hits CD", "Hits on MP3"`|
| `redirect.shirt.size` | `"XS", "I have M", "I want Large", "My size is 3XL", "Extra Large is the size"` |
| `redirect.my.order` | `"About my order", "I have a question about my order", "My order is ABCD123, I have a question about my order."` |
| `redirect.my.order.status` | `"My order is ABCD123, what is the status?", "What is the status of my order?", "What my order status?", "When will I receive my item?"` |
| `redirect.my.order.canceled` | `"I want to cancel my order", "I want to cancel order ABCD123", "Please cancel order ABCD123", "Undo my order", "Stop my order", "Cancel"` |
| `redirect.shipping.info` | `"How long will it take?", "How long is shipping?", "How long does shipping take?", "When will I receive it?"` |
| `redirect.refund.info` | `"I want a refund.", "Can I get a refund", "I want to return the CD", "I want to return my t-shirt"` |
| `redirect.swapping.info` | `"I want to swap my item", "Can I change my t-shirt for a larger size?", "Can I change my product?", "I want to swap the CD"` |
| `redirect.order.process` | `"I want to buy a t-shirt of the Google Dolls, size S", "Let me buy the digital CD of Alice Googler", "Get me the tour movie of G's N' Roses", "Buy a longsleeve shirt of The Goo Fighters", "Purchase the Alice Googler t-shirt", "Please order me the Google Dolls CD"` |
| `confirm.proceed.order` | `"Yes", "Yes, please continue", "Yes order", "I want to order", "Yeah", "Yep", "I confirm", "Agree", "Go ahead", "Order", "Buy it", "Purchase", "Okay"` |
| `decline.proceed.order` | `"No", "I rather not", "I don't want it anymore", "Don't order", "Stop", "Not anymore", "Nope", "Go back", "Reset", "Decline", "I don't need it"` |
| `redirect.home` | `"Go back", "Home", "Help", "What else can I ask", "Restart", "Can you tell me what I can order?", "What questions can I ask", "I need help", "Advice please", "Hi", "Hello", "Good day!"` |
| `redirect.end` | `"No that's it, goodbye", "Bye", "Cheers", "End", "That's it", "No more questions", "Exit", "Have a good day", "End Call", "Close"` |


Now that our reusable elements (flows, entities, and intents) are prepared, we can put this together by creating Pages and State Handlers.

## Pages & State Handlers

Duration: 30:00

A Dialogflow CX conversation (a session) can be described and visualized as a [finite state
machine](https://en.wikipedia.org/wiki/Finite-state_machine). Take a vending machine as an example, it could be modelled as a finite state machine. It has the following states: Waiting
for Coins, Select Candy, Give Candy and given a set of inputs, it moves between those states. For example, inserting a coin moves the vending machine from Waiting for coins to Select Candy. [**Pages**](https://cloud.google.com/dialogflow/cx/docs/concept/page?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) are how we can model these states for a Dialogflow CX virtual agent.

As an end user interacts with Dialogflow CX in a conversation, the conversation moves from page to page so at any given moment,
exactly one page is the current page, the current page is considered active, and also the
flow associated with that page is considered active.

For each [**Flow**](https://cloud.google.com/dialogflow/cx/docs/concept/flow?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-), you define many pages, where your combined pages can handle a complete conversation on the topic(s) the flow is designed for. Every flow has a special start page. When a flow initially becomes active, the start
page becomes the current page. For each conversational turn, the current page will
either stay the same or transition to another page. This concept will allow you to create larger agents with many pages and multiple
conversation turns.

Pages contain **fulfillments** (static entry dialogues and/or webhooks), **parameters**, and **state handlers**. Conversation control happens through state handlers, which
allows you to create various *transition routes* to transition to another Dialogflow CX
page, including making it conditional (for branching of conversations).


A conversation's state is controlled by handling tranisitions between pages with three different types of routes:
* *Intent routes*: When an intent should be matched (e.g. changing page based on what an end user says). (Blue lines in the visual diagram.)
* *Condition routes*: When a condition should be checked (e.g. changing page based on certain parameters stored in the session) (Orange lines in the visual diagram.)
* *Event handlers*: When a certain fallback event should be handled (e.g. handling no input, no match, in order to disambiguate the end user to either an intent or condition route) (Green lines in the visual diagram.)

The conversation utterances (i.e. the content or response back to the user) is defined by fulfillment, which can be either static or dynamic:
* *Static fulfillment*: When a static fulfillment response is provided
* *Dynamic fulfillment*: When a fulfillment webhook is called for dynamic responses

For our retail bot, we will create some **intent routes** and provide some *static entry fulfillment* responses, which will be presented to the user as soon as a page gets activated. Later, we will create **parameters** with **condition routes** to gather the information you will need to make an merchandise order.

>**Note:** In the previous steps you have created regular intents. Dialogflow CX also gives you the option to create [**Route Groups**](https://cloud.google.com/dialogflow/cx/docs/concept/handler#route-group). Route Groups makes sense for when you
are building a virtual agent that have many pages with a common set of intent, event or conditional routes. To make routes reusable, you can define route groups (see the **Manage** tab), a resource for the entire flow.
You can attach the route group to a flow or page. The routes within these groups may be called when the page is active.


### Page Intent Routes

#### Creating the pages in the Default Start Flow

Here's a flow chart of the default start flow:

![Catalog Connected Pages](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/flows-start.png?raw=true)

Let's click this together:

1. Click **Build > Default Start Flow**

2. Click the **Start Page**

3. Click the **+** icon next to **Routes**

4. Add **redirect.artists.overview**

5. Scroll down to *Transition*, and transition to the **Catalog** flow.

6. Hit **Save**

7. Repeat the above steps for `redirect.product.overview` and the other 11 rows from this table:

| **Page (In Flow)** | **Routes > Intent** | **Routes > Transition To** |
|-|-|-|
| Start | `Default Welcome Intent` | - |
| Start | `redirect.artists.overview` | Flow: Catalog |
| Start | `redirect.product.overview` | Flow: Catalog |
| Start | `redirect.shirts` | Flow: Catalog |
| Start | `redirect.music` | Flow: Catalog |
| Start | `redirect.product`| Flow: Catalog |
| Start | `redirect.product.of.artist` | Flow: Catalog |
| Start | `redirect.refund.info` | Flow: Customer Care |
| Start | `redirect.shipping.info` | Flow: Customer Care |
| Start | `redirect.swapping.info` | Flow: Customer Care |
| Start | `redirect.my.order` | Flow: My Order |
| Start | `redirect.my.order.cancel` | Flow: My Order |
| Start | `redirect.my.order.status` | Flow: My Order |
| Start | `redirect.end` | Page: End Session |

![Default Start Page Routes](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/routes-default-start2.png?raw=true)

The Default Start Flow will work like an option menu works when calling a call center.
However, in this virtual agent it is trained with Natural Language, with the training phrases in intents. Therefore the interaction is driven by conversation and not by DTMF options and is more natural and human-like.

![Default Start Page Routes](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/flows-visualization.png?raw=true)

>**Note:** You might have noticed the two Page Transition options: **End Session** and **End Flow**. End Session closes the full chat session (chat conversation or call), where End Flow, closes the flow and jumps back to the last active flow.
This is an important concept in Dialoglfow CX. You will need to 'close' your flows once you want to jump elsewhere, else you might end up in a nested conversation.
By default, Dialogflow CX will try to stick to a conversation flow until it reaches the end. If, while in conversation, you go to another flow it follows the pages until the end of the flow and then gets back to the last visited flow and goes on until it finishes that one too, and so on until the chat conversation was ended with "End Session". This allows you to build complex conversations with deviations.


#### Creating the pages in the Catalog Flow

The following chat transcript belongs to the Catalog flow:

```
> "Hi"
"Welcome, I am the virtual agent of G-Records, a fictional rock label.
You can order artists merchandise, ask questions about your order or shipping,
and I can tell you more which artists are currently signed with us. How can I help?"
> "Which bands are signed with this record label?"
"The following bands are signed with G-Records:
Alice Googler, G's N' Roses, The Goo Fighters and The Google Dolls.

From which of these artists would you like to order merchandise?"
> "Alice Googler"
"You want to rock with Alice Googler merchandise. Awesome!

We sell shirts, music or the tour movie.

Which merchandise item do you want?"
"(Suggestion chips: [Shirts] [Music] [Tour Movie])"
> "I would like to buy a Shirt"
"Do you want a longsleeve or a t-shirt?"
"(Suggestion chips: [T-shirt, Longsleeve, Price?])"
> "What's the price difference?"
"A t-shirt costs $25 and a longsleeve costs $30.

Do you want a longsleeve or a t-shirt?"
> "A t-shirt",
"What shirt size do you want?"
"(Suggestion chips: [XS, S, M, L, XL, 2XL, 3XL])"
> "M"
"A T-shirt of Alice Googler size: M costs $25. Shall I continue to order?"
```

The dialogue will be different when you choose *Music* or *Tour Movie*:
For Music the dialogue will look like this:

```
 > "Music"
"We have a Greatest Hits Album or the Live Album. Which one do you want?"
"(Suggestion chips: [Greatest Hits, Live, Price?])"
> "The Live Album"
"Do you want this album on CD or MP3?"
"(Suggestion chips: [CD, MP3])"
> "What's the price difference?"
"A CD costs $15. The digital album on MP3 costs $10.
Do you want this album on CD or MP3?"
> "Digital Album",
"The Digital Album: Alice Googler - Live costs $10. Shall I continue to order?"
```

For the Tour Movie the dialogue will look like this:

```
 > "Tour Movie"
"The Tour Movie of G's N' Roses costs $25. Shall I continue to order?
```
----

Here's a flow chart of all the pages within the Catalog flow:

![Catalog Connected Pages](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/catalog-pages.png?raw=true)

Notice the complexity of this flow:
* I could skip the *Which artists question* and immediately ask *"Which merchandise items are available"*.
* From the Default Start Flow, I could ask: "I want to buy The Google Dolls t-shirt", or "I want to buy something". Which means the virtual agent will ask follow up questions, to fill the slots for these required parameters. It jumps directly to the Product Page.
* The Price dialogue comes from the Price page that will be reused.
* Although the dialogue for the Tour Movie looks like it's the most simple dialogue, we will actually do something special with it. We will reuse this part of the dialogue, so end-users can also enter it directly for one of the other products, if they specialize all the information at once:

```
 > "I want The Goo Fighters longsleeve size S."
"The longsleeve of The Goo Fighters size S costs $30. Shall I continue to order?"
```

>**Note:** This use case shows how powerful Dialogflow CX really is! Of course you could build retail agents with [Dialogflow ES](https://cloud.google.com/dialogflow/es/docs?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-), but you can't reuse intents, and keeping context and parameters are limited, so you would need to address this by manually coding back-end fulfillments (and would require developers).
In Dialogflow CX, the conversational architect can model these complex flows as a state machine and reuse design elements such as intents.

Let's first start with connecting the pages.

1. Click **Build > Catalog**

2. Click the **Start Page**

3. Click the **+** icon next to **Routes**

4. Add **redirect.artists.overview**

5. Scroll down to *Transition*, select **Page** and choose: **+ new Page**

6. Use the page name: `Artist Overview` and hit **Save**

Now let's finish the rest of the flow:

7. The previous steps can be repeated with the following pages, intents and fulfillments.
Take over this table. *Page*, is the Page you will select in the flow, *Routes > Transition To* is the new flow or page you will create and transition to.

| **Page (In Flow)** | **Routes > Intent** | **Routes > Transition To** |
|-|-|-|
| Catalog Start | `redirect.artists.overview` | Artist Overview |
| Catalog Start | `redirect.product` | Product |
| Catalog Start | `redirect.product.overview` | Product Overview |
| Catalog Start | `redirect.product.of.artist` | Product Overview |
| Catalog Start | `redirect.shirts` | Shirts |
| Catalog Start | `redirect.music` | Music |
| Catalog Start | `redirect.end` | End Session |
| Catalog Start | `redirect.home` | End Flow |
| Artist Overview | `redirect.product.of.artist`|  Product Overview |

Now let's continue and add more static fulfillments.

12. In the Catalog flow, click on the **Artist Overview** page.

13. Click **Edit fulfillment** in the *Entry fulfillment* section.

14. Use the following static fulfillments (*Agent says*):
* `The following bands are signed with G-Records: Alice Googler, G's N' Roses, The Goo Fighters and The Google Dolls.`

15. Click **Save**

16. In the Catalog flow, click on the **Product Overview** page.

17. Click **Edit fulfillment** in the *Entry fulfillment* section.

18. Use the following static fulfillment (*Agent says*):
* `We sell shirts, music or the tour movie.`

19. Hit **Save**.

### Page Parameters

**Parameters** are used to capture and reference values that have been supplied by the end-user during a session.
Each parameter has a name and an entity type. `@Artist` and `@Merch` are the minimum parameters that we need to collect to make a merchandise order.
For T-shirts or Longsleeves, you also want to collect `@ShirtSize` and in case you want to order music, you will also need a `@Carrier` and `@Album` name.

Those parameters will need to be marked as *required*. And once it's required, you would want to provide custom prompts
to remember your end user, to provide the correct answers so these parameters can be collected.
There are a few mechanisms in Dialogflow CX that can help you with this.

For example, you could provide custom static fulfillment messages in the *Parameter* section. If the parameter is required, then these parameter fulfillments will be shown.
These response messages will be added to the **response queue**.
During an agent's turn, it is possible (and sometimes desirable) to call multiple fulfillments, each of which may generate a response message. Dialogflow maintains these responses in a **response queue**.
To read more about the page life cycle, and the order these fulfillments will be added to the response queue, read the [Dialogflow CX Page Docs](https://cloud.google.com/dialogflow/cx/docs/concept/page?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-).

#### Creating the parameters on the Artist Overview page

Let's define some page parameters:

1. In the **Catalog** Flow, click on the **Artist Overview** page.

2. Click the **+** in the **Parameters** block. Add the **artist** parameter:
* Display Name: `artist`
* Entity Type: `@Artist`
* Required: Check
* Redact in log: Check

3. Now we will add some custom parameter fulfillment messages. If the *artist* parameter haven't been collected by the virtual agent yet, the end user will get this agent response added to the response queue:

`From which of these artists would you like to order merchandise?`

4. Add a second dialogue option that provides rich suggestion chips. Click **Add dialogue option** and use this code (in [JSON](https://en.wikipedia.org/wiki/JSON)):

```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "The Google Dolls"
          },
          {
            "text": "The Goo Fighters"
          },
          {
            "text": "Alice Googler"
          },
          {
            "text": "G's N' Roses"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```

It is possible to handle different fallback fulfillment prompts based on the amount of tries your end-user tried to answer these. You would do this with **parameters event handlers**.
There are various built-in event handlers to choose from such as *Invalid Parameters*, *Utterances too long*, *No input*, *No input 1st try*, *2nd try*, or *No Match*.
The difference between no input and no match, is that with no input a user never provided an answer, where with no match, the user did provide an answer
but Dialogflow CX could not intent match this with a page.

5. Scroll down to the **Event Handlers** section.

6. Click **Add event handler** and select the event: `No-match default`

7. Use the following event static *text fulfillment*:

`I missed that. Please, specify the artist. You can choose between: Alice Googler, G's N' Roses, The Google Dolls or The Goo Fighters. Which artist do you want to buy merchandise from?`

8. Click **Save**

9. Click **Add event handler** ans select the event: `No-input default`

10. Use the following event static *text fulfillment*:

`I am sorry, I could understand the artist's name. You can choose between Alice Googler, G's N' Roses, The Google Dolls or The Goo Fighters. Which artist do you want to buy merchandise from?`

11. Click **Save**

### Page Condition Routes

Duration: 15:00

Parameters are very powerful in combination with **Page Conditional Routes**. When a condition evaluates to true, the associated page route will be called.
A condition could be, *A parameter equals a specific value*, or *A parameter can't be missing*, or *A form that has been completed*, and many more.
You can find more information on [Parameters](https://cloud.google.com/dialogflow/cx/docs/concept/parameter?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) and [Conditions](https://cloud.google.com/dialogflow/cx/docs/reference/condition?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) in the Dialogflow CX Documentation.

For our retail virtual agent, we will need to collect a sequence of parameters, hence we will need to create a condition, to check if a 'form' has been completed.
A form is a list of parameters that should be collected from the end-user for the page. The virtual agent interacts with the end-user for multiple conversation turns, until it has collected all of the required form parameters, which are also known as page parameters.

Dialogflow CX automatically sets parameter values provided by the end-user during form filling.
To check whether the current page's complete form is filled, use the following condition: `$page.params.status = "FINAL"`

#### Creating the conditional routes on the Artist Overview page

Let's create a conditional route, which will transition to the next page, once the artist is known:

1. In the **Artist Overview** page, click on the *+* icon in the **Routes** section.

2. Scroll down to the **Condition** section.

3. Select *At least one* (OR)

3. Next, we will write an expression that
* Parameter: `$page.params.status`
* Operator: `=`
* Value: `"FINAL"`

4. Now, we will create a specific static fulfillment message on the route. Confirming the end user's choice. Scroll down to the **Fulfillment** block and write the following fulfillment messages:
* `$session.params.artist, great choice! Rock on!`
* `You want to rock with $session.params.artist merchandise. Awesome!`

5. When the condition is true, you should transition to the *Product Overview* page. Scroll down to the **Transition** section and use the following page:
`Product Overview`

6. Hit **Save**.

![Parameters](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/parameters.png?raw=true)

#### Creating the routes on the Product Overview page

Now, that we know how you can create parameters and conditional routes, let's create more parameters for the following pages:

##### Product Overview

1. Create `artist` parameter in the *Product Overview* Page:

* Display Name: `artist`
* Entity Type: `@Artist`
* Required: Check
* Redact in log: Check
* Initial prompt fulfillment: `From which of these artists would you like to order merchandise?`
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "The Google Dolls"
          },
          {
            "text": "The Goo Fighters"
          },
          {
            "text": "Alice Googler"
          },
          {
            "text": "G's N' Roses"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```
* Event Handler > `No-match default`:
`To buy merchandise you can choose between the following artists: Alice Googler, G's N' Roses, The Google Dolls or The Goo Fighters. Which artist do you want to buy merchandise from?`
* Custom payload:
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "The Google Dolls"
          },
          {
            "text": "The Goo Fighters"
          },
          {
            "text": "Alice Googler"
          },
          {
            "text": "G's N' Roses"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```
* Event Handler > `No-input default`:
`To buy merchandise you can choose between the following artists: Alice Googler, G's N' Roses, The Google Dolls or The Goo Fighters. Which artist were you trying to mention?`
* Custom payload:
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "The Google Dolls"
          },
          {
            "text": "The Goo Fighters"
          },
          {
            "text": "Alice Googler"
          },
          {
            "text": "G's N' Roses"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```

2. Create `merch` parameter:

* Display Name: `merch`
* Entity Type: `@Merch`
* Required: Check
* Fulfillment: `Which merchandise item do you want?`
* Click: **Add dialogue option > Custom payload**:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Shirts"
            },
            {
              "text": "Music"
            },
            {
              "text": "Tour movie"
            }
          ]
        }
      ]
    ]
  }
```
* Event Handler > `No-match default`
* Event Handler Fulfillment: `We sell Shirts, Music or the Tour movie. Which of these items do you want?`
* Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Shirts"
            },
            {
              "text": "Music"
            },
            {
              "text": "Tour movie"
            }
          ]
        }
      ]
    ]
  }
```

* Event Handler > `No-input default`
* Event Handler Fulfillment: `I couldn't understand which merchandise item you wanted to buy. You can choose between: Shirts, Music or the Tour movie. Which item do you want?`
* Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Shirts"
            },
            {
              "text": "Music"
            },
            {
              "text": "Tour movie"
            }
          ]
        }
      ]
    ]
  }
```

3. Create a route which will transition to the **Product** page for when `artist` is provided and the `merch` item is provided.

* Condition:
* Match Every rule (AND)
* Expression: `$session.params.artist != null`
* Expression: `$session.params.merch != null`
* Fulfillment: `Alright! $session.params.merch of $session.params.artist, let's go!`
* Transition: Create new Page: `Product`

>**Note:** This time we fetch the parameters from the session instead of the page. That is because you could have provided those parameters on another page.
In our case, we mentioned the artist's name in the turn before.

4. Create a route for when the user says "Shirts"

* Intent: **redirect.shirts**
* Transition: Create new Page: `Shirts`

5.  Create a route for when the user says "Music"

* Intent: **redirect.music**
* Transition: Create new Page: `Music`

6.  Create a route for when the user ask for price information

* Intent: **redirect.price**
* Transition: Create new Page: `Price`

When you have set the above configuration, you will see a visualization that's almost similar to the picture below. Note that intent routes
are blue in the diagram, and condition routes are orange. FWW, event handlers are green, and when multiple route types transition to a page, the line will be grey.

![The start of the Catalog flow](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/catalog-flow1.png?raw=true)

By now, you have learned how to create **Flows**, **Entities**, **Intents**, and **Pages** with **State Handlers** like: **Intent Routes** and **Conditional Routes** based on **Parameters**.
Later in this lab, we will use conditional branching in the fulfillment, to provide different dialogues based on the input.

You can use the following configurations, to finalize our virtual agent.

#### Shirts Page:

1. Create the following configurations in the *Shirts* Page:

* Entry fulfillment: `Do you want a longsleeve or a t-shirt?`
* Entry fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "T-shirt"
            },
            {
              "text": "Longsleeve"
            },
            {
              "text": "Price?"
            }
          ]
        }
      ]
    ]
  }
```

2. Create an **Intent Route**: `redirect.price` with a transition to the `Price` Page

3. Create the following parameter:

* Parameter: `merch` - Entity Type: `@Merch`, `Required` and `Redact in log`
* Parameter > Event Handler > `No-match default`
* Parameter > Event Handler Fulfillment: `You can choose between a t-shirt or a longsleeve. Which of these do you want?`
* Parameter > Event Handler Fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "T-shirt"
            },
            {
              "text": "Longsleeve"
            }
          ]
        }
      ]
    ]
  }
```
* Parameter > Event Handler > `No-input default`
* Parameter > Event Handler Fulfillment: `I couldn't understand if you want the t-shirt or the longsleeve. Which of these do you want?`
* Parameter > Event Handler Fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "T-shirt"
            },
            {
              "text": "Longsleeve"
            }
          ]
        }
      ]
    ]
  }
```

4. Click on the entry fulfillment and scroll down to **Parameter presets**, each time when the Shirts page gets active, the category parameter will be set to *shirts*:

| **Parameter** | **Value** |
|-|-|
| `category` | `shirts` |


5. Add a Conditional Route:

* Match Every rule (OR)
* Expression: `$session.params.merch = "T-shirt"`
* Expression: `$session.params.merch = "Longsleeve"`
* Transition to new page: `Shirt Size`

#### Price Page:

Since the messages for prices will be depending on the choosen merchandise item or category (music or shirts), we will fix this part later in the lab.
Just entering a placeholder is more than enough for now.

1. Create the following configurations in the *Price* Page:

* Entry fulfillment: `PRICE TODO`

Since you can request the price from various places in the conversation, it should
always give you an answer and transfer you back to the previous part of the dialogue, to continue the order.
There are 5 places in the dialogue tree where you can branch of to get price information.
(Shirt, Shirt Size, Music, Carrier and also direct via an Intent Route), thus we will need some conditional routes to move back:

2. Add a conditional Route:

* Match Every rule (AND)
* Expression: `$session.params.category = "shirts"`
* Expression: `$session.params.merch = "null"`
* Transition to new page: `Shirts`

3. Add a Conditional Route:

* Match Every rule (AND)
* Expression: `$session.params.category = "shirts"`
* Expression: `$session.params.size = "null"`
* Transition to new page: `Shirt Size`

4. Add a Conditional Route:

* Match Every rule (AND)
* Expression: `$session.params.category = "music"`
* Expression: `$session.params.album = "null"`
* Transition to new page: `Music`

5. Add a Conditional Route:

* Match Every rule (AND)
* Expression: `$session.params.category = "music"`
* Expression: `$session.params.merch = "null"`
* Transition to new page: `Carrier`

6. Add a Conditional Route:

* Match Every rule (AND)
* Expression: `$session.params.category = "null"`
* Transition to new page: `Product Overview`


#### Shirt Size Page:

1. Create the following configurations in the *Shirt Size* Page:

* Entry fulfillment: `What shirt size do you want?`
* Entry fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "XS"
            },
            {
              "text": "S"
            },
            {
              "text": "M"
            },
            {
              "text": "L"
            },
            {
              "text": "XL"
            },
            {
              "text": "2XL"
            },
            {
              "text": "3XL"
            }
          ]
        }
      ]
    ]
  }
```
2. Create an **Intent route**: `redirect.price` with a transition to the `Price` Page.

3. Create the following parameter:

* Parameter: `shirtsize` - Entity Type: `@ShirtSize` - `Required`, `Redact In Log`
* Parameter > Event Handler > `No-match default`
* Parameter > Event Handler Fulfillment: `Please tell me the shirt size, such as XL.`
* Parameter > Event Handler Fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "XS"
            },
            {
              "text": "S"
            },
            {
              "text": "M"
            },
            {
              "text": "L"
            },
            {
              "text": "XL"
            },
            {
              "text": "2XL"
            },
            {
              "text": "3XL"
            }
          ]
        }
      ]
    ]
  }
```
* Parameter > Event Handler > `No-input default`
* Parameter > Event Handler Fulfillment: `I couldn't understand the shirt size. What size do you want?`
* Parameter > Event Handler Fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "XS"
            },
            {
              "text": "S"
            },
            {
              "text": "M"
            },
            {
              "text": "L"
            },
            {
              "text": "XL"
            },
            {
              "text": "2XL"
            },
            {
              "text": "3XL"
            }
          ]
        }
      ]
    ]
  }
```

4. Add a Conditional Route:

* Match Every rule (OR)
* Expression: `$page.params.shirtsize != "null"`
* Transition to Page: `Product`

#### Music Page:

1. Create the following configurations in the *Music* Page:

* Entry fulfillment: `We have a Greatest Hits Album or the Live Album. Which one do you want?`
* Entry fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Greatest Hits"
            },
            {
              "text": "Live"
            },
            {
              "text": "Price?"
            }
          ]
        }
      ]
    ]
  }
```

2. Create an **Intent Route**: `redirect.price` with a transition to Page: `Price`.

3. Create the following parameter:

* Parameter: `album` - Entity Type: `@Album` - `Required`, `Redact In Log`
* Parameter > Event Handler > `No-match default`
* Parameter > Event Handler Fulfillment: `You can choose between Greatest Hits and Live Album. Which of these do you want?`
* Parameter > Event Handler Fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Greatest Hits"
            },
            {
              "text": "Live"
            }
          ]
        }
      ]
    ]
  }
```
* Parameter > Event Handler > `No-input default`
* Parameter > Event Handler Fulfillment: `I couldn't understand if you want the album: Greatest Hit or Live.  Which of these do you want?`
* Parameter > Event Handler Fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Greatest Hits"
            },
            {
              "text": "Live"
            }
          ]
        }
      ]
    ]
  }
```

4. Click on the entry fulfillment and scroll down to **Parameter presets**, each time when the Music page gets active, the category parameter will be set to *music*:

| **Parameter** | **Value** |
|-|-|
| `category` | `music` |


4. Add a Conditional Route:

* Match Every rule (OR)
* Expression: `$page.params.album != "null"`
* Transition to new Page: `Carrier`

#### Carrier Page:

1. Create the following configurations in the *Carrier* Page:

* Entry fulfillment: `Do you want this album on CD or MP3?`
* Entry fulfillment Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "CD"
            },
            {
              "text": "MP3"
            },
            {
              "text": "Price?"
            }
          ]
        }
      ]
    ]
  }
```
2. Create an **Intent route**: `redirect.price` which transitions to the `Price` Page.

3. Create the following parameter:

* Parameter: `merch` - Entity Type: `@Merch` - `Required`, `Redact In Log`
* Parameter > Event Handler > `No-match default`
* Parameter > Event Handler Fulfillment: `Do you want a physical CD or the digital album?`
* Parameter > Event Handler Fulfillment: Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "CD"
            },
            {
              "text": "Digital Album"
            }
          ]
        }
      ]
    ]
  }
```

* Parameter > Event Handler > `No-input default`
* Parameter > Event Handler Fulfillment: `I couldn't understand if you mean CD or MP3.  Which one do you want?`
* Parameter > Event Handler Fulfillment: Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "CD"
            },
            {
              "text": "MP3"
            }
          ]
        }
      ]
    ]
  }
```

4. Add a Conditional Route:

* Match Every rule (OR)
* Expression: `$page.params.merch != "null"`
* Transition to Page: `Product`


#### Product Page:

1. Create the following parameters:

| **Parameter Displayname** | **Parameter Entity Type** | **Checks** |
|-|-|-|
| `artist` | `@Artist` | Required, Redact in log |
| `merch` | `@Merch` | Required, Redact in log |


2. The *artist* parameter needs the following initial prompt fulfillment, which will be shown when the artist isn't known.
`You didn't mention which artist you are interested in. You can ask me to buy the $session.params.merch of the artist you like or ask which artists we signed. How can I help?`
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "Which artists?"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```

* Also add a `No-input default` event handler with fulfillment: `I couldn't understand what you just said. Ask me which artists are signed.`
* And a `No-match default` event handler with fulfillment: `I missed that. Please ask me which artists are signed.`

3. The *merch* parameter needs reprompt event handlers as well.
* Add a `No-input default` event handler with fulfillment: `I couldn't understand what you just said. Which merchandise item do you want?`
* And a `No-match default` event handler with fulfillment: `I missed that. Which merchandise item do you want?`

The next route will transition to the confirmation page when the artist is known and the user chooses a "Tour Movie".

4. Add a Conditional Route:

* Match Every rule (AND)
* Expression: `$session.params.artist != null`
* Expression: `$session.params.merch = "Tour Movie"`
* Parameter Presets Add Parameter > `price = 25`
* Transition to new page: `Confirmation`

The next route will transition to the confirmation page when the artist is known and the user chooses a "T-shirt" and the shirt size is chosen.

5. Add a Conditional Route:

* Custom Expression: `$session.params.artist != null AND $session.params.merch = "T-shirt" AND $session.params.shirtsize != null`
* Parameter Presets Add Parameter > `price = 25`
* Transition to page: `Confirmation`

The next route will transition to the confirmation page when the artist is known and the user chooses a "Longsleeve" and the shirt size is chosen.

6. Add a Conditional Route:

* Custom Expression: `$session.params.artist != null AND $session.params.merch = "Longsleeve" AND $session.params.shirtsize != null`
* Parameter Presets Add Parameter > `price = 30`
* Transition to page: `Confirmation`

The next route will transition to the confirmation page when the artist is known and the user chooses a "CD" also the album name is chosen.

7. Add a Conditional Route:

* Custom Expression: `$session.params.artist != null AND $session.params.merch = "CD" AND $session.params.album != null`
* Parameter Presets Add Parameter > `price = 15`
* Transition to page: `Confirmation`

The next route will transition to the confirmation page when the artist is known and the user chooses a "Digital Album" and the album name is chosen.

8. Add a Conditional Route:

* Custom Expression: `$session.params.artist != null AND $session.params.merch = "Digital Album" AND $session.params.album != null`
* Parameter Presets Add Parameter > `price = 10`
* Transition to page: `Confirmation`

Next, we will now make some advanced conditionals with prompts that detect missing information.
The next route will transition back to the music page when the artist is known and the user chooses a "CD" or a "Digital Album" but the album name was not chosen.

9. Add a Conditional Route:

* Match Every rule (AND)
* Custom Expression: `$session.params.artist != null AND ($session.params.merch = "CD" OR $session.params.merch = "Digital Album") AND $session.params.album = null`
* Fulfillment: `I would also need to know which album you would like to buy!`
* Transition to page: `Music`

And the last route will transition to the confirmation page when the artist is known and the user choose a "T-shirt" or a "Longsleeve", and also the t-shirt size was not chosen.

10. Add a conditional Route:

* Custom Expression: `$session.params.artist != null AND ($session.params.merch = "T-shirt" OR $session.params.merch = "Longsleeve") AND $session.params.shirtsize = null`
* Fulfillment: `I would also need to know which shirt size you need!`
* Transition to page: `Shirt Size`

In the next part of the lab, we will make use of conditional fulfillments to give different fulfillment messages depending on the input.

## Conditional Responses

Duration: 10:00

Some responses will return a different dialogue based on the input, The dialogues will branch off, we call this *conditional responses*.
This can become interesting, when you are not making use of webhook fulfillments, where the conditional responses were determined on the back-end.
An example could look like:

```
if [condition]
  [response]
elif [condition]
  [response]
elif [condition]
  [response]
else
  [response]
endif
```

* An example of a [condition] could be: `$session.params.user-age >= 21`. It uses a similar formatting as the conditions in the routes.
* An [response] takes the static text response
* Conditional responses always start with `if`
* `elif` and `else` blocks are optional

Dialogflow CX can also make use of built-in [system functions](https://cloud.google.com/dialogflow/cx/docs/reference/system-functions?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) to make use of.
For example to format a date or time, or to display the current time (`$sys.func.NOW()`)

Let's finalize the *Catalog* flow, by fixing the *Confirmation* and *Price* Pages.

### Confirmation Page:

Now we will build the confirmation page.
It has the following requirements:

* If *merch* is *CD* or *Digital Album*. We will show the following fields in the confirmation: *artist*, *merch*, *album* and *price*.
* If *merch* is *T-shirt* or *Longsleeve*. We will show the following fields in the confirmation: *artist*, *merch*, *size* and *price*.
* Else (and thus if *merch* is *Tour Movie*). We will show the following fields in the confirmation: *artist*, *merch* and *price*.

1. Click on the *Confirmation* Page.

2. Click on **Add dialogue** option > **Conditional Response**:

```
if ($session.params.merch = "CD" OR $session.params.merch = "Digital Album")
  The $session.params.merch: $session.params.artist - $session.params.album costs $$session.params.price. Shall I continue to order?
elif ($session.params.merch = "T-shirt" OR $session.params.merch = "Longsleeve")
  A $session.params.merch of $session.params.artist size: $session.params.shirtsize costs $$session.params.price. Shall I continue to order?
elif $session.params.merch = "Tour Movie"
  The $session.params.merch of $session.params.artist costs $$session.params.price. Shall I continue to order?
else
  It looks like something went wrong with your order. You can say "Reset", to restart the order process.
endif
```

3. Create the following Custom payload:
* Custom payload:
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "Yes, confirm"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```

Next, create two intent routes:

3. `confirm.proceed.order` transitions to: `Order Process` Flow.

4. `decline.proceed.order` transitions to `End Flow`

When the user declines the order, and does not want to proceed the order process, we will have to transition back to the welcome page,
but all the parameters have to be cleared. We can do this by specifically setting null to all the possible parameters.
You can do this with **Parameter presets**.

5. In the **decline.proceed.order** intent, scroll down to **Parameter presets** and add the following parameters:

| **Parameter** | **Value** |
|-|-|
| `artist` |  `null` |
| `merch` | `null` |
| `shirtsize` | `null` |
| `category` | `null` |
| `album` | `null` |
| `price` | `null` |
| `restart` | `true` |

Notice that we have created an additional parameter called `restart`. If this parameter is present, the Default Start Flow, should know to continue the conversation by showing a customized message.

6. Click on the **Default Start Flow**, **Start** Page, and create another *Conditional Route*:
* `$session.params.restart = "true"`
* Fulfillment: `"Welcome back, as the virtual agent of G-Records, I can help you order artists merchandise, you can ask questions about your order or shipping, and I can tell you more which artists are currently signed with us. How can I help?"`
* Custom payload:
```
{
    "richContent": [
      [
        {
          "type": "chips",
          "options": [
            {
              "text": "Which artists?"
            },
            {
              "text": "Which products?"
            },
            {
              "text": "About my order..."
            }
          ]
        }
      ]
    ]
  }
```

7. Select the **Start** Page and click on the `redirect.end` intent. Create the following fulfillment:
`Thank you for contacting G-Records! Have a nice day!`

### Price Page:

Let's also fix the Price TODOs. The price information will be static for now.
Click on the **Price** Page in the *Catalog* Flow, and use the following entry fulfillment:

* Delete the Agent Says entry fulfillment.
* Create a new Conditional Response:

```
if $session.params.category = "shirts"
  A t-shirt costs $25 and a longsleeve costs $30.
elif $session.params.category = "music"
  A CD costs $15. The digital album on MP3 costs $10.
else
  A t-shirt costs $25 and a longsleeve costs $30. A CD costs $15 and a digital album on MP3 $10. In case you are interested in the Tour Movie, that one is $25.
endif
```

![Conditional Responses](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/conditional-responses.png?raw=true)

Well done, by now you completed the **Catalog** flow. Your flow should look similar to this diagram:

## Wrapping up the agent

Duration: 20:00

We are almost at the end of this lab. Let's configure the last flows together, and take in practice all the new things that we have learned.

### Creating the My Order Flow

1. Go to the **My Order** Flow, and create the following intent transitions:

| **Page (In Flow)** | **Routes > Intent** | **Routes > Transition To** |
|-|-|-|
| My Order Start | `redirect.my.order` | My Order |
| My Order Start | `redirect.my.order.status` | My Order Status |
| My Order Start | `redirect.my.order.canceled` | My Order Cancellation |
| My Order Start | `redirect.end` | End Session |
| My Order Start | `redirect.home` | End Flow |
| Default Start Flow | `redirect.my.order.canceled` | Flow: My Order |
| Default Start Flow | `redirect.my.order.status` | Flow: My Order |

2. Let's create the following entry fulfillment for the **My Order** Page:
* Entry fulfillment:
`I can look up the status of your order, or I can cancel an order.`

3. In the **My Order** Page create the following parameter:
* Displayname: `ordernumber`
* Entity Type: `@OrderNumber`
* Required: **checked**
* Initial prompt fulfillment:  `What's the order number? For example ABCD123.`
* Event Handler: `No-match default`: `To proceed with your order I will need an order number. Order numbers start with 4 characters and end with 3 numbers, such as ABCD123. Which order number may I use?`
* Event Handler: `No-input default`: `I missed that. To proceed with your order I will need an order number. Order numbers start with 4 characters and end with 3 numbers, such as ABCD123. Which order number may I use?`

4. Create the following conditional route:
* Customize Expression: `$page.params.status = "FINAL"`
* Fulfillment: `And do you want to Cancel your order, or should I look up the status?`

5. Click on **Add state handler > Event Handlers** and create the Event Handler: `No-input default`
* Fulfillment:  `I'm sorry, what was that? Would you like me to cancel an order or look up the status?`
* Custom payload:
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "Status"
          },
          {
            "text": "Cancel"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```

6. Create the Event Handler: `No-match default`
* Fulfillment:  `Would you like me to cancel an order or lookup the status?`
* Custom payload:
```
{
  "richContent": [
    [
      {
        "options": [
          {
            "text": "Status"
          },
          {
            "text": "Cancel"
          }
        ],
        "type": "chips"
      }
    ]
  ]
}
```

7. In the **My Order Status** Page create the following parameter:
* Displayname: `ordernumber`
* Entity Type: `@OrderNumber`
* Required checked
* Initial prompt fulfillment:  `What's the order number? For example ABCD123.`
* Event Handler: `No-match default`: `To proceed with your order I will need an order number. Order numbers start with 4 characters and end with 3 numbers, such as ABCD123. Which order number may I use?`
* Event Handler: `No-input default`: `I missed that. To proceed with your order I will need an order number. Order numbers start with 4 characters and end with 3 numbers, such as ABCD123. Which order number may I use?`

8. In the **My Order Status** Page create the following conditional route:
* Customize Expression: `$session.params.ordernumber != null`
* Fulfillment: `Your order $session.params.ordernumber has been shipped, it can take up to approx 2 weeks before you will receive your items.`
* Add dialogue option > Text: `Is there anything else I can help you with?`

9. In the **My Order Cancelation** Page create the following parameter:
* Displayname: `ordernumber`
* Entity Type: `@OrderNumber`
* Required checked
* Initial prompt fulfillment:  `What's the order number? For example ABCD123.`
* Event Handler: `No-match default`: `To proceed with your order I will need an order number. Order numbers start with 4 characters and end with 3 numbers, such as ABCD123. Which order number may I use?`
* Event Handler: `No-input default`: `I missed that. To proceed with your order I will need an order number. Order numbers start with 4 characters and end with 3 numbers, such as ABCD123. Which order number may I use?`

10. In the **My Order Cancelation** Page create the following conditional route:
* Customize Expression: `$session.params.ordernumber != null`
* Fulfillment: `Your order $session.params.ordernumber has been canceled.`
* Add dialogue option > Text: `Is there anything else I can help you with?`

11. Test the flow and create the following two test scenarios:

```
>"About my order"
>"ABCD123"
>"Status"
```

And:

```
>"What's the status of order DEFG222"
```

12. Select the **Start** Page and click on the `redirect.end` intent. Create the following fulfillment:
`Thank you for contacting G-Records! Have a nice day!`

13. Select the **Start** Page and click on the `redirect.home` intent. Create the following parameter preset:
`restart = true`

### Default Negative intents (Fallback)

When you create a virtual agent, a default negative intent is created for you. You can add training phrases to this intent that act as negative examples that will trigger a **No-match event**.
There may be cases where end-user input has a slight resemblance to training phrases in normal intents, but you do not want these inputs to match any normal intents.

1. Try in the simulator: `I don't like Alice Googler`.

You will see that the virtual agent answers with the *Product Overview* Page, to continue ordering *Alice Googler* merchandise.
However, your end user does not like that artist.
Let's use the Default Negative Intent for this.

2. Go to **Manage > Intents** and select the **Default Negative Intent**.

3. Add the following training phrases that will trigger the *No-match* event.

* `I don't like Alice Googler`
* `I am not a fan of G's N' Roses`
* `I can't stand the music of the Google Dolls`

4. Hit **Save** and test the following sentence in the simulator:
`I am really not a fan of the Goo Fighters`

This time the `No-match` event was triggered, you stayed on the **Start** Page.

### Default Fallback Messages

1. Click the **Default Start Flow**, select the `sys.no-input-default` event handler.

The **No-input** fallback basically means: No text or speech answers were detected. Likely no answers were given, or the system couldn't hear it.
Therefore, let's make the fallback messages more specific. Use the *tab* key, to create alternative dialogues:

2. Remove all answers, and add these text dialogues:

* `I'm sorry, I didn't receive an answer. Can you say it again?`
* `I missed your answer, can you say it again?`
* `Sorry, I didn't hear anything. Can you say it again?`
* `I couldn't hear what you were saying, what was that?`
* `I'm sorry, I missed your answer. What were you trying to say?`

Don't forget to click **Save**.

3. Click the **Default Start Flow**, select the `sys.no-match-default` event handler.

The No Match fallback basically means: Text or speech answers were detected but nothing in Dialogflow CX got matched.

4. Remove all answers, and add these text dialogues:

* `Sorry, I didn't get that. Can you please rephrase?`
* `I'm sorry, I don't understand. Can you please rephrase?`
* `I don't understand, please rephrase.`
* `Sorry, I didn't get that. What was that?`
* `I didn't get that, can you please rephrase?`

Don't forget to click **Save**.

5. Repeat these steps for the **Catalog**, **My Order**, **Order Process** and **Customer Care** flows.

When creating fallback messages, you actually would like to make it more explicit, by rephrasing the previous question or by mentioning an example.
You could create these type of *No-match* and *No-input* events on Page level when creating parameters. In our labs, we have already done this.

### Creating the Order Process Flow

1. Go to the **Order Process** Flow, and create the following intent transitions:

| **Page (In Flow)** | **Routes > Intent** | **Routes > Transition To** |
|-|-|-|
| Order Process Start | `redirect.end` | End Session |
| Order Process Start | `redirect.home` | End Flow |
| Order Process Start | `confirm.proceed.order` | New Page: Shipping Details |

2. Let's create the following entry fulfillment for the **Shipping Details** Page:
* Entry fulfillment:
`To complete your order I will first need to collect your shipping details.`

3. Create the following parameters:

These parameters will make use of built-in system entities.
System entity support differs for each language. See the [docs](https://cloud.google.com/dialogflow/cx/docs/reference/system-entities?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-) for more information.

| **Parameter Display name** | **Entity** | **Required?** | **Initial prompt fulfillment** | **No-match default** | **No-input default** |
|-|-|-|-|-|-|
| `firstname` | @sys.person | Required | `What's your first name?` | `I'm sorry I missed that. What's the first name?` | `I'm sorry, I didn't understand. What's the first name?`|
| `lastname` | @sys.person | Required | `What's your last name?` | `I'm sorry I missed that. What's the last name?` | `I'm sorry, I didn't understand. What's the last name?`|
| `address` | @sys.address | Required | `What's your address?` | `I missed that. What's the address?` | `I'm sorry, I didn't understand. What's the address?`|
| `zipcode` | @sys.any| Required | `What postal code or zipcode do you have?` | `I'm sorry, what's the zip or postal code? For example: 1234AB or 10001.` | `I'm sorry, I didn't understand. What's the zip or postal code? For example: 1234AB or 10001.`|
| `city` | @sys.geo-city | Required | `What's the name of the city?` | `I missed that, what's the name of the city?` | `I'm sorry, I didn't understand. What's the name of the city?`|
| `country` | @sys.geo-country | Required | `What's the name of the country?` | `I missed that, what's the name of the country?` | `I'm sorry, I didn't understand. What's the name of the country?`|
| `email` | @sys.email | Required | `Lastly, what's your email address?` | `I am sorry. What's the email address? For example name@domain.com.` | `I am sorry, I didn't understand. What's the email address? For example name@domain.com.` |

4. Create the following conditional route:
* Customize Expression: `$page.params.status = "FINAL"`
* Transition to new Page: `Payment Details`

5. Create the following entry fulfillment.

Let's fake it that this virtual agent makes use of Google Pay. Don't worry this tutorial won't make real transactions.
Create the following entry dialogues:

* Agent Says:
```
Alright $session.params.firstname! We will make use of Google Pay, that's connected to your email account: $session.params.email.
```

* Conditional Response
```
if $session.params.merch != "Digital Album"
  Shipping costs an additional 5 dollars. This will make the total price $$sys.func.TO_TEXT($sys.func.ADD($session.params.price, 5)).
  Your merchandise will be shipped to:
  $session.params.firstname $session.params.lastname
  $session.params.address
  $session.params.zipcode $session.params.city
  $session.params.country
  To continue the order process please explicitly say "I confirm". Do you want to confirm your $session.params.artist $session.params.merch order?
else
  The total costs will be: $$session.params.price.
  After purchasing the digital album, you will receive an email with the download link.
  To continue the order process please explicitly say "I confirm".
  Do you want to confirm your $session.params.artist $session.params.merch order?
endif
```

6. Create the following **Intent Route**
* Intent: `confirm.proceed.order`
* Agent Says: 'Thank you for your order! Your merchandise will be shipped today!`
* Add Dialogue Option > Text: `Here's the order number: ABCD123`.
* Add Dialogue Option > Text: `Have a good day!`
* Transition: `End Session`

7. Select the **Start** Page and click on the `redirect.end` intent. Create the following fulfillment:
`Thank you for contacting G-Records! Have a nice day!`

8. Select the **Start** Page and click on the `redirect.home` intent. Create the following parameter preset:
`restart = true`

Awesome! By now we have a fully working realworld retailer chatbot!
In the next lab, we will test how well the virtual agent performs!

## Test your virtual agent

Duration: 5:00

You can use the built-in simulator to test the dialogues of your virtual agent. The advantage of testing the flows in the simulator is that you will see a nice overview of flows, pages, parameters, and (DTMF) events that the
simulator collected while walking through your flows. This makes testing easier than testing it directly in an integration, as those types of information will be hidden from the end user.
It's even possible to create test cases, save and reuse those test cases. This makes a lot of sense, for when you maintain or edit your flows over time, and you want to be sure that none of your changes break your previous work.

It is also possible to export and import previously made test cases, by storing the tests in Google Cloud Storage or local. Exporting a test will download a blob file.
To learn more about the simulator and test cases check out the [Simulator / Test Cases Docs](https://cloud.google.com/dialogflow/cx/docs/concept/test-case?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-).

Before creating some test cases, let's first finalize the rest of our virtual agent:

### Creating the Customer Care Flow

1. Go to the **Customer Care** Flow, and create the following intent transitions:

| **Page (In Flow)** | **Routes > Intent** | **Routes > Transition To** |
|-|-|-|
| Customer Care Start | `redirect.shipping.info` | Shipping
| Customer Care Start | `redirect.refund.info` | Refund
| Customer Care Start | `redirect.swapping.info` | Swapping
| Customer Care Start | `redirect.home` | End Flow
| Customer Care Start | `redirect.end` | End Session

![Customer Care Flow](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/customarecare-flow.png?raw=true)

2. Create the following entry fulfillments for the **Shipping** Page:

* `Shipping physical merchandise items can take up to 2 weeks.`
* `Is there anything else I can help you with?`

3. Create the following entry fulfillments for the **Refund** Page:

* `We offer free returns and refunds. We provide one free return label for each order. You can use it within 30 days from receiving your order. If your refund is accepted, we will refund the price you paid for your item back to your original payment method.`
* `Is there anything else I can help you with?`

4. Create the following entry fulfillments for the **Swapping** Page:

* `If you would like to change your item for a different one, please return your unwanted item and place a new order. If your refund is accepted, we will refund the price you paid for your item back to your original payment method.`
* `Is there anything else I can help you with?`

5. Select the **Start** Page and click on the `redirect.end` intent. Create the following fulfillment:
`Thank you for contacting G-Records! Have a nice day!`

6. Select the **Start** Page and click on the `redirect.home` intent. Create the following parameter preset:
`restart = true`

### Create test cases

1. Click the **Test Agent** button on the right side of the screen.

When you first open the simulator, you need to select an agent environment and active flow. In most cases, you should use the draft environment and default start flow.

2. Type: `Hi`

![Customer Care Flow](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/simulator3.png?raw=true)

3. Ask: `Which artists are signed with your label?`

4. Say: `The Google Dolls`

5. Say: `I am interested in buying a shirt`

6. Say: `A t-shirt`

7. Say: `Medium`

8. Now click on the save test case button. Which you can find in the top of the simulator (next to the redo arrow, and reset trash bin icon)

![Customer Care Flow](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/simulator-save.png?raw=true)

9.  Give it the following details:
* Test case name: `Buy Google Dolls t-shirt size M`
* Tags: #catalog, #shirts, #t-shirt, #TheGoogleDolls

10. Click **Save**

Let's create more test cases.

11. First clear the current dialogue, by clicking on the Reset (thrash bin) icon.

12. Create the following test cases:

Buy the Alice Googler t-shirt:

```
>"Buy the Alice Googler t-shirt."
>"XL"
```
* Test case name: `Buy the Alice Googler t-shirt`
* Tags: `#catalog, #shirts, #t-shirt, #AliceGoogler`

Buy a t-shirt size M:
(Note the Artist name hasn't been mentioned, but you do want to skip the bands overview, products overview, shirts and shirt size pages)

```
>"Buy a t-shirt size M"
>"The Google Fighters"
```
* Test case name: `Buy a t-shirt size M`
* Tags: #catalog, `#shirts, #t-shirt, #TheGoogleFighters`
* Description: (Note the Artist name hasn't been mentioned, but you do want to skip the bands overview, products overview, shirts and shirt size pages)

Purchase Music of G's N' Roses
(Note this will skip the bands overview and products overview page)

```
>"Purchase music of G's N' Roses"
>"Live"
>"CD"
```
* Test case name: `Purchase music of G's N' Roses`
* Tags: `#catalog, #music, #CD, #GsNRoses, #live`
* Description: (Note this will skip the bands overview and products overview page)

Check price information:

```
>"Which products"
>"Shirts"
>"What's the price difference?"
>"Longsleeve"
>"What does it cost?"
>"M"
>"The Google Dolls"
>"No"
>"Which bands"
>"The Gooo Fighters"
>"Music"
>"How much does it cost?"
>"Greatest Hits"
>"What's the price difference?"
>"Mp3"
>"No"
>"I want to buy the tour movie"
>"Alice Googler"
>"Yes"
```

* Test case name: `Price info`
* Tags: `#catalog, #music, #tourmovie, #shirts`
* Description: Test price info on various points in the dialogue

### Test pre-recorded test cases

1. Select **Manage** > **Test Cases** in the Dialogflow main menu on the left.

2. Select all the test cases and press the **Run** button, above the table.

Dialogflow CX will run all the selected test cases against the recording that was saved as a "Golden Test Case",
if the results are the same as how you saved it, then the tests are passed. - Did something change in the flows
like Pages that are not correctly configured, or intents that directed you to the wrong pages, then the tests will fail.

![Test Cases](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/testcases.png?raw=true)

4. In the simulator ask the following question: `How long will shipping take?`

5. Note the result, and save the test case as `Shipping` with the tag: `#shipping`.

6. Go to the Manage > Test Cases panel and press the **Run** button on the top right of the grid, to run only the `Shipping` test case.

This test should pass.

7. Go back to the Customer Care Flow, Select the **Start** Page and click on the **Routes** header.

This will show a screen with a grid that shows all the routes.

8. Remove the `redirect.shipping.info route`

9. Go to the Manage > Test Cases panel and press the **Run** button on the top right of the grid, to run only the `Shipping` test case.

This test should fail.

10. You can click on the failed test, to see the details of the fail.

In this case the test failed with the below error message:

```
Page: Page mismatch:
Expected: Shipping
Actual: Start Page
```

The reason for this is because the page doesn't exist in the flow anymore. We expected the `Shipping` Page, but instead we never moved away from the `Start` Page.
(or your end-users would receive a fallback message.)

With other words, this is a missed request, a **False Negative** Test result. The test failed. We expected the *Shipping* Page, but nothing happens, or a fallback message was shown.

11. Go back to the Customer Care Flow, and add the `redirect.shipping.info` as an intent route, to the **Start** Page. Don't forget to transition to the **Shipping** Page and hit **Save**.

12. In the simulator record the following test case: `I want to swap my item`, save this test case as `Swapping` `#swapping`.

13. Open **Manage >Intents > redirect.refund.info** and add the following training phrase:
`I want to swap this item for a refund`

Without that training phrase, when a user would ask to change an item for a refund, it would hit the *redirect.swapping.info* intent,
but we don't want to give information on changing items, we want to give information on refunds.

14. Create the following golden test case: `I want to swap this item for a refund` in the simulator, and save this test case as `Swap for Refund` `#refund`

15. Go back to the **Manage >Intents > redirect.refund.info** intent, and remove the `I want to swap this item for a refund` line.

16. Go back to **Manage > Test Cases**, select the *Swap for Refund* test case, and **Run** it.

Your latest test failed, with the below error message:

```
If you would like to change your item for a different one, please return your unwanted item and place a new order. If your refund is accepted, we will refund the price you paid for your item back to your original payment method.`
Is there anything else I can help you with?
 Page: Page mismatch:
Expected: Refund
Actual: Swapping
```

With other words, this is a missed understood request, a **False Positive** Test result. The test failed. We expected the *Refund* Page, but *Swapping* Page became active.

### Coverage

In Dialogflow CX, test **coverage** is a measure used to describe the degree to which the dialogue of the virtual agent (Pages and Intents) is executed when a particular test suite runs.
A virtual agent with high test coverage, measured as a percentage, has had more of its dialogues executed during testing, which suggests it has a lower chance of containing undetected bugs (like missed understood requests) compared to a virtual agent with low test coverage.

1. To view a test coverage report for all test cases, click **Coverage**.

2. Click on the tab **Transitions**.

This will show you the test coverage for all the page transitions.

![Transitions Coverage](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/coverage-transitions.png?raw=true)

3. Click on the tab **Intents**.

This will show you the test coverage for all the intents.

![Intents Coverage](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/coverage-intents.png?raw=true)

Congratulations, by now you have built and tested a complete real world example of a retailer bot!
Let's go to the next lab page to read the conclusion and find some handy references!

## Conclusion

Dialogflow CX is a Conversational AI Platform (CAIP) for creating virtual agents like chat or voice bots. Dialogflow CX empowers your team to accelerate creating enterprise-level conversational experiences through visual bot builders, reusable intents, and the ability to address multi-turn conversations.

In this codelab, you have learned how to build a real world retail virtual agent.
We addressed the following concepts:

+   Flows
+   Parameters, Custom & System Entities
+   Pages
+   State Handlers like Intent Routes and Condition Routes
+   Static Fulfillment Messages and Conditional Responses
+   Fallback intents
+   Simulator, Test Cases and Coverage
 
>aside positive
The retail virtual agent that you have built has quite some complexity.
As you can see in the below image, there are various conversational paths that can lead to various ends.
Certain conversational paths have been reused, and some could have been skipped.
This use case shows how powerful Dialogflow CX really is! Of course you could build retail agents with [Dialogflow ES](https://cloud.google.com/dialogflow/es/docs?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-), but you can't reuse intents, and keeping context and parameters are limited, so you would likely end up by manually coding back-end fulfillments, which require developers.
In Dialogflow CX, once you get the hang of it, it's a matter of clicks, the conversational architect can configure flows with complexity.

![Final Result](https://github.com/savelee/dialogflow-cx-labs/blob/master/img/final-result.png?raw=true)

### References

To learn more about Dialogflow CX have a look into the following blogs and documentation!

* [Dialogflow CX Technical Guides](https://cloud.google.com/dialogflow/cx/docs/quick?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-)
* [Dialogflow CX API Reference](https://cloud.google.com/dialogflow/cx/docs/reference?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-)
* [Dialogflow CX Components built by the community](https://cloud.google.com/dialogflow/cx/docs/tutorials/samples?utm_source=codelabs&utm_medium=et&utm_campaign=CDR_lee_aiml_leedialogflowlabs_cx_&utm_content=-)
* [Dialogflow blog by Conversational AI developer advocate Lee Boonstra](https://www.leeboonstra.dev)
* [Dialogflow CX Prototype / Conversation Design tool built by Lee Boonstra](https://ccai-360.nw.r.appspot.com/)
