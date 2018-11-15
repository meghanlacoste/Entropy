# Entropy

package com.company;


import java.util.*;
import java.io.*;
import static com.company.ProjConstants.*;
public class Main {

    public static void main(String[] args) {
	// write your code here


     // Creates an object from Entropy Class
    Entropy calcE = new Entropy();


    //Creates new 2d array:
       // Column [n][0] represents the unique message.
       // Column [n][1] represents the frequency of the message
       // Column [n][2] represents probability of message
       // Column [n][3] represents the number of bits required for the message
       // Column [n][4] represents the message identifying number

        String arr_[][] = new String [MAX_DATA][3];


        // Initialize the array to a known value
        // - loops over each row using "i", and columns using "j"
        // -  sets all array values to INVALID

        for(int i=0; i < MAX_DATA; i++) {
            for(int j = 0; j < 2; j++){
                arr_[i][j] = "INVALID";
            }
        }


        // DECLARE VARIABLES

        Boolean replicatedWord=false;
        int totalFreq=INVALID;
        String MsgProb;
        int maxBits = INVALID;
        double entropy = INVALID;
        Boolean encodeFile = false;
        String encodeFileName =  "user_msg_filename.txt ";
        Boolean decodeFile = false;
        String decodeFileName;
        Boolean messageFile = false;
        Boolean noFiles = false;
        Boolean validWord = false;
        Boolean fileDone=false;
        try {
            // --------------------------------
            // create file, and scanner objects
            // - file object is called tempfilenums.txt and is in your project directory
            //   that is the same folder as the iml file
            //

            File userFile = new File(encodeFileName);
            Scanner scanUserFile = new Scanner(userFile);

            // ---------------------------------------------
            // Reads in values from the file in a for loop
            //

            for(int i = 0; i < MAX_DATA; i++) {


                // sets boolean back to false at the start of each repetition
                replicatedWord = false;

                // --------------------------------------------------
                // The scanner checks if there is another string
                //

                if (scanUserFile.hasNext()) {

                    // converts string to lower case

                    String fileWord =  scanUserFile.next().toLowerCase();


                    // starting at the current index position 'i', loops backwards over previously stored
                    // strings in the array and checks if the file word is equal to any of the previous values

                    for (int index = i; index >= 0; index --) {

                        if (fileWord.equalsIgnoreCase(arr_[index][0])) {

                            // calls upon method to get and return new frequency (adds one to the frequency)


                            // converts string stored in array to integer
                            int frequency = Integer.parseInt(arr_[index][1]);

                            // adds one to the frequency
                            frequency  += 1 ;

                            // converts the new frequency back to a string and stores it back into the array
                            arr_ [index][1] = Integer.toString(frequency);

                            replicatedWord = true;

                        }

                    }

                    // ---------------------------------------
                    //
                    //  pre-conditions:
                    //      - the word is unique

                    if (!replicatedWord) {

                        // stores the word from the file in the next position of the array

                       for (int i2=0; i2< MAX_DATA; i2++){
                           if (arr_[i2][0].equals("INVALID")){

                               arr_ [i2][0] = fileWord;

                               // the value of 1 will be converted into a string type so that it can be stored in
                               // the second colomn of the array.
                               arr_ [i2][1] = Integer.toString(1);
                               break;
                           }
                       }

                    }// end for-loop



                }// end if scanner has next

                else {
                    // ---------------------------------------------
                    // The scanner detected no other integers
                    // - sets boolean "fileDone" to true
                    // - closes the scanner
                    // - breaks out of the for loop
                    //

                    fileDone = true;
                    scanUserFile.close();
                    break;
                }
            }

            // ---------------------------------------------
            // The file has been completely read into the array, so the user is notified.
            //

            System.out.println();
            if (fileDone) {
                System.out.println("\n\tFile data has been read into array\n");

            }

            // ---------------------------------------------
            // If there is not enough space in the array so make sure the user is notified that the file contains
            // more data than the array can hold.
            //

            if (!fileDone) {
                System.out.println("\n\tCAUTION: file has additional data, consider making array larger.\n");
            }


            // ---------------------------------------------
            // If the file cannot be found then an exception (error) is generated (thrown) that we have to
            // deal with (catch).
            // - we print "e" the exception, and
            // - show the user where it was using the "exceptions" stack trace information
            //

            System.out.println();
            for (int i = 0; i < MAX_DATA; i++) {
                if (arr_[i][0] != "INVALID")
                    System.out.println(arr_[i][0] + " index " + i + " freq: " + arr_[i][1]);
            }


        } catch (FileNotFoundException e) {
            System.out.println(e);
            e.printStackTrace();
        }



/*
  for (int i=0; i < MAX_DATA; i++){
           if (arr_ [i][1] != "INVALID") {
               calcE.calcTotalFreq(arr_ [i][1]);

           }

        }
 */

        System.out.println("Total Freq: " + calcE.calcTotalFreq(arr_));

        System.out.println("Number of messages: " + calcE.numMsgs(arr_));

        System.out.println("MaxBits: " + calcE.calcMaxBits());

        for (int i = 0; i < MAX_DATA; i++) {
            if (arr_[i][1] != "INVALID") {

                System.out.println(arr_[i][1]);

                calcE.calcMsgProb(arr_[i][1]);


                arr_[i][2] =  Double.toString( calcE.calcMsgProb(arr_[i][1]));

                System.out.print("****" + calcE.calcMsgProb(arr_[i][1]));
                System.out.println(" Message Probability: " + arr_[i][2]);

                System.out.println("Message Bits: " + calcE.calcMsgBits(arr_[i][2]));


            }
        }


















    }// end main method
}// end main class
