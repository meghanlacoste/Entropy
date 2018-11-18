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
    private int  msgEncode = 0;
    private int  temp_int_arr [][] = new int [MAX_DATA][3];


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


    // returns the number of unique messages stored in the array
    public int numMsgs (String array [][]) {
        if (totalFreq != 0) {
            numMsgs = 0;
            for (int i = 0; i < MAX_DATA; i++) {
                if (array[i][0] != "INVALID") {
                    numMsgs += 1;
                }


            }
        }

        return numMsgs;
    }



    //
    public int calcMaxBits (){
        double  double_maxBits = 0 ;
        double_maxBits =(Math.log(numMsgs)) / Math.log(2);

        // rounds up always to nearest integer and
        maxBits = (int)Math.ceil(double_maxBits);

        return maxBits;

    }


    public double calcMsgProb (String MsgFreq) {

        if(totalFreq!=0) {


            msgProb = (double) Integer.parseInt(MsgFreq)/totalFreq;


        } else {

            msgProb = INVALID;
        }


        return msgProb;
    }




    public int calcMsgBits(String string_msgProb) {

        double msgProb =  Double.parseDouble(string_msgProb);

        double double_msgBits = - ((Math.log(msgProb)) / Math.log(2));

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


        for(int i=0; i < MAX_DATA; i++) {
            if (arr_[i][0]!= "INVALID") {
                temp_int_arr[i][0] = i;
                temp_int_arr[i][1] = Integer.parseInt(arr_[i][1]);
                // System.out.println(temp_int_arr[i][0] + " " + temp_int_arr[i][1]);


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

        for (int i = 0; i < MAX_DATA; i++) {

            if (index == temp_int_arr[i][0]){
                msgEncode = temp_int_arr[i][2];

            }
        }


        return msgEncode;
    }





/*





- if there were 2 values Word1 and Word2 the entropy would be calculated by Entropy = -[ probWord (log2 probWord) +  probWord2 (log2 probWord2) ]
Preconditions:
 	- checks that there is more than zero message items.

+calcEntropy  that returns a double precision value representing the entropy, or INVALID
Loop through all all msgs and the corresponding msgBits.
Entropy +=  (msgProb x msgBits)
End loop

+getEntropy
Returns entropy


+ encodeMsg(string prevNumb) that returns an integer value that represents the message number for the String parameter that it was given. Recall that the most frequently used messages should be the lowest message numbers i.e. sort the messages by frequency before and assigning message numbers

Parse string prevNumb into int
msgNumb =  prevNumb + 1

+getEncodeMsg
 Returns msgNumb


+ decodeMsg that returns a String that represents the message number that it was given. Recall that the most frequently used messages should be the lowest message numbers i.e. sort the messages by frequency before and assigning message numbers




 */





}// end Entropy Class
