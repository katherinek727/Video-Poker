# Video-Poker

### 1. Define required constants:
- 1.1) Define a constant array of strings called `deck` for the deck of cards, which includes all 52 cards. The first character of the string indicates the suit. The second character of the string indicates the value.
- 1.2) Define a constant object called `payouts` for the different winning hands and their corresponding payouts.

### 2. Define required variables used to track the state of the game:
- 2.1) Use an array to represent the player's hand.
- 2.2) Use a variable to keep track of the player's current credits.
- 2.3) Use a variable to keep track of the current bet size.
- 2.4) Use a variable to keep track of whether the game is in the deal or draw phase.
- 2.5) Use a variable to track if the game is over (when credits = 0).

### 3. Store elements on the page that will be accessed in code more than once in variables to make code more concise, readable, and performant:
- 3.1) Store the elements representing the player's hand (cards) on the page.
- 3.2) Store the element representing the player's current credits.
- 3.3) Store the element representing the current bet size.
- 3.4) Store the element representing the deal/draw button.
- 3.5) Store the element representing the on-screen messages for payouts (`<h3>` element).
- 3.6) Store the element representing the payouts table (a flexbox containing 9 rows and 6 columns).

### 4. Upon loading, the app should:
#### 4.1) Initialize the state variables:
- 4.1.1) Initialize the player's hand to an empty array.
-  4.1.2) Initialize the player's credits to a default value (1000 credits).
-  4.1.3) Initialize the bet size to a default value (1 credit).
-  4.1.4) Set the game phase to "deal".
#### 4.2) Render those state variables to the page:
-  4.2.1) Render the player's hand:
    - 4.2.1.1) Loop over each card element and set its value to the back of a card using the appropriate CSS class.
#### 4.2.2) Render the player's credits.
- 4.2.3) Render the current bet size = 1.
-  4.2.4) Render the deal button with appropriate text ("Deal").
-  4.2.5) Clear the on-screen messages for payouts.
-  4.2.6) Render the payouts table.

### 5. Handle a player clicking the bet size button:
- 5.1) Initialize the variable `betSize` with a value of 1.
- 5.2) Initialize the variable `maxBetSize` with a value of 5.
- 5.3) Define the function `incrementBetSize` which increments the bet size by one. If the bet size is greater than `maxBetSize`, the `betSize` goes back to 1 credit.
- 5.4) Render the updated bet size on the screen updating the `betSizeEl.innerText`.

### 6. Handle a player clicking the deal/draw button:

#### 6.1) If the game phase is "deal":

##### 6.1.1) Deduct the current bet size from the player's credits:
- Subtract the bet size from the credits.

##### 6.1.2) Render the updated player's credits:
- Define `renderCredits()`:
  - Find the element on the screen that displays the credits.
  - Update the element's text with the current credits value.

##### 6.1.3) Clear the on-screen messages for payouts:
- Define `clearMessages()`:
  - Find the element on the screen that displays messages.
  - Clear the text of this element.

##### 6.1.4) Deal 5 random cards to the player's hand:
  - **6.1.4.1) Shuffle the deck:**
    - Define `shuffleDeck(deck)`:
      - The function takes one parameter `deck`, which is an array representing the deck of cards.
      - Use a FOR loop to iterate through the deck from the last element to the first.
      - In each iteration, generate a random index using `Math.floor(Math.random() * (i + 1))`.
      - Swap the card at the current index with the card at the randomly generated index.
      - Return the shuffled deck.
      - The function returns a new array representing the shuffled deck.

  - **6.1.4.2) Draw the top 5 cards from the shuffled deck:**
    - Define `dealCards(deck, numberOfCards)`:
      - The function takes two parameters: `deck` (the shuffled deck) and `numberOfCards` (the number of cards to draw, which is 5 in this case).
      - Use the `.slice()` method to extract the top `numberOfCards` from the deck.
      - Store the drawn cards in an array called `hand`.
      - Store the remaining cards in an array called `remainingDeck` using `.slice(numberOfCards)`.
      - Return an object containing `hand` and `remainingDeck`.

  - **6.1.4.3) Update the player's hand array with the drawn cards:**
    - In the event handler for the deal button:
      - Call the `shuffleDeck(deck)` function to shuffle the deck.
      - Call the `dealCards(shuffledDeck, 5)` function to draw 5 cards.
      - Update the `playerHand` variable with the drawn cards (`hand`).
      - Update the `remainingDeck` variable with the remaining cards (`remainingDeck`).

  - **6.1.4.4) Render the player's hand on the screen:**
    - Define `renderPlayerHand()`:
      - Use a FOR loop to iterate through the `playerHand` array.
      - For each card in the `playerHand`, find the corresponding card element on the screen (using its ID).
      - Update the card element to display the card value (e.g, "dA", "hQ") using its corresponding CSS class.
      - return null;

##### 6.1.5) Set the game phase to "draw":
- Change the `gamePhase` state variable to "draw".

#### 6.2) If the game phase is "draw":

##### 6.2.1) Allow the player to select cards to hold:
  - **Initialize the `heldCards` Array:**
    - Create an array `heldCards` initialized with `false` values indicating no cards are held at the start.
    - Example: `let heldCards = [false, false, false, false, false];`
  - Add event listeners to each card element to toggle the hold status.
  - The event listener toggles the hold status of the corresponding card, changes the `heldCards` index value to `true`, and adds a CSS class that visually indicates that the card is being held.

##### 6.2.2) Replace the non-held cards with new random cards:
- When the player clicks on the deal button:
  - Replace the cards in the player's hand that are not held with new cards from the shuffled deck.
  - Define `replaceCards()`:
    - Use a FOR loop to iterate through the `playerHand` array.
    - For each card:
      - Check the corresponding value in the `heldCards` array.
      - If the card is not held (`false`):
        - Replace it with the next card from the shuffled remaining deck using `.pop()`.
        - Update the `playerHand` with the new card.
        - Remove the used card from the remaining deck.

##### 6.2.3) Evaluate the player's hand for any winning combinations:
- Check the player's hand against predefined winning combinations.
- Define `evaluateHand(hand)`:
  - Use IF conditions to check the hand against each winning combination.
  - Return the winning combination and payout, if any.
  - A winning payout starts at "Jacks or Better":
    - A hand qualifies for "Jacks or Better" if it contains a pair of Jacks, Queens, Kings, or Aces.

  - **Check the Player's Hand:**
    - Extract the ranks of the cards and save them to an empty array called `ranks`.
    - Iterate through the player's hand to extract the rank of each card using `.slice(1)` to remove the first character (suit).
    - Example: For card "dJ", the rank is "J".

  - **Count the Occurrences of Each Rank:**
    - Use an object (`rankCounts`) and a FOR loop to count how many times each rank appears in the hand.
    - Example: `{ "2": 0, "3": 0, ..., "J": 1, "Q": 2, ... }`

  - **Check for a Pair of Jacks, Queens, Kings, or Aces:**
    - Use IF conditions to check if there is a pair of any of the specified ranks.
    - Example: `if (rankCounts["J"] >= 2 || rankCounts["Q"] >= 2 || rankCounts["K"] >= 2 || rankCounts["A"] >= 2)`

  - **Return the Winning Combination and Payout:**
    - If a winning combination is found, return the combination name and the corresponding payout, as described in the `payouts` object.
    - Example: Return `{ combination: "Jacks or Better", payout: 1 }`.

##### 6.2.4) Update the player's credits based on the winning combination (if any):
  - Add the payout amount to the player's credits.
  - Example: `playerCredits += payout;`
  - Update the credits display using `creditsEl.innerText = `Credits: ${playerCredits}`.

##### 6.2.5) Render a message indicating the result of the hand (e.g., win/loss and credits won):
- If the payout is greater or equal to 1, it means the player won.
- Display a message using the `<h3>` element's `innerText` to indicate the winning combination.

##### 6.2.6) Set the game phase back to "deal":
- Change the `gamePhase` state variable to "deal".
