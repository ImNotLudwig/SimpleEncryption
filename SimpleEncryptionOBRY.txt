import java.util.Scanner;
import java.lang.Math;

/**
 * 
 * @author Y'Shua O'Brien
 * @version 09/15/2021
 */

public class SimpleEncryptionOBRY
{
  public static void main(String args[])
  {
    //=== DECLARATIONS ===
    String str, strCopy="";
    int rotateAmount, encryptOrDecrypt;
    
    //=== PROGRAM DESCRIPTION ===
    
    //=== INPUT ===
    str=getStrInput();
    rotateAmount=getRotateAmount();
    encryptOrDecrypt=askEncryptOrDecrypt();
    
    //=== PROCESSING ===
    strCopy=encrypt(str, rotateAmount, encryptOrDecrypt); //Switches the string chars (left) in the alphabet by the ratateAmount
    //strCopy=decrypt(str, rotateAmount); //Switches the string chars (right) in the alphabet by the ratateAmount
    
    //=== OUTPUT ===
    outputEncryption(encryptOrDecrypt, str, strCopy);
  }
  
  /**
   * This will ask the user for a string to be encrypted/decrypted.
   * Numbers or special characters will not be encrypted/decrypted, 
   * this program will ignore them and keep encrypting/decrypting characters from the alphabet.
   * 
   * @return str will return the users input as is
   */
  public static String getStrInput()
  {
    Scanner strIn=new Scanner(System.in);
    String str;
    
    System.out.println("Print phrase to encrypt or decrypt. Any numbers or special characters will not be encrypted/decrypted.");
    str=strIn.nextLine();
    
    return str;
  }
  
  /**
   * This will get the amount that the string will be rotated.
   * Any letters from the alphabet will not be accepted, the console will re-prompt for input if one is entered.
   * 
   * @return rotateAmount will return the amount the string will rotate
   */
  public static int getRotateAmount()
  {
    Scanner input=new Scanner(System.in);
    
    String rotateAmountInput="";
    int rotateAmount=0;
    boolean gotInput=true;
    
    do{
      System.out.println("Enter the amount you want it to be rotated (1-25)");
      rotateAmountInput=input.nextLine();
      try
      {
        rotateAmount=Integer.parseInt(rotateAmountInput); //will check for letters
        gotInput=false;
      }
      catch (NumberFormatException error)
      {
        System.out.println("Invalid input. Must be a number from (1-25)");
      }
    } while (gotInput);
    
    return rotateAmount;
  }
  
  /**
   * This will ask the user if they would like to encrypt or decrypt a string, (or go right or left in the alphabet).
   * 
   * @return encryptOrDecrypt will return the value 1 or 2, and the value will be used for printing the correct output in the end
   */
  public static int askEncryptOrDecrypt()
  {
    Scanner input=new Scanner(System.in);
    String encryptOrDecryptInput="";
    int encryptOrDecrypt=0;
    boolean gotInput=false;
    
    while (encryptOrDecrypt==0)
    {
      do{
        System.out.println("Press '1' for encryption");
        System.out.println("Press '2' for decryption");
        encryptOrDecryptInput=input.nextLine();
        try
        {
          encryptOrDecrypt=Integer.parseInt(encryptOrDecryptInput); //will check for letters
          gotInput=false;
        }
        catch (NumberFormatException error)
        {
          System.out.println("Invalid input. No letters, spaces, or special characters.");
          gotInput=true;
        }
        
        if (encryptOrDecrypt>=3)
        {
          System.out.println("Invalid input. Input must be a range of 1-2.");
          encryptOrDecrypt=0;
          gotInput=true;
        }
      } while (gotInput);
    }
    
    return encryptOrDecrypt;
  }
  
  /**
   * This will take the original string and rotate amount to change each letter in a string by going through the alphabet (can be left or right in this case) according to the rotateAmount value.
   * Spaces inside the string will be avoided.
   * 
   * @param str is the users original string
   * @param rotateAmount is the amount each char will be rotated (left) in the alphabet
   * @param encryptOrDecrypt will decide if the program will be encrypting or decrypting a string (going right or left in the alphabet)
   * 
   * @return strCopy will return the new encrypted string
   */
  public static String encrypt(String str, int rotateAmount, int encryptOrDecrypt)
  {
    String al="ABCDEFGHIJKLMNOPQRSTUVWXYZ", al2="abcdefghijklmnopqrstuvwxyz", strCopy="";
    int rotateRemainder;
    
    if (encryptOrDecrypt==1)
    {
      for (int i=0; i<str.length(); i++)
      {
        if ((int)str.charAt(i) == (32))
        {
          strCopy=strCopy + " ";
        }
        
        for (int x=0; x<al2.length(); x++)
        {
          if (str.charAt(i) == (al2.charAt(x)))
          {
            try
            {
              strCopy=strCopy + al2.charAt(x+rotateAmount);
            }
            catch (StringIndexOutOfBoundsException error)
            {
              rotateRemainder=(x+rotateAmount)-26;
              strCopy=strCopy + al2.charAt(rotateRemainder);
            }
          }
          else if (str.charAt(i) == (al.charAt(x)))
          {
            try
            {
              strCopy=strCopy + al.charAt(x+rotateAmount);
            }
            catch (StringIndexOutOfBoundsException error)
            {
              rotateRemainder=(x+rotateAmount)-26;
              strCopy=strCopy + al.charAt(rotateRemainder);
            }
          }
        }
      }
    }
    else
    {
      for (int i=0; i<str.length(); i++)
      {
        if ((int)str.charAt(i) == (32))
        {
          strCopy=strCopy + " ";
        }
        
        for (int x=0; x<al2.length(); x++)
        {
          if (str.charAt(i) == (al2.charAt(x)))
          {
            try
            {
              strCopy=strCopy + al2.charAt(x-rotateAmount);
            }
            catch (StringIndexOutOfBoundsException error)
            {
              rotateRemainder=(x-rotateAmount)+26;
              strCopy=strCopy + al2.charAt(rotateRemainder);
            }
          }
          else if (str.charAt(i) == (al.charAt(x)))
          {
            try
            {
              strCopy=strCopy + al.charAt(x-rotateAmount);
            }
            catch (StringIndexOutOfBoundsException error)
            {
              rotateRemainder=(x-rotateAmount)+26;
              strCopy=strCopy + al.charAt(rotateRemainder);
            }
          }
        }
      }
    }
    return strCopy;
  }

/**
 * This will print the users original string,
 * and then the encryption or decryption of the users original string afterwards.
 * 
 * @param encryptOrDecrypt holds a value of 1 or 2 and decides which output strings will be printed
 * @param str is the users original string
 * @param strCopy prints the new encrypted or decrypted string
 */
public static void outputEncryption(int encryptOrDecrypt, String str, String strCopy)
{
  if (encryptOrDecrypt==1)
  {
    System.out.println("Users input: " + str);
    System.out.println("Encryption text: " + strCopy);
  }
  else
  {
    System.out.println("Users input: " + str);
    System.out.println("Decryption text: " + strCopy);
  }
}
}