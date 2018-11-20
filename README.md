package com.company;

public class ProjConstants {


        public static final int INVALID = -1;
        public static final int ENCODE = 150;
        public static final int INVALID_ENCODE = -150;
        public static final int DECODE = 160;
        public static final int INVALID_DECODE = -160;
        public static final int MAX_DATA = 2000;
        public static final int MAX_MSGS = 64;
        public static final int MAX_BITS = 6;
        public static final int INVALID_MSGS = -164;
        public static final int INVALID_BITS = -16;

        private ProjConstants(){
                //this prevents even the native class from calling this constructor as well
                throw new AssertionError();
        }

}


