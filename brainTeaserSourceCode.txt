# include <iostream.h>
# include <stdlib.h>
# include <string.h>
# include <conio.h>

const int max_length=80;
const int max_tries=5;
//const int MAXROW=7;

class dipesh
{       protected:
	char name[20];
	//string name;
	int wrong_guesses;
	public:
	void getdata();
	void display();
	//int secret_word(char, char[],char[]);
	//int secret_word(char[], char[],int);
	virtual void play()=0; //Pure Virtual Function
	//void friend winner(dipesh,dipesh);
};

class shrestha:public dipesh
{
	public:
	void play();
	int secret_word(char, char[], char[]);
	int secret_word(char[], char[], int);
	void friend winner(shrestha, shrestha);
};

void winner(shrestha player1,shrestha player2)
{

	clrscr();
	cout<<"\n\t\tPLAYER 1:"<<player1.name<<"\t\t No of Wrong Guesses="<<player1.wrong_guesses;
	cout<<"\n\n\t\tPLAYER 2:"<<player2.name<<"\t\t No of Wrong Guesses="<<player2.wrong_guesses;

	if(player1.wrong_guesses<player2.wrong_guesses)
		cout<<"\n\n\n\n\n\n\n\t\t\t"<<player1.name<<"! You won it"<<endl;
	else if(player1.wrong_guesses==player2.wrong_guesses)
		cout<<"\n\n\n\n\n\n\n\t\t\t"<<player1.name<<" and "<<player2.name<<" It's a tie";


	else
		cout<<"\n\n\n\n\n\n\n\t\t\t"<<player2.name<<"! You won it"<<endl;
}


void dipesh::display()
{
	cout<<name;
}

void dipesh::getdata()
{
	cout<<"\nEnter your name: "<<endl;
	cin>>name;
	wrong_guesses=0;
}

int shrestha::secret_word(char word[], char unknown[],int length)
{
int i;
length = strlen(word);
for (i = 0; i < length; i++)
unknown[i]='*';
unknown[i]='\0';
return length;
}

int shrestha::secret_word(char guess, char secretword[], char encrypted_word[])
{
int i,matches=0;
for (i = 0; secretword[i]!='\0'; i++)
{
// checking if already guessed
if (guess == encrypted_word[i])
return 0;
// checking if the guessed word matches
if (guess == secretword[i])
{
encrypted_word[i] = guess;
matches++;
}
}
return matches;
}

void shrestha::play()
{
	char unknown[max_length],word[max_length];
	char letter;
	int check_length,choice;

	char data[][max_length] =
	{
		"india",
		"zebra",
		"nepal",
		"lion",
		"watch",
		"buddha",
		"everest",
		"tiger",
		"sahara"
	};
	cout<<"\nChoose a number between 1-20 Except(12,14,15,16,19,20)"<<endl;
	choose: cin>>choice;

	int n = rand()%choice;
	strcpy(word,data[n]);
	clrscr();
	switch(choice)

	{
		case 1:
		case 2:
		case 5:

		{
			cout<<"\n Hint: The Country of Mahatma Gandhi or The animal whose skin looks like Zebra Crossing"<<endl;
			break;
		}
		case 10:
			cout<<"\n Hint: The Country of Mahatma Gandhi or The highest PEAK of the world (Height:8848m)"<<endl;
			break;
		case 3:
		//case 15:
		{
			cout<<"\n Hint: The animal whose skin looks like Zebra Crossing"<<endl;
			break;
		}
		case 4:
		case 8:
		//case 16:
		{
			cout<<"\n Hint: The Country with unique Flag and Mt Everest"<<endl;
			break;
		}
		case 6:
		case 9:
		//case 14:
		case 18:
	       //case 19:

		{
			cout<<"\n Hint: The thing that makes us Punctual"<<endl;
			break;
		}
		case 7:
			cout<<"\n Hint: The KING of jungle or The thing that makes us PUNCTUAL"<<endl;
			break;
		case 11:
		{
			cout<<"\n Hint: The god who follows PEACE and was born in Nepal"<<endl;
			break;
		}
		case 13:
		{
			cout<<"\n Hint: The biggest DESERT of world or The Country of Mahatma Gandhi"<<endl;
			break;
		}
		case 17:
		//case 20:
		{

			cout<<"\n Hint: The highest PEAK of the world (Height:8848m)"<<endl;
			break;
		}
		case 12:
		case 15:
		case 16:
		case 19:
		case 20:
		case 14:
		default:
			cout<<"\nEnter valid choice"<<endl;
			goto choose;
	}

	check_length=secret_word(word, unknown,check_length);

	cout << "\n\nWelcome " <<name<<" to the game";
	cout << "\n\nGuess the encrypted word";
	cout << "\n\nYou have " << max_tries << " tries left";
	cout << "\n~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~";
	//cout<<"\nHINT: The length of the word is :"<<check_length;
	while(wrong_guesses < max_tries)
	{
	cout << "\n\n" << unknown;
	cout << "\n\nGuess a letter: ";
	cin >> letter;
       //check_length=strlen(letter);
	/*
	while(!cin.good())
	{
		cout<<"Ivalid! Please enter only a letter."<<endl;
		cin.ignore(1024,'\n');
		cin>>letter;
	}*/

	if (secret_word(letter, word, unknown)==0)
	{
	cout << endl << "Whoops! That letter '"<<letter<<"' isn't in there!\n" << endl;
	wrong_guesses=wrong_guesses+1;
	}
	else cout << endl << "\nYou found a letter! Isn't that exciting!\n" << endl;

	display();
	cout << "! You have " << max_tries - wrong_guesses;
	cout << " guesses left.\n" << endl;

	if (strcmp(word, unknown) == 0)
	{
	cout << word << endl;
	display();
	cout << "! You got it!\n";
	break;
	}
	}
	if(wrong_guesses == max_tries)
	{
	cout << "Sorry! The word was : " << word << endl;
	}
}

int main ()
{
	clrscr();
	cout<<"\nFirst Player\n";
	dipesh *ptr; //creating pointer of base class for Pure Virtual fun
	shrestha s1,s2;
	s1.getdata();
	//d1.play();
	ptr=&s1; //base class pointer pointing to object of derived class
	ptr->play();//calling Pure Virtual Function play()
	cout<<"\nSecond Player\n";
	s2.getdata();
	//d2.play();
	ptr=&s2;
	ptr->play();
	winner(s1,s2);
	getch();
	return 0;
}
