# How to build a virtual assistant using Watson
We will be creating an assistant to take coffee orders.

## Creating an IBM Cloud Account
1. Go to this link and create an account: https://console.bluemix.net/registration/
2. Or, go to https://console.bluemix.net/ and login if you have an account already and login

## Provisioning a Watson Assistant instance
1. Once logged in, click on `Catalog` in the upper right corner of the screen
2. Search for `Assistant`
3. Under the `AI` Category, click on `Watson Assistant`
4. A service name is generated for you, but you can chagne it if you like
4. Scroll down and make sure `Lite` Plan is selected for the free plan
5. Click `Create`
6. Click on `Launch tool`

## Creating a Skill
1. Once in the tooling, navigate to the `Skills` tab and click `Create new`
2. Name it something like `Coffee-assistant` and select the desired language

## Building Intents
1. Click `Add intent`
2. Name the new intent `order-drink`
3. Add a description of what the intent will do. For this, let's use "User wants to order a drink."
4. Hit `Enter` to create the intent
5. Start adding a few exaxmples of how a user would order a drink (at least 5 examples are recommended). Let's use the following:
  - i would like to order a drink please
  - I need some caffeine
  - order espresso
  - a cappuccino would be lovely
  - a latte please
6. Open the `Try it` panel by clicking the button in the upper right corner. This allows you to test how your assistant will respond
7. Wait for the assistant to finish training, then type `can I order a coffee`. It should classify the intent as `#order-drink`. Even though you didn't train the intent on this exact sentence, Watson can still understand it.
8. Add a few more intents to make your assistant more robust. Try creating the following intents and adding a few examples to each:
  - #see-menu (User wants to see what's on the menu)
  - #greetings (User greets the assistant)
  - #thanks (User thanks the assistant)
  
Here are the finished intents:
![finished intents](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-intents.png)

## Building Entities
1. Click on the `Entities` tab at the top of the page
2. Click `Add entity` and add the name `drink`
3. Turn `Fuzzy Matching` on if you want Watson to understand misspellings
4. Add a value `coffee` with the synonym of `cafe`. 
5. Click `Add value` to create the first value for this entity type
6. Add some additional values that you allow your users to order and any synonyms, for example:
  - espresso
  - cappuccino
  - latte
  - tea
7. Click `show recommendations` to see some common synonyms for these entity values 
8. Exit the page, and click on `System entities` underneath the `Entities` tab
9. Turn on `@sys-number`

Here is how your finished entity `@drink` should look:
![finished entity](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-entity.png)

## Building Dialog
1. Click on the `Dialog` tab at the top of the page
2. Click `Create`
3. Click on the `Welcome` node if you would like to change the intro message
4. Click `Add node` in the upper left, and name it `Greetings`
5. Add your `#greetings` intent in the field for `If assistant recognizes`. Use the drop down suggestions for easier navigation
6. Fill in a response that says something like "Hi! How can I help you today?"
7. Create two more nodes for `#thanks` and `#see-menu` and add responses
8. Make sure they are at the `root` level, equal to the other 3 nodes you already have
9. You can add response variatons so that the assistant will respond differently each time your users reach this node
10. You can also add additional response bubbles by click `+Add response type` so that Watson will respond with two messages at the same time.
11. Note that if desired, you can create clickable buttons/drop down menu using the `Option` response type, you can use the `Image` response type to display an image from a URL, and you can ask Watson to pause if you want want a delay before the message shows.
12. Create another node and name it `Order Drink`
13. To the right of the name, click on `Customize`
14. Turn on `Slots` and hit `Apply`
15. Add the intent `#order-drink`to `If assistant recognizes:`
16. Under `Check for`, add the entity `@drink`, notice that Watson automatically creates a context variable for you
17. Under `If not present, ask` add a question like "What would you like to drink?"
18. Click `Add slot`, and add a condition and prompt for `@sys-number`: "How many cups of $drink would you like?" (Note: the syntax `$variable` is short hand for accessing Context variables. Context variables allow you to pass information between your application and Watson Assistant, as well as reuse information throughout the conversation.)
19. Add in the response, "Ok, I have $number $drink coming right up!"
20. Try it out! Try asking questions related to the dialog you have just created. Witness the power of slots by giving different amounts of information each time. "I would like a drink", "I want coffee", and "I want two coffees"

Finished dialog tree with `Order Drink` open:
![finished dialog](https://github.com/desmarchris/think-lab/blob/master/pictures/finished-dialog.png)

## Connecting to an Assistant
1. Click the blue arrow in the upper left to navigate back to the `Home` screen
2. Click the `Assistants` tab in the top left
3. Click `Create new` to create a new Assistant, give it a name like `Coffee Virtual Assistant`
4. Add your Coffee Dialog Skill to your Coffee Assistant
5. You will see the `Preview Link` is already enabled as an Integration, you can also add Slack or Facebook if you are an admiin of a Slack team (or want to create a free one) or are a Facebook Page Admin (or want to create a free one)
6. You can test your new assistant at the `Preview Link` and share the link with your friends and coworkers for testing and to gather data. 

## Development Lifecycle
Any messages received from outside of the `Try it` panel will show up in the `Improve` tab
1. Re-enter your `Coffee assistant` Dialog Skill, and on the left, click the tab on the far left navigation bar in the shape of a graph, just below the `Build` tab that is highlighted
2. See how many total conversations your assistant has had, as well as some of the top statistics here
3. Navigate to the `User conversations` tab to see all the messages the assistant has received lately
4. Before making any changes, you will want to save the state of your current workspace so that your users do not interact with something that may be in progress or untested
5. Navigate back to the `Build` tab on the left had nav bar
6. Click `Save new version` in the upper right
7. Give it a `Description` like `First release of coffee ordering virtual assistant`
8. Navigate to the `Version History` tab to see your newly saved version, click the three dots to the right of your new version and choose `Assign to assistant...` to connect this new, static version to your live coffee assistant. Your Development version can remain unlinked
9. Navigate back to the  `Improve` tab, `User conversations` page, and new data based on what was input into your `Preview Link` or other integrations
10. Click the pencil to the right of an input to add new intent data, or the pencil further to the right to add new entities. Adding new data can make Watson more confident in something you already taught, or add new intents and entities all together
11. If you added brand new intents or entities, go back to the Dialog tab and create new nodes to give them responses
12. Once you've added new data, test it in the `Try it` panel. If everything works as you expect, save a second new version back on the `Build` tab, give it a description like `Improved confidence of first release` and link it to your coffee assistant. If things did not go as expected or you broke something you had built before, you can always go back to your `Version History` and choose to copy your first version back to your development environment to continue from the original state

## If you want more...
Did you finish the above and want to learn more? Try some of the following methods to bolster your Coffee Assistant.

### Resetting context
If your user orders a drink and completes the flow, and they try to make another order, the values found from the first flow will still be there so they will not be able to order something else. To fix this, we need to clear the context after a successful order so the values are not stored for the next order.
1. Create a node above the Slots node `Order Drink` called `Order Drink - Clear Context`
2. Set the condition to `#order-drink`
3. In the response section, click on the three button menu on the right and click on `Open context editor`
4. Fill in both of the variables (`drink` and `number`) and set the values to `null`
5. Click on the three dot menu on the right side of original Slots node `Order Drink`, and select `Move`. Then, click the new context clearing node and move to `As Child Node` (So, the parent node is the context clearing node, and the slots node is the child)
6. Go to the section called `And finally` at the bottom of the context clearing node. Select `Jump to` and click the slots node, then `If assistant recognizes condition`
7. Change the condition of the slots node from `#order-drink` to `true` (Use this condition if you want the node to always fire)
8. Try it out! Without clearing the try it out panel, order a drink. Once finished, try ordering another drink and it should prompt you for the two needed variables again. Here's what the finished context clearing node will look like:
![clear context](https://github.com/desmarchris/think-lab/blob/master/pictures/clear-context.png)

### Help - Digressions
Sometimes, you will want an intent to be handled no matter where the user is in their flow. Digressions allow you to respond to an intent even if a user is in the middle of a process flow, and then it allows them to return to their prior flow without losing their progress. 
1. Create a `#help` intent with examples like: "I need help"
2. Create a node below your `Order Drink` node
3. Add the condition of `#help` with a response like: "I can help you order a drink from my coffee shop. Just say order drink to get started!"
4. Go into the `Customize` portion of the node by clicking in the upper right
5. Click on the `Digressions` tab
6. Enable `Return after digression` (Digressions should be on by default, this setting allows you to handle the intent and then return back to the flow)
7. Now to test this out, we need to get in the middle of our order drink flow. But first, since it is a slot, we need to go into the `Digressions` tab in the `Order Drink` slots node
8. Turn on `Allow digressions away while slot filling` and click the button that only allows nodes with returns enabled. This will help you to control which nodes you want to allow to digress to
9. Try it out by saying "order drink", then when asked for what kind of drink you want, say "help". You should see a response from your help node with another follow up message for the next slot filling question
