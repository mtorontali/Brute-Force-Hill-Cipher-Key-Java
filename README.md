# Brute-Force-Hill-Cipher-Key-Java
A java program the finds a hill cipher key by brute forcing with known ciphertext and plaintext.

```java
// Matthew Torontali
// 9/22/20
// Used modified code from Arup Guha's Hill cipher encrypter program called hill.java found at:
// http://www.cs.ucf.edu/~dmarino/ucf/cis3362/progs/hill.java

// Imports
import java.lang.Math;

public class findHillCipherKey
{

    // Global variable n for n x n matrix key and global variable to hold the key
    public static int n;
    public static int[][] key;

    // Brute force Hill Cipher key using Math.random() to test different keys randomly until cipherGuess matches
    // cipher text.
    public static void findKey(String plainText, String cipherText)
    {
        // Initialize variable to hold cipher guess
        String cipherGuess = "";
        // Initialize Math.random() variables.
        int max = 50;
        int min = 0;
        int range = max - min + 1;

        while (!cipherGuess.equals(cipherText))
        {
            // Initialize 2D array to hold the n x n matrix key
            key = new int[n][n];

            // Iterate through the 2D key matrix array and set each spot to a random number to test
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    key[i][j] = (int) (Math.random() * range) + min;
                }
            }

            // Call function to encrypt plaintext with Hill Cipher
            cipherGuess = encrypt(plainText);
            System.out.println(cipherGuess);
        }
    }

    // Print out the key that was found
    // Iterate through the 2D array to print the key matrix
    public static void printKey()
    {
        System.out.println("Key found: ");
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                System.out.println(key[i][j]);
    }

    // Modified version of Arup Guha's encrypt method for Hill Cipher from hill.java used with permission
    public static String encrypt(String plainText)
    {

        // Pad until plain text is multiple of key size.
        while (plainText.length() % n != 0) plainText = plainText + "X";

        // Put together the encryption of each block and return.
        String ret = "";
        for (int i = 0; i < plainText.length(); i += n)
        {
            ret = ret + encryptBlock(plainText.substring(i, i + n));
        }

        // Return response
        return ret;
    }

    // Encrypt Block method used by Encrypt method made by Arup Guha from his hill.java
    public static String encryptBlock(String block)
    {

        // Character array to hold return response.
        char[] ret = new char[n];
        for (int i = 0; i < n; i++)
        {
            int sum = 0;
            for (int j = 0; j < n; j++)
            {
                sum += (key[i][j] * (block.charAt(j) - 'A'));
            }
            ret[i] = (char)('A' + (sum % 26));
        }

        // Return as a string.
        return new String(ret);
    }

    // Main method
    public static void main(String[] args)
    {

        // Initialize variables.
        String cipherText = "ABYE";
        String plainText = "BOOK";
        n = 2; // Set n equal to the size of the key matrix e.g. 2 x 2 matrix

        // Finds the key by brute forcing with Math.random()
        findKey(plainText, cipherText);

        // Print out the key that was found
        printKey();
    }
}
```
