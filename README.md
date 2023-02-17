# phptest
testing 
In this we are going to see how to insert dummy data in MySQL database using PHP and PDO. Most of time when we develop an application we need sample data according to our requirement to develop, test or do performance run on the application.


Requirements:
1.	Faker PHP Plugin
2.	PDO Driver
Step 1: Install PHP Faker Plugin Using Composer
Create new project called Mysql_DB_Anonymizer in your htdocs/workspace, and install following PHP plugin using composer.
Note: If you have not installed Composer in your system already, then install Composer using following tutorial
1.	Install Composer In XAMPP/WAMP Windows
2.	Composer Installation – Linux/Unix/OSX
Once you installed composer successfully in your system, then go to your project folder and run the following command
composer require fzaninotto/faker
Step 2: Create MySQL Database and Tables:
Now create your database and tables in your MySQL server. I have sample test database and users table in it. Create users in your database using following sql query.

<?php return array (
    'DB_HOST' => '20.23.102.200',
    'DB_NAME' => 'mysql_test',
    'DB_USER' => 'root or user-name',
    'DB_PASSWORD' => 'password',
    'NB_MAX_MYSQL_CLIENT' => 50,
    'NB_MAX_PROMISE_IN_LOOP' => 50,
    'DEFAULT_GENERATOR_LOCALE' => 'en_US'
);
SQL
Copy
Step 3: Import Faker Object and Connect Database to generate Dummy Data:
Finally create index.php file, then include autoload.php file from vendor directory. That will taking care including Faker classes automatically when we instantiate the Faker class.
Here is my sample script which will make the connection with MySQL database and insert dummy data in it. Specify number of records in $count variable.
<?php

require './vendor/autoload.php';
use Globalis\MysqlDataAnonymizer\Anonymizer;

$anonymizer = new Anonymizer();

// Describe `users` table.
$anonymizer->table('admin_user', function ($table) {
    
    // Specify a primary key of the table. An array should be passed in for composite key.
    $table->primary('id');

    // Add a global filter to the queries.
    // Only string is accepted so you need to write down the complete WHERE statement here.
    //$table->globalWhere('email4 != email5 AND id != 10');

    // Replace with static data.
    // $table->column('email1')->replaceWith('john@example.com');
    $table->column('email')->replaceWith(function ($generator) {
        return $generator->email;
    });

    // Use #row# template to get "email_0@example.com", "email_1@example.com", "email_2@example.com"
    $table->column('password')->replaceWith(function ($generator) {
		//echo "<pre>";print_R($generator->email);die;
        return $generator->password;
    });

    // To replace with dynamic data a $generator is needed.
    // By default, a fzaninotto/Faker generator will be used. 
    // Any generator object can be set like that - `$anonymizer->setGenerator($generator);`
    $table->column('name')->replaceWith(function ($generator) {
		//echo "<pre>";print_R($generator->email);die;
        return $generator->name;
    });
	//die("ddd");
    // Use `where` to leave some data untouched for a specific column.
    // If you don't list a column here, it will be left untouched too.
     $table->column('can_edit')->replaceWith(function ($generator) {
	     //echo "<pre>";print_R($generator->email);die;
         return $generator->randomDigit;
     });
	
    // Use the values of current row to update a field
    // This is a position sensitive operation, so the value of field 'email4' here is the updated value.
    // So if you put this line before the previous one, the value of 'email4' here would be the valeu of 'email4' before update.
     $table->column('is_active')->replaceWith(function ($generator) {
	// 	//echo "<pre>";print_R($generator->email);die;
         return $generator->randomDigit;
     });

    // Here we assume that there is a foreign key in the table 'class' on the column 'user_id'.
    // To make sure 'user_id' get updated when we update 'id', use function 'synchronizeColumn'.

    $table->column('created_at')->replaceWith(function ($generator) {
		//echo "<pre>";print_R($generator->email);die;
        return $generator->date;
    });

    $table->column('updated_at')->replaceWith(function ($generator) {
		//echo "<pre>";print_R($generator->email);die;
        return $generator->date;
    });
    //$table->column('id')->replaceWith(function ($generator) {
    //    return $generator->unique()->uuid;
    //})->synchronizeColumn(['user_id', 'class']);
});

echo "vinod kumar ";
echo $anonymizer->run();

echo 'Anonymization has been completed!';




Formatters
Each of the generator properties (like name, address, and lorem) are called "formatters". A faker generator has many of them, packaged in "providers". Here is a list of the bundled formatters in the default locale.
Faker\Provider\Base
randomDigit             // 7
randomDigitNot(5)       // 0, 1, 2, 3, 4, 6, 7, 8, or 9
randomDigitNotNull      // 5
randomNumber($nbDigits = NULL, $strict = false) // 79907610
randomFloat($nbMaxDecimals = NULL, $min = 0, $max = NULL) // 48.8932
numberBetween($min = 1000, $max = 9000) // 8567
randomLetter            // 'b'
// returns randomly ordered subsequence of a provided array
randomElements($array = array ('a','b','c'), $count = 1) // array('c')
randomElement($array = array ('a','b','c')) // 'b'
shuffle('hello, world') // 'rlo,h eoldlw'
shuffle(array(1, 2, 3)) // array(2, 1, 3)
numerify('Hello ###') // 'Hello 609'
lexify('Hello ???') // 'Hello wgt'
bothify('Hello ##??') // 'Hello 42jz'
asciify('Hello ***') // 'Hello R6+'
regexify('[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}'); // sm0@y8k96a.ej
Faker\Provider\Lorem
word                                             // 'aut'
words($nb = 3, $asText = false)                  // array('porro', 'sed', 'magni')
sentence($nbWords = 6, $variableNbWords = true)  // 'Sit vitae voluptas sint non voluptates.'
sentences($nb = 3, $asText = false)              // array('Optio quos qui illo error.', 'Laborum vero a officia id corporis.', 'Saepe provident esse hic eligendi.')
paragraph($nbSentences = 3, $variableNbSentences = true) // 'Ut ab voluptas sed a nam. Sint autem inventore aut officia aut aut blanditiis. Ducimus eos odit amet et est ut eum.'
paragraphs($nb = 3, $asText = false)             // array('Quidem ut sunt et quidem est accusamus aut. Fuga est placeat rerum ut. Enim ex eveniet facere sunt.', 'Aut nam et eum architecto fugit repellendus illo. Qui ex esse veritatis.', 'Possimus omnis aut incidunt sunt. Asperiores incidunt iure sequi cum culpa rem. Rerum exercitationem est rem.')
text($maxNbChars = 200)                          // 'Fuga totam reiciendis qui architecto fugiat nemo. Consequatur recusandae qui cupiditate eos quod.'
Faker\Provider\en_US\Person
title($gender = null|'male'|'female')     // 'Ms.'
titleMale                                 // 'Mr.'
titleFemale                               // 'Ms.'
suffix                                    // 'Jr.'
name($gender = null|'male'|'female')      // 'Dr. Zane Stroman'
firstName($gender = null|'male'|'female') // 'Maynard'
firstNameMale                             // 'Maynard'
firstNameFemale                           // 'Rachel'
lastName                                  // 'Zulauf'
Faker\Provider\en_US\Address
cityPrefix                          // 'Lake'
secondaryAddress                    // 'Suite 961'
state                               // 'NewMexico'
stateAbbr                           // 'OH'
citySuffix                          // 'borough'
streetSuffix                        // 'Keys'
buildingNumber                      // '484'
city                                // 'West Judge'
streetName                          // 'Keegan Trail'
streetAddress                       // '439 Karley Loaf Suite 897'
postcode                            // '17916'
address                             // '8888 Cummings Vista Apt. 101, Susanbury, NY 95473'
country                             // 'Falkland Islands (Malvinas)'
latitude($min = -90, $max = 90)     // 77.147489
longitude($min = -180, $max = 180)  // 86.211205
Faker\Provider\en_US\PhoneNumber
phoneNumber             // '201-886-0269 x3767'
tollFreePhoneNumber     // '(888) 937-7238'
e164PhoneNumber     // '+27113456789'
Faker\Provider\en_US\Company
catchPhrase             // 'Monitored regional contingency'
bs                      // 'e-enable robust architectures'
company                 // 'Bogan-Treutel'
companySuffix           // 'and Sons'
jobTitle                // 'Cashier'
Faker\Provider\en_US\Text
realText($maxNbChars = 200, $indexSize = 2) // "And yet I wish you could manage it?) 'And what are they made of?' Alice asked in a shrill, passionate voice. 'Would YOU like cats if you were never even spoke to Time!' 'Perhaps not,' Alice replied."
Faker\Provider\DateTime
unixTime($max = 'now')                // 58781813
dateTime($max = 'now', $timezone = null) // DateTime('2008-04-25 08:37:17', 'UTC')
dateTimeAD($max = 'now', $timezone = null) // DateTime('1800-04-29 20:38:49', 'Europe/Paris')
iso8601($max = 'now')                 // '1978-12-09T10:10:29+0000'
date($format = 'Y-m-d', $max = 'now') // '1979-06-09'
time($format = 'H:i:s', $max = 'now') // '20:49:42'
dateTimeBetween($startDate = '-30 years', $endDate = 'now', $timezone = null) // DateTime('2003-03-15 02:00:49', 'Africa/Lagos')
dateTimeInInterval($startDate = '-30 years', $interval = '+ 5 days', $timezone = null) // DateTime('2003-03-15 02:00:49', 'Antartica/Vostok')
dateTimeThisCentury($max = 'now', $timezone = null)     // DateTime('1915-05-30 19:28:21', 'UTC')
dateTimeThisDecade($max = 'now', $timezone = null)      // DateTime('2007-05-29 22:30:48', 'Europe/Paris')
dateTimeThisYear($max = 'now', $timezone = null)        // DateTime('2011-02-27 20:52:14', 'Africa/Lagos')
dateTimeThisMonth($max = 'now', $timezone = null)       // DateTime('2011-10-23 13:46:23', 'Antarctica/Vostok')
amPm($max = 'now')                    // 'pm'
dayOfMonth($max = 'now')              // '04'
dayOfWeek($max = 'now')               // 'Friday'
month($max = 'now')                   // '06'
monthName($max = 'now')               // 'January'
year($max = 'now')                    // '1993'
century                               // 'VI'
timezone                              // 'Europe/Paris'
Methods accepting a $timezone argument default to date_default_timezone_get(). You can pass a custom timezone string to each method, or define a custom timezone for all time methods at once using $faker::setDefaultTimezone($timezone).
Faker\Provider\Internet
email                   // 'tkshlerin@collins.com'
safeEmail               // 'king.alford@example.org'
freeEmail               // 'bradley72@gmail.com'
companyEmail            // 'russel.durward@mcdermott.org'
freeEmailDomain         // 'yahoo.com'
safeEmailDomain         // 'example.org'
userName                // 'wade55'
password                // 'k&|X+a45*2['
domainName              // 'wolffdeckow.net'
domainWord              // 'feeney'
tld                     // 'biz'
url                     // 'http://www.skilesdonnelly.biz/aut-accusantium-ut-architecto-sit-et.html'
slug                    // 'aut-repellat-commodi-vel-itaque-nihil-id-saepe-nostrum'
ipv4                    // '109.133.32.252'
localIpv4               // '10.242.58.8'
ipv6                    // '8e65:933d:22ee:a232:f1c1:2741:1f10:117c'
macAddress              // '43:85:B7:08:10:CA'
Faker\Provider\UserAgent
userAgent              // 'Mozilla/5.0 (Windows CE) AppleWebKit/5350 (KHTML, like Gecko) Chrome/13.0.888.0 Safari/5350'
chrome                 // 'Mozilla/5.0 (Macintosh; PPC Mac OS X 10_6_5) AppleWebKit/5312 (KHTML, like Gecko) Chrome/14.0.894.0 Safari/5312'
firefox                // 'Mozilla/5.0 (X11; Linuxi686; rv:7.0) Gecko/20101231 Firefox/3.6'
safari                 // 'Mozilla/5.0 (Macintosh; U; PPC Mac OS X 10_7_1 rv:3.0; en-US) AppleWebKit/534.11.3 (KHTML, like Gecko) Version/4.0 Safari/534.11.3'
opera                  // 'Opera/8.25 (Windows NT 5.1; en-US) Presto/2.9.188 Version/10.00'
internetExplorer       // 'Mozilla/5.0 (compatible; MSIE 7.0; Windows 98; Win 9x 4.90; Trident/3.0)'
Faker\Provider\Payment
creditCardType          // 'MasterCard'
creditCardNumber        // '4485480221084675'
creditCardExpirationDate // 04/13
creditCardExpirationDateString // '04/13'
creditCardDetails       // array('MasterCard', '4485480221084675', 'Aleksander Nowak', '04/13')
// Generates a random IBAN. Set $countryCode to null for a random country
iban($countryCode)      // 'IT31A8497112740YZ575DJ28BP4'
swiftBicNumber          // 'RZTIAT22263'
Faker\Provider\Color
hexcolor               // '#fa3cc2'
rgbcolor               // '0,255,122'
rgbColorAsArray        // array(0,255,122)
rgbCssColor            // 'rgb(0,255,122)'
safeColorName          // 'fuchsia'
colorName              // 'Gainsbor'
hslColor               // '340,50,20'
hslColorAsArray        // array(340,50,20)
Faker\Provider\File
fileExtension          // 'avi'
mimeType               // 'video/x-msvideo'
// Copy a random file from the source to the target directory and returns the fullpath or filename
file($sourceDir = '/tmp', $targetDir = '/tmp') // '/path/to/targetDir/13b73edae8443990be1aa8f1a483bc27.jpg'
file($sourceDir, $targetDir, false) // '13b73edae8443990be1aa8f1a483bc27.jpg'
Faker\Provider\Image
// Image generation provided by LoremPixel (http://lorempixel.com/)
imageUrl($width = 640, $height = 480) // 'http://lorempixel.com/640/480/'
imageUrl($width, $height, 'cats')     // 'http://lorempixel.com/800/600/cats/'
imageUrl($width, $height, 'cats', true, 'Faker') // 'http://lorempixel.com/800/400/cats/Faker'
imageUrl($width, $height, 'cats', true, 'Faker', true) // 'http://lorempixel.com/gray/800/400/cats/Faker/' Monochrome image
image($dir = '/tmp', $width = 640, $height = 480) // '/tmp/13b73edae8443990be1aa8f1a483bc27.jpg'
image($dir, $width, $height, 'cats')  // 'tmp/13b73edae8443990be1aa8f1a483bc27.jpg' it's a cat!
image($dir, $width, $height, 'cats', false) // '13b73edae8443990be1aa8f1a483bc27.jpg' it's a filename without path
image($dir, $width, $height, 'cats', true, false) // it's a no randomize images (default: `true`)
image($dir, $width, $height, 'cats', true, true, 'Faker') // 'tmp/13b73edae8443990be1aa8f1a483bc27.jpg' it's a cat with 'Faker' text. Default, `null`.
Faker\Provider\Uuid
uuid                   // '7e57d004-2b97-0e7a-b45f-5387367791cd'
Faker\Provider\Barcode
ean13          // '4006381333931'
ean8           // '73513537'
isbn13         // '9790404436093'
isbn10         // '4881416324'
Faker\Provider\Miscellaneous
boolean // false
boolean($chanceOfGettingTrue = 50) // true
md5           // 'de99a620c50f2990e87144735cd357e7'
sha1          // 'f08e7f04ca1a413807ebc47551a40a20a0b4de5c'
sha256        // '0061e4c60dac5c1d82db0135a42e00c89ae3a333e7c26485321f24348c7e98a5'
locale        // en_UK
countryCode   // UK
languageCode  // en
currencyCode  // EUR
emoji         // 😁
Faker\Provider\Biased
// get a random number between 10 and 20,
// with more chances to be close to 20
biasedNumberBetween($min = 10, $max = 20, $function = 'sqrt')

