```
fun main ( ) {  
//----------------------------------------------------------------------------------------------------------------------  
    // NULLABLE VALUE  
    println ( " Enter x1 ( must be a valid number ) : " )  
    val x1 = readln ( ) .toInt ( ) // possible : type is Int  
  
    println ( " Enter x2 ( can be a number or invalid ) : " )  
    val x2 = readln ( ) .toIntOrNull ( ) // possible : type Int?  
    // the question mark is for null safety means x2 can be Int or Null  
//----------------------------------------------------------------------------------------------------------------------  
    // OPERATIONS ON NULLABLE VALUE  
    //val x3 : Int = readln ( ) .toIntOrNull ( ) ; not Possible : type mismatched : Int != Int?    println ( " Enter x3 ( for Int? type ) : " )  
    val x3 : Int? = readln ( ) .toIntOrNull ( ) // possible : type matched  
  
    // Operations on Nullable values ( !! )    /*    val x4 = x2 + x3 ; not possible because both value can be null  
    here , operation cannot be performed    So that's why to make code run we have to use !! operator after every nullable value    therefore    val x4 = x2!! + x3 ; not possible    val x4 = x2 + x3!! ; not possible    since both can value be null so !! needs to be used for every variable     */  
    // We only calculate x4 if x2 and x3 were entered successfully    val x4 = x2!! + x3!! // possible , but can throw a NullPointerException if there is a null value  
    println("Addition of x2 + x3 is : $x4")  
//----------------------------------------------------------------------------------------------------------------------  
    // DEFAULT VALUE IN CASE OF NULLABLE OR INVALID VALUE  
    // 1 ] Elvis Operator ( ?: ) [    // To prevent NullPointerException we can assign a default value in case of invalid value using ?:    println ( " Enter x5 ( toInt with Elvis ) : " )  
    val x5 = readln ( ) .toIntOrNull ( ) ?: 0  
  
    println ( " Enter x6 ( toIntOrNull with Elvis ) : " )  
    val x6 = readln ( ) .toInt ( ) ?: 0  
    // if the value on the left side of elvis operator is NULL so the default right side value will be assigned using this operator  
  
    // 2 ] Safe Call operator ( ?. )    // It provides a safe way to access a property or call a method on an object that might be null .    // If the variable is not null , the call proceeds normally . If the variable is null , the operation is skipped , and the entire expression simply returns null instead of throwing a NullPointerException ( NPE ) .    // NOTE : After ?. there can only be a function call    println ( " Enter x7 : " )  
    val x7 : Int? = readln ( ) .toIntOrNull ( )  
    val x8 = x7?.inc ( ) // this is increment fun which will be executed if the value on left is NOT NULL  
    // If x7 is Int let's say 4 then x8 will be 5    // If x7 is NULL then x8 will be null  
    println ( " Result of x8 ( x7 incremented ) : $x8 " )  
  
    /* Difference between Safe Call & Elvis Operator  
    +----------+----------------+--------------------------+-----------------------+--------------------------+    | OPERATOR | NAME           | LOGIC ( IF NULL )        | LOGIC ( IF NOT NULL ) | TYPICAL USE CASE         |    +----------+----------------+--------------------------+-----------------------+--------------------------+    | ?.       | Safe Call      | Returns NULL             | Executes the access   | Accessing properties     |    | ?:       | Elvis Operator | Returns the RIGHT side   | Returns the LEFT side | Providing default values |    +----------+----------------+--------------------------+-----------------------+--------------------------+    */  
  
  
//----------------------------------------------------------------------------------------------------------------------  
    // ADVANCED NULL SAFETY FEATURES  
    // 3 ] Smart Casts [    // If you check for null , Kotlin treats the variable as non-nullable inside that block    println ( " Checking x7 for Smart Cast ... " )  
    if ( x7 != null ) {  
        // No ?. or !! needed here  
        val square = x7 * x7  
        println ( " Smart Cast : x7 squared is $square " )  
    }  
  
  
    // 4 ] Safe Cast ( as? ) [  
    // Tries to cast to a type ; returns null if it fails instead of crashing    println ( " Enter something to try a Safe Cast to Int : " )  
    val input : Any = readln ( )  
    val safeInt : Int ? = input as? Int  
    println ( " Safe Cast result : $safeInt " )  
  
  
    // 5 ] The let function [  
    // Executes a block of code ONLY if the value is not null    println ( " Enter an email ( or leave blank ) : " )  
    val email : String ? = readln ( ) .takeIf { it .isNotBlank ( ) }  
    email ?.let {  
        println ( " Sending verification to : $it " )  
    }  
  
  
    // 6 ] Nullability in Collections [  
    // You can have a list that contains nulls    val numbersWithNulls : List < Int ? > = listOf ( 10 , null , 30 )  
    val onlyNumbers = numbersWithNulls .filterNotNull ( ) // Removes nulls  
    println ( " Clean collection : $onlyNumbers " )  
  
//----------------------------------------------------------------------------------------------------------------------  
  
/* Expanded Comparison Table  
+----------+-------------------+--------------------------+-----------------------+----------------------------------+  
| OPERATOR | NAME              | LOGIC ( IF NULL )        | LOGIC ( IF NOT NULL ) | TYPICAL USE CASE                 |  
+----------+-------------------+--------------------------+-----------------------+----------------------------------+  
| ?.       | Safe Call         | Returns NULL             | Executes the access   | Accessing properties/methods     |  
| ?:       | Elvis Operator    | Returns the RIGHT side   | Returns the LEFT side | Providing default values         |  
| !!       | Not-null Assertion| Throws NPE ( Crash )     | Unwraps the value     | When you are 100% sure           |  
| as?      | Safe Cast         | Returns NULL             | Performs the cast     | Safe Type Conversion             |  
| ?.let {} | Scope Function    | Skips the block          | Executes the block    | Running code on non-null objects |  
+----------+-------------------+--------------------------+-----------------------+----------------------------------+  
*/  
}
```