# Hangman Game - Detailed Explanation

This repository contains a Python implementation of the classic Hangman game. The game selects a random word, and the player attempts to guess it letter by letter. If the player makes too many incorrect guesses, the game ends.

## Code Breakdown

### Importing Required Modules
```python
import random
```
The `random` module is imported to enable the selection of a random word from the predefined word list.

### Defining the Hangman Stages
```python
HANGMAN_PICS = ['''
+---+
    |
    |
    |
   ===''', '''
+---+
O   |
    |
    |
   ===''', '''
+---+
O   |
|   |
    |
   ===''', '''
+---+
O   |
/|   |
    |
   ===''', '''
+---+
O   |
/|\  |
    |
   ===''', '''
+---+
O   |
/|\  |
/    |
   ===''', '''
+---+
O   |
/|\  |
/ \  |
   ===''']
```
This list defines the different stages of the Hangman game, visually representing the hangman as the player makes incorrect guesses.

### Defining the Word List
```python
words = 'ant baboon badger bat bear ... zebra'.split()
```
A string of words is provided, and `.split()` is used to convert it into a list of words.

### Function to Select a Random Word
```python
def getRandomWord(wordList):
    wordIndex = random.randint(0, len(wordList) - 1)
    return wordList[wordIndex]
```
This function selects a random word from the list using `random.randint()`.

### Function to Display the Game Board
```python
def displayBoard(missedLetters, correctLetters, secretWord):
    print(HANGMAN_PICS[len(missedLetters)])
    print('\nMissed Letters:', end=' ')
    for letter in missedLetters:
        print(letter, end=' ')
    print()
    blanks = '_' * len(secretWord)
    for i in range(len(secretWord)):
        if secretWord[i] in correctLetters:
            blanks = blanks[:i] + secretWord[i] + blanks[i+1:]
    for letter in blanks:
        print(letter, end=' ')
    print()
```
This function:
- Displays the current hangman stage based on the number of missed guesses.
- Shows letters guessed incorrectly.
- Replaces underscores with correctly guessed letters in the secret word.

### Function to Get the Player's Guess
```python
def getGuess(alreadyGuessed):
    while True:
        print('Guess a letter.')
        guess = input().lower()
        if len(guess) != 1:
            print('Please enter a single letter.')
        elif guess in alreadyGuessed:
            print('You have already guessed that letter. Choose again.')
        elif guess not in 'abcdefghijklmnopqrstuvwxyz':
            print('Please enter a LETTER.')
        else:
            return guess
```
This function:
- Ensures the player inputs only a single valid letter.
- Prevents repeated guesses.
- Converts the input to lowercase for consistency.

### Function to Ask if the Player Wants to Replay
```python
def playAgain():
    print('Do you want to play again? (yes or no)')
    return input().lower().startswith('y')
```
This function asks the player if they want to play again and returns `True` or `False`.

### Main Game Logic
```python
print('T H E  H A N G M A N  G A M E')
missedLetters = ''
correctLetters = ''
secretWord = getRandomWord(words)
gameIsDone = False

while True:
    displayBoard(missedLetters, correctLetters, secretWord)
    guess = getGuess(missedLetters + correctLetters)
    if guess in secretWord:
        correctLetters += guess
        foundAllLetters = all(secretWord[i] in correctLetters for i in range(len(secretWord)))
        if foundAllLetters:
            print('Yes! The secret word is "' + secretWord + '"! You have won!')
            gameIsDone = True
    else:
        missedLetters += guess
        if len(missedLetters) == len(HANGMAN_PICS) - 1:
            displayBoard(missedLetters, correctLetters, secretWord)
            print('You have run out of guesses! The word was "' + secretWord + '"')
            gameIsDone = True
    if gameIsDone:
        if playAgain():
            missedLetters = ''
            correctLetters = ''
            gameIsDone = False
            secretWord = getRandomWord(words)
        else:
            break
```
This block:
- Initializes the game state.
- Runs a loop where the game board updates after each guess.
- Checks if the player has won or lost.
- Offers a replay option at the end.

## How to Run the Program
1. Save the script as `hangman.py`.
2. Run the script using Python:
   ```sh
   python hangman.py
   ```
3. Follow the prompts to play the game.

## Conclusion
This Hangman game is a simple demonstration of Python logic, user input handling, and string manipulation. It can be further improved by adding difficulty levels, a graphical interface, or a word dictionary.

Enjoy coding and playing Hangman!
