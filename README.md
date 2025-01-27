# HAngman

#include <iostream> 
#include <string>
#include<ctime>
#include <algorithm>
using namespace std;
//global constansts and variables
const int MAX_INCORRECT = 6; 
char incorrect[MAX_INCORRECT];
int numIncorrect = 0;
int misses = 0;
// Function to display the hangman drawing based on how many times the player has guessed wrong
void displayHangman() {
	cout << "\n"; 
	switch (misses) {
	case 0:
		cout << " +---+\n";
		cout << " |   |\n";
		cout << "     |\n";
		cout << "     |\n";
		cout << "     |\n";
		cout << "     |\n";
		cout << "========\n";
		break;
	case 1:
		cout << "  +---+\n";
		cout << "  |   |\n";
		cout << "  O   |\n";
		cout << "      |\n";
		cout << "      |\n";
		cout << "      |\n";
		cout << "=========\n";
		break;
		// Additional cases for more misses would continue here
	}
}
// Function to display the current state of the game
void display(string guessedword) {
	cout << "\n Word: ";
	for (int i = 0; i < guessedword.length(); i++) {
		cout << guessedword[i] << ' ';
	}
	cout << "\n Incorrect guesses: ";
	for (int i = 0; i < guessedword.length(); i++) {
		cout << incorrect[i] << ' ';
	}
	cout << "\n Misses left:" << MAX_INCORRECT - misses << "\n";
	displayHangman();
}
string processGuess(char guess, string word, string guessedWord) {
	bool iscorrect = false;
	for (int i = 0; i < word.length(); i++) {
		if (word[i] == guess && guessedWord[i] == '_') {
			guessedWord[i] = guess;
			iscorrect = true;

		}
	
	}
	if (!iscorrect) {
		bool alreadyGuessed = false;
		for (int i = 0; i < numIncorrect; i++) {
			if (incorrect[i] == guess) {
				alreadyGuessed = true;
				break;
			}

	}
	
		if (!alreadyGuessed) {
			incorrect[numIncorrect++] = guess;
			misses += 1;
		}
	}
	return guessedWord;
}
int main() {
	string wordlist[] = { "computer", "science", "programming", "hangman", "game" };
	srand(time(0));
	string word = wordlist[rand() % 5];
	string guessedWord(word.length(), '_');

	cout << "Welcome to Hangman!\n";

	while (misses < MAX_INCORRECT && guessedWord != word) {
		display(guessedWord);
		cout << "Enter your guess: ";
		char guess;
		cin >> guess;

		string oldGuessedWord = guessedWord;
		guessedWord = processGuess(guess, word, guessedWord);

		if (guessedWord == oldGuessedWord) {
			cout << "Oops! That letter isnt in there!\n";
		}
		else {
			cout << "Good guess!\n";
		}
	}
	if (guessedWord == word) {
		cout << "Congratulations! You guessed the word:" << word << "\n";

	}
	else {
		cout << "Game Over! The word was:" << word << "\n";
	}
	return 0;
}
