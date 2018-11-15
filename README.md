# Entropy

package com.company;

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
    private int  msgEncode = INVALID;


    //    The number of messages is equal to the highest index number
    //   Starting at MAX_DATA  loop backwards
    //   If the value at the index position does not equal "INVALID"
    //
    //  Break;
    // The index position of that value is the number of messages in the file.



    public int calcTotalFreq(String array[][]){

        for (int i=0; i < MAX_DATA; i++) {
            if (array[i][1] != "INVALID") {
                int freq = Integer.parseInt(array[i][1]);
                totalFreq += freq;

            }
        }

        return totalFreq;
    }



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




    public int calcMaxBits (){
      double  double_maxBits = 0 ;
        double_maxBits =(Math.log(numMsgs)) / Math.log(2);

        // rounds up always to nearest integer and
        maxBits = (int)Math.ceil(double_maxBits);

        return maxBits;

    }

    public double calcMsgProb (String MsgFreq) {

        if(totalFreq!=0) {

            msgProb = ((Integer.parseInt(MsgFreq))/totalFreq );


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





/*





//takes an String parameter for the messages and calculates number of bits required to //encode the message
+ calcMsgBits(string msg, msgProb)
msgBits = - log2  msgProb


//returns an integer that represents the number of bits that could be required to encode the messages, or INVALID_BITS
+getMsgBits
Return msgBits


+ calcMsgProb (int totalFreq, String msgFreq)
that returns a double precision value, for the message parameter that it was given (a String), or INVALID
Int msgFreq = Parse string array [][x] to int
 msgProb= msgFreq divided by totalFreq

+ getMsgProb
Return msgProb


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
