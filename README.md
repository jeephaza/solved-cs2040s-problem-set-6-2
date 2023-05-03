Download Link: https://assignmentchef.com/product/solved-cs2040s-problem-set-6-2
<br>
<h1>Problem 6.          (Automatic Writing)</h1>

Writing-intensive modules can be hard: so many 10-page essays, and not nearly enough time to catch up on the latest e-lectures (please watch them!). CS2040S is here to help. For this problem set, you will develop an automatic writing program that can easily produce pages and pages of new text. And it will adapt to your chosen style. If you use an old essay as input, your new essay will sound just like it was written by you! If you use Shakespeare as input, your new essay will sound as if it was written by the Bard himself.

The basic idea is to take an input text and calculate its statistical properties. For example, given a specific string “prope”, what is the probability that the next letter is an ‘r’? What is the probability that the next letter is a ‘q’? Your program will take a text as input, calculate this statistical information, and then use it to produce a reasonable output text.

Claude Shannon first suggested this technique in a seminal paper <em>A Mathematical Theory of Communication </em>(1948). This paper contained many revolutionary ideas, but one of them was to use a <em>Markov Chain </em>to model the statistical properties of a particular text. Markov Chains are now used everywhere; for example, the Google PageRank algorithm is built on ideas related to Markov Chains.

<h1>Markov Models</h1>

Given a text, you can build up the Markov Model. A Markov Model captures the frequency of a specific letter/character appearing <strong>after </strong>a specific preceding string (which can be of varying length). <strong>The </strong><em>order </em><strong>of the Markov model is the length of that preceding string.</strong>

For example, if we have the following text:

a b d a c a b d a c b d a b d a c d a

We can build the following Markov Model of order 1:

<table width="87">

 <tbody>

  <tr>

   <td width="32">a</td>

   <td width="16">b</td>

   <td width="38">1<em>/</em>2</td>

  </tr>

  <tr>

   <td width="32">a</td>

   <td width="16">c</td>

   <td width="38">1<em>/</em>2</td>

  </tr>

  <tr>

   <td width="32">b</td>

   <td width="16">d</td>

   <td width="38">1</td>

  </tr>

  <tr>

   <td width="32">c</td>

   <td width="16">a</td>

   <td width="38">1<em>/</em>3</td>

  </tr>

  <tr>

   <td width="32">c</td>

   <td width="16">b</td>

   <td width="38">1<em>/</em>3</td>

  </tr>

  <tr>

   <td width="32">c</td>

   <td width="16">d</td>

   <td width="38">1<em>/</em>3</td>

  </tr>

  <tr>

   <td width="32">d</td>

   <td width="16">a</td>

   <td width="38">1</td>

  </tr>

 </tbody>

</table>

This implies the following:

<ul>

 <li>After the string ‘a’, half the time you find a ‘b’, and half the time you find a ‘c’.</li>

 <li>After the string ‘b’, you always find a ‘d’.</li>

 <li>After the string ‘c’, one-third of the time you find letters ‘a’, ‘b’, or ‘d’ respectively (i.e., they are equally common after a ’c’).</li>

 <li>After the string ‘d’, you always find an ‘a’.</li>

</ul>

You can think of these as probabilities (though so far, there is no randomness at all). Notice that in the text above the table, there are three instances when the character ‘a’ is followed by a ‘b’, and there are three instances when ‘a’ is followed by a ‘c’. Similarly, ‘b’ is always followed by a ‘d’, and ‘d’ is always followed by an ‘a’. The character ‘c’ is followed by an ‘a’ once, a ‘b’ once, and a ‘d’ once.

A Markov Model of order 2 captures how likely a given letter is to follow a string of length 2. Suppose we have the following text:

a b c d a b d d a b c d d a b d

Here we have an example of Markov Model of order 2 built by the text above:

<table width="95">

 <tbody>

  <tr>

   <td width="41">ab</td>

   <td width="16">c</td>

   <td width="38">1<em>/</em>2</td>

  </tr>

  <tr>

   <td width="41">ab</td>

   <td width="16">d</td>

   <td width="38">1<em>/</em>2</td>

  </tr>

  <tr>

   <td width="41">bc</td>

   <td width="16">d</td>

   <td width="38">1</td>

  </tr>

  <tr>

   <td width="41">bd</td>

   <td width="16">d</td>

   <td width="38">1</td>

  </tr>

  <tr>

   <td width="41">cd</td>

   <td width="16">a</td>

   <td width="38">1<em>/</em>2</td>

  </tr>

  <tr>

   <td width="41">cd</td>

   <td width="16">d</td>

   <td width="38">1<em>/</em>2</td>

  </tr>

  <tr>

   <td width="41">da</td>

   <td width="16">b</td>

   <td width="38">1</td>

  </tr>

  <tr>

   <td width="41">dd</td>

   <td width="16">a</td>

   <td width="38">1</td>

  </tr>

 </tbody>

</table>

Notice that in the text above, there are two instances when the string ’ab’ is followed by the letter ‘c’ and two instances when the string ’ab’ is followed by the letter ‘d’. After the string ‘bc’, you always get the letter ‘d’, and after the string ‘bd’, you always get the letter ‘d’, etc.

<h1>Producing a New Text</h1>

Once you have your Markov Model, you can go about generating a new text. You need to start with a seed string of the same length as the order of the Markov Model. For example, if the Markov Model is of order 6, you need to start with a string of length 6.

<strong>We use the term ‘kgrams’ to refer to the </strong><em>k</em><strong>-character strings, where </strong><em>k </em><strong>is the order. </strong>In order to generate the next character, you look back at the previous <em>k </em>characters (inclusive of the current last character). Look up that kgram in your Markov Model, and find the frequency that each character appears after that kgram. If the kgram never appeared in your Markov Model, then your newly-generated text is completed. Otherwise, you randomly choose the next character based on the probability distribution indicated by the Markov Model.

Once you have found the next character that way, you add it to the end of your string, and repeat the process as many times as you want!

<h1>Problem Details</h1>

For this problem, you have been provided with the TextGenerator Java class, more information will be provided in the section below. You will submit one Java class: MarkovModel.

<strong>Problem 6.a.           </strong>Implement <strong>only </strong>the following 4 methods:

MarkovModel(int order, long seed): Constructs a Markov Model of the specified order. You can assume that the order will be at least 1. The seed should be used to initialize the pseudorandom number generator that your class will use (the template code already does this). A pseudorandom number generator’s number sequence is completely determined by the seed. So, if a pseudorandom number generator is reinitialized with the same seed, it will produce the same sequence of numbers.

void initializeText(String text): This builds your Markov Model based on the specified text. When this routine is complete, your class would have computed the table that maps each kgram to the <strong>frequency </strong>that each ASCII character follows that kgram (where <em>k </em>is the order of the Markov Model).

int getFrequency(String kgram): Returns the number of times the specified string kgram appears in the input text. The behaviour of this method is only defined if the length of the kgram is equal to the order of the Markov Model (note that you are also expected to handle invalid cases explicitly). This should return a number zero or greater. For example, in the Figure 1 below, the frequency of kgram ’aa’ is 1. (Notice we do not count the kgram which appears at the end of the text, where nothing may follow it)

int getFrequency(String kgram, char c): Returns the number of times the specified character c appears immediately after the string kgram in the input text. The behaviour of this method is only defined if the length of the kgram is equal to the order of the Markov Model. For example, in the Figure 1 below, the frequency of ’g’ appearing after the kgram ’ga’ is 4. If the string kgram never appears in the original text, then you should return 0.

<strong>Problem 6.b.              </strong>Next, implement the nextCharacter method:

char nextCharacter(String kgram): Returns a random character chosen according to the getFrequency(kgram,c)

Markov Model. The probability of a character ‘c’ should be equal to                                                   .

getFrequency(kgram)

That is, the probability of character ‘c’ should be equal to the frequency that ‘c’ follows the string kgram in the text. If there is <em>no possible </em>next character (e.g., because the kgram does not appear in the text, or only appears at the very end of the text), then return the specially defined token final char NOCHARACTER = 0 (defined in the template file). The kgram must be the length specified by the order of the Markov model. To generate the random choice, you must use the pseudorandom number generator with the specified seed. <strong>You must use the process described in ”Random character generation” below to generate the random choice.</strong>

You may assume that every character in the text is an <a href="https://theasciicode.com.ar/">ASCII character</a> (inclusive of the extended ASCII character), except for 0 (the NULL character), which does not count as a character. Figure 1 has an example of the information computed by your Markov Model for a specific example string.

<table width="246">

 <tbody>

  <tr>

   <td width="39"> </td>

   <td width="16"> </td>

   <td colspan="3" width="78">frequency</td>

   <td colspan="3" width="114">probability</td>

  </tr>

  <tr>

   <td width="39"> </td>

   <td width="16"> </td>

   <td width="31">a</td>

   <td width="27">c</td>

   <td width="20">g</td>

   <td width="46">a</td>

   <td width="38">c</td>

   <td width="30">g</td>

  </tr>

  <tr>

   <td width="39">aa</td>

   <td width="16">1</td>

   <td width="31">1</td>

   <td width="27">0</td>

   <td width="20">0</td>

   <td width="46">1</td>

   <td width="38">0</td>

   <td width="30">0</td>

  </tr>

  <tr>

   <td width="39">ag</td>

   <td width="16">4</td>

   <td width="31">2</td>

   <td width="27">0</td>

   <td width="20">2</td>

   <td width="46">2<em>/</em>4</td>

   <td width="38">0</td>

   <td width="30">2<em>/</em>4</td>

  </tr>

  <tr>

   <td width="39">cg</td>

   <td width="16">1</td>

   <td width="31">1</td>

   <td width="27">0</td>

   <td width="20">0</td>

   <td width="46">1</td>

   <td width="38">0</td>

   <td width="30">0</td>

  </tr>

  <tr>

   <td width="39">ga</td>

   <td width="16">5</td>

   <td width="31">1</td>

   <td width="27">0</td>

   <td width="20">4</td>

   <td width="46">1<em>/</em>5</td>

   <td width="38">0</td>

   <td width="30">4<em>/</em>5</td>

  </tr>

  <tr>

   <td width="39">gc</td>

   <td width="16">1</td>

   <td width="31">0</td>

   <td width="27">0</td>

   <td width="20">1</td>

   <td width="46">0</td>

   <td width="38">0</td>

   <td width="30">1</td>

  </tr>

  <tr>

   <td width="39">gg</td>

   <td width="16">3</td>

   <td width="31">1</td>

   <td width="27">1</td>

   <td width="20">1</td>

   <td width="46">1<em>/</em>3</td>

   <td width="38">1<em>/</em>3</td>

   <td width="30">1<em>/</em>3</td>

  </tr>

 </tbody>

</table>

<strong>Figure 1: </strong>Markov Model produced by the string gagggagaggcgagaaa.

<h1>Implementation advice</h1>

There are several approaches to designing the MarkovModel class. Here we provide some tips for how you might implement it.

<strong>Basic structure.</strong>

There are two standard ways you might store the information about the kgram. You might use a symbol table (i.e., a hash table) that maps strings of length <em>k </em>(where <em>k </em>is the order of the Markov model) to an array containing 255 integers, one representing each possible ASCII character. The array records the number of time each character follows the given string. For example, the character ‘a’ is 97, in ASCII. Hence, if <em>k </em>= 2, given an input string ‘xya’, you would add to your hash table an entry with the key equal to ‘xy’ and the value equal to an array of integers where value[97] == 1.

Alternatively, you might use a symbol table that maps strings of length <em>k </em>to <em>another </em>symbol table; the second symbol table maps ASCII characters to integers. Again, these integers represent the frequency that a given character follows the initial string. You may also use an alternate solution, as long as it efficiently supports the required operations.

For the purposes of this Problem Set, <strong>you should use a Hash Table and NOT use Tries or TreeSet or TreeMap </strong>to store information about the kgram. If you are unsure whether you can use a particular data structure, please post in the forums to seek clarifications.

<h2>Random character generation</h2>

Presumably you have now built your Markov Model, and now know the proper frequencies that each character is followed by each kgram. In order to generate the next character in the text, you need to make a random choice. For testing purposes, you must use the random number generator specified in the template code: java.util.Random, and you must use the seed specified in the constructor (via a setSeed call on the random number generator object which is already in the template code). If you run the same test twice with the same seed, you will get the same answer! This is very useful for testing.

A second requirement (for testing purposes) is that you choose the next character randomly in the following way. Here is an example. Imagine that for the given kgram, there are four characters that might follow it: ‘j’, ‘m’, ‘p’, ‘z’ (all the other characters appear zero times after this kgram). The ‘j’ appears 2 times, the ‘m’ appears 5 times, the ‘p’ appears 3 times, and the ‘z’ appears 1 time. Thus this particular kgram appears 11 times (not counting the end of the text, where nothing may follow it).

To choose a random character, you first select a random integer from [0<em>,</em>10], i.e., a range containing 11 integers. You can do this by calling generator.nextInt(11). You can then partition this range of random numbers:

<ul>

 <li>if you get {0<em>,</em>1} then you return ’j’;</li>

 <li>if you get {2<em>,</em>3<em>,</em>4<em>,</em>5<em>,</em>6}, then you return ’m’;</li>

 <li>if you get {7<em>,</em>8<em>,</em>9}, then you return ‘p’;</li>

 <li>if you get {10}, then you return ‘z’.</li>

</ul>

You <strong>must </strong>process the possible letters in alphabetic order (or by order of their ASCII character values).

In general, if there is a set of <em>C </em>possible next characters which appear a total of <em>N </em>times after the <em>k</em>-letter prefix, you should choose a random number from [0<em>,N </em>− 1], and then go through the set in alphabetical order to determine which character was chosen by the random selection, weighting each character by the number of times it appears.

The reason we ask you to choose the random character in this way is twofold: first, it is a reasonably efficient way to sample from a distribution, and second, it will allow us to ensure that every solution produces the same random sequence (which makes it easier to test).

<h2>Other tips</h2>

Here are a few other things that may be useful:

<ul>

 <li>You can treat an ASCII character either as a char or an int. If you have a char ch, you can store it in an integer as follows: int iChar = (int) ch. Similarly, you can do the opposite: char newChar = (char) iChar. In general, it is not recommended that you force Java to ignore types like this. However, in this case, as long as you are careful to not use any values above 255, this is a safe thing to do. (If your integer is bigger than 255 and you put it in a char, you may not get what you expect.) This is convenient, for example, if you want to enumerate all the ASCII characters using a for loop, since you can just count up to 255.</li>

 <li>A Java <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/HashMap.html">HashMap</a> is a parameterized data type. That means that when you create it, you have to specify what the key and value types are. For example, if you want to use a key type K and a value type V, you would use the class HashMap&lt;K, V&gt;. In your case today, if you want a key type of String and a value type of Integer, you use a HashMap&lt;String, Integer&gt;. (Whenever you need to use the name of the class, whether to declare the variable or to create a new object, you can just use that full name including the String and Integer )</li>

 <li>Notice, though, that for your key and value types, you cannot use primitive types like int or char. Instead, you have to use the wrapper class version of these types: Integer, Character, etc. That is why, above, we used Integer as the value type. You can mostly use int and Integer interchangeably and everything just works, with Java automatically converting back and forth between the two as needed. (An Integer is just a class that contains inside it an int.)</li>

 <li>Once you have declared your hashmap with the key and value types as String and Integer, then when you use the put and get methods, they act just like they should, e.g., get takes a String as input and returns an Integer as output.</li>

 <li>You might find a variety of the methods in the <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/String.html">String</a> class useful. If you have not already, look them up in the Java documentation. For example, you can use the charAt method to retrieve a particular character in a string; you can use substring(int j) to retrieve the suffix of a string (from <em>j </em>onwards), and you can use substring(int i, int j) to retrieve the substring between character <em>i </em>(inclusive) and <em>j </em>(exclusive). Do look up the details.</li>

</ul>

<h1>Text Generator</h1>

We have provided you with a text generator class that you can use with your Markov Model class to generate text. The text generator class takes three input parameters, i.e., the main method has argument (k, n, filename):

<ul>

 <li><em>k</em>, the order of the Markov model;</li>

 <li><em>n</em>, the number of characters to generate;</li>

 <li>the filename of the text to use as a model. (This file should be in the project root directory, above the src folder.)</li>

</ul>

To run, you can set the input parameters in IntelliJ under Run → Edit Configurations, where you can create a new configuration (of type <em>Application</em>). There you can set the program arguments. For example: ”3 15 PS6Test.in” as the program arguments. Alternatively, you can also compile MarkovModel.java and TextGenerator.java and run TextGenerator on your terminal.

The text generator reads in the file as a long text String, creates the MarkovModel, and calls initializeString on your Markov Model class using the text.

It then generates a new text, using the first <em>k </em>characters of the original input text as a seed (and as the first <em>k </em>characters of the output text). In more detail, it begins with a String kgram equal to the first <em>k </em>letters from your text file. It generates the next character by calling nextCharacter on your Markov Model, using the initial kgram as your input string. Then it updates the kgram, adding the new character to the end. It continues in this way, at every step using the most recent <em>k </em>characters to query the Markov Model for the next character.

If it ever reaches a point where there is <em>no possible </em>next character, then it stops (outputting a string shorter than desired).

We will test the functionality of your nextCharacter method in Problem 6.b. by making use of this TextGenerator class that we have provided for you.

<h2>Optional Experiments</h2>

You may want to experiment with different values of <em>k </em>to determine which values yield reasonable sounding texts. If <em>k </em>is too large, then the text output will be identical to the input text. On the other hand, if <em>k </em>is too small, then the output text is quite garbled and ungrammatical English. There is a small range of <em>k </em>for which the output text both sounds like real English, but also sounds like a new and unique text.

<strong>Problem 6.c.          </strong>(Bonus) Word-based Markov Model:

You might also experiment with using words instead of characters. For example, instead of looking at the probability that character ‘c’ comes after the string ‘cra’, you could look at the probability that the word <em>cat </em>comes after the word <em>yellow </em>(i.e., order 1, where order is defined based on the number of words), or the probability that <em>cat </em>comes after the phrase <em>the vicious yellow </em>(i.e., order 3). If you develop your Markov Model based on words, you might get a more interesting text, as long as you begin with a sufficiently long text.

In order to use words instead of characters, you will have to design your MarkovModel class more carefully. You are now not restricted to the same scheme as the Random character generation, and you may wish to explore other schemes for random word generation.

Submit your <strong>well-commented </strong>MarkovModel class to Coursemology. You are also to upload your (potentially) modified TextGenerator class, OR any other class which makes use of your modified MarkovModel class to generate text. This is to allow your tutors to test your implementation for this problem.

Note: This bonus problem is an optional extension to the basic character-based Markov Model. You <strong>must still submit your basic version</strong>.

<h2>Creative Competition</h2>

Submit the best, most interesting text that your program produces. Post a screenshot of your best, most creative work to the forum (look out for the special forum thread!). You could aim for several goals: (i) plausibility (i.e., does it read like a real text), (ii) novelty, and (iii) humor. All who post a valid submission in the forum will attain the “Born to Write” achievement.

In order to generate an interesting text, you will need to find a good (long) source text. In the past, people have used legal documents (e.g., the text of a specific law), Shakespeare, news reports, and/or arbitrary texts from Project Gutenberg (gutenberg.org). You may splice together 2-3 input texts. You may also experiment with modifying the initial kgram. Instead of using the first <em>k </em>letters of the text as an initial kgram, you may choose an alternate choice of <em>k </em>characters. (Specify whether your entry was generated using characters, as in the assignment, or using words, as in the extension, or using something else.) On a closing note:

<em>“Half the fire a funny little man began to stay at heavens. ‘How beautiful this kingdom’ said their new emperor.”</em>