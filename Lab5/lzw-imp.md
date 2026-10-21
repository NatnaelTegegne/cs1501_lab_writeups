# Nat205 Implementation

```java
/*************************************************************************
 *  Compilation:  javac LZW.java
 *  Execution:    java LZW - < input.txt   (compress)
 *  Execution:    java LZW + < input.txt   (expand)
 *  Dependencies: BinaryIn.java BinaryOut.java
 *
 *  Compress or expand binary input from standard input using LZW.
 *
 *  WARNING: STARTING WITH ORACLE JAVA 6, UPDATE 7 the SUBSTRING
 *  METHOD TAKES TIME AND SPACE LINEAR IN THE SIZE OF THE EXTRACTED
 *  SUBSTRING (INSTEAD OF CONSTANT SPACE AND TIME AS IN EARLIER
 *  IMPLEMENTATIONS).
 *
 *  See <a href = "http://java-performance.info/changes-to-string-java-1-7-0_06/">this article</a>
 *  for more details.
 *
 *************************************************************************/
package cs1501_lzw_lab;

public class LZW {
    private static final int R = 256; // number of input chars
    private static int L = 512; // number of codewords = 2^W
    private static final int maxL = 65536;
    private static int W = 9; // codeword width
    private static final int maxW = 16; // the max width

    public static void compress() {
        String input = BinaryStdIn.readString();
        TST<Integer> st = new TST<Integer>();
        for (int i = 0; i < R; i++)
            st.put("" + (char) i, i);
        int code = R + 1; // R is codeword for EOF

        while (input.length() > 0) {
            String s = st.longestPrefixOf(input); // Find max prefix match s.

            BinaryStdOut.write(st.get(s), W); // Print s's encoding.
            if(code == L && W < maxW) { // increase the codeword width when the curr dict capacity is reached
                W++;
               L = 2*L; //if the dict is full, double num of codewords
            }
            int t = s.length();
            if (t < input.length() && code < L) // Add s to symbol table.
                st.put(input.substring(0, t + 1), code++);

            input = input.substring(t); // Scan past s in input.
        }
        BinaryStdOut.write(R, W);
        BinaryStdOut.close();
    }

    public static void expand() {
        String[] st = new String[maxL];
        int i; // next available codeword value

        // initialize symbol table with all 1-character strings
        for (i = 0; i < R; i++)
            st[i] = "" + (char) i;
        st[i++] = ""; // (unused) lookahead for EOF

        int codeword = BinaryStdIn.readInt(W);
        if (codeword == R)
            return; // expanded message is empty string
        String val = st[codeword];

        while (true) {
            BinaryStdOut.write(val);
            // the expander adds the entries one step behind the compressor, so it widens one step later
            if(i==L && W< maxW) {
                W++;
                L= 2*L;
            }
            codeword = BinaryStdIn.readInt(W);
            if (codeword == R)
                break;
            String s = st[codeword];
            if (i == codeword)
                s = val + val.charAt(0); // special case hack
            if (i < L)
                st[i++] = val + s.charAt(0);
            val = s;
        }
        BinaryStdOut.close();
    }
}
```

# How my implementation works

The original LZW code always uses fixed width codewords, so every codeword takes the same number of bits even when the dictionary is still small. In my code I changed it to use variable width codewords that start at 9 bits (W = 9, L = 512 codewords) and grow up to 16 bits (maxW = 16, maxL = 65536). In compress, after writing each codeword I check if the dictionary is full (code == L). If it is and we haven't reached the max width, I increase W by 1 and double L, so the next codewords are written with one more bit and there is room for more entries. Once W hits 16 the dictionary stops growing, and the code < L check stops adding new entries.

In expand I do the same thing, but the interesting part is that the expander adds its entries one step behind the compressor. Because of that, I check i == L right after writing val and before reading the next codeword, so the expander increases its width at the same point the compressor did. If they don't widen at the same time, the expander reads the wrong number of bits and the rest of the output gets messed up.


# Runtime and Memory Analysis

## Runtime
For each codeword, compress finds the longest prefix in the TST, which takes O(M) where M is the length of the match. But input.substring(t) creates a new string every iteration (as the warning at the top says), which is O(N) where N is the length of the input. So in the worst case the compress runtime is O(N^2). Expand reads each codeword once and does constant work for each lookup in the array, so it is O(N) not counting building the output strings.


## Space
The dictionary holds at most 2^16 = 65536 entries. In compress this is the TST, and in expand it is the String array of size maxL. Each entry can be a string of length up to M, so the space is O(2^16 * M).
