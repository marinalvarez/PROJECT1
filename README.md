# PROJECT1
//This code is a simple version of the Simon says game, which was installed into a punching bag. The user has to remember the sequence the punching bag shows him and repeat it. You can add levels to increase the difficulty.

const int MAX_LEVEL = 100;  //Maximum game level


int sequence[MAX_LEVEL];


int your_sequence[MAX_LEVEL];


int level = 1;   //Level starts in 1


int velocity = 1000;    //Value to increase or decrease difficulty, the time leds stay in high or low mode changes


void setup() {    //In this void the different leds (outouts) and buttons pins (input) are stablish. Also, it is too the inicial leds state, low.
 
  pinMode(A0, INPUT);
  
  
  pinMode(A1, INPUT);
  
  
  pinMode(A2, INPUT);
  
  
  pinMode(A3, INPUT);

  pinMode(2, OUTPUT);
  
  
  pinMode(3, OUTPUT);
  
  
  pinMode(4, OUTPUT);
  
  
  pinMode(5, OUTPUT);

  digitalWrite(2, LOW);
  
  
  digitalWrite(3, LOW);
  
  
  digitalWrite(4, LOW);
  
  
  digitalWrite(5, LOW);
}

void loop()   //In this void it is summarised the basic functioning of the programme 
{


  if (level == 1) 
  
  
    generate_sequence();  //Firstly, the game generates a sequence (functions are better explained after)


  if (digitalRead(A4) == LOW || level != 1) //Lights are off start button is pressed OR you're winning because you have accomplished the level (you stay in the loop)
  
  
  {
    show_sequence();    //Afterwords,it shows the user the sequence by turning on lights
    get_sequence();     //Last, the programme waits for the user sequence to be introduced, and it decides what to do depending on if the introduced sequence is right or wrong
    
    
  }
}

void show_sequence()  //This void is the one showing the sequence. It turns the lights off and only turns on the light of the sequence in order. 
{
  digitalWrite(2, LOW); 
  
  
  digitalWrite(3, LOW);
  
  
  digitalWrite(4, LOW);
  
  
  digitalWrite(5, LOW);
  

  for (int i = 0; i < level; i++) //Searchs in the sequence vector the random values up to the one you are in. As you increase the level it will show more lights. 
  {  
    digitalWrite(sequence[i], HIGH); //Turns on the led to the corresponding possition of the vector
    delay(velocity);
    digitalWrite(sequence[i], LOW);  //Turns off the led to the corresponding possition of the vector
    delay(200);  // If you are in a level over 1, it has a delay between each light
  }
}

void get_sequence() //This void is the one gathering together the users' answer
{
  int flag = 0; // This flag indicates if the sequence is right

  for (int i = 0; i < level; i++) // If the sequence progress...
  {
    flag = 0;
    while (flag == 0)
    {
      if (digitalRead(A0) == LOW)   //If The user hits button number 0
      {
        digitalWrite(5, HIGH);  //Light number 5 has to turn on
        your_sequence[i] = 5; //Now the user sequence is 5
        flag = 1; 
        delay(200);
        if (your_sequence[i] != sequence[i]) // If the number 5 doesn't correspond to the Arduino sequence, it is wrong (game over)
        {
          wrong_sequence();
          return;  //In case the sequence marked is wrong the game starts again
        }
        digitalWrite(5, LOW); //Light number 5 turns off
      }

      if (digitalRead(A1) == LOW) //If The user hits button number 1
      {
        digitalWrite(4, HIGH);  //Light number 4 has to turn on
        your_sequence[i] = 4; //Now the user sequence is 4
        flag = 1;
        delay(200);
        if (your_sequence[i] != sequence[i])  // If the number 4 doesn't correspond to the Arduino sequence, it is wrong (game over)
        {
          wrong_sequence();
          return;   //In case the sequence marked is wrong the game starts again
        }
        digitalWrite(4, LOW); //Light number 4 turns off
      }

      if (digitalRead(A2) == LOW) //If The user hits button number 2
      {
        digitalWrite(3, HIGH);  //Light number 3 has to turn on
        your_sequence[i] = 3; //Now the user sequence is 3
        flag = 1;
        delay(200);
        if (your_sequence[i] != sequence[i])  // If the number 3 doesn't correspond to the Arduino sequence, it is wrong (game over)
        {
          wrong_sequence();
          return; //In case the sequence marked is wrong the game starts again
        }
        digitalWrite(3, LOW); //Light number 3 turns off
      }

      if (digitalRead(A3) == LOW) //If The user hits button number 3
      {
        digitalWrite(2, HIGH);  //Light number 2 has to turn on
        your_sequence[i] = 2; //Now the user sequence is 2
        flag = 1;
        delay(200);
        if (your_sequence[i] != sequence[i])  //If the number 2 doesn't correspond to the Arduino sequence, it is wrong (game over)
        {
          wrong_sequence();
          return; //In case the sequence marked is wrong the game starts again
        }
        digitalWrite(2, LOW); //Light number 2 turns off
      }

    }
  }
  right_sequence(); //If the sequence marked correspond to the Arduino sequence, it is right (game keeps on, so does the loop)
}

void generate_sequence() //This void is the one generating the sequence the users' has to follow
{
  randomSeed(millis()); //RandomSeed initializes the pseudo-random number generator, causing it to start at an arbitrary point in its random sequence. This sequence, while very long, and random, is always the same. 

  for (int i = 0; i < MAX_LEVEL; i++)
  {
    sequence[i] = random(2, 6); //Creates the vector with random numbers including led pin number two, three, four, and five; the number six is excluded 
  }
}
void wrong_sequence() //This void is the one defining what happens if the sequence introduced is wrong
  for (int i = 0; i < 3; i++)
  {
    digitalWrite(2, HIGH);  //Led number 2 turn on and blink three times
    digitalWrite(3, HIGH);  //Led number 3 turn on and blink three times
    digitalWrite(4, HIGH);  //Led number 4 turn on and blink three times
    digitalWrite(5, HIGH);  //Led number 5 turn on and blink three times
    delay(250);
    digitalWrite(2, LOW); //Led number 2 turns off
    digitalWrite(3, LOW); //Led number 3 turns off
    digitalWrite(4, LOW); //Led number 4 turns off
    digitalWrite(5, LOW); //Led number 5 turns off
    delay(250);
  }
  level = 1;
  velocity = 1000;  //Decrease difficulty, lights stay in high mode more time if this value increases
}

void right_sequence() //This void is the one defining what happens if the sequence introduced is right
{
  digitalWrite(2, LOW); //Once you have introduced the right sequence, led 2 turns off
  digitalWrite(3, LOW); //Once you have introduced the right sequence, led 3 turns off
  digitalWrite(4, LOW); //Once you have introduced the right sequence, led 4 turns off
  digitalWrite(5, LOW); //Once you have introduced the right sequence, led 5 turns off
  delay(250);

  digitalWrite(2, HIGH);  //Led 2 turns on
  digitalWrite(3, HIGH);  //Led 3 turns on
  digitalWrite(4, HIGH);  //Led 4 turns on
  digitalWrite(5, HIGH);  //Led 5 turns on
  delay(500);
  digitalWrite(2, LOW); //Led 2 turns off
  digitalWrite(3, LOW); //Led 3 turns off
  digitalWrite(4, LOW); //Led 4 turns off
  digitalWrite(5, LOW); //Led 5 turns off
  delay(500);

  if (level < MAX_LEVEL)  //If the level achieved is lower than the maximum level, you keep on to the next one 
  level++;

  velocity -= 50; //Increase difficulty, lights stay in high mode less time if this value decreases. If the - was deleated it would be the other way around.
}
