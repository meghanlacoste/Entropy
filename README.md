# Entropy
package com.company;


import java.util.*;
import java.io.*;
import java.text.DecimalFormat;
import static com.company.ProjConstants.*;
public class Main {


    private static String NEWLINE = System.getProperty("line.separator");


//-----------------------------------------------------------------------------------------------------------------

    private static void  displayFinalData(String originalMsg[], String encodedMsg[], String decodedMsg []) {


        System.out.println("======================================================================================");
        System.out.println("\n\t\tDisplay File Data:\n");

        System.out.println("\t\tOriginal Data\t\tEncoded Data\t\tDecoded Data\n");
        System.out.println("     ------------------------------------------------------------------\n");

        for (int i = 0; i < MAX_DATA; i++) {
            if (originalMsg[i]!=null){
                System.out.println("\t\t\t" +originalMsg[i] + "\t\t\t\t" + encodedMsg[i] + "\t\t\t\t" + decodedMsg[i]);
            }

        }

        System.out.println("\n     ------------------------------------------------------------------\n");
    }


    //-----------------------------------------------------------------------------------------------------------------

    public static void main(String[] args) {
        // write your code here


        // Creates objects from Entropy Class

        Entropy Encode = new Entropy(ENCODE);
        Entropy Decode = new Entropy(DECODE);


        //-------------------------------------------------------------------

        //Creates new 2d array which encoding/decoding will be based on:

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


        //-------------------------------------------------------------------

        // DECLARE VARIABLES:

        Boolean replicatedWord = false;
        Boolean encodeFile = false;
        String user_msg_filename = null;
        String user_msg_filename_out;
        String outString;
        String userInputFileName;
        String userOutputFileName =null;
        String decodedFileName = null;
        String fileString;
        Boolean fileDone = false;
        Boolean noFiles = false;


        //-------------------------------------------------------------------



        System.out.println("\nEnter the name of the file \n");


        // -------------------------------
        //

        Scanner scanSystemIn = new Scanner(System.in, "UTF-8");

        // --------------------------------
        // Use the Scanner to get the "next" input from "System.in"
        //

        user_msg_filename = scanSystemIn.next();

        // --------------------------------
        // Display the user input now stored in "userInput"
        //

        System.out.println("\nThe user input: " + user_msg_filename);

        // ---------------------------------------------
        // This is where we "try" to process the file
        //


        try {
            // --------------------------------
            // create file, and scanner objects
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
                                // the second column of the array.
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

            // Calls on Entropy Class to calculate the total frequency, number of messages and Max Bits
            //

            Encode.calcTotalFreq(arr_);

            Encode.numMsgs(arr_);

            Encode.calcMaxBits();


            //------------------------------------------------------------------------

            // stores the message probability and message bits required into the array

            for (int i = 0; i < MAX_DATA; i++) {

                // pre-condition: the frequency cannot be invalid
                if (arr_[i][1] != "INVALID") {


                    Encode.calcMsgProb(arr_[i][1]);

                    arr_[i][2] = Double.toString(Encode.calcMsgProb(arr_[i][1]));

                    arr_[i][3] = Integer.toString(Encode.calcMsgBits(arr_[i][2]));


                }
            }

            // calls on Entropy class to create temporary array that will be
            // required for encoding/decoding

            Encode.tempArray(arr_);

            // stores the message number (encoding key) in the String array

           for (int i = 0; i < MAX_DATA; i++) {

                if (arr_[i][0] != "INVALID") {

                  arr_[i][4] = Integer.toString(Encode.encodeMsg(i));

              }

           }


            //Prints results to screen
            //
            System.out.println("==========================================================================================");
            System.out.println("                       Data Result File \n");

            System.out.println("\tData File:............\t\t\t " + user_msg_filename);
            System.out.printf("\tNumber of Messages:....\t%10s\n", Integer.toString(Encode.numMsgs(arr_)));
            System.out.printf("\tMax Number of Bits.....\t%10s\n", Integer.toString(Encode.calcMaxBits()));
            System.out.printf("\tEntropy:...............\t%13.2f\n", Encode.calcEntropy(arr_));
            System.out.println("\n            Message          Frequency         # of bits       Probability ");
            System.out.println("           ------------------------------------------------------------------\n");

            for (int i = 0; i < MAX_DATA; i++) {
                if (arr_[i][0] != "INVALID") {
                    System.out.printf("\t\t %10s      %10s      %10s         %10.2f\n", arr_[i][0], arr_[i][1], arr_[i][3], Double.parseDouble(arr_[i][2]));

                }
            }

            System.out.println("\n           ------------------------------------------------------------------\n");
            System.out.println("\n==========================================================================================\n");


            //------------------------------------------------------------------------------------------------------
            // creates a new file or rewrites file  to store results



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
                outString = "\t\tNumber of Messages:.... \t\t" + Integer.toString(Encode.numMsgs(arr_)) +  NEWLINE;
                fileOut.write(outString);
                outString = "\t\tMaxNumber of Bits:.....  \t\t" + Integer.toString(Encode.calcMaxBits()) + NEWLINE;
                fileOut.write(outString);
                outString = "\t\tEntropy:...............   \t\t" + numberFormat.format(Encode.calcEntropy(arr_))+  NEWLINE + NEWLINE;
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





        //========================================================================================================
        //===================================================================================================

        //  NOW A NEW FILE GIVEN BY THE USER
        // WILL BE ENCODED AND ONLY ITS ENCODED FORM WILL BE WRITTEN INTO A NEW FILE


        // loops so long as there are more files remaining the user would like to encode/decode
        while (!noFiles) {


            System.out.println("\nEnter the name of the file you would like to encode \n");


            // -------------------------------
            //

            Scanner scanner = new Scanner(System.in, "UTF-8");

            // --------------------------------
            // Use the Scanner "userFileName" to get the "next" input from "System.in"
            //

            userInputFileName = scanner.next();

            // --------------------------------
            // Display the user input now stored in "userInput"
            //

            System.out.println("\nThe user input: " + userInputFileName);

            // ---------------------------------------------
            // This is where we "try" to process the file
            //


            String originalMsg [] = new String[MAX_DATA];
            String encodedMsg [] = new String[MAX_DATA];
            String decodedMsg [] = new String[MAX_DATA];


            try {

                // --------------------------------
                // create file, and scanner objects
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
                        originalMsg[i] = fileString;

                        for(int i2=0; i2 < MAX_DATA; i2++) {

                            if (arr_[i2][0] != "INVALID"){
                                if (arr_[i2][0].equalsIgnoreCase(fileString)){

                                    encodedMsg[i] = Integer.toString(Encode.encodeMsg(i2));

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


                //------------------------------------------------------------------------------------------------------
                // creates a new file that stores only encoded message from file above



                userOutputFileName = userInputFileName.replace(".txt","_EC.txt");
                File outputFile = new File(userOutputFileName);

                if (outputFile.createNewFile()){
                    System.out.println(userOutputFileName + " was created"); // if file was created
                }
                else {
                    System.out.println(userOutputFileName + " existed and is being overwritten.");
                    // if file had already existed
                }

                // --------------------------------
                // If the file creation of access permissions to write into it
                // are incorrect the program throws an exception
                //

                if ((outputFile.isFile()|| outputFile.canWrite())){
                    BufferedWriter fileOut = new BufferedWriter(new FileWriter(outputFile));

                    for (int i=0; i < MAX_DATA; i++){

                        if (originalMsg[i] != null) {

                            outString = encodedMsg[i];
                            fileOut.write(outString + NEWLINE);
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


            // -------------------------------------------------------------------------------
            // reads in the file which contains the encoded message ('userOutputFileName')
            // and decodes message back to the original message

            try {

                // --------------------------------
                // create file, and scanner objects
                //   that is the same folder as the iml file
                //

                File decodeFile = new File(userOutputFileName);

                Scanner s = new Scanner(decodeFile, "UTF-8");

                //calls tempArray method using the decode constructor so it is availiable

                Decode.tempArray(arr_);

                for(int i=0; i < MAX_DATA; i++) {

                    // ---------------------------------------------
                    // The scanner checks if there is another integer and prints it
                    // if there is
                    //

                    if (s.hasNext()) {

                        fileString = s.next();


                        for(int i2=0; i2 < MAX_DATA; i2++) {

                            if (arr_[i2][0] != "INVALID"){

                                    decodedMsg[i] = Decode.decodeMsg(arr_, Integer.parseInt(fileString));

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
                        s.close();

                        // A break statement allows us to exit the loop before we have reach the end
                        break;
                    }
                }


                decodedFileName = userOutputFileName.replace("_EC.txt","_DC.txt");
                File outputFile = new File(decodedFileName);

                if (outputFile.createNewFile()){
                    System.out.println(decodedFileName + " was created"); // if file was created
                }
                else {
                    System.out.println(decodedFileName + " existed and is being overwritten."); // if file had already
                    // existed
                }

                // --------------------------------
                // If the file creation of access permissions to write into it
                // are incorrect the program throws an exception
                //

                if ((outputFile.isFile()|| outputFile.canWrite())){
                    BufferedWriter fileOut = new BufferedWriter(new FileWriter(outputFile));

                    for (int i=0; i < MAX_DATA; i++){

                        if (originalMsg[i] != null) {

                            //writes decoded message into new file
                            outString = decodedMsg[i];
                            fileOut.write(outString + NEWLINE);
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



            // calls on private method in the class passing along the string array's of the original file data
            // the encoded file data, and the decoded file data.

            displayFinalData(originalMsg, encodedMsg, decodedMsg);



            String userIn;
            System.out.println("");
            System.out.println("Do you have any more files to process? Type 'NO' if you are done. To continue, enter any other character \n");

            Scanner scan= new Scanner (System.in);
            userIn  = scan.next();



            if(userIn.equalsIgnoreCase("no") ) {

                System.out.println("process finished");
                noFiles = true;

            }






        }// end while !noFiles







    }// end main method
}// end main class
