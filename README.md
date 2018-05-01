
import javax.swing.*;
import java.awt.*;
import java.awt.image.*;
import javax.imageio.*;
import java.io.*;
import java.util.Random;
import java.util.Scanner;
import java.util.*;
import java.io.File;
import java.io.IOException;
import java.io.FileWriter;
import java.io.BufferedWriter;
/**
 * The class ExplodingKittens is a replica of the Exploding Kittens game. 
 * @prereq      none
 * @param      oneChoice, twoChoice, threeChoice, fourChoice, player, pSteal
 * @return       none             
 * @post        The program prints out a window containing a simulation of the game "Exploding Kittens" and inputs into and outputs files representing components of "Exploding Kittens".
 * Andy Yim and Tommy Yim 
 * 
 */
public class ExplodingKittens
{
    // instance variables
    Random rand = new Random();
    ArrayList<String> EKC = new ArrayList<String>();
    ArrayList<String> EK = new ArrayList<String>();
    ArrayList<String> discard = new ArrayList<String>();
    ArrayList<String> player1 = new ArrayList<String>();
    ArrayList<String> player2 = new ArrayList<String>();
    ArrayList<String> player3 = new ArrayList<String>();
    ArrayList<String> player4 = new ArrayList<String>();
    private Help c2;
    private int size;
    private String choice1;
    private int choice2;
    private String oneChoice;
    private String twoChoice;
    private String threeChoice;
    private String fourChoice;
    /**
     * Constructor for objects of class ExplodingKittens
     */
    public ExplodingKittens()
    {
        Scanner scan = new Scanner(System.in);
        String key;
        System.out.println("----------EXPLODING KITTENS BETA----------");
        System.out.println("***There are some glitches. If you encounter them, ignore them");
        System.out.println("and continue with the game or rerun the program.***");
        System.out.println("In the deck of cards are some Exploding Kittens."); 
        System.out.println("There are also action cards that you can type to activate their effects.");
        System.out.println("These effects will help you gain a less likely chance to draw an Exploding Kitten.");
        System.out.println("Players can play as many action cards as they want. Their turn ends when they draw a card.");
        System.out.println("You play the game by taking turns drawing cards until someone draws an Exploding Kitten.");
        System.out.println("When that happens, that person can use a defuse card."); 
        System.out.println("If they can't, they are now dead and out of the game.");
        System.out.println("Commands: 1) type the card's EXACT name 2) d# to draw a card and end turn 3) h for help 4) ragequit");
        System.out.println("Keep track of and open files that may/may not appear on your desktop!");
        System.out.println("1) Player Files (EK.Hand#.txt) 2) Help File (EKHelp.txt) 3) Discard File (EKDiscard.txt)");
        System.out.println("IMPORTANT: CONSTANTLY OPEN AND CLOSE YOUR FILES AND CHECK YOUR FILES"); 
        System.out.println("AT THE START AND END OF YOUR TURN TO REFRESH AND UPDATE YOUR HAND");
        System.out.println("The LATEST LINE in the file is your hand.");
        System.out.println("Remember to DELETE the Files once the players are finished playing.  Thank you and GLHF.");
        System.out.println("Press 'h' to go to the help menu to learn the special effects: ");
        System.out.println("Press 'r' when players are ready: ");
        key = scan.nextLine();
        System.out.println();
        if (key.equals("r"))
        {
            gameStart();
        }//start the game if players are ready ("r")
        if (key.equals("h"))
        {
            c2 = new Help();
        }//go to help class if players need help ("h")
    }//constructor of ExplodingKittens()

    public void gameStart()
    {
        Scanner scan = new Scanner(System.in);
        System.out.println("How many players are playing?(2-4) ");
        size = scan.nextInt();
        System.out.println("Now, check your desktop for player files during the players' first turn :)");
        System.out.println("There should be 5 cards in each hand. If there's 6 in one person's hand or an error, then rerun the program.");
        System.out.println();
        for(int j = 0; j < 4; j++)
        {
            EKC.add(j, "Skip");
        }//add 4 "Skip" cards to EKC deck
        for(int k = 4; k < 8; k++)
        {
            EKC.add(k, "Attack");
        }//add 4 "Attack" cards to EKC deck
        for(int l = 8; l < 12; l++)
        {
            EKC.add(l, "Shuffle");
        }//add 4 "Shuffle" cards to EKC deck
        for(int m = 12; m < 16; m++)
        {
            EKC.add(m, "Favor");
        }//add 4 "Favor" cards to EKC deck
        for(int n = 16; n < 20; n++)
        {
            EKC.add(n, "Hairy Potato Cat");
        }//add 4 "Hairy Potato Cat" cards to EKC deck
        for(int o = 20; o < 24; o++)
        {
            EKC.add(o, "Cattermelon");
        }//add 4 "Cattermelon" cards to EKC deck
        for(int p = 24; p < 28; p++)
        {
            EKC.add(p, "Rainbow-Ralphing Cat");
        }//add 4 "Rainbow-Ralphing Cat" cards to EKC deck
        for(int q = 28; q < 32; q++)
        {
            EKC.add(q, "Beard Cat");
        }//add 4 "Beard Cat" cards to EKC deck
        for(int r = 32; r < 36; r++)
        {
            EKC.add(r, "Taco Cat");
        }//add 4 "Taco Cat" cards to EKC deck
        for(int s = 36; s < 41; s++)
        {
            EKC.add(s, "See the Future");
        }//add 4 "See the Future" cards to EKC deck
        for(int t = 41; t < 46; t++)
        {
            EKC.add(t, "Nope");
        }//add 5 "Nope" cards to EKC deck
        if (size < 2 || size > 4)
        {
            System.exit(0);
        }//exits program if the # of players is not between 2 and 4
        if (size == 2)
        {
            EKC.add(46, "Exploding Kittens");
            Shuffle();
            firstPlayerStart();
        }//if # of players = 2, then add 1 "Exploding Kittens" card to EKC deck
        if (size == 3)
        {
            EKC.add(46, "Exploding Kittens");
            EKC.add(47, "Exploding Kittens");
            Shuffle();
            firstPlayerStart();
        }//if # of players = 3, then add 2 "Exploding Kittens" card to EKC deck
        if (size == 4)
        {
            EKC.add(46, "Exploding Kittens");
            EKC.add(47, "Exploding Kittens");
            EKC.add(48, "Exploding Kittens");
            Shuffle();
            firstPlayerStart();
        }//if # of players = 4, then add 3 "Exploding Kittens" card to EKC deck
    }

    public void Shuffle()
    {
        Collections.shuffle(EKC); 
    }//shuffles EKC deck 

    public void player1Shuffle()
    {
        Collections.shuffle(player1);
    }//shuffles hand of player1 

    public void player2Shuffle()
    {
        Collections.shuffle(player2);
    }//shuffles hand of player2 

    public void player3Shuffle()
    {
        Collections.shuffle(player3);
    }//shuffles hand of player3 

    public void player4Shuffle()
    {
        Collections.shuffle(player4);
    }//shuffles hand of player4 

    public void defuseAndShuffle()
    {
        if (size == 2)
        {
            for (int u = 39; u < 39+(6-size); u++)
            {
                EKC.add(u, "Defuse");
            }
            Collections.shuffle(EKC);
        }// add 4 defuse to EKC deck to index 39 - 42 and shuffle EKC deck if # of players = 2
        if (size == 3)
        {
            for (int u = 36; u < 36+(6-size); u++)
            {
                EKC.add(u, "Defuse");
            }
            Collections.shuffle(EKC);
        }// add 3 defuse to EKC deck to index 36 - 38 and shuffle EKC deck if # of players = 3
        if (size == 4)
        {
            for (int u = 32; u < 32+(6-size); u++)
            {
                EKC.add(u, "Defuse");
            }
            Collections.shuffle(EKC);
        }// add 2 defuse to EKC deck to index 32 - 33 and shuffle EKC deck if # of players = 4
    }

    public void firstPlayerStart()
    {
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        Random rand = new Random();
        for (int i = 0; i < 4; i++)
        {
            if (EKC.get(i).equals("Exploding Kittens") && !EKC.get(i+1).equals("Exploding Kittens"))
            {
                player1.add(EKC.get(i+1));
                EKC.remove(EKC.get(i+1));
            }//if the first card is EK second card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && !EKC.get(i+2).equals("Exploding Kittens"))
            {
                player1.add(EKC.get(i+2));
                EKC.remove(EKC.get(i+2));
            }//if the first card and second card is EK and if the third card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && EKC.get(i+2).equals("Exploding Kittens") && !EKC.get(i+3).equals("Exploding Kittens"))
            {
                player1.add(EKC.get(i+3));
                EKC.remove(EKC.get(i+3));
            }//if the first card and second card and third card is EK and if the fourth card is not an EK, then draw a card
            if (!EKC.get(i).equals("Exploding Kittens"))
            {
                player1.add(EKC.get(i));
                EKC.remove(i);
            }//if the first card is not an EK, then draw a card
        }//gets 4 cards for p1
        player1.add("Defuse"); //adds a defuse to p1's hand
        player1Shuffle();
        try 
        {
            FileWriter fw = new FileWriter(p1, true);
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[Original] Hand 1: " + player1);
            bw.close();
        }//writes p1's hand into file
        catch(IOException e)
        {
            System.out.println("Error " + e);
        }//checks and displays error if there is an error
        secondPlayerStart();
    }//ends player1's method for getting their hand

    public void secondPlayerStart()
    {
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        Random rand = new Random();
        for (int i = 0; i < 4; i++)
        {
            if (EKC.get(i).equals("Exploding Kittens") && !EKC.get(i+1).equals("Exploding Kittens"))
            {
                player2.add(EKC.get(i+1));
                EKC.remove(EKC.get(i+1));
            }//if the first card is EK second card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && !EKC.get(i+2).equals("Exploding Kittens"))
            {
                player2.add(EKC.get(i+2));
                EKC.remove(EKC.get(i+2));
            }//if the first card and second card is EK and if the third card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && EKC.get(i+2).equals("Exploding Kittens") && !EKC.get(i+3).equals("Exploding Kittens"))
            {
                player2.add(EKC.get(i+3));
                EKC.remove(EKC.get(i+3));
            }//if the first card and second card and third card is EK and if the fourth card is not an EK, then draw a card
            if (!EKC.get(i).equals("Exploding Kittens"))
            {
                player2.add(EKC.get(i));
                EKC.remove(i);
            }//if the first card is not an EK, then draw a card
        }//gets 4 cards for p2
        player2.add("Defuse");
        player2Shuffle();
        try 
        {
            FileWriter fw = new FileWriter(p2, true);
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[Original] Hand 2: " + player2);
            bw.close();
        }//writes p2's hand into file
        catch(IOException e)
        {
            System.out.println("Error " + e);
        }//checks and displays error if there is an error
        if (size == 3 || size == 4)
        {
            thirdPlayerStart();
        }//if there are three or four players, then go form p3's hand
        else
        {
            defuseAndShuffle();
            firstPlayer("null","null","null"); 
        }//if there are only two players, then go to defuseAndShuffle() and start the game with p1 after
    }//ends player2's method for getting their hand

    public void thirdPlayerStart()
    {
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        Random rand = new Random();
        for (int i = 0; i < 4; i++)
        {
            if (EKC.get(i).equals("Exploding Kittens") && !EKC.get(i+1).equals("Exploding Kittens"))
            {
                player3.add(EKC.get(i+1));
                EKC.remove(EKC.get(i+1));
            }//if the first card is EK second card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && !EKC.get(i+2).equals("Exploding Kittens"))
            {
                player3.add(EKC.get(i+2));
                EKC.remove(EKC.get(i+2));
            }//if the first card and second card is EK and if the third card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && EKC.get(i+2).equals("Exploding Kittens") && !EKC.get(i+3).equals("Exploding Kittens"))
            {
                player3.add(EKC.get(i+3));
                EKC.remove(EKC.get(i+3));
            }//if the first card and second card and third card is EK and if the fourth card is not an EK, then draw a card
            if (!EKC.get(i).equals("Exploding Kittens"))
            {
                player3.add(EKC.get(i));
                EKC.remove(i);
            }//if the first card is not an EK, then draw a card
        }//gets 4 cards for p3
        player3.add("Defuse");
        player3Shuffle();
        try 
        {
            FileWriter fw = new FileWriter(p3, true);
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[Original] Hand 3: " + player3);
            bw.close();
        }//writes p3's hand into file
        catch(IOException e)
        {
            System.out.println("Error " + e);
        }//checks and displays error if there is an error
        if (size == 4)
        {
            fourthPlayerStart();
        }//if there are four players, then go form p4's hand
        else
        {
            defuseAndShuffle();
            firstPlayer("null","null","null"); 
        }//if there are two or three players, then go to defuseAndShuffle() and start the game with p1 after
    }//ends player3's method for getting their hand

    public void fourthPlayerStart()
    {
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        Random rand = new Random();
        for (int i = 0; i < 4; i++)
        {
            if (EKC.get(i).equals("Exploding Kittens") && !EKC.get(i+1).equals("Exploding Kittens"))
            {
                player4.add(EKC.get(i+1));
                EKC.remove(EKC.get(i+1));
            }//if the first card is EK second card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && !EKC.get(i+2).equals("Exploding Kittens"))
            {
                player4.add(EKC.get(i+2));
                EKC.remove(EKC.get(i+2));
            }//if the first card and second card is EK and if the third card is not an EK, then draw a card
            if (EKC.get(i).equals("Exploding Kittens") && EKC.get(i+1).equals("Exploding Kittens") && EKC.get(i+2).equals("Exploding Kittens") && !EKC.get(i+3).equals("Exploding Kittens"))
            {
                player4.add(EKC.get(i+3));
                EKC.remove(EKC.get(i+3));
            }//if the first card and second card and third card is EK and if the fourth card is not an EK, then draw a card
            if (!EKC.get(i).equals("Exploding Kittens"))
            {
                player4.add(EKC.get(i));
                EKC.remove(i);
            }//if the first card is not an EK, then draw a card
        }//gets 4 cards for p4
        player4.add("Defuse");
        player4Shuffle();
        try 
        {
            FileWriter fw = new FileWriter(p4, true);
            BufferedWriter bw = new BufferedWriter(fw);
            bw.write("[Original] Hand 4: " + player4);
            bw.close();
        }//writes p4's hand into file
        catch(IOException e)
        {
            System.out.println("Error " + e);
        }//checks and displays error if there is an error
        defuseAndShuffle();
        firstPlayer("null","null","null");
    }//ends player4's method for getting their hand

    public void threeCatEffect(int player, int pSteal)
    {
        Scanner scan = new Scanner(System.in);
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        String strSteal = "";
        switch (pSteal)
        {
            case 1:
            System.out.println("Type a card that you want to steal from Player " + pSteal);
            strSteal = scan.nextLine();
            if (player1.contains(strSteal) && player == 2 || player == 02)
            {
                player1.remove(strSteal);
                player2.add(strSteal);
                if (player == 2)
                {
                    secondPlayer("null","null","null");
                }//go back to secondPlayer after effect 
                else if (player == 02)
                {
                    secondPlayerAttacked("null","null","null");
                }//go back to secondPlayerAttacked after effect 
            }//p2 stealing a card from p1 if p1 has the card p2 wants
            else if (player1.contains(strSteal) && player == 3 || player == 03)
            {
                player1.remove(strSteal);
                player3.add(strSteal);
                if (player == 3)
                {
                    thirdPlayer("null","null","null");
                }//go back to thirdPlayer after effect 
                else if (player == 03)
                {
                    thirdPlayerAttacked("null","null","null");
                }//go back to thirdPlayerAttacked after effect 
            }//p3 stealing a card from p1 if p1 has the card p3 wants
            else if (player1.contains(strSteal) && player == 4 || player == 04)
            {
                player1.remove(strSteal);
                player4.add(strSteal);
                if (player == 4)
                {
                    fourthPlayer("null","null","null");
                }//go back to fourthPlayer after effect 
                else if (player == 04)
                {
                    fourthPlayerAttacked("null","null","null");
                }//go back to fourthPlayerAttacked after effect 
            }//p4 stealing a card from p1 if p1 has the card p4 wants
            else
            {
                System.out.println("You got nothing.");
            }//Print "You got nothing" if p1 doesn’t have the card the other player wants
            break;
            case 2:
            System.out.println("Type a card that you want to steal from Player " + pSteal);
            strSteal = scan.nextLine();
            if (player2.contains(strSteal) && player == 1 || player == 01)
            {
                player2.remove(strSteal);
                player1.add(strSteal);
                if (player == 1)
                {
                    firstPlayer("null","null","null");
                }//go back to firstPlayer after effect 
                else if (player == 01)
                {
                    firstPlayerAttacked("null","null","null");
                }//go back to firstPlayerAttacked after effect 
            }//p1 stealing a card from p2 if p2 has the card p1 wants
            else if (player2.contains(strSteal) && player == 3 || player == 03)
            {
                player2.remove(strSteal);
                player3.add(strSteal);
                if (player == 3)
                {
                    thirdPlayer("null","null","null");
                }//go back to thirdPlayer after effect 
                else if (player == 03)
                {
                    thirdPlayerAttacked("null","null","null");
                }//go back to thirdPlayerAttacked after effect 
            }//p3 stealing a card from p2 if p2 has the card p3 wants
            else if (player2.contains(strSteal) && player == 4 || player == 04)
            {
                player2.remove(strSteal);
                player4.add(strSteal);
                if (player == 4)
                {
                    fourthPlayer("null","null","null");
                }//go back to fourthPlayer after effect 
                else if (player == 04)
                {
                    fourthPlayerAttacked("null","null","null");
                }//go back to fourthPlayerAttacked after effect 
            }//p4 stealing a card from p2 if p2 has the card p4 wants
            else
            {
                System.out.println("You got nothing.");
            }//Print "You got nothing" if p2 doesnt have the card the other player wants
            break;
            case 3:
            System.out.println("Type a card that you want to steal from Player " + pSteal);
            strSteal = scan.nextLine();
            if (player3.contains(strSteal) && player == 1 && player == 01)
            {
                player3.remove(strSteal);
                player1.add(strSteal);                
                if (player == 1)
                {
                    firstPlayer("null","null","null");
                }//go back to firstPlayer after effect 
                else if (player == 01)
                {
                    firstPlayerAttacked("null","null","null");
                }//go back to firstPlayerAttacked after effect 
            }//p1 stealing a card from p3 if p3 has the card p1 wants
            if (player3.contains(strSteal) && player == 2 && player == 02)
            {
                player3.remove(strSteal);
                player2.add(strSteal);
                if (player == 2)
                {
                    secondPlayer("null","null","null");
                }//go back to secondPlayer after effect 
                else if (player == 02)
                {
                    secondPlayerAttacked("null","null","null");
                }//go back to secondPlayerAttacked after effect 
            }//p2 stealing a card from p3 if p3 has the card p2 wants
            if (player3.contains(strSteal) && player == 4 && player == 04)
            {
                player3.remove(strSteal);
                player4.add(strSteal);
                if (player == 4)
                {
                    fourthPlayer("null","null","null");
                }//go back to fourthPlayer after effect 
                else if (player == 04)
                {
                    fourthPlayerAttacked("null","null","null");
                }//go back to fourthPlayerAttacked after effect 
            }//p4 stealing a card from p3 if p3 has the card p4 wants
            else
            {
                System.out.println("You got nothing.");
            }//Print "You got nothing" if p3 doesnt have the card the other player wants
            break;
            case 4:
            System.out.println("Type a card that you want to steal from Player " + pSteal);
            strSteal = scan.nextLine();
            if (player4.contains(strSteal) && player == 1 && player == 01)
            {
                player4.remove(strSteal);
                player1.add(strSteal);
                if (player == 1)
                {
                    firstPlayer("null","null","null");
                }//go back to firstPlayer after effect 
                else if (player == 01)
                {
                    firstPlayerAttacked("null","null","null");
                }//go back to firstPlayerAttacked after effect 
            }//p1 stealing a card from p4 if p4 has the card p1 wants
            else if (player4.contains(strSteal) && player == 2 && player == 02)
            {
                player4.remove(strSteal);
                player2.add(strSteal);
                if (player == 2)
                {
                    secondPlayer("null","null","null");
                }//go back to secondPlayer after effect 
                else if (player == 02)
                {
                    secondPlayerAttacked("null","null","null");
                }//go back to secondPlayerAttacked after effect 
            }//p2 stealing a card from p4 if p4 has the card p2 wants
            else if (player4.contains(strSteal) && player == 3 && player == 03)
            {
                player4.remove(strSteal);
                player3.add(strSteal);
                if (player == 3)
                {
                    thirdPlayer("null","null","null");
                }//go back to thirdPlayer after effect 
                else if (player == 03)
                {
                    thirdPlayerAttacked("null","null","null");
                }//go back to thirdPlayerAttacked after effect 
            }//p3 stealing a card from p4 if p4 has the card p3 wants
            else
            {
                System.out.println("You got nothing.");
            }//Print "You got nothing" if p4 doesnt have the card the other player wants
            break;
        }//ends switch statement
    }//ends method for effects for having and using three of the same kind of cat

    public void firstPlayerAttacked(String twoChoice, String threeChoice, String fourChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            try 
            {
                FileWriter fw = new FileWriter(p1, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 1: " + player1);
                bw.close();
            }//write p1's hand into p1 file
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }//checks and displays error if there is an error
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 1 - Play or Draw (d1) or h (for help) or ragequit: ");
            oneChoice = scan.nextLine();
            if (oneChoice.equals("ragequit"))
            {
                System.exit(0);
            }//exit system if user types "ragequit"
            if (oneChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }//writes help instructions into file if player needs help ("h")
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }//checks and displays error if there is an error
                System.out.println();
            }//help instructions
            if (!oneChoice.equals("d1") && player1.contains(oneChoice))
            {
                player1.remove(oneChoice);
                discard.add(n, oneChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }//writes discard pile into discard file
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }//checks and displays error if there is an error
                System.out.println();
                n++;
                if (player2.contains("Nope") || player3.contains("Nope") || player4.contains("Nope"))
                {
                    if (!oneChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 1's " + oneChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 2:
                                if (player2.contains("Nope"))
                                {
                                    player2.remove("Nope");
                                    discard.add("Nope");
                                }//remove p2’s nope after they use it and add to discard pile
                                break;
                                case 3:
                                if (player3.contains("Nope"))
                                {
                                    player3.remove("Nope");
                                    discard.add("Nope");
                                }//remove p3’s nope after they use it and add to discard pile
                                break;
                                case 4:
                                if (player4.contains("Nope"))
                                {
                                    player4.remove("Nope");
                                    discard.add("Nope");
                                }//remove p4’s nope after they use it and add to discard pile
                                break;
                            }//ends nopeChoice switch statement
                            nopeChoice = 0;
                            firstPlayerAttacked("null","null","null");
                        }//ends if statement for nopeChoice != 0
                    }//ends if statement for nope outcomes
                    else
                    {
                        if (oneChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 1's " + oneChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 2:
                                    if (player2.contains("Nope"))
                                    {
                                        player2.remove("Nope");
                                        discard.add("Nope");
                                    }//remove p2’s nope after they use it and add to discard pile
                                    break;
                                    case 3:
                                    if (player3.contains("Nope"))
                                    {
                                        player3.remove("Nope");
                                        discard.add("Nope");
                                    }//remove p3’s nope after they use it and add to discard pile
                                    break;
                                    case 4:
                                    if (player4.contains("Nope"))
                                    {
                                        player4.remove("Nope");
                                        discard.add("Nope");
                                    }//remove p4’s nope after they use it and add to discard pile
                                    break;
                                }
                                nopeChoice = 0;
                                firstPlayerAttacked("null","null","null");
                            }//ends if statement for nopeChoice != 0
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 2:
                                    if (!player2.contains("Exploding Kittens"))
                                    {
                                        secondPlayer(oneChoice,"null","null");
                                    }//goes to p2 method if p1 wants a favor from p2
                                    break;
                                    case 3:
                                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                                    {   
                                        thirdPlayer(oneChoice,"null","null");
                                    }//goes to p3 method if p1 wants a favor from p3
                                    break;
                                    case 4:
                                    if (size == 4 && !player4.contains("Exploding Kittens"))
                                    {
                                        fourthPlayer(oneChoice,"null","null");
                                    }//goes to p4 method if p1 wants a favor from p4
                                    break;
                                }//ends numFavor switch statement for execution of favor effect
                                firstPlayerAttacked("null","null","null");
                            }//ends if statement for nopeChoice == 0 and for execution of favor effect
                            System.out.println();
                        }//ends else statement for when oneChoice is favor
                    }//ends if statement for when at least one other player has a nope card
                }//asks if p2, p3, or p4 wants to use nope when p1 uses effect
	if (oneChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                    switch (numFavor)
                    {
                        case 2:
                        if (!player2.contains("Exploding Kittens"))
                        {
                            secondPlayer(oneChoice,"null","null");
                        }//go to secondPlayer if p1 wants a favor from p2
                        break;
                        case 3:
                        if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                        {   
                            thirdPlayer(oneChoice,"null","null");
                        }//go to thirdPlayer if p1 wants a favor from p3
                        break;
                        case 4:
                        if (size == 4 && !player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer(oneChoice,"null","null");
                        }//go to fourthPlayer if p1 wants a favor from p4
                        break;
                    }
                }// if choice is "Favor", then ask for a favor from any player
                if (oneChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    firstPlayer("null","null","null");
                }//if choice is "Nope", deny them 
                if (oneChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p1, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }//write the first three cards in the deck into p1’s hand file
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }//checks and displays error if there is an error
                }//if choice is "See the Future", then see first three cards
                if (oneChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }//goes to secondPlayer 
                    else if (size == 3 || size == 4 && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }//goes to thirdPlayer if secondPlayer is dead
                    else if (size == 4 && player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }//goes to fourthPlayer if secondPlayer and thirdPlayer is dead
                }//if choice is "Skip", then skip p1's turn and move on to next player
                if (oneChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer(oneChoice,"null","null");
                    }//goes to secondPlayer & secondPlayer is "Attacked"
                    else if (size == 3 || size == 4 && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer(oneChoice,"null","null");
                    }//goes to thirdPlayer & thirdPlayer is "Attacked"
                    else if (size == 4 && player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer(oneChoice,"null","null");
                    }//goes to fourthPlayer & fourthPlayer is "Attacked"
                }//if choice is "Attack", then go to next player & next player has 2 turns
                if (oneChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }//if choice is "Shuffle", then shuffle deck
                if (oneChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Hairy Potato Cat"))
                    {
                        player1.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }//gives Hairy Potato Cat back to user if they only have and use 1 Hairy Potato Cat
                    else if (player1.contains("Hairy Potato Cat"))
                    {
                        player1.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player1.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }//ends switch statement for when p1 has and uses 2 Hairy Potato Cat
                        }//checks if p1 has any more than 2 Hairy Potato Cat
                        else if (player1.contains("Hairy Potato Cat"))
                        {
                            player1.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(01,pSteal);
                        }//if p1 has and uses 3 Hairy Potato Cat, then go to threeCatEffect method
                    }//checks if p1 has 2 Hairy Potato Cat
                }//when choice is Hairy Potato Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card 
                if (oneChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player1.contains("Cattermelon"))
                    {
                        player1.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }//gives Cattermelon back to user if they only have and use 1 Cattermelon
                    else if (player1.contains("Cattermelon"))
                    {
                        player1.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player1.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }//ends switch statement for when p1 has and uses 2 Cattermelon
                        }//checks if p1 has any more than 2 Cattermelon
                        else if (player1.contains("Cattermelon"))
                        {
                            player1.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(01,pSteal);
                        }//if p1 has and uses 3 Cattermelon, then go to threeCatEffect method
                    }//checks if p1 has 2 Cattermelon
                }//when choice is Cattermelon, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card 
                if (oneChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Rainbow-Ralphing Cat"))
                    {
                        player1.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }//gives Rainbow-Ralphing Cat back to p1 if they only have and use 1 Hairy Potato Cat
                    else if (player1.contains("Rainbow-Ralphing Cat"))
                    {
                        player1.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player1.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }//ends switch statement for when p1 has and uses 2 Rainbow-Ralphing Cat
                        }//checks if p1 has any more than 2 Rainbow-Ralphing Cat
                        else if (player1.contains("Rainbow-Ralphing Cat"))
                        {
                            player1.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(01,pSteal);
                        }//if p1 has and uses 3 Rainbow-Ralphing Cat, then go to threeCatEffect method
                    }//checks if p1 has 2 Rainbow-Ralphing Cat
                }//when choice is Rainbow Ralphing-Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card 
                if (oneChoice.equals("Beard Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Beard Cat"))
                    {
                        player1.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }//gives Beard Cat back to p1 if they only have and use 1 Beard Cat
                    else if (player1.contains("Beard Cat"))
                    {
                        player1.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player1.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }//ends switch statement for when p1 has and uses 2 Beard Cat
                        }//checks if p1 has any more than 2 Beard Cat
                        else if (player1.contains("Beard Cat"))
                        {
                            player1.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(01,pSteal);
                        }//if p1 has and uses 3 Beard Cat, then go to threeCatEffect method
                    }//checks if p1 has 2 Beard Cat
                }//when choice is Beard Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card 
                if (oneChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Taco Cat"))
                    {
                        player1.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }//gives Taco Cat back to p1 if they only have and use 1 Taco Cat
                    else if (player1.contains("Taco Cat"))
                    {
                        player1.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player1.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }//ends switch statement for when p1 has and uses 2 Taco Cat
                        }//checks if p1 has any more than 2 Taco Cat
                        else if (player1.contains("Taco Cat"))
                        {
                            player1.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(01,pSteal);
                        }//if p1 has and uses 3 Taco Cat, then go to threeCatEffect method
                    }//checks if p1 has 2 Taco Cat
                }//when choice is Taco Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card 
            }//ends if statement containing the effects
        }while (!oneChoice.equals("d1"));
        if (oneChoice.equals("d1"))
        {
            player1.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player1.contains("Exploding Kittens"))
            {
                System.out.println("Player 1 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player1.contains("Defuse") && choice1.equals("y"))
                {
                    player1.remove("Defuse");
                    player1.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p1, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 1: " + player1);
                        bw.close();
                    }//writes p1's hand into p1 hand file
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }//checks and displays error if there is an error
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }//shifts the cards at & after where the EK is placed over by one index towards the bottom of the deck
                    EKC.add(choice2, "Exploding Kittens");
                }//if p1 uses defuse on EK, then they are saved and they can put EK anywhere in deck
                else
                {
                    System.out.println("Player 1 has exploded.");
                    System.out.println();
                    status();
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }//go to secondPlayer
                    else if (player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }//go to thirdPlayer
                    else if (player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }//go to fourthPlayer
                }//p1 exploded, checks if others exploded, and goes on to next player
            }//if p1 draws an EK, then they can defuse or they can't defuse
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p1, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 1: " + player1);
                    bw.close();
                }//writes p1's hand into p1 hand file
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }//checks and displays error if there is an error
                firstPlayer("null", "null", "null");
            }//update p1's hand and go to firstPlayer
        }//ends p1's drawing phase
    }//ends p1's attacked turn and goes to p1’s normal turn

    public void firstPlayerFavored(String twoChoice, String threeChoice, String fourChoice)
    {
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        Scanner scan = new Scanner(System.in);
        if (twoChoice.equals("Favor"))
        {
            System.out.println("P1, Choose a number to give a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
            int numSteal = scan.nextInt();
            player2.add(player1.get(numSteal));
            player1.remove(numSteal);
            secondPlayer("null","null","null");
        }//if p2's choice is Favor & p2 chose p1, then p1 gives a card of p1's choosing to p2
        if (threeChoice.equals("Favor"))
        {
            System.out.println("P1, choose a number to give a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
            int numSteal = scan.nextInt();
            player3.add(player1.get(numSteal));
            player1.remove(numSteal);
            thirdPlayer("null","null","null");
        }//if p2's choice is Favor & p2 chose p1, then p1 gives a card of p1's choosing to p2
        if (fourChoice.equals("Favor"))
        {
            System.out.println("P1, Choose a number to give a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
            int numSteal = scan.nextInt();
            player4.add(player1.get(numSteal));
            player1.remove(numSteal);
            fourthPlayer("null","null","null");
        }//if p2's choice is Favor & p2 chose p1, then p1 gives a card of p1's choosing to p2
    }//end firstPlayerFavored method

    public void firstPlayer(String twoChoice, String threeChoice, String fourChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        System.out.println("---------------------------------------------");
        do
        {
            if (twoChoice.equals("Attack") || threeChoice.equals("Attack") || fourChoice.equals("Attack"))
            {
                firstPlayerAttacked(twoChoice, threeChoice, fourChoice);
            }//if p2 or p3 or p4 choice is Attack and the next turn is p1, then go to p1 attacked method
            if (twoChoice.equals("Favor") || threeChoice.equals("Favor") || fourChoice.equals("Favor"))
            {
                firstPlayerFavored(twoChoice, threeChoice, fourChoice);
            }//if p2 or p3 or p4 choice is Favor and wants a favor from p1, then go to p1 favored method
            try 
            {
                FileWriter fw = new FileWriter(p1, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 1: " + player1);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }//write p1's hand into p1 file
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 1 - Play or Draw (d1) or h (for help) or ragequit: ");
            oneChoice = scan.nextLine();
            if (oneChoice.equals("ragequit"))
            {
                System.exit(0);
            }//exit system if user types "ragequit"
            if (oneChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }//help instructions
            if (!oneChoice.equals("d1") && player1.contains(oneChoice))
            {
                player1.remove(oneChoice);
                discard.add(n, oneChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }//writes discard pile into discard file
                System.out.println();
                n++;
                if (player2.contains("Nope") || player3.contains("Nope") || player4.contains("Nope"))
                {
                    if (!oneChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 1's " + oneChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 2:
                                if (player2.contains("Nope"))
                                {
                                    player2.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 3:
                                if (player3.contains("Nope"))
                                {
                                    player3.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 4:
                                if (player4.contains("Nope"))
                                {
                                    player4.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            firstPlayer("null","null","null");
                        }
                    }
                    else
                    {
                        if (oneChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 1's " + oneChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 2:
                                    if (player2.contains("Nope"))
                                    {
                                        player2.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 3:
                                    if (player3.contains("Nope"))
                                    {
                                        player3.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 4:
                                    if (player4.contains("Nope"))
                                    {
                                        player4.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                firstPlayer("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 2:
                                    if (!player2.contains("Exploding Kittens"))
                                    {
                                        secondPlayer(oneChoice,"null","null");
                                    }
                                    break;
                                    case 3:
                                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                                    {   
                                        thirdPlayer(oneChoice,"null","null");
                                    }
                                    break;
                                    case 4:
                                    if (size == 4 && !player4.contains("Exploding Kittens"))
                                    {
                                        fourthPlayer(oneChoice,"null","null");
                                    }
                                    break;
                                }
                                firstPlayer("null","null","null");
                            }
                            System.out.println();
                        }
                    }
                }//asks if p2, p3, or p4 wants to use nope when p1 uses effect
                if (oneChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    firstPlayer("null","null","null");
                }//if choice is "Nope", deny them 
                if (oneChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p1, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }//if choice is "See the Future", then see first three cards
                if (oneChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (size == 3 && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (size == 4 && player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                }//if choice is "Skip", then skip p1's turn and move on to next player
                if (oneChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer(oneChoice,"null","null");
                    }
                    else if (size == 3 && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer(oneChoice,"null","null");
                    }
                    else if (size == 4 && player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer(oneChoice,"null","null");
                    }
                }//if choice is "Attack", then go to next player & next player has 2 turns
                if (oneChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();    
                        switch (numFavor)
                        {
                            case 2:
                            if (!player2.contains("Exploding Kittens"))
                            {
                                secondPlayer(oneChoice,"null","null");
                            }
                            break;
                            case 3:
                            if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                            {   
                                thirdPlayer(oneChoice,"null","null");
                            }
                            break;
                            case 4:
                            if (size == 4 && !player4.contains("Exploding Kittens"))
                            {
                                fourthPlayer(oneChoice,"null","null");
                            }
                            break;
                        }
                }//if choice is "Favor", then ask for a favor from any player
                if (oneChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }//if choice is "Shuffle", then shuffle deck
                if (oneChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Hairy Potato Cat"))
                    {
                        player1.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player1.contains("Hairy Potato Cat"))
                    {
                        player1.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player1.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player1.contains("Hairy Potato Cat"))
                        {
                            player1.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(1,pSteal);
                        }
                    }
                }//when choice is Hairy Potato Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card
                if (oneChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player1.contains("Cattermelon"))
                    {
                        player1.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player1.contains("Cattermelon"))
                    {
                        player1.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player1.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player1.contains("Cattermelon"))
                        {
                            player1.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(1,pSteal);
                        }
                    }
                }//when choice is Cattermelon, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card
                if (oneChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Rainbow-Ralphing Cat"))
                    {
                        player1.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player1.contains("Rainbow-Ralphing Cat"))
                    {
                        player1.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player1.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player1.contains("Rainbow-Ralphing Cat"))
                        {
                            player1.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(1,pSteal);
                        }
                    }
                }//when choice is Rainbow-Ralphing Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card
                if (oneChoice.equals("Beard Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Beard Cat"))
                    {
                        player1.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player1.contains("Beard Cat"))
                    {
                        player1.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player1.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player1.contains("Beard Cat"))
                        {
                            player1.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(1,pSteal);
                        }
                    }
                }//when choice is Beard Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card
                if (oneChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player1.contains("Taco Cat"))
                    {
                        player1.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player1.contains("Taco Cat"))
                    {
                        player1.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player1.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player1.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player1.contains("Taco Cat"))
                        {
                            player1.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(1,pSteal);
                        }
                    }
                }//when choice is Taco Cat, if only 1 -> do nothing, if 2 -> steal random card of choosing, if 3 -> steal any card
            }//ends if statement containing the effects
        }while (!oneChoice.equals("d1"));
        if (oneChoice.equals("d1"))
        {
            player1.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player1.contains("Exploding Kittens"))
            {
                System.out.println("Player 1 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player1.contains("Defuse") && choice1.equals("y"))
                {
                    player1.remove("Defuse");
                    player1.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p1, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 1: " + player1);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (size == 3 && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (size == 4 && player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                    else
                    {
                        System.out.println("Congratulations, Player 1 is the winner!");
                    }
                }//if p1 has defuse, they can defuse the EK and put the EK anywhere in the deck
                else
                {
                    System.out.println("Player 1 has exploded.");
                    System.out.println();
                    status();
                    if (!player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                }//if p1 doesn’t have a defuse, they explode
            }//checks if p1 drew an EK or not
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p1, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 1: " + player1);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                if (!player2.contains("Exploding Kittens"))
                {
                    secondPlayer("null","null","null");
                }
                else if (player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                {
                    thirdPlayer("null","null","null");
                }
                else if (player2.contains("Exploding Kittens") && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                {
                    fourthPlayer("null","null","null");
                }
                else
                {
                    System.out.println("Congratulations, Player 1 is the winner!");
                }
            }//update p1’s hand and go to next player’s turn
        }//ends p1’s drawing phase
    }//ends p1’s turn

    public void secondPlayerFavored(String oneChoice, String threeChoice, String fourChoice)
    {
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        Scanner scan = new Scanner(System.in);
        if (oneChoice.equals("Favor"))
        {
            System.out.println("P2, choose a number to give a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
            int numSteal = scan.nextInt();
            player1.add(player2.get(numSteal));
            player2.remove(numSteal);
            firstPlayer("null","null","null");
        }
        if (threeChoice.equals("Favor"))
        {
            System.out.println("P2, choose a number to give a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
            int numSteal = scan.nextInt();
            player3.add(player2.get(numSteal));
            player2.remove(numSteal);
            thirdPlayer("null","null","null");
        }
        if (fourChoice.equals("Favor"))
        {
            System.out.println("P2, choose a number to give a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
            int numSteal = scan.nextInt();
            player4.add(player2.get(numSteal));
            player2.remove(numSteal);
            fourthPlayer("null","null","null");
        }
    }

    public void secondPlayerAttacked(String oneChoice, String threeChoice, String fourChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            try 
            {
                FileWriter fw = new FileWriter(p2, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 2: " + player2);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 2 - Play or Draw (d2) or h (for help) or ragequit: ");
            twoChoice = scan.nextLine();
            if (twoChoice.equals("ragequit"))
            {
                System.exit(0);
            }
            if (twoChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }
            if (!twoChoice.equals("d2") && player2.contains(twoChoice))
            {
                player2.remove(twoChoice);
                discard.add(n, twoChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
                n++;
                if (player1.contains("Nope") || player3.contains("Nope") || player4.contains("Nope"))
                {
                    if (!twoChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 2's " + twoChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 1:
                                if (player1.contains("Nope"))
                                {
                                    player1.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 3:
                                if (player3.contains("Nope"))
                                {
                                    player3.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 4:
                                if (player4.contains("Nope"))
                                {
                                    player4.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            secondPlayerAttacked("null","null","null");
                        }
                    }
                    else
                    {
                        if (twoChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 2's " + twoChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 1:
                                    if (player1.contains("Nope"))
                                    {
                                        player1.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 3:
                                    if (player3.contains("Nope"))
                                    {
                                        player3.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 4:
                                    if (player4.contains("Nope"))
                                    {
                                        player4.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                secondPlayerAttacked("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 1:
                                    if (!player1.contains("Exploding Kittens"))
                                    {
                                        firstPlayer(twoChoice,"null","null");
                                    }
                                    break;
                                    case 3:
                                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                                    {   
                                        thirdPlayer("null",twoChoice,"null");
                                    }
                                    break;
                                    case 4:
                                    if (size == 4 && !player4.contains("Exploding Kittens"))
                                    {
                                        fourthPlayer("null",twoChoice,"null");
                                    }
                                    break;
                                }
                                secondPlayerAttacked("null","null","null");
                            }
                            System.out.println();
                        }
                    }
                }
                if (twoChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    secondPlayerAttacked("null","null","null");
                }
                if (twoChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p2, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }
                if (twoChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (size == 3 && !player3.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (size == 2)
                    {
                        firstPlayer("null","null","null");
                    }
                }
                if (twoChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null",twoChoice,"null");
                    }
                    else if (size == 3 && !player3.contains("Exploding Kittens"))
                    {
                        firstPlayer(twoChoice,"null","null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null",twoChoice,"null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer(twoChoice,"null","null");
                    }
                    else if (size == 2)
                    {
                        firstPlayer(twoChoice,"null","null");
                    }
                }
                if (twoChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }
                if (twoChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                    switch (numFavor)
                    {
                        case 1:
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer(twoChoice, "null", "null");
                        }
                        break;
                        case 3:
                        if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                        {   
                            thirdPlayer("null", twoChoice,"null");
                        }
                        break;
                        case 4:
                        if (size == 4 && !player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null", twoChoice,"null");
                        }
                        break;
                    }
                }//if choice is "Favor", then ask for a favor from any player
                if (twoChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player2.contains("Hairy Potato Cat"))
                    {
                        player2.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player2.contains("Hairy Potato Cat"))
                    {
                        player2.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player2.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Hairy Potato Cat"))
                        {
                            player2.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(02,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player2.contains("Cattermelon"))
                    {
                        player2.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player2.contains("Cattermelon"))
                    {
                        player2.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player2.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Cattermelon"))
                        {
                            player2.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(02,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player2.contains("Rainbow-Ralphing Cat"))
                    {
                        player2.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player2.contains("Rainbow-Ralphing Cat"))
                    {
                        player2.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player2.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Rainbow-Ralphing Cat"))
                        {
                            player2.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(02,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Beard Cat") && nopeChoice == 0)
                {
                    if (!player2.contains("Beard Cat"))
                    {
                        player2.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player2.contains("Beard Cat"))
                    {
                        player2.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player2.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Beard Cat"))
                        {
                            player2.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(02,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player2.contains("Taco Cat"))
                    {
                        player2.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player2.contains("Taco Cat"))
                    {
                        player2.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player2.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Taco Cat"))
                        {
                            player2.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(02,pSteal);
                        }
                    }
                }
            }
        }while (!twoChoice.equals("d2"));
        if (twoChoice.equals("d2"))
        {
            player2.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player2.contains("Exploding Kittens"))
            {
                System.out.println("Player 2 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player2.contains("Defuse") && choice1.equals("y"))
                {
                    player2.remove("Defuse");
                    player2.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p2, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 2: " + player2);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                }
                else
                {
                    System.out.println("Player 2 has exploded.");
                    System.out.println();
                    status();
                    if (size == 3)
                    {
                        if (!player3.contains("Exploding Kittens"))
                        {
                            thirdPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player3.contains("Exploding Kittens"))
                        {
                            thirdPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                    }
                }
            }
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p2, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 2: " + player2);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                secondPlayer("null", "null", "null");
            }
        }
    }

    public void secondPlayer(String oneChoice, String threeChoice, String fourChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            if (oneChoice.equals("Attack") || threeChoice.equals("Attack") || fourChoice.equals("Attack"))
            {
                secondPlayerAttacked(oneChoice, threeChoice, fourChoice);
            }
            if (oneChoice.equals("Favor") || threeChoice.equals("Favor") || fourChoice.equals("Favor"))
            {
                secondPlayerFavored(oneChoice, threeChoice, fourChoice);
            }
            try 
            {
                FileWriter fw = new FileWriter(p2, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 2: " + player2);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 2 - Play or Draw (d2) or h (for help) or ragequit: ");
            twoChoice = scan.nextLine();
            if (twoChoice.equals("ragequit"))
            {
                System.exit(0);
            }
            if (twoChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }
            if (!twoChoice.equals("d2") && player2.contains(twoChoice))
            {
                player2.remove(twoChoice);
                discard.add(n, twoChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
                n++;
                if (player1.contains("Nope") || player3.contains("Nope") || player4.contains("Nope"))
                {
                    if (!twoChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 2's " + twoChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 1:
                                if (player1.contains("Nope"))
                                {
                                    player1.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 3:
                                if (player3.contains("Nope"))
                                {
                                    player3.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 4:
                                if (player4.contains("Nope"))
                                {
                                    player4.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            secondPlayer("null","null","null");
                        }
                    }
                    else
                    {
                        if (twoChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 2's " + twoChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 1:
                                    if (player1.contains("Nope"))
                                    {
                                        player1.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 3:
                                    if (player3.contains("Nope"))
                                    {
                                        player3.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 4:
                                    if (player4.contains("Nope"))
                                    {
                                        player4.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                secondPlayer("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 1:
                                    if (!player1.contains("Exploding Kittens"))
                                    {
                                        firstPlayer(twoChoice,"null","null");
                                    }
                                    break;
                                    case 3:
                                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                                    {   
                                        thirdPlayer("null",twoChoice,"null");
                                    }
                                    break;
                                    case 4:
                                    if (size == 4 && !player4.contains("Exploding Kittens"))
                                    {
                                        fourthPlayer("null",twoChoice,"null");
                                    }
                                    break;
                                }
                                secondPlayer("null","null","null");
                            }
                            System.out.println();
                        }
                    }
                }
                if (twoChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    secondPlayer("null","null","null");
                }
                if (twoChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p2, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }
                if (twoChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (size == 3 && !player3.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (size == 2)
                    {
                        firstPlayer("null","null","null");
                    }
                }
                if (twoChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null",twoChoice,"null");
                    }
                    else if (size == 3 && !player3.contains("Exploding Kittens"))
                    {
                        firstPlayer(twoChoice,"null","null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null",twoChoice,"null");
                    }
                    else if (size == 4 && player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer(twoChoice,"null","null");
                    }
                    else if (size == 2)
                    {
                        firstPlayer(twoChoice,"null","null");
                    }
                }
                if (twoChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }
                if (twoChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                    switch (numFavor)
                    {
                        case 1:
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer(twoChoice, "null", "null");
                        }
                        break;
                        case 3:
                        if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                        {   
                            thirdPlayer("null", twoChoice,"null");
                        }
                        break;
                        case 4:
                        if (size == 4 && !player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null", twoChoice,"null");
                        }
                        break;
                    }
                }//if choice is "Favor", then ask for a favor from any player
                if (twoChoice.equals("Hairy Potato Cat"))
                {
                    if (!player2.contains("Hairy Potato Cat"))
                    {
                        player2.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player2.contains("Hairy Potato Cat"))
                    {
                        player2.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player2.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Hairy Potato Cat"))
                        {
                            player2.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(2,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Cattermelon"))
                {
                    if (!player2.contains("Cattermelon"))
                    {
                        player2.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player2.contains("Cattermelon"))
                    {
                        player2.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player2.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Cattermelon"))
                        {
                            player2.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(2,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Rainbow-Ralphing Cat"))
                {
                    if (!player2.contains("Rainbow-Ralphing Cat"))
                    {
                        player2.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player2.contains("Rainbow-Ralphing Cat"))
                    {
                        player2.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player2.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Rainbow-Ralphing Cat"))
                        {
                            player2.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(2,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Beard Cat"))
                {
                    if (!player2.contains("Beard Cat"))
                    {
                        player2.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player2.contains("Beard Cat"))
                    {
                        player2.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player2.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Beard Cat"))
                        {
                            player2.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(2,pSteal);
                        }
                    }
                }
                if (twoChoice.equals("Taco Cat"))
                {
                    if (!player2.contains("Taco Cat"))
                    {
                        player2.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player2.contains("Taco Cat"))
                    {
                        player2.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player2.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player2.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player2.contains("Taco Cat"))
                        {
                            player2.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(2,pSteal);
                        }
                    }
                }
            }
        }while (!twoChoice.equals("d2"));
        if (twoChoice.equals("d2"))
        {
            player2.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player2.contains("Exploding Kittens"))
            {
                System.out.println("Player 2 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player2.contains("Defuse") && choice1.equals("y"))
                {
                    player2.remove("Defuse");
                    player2.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p2, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 2: " + player2);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                    if (size == 3)
                    {
                        if (!player3.contains("Exploding Kittens"))
                        {
                            thirdPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else
                        {
                            System.out.println("Congratulations, Player 2 is the winner!");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player3.contains("Exploding Kittens"))
                        {
                            thirdPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else
                        {
                            System.out.println("Congratulations, Player 2 is the winner!");
                        }
                    }
                    else
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                    }
                }
                else
                {
                    System.out.println("Player 2 has exploded.");
                    System.out.println();
                    status();
                    if (size == 3)
                    {
                        if (!player3.contains("Exploding Kittens"))
                        {
                            thirdPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player3.contains("Exploding Kittens"))
                        {
                            thirdPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                    }
                }
            }
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p2, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 2: " + player2);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                if (size == 3)
                {
                    if (!player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (player3.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else
                    {
                        System.out.println("Congratulations, Player 2 is the winner!");
                    }
                }
                if (size == 4)
                {
                    if (!player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else if (player3.contains("Exploding Kittens") && !player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                    else if (player3.contains("Exploding Kittens") && player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else
                    {
                        System.out.println("Congratulations, Player 2 is the winner!");
                    }
                }
                else
                {
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                }
            }
        }
    }

    public void thirdPlayerFavored(String oneChoice, String twoChoice, String fourChoice)
    {
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        Scanner scan = new Scanner(System.in);
        if (oneChoice.equals("Favor"))
        {
            System.out.println("P3, choose a number to give a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
            int numSteal = scan.nextInt();
            player1.add(player3.get(numSteal));
            player3.remove(numSteal);
            try 
            {
                FileWriter fw = new FileWriter(p1, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[End] Hand 1: " + player1);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            firstPlayer("null","null","null");
        }
        if (twoChoice.equals("Favor"))
        {
            System.out.println("P3, choose a number to give a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
            int numSteal = scan.nextInt();
            player2.add(player3.get(numSteal));
            player3.remove(numSteal);
            try 
            {
                FileWriter fw = new FileWriter(p2, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[End] Hand 2: " + player2);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            secondPlayer("null","null","null");
        }
        if (fourChoice.equals("Favor"))
        {
            System.out.println("P3, choose a number to give a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
            int numSteal = scan.nextInt();
            player4.add(player3.get(numSteal));
            player3.remove(numSteal);
            try 
            {
                FileWriter fw = new FileWriter(p4, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[End] Hand 4: " + player4);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            fourthPlayer("null","null","null");
        }
    }

    public void thirdPlayerAttacked(String oneChoice, String twoChoice, String fourChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            try 
            {
                FileWriter fw = new FileWriter(p3, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 3: " + player3);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 3 - Play or Draw (d3) or h (for help) or ragequit: ");
            threeChoice = scan.nextLine();
            if (threeChoice.equals("ragequit"))
            {
                System.exit(0);
            }
            if (threeChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }
            if (!threeChoice.equals("d3") && player3.contains(threeChoice))
            {
                player3.remove(threeChoice);
                discard.add(n, threeChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
                n++;
                if (player1.contains("Nope") || player2.contains("Nope") || player4.contains("Nope"))
                {
                    if (!threeChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 3's " + threeChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 1:
                                if (player1.contains("Nope"))
                                {
                                    player1.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 2:
                                if (player2.contains("Nope"))
                                {
                                    player2.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 4:
                                if (player4.contains("Nope"))
                                {
                                    player4.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            thirdPlayerAttacked("null","null","null");
                        }
                    }
                    else
                    {
                        if (threeChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 3's " + threeChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 1:
                                    if (player1.contains("Nope"))
                                    {
                                        player1.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 2:
                                    if (player2.contains("Nope"))
                                    {
                                        player2.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 4:
                                    if (player4.contains("Nope"))
                                    {
                                        player4.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                thirdPlayerAttacked("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 1:
                                    if (!player1.contains("Exploding Kittens"))
                                    {
                                        firstPlayer("null",threeChoice,"null");
                                    }
                                    break;
                                    case 2:
                                    if (!player2.contains("Exploding Kittens"))
                                    {   
                                        secondPlayer("null",threeChoice,"null");
                                    }
                                    break;
                                    case 4:
                                    if (size == 4 && !player4.contains("Exploding Kittens"))
                                    {
                                        fourthPlayer("null","null",threeChoice);
                                    }
                                    break;
                                }
                                thirdPlayerAttacked("null","null","null");
                            }
                            System.out.println();
                        }
                    }
                }
                if (threeChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    thirdPlayerAttacked("null","null","null");
                }
                if (threeChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p3, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }
                if (threeChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                }
                if (threeChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null",threeChoice,"null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null",threeChoice,"null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null",threeChoice);
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null",threeChoice,"null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null",threeChoice,"null");
                        }
                    }
                }
                if (threeChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }
                if (threeChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                                            switch (numFavor)
                        {
                            case 1:
                            if (!player1.contains("Exploding Kittens"))
                            {
                                firstPlayer("null", threeChoice, "null");
                            }
                            break;
                            case 2:
                            if (!player2.contains("Exploding Kittens"))
                            {   
                                secondPlayer("null", threeChoice,"null");
                            }
                            break;
                            case 4:
                            if (size == 4 && !player4.contains("Exploding Kittens"))
                            {
                                fourthPlayer("null", "null", threeChoice);
                            }
                            break;
                        }
                }

                if (threeChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Hairy Potato Cat"))
                    {
                        player3.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player3.contains("Hairy Potato Cat"))
                    {
                        player3.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player3.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Hairy Potato Cat"))
                        {
                            player3.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(03,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player3.contains("Cattermelon"))
                    {
                        player3.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player3.contains("Cattermelon"))
                    {
                        player3.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player3.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }   
                        }
                        else if (player3.contains("Cattermelon"))
                        {
                            player3.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(03,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Rainbow-Ralphing Cat"))
                    {
                        player3.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player3.contains("Rainbow-Ralphing Cat"))
                    {
                        player3.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player3.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Rainbow-Ralphing Cat"))
                        {
                            player3.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(03,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Beard Cat"))
                {
                    if (!player3.contains("Beard Cat"))
                    {
                        player3.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player3.contains("Beard Cat"))
                    {
                        player3.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player3.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Beard Cat"))
                        {
                            player3.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(03,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Taco Cat"))
                    {
                        player3.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player3.contains("Taco Cat"))
                    {
                        player3.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player3.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Taco Cat"))
                        {
                            player3.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(03,pSteal);
                        }
                    }
                }
            }
        }while (!threeChoice.equals("d3"));
        if (threeChoice.equals("d3"))
        {
            player3.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();            
            if (player3.contains("Exploding Kittens"))
            {
                System.out.println("Player 3 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player3.contains("Defuse") && choice1.equals("y"))
                {
                    player3.remove("Defuse");
                    player3.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p3, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 3: " + player3);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                }
                else
                {
                    System.out.println("Player 3 has exploded.");
                    System.out.println();
                    status();
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                }
            }
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p3, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 3: " + player3);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                thirdPlayer("null", "null", "null");
            }
        }
    }

    public void thirdPlayer(String oneChoice, String twoChoice, String fourChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            if (oneChoice.equals("Attack") || twoChoice.equals("Attack") || fourChoice.equals("Attack"))
            {
                thirdPlayerAttacked(oneChoice, twoChoice, fourChoice);
            }
            if (oneChoice.equals("Favor") || twoChoice.equals("Favor") || fourChoice.equals("Favor"))
            {
                thirdPlayerFavored(oneChoice, twoChoice, fourChoice);
            }
            try 
            {
                FileWriter fw = new FileWriter(p3, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 3: " + player3);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 3 - Play or Draw (d3) or h (for help) or ragequit: ");
            threeChoice = scan.nextLine();
            if (threeChoice.equals("ragequit"))
            {
                System.exit(0);
            }
            if (threeChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }
            if (!threeChoice.equals("d3") && player3.contains(threeChoice))
            {
                player3.remove(threeChoice);
                discard.add(n, threeChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
                n++;
                if (player1.contains("Nope") || player2.contains("Nope") || player4.contains("Nope"))
                {
                    if (!threeChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 3's " + threeChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 1:
                                if (player1.contains("Nope"))
                                {
                                    player1.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 2:
                                if (player2.contains("Nope"))
                                {
                                    player2.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 4:
                                if (player4.contains("Nope"))
                                {
                                    player4.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            thirdPlayer("null","null","null");
                        }
                    }
                    else
                    {
                        if (threeChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 3's " + threeChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 1:
                                    if (player1.contains("Nope"))
                                    {
                                        player1.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 2:
                                    if (player2.contains("Nope"))
                                    {
                                        player2.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 4:
                                    if (player4.contains("Nope"))
                                    {
                                        player4.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                thirdPlayer("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 1:
                                    if (!player1.contains("Exploding Kittens"))
                                    {
                                        firstPlayer("null",threeChoice,"null");
                                    }
                                    break;
                                    case 2:
                                    if (!player2.contains("Exploding Kittens"))
                                    {   
                                        secondPlayer("null",threeChoice,"null");
                                    }
                                    break;
                                    case 4:
                                    if (size == 4 && !player4.contains("Exploding Kittens"))
                                    {
                                        fourthPlayer("null","null",threeChoice);
                                    }
                                    break;
                                }
                                thirdPlayer("null","null","null");
                            }
                            System.out.println();
                        }
                        if (threeChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    thirdPlayer("null","null","null");
                }
                if (threeChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p3, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }
                if (threeChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                }
                if (threeChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null",threeChoice,"null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null",threeChoice,"null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null",threeChoice);
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null",threeChoice,"null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null",threeChoice,"null");
                        }
                    }
                }
                if (threeChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }
                if (threeChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                                            switch (numFavor)
                        {
                            case 1:
                            if (!player1.contains("Exploding Kittens"))
                            {
                                firstPlayer("null", threeChoice, "null");
                            }
                            break;
                            case 2:
                            if (!player2.contains("Exploding Kittens"))
                            {   
                                secondPlayer("null", threeChoice,"null");
                            }
                            break;
                            case 4:
                            if (size == 4 && !player4.contains("Exploding Kittens"))
                            {
                                fourthPlayer("null", "null", threeChoice);
                            }
                            break;
                        }
                }
                if (threeChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Hairy Potato Cat"))
                    {
                        player3.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player3.contains("Hairy Potato Cat"))
                    {
                        player3.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player3.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Hairy Potato Cat"))
                        {
                            player3.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(3,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player3.contains("Cattermelon"))
                    {
                        player3.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player3.contains("Cattermelon"))
                    {
                        player3.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player3.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Cattermelon"))
                        {
                            player3.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(3,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Rainbow-Ralphing Cat"))
                    {
                        player3.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player3.contains("Rainbow-Ralphing Cat"))
                    {
                        player3.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player3.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Rainbow-Ralphing Cat"))
                        {
                            player3.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(3,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Beard Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Beard Cat"))
                    {
                        player3.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player3.contains("Beard Cat"))
                    {
                        player3.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player3.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Beard Cat"))
                        {
                            player3.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(3,pSteal);
                        }
                    }
                }
                if (threeChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player3.contains("Taco Cat"))
                    {
                        player3.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player3.contains("Taco Cat"))
                    {
                        player3.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player3.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 4:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
                                numSteal = scan.nextInt();
                                player3.add(player4.get(numSteal));
                                player4.remove(numSteal);
                                break;
                            }
                        }
                        else if (player3.contains("Taco Cat"))
                        {
                            player3.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(3,pSteal);
                        }
                    }
                }
                    }
                }

            }
        }while (!threeChoice.equals("d3"));
        if (threeChoice.equals("d3"))
        {
            player3.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player3.contains("Exploding Kittens"))
            {
                System.out.println("Player 3 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player3.contains("Defuse") && choice1.equals("y"))
                {
                    player3.remove("Defuse");
                    player3.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p3, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 3: " + player3);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                        else
                        {
                            System.out.println("Congratulations, Player 3 is the winner!");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                        else
                        {
                            System.out.println("Congratulations, Player 3 is the winner!");
                        }
                    }
                }
                else
                {
                    System.out.println("Player 3 has exploded.");
                    System.out.println();
                    status();
                    if (size == 3)
                    {
                        if (!player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                    if (size == 4)
                    {
                        if (!player4.contains("Exploding Kittens"))
                        {
                            fourthPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                        {
                            firstPlayer("null","null","null");
                        }
                        else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                        {
                            secondPlayer("null","null","null");
                        }
                    }
                }
            }
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p3, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 3: " + player3);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                if (size == 3)
                {
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else
                    {
                        System.out.println("Congratulations, Player 3 is the winner!");
                    }
                }
                if (size == 4)
                {
                    if (!player4.contains("Exploding Kittens"))
                    {
                        fourthPlayer("null","null","null");
                    }
                    else if (player4.contains("Exploding Kittens") && !player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player4.contains("Exploding Kittens") && player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else
                    {
                        System.out.println("Congratulations, Player 3 is the winner!");
                    }
                }
            }
        }
    }

    public void fourthPlayerFavored(String oneChoice, String twoChoice, String threeChoice)
    {
        String p1 = "C:\\Users\\1049467\\Desktop\\EKHand1.txt";
        String p2 = "C:\\Users\\1049467\\Desktop\\EKHand2.txt";
        String p3 = "C:\\Users\\1049467\\Desktop\\EKHand3.txt";
        Scanner scan = new Scanner(System.in);
        if (oneChoice.equals("Favor"))
        {
            System.out.println("P4, choose a number to give a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
            int numSteal = scan.nextInt();
            player1.add(player4.get(numSteal));
            player4.remove(numSteal);
            try 
            {
                FileWriter fw = new FileWriter(p1, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[End] Hand 1: " + player1);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            firstPlayer("null","null","null");
        }
        if (twoChoice.equals("Favor"))
        {
            System.out.println("P4, choose a number to give a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
            int numSteal = scan.nextInt();
            player2.add(player4.get(numSteal));
            player4.remove(numSteal);
            try 
            {
                FileWriter fw = new FileWriter(p2, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[End] Hand 2: " + player2);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            secondPlayer("null","null","null");
        }
        if (threeChoice.equals("Favor"))
        {
            System.out.println("P4, choose a number to give a card from the hand: " + "(left to right) 0-" + (player4.size()-1));
            int numSteal = scan.nextInt();
            player3.add(player4.get(numSteal));
            player4.remove(numSteal);
            try 
            {
                FileWriter fw = new FileWriter(p3, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[End] Hand 3: " + player3);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            thirdPlayer("null","null","null");
        }
    }

    public void fourthPlayerAttacked(String oneChoice, String twoChoice, String threeChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            try 
            {
                FileWriter fw = new FileWriter(p4, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 4: " + player4);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 4 - Play or Draw (d4) or h (for help) or ragequit: ");
            fourChoice = scan.nextLine();
            if (fourChoice.equals("ragequit"))
            {
                System.exit(0);
            }
            if (fourChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }
            if (!fourChoice.equals("d4") && player4.contains(fourChoice))
            {
                player4.remove(fourChoice);
                discard.add(n, fourChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
                n++;
                if (player1.contains("Nope") || player2.contains("Nope") || player3.contains("Nope"))
                {
                    if (!fourChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 4's " + fourChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 1:
                                if (player1.contains("Nope"))
                                {
                                    player1.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 2:
                                if (player2.contains("Nope"))
                                {
                                    player2.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 3:
                                if (player3.contains("Nope"))
                                {
                                    player3.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            fourthPlayerAttacked("null","null","null");
                        }
                    }
                    else
                    {
                        if (fourChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 4's " + fourChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 1:
                                    if (player1.contains("Nope"))
                                    {
                                        player1.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 2:
                                    if (player2.contains("Nope"))
                                    {
                                        player2.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 3:
                                    if (player3.contains("Nope"))
                                    {
                                        player3.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                fourthPlayerAttacked("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 1:
                                    if (!player1.contains("Exploding Kittens"))
                                    {
                                        firstPlayer("null","null",fourChoice);
                                    }
                                    break;
                                    case 2:
                                    if (!player2.contains("Exploding Kittens"))
                                    {   
                                        secondPlayer("null","null",fourChoice);
                                    }
                                    break;
                                    case 3:
                                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                                    {
                                        thirdPlayer("null","null",fourChoice);
                                    }
                                    break;
                                }
                                fourthPlayerAttacked("null","null","null");
                            }
                            System.out.println();
                        }
                    }
                }
                if (fourChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    fourthPlayerAttacked("null","null","null");
                }
                if (fourChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p4, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }
                if (fourChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                }
                if (fourChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null",fourChoice);
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null",fourChoice);
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null",fourChoice);
                    }
                }
                if (fourChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }
                if (fourChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                                            switch (numFavor)
                        {
                            case 1:
                            if (!player1.contains("Exploding Kittens"))
                            {
                                firstPlayer("null","null",fourChoice);
                            }
                            break;
                            case 2:
                            if (!player2.contains("Exploding Kittens"))
                            {   
                                secondPlayer("null","null",fourChoice);
                            }
                            break;
                            case 3:
                            if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                            {
                                thirdPlayer("null","null",fourChoice);
                            }
                            break;
                        }
                }
                if (fourChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Hairy Potato Cat"))
                    {
                        player4.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player4.contains("Hairy Potato Cat"))
                    {
                        player4.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player4.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Hairy Potato Cat"))
                        {
                            player4.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(04,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player4.contains("Cattermelon"))
                    {
                        player4.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player4.contains("Cattermelon"))
                    {
                        player4.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player4.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Cattermelon"))
                        {
                            player4.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(04,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Rainbow-Ralphing Cat"))
                    {
                        player4.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player4.contains("Rainbow-Ralphing Cat"))
                    {
                        player4.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player4.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Rainbow-Ralphing Cat"))
                        {
                            player4.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(04,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Beard Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Beard Cat"))
                    {
                        player4.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player4.contains("Beard Cat"))
                    {
                        player4.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player4.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Beard Cat"))
                        {
                            player4.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(04,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Taco Cat"))
                    {
                        player4.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player4.contains("Taco Cat"))
                    {
                        player4.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player4.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Taco Cat"))
                        {
                            player4.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(04,pSteal);
                        }
                    }
                }
            }
        }while (!fourChoice.equals("d4"));
        if (fourChoice.equals("d4"))
        {
            player4.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player4.contains("Exploding Kittens"))
            {
                System.out.println("Player 4 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player4.contains("Defuse") && choice1.equals("y"))
                {
                    player4.remove("Defuse");
                    player4.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p4, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 4: " + player4);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                }
                else
                {
                    System.out.println("Player 4 has exploded.");
                    System.out.println();
                    status();
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                }
            }
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p4, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 4: " + player4);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                fourthPlayer("null", "null", "null");
            }
        }
    }

    public void fourthPlayer(String oneChoice, String twoChoice, String threeChoice)
    {
        Random rand = new Random();
        Scanner scan = new Scanner(System.in);
        String p = "C:\\Users\\1049467\\Desktop\\EKHelp.txt";
        String p4 = "C:\\Users\\1049467\\Desktop\\EKHand4.txt";
        String d = "C:\\Users\\1049467\\Desktop\\EKDiscard.txt";
        int pSteal;
        int numSteal;
        int numFavor;
        int nopeChoice = 0;
        String strSteal;
        int n = 0;
        do
        {
            if (oneChoice.equals("Attack") || twoChoice.equals("Attack") || threeChoice.equals("Attack"))
            {
                fourthPlayerAttacked(oneChoice, twoChoice, fourChoice);
            }
            if (oneChoice.equals("Favor") || twoChoice.equals("Favor") || threeChoice.equals("Favor"))
            {
                fourthPlayerFavored(oneChoice, twoChoice, threeChoice);
            }
            try 
            {
                FileWriter fw = new FileWriter(p4, true);
                BufferedWriter bw = new BufferedWriter(fw);
                bw.newLine();
                bw.write("[Start] Hand 4: " + player4);
                bw.close();
            }
            catch(IOException e)
            {
                System.out.println("Error " + e);
            }
            System.out.println("Card Count: " + (EKC.size()));
            System.out.println("Player 4 - Play or Draw (d4) or h (for help) or ragequit: ");
            fourChoice = scan.nextLine();
            if (fourChoice.equals("ragequit"))
            {
                System.exit(0);
            }
            if (fourChoice.equals("h"))
            {
                System.out.println("Check your desktop for a file called EKHelp.txt.");
                System.out.println("Delete all files when everyone is finished or when the game is discontinued.");
                try 
                {
                    FileWriter fw = new FileWriter(p, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.write("(*2) 2 cats = user chooses any player and takes a random card of user's choice");
                    bw.newLine();
                    bw.write("(*3) 3 cats = user chooses any player and types any card of their choice");
                    bw.newLine();
                    bw.write("(*3) (cont). if the opposing player has the card, then the user takes the card");
                    bw.newLine();
                    bw.write("(*3) (cont). else the opposing player doesn't have the card, then the user does not take any card");
                    bw.newLine();
                    bw.write("Skip - skips the player's turn who used Skip");
                    bw.newLine();
                    bw.write("Attack - skips the player's turn who used Attack and gives the next player 2 more turns");
                    bw.newLine();
                    bw.write("Shuffle - shuffles the deck");
                    bw.newLine();
                    bw.write("Favor - ask any player to give the player who used favor any card of the recipient's choice");
                    bw.newLine();
                    bw.write("Hairy Potato Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Cattermelon - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Rainbow-Ralphing Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("Beard Cat - a cat (*2) (*3) ");
                    bw.newLine();
                    bw.write("Taco Cat - a cat (*2) (*3)");
                    bw.newLine();
                    bw.write("See the Future - sees the top 3 cards of the deck");
                    bw.newLine();
                    bw.write("Nope - denies the action of any cards with special effects (including the 2 and 3 cat effects)");
                    bw.newLine();
                    bw.write("Exploding Kittens - a deadly kitten (cannot use Nope on this card) that explodes unless you have a defuse");
                    bw.newLine();
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
            }
            if (!fourChoice.equals("d4") && player4.contains(fourChoice))
            {
                player4.remove(fourChoice);
                discard.add(n, fourChoice);
                try 
                {
                    FileWriter fw = new FileWriter(d, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("Discard Pile: " + discard);
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                System.out.println();
                n++;
                if (player1.contains("Nope") || player2.contains("Nope") || player3.contains("Nope"))
                {
                    if (!fourChoice.equals("Favor"))
                    {
                        System.out.println("Does anyone want to use their Nope on Player 4's " + fourChoice + "?");
                        System.out.println("(Enter your player # for yes or enter 0 for no)");
                        nopeChoice = scan.nextInt();
                        System.out.println();
                        if (nopeChoice != 0)
                        {
                            switch (nopeChoice)
                            {
                                case 1:
                                if (player1.contains("Nope"))
                                {
                                    player1.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 2:
                                if (player2.contains("Nope"))
                                {
                                    player2.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                                case 3:
                                if (player3.contains("Nope"))
                                {
                                    player3.remove("Nope");
                                    discard.add("Nope");
                                }
                                break;
                            }
                            nopeChoice = 0;
                            fourthPlayer("null","null","null");
                        }
                    }
                    else
                    {
                        if (fourChoice.equals("Favor"))
                        {
                            System.out.println("Enter the player who you want to use favor on: ");
                            numFavor = scan.nextInt();
                            System.out.println("Does anyone want to use their Nope on Player 4's " + fourChoice + "?");
                            System.out.println("(Enter your player # for yes or enter 0 for no)");
                            nopeChoice = scan.nextInt();
                            System.out.println();
                            if (nopeChoice != 0)
                            {
                                switch (nopeChoice)
                                {
                                    case 1:
                                    if (player1.contains("Nope"))
                                    {
                                        player1.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 2:
                                    if (player2.contains("Nope"))
                                    {
                                        player2.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                    case 3:
                                    if (player3.contains("Nope"))
                                    {
                                        player3.remove("Nope");
                                        discard.add("Nope");
                                    }
                                    break;
                                }
                                nopeChoice = 0;
                                fourthPlayer("null","null","null");
                            }
                            if (nopeChoice == 0)
                            {
                                switch (numFavor)
                                {
                                    case 1:
                                    if (!player1.contains("Exploding Kittens"))
                                    {
                                        firstPlayer("null","null",fourChoice);
                                    }
                                    break;
                                    case 2:
                                    if (!player2.contains("Exploding Kittens"))
                                    {   
                                        secondPlayer("null","null",fourChoice);
                                    }
                                    break;
                                    case 3:
                                    if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                                    {
                                        thirdPlayer("null","null",fourChoice);
                                    }
                                    break;
                                }
                                fourthPlayer("null","null","null");
                            }
                            System.out.println();
                        }
                    }
                }
                if (fourChoice.equals("Nope"))
                {
                    System.out.println("Nope can only be used to counter another card!");
                    fourthPlayer("null","null","null");
                }
                if (fourChoice.equals("See the Future") && nopeChoice == 0)
                {
                    System.out.println("Check your file to know the first three cards.");
                    System.out.println();
                    try 
                    {
                        FileWriter fw = new FileWriter(p4, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("First Three Cards in the Deck: ");
                        bw.newLine();
                        for (int i = 0; i < 3; i++)
                        {
                            bw.write(EKC.get(i));
                            bw.newLine();
                        }
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                }
                if (fourChoice.equals("Skip") && nopeChoice == 0)
                {
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                }
                if (fourChoice.equals("Attack") && nopeChoice == 0)
                {
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null",fourChoice);
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null",fourChoice);
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null",fourChoice);
                    }
                }
                if (fourChoice.equals("Shuffle") && nopeChoice == 0)
                {
                    Shuffle();
	        System.out.println("The Exploding Kittens deck has been shuffled.");
	        System.out.println();
                }
                if (fourChoice.equals("Favor") && nopeChoice == 0)
                {
                    System.out.println("Enter the player who you want to use favor on: ");
                    numFavor = scan.nextInt();
                                            switch (numFavor)
                        {
                            case 1:
                            if (!player1.contains("Exploding Kittens"))
                            {
                                firstPlayer("null","null",fourChoice);
                            }
                            break;
                            case 2:
                            if (!player2.contains("Exploding Kittens"))
                            {   
                                secondPlayer("null","null",fourChoice);
                            }
                            break;
                            case 3:
                            if (size == 3 || size == 4 && !player3.contains("Exploding Kittens"))
                            {
                                thirdPlayer("null","null",fourChoice);
                            }
                            break;
                        }
                }
                if (fourChoice.equals("Hairy Potato Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Hairy Potato Cat"))
                    {
                        player4.add("Hairy Potato Cat");
                        discard.remove("Hairy Potato Cat");
                    }
                    else if (player4.contains("Hairy Potato Cat"))
                    {
                        player4.remove("Hairy Potato Cat");
                        discard.add("Hairy Potato Cat");
                        if (!player4.contains("Hairy Potato Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Hairy Potato Cat"))
                        {
                            player4.remove("Hairy Potato Cat");
                            discard.add("Hairy Potato Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(4,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Cattermelon") && nopeChoice == 0)
                {
                    if (!player4.contains("Cattermelon"))
                    {
                        player4.add("Cattermelon");
                        discard.remove("Cattermelon");
                    }
                    else if (player4.contains("Cattermelon"))
                    {
                        player4.remove("Cattermelon");
                        discard.add("Cattermelon");
                        if (!player4.contains("Cattermelon"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Cattermelon"))
                        {
                            player4.remove("Cattermelon");
                            discard.add("Cattermelon");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(4,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Rainbow-Ralphing Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Rainbow-Ralphing Cat"))
                    {
                        player4.add("Rainbow-Ralphing Cat");
                        discard.remove("Rainbow-Ralphing Cat");
                    }
                    else if (player4.contains("Rainbow-Ralphing Cat"))
                    {
                        player4.remove("Rainbow-Ralphing Cat");
                        discard.add("Rainbow-Ralphing Cat");
                        if (!player4.contains("Rainbow-Ralphing Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Rainbow-Ralphing Cat"))
                        {
                            player4.remove("Rainbow-Ralphing Cat");
                            discard.add("Rainbow-Ralphing Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(4,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Beard Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Beard Cat"))
                    {
                        player4.add("Beard Cat");
                        discard.remove("Beard Cat");
                    }
                    else if (player4.contains("Beard Cat"))
                    {
                        player4.remove("Beard Cat");
                        discard.add("Beard Cat");
                        if (!player4.contains("Beard Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Beard Cat"))
                        {
                            player4.remove("Beard Cat");
                            discard.add("Beard Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(4,pSteal);
                        }
                    }
                }
                if (fourChoice.equals("Taco Cat") && nopeChoice == 0)
                {
                    if (!player4.contains("Taco Cat"))
                    {
                        player4.add("Taco Cat");
                        discard.remove("Taco Cat");
                    }
                    else if (player4.contains("Taco Cat"))
                    {
                        player4.remove("Taco Cat");
                        discard.add("Taco Cat");
                        if (!player4.contains("Taco Cat"))
                        {
                            System.out.println("Input a player's number to steal a card from.");
                            pSteal = scan.nextInt();
                            switch (pSteal)
                            {
                                case 1:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player1.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player1.get(numSteal));
                                player1.remove(numSteal);
                                break;
                                case 2:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player2.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player2.get(numSteal));
                                player2.remove(numSteal);
                                break;
                                case 3:
                                System.out.println("Choose a number to randomly pick a card from the hand: " + "(left to right) 0-" + (player3.size()-1));
                                numSteal = scan.nextInt();
                                player4.add(player3.get(numSteal));
                                player3.remove(numSteal);
                                break;
                            }
                        }
                        else if (player4.contains("Taco Cat"))
                        {
                            player4.remove("Taco Cat");
                            discard.add("Taco Cat");
                            System.out.println("Input a player's number to steal a card from: ");
                            pSteal = scan.nextInt();
                            threeCatEffect(4,pSteal);
                        }
                    }
                }
            }
        }while (!fourChoice.equals("d4"));
        if (fourChoice.equals("d4"))
        {
            player4.add(EKC.get(0));
            EKC.remove(0);
            System.out.println();
            if (player4.contains("Exploding Kittens"))
            {
                System.out.println("Player 4 - You got an Exploding Kitten. Defuse y/n?: ");
                choice1 = scan.nextLine();
                if (player4.contains("Defuse") && choice1.equals("y"))
                {
                    player4.remove("Defuse");
                    player4.remove("Exploding Kittens");
                    try 
                    {
                        FileWriter fw = new FileWriter(p4, true);
                        BufferedWriter bw = new BufferedWriter(fw);
                        bw.newLine();
                        bw.write("[End] Hand 4: " + player4);
                        bw.close();
                    }
                    catch(IOException e)
                    {
                        System.out.println("Error " + e);
                    }
                    System.out.println("Where do you want to place the Exploding Kitten? (0-" + ((EKC.size())-1) + ")(0 is the top of the deck. " + ((EKC.size())-1) + " is the bottom of the deck.)(Ex. 1 is on top of the second card. 2 above the third card. Etc.)");
                    choice2 = scan.nextInt();
                    System.out.println();
                    for (int j = choice2 + 1; j < EKC.size(); j++)
                    {
                        EKC.set(j, EKC.get(j));
                    }
                    EKC.add(choice2, "Exploding Kittens");
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                    else
                    {
                        System.out.println("Congratulations, Player 4 is the winner!");
                    }
                }
                else
                {
                    System.out.println("Player 4 has exploded.");
                    System.out.println();
                    status();
                    if (!player1.contains("Exploding Kittens"))
                    {
                        firstPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                    {
                        secondPlayer("null","null","null");
                    }
                    else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                    {
                        thirdPlayer("null","null","null");
                    }
                }
            }
            else
            {
                try 
                {
                    FileWriter fw = new FileWriter(p4, true);
                    BufferedWriter bw = new BufferedWriter(fw);
                    bw.newLine();
                    bw.write("[End] Hand 4: " + player4);
                    bw.close();
                }
                catch(IOException e)
                {
                    System.out.println("Error " + e);
                }
                if (!player1.contains("Exploding Kittens"))
                {
                    firstPlayer("null","null","null");
                }
                else if (player1.contains("Exploding Kittens") && !player2.contains("Exploding Kittens"))
                {
                    secondPlayer("null","null","null");
                }
                else if (player1.contains("Exploding Kittens") && player2.contains("Exploding Kittens") && !player3.contains("Exploding Kittens"))
                {
                    thirdPlayer("null","null","null");
                }
                else
                {
                    System.out.println("Congratulations, Player 4 is the winner!");
                }
            }
        }
    }

    public void status()
    {
        String p1status, p2status, p3status, p4status;
        if (player1.contains("Exploding Kittens"))
        {
            p1status = "Dead";
        }
        else
        {
            p1status = "Alive";
        }
        if (player2.contains("Exploding Kittens"))
        {
            p2status = "Dead";
        }
        else
        {
            p2status = "Alive";
        }
        if (player3.contains("Exploding Kittens"))
        {
            p3status = "Dead";
        }
        else
        {
            p3status = "Alive";
        }
        if (player4.contains("Exploding Kittens"))
        {
            p4status = "Dead";
        }
        else
        {
            p4status = "Alive";
        }
        if (size == 2)
        {
            if (p1status.equals("Alive") && p2status.equals("Dead"))
            {
                System.out.println("Congratulations, Player 1 wins!");
            }
            if (p1status.equals("Dead") && p2status.equals("Alive"))
            {
                System.out.println("Congratulations, Player 2 wins!");
            }
        }
        if (size == 3)
        {
            if (p1status.equals("Alive") && p2status.equals("Dead") && p3status.equals("Dead"))
            {
                System.out.println("Congratulations, Player 1 wins!");
            }
            if (p1status.equals("Dead") && p2status.equals("Alive") && p3status.equals("Dead"))
            {
                System.out.println("Congratulations, Player 2 wins!");
            }
            if (p1status.equals("Dead") && p2status.equals("Dead") && p3status.equals("Alive"))
            {
                System.out.println("Congratulations, Player 3 wins!");
            }
        }
        if (size == 4)
        {
            if (p1status.equals("Alive") && p2status.equals("Dead") && p3status.equals("Dead") && p4status.equals("Dead"))
            {
                System.out.println("Congratulations, Player 1 wins!");
            }
            if (p1status.equals("Dead") && p2status.equals("Alive") && p3status.equals("Dead") && p4status.equals("Dead"))
            {
                System.out.println("Congratulations, Player 2 wins!");
            }
            if (p1status.equals("Dead") && p2status.equals("Dead") && p3status.equals("Alive") && p4status.equals("Dead"))
            {
                System.out.println("Congratulations, Player 3 wins!");
            }
            if (p1status.equals("Dead") && p2status.equals("Dead") && p3status.equals("Dead") && p4status.equals("Alive"))
            {
                System.out.println("Congratulations, Player 4 wins!");
            }
        }
    }//ends status method which determines the winner
}//ends ExplodingKittens class















