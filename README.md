# Entropy

package com.company;

import java.util.*;

import static com.company.ProjConstants.*;

/**
 * Created by 13549lac on 13/11/2018.
 */


public class Entropy {

    // Declare/ Initialize Class variables:

    private int numMsgs = INVALID_MSGS;
    private int maxBits =  INVALID_BITS;
    private int  totalFreq = 0;
    private double msgProb =  INVALID;
    private double entropy = INVALID;
    private int  msgBits = INVALID;
    private int  msgEncode = INVALID_ENCODE;
    private int  temp_int_arr [][] = new int [MAX_DATA][3];
    private String decodeMsg = "INVALID_DECODE";
    private boolean encode = false;
    private boolean decode = false;




    //parameterized constructor that takes in one integer parameter

    public  Entropy (int inputMode){

        switch (inputMode) {
            case ENCODE: {

                encode = true;
                decode = false;
            }

            case DECODE: {
                decode = true;
                encode = false;
            }
            default:{

                decode = true;
                encode = true;
            }



        }

    }




    // Function to sort by column
    public static void sortbyColumn(int arr[][], int col)


    {
        // Using built-in sort function Arrays.sort
        Arrays.sort(arr, new Comparator<int[]>() {

            @Override
            // Compare values according to columns
            public int compare(final int[] entry1,
                               final int[] entry2) {

                // To sort in descending order revert
                // the '>' Operator
                if (entry1[col] < entry2[col])
                    return 1;
                else if (entry1[col] == entry2[col]){
                    return 0;
                } else
                    return -1;

            }
        });  // End of function call sort().


    }


    // adds together all the frequencies of the individual words to get the total
    // number of data items stored in the file

    public int calcTotalFreq(String array[][]){

        for (int i=0; i < MAX_DATA; i++) {

            if (array[i][1] != "INVALID") {

                int freq = Integer.parseInt(array[i][1]);
                totalFreq += freq;


            }
        }



        return totalFreq;
    }

    //pre-conditions:
    //      - must have at least one item stored in the array (the total frequency must be valid)
    // returns the number of unique messages stored in the array by adding
    //one to the counter until it reaches the point where no more valid words
    // are stored in the first column of the String array


    public int numMsgs (String array [][]) {
        if (totalFreq != 0) {

            numMsgs = 0;

            for (int i = 0; i < MAX_DATA; i++) {

                if (array[i][0] != "INVALID") {

                    numMsgs += 1;

                }


            }
        } else {

            System.out.print("INVALID NUMBER OF MESSAGES");
        }

        return numMsgs;
    }



    ////pre-conditions:
    //   - must have at least one item stored in the array (the number of messages must be valid)
    //
    // returns an integer value representing the max number of bits required by taking the log base 2
    // of the number of unique messages



    public int calcMaxBits (){

        if (numMsgs != INVALID_MSGS ) {

            double double_maxBits = 0;
            double_maxBits = (Math.log(numMsgs)) / Math.log(2);

            // rounds UP to nearest integer
            maxBits = (int) Math.ceil(double_maxBits);

        } else {

            System.out.print("INVALID NUMBER OF MESSAGES");
        }

        return maxBits;
    }


    public double calcMsgProb (String MsgFreq) {

        if(totalFreq!=0) {

            // the probability is calcuted by dividing the frequency of the message
            //by the total number of data items in the array
            msgProb = (double) Integer.parseInt(MsgFreq)/totalFreq;


        } else {

            msgProb = INVALID;
        }


        return msgProb;
    }




    public int calcMsgBits(String string_msgProb) {

        // calculates the message bits required using the message probability

        double msgProb =  Double.parseDouble(string_msgProb);

        double double_msgBits = - ((Math.log(msgProb)) / Math.log(2));

        // rounds up to the nearest whole number
        msgBits = (int)Math.ceil(double_msgBits);

        return msgBits;
    }




    public void tempArray(String arr_[][]){


        // Initialize the array to a known value
        // - loops over each row using "i", and columns using "j"
        // -  sets all array values to INVALID


        for(int i=0; i < MAX_DATA; i++) {
            for(int j = 0; j < 2; j++){
                temp_int_arr[i][j] = INVALID;
            }
        }


        // the index position of the String array 'i' is stored in the first column of the temporary array
        // the frequency of the word in the row 'i' is stored in the second column of the temporary array

        for(int i=0; i < MAX_DATA; i++) {

            // excecutes as long as there is a string stored in that row position of the String array
            // (not the default "INVALID")

            if (arr_[i][0]!= "INVALID") {

                temp_int_arr[i][0] = i;
                temp_int_arr[i][1] = Integer.parseInt(arr_[i][1]);



            }

        }


        sortbyColumn(temp_int_arr, 1);



        for (int i = 0; i < MAX_DATA; i++) {


            if (temp_int_arr[i][0] != INVALID) {

                temp_int_arr[i][2] = i;

            }

        }

    }



    public int encodeMsg (int index) {

        //pre-condition:
        // - the constructor cannot be given a decode argument (the decode object cannot encode)
        // - will set to INVALID_ENCODE if the this condition not met

        if (encode) {

            for (int i = 0; i < MAX_DATA; i++) {

                // if the index position of the word matches the value stored in
                // the first column of the temporary array, the message number
                // is set to the value stored in the third column of the temporary array

                if (index == temp_int_arr[i][0]) {


                    msgEncode = temp_int_arr[i][2];


                }
            }
        }


        return msgEncode;
    }


    public String decodeMsg (String arr_[][] , int encodeNumb) {

        //pre-condition:
        // - the constructor cannot be given an encode argument (the encode object cannot decode)
        // - will set to INVALID_DECODE if the this condition not met

       if (decode) {


            for (int i = 0; i < MAX_DATA; i++) {

                // pre-conditions: - the value of the index cannot be invalid
                // - the message number given must equal the message number stored in the temporary array

                if ((temp_int_arr [i][0]!= INVALID) && (encodeNumb == temp_int_arr [i][2])){

                    //returns the original word stored in the string array as it's index position
                    // is stored in the first column of the temporary array
                    decodeMsg = arr_[temp_int_arr[i][0]][0];
                }

            }

        }


        return decodeMsg;
    }




    public double calcEntropy (String arr_ [][]) {

        entropy = 0;

        double message_prob;

        int message_bits;

        // finds the sum of the products of each word's probability and it's corresponding number of bits

        for (int i = 0; i < MAX_DATA; i++) {

            if (arr_[i][0] != "INVALID"){

                message_prob = Double.parseDouble(arr_[i][2]);

                message_bits = Integer.parseInt(arr_[i][3]);

                entropy += (message_prob * message_bits);
            }

        }



        return entropy;
    }






}// end Entropy Class
