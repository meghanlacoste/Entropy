# Entropy
package com.company;


import java.util.*;
import java.io.*;
import java.text.DecimalFormat;
import static com.company.ProjConstants.*;
public class Main {



    private static String NEWLINE = System.getProperty("line.separator");


    public static void main(String[] args) {
        // write your code here


        // Creates an object from Entropy Class
        Entropy calcE = new Entropy(0);
        Entropy Encode = new Entropy(ENCODE);
        Entropy Decode = new Entropy(DECODE);


        //Creates new 2d array:
        // Column [n][0] represents the unique message.
        // Column [n][1] represents the frequency of the message
        // Column [n][2] represents probability of message
        // Column [n][3] represents the number of bits required for the message
        // Column [n][4] represents the message identifying number

        String arr_[][] = new String[MAX_DATA][5];


        // Initialize the array to a known value
        // - loops over each row using "i", and columns using "j"
        // -  sets all array values to INVALID

        for (int i = 0; i < MAX_DATA; i++) {
            for (int j = 0; j < 5; j++) {
                arr_[i][j] = "INVALID";
            }
        }


        // DECLARE VARIABLES

        Boolean replicatedWord = false;
        int totalFreq = INVALID;
        String MsgProb;
        int maxBits = INVALID;
        double entropy = INVALID;
        Boolean encodeFile = false;
        String user_msg_filename = "user_msg_filename.txt";
        String user_msg_filename_out;
        String outString;
        String userInputFileName;
        String userOutputFileName;
        String fileString;


        Boolean fileDone = false;
        try {
            // --------------------------------
            // create file, and scanner objects
            // - file object is called tempfilenums.txt and is in your project directory
            //   that is the same folder as the iml file
            //

            File userFile = new File(user_msg_filename);
            Scanner scanUserFile = new Scanner(userFile);

            // ---------------------------------------------
            // Reads in values from the file in a for loop
            //

            for (int i = 0; i < MAX_DATA; i++) {


                // sets boolean back to false at the start of each repetition
                replicatedWord = false;

                // --------------------------------------------------
                // The scanner checks if there is another string
                //

                if (scanUserFile.hasNext()) {

                    // converts string to lower case

                    String fileWord = scanUserFile.next().toLowerCase();


                    // starting at the current index position 'i', loops backwards over previously stored
                    // strings in the array and checks if the file word is equal to any of the previous values

                    for (int index = i; index >= 0; index--) {

                        if (fileWord.equalsIgnoreCase(arr_[index][0])) {

                            // calls upon method to get and return new frequency (adds one to the frequency)


                            // converts string stored in array to integer
                            int frequency = Integer.parseInt(arr_[index][1]);

                            // adds one to the frequency
                            frequency += 1;

                            // converts the new frequency back to a string and stores it back into the array
                            arr_[index][1] = Integer.toString(frequency);

                            replicatedWord = true;

                        }

                    }

                    // ---------------------------------------
                    //
                    //  pre-conditions:
                    //      - the word is unique

                    if (!replicatedWord) {

                        // stores the word from the file in the next position of the array

                        for (int i2 = 0; i2 < MAX_DATA; i2++) {
                            if (arr_[i2][0].equals("INVALID")) {

                                arr_[i2][0] = fileWord;

                                // the value of 1 will be converted into a string type so that it can be stored in
                                // the second colomn of the array.
                                arr_[i2][1] = Integer.toString(1);
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


            //------------------------------------------------------------------------

            calcE.calcTotalFreq(arr_);

            calcE.numMsgs(arr_);

            calcE.calcMaxBits();

            for (int i = 0; i < MAX_DATA; i++) {
                if (arr_[i][1] != "INVALID") {


                    calcE.calcMsgProb(arr_[i][1]);

                    arr_[i][2] = Double.toString(calcE.calcMsgProb(arr_[i][1]));

                    arr_[i][3] = Integer.toString(calcE.calcMsgBits(arr_[i][2]));


                }
            }


            calcE.tempArray(arr_);


            for (int i = 0; i < MAX_DATA; i++) {

                if (arr_[i][0] != "INVALID") {

                    arr_[i][4] = Integer.toString(calcE.encodeMsg(i));

                }

            }

            // Display the sorted Matrix
            for (int i = 0; i < MAX_DATA; i++) {
                if (arr_[i][0] != "INVALID") {
                    //  System.out.println(arr_[i][0] + " " +arr_[i][1]  + " " + arr_[i][2] + " " + arr_[i][3] + " " +  arr_[i][4]  );

                }

            }



            System.out.println("==========================================================================================");
            System.out.println("                       Data Result File \n");

            System.out.println("\tData File:............\t\t\t " + user_msg_filename);
            System.out.printf("\tNumber of Messages:....\t%10s\n", Integer.toString(calcE.numMsgs(arr_)));
            System.out.printf("\tMax Number of Bits.....\t%10s\n", Integer.toString(calcE.calcMaxBits()));
            System.out.printf("\tEntropy:...............\t%13.2f\n", calcE.calcEntropy(arr_));
            System.out.println("\n            Message          Frequency         # of bits       Probability ");
            System.out.println("           ------------------------------------------------------------------\n");

            for (int i = 0; i < MAX_DATA; i++) {
                if (arr_[i][0] != "INVALID") {
                    System.out.printf("\t\t %10s      %10s      %10s         %10.2f\n", arr_[i][0], arr_[i][1], arr_[i][3], Double.parseDouble(arr_[i][2]));

                }
            }

            System.out.println("\n           ------------------------------------------------------------------\n");
            System.out.println("\n==========================================================================================\n");


            user_msg_filename_out = user_msg_filename.replace(".txt", "_out.txt");
            File outputFile = new File(user_msg_filename_out);


            if (outputFile.createNewFile()){
                System.out.println(user_msg_filename_out + " was created"); // if file was created
            }
            else {
                System.out.println(user_msg_filename_out + " existed and is being overwritten."); // if file had already existed
            }

            // --------------------------------
            // If the file creation of access permissions to write into it
            // are incorrect the program throws an exception
            //

            if ((outputFile.isFile()|| outputFile.canWrite())){
                BufferedWriter fileOut = new BufferedWriter(new FileWriter(outputFile));

                DecimalFormat numberFormat = new DecimalFormat("0.00");


                fileOut.write("==========================================================================================");
                fileOut.write("                       Data Result File " + NEWLINE);

                outString = NEWLINE + "\t\tData File.............. \t\t" +  user_msg_filename + NEWLINE;
                fileOut.write(outString);
                outString = "\t\tNumber of Messages:.... \t\t" + Integer.toString(calcE.numMsgs(arr_)) +  NEWLINE;
                fileOut.write(outString);
                outString = "\t\tMaxNumber of Bits:.....  \t\t" + Integer.toString(calcE.calcMaxBits()) + NEWLINE;
                fileOut.write(outString);
                outString = "\t\tEntropy:...............   \t\t" + numberFormat.format(calcE.calcEntropy(arr_))+  NEWLINE + NEWLINE;
                fileOut.write(outString);
                fileOut.write (NEWLINE + "            Message          Frequency         # of bits       Probability " + NEWLINE) ;
                fileOut.write("       --------------------------------------------------------------------------" + NEWLINE);

                for (int i = 0; i < MAX_DATA; i++) {
                    if (arr_[i][0] != "INVALID") {

                        outString = "\t\t\t" + arr_[i][0] + "\t\t\t\t" + arr_[i][1] +  "\t\t\t\t   " + arr_[i][3] + "\t\t\t\t" + (numberFormat.format(Double.parseDouble(arr_[i][2])));
                        fileOut.write(NEWLINE + outString);
                    }
                }


                fileOut.write(NEWLINE + NEWLINE + "==========================================================================================");

                fileOut.close();

            }
            else {
                throw new IOException();
            }

        } // end of try

        // ---------------------------------------------
        // Catch Statements from the try above

        // ---------------------------------------------
        // If the file cannot be found then an exception (error) is generated (thrown) that we have to
        // deal with (catch).
        //
        catch (FileNotFoundException e) {
            System.err.format("File Not Found Exception: %s%n", e);
            e.printStackTrace();
        }

        // ---------------------------------------------
        // If for some reason the output file could not be created we throw an IO Exception
        //
        catch (IOException e) { // in case for some reason the output file could not be created
            System.err.format("IOException: %s%n", e);
            e.printStackTrace();
        }




        //============================================================================================================================
        //
        System.out.println("\nEnter the name of the file you would like to encode");


        // -------------------------------
        //

        Scanner scanSystemIn = new Scanner(System.in, "UTF-8");

        // --------------------------------
        // Use the Scanner "userFileName" to get the "next" input from "System.in"
        //

        userInputFileName = scanSystemIn.next();

        // --------------------------------
        // Display the user input now stored in "userInput"
        //

        System.out.println("\nThe user input: " + userInputFileName);

        // ---------------------------------------------
        // This is where we "try" to process the file
        //



            try {

                // --------------------------------
                // create file, and scanner objects
                // - file object is called tempfilenums.txt and is in your project directory
                //   that is the same folder as the iml file
                //

                File userFile = new File(userInputFileName);
                Scanner scanUserFile = new Scanner(userFile, "UTF-8");

                // ---------------------------------------------
                // Reads in values from the file in a for loop
                //

                for(int i=0; i < MAX_DATA; i++) {

                    // ---------------------------------------------
                    // The scanner checks if there is another integer and prints it
                    // if there is
                    //

                    if (scanUserFile.hasNext()) {

                        fileString = scanUserFile.next();

                        for(int i2=0; i2 < MAX_DATA; i2++) {

                            if (arr_[i2][0] != "INVALID"){
                                if (arr_[i2][0].equalsIgnoreCase(fileString)){

                                    System.out.println(Integer.toString(calcE.encodeMsg(i2)));

                                }
                            }

                        }

                    }
                    else {
                        // ---------------------------------------------
                        // The scanner detected no other integers
                        // - closes the scanner for the file
                        // - breaks out of the for loop
                        //
                        System.out.print("\n\nDataFileFILE has been completely READ\n\n");
                        scanUserFile.close();

                        // A break statement allows us to exit the loop before we have reach the end
                        break;
                    }
                }



                userOutputFileName = userInputFileName.replace(".txt","_out.txt");
                File outputFile = new File(userOutputFileName);

                if (outputFile.createNewFile()){
                    System.out.println(userOutputFileName + " was created"); // if file was created
                }
                else {
                    System.out.println(userOutputFileName + " existed and is being overwritten."); // if file had already existed
                }

                // --------------------------------
                // If the file creation of access permissions to write into it
                // are incorrect the program throws an exception
                //

                if ((outputFile.isFile()|| outputFile.canWrite())){
                    BufferedWriter fileOut = new BufferedWriter(new FileWriter(outputFile));


                    for(int i=0; i < MAX_DATA; i++) {

                        if (scanUserFile.hasNext()) {

                            for(int i2=0;i2 < arr_.length; i2++) {
                                if (scanUserFile.next().equalsIgnoreCase(arr_[i2][0])){
                                    outString = Integer.toString(calcE.encodeMsg(i2));
                                    fileOut.write(outString + NEWLINE);
                                }
                            }

                        }
                    }


                    fileOut.close();

                }
                else {
                    throw new IOException();
                }
            } // end of try

            // ---------------------------------------------
            // Catch Statements from the try above

            // ---------------------------------------------
            // If the file cannot be found then an exception (error) is generated (thrown) that we have to
            // deal with (catch).
            //
            catch (FileNotFoundException e) {
                System.err.format("File Not Found Exception: %s%n", e);
                e.printStackTrace();
            }

            // ---------------------------------------------
            // If for some reason the output file could not be created we throw an IO Exception
            //
            catch (IOException e) { // in case for some reason the output file could not be created
                System.err.format("IOException: %s%n", e);
                e.printStackTrace();
            }






        }// end main method
}// end main class
