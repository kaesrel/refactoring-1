## Refactoring Examples


## WordCount

From Phuwanat's Readability code: https://github.com/OOP2020/pa4-phuwanutj

### haveVowel() Method

In the `src/readability/WordCount.java` class consider this code:

https://github.com/OOP2020/pa4-phuwanutj/blob/master/src/readability/WordCount.java

it has:

```java
    public boolean haveVowel(String word){
        for (int i = 0 ; i < word.length() ; i++ ) {
            String sub = word.substring(i,i+1);
            if (sub.equals("a") || sub.equals("e") || sub.equals("i") || sub.equals("o") || sub.equals("u") || sub.equals("y")
                    || sub.equals("A") || sub.equals("E") || sub.equals("I") || sub.equals("O") || sub.equals("U") || sub.equals("Y")) {
                return true;
            }
        }
        return false;
    }
```

* Refactoring Signs: 
  - the "if" expression is long and complex.
  - many calls to string.equals() when really it just looks for a char.

* Coding Style Error:
  - has space before ";" and ")"

* Refactoring: introduce named constant for the vowels

```java
    public boolean haveVowel(String word){
        final String VOWELS = "aeiouyAEIOUY";
        for (int i = 0; i < word.length(); i++) {
            String ch = word.substring(i, i+1);
            if (VOWELS.contains(ch)) return true;
        }
        return false;
    }
```

* Refactoring: replace indexed for loop with for-each loop
  - also use string.indexOf(char) instead of the more complex string.contains(string)
  - this avoids creating a lot of 1-char string objects inside the loop

```java
    public boolean haveVowel(String word){
        final String VOWELS = "aeiouyAEIOUY";
        for (char c: word.toCharArray()) { 
            if (VOWELS.indexOf(c) >= 0) return true;
        }
        return false;
    }
```

* Refactoring: rename method.

Grammatically, it should be "word *has* a vowel?" (not "have vowel").

In Eclipse or IntelliJ, select the method name and use `Refactor -> Rename` to change it everywhere.

```java
    public boolean hasVowel(String word){
       ...
```

### BufferWordCount() method

Another method in the same file.  It has **two** blocks of code like this:

```java
    for (int i = 0; i < lineArray.length-1; i++) {
        String trimSentence = lineArray[i].trim();
        for (int j = 0; j<trimSentence.length() ; j++) {
            if (trimSentence.charAt(j) == 'a' || trimSentence.charAt(j) == 'e' || trimSentence.charAt(j) == 'i'
                    || trimSentence.charAt(j) == 'o' || trimSentence.charAt(j) == 'u' || trimSentence.charAt(j) == 'y'
                    || trimSentence.charAt(j) == 'A' || trimSentence.charAt(j) == 'E' || trimSentence.charAt(j) == 'I'
                    || trimSentence.charAt(j) == 'O' || trimSentence.charAt(j) == 'U' || trimSentence.charAt(j) == 'Y') {
                this.sentenceCount += 1;
                break;
            }
        }
    }
```

* Refactoring Signs:
  1. Redundantly calling `trimSentence.charAt(j)`.  Should assign it to a local variable.
  2. Duplicate Code. This block does the same thing as `hasVowel`.

* Refactor: use the existing method! 

```java
    for (int i = 0; i < lineArray.length-1; i++) {
        String trimSentence = lineArray[i].trim();
        if (hasVowel(trimSentence)) this.sentenceCount += 1;
    }
```

* Refactor: replace indexed for loop with for-each
  - Note that it is not necessary to trim the sentence before calling hasVowel. This avoid creating another string.

```java
    for (String sentence: lineArray) {
        if (hasVowel(sentence)) this.sentenceCount += 1;
    }
```

* Refactor: this same code block appears **again** in the same method. 

* Refactor: rename method.  `BufferWordCount` violates the Java naming convention.

```java
public void bufferWordCount(BufferedReader br, String line)
```


