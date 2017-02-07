// CardShuflle.cpp 
// ME 493
// Ryan Takayama created 2/4/17

#include <iostream>
#include <assert.h>
#include <string.h>
#include <random>
#include <time.h>
#include <fstream>
#include <time.h>
#include <stdio.h>
#include <cstdlib>
#include <stdlib.h>

#define RTRAND (double)rand()/RAND_MAX

using namespace std;

class card {
public:
	char suite;
	int value;
	};

void getInputFile(char*);
void getUnShuff(char*, card*, int);
void displayUnshuff(card*, int);
void getOutputFile(char*);
void outputFile(char*, card*, int);
void shuffle(char*, card*, card*, int);
void menu();

char choice;
char inFile[20], outFile[20];
int s = 0;
char ret;
card unShuffled[50000];
card * unshuff = unShuffled;
card shuffled[50000];
int ndecks;
//srand(time(NULL)); // I tried to make it completely random but it didn't work

int main()
{
	srand(time(NULL));
	while (choice != 'q') {
		menu();
		cin >> choice;

		switch (choice) {
		case 'g':
			for (s = 0; s < 20; s++) {
				cout << endl;
			}

			cout << endl;
			cout << "How many decks?: ";
			cin >> ndecks;
			cout << endl;
			cout << endl;

			getInputFile(inFile);

			//Get Unshuffled Deck
			getUnShuff(inFile, unShuffled, ndecks);

			cout << "Type a key to return to menu: ";
			cin >> ret;
			break;

		case 'p':
			for (s = 0; s < 20; s++) {
				cout << endl;
			}

				displayUnshuff(unShuffled, ndecks);
			

			cout << "Type a key to return to menu: ";
			cin >> ret;
			break;

		case 's':
			for (s = 0; s < 20; s++) {
				cout << endl;
			}

			getOutputFile(outFile);

			shuffle(outFile, shuffled, unShuffled, ndecks);
			
			outputFile(outFile, shuffled, ndecks);

			cout << endl;
			cout << "Type a key to return to menu: ";
			cin >> ret;
			break;
		}

		}
	
	return 0;
}

void menu()
{
	cout << endl;
	cout << "------Deck Shuffle-----" << endl;
	cout << "=======================" << endl;
	cout << endl;
	cout << "        Main Menu      " << endl;
	cout << "-----------------------" << endl;
	cout << " <g> - Get Deck and Number of Decks" << endl;
	cout << " <p> - Display Cards " << endl;
	cout << " <s> - Shuffle and Output Cards" << endl;
	cout << " <q> - Quit            " << endl;
	cout << "-----------------------" << endl;
	cout << endl;
	cout << "Enter Selection: ";

}

void getInputFile(char * inFile) {

	cout << "Enter Deck Name: ";
	cin >> inFile;
	cout << endl;

}

void getOutputFile(char * outFile) {

	cout << "Enter Output File Name: ";
	cin >> outFile;
	cout << endl;

}

void getUnShuff(char * inFile, card * unshuff, int dec)
{

	ifstream fin;
	char junk[30];
	char * junkptr = junk;
	fin.open(inFile);

	//Get suite/value Cards
		for (int i = 0; i < 52*dec; i++, unshuff++)
		{
			fin >> junkptr;
			(*unshuff).suite = *junkptr;
			cout << (*unshuff).suite;
			fin >> (*unshuff).value;
		}
		
	fin.close();



}

void displayUnshuff(card * unshuff, int ndecks)
{
		for (int l = 0; l < 52*ndecks; l++, unshuff++)
		{
			cout << (*unshuff).suite;
			cout << (*unshuff).value;
			cout << endl;

		}

}

void outputFile(char * outFile, card * shuff, int ndk)
{
	ofstream fout;
	int i;

	//open output file
	fout.open(outFile);

	for (i = 0; i < 52*ndk; i++, shuff++)
	{
		
			fout << (*shuff).suite;
			fout << ' ';
			fout << (*shuff).value;
			fout << endl;

	}

	//close output file
	fout.close();
}

void shuffle(char * outFile, card * shuff, card * unshuff, int decksn)
{

	int x = 0;
	card * start = unshuff;
	int random;
	int check[50000];
	bool test;
	int r = 0;
	int l = 0;

	for (int i = 0; i < 52*decksn; i++, shuff++)
	{

		do
		{
			random = RTRAND * (52 * decksn);
			test = true;
			for (r = 0; r < i; r++) 
			{
				if (random == check[r])
				{
					test = false;
					break;
				}

			}
		} while (!test);

		check[i] = random;

		(*shuff).suite = (*(unshuff + random)).suite;
		cout << (*shuff).suite;

		(*shuff).value = (*(unshuff + random)).value;
		cout << ',' << (*shuff).value << endl;


	}

}

