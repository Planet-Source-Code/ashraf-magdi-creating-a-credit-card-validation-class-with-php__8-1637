<div align="center">

## Creating a Credit Card Validation Class With PHP


</div>

### Description

Although online payment options such as PayPal have become extremely popular in the last couple of years, the majority of online stores still use some sort of merchant system to accept credit card payments from their Websites. Before you actually encrypt your customer's credit card numbers to a database or forward them to a merchant server, it's a good idea to implement your own credit card validation routine.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Ashraf Magdi](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/ashraf-magdi.md)
**Level**          |Intermediate
**User Rating**    |4.5 (18 globes from 4 users)
**Compatibility**  |PHP 4\.0
**Category**       |[Validation/ Processing](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/validation-processing__8-16.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/ashraf-magdi-creating-a-credit-card-validation-class-with-php__8-1637/archive/master.zip)





### Source Code

<table border="0" cellpadding="0" cellspacing="0" style="border-collapse: collapse" bordercolor="#111111" width="100%">
 <tr>
  <td width="100%"><STRONG>Although online payment options such as PayPal have become extremely
popular in the last couple of years, the majority of online stores still use
some sort of merchant system to accept credit card payments from their Websites.
Before you actually encrypt your customer's credit card numbers to a database or
forward them to a merchant server, it's a good idea to implement your own credit
card validation routine.</STRONG><P>In this article we're going to work through the development of a
class=glossary
title="PHP, or Hypertext Preprocessor, is an open source, server-side programming language."PHP</A> class that stores the details of a
credit card and validates its number using the Mod 10 algorithm. To implement
the class we'll create in this article, you should have access to an
class=glossary
title="Apache is one of the world's most widely-used Web servers.Apache</A> web server running PHP 4.1.0 or
later.</P>
<H5>Credit Card Validation</H5>
<P>What do we actually mean when we say "validate a credit card number"? Quite
simply it means that we run a credit card number through a special algorithm
known as the Mod 10 algorithm.</P>
<P>This algorithm processes some simple numerical data validation routines
against the number, and the result of this algorithm can be used to determine
whether or not a credit card number is valid. There are several different types
of credit cards that one can use to make a purchase, however they can all be
validated using the Mod 10 algorithm.</P>
<P>As well as passing the Mod 10 algorithm, a credit card number must also pass
several different formatting rules. A list of these rules for each of the six
most popular credit cards is shown below:</P>
<P>
<UL>
<LI><STRONG>mastercard:</STRONG> Must have a prefix of 51 to 55, and must be 16
digits in length.
<LI><STRONG>Visa:</STRONG> Must have a prefix of 4, and must be either 13 or 16
digits in length.
<DIV id=adz style="DISPLAY: block"></DIV>
<LI><STRONG>American Express:</STRONG> Must have a prefix of 34 or 37, and must
be 15 digits in length.
<LI><STRONG>Diners Club: </STRONG>Must have a prefix of 300 to 305, 36, or 38,
and must be 14 digits in length.
<LI><STRONG>Discover:</STRONG> Must have a prefix of 6011, and must be 16 digits
in length.
<LI><STRONG>JCB:</STRONG> Must have a prefix of 3, 1800, or 2131, and must be
either 15 or 16 digits in length. </LI></UL>
<P></P>
<P>As mentioned earlier, in this article we will create a PHP class that will
hold the details of a credit card number and expose a function that indicates
whether or not the number of that credit card is valid (i.e. whether it passed
the Mod 10 algorithm or not). Before we create that class however, let's look at
how the Mod 10 algorithm works.</P>
<H5>The Mod 10 Algorithm</H5>
<P>There are three steps that the Mod 10 algorithm takes to determine whether or
not a credit card number is valid. We will use the valid credit card number
378282246310005 to demonstrate these steps:</P>
<P><EM><STRONG>Step One</STRONG></EM></P>
<P>The number is reversed and the value of every second digit is doubled,
starting with the digit in second place:</P>
<P>378282246310005</P>
<P>becomes...</P>
<P>500013642282873</P>
<P>and the value of every second digit is doubled:</P>
<P>5 0 0 0 1 3 6 4 2 2 8 2 8 7 3</P>
<P>x2 x2 x2 x2 x2 x2 x2</P>
<P>-------------------------------------------</P>
<P>0 0 6 8 4 4 14</P>
  <p><em><strong>Step Two</strong></em></p>
  <p>The values of the numbers that resulted from multiplying every second
  digit by two are added together (i.e. in our example above, multiplying the
  7 by two resulted in 14, which is 1 + 4 = 5). The result of these additions
  is added to the value of every digit that was not multiplied (i.e. the first
  digit, the third, the fifth, etc):</p>
  <p>5 + (0) + 0 + (0) + 1 + (6) + 6 + (8) + 2 + (4) + 8 + (4) + 8 + (1 + 4) +
  3</p>
  <p>= 60</p>
  <p><em><strong>Step Three</strong></em></p>
  <p>When a modulus operation is applied to the result of step two, the
  remainder must equal 0 in order for the number to pass the Mod 10 algorithm.
  The modulus operator simply returns the remainder of a division, for
  example:</p>
  <p>10 MOD 5 = 0 (5 goes into 10 two times and has a remainder of 0)</p>
  <p>20 MOD 6 = 2 (6 goes into 20 three times and has a remainder of 2)</p>
  <p>43 MOD 4 = 3 (4 goes into 43 ten times and has a remainder of 3)</p>
  <p>So for our test credit card number 378282246310005, we apply a modulus of
  10 to the result from step two, like this:</p>
  <p>60 MOD 10 = 0</p>
  <p>The modulus operation returns 0, indicating that the credit card number
  is valid.</p>
  <div id="adz">
&nbsp;</div>
  <p>Now that we understand the Mod 10 algorithm, it's really quite easy to
  create our own version to validate credit card numbers with
   glossary PHP, or Hypertext Preprocessor, is an open source, server-side programming language.PHP</a>. Let's create our credit card class now.</p>
  <h5>Creating the CCreditCard Class</h5>
  <p>Let's now create a PHP class that we can use to store and validate the
  details of a credit card. Our class will be able to hold the cardholder's
  name, the card type (mastercard, visa, etc), the card number, and the expiry
  month and date.</p>
  <p>Create a new PHP file called class.creditcard.php. As we walk through the
  following steps, copy-paste each piece of code shown to the file and save
  it.</p>
  <p>We start of by defining several card type constants. These values will be
  used to represent the type of card that our class will be validating:</p>
  <p><code>&lt;?php <br>
  <br>
  define(&quot;CARD_TYPE_MC&quot;, 0); <br>
  define(&quot;CARD_TYPE_VS&quot;, 1); <br>
  define(&quot;CARD_TYPE_AX&quot;, 2); <br>
  define(&quot;CARD_TYPE_DC&quot;, 3); <br>
  define(&quot;CARD_TYPE_DS&quot;, 4); <br>
  define(&quot;CARD_TYPE_JC&quot;, 5);</code></p>
  <p>Next, we have our class declaration. Our class is called <code>
  CCreditCard</code>. Note that there is an extra 'C' at the front of the
  class name intentionally: it's a common programming practice to prefix the
  name of a class with 'C' to in fact indicate that it is a class.</p>
  <p>We also define five member variables, which will be used internally to
  hold the credit card's name, type, number, expiry month and year:</p>
  <p><code>class CCreditCard <br>
  { <br>
  // Class Members <br>
  var $__ccName = ''; <br>
  var $__ccType = ''; <br>
  var $__ccNum = ''; <br>
  var $__ccExpM = 0; <br>
  var $__ccExpY = 0;</code></p>
  <p>Next we have our class' custom constructor. A constructor is a function
  that has the same names as the class in which it exists. It returns no
  value. It is special in the sense that it is automatically executed whenever
  we create a new instance of that class.</p>
  <p>Whenever we want to create a new instance of our <code>CCreditCard</code>
  class, we must explicitly pass in five arguments to its constructor: the
  cardholder's name, card type, number, and expiry date. Because we have
  created our own custom constructor ( class="glossary" title="PHP, or Hypertext Preprocessor, is an open source, server-side programming language.PHP</a>
  implements a default constructor that accepts no arguments if we don't
  explicitly create one), we must pass in values for each of these five
  arguments every time we instantiate the class. If we omit them, PHP will
  raise an error.</p>
  <p><code>// Constructor <br>
  function CCreditCard($name, $type, $num, $expm, $expy) <br>
  {</code></p>
  <p>If the value of the <code>$name</code> variable passed into the
  constructor is empty, then we use the <code>die() </code>function to
  terminate the instantiation of our class and output an error message telling
  the user that they must pass a name to the constructor:</p>
  <p><code>// Set member variables <br>
  if(!empty($name)) <br>
  { <br>
  $this-&gt;__ccName = $name; <br>
  } <br>
  else <br>
  { <br>
  die('Must pass name to constructor'); <br>
  }</code></p>
  <p>Our <code>CCreditCard</code> class is flexible: it accepts several
  different ways to specify the type of card that is being stored. For
  example, if we want to add the details of a mastercard to a new instance of
  our <code>CCreditCard</code> class, then we could pass in the following
  values for the <code>$type </code>variable of the constructor: &quot;mc&quot;,
  &quot;mastercard&quot;, &quot;m&quot;, or &quot;1&quot;.</p>
  <p>We make sure that a valid card type has been passed in, and set the value
  of our classes <code>$__ccType</code> variable to one of the constant card
  type values that we defined earlier:</p>
  <p><code>// Make sure card type is valid <br>
  switch(strtolower($type)) <br>
  { <br>
  &nbsp; case 'mc': <br>
  &nbsp; case 'mastercard': <br>
  &nbsp; case 'm': <br>
  &nbsp; case '1': <br>
  &nbsp; &nbsp; $this-&gt;__ccType = CARD_TYPE_MC; <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; case 'vs': <br>
  &nbsp; case 'visa': <br>
  &nbsp; case 'v': <br>
  &nbsp; case '2': <br>
  &nbsp; &nbsp; $this-&gt;__ccType = CARD_TYPE_VS; <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; case 'ax': <br>
  &nbsp; case 'american express': <br>
  &nbsp; case 'a': <br>
  &nbsp; case '3': <br>
  &nbsp; &nbsp; $this-&gt;__ccType = CARD_TYPE_AX; <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; case 'dc': <br>
  &nbsp; case 'diners club': <br>
  &nbsp; case '4': <br>
  &nbsp; &nbsp; $this-&gt;__ccType = CARD_TYPE_DC; <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; case 'ds': <br>
  &nbsp; case 'discover': <br>
  &nbsp; case '5': <br>
  &nbsp; &nbsp; $this-&gt;__ccType = CARD_TYPE_DS; <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; case 'jc': <br>
  &nbsp; case 'jcb': <br>
  &nbsp; case '6': <br>
  &nbsp; &nbsp; $this-&gt;__ccType = CARD_TYPE_JC; <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; default: <br>
  &nbsp; &nbsp; die('Invalid type ' . $type . ' passed to constructor'); <br>
  }</code></p>
  <p>If an invalid card type is passed in, then the default branch of our
  switch statement will be called, resulting in our script terminating with
  the <code>die()</code> function.</p>
  <p>We can take advantage of
   class="glossary" title="PHP, or Hypertext Preprocessor, is an open source, server-side programming language.
  PHP</a>'s built-in support for regular expressions by using the <code>
  ereg_replace</code> function to strip out all non-numeric characters from
  the credit card number:</p>
  <p><code>// Don't check the number yet, <br>
  // just kill all non numerics <br>
  if(!empty($num)) <br>
  { <br>
  &nbsp; $cardNumber = ereg_replace(&quot;[^0-9]&quot;, &quot;&quot;, $num); <br>
  <br>
  &nbsp; // Make sure the card number isnt empty <br>
  &nbsp; if(!empty($cardNumber)) <br>
  &nbsp; { <br>
  &nbsp; &nbsp; $this-&gt;__ccNum = $cardNumber; <br>
  &nbsp; } <br>
  &nbsp; else <br>
  &nbsp; { <br>
  &nbsp; &nbsp; die('Must pass number to constructor'); <br>
  &nbsp; } <br>
  } <br>
  else <br>
  { <br>
  &nbsp; die('Must pass number to constructor'); <br>
  }</code></p>
  <p>We finish off our <code>CCreditCard </code>constructor by making sure
  that both the expiry month and year are valid, numerical values:</p>
  <p><code>if(!is_numeric($expm) || $expm &lt; 1 || $expm &gt; 12) <br>
  { <br>
  &nbsp; die('Invalid expiry month of ' . $expm . ' passed to constructor'); <br>
  } <br>
  else <br>
  { <br>
  &nbsp; $this-&gt;__ccExpM = $expm; <br>
  } <br>
  <br>
  // Get the current year <br>
  $currentYear = date('Y'); <br>
  settype($currentYear, 'integer'); <br>
  <br>
  if(!is_numeric($expy) || $expy &lt; $currentYear || $expy <br>
  &gt; $currentYear + 10) <br>
  { <br>
  &nbsp; die('Invalid expiry year of ' . $expy . ' passed to constructor'); <br>
  } <br>
  else <br>
  { <br>
  &nbsp; $this-&gt;__ccExpY = $expy; <br>
  } <br>
  }</code></p>
  <p>In our <code>CCreditCard</code> class, the only way to set the values of
  the credit card's details is through the constructor. To retrieve the values
  of our class-specific variables (<code>$__ccName</code>, <code>$__ccType</code>,
  etc), we create several functions, like this: </p>
  <p><code>function Name() <br>
  { <br>
  &nbsp; return $this-&gt;__ccName; <br>
  } <br>
  <br>
  function Type() <br>
  { <br>
  &nbsp; switch($this-&gt;__ccType) <br>
  &nbsp; &nbsp; { <br>
  &nbsp; &nbsp; case CARD_TYPE_MC: <br>
  &nbsp; &nbsp; &nbsp; return 'mastercard [1]'; <br>
  &nbsp; &nbsp; &nbsp; break; <br>
  &nbsp; &nbsp; case CARD_TYPE_VS: <br>
  &nbsp; &nbsp; &nbsp; return 'Visa [2]'; <br>
  &nbsp; &nbsp; &nbsp; break; <br>
  &nbsp; &nbsp; case CARD_TYPE_AX: <br>
  &nbsp; &nbsp; &nbsp; return 'Amex [3]'; <br>
  &nbsp; &nbsp; &nbsp; break; <br>
  &nbsp; &nbsp; case CARD_TYPE_DC: <br>
  &nbsp; &nbsp; &nbsp; return 'Diners Club [4]'; <br>
  &nbsp; &nbsp; &nbsp; break; <br>
  &nbsp; &nbsp; case CARD_TYPE_DS: <br>
  &nbsp; &nbsp; &nbsp; return 'Discover [5]'; <br>
  &nbsp; &nbsp; &nbsp; break; <br>
  &nbsp; &nbsp; case CARD_TYPE_JC: <br>
  &nbsp; &nbsp; &nbsp; return 'JCB [6]'; <br>
  &nbsp; &nbsp; &nbsp; break; <br>
  &nbsp; &nbsp; default: <br>
  &nbsp; &nbsp; &nbsp; return 'Unknown [-1]'; <br>
  &nbsp; } <br>
  } <br>
  <br>
  function Number() <br>
  { <br>
  &nbsp; return $this-&gt;__ccNum; <br>
  } <br>
  <br>
  function ExpiryMonth() <br>
  { <br>
  &nbsp; return $this-&gt;__ccExpM; <br>
  } <br>
  <br>
  function ExpiryYear() <br>
  { <br>
  &nbsp; return $this-&gt;__ccExpY; <br>
  }</code></p>
  <p>These functions allow us to retrieve the values of the variables
  contained within our class. For example, if I created an instance of our
  <code>CCreditCard</code> class called <code>$cc1</code>, then I could
  retrieve its expiration month using <code>$cc1-&gt;ExpiryMonth()</code>.</p>
  <p>A common function when working with credit cards is displaying the
  details that you've captured from that user back to them as a confirmation.
  For example, if the user entered a credit card number of 4111111111111111,
  then you might want to only show part of the number to them, such as
  4111111111111xxxx. Our <code>CCreditCard </code>class contains a function
  called <code>SafeNumber</code>, which accepts two arguments. The first is
  the character to mask the digits with, and the second is the number of
  digits to mask (from the right):</p>
  <p><code>function SafeNumber($char = 'x', $numToHide = 4) <br>
  { <br>
  &nbsp; // Return only part of the number <br>
  &nbsp; if($numToHide &lt; 4) <br>
  &nbsp; { <br>
  &nbsp; &nbsp; $numToHide = 4; <br>
  &nbsp; } <br>
  <br>
  &nbsp; if($numToHide &gt; 10) <br>
  &nbsp; { <br>
  &nbsp; &nbsp; $numToHide = 10; <br>
  &nbsp; } <br>
  <br>
  &nbsp; $cardNumber = $this-&gt;__ccNum; <br>
  &nbsp; $cardNumber = substr($cardNumber, 0, strlen($cardNumber) - $numToHide);
  <br>
  <br>
  &nbsp; for($i = 0; $i &lt; $numToHide; $i++) <br>
  &nbsp; { <br>
  &nbsp; &nbsp; $cardNumber .= $char; <br>
  &nbsp; } <br>
  <br>
  &nbsp; return $cardNumber; <br>
  }</code></p>
  <p>If we had an instance of our <code>CCreditCard</code> class called <code>
  $cc1 </code>and the credit card number stored in this class was
  4242424242424242, then we could mask the last 6 digits like this: <code>echo
  $cc1-&gt;SafeNumber('x', 6)</code>.</p>
  <p>The last function contained in our <code>CCreditCard</code> class is
  called <code>IsValid</code>, and implements the Mod 10 algorithm against the
  credit card number of our class, returning true/false.</p>
  <p>It starts of by setting two variables (<code>$validFormat</code> and
  <code>$passCheck</code>) to false:</p>
  <p><code>function IsValid() <br>
  { <br>
  &nbsp; // Not valid by default <br>
  &nbsp; $validFormat = false; <br>
  &nbsp; $passCheck = false;</code></p>
  <p>Next we make sure that the credit card number is formatted correctly. We
  use
   class="glossary" title="PHP, or Hypertext Preprocessor, is an open source, server-side programming language.
  PHP</a>'s <code>ereg</code> function to do this. The regular expression that
  must be matched is different for each card:</p>
  <p><code>// Is the number in the correct format? <br>
  switch($this-&gt;__ccType) <br>
  { <br>
  &nbsp; case CARD_TYPE_MC: <br>
  &nbsp; &nbsp; $validFormat = ereg(&quot;^5[1-5][0-9]{14}$&quot;, $this-&gt;__ccNum); <br>
  &nbsp; &nbsp; break; <br>
  case CARD_TYPE_VS: <br>
  &nbsp; &nbsp; $validFormat = ereg(&quot;^4[0-9]{12}([0-9]{3})?$&quot;, $this-&gt;__ccNum); <br>
  &nbsp; &nbsp; break; <br>
  case CARD_TYPE_AX: <br>
  &nbsp; &nbsp; $validFormat = ereg(&quot;^3[47][0-9]{13}$&quot;, $this-&gt;__ccNum); <br>
  &nbsp; &nbsp; break; <br>
  case CARD_TYPE_DC: <br>
  &nbsp; &nbsp; $validFormat = ereg(&quot;^3(0[0-5]|[68][0-9])[0-9]{11}$&quot;, $this-&gt;__ccNum);
  <br>
  &nbsp; &nbsp; break; <br>
  case CARD_TYPE_DS: <br>
  &nbsp; &nbsp; $validFormat = ereg(&quot;^6011[0-9]{12}$&quot;, $this-&gt;__ccNum); <br>
  &nbsp; &nbsp; break; <br>
  case CARD_TYPE_JC: <br>
  &nbsp; &nbsp; $validFormat = ereg(&quot;^(3[0-9]{4}|2131|1800)[0-9]{11}$&quot;, $this-&gt;__ccNum);
  <br>
  &nbsp; &nbsp; break; <br>
  &nbsp; default: <br>
  &nbsp; // Should never be executed <br>
  &nbsp; $validFormat = false; <br>
  }</code></p>
  <p>At this point, <code>$validFormat</code> will be true (<code>ereg</code>
  returns true/false) if the credit card number is in the correct format, and
  false if it's not.</p>
  <p>We now implement a
   class="glossary" title="PHP, or Hypertext Preprocessor, is an open source, server-side programming language.'
  PHP</a> version of the Mod 10 algorithm, using exactly the same steps that
  we described earlier:</p>
  <p><code>// Is the number valid? <br>
  $cardNumber = strrev($this-&gt;__ccNum); <br>
  $numSum = 0; <br>
  <br>
  for($i = 0; $i &lt; strlen($cardNumber); $i++) <br>
  { <br>
  &nbsp; $currentNum = substr($cardNumber, $i, 1); <br>
  <br>
  // Double every second digit <br>
  if($i % 2 == 1) <br>
  { <br>
  &nbsp; $currentNum *= 2; <br>
  } <br>
  <br>
  // Add digits of 2-digit numbers together <br>
  if($currentNum &gt; 9) <br>
  { <br>
  &nbsp; $firstNum = $currentNum % 10; <br>
  &nbsp; $secondNum = ($currentNum - $firstNum) / 10; <br>
  &nbsp; $currentNum = $firstNum + $secondNum; <br>
  } <br>
  <br>
  $numSum += $currentNum; <br>
  }</code></p>
  <p>The <code>$numSum</code> variable will contain the sum of all of the
  variables from step two of the Mod 10 algorithm, which we described earlier.
  PHP's symbol for the modulus operator is '<code>%</code>', so we assign
  true/false to the <code>$passCheck </code>variable, depending on whether or
  not <code>$numSum</code> has a modulus of zero:</p>
  <p><code>// If the total has no remainder it's OK <br>
  $passCheck = ($numSum % 10 == 0);</code></p>
  <p>If both <code>$validFormat </code>and <code>$passCheck</code> are true,
  then we return true, to indicate that the card number is valid. If not, we
  return false, to indicate that either the card number was in an incorrect
  format, or if failed the Mod 10 check:</p>
  <p><code>&nbsp; if($validFormat &amp;&amp; $passCheck) return true; <br>
  &nbsp; else return false; <br>
  &nbsp;} <br>
  } <br>
  ?&gt;</code></p>
  <div id="adz">
&nbsp;</div>
  <p>And that's all there is to our <code>CCreditCard</code> class! Let's now
  look at a simple validation example using HTML forms, PHP, and an instance
  of our <code>CCreditCard</code> class.</p>
  <h5>Using our CCreditCard Class</h5>
  <p>Create a new file called testcc.php and save it in the same directory as
  the class.creditcard.php file. Enter the following code into testcc.php:</p>
  <p><code>&lt;?php include('class.creditcard.php'); ?&gt; <br>
  &lt;?php <br>
  if(!isset($submit)) <br>
  { <br>
  ?&gt; <br>
  <br>
  &nbsp; &lt;h2&gt;Validate Credit Card&lt;/h2&gt; <br>
  &nbsp; &lt;form name=&quot;frmCC&quot; action=&quot;testcc.php&quot; method=&quot;post&quot;&gt; <br>
  <br>
  &nbsp; Cardholders name: &lt;input type=&quot;text&quot; name=&quot;ccName&quot;&gt;&lt;br&gt; <br>
  &nbsp; Card number: &lt;input type=&quot;text&quot; name=&quot;ccNum&quot;&gt;&lt;br&gt; <br>
  &nbsp; Card type: &lt;select name=&quot;ccType&quot;&gt; <br>
  &nbsp; &lt;option value=&quot;1&quot;&gt;mastercard&lt;/option&gt; <br>
  &nbsp; &lt;option value=&quot;2&quot;&gt;Visa&lt;/option&gt; <br>
  &nbsp; &lt;option value=&quot;3&quot;&gt;Amex&lt;/option&gt; <br>
  &nbsp; &lt;option value=&quot;4&quot;&gt;Diners&lt;/option&gt; <br>
  &nbsp; &lt;option value=&quot;5&quot;&gt;Discover&lt;/option&gt; <br>
  &nbsp; &lt;option value=&quot;6&quot;&gt;JCB&lt;/option&gt; <br>
  &nbsp; &lt;/select&gt;&lt;br&gt; <br>
  <br>
  &nbsp; Expiry Date: &lt;select name=&quot;ccExpM&quot;&gt; &nbsp;<br>
  <br>
  &nbsp; &lt;?php <br>
  <br>
  &nbsp; &nbsp; for($i = 1; $i &lt; 13; $i++) <br>
  &nbsp; &nbsp; { echo '&lt;option&gt;' . $i . '&lt;/option&gt;'; } <br>
  <br>
  &nbsp; ?&gt; &nbsp;<br>
  <br>
  &nbsp; &lt;/select&gt; <br>
  <br>
  &nbsp; &lt;select name=&quot;ccExpY&quot;&gt; <br>
  <br>
  &nbsp; &lt;?php <br>
  <br>
  &nbsp; &nbsp; for($i = 2002; $i &lt; 2013; $i++) <br>
  &nbsp; &nbsp; { echo '&lt;option&gt;' . $i . '&lt;/option&gt;'; } <br>
  <br>
  &nbsp; ?&gt; &nbsp;<br>
  <br>
  &nbsp; &lt;/select&gt;&lt;br&gt;&lt;br&gt; <br>
  <br>
  &nbsp; &lt;input type=&quot;submit&quot; name=&quot;submit&quot; value=&quot;Validate&quot;&gt; <br>
  &nbsp; &lt;/form&gt; <br>
  <br>
  &nbsp; &lt;? <br>
  <br>
  &nbsp; } <br>
  &nbsp; else <br>
  &nbsp; { <br>
  &nbsp; // Check if the card is valid <br>
  &nbsp; $cc = new CCreditCard($ccName, $ccType, $ccNum, $ccExpM, $ccExpY); <br>
  <br>
  &nbsp; ?&gt; <br>
  <br>
  &nbsp; &lt;h2&gt;Validation Results&lt;/h2&gt; <br>
  &nbsp; &lt;b&gt;Name: &lt;/b&gt;&lt;?=$cc-&gt;Name(); ?&gt;&lt;br&gt; <br>
  &nbsp; &lt;b&gt;Number: &lt;/b&gt;&lt;?=$cc-&gt;SafeNumber('x', 6); ?&gt;&lt;br&gt; <br>
  &nbsp; &lt;b&gt;Type: &lt;/b&gt;&lt;?=$cc-&gt;Type(); ?&gt;&lt;br&gt; <br>
  &nbsp; &lt;b&gt;Expires: &lt;/b&gt;&lt;?=$cc-&gt;ExpiryMonth() . '/' . <br>
  &nbsp; $cc-&gt;ExpiryYear(); ?&gt;&lt;br&gt;&lt;br&gt; <br>
  <br>
  &nbsp; &lt;?php <br>
  &nbsp;<br>
  &nbsp; &nbsp; echo '&lt;font color=&quot;blue&quot; size=&quot;2&quot;&gt;&lt;b&gt;'; &nbsp;<br>
  <br>
  &nbsp; &nbsp; if($cc-&gt;IsValid()) <br>
  &nbsp; &nbsp; echo 'VALID CARD'; <br>
  &nbsp; &nbsp; else <br>
  &nbsp; &nbsp; echo 'INVALID CARD'; <br>
  <br>
  &nbsp; &nbsp; echo '&lt;/b&gt;&lt;/font&gt;'; <br>
  &nbsp; } <br>
  ?&gt;</code></p>
  <p>Run the script in your browser and see what happens...</td>
 </tr>
</table>

