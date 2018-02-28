# Categories
- [Scripting](#scripting)
- [Cryptography](#cryptography)
- [Reversing](#reversing)
- [Interweb](#interweb)
- [Network](#network)
- [Recon](#recon)
- [Passwords](#passwords)
- [Trivia](#trivia)
- [Blast from the Past](#blastfromthepast)

# Scripting
## Basic Math - 10
We are given a file with numbers on each line and told to find the sum. Because programmers never do calculations by hand, let's write a Python script to do the math for us.
```python
with open('input.txt') as f:
	s = 0
	for line in f:
		s += int(line.strip())
	print(f'Sum: {s}')
```
Answer: `49562942146280612`

## More Basic Math - 50
Now we have many lines in the input, but we can use the same script that we wrote for the previous problem.
Answer: `50123971501856573397`

## Even More Basic Math with Some Junk - 100
Our input now contains multiple numbers per line and some extra characters. Let's look at the first few lines:
```
2222689725896    2 47855705     3569292   ,
4074440    , 
77859765972   7318  ,7241533041280677     ,1167965   69376150    012131965722352    
4957969707876    ,
...
```
We want to add all of the numbers on each line, and add that to our total. Using regular expressions, we can extract all of the numbers on each line. Here is the modified version of the above program:
```python
import re
with open('input.txt') as f:
    s = 0
    for line in f:
        s += sum(int(x) for x in re.findall(r'\d+',line.strip()))
    print('Sum:',s)
```
Answer: `34659711530484678082`

## JSON Parsing 1 - 200
## JSON Parsing 2 - 300
## JSON Parsing, Exploring Data, and Using Imports - 400

# Cryptography
## I have a message for you - 100
The first encoding is the binary representation of ASCII characters. [Deconding](https://www.rapidtables.com/convert/number/binary-to-ascii.html) this results in the string:
```
Tmh5b2IgdmZzIGEgY2N5IGR2IHZqaGV6bXYgYnltdyBkZWNyIHhmZ3YgaCBkZmhwcmdjIHZvZXF0aiBkb21tIGdjIGhrcnNkZHcgcnIgemJ1IGdoa3FiIHB6IG5jbHhxZHd0cHVxcg==
```
This is a base64 encoded string. After we [decode](https://www.base64decode.org/) this, we are left with another strange encryption:
```
Nhyob vfs a ccy dv vjhezmv bymw decr xfgv h dfhprgc voeqtj domm gc hkrsddw rr zbu ghkqb pz nclxqdwtpuqr
```

## Always in a rush to find it... - 200
This message was encrypted using a Jefferson Wheel Cipher. [Deciphering](https://www.dcode.fr/jefferson-wheel-cipher#0) the message gives us the code:
```
FOURTHREEDOTEIGHTSEVENNINEZEROONESIXCOMMASPACETACONEZEROTHREEDOTFOURFIVENINEZEROZEROFOUR
```
This can be interpreted as coordinates to a real location! The above turns into these coordinates `43.87901, -103.459004`
Looking on a [map](https://www.google.com/maps/place/43%C2%B052'44.4%22N+103%C2%B027'32.4%22W/@43.8789567,-103.4590225,97m/data=!3m1!1e3!4m5!3m4!1s0x0:0x0!8m2!3d43.87901!4d-103.459004), you can see that this is right on top of Mount Rushmore which happens to be the flag.
Answer: `Mount Rushmore`

## Story Time! - 200
What on earth is this cipher? It looks a lot more daunting than it really is. First, we need to determine what those strange symbols are in the cipher († and ‡). A quick search shows that these are called daggers and double daggers. Next, I simply searched "ciphers containing daggers", and the **Gold-Bug** cipher came up. This cipher is a mono-alphabetic substitution where each symbol is replaced by a letter.

*Note:* The problem has some italicized letters. This is because surrounding text with asterisks causes it to become italicized. In the cipher, `*` represents the letter `N`. Here is the correct ciphertext:
```
-5.;56* 76†† ?)8† ;48 3‡0† 2?3 -6.48( ;‡ 46†8 ;48 0‡-5;6‡* ‡1 46) ;(85)?(8 6* 5 );‡(: ](6;;8* 2: 8†35( 5005* .‡8 6* 1053 6) .6(5;8)5*††5338()
```
Using [this](https://www.dcode.fr/gold-bug-poe) tool, I was able to decrypt the message.
```
CAPTAIN KIDD USED THE GOLD BUG CIPHER TO HIDE THE LOCATION OF HIS TREASURE IN A STORY WRITTEN BY EDGAR ALLAN POE IN FLAG IS PIRATESANDDAGGERS
```
Answer: `flag{piratesanddaggers}`

## Picture Words - 200
This one was very fun because I did it the old fashioned way. I printed out the image and assigned a letter to each unique glyph I saw. I ended up with a code like this:
```
ABCDEFGHIDJKLFMGCNEOIJPQQBCLFFQEIDEQCPOFRVSEQBPTUIDJPQQBCGCPHCERGPJIEKITQSLCEAFLQBPQBFSEPDOAFLOE
```
Using [this](https://quipqiup.com/) cryptogram solver, I was able to successfully decipher the code into this:
```
WHEN SOLVING PROBLEMS DIG AT THE ROOTS INSTEAD OF JUST HACKING AT THE LEAVES FLAG IS PICTURES WORTH A THOUSAND WORDS
```
Answer: `picturesworthathousandwords`

## Crypto is an Art Form - 300

## How much can you throw on a Caesar salad? - 300

## Dot Dot Dashish - 300
The title of the challenge refers to morse code; however, we are given only numbers. In order to determine what type of encryption this was, I searched for "morse code numeric cipher". The Morbit Cipher came up, which takes a 9-letter keyword to associate with the nine bigrams of morse code (`..	.-	./	-.	--	-/	/.	/-	//`). If you look carefully at the problem description, you can see that the 9-letter key is `NEVERLANC` because it is in capital letters. We can use an online Morbit Cipher [decoder](https://www.dcode.fr/morbit-cipher) to decrpyt the message into this:
```
EVEN MORSE CODE HAD ENCRYPTION THROUGH OUT HISTORY HUMAN HAVE LOVED SECRETS YOUR FLAG IS ENCRYPTALLTHETHINGS
```
Answer: `flag{encryptallthethings}`

## That's A Big File - 300
In this file we are given a large string of base64 encoded data. When we decode it, we get a smaller string of base64 data. Using a simple loop in a Python script, we can keep decoding the string until we are left with the flag.
Answer `flag{persistant_are_we?}`

## Don't Hate Me - 500
We are given a password protected zip file and told that all characters are lowercase. Brute-forcing this would have been infeasible - the first hint reveals the password to be `youwillneverguessme`. Inside the zip archive, there is another cryptogram (similar to "Picture Words") where each figure represents a letter. I had fun and did this by hand on paper, and created the following code:
```
ABCDEFGHFIJKCELEMNCKKCEAEBNGIGEEOOPEAHAHQEMRRSTNFUBHPKUHUVJLJRWNJTKFCESBXYMJHEOZBTADPKVFFCJCWNYDRIWQQQIMPWIUHDMPWDBPQRQSAPDUAPIO
```

# Recon
## Viking's Recon - 100
The problem hints that we should check out Twitter. After finding the correct Twitter handle, the flag can be found on [this](https://twitter.com/sgviking_/with_replies) page.
Answer: `flag{Hackz_All_the_things!!}`

## Neo's Recon - 200
After some searching, we come across [Neo's page](https://neverlanctf.com/Neo) on the NeverLAN website. In the bottom-right corner, there is a Submit button. If we inspect it in Developer Tools, there is a riddle hidden in the comments. The phrases "mental perception" and "cognizance" are clues to the answer, which is `ken`. We change the input value associated with the form submission to `ken`, and are taken to [this](https://neverlanctf.com/ZD.php) page (which is only accessible after submitting the correct answer), containing the flag.
Answer: `flag{dat_ken_though}`

## purvesta's Recon - 200
After some digging around, I found purvesta's [SoundCloud](https://soundcloud.com/purvesta) account. There is a rap listed there and clicking on it reveals a comment encoded in base64. Decoding it twice results in the flag.
Answer: `flag{M1xT@pe_1n_th3_m@k1ng?}`

## bashninja's Recon - 200
After searching BashNinja online for a bit, you can eventually [discover](https://keybase.io/bashninja/sigs/qfTfkxDWtj1i-azmg7KM0ZsYH2PTVNmaRAQuRWMVIZE) that his real name is Mike Weaver. That website includes links to his social media portals, so naturally I began to check them out one by one. Much akin to the other Recon challenges, his [Twitter](https://twitter.com/miketweaver) account is where I found the name of the organization by searching `CTF`.
The [Cyber Security Club](https://www.uvucsc.com/) at UVU has the website that we need to examine. With pure luck, I eventually found the page by guessing that Mike (being the President of the club) would have [his own page](https://www.uvucsc.com/mike/); however, it is easier to discover [this](https://www.uvucsc.com/officers/) page which has all of the officers of the club. Hovering over Mike's picture, we see a message come up with the following code:
```
f{-ustc-y-o.dlgyusol-tr--lba-orsho..-i}aohdaautuclId
```
He gives us a clue by saying he was on the "fence" about this challenge. [Decoding](http://rumkin.com/tools/cipher/railfence.php) the message as a Railfence cipher gives us the flag.
Answer: `flag{you-should-start-a-club-at-your-school...i-did}`

## voldemortensen's Recon - 200

## Zesty's Challenge - 200
In order to block something from robots, which are crawlers that traverse the web to index pages on Google and other search engines, you need to make changes to a file called `robots.txt`. Going to `/robots.txt` on the website in the problem, there is a link: `/zestyschallenge.php`. Going to that page shows a video. If we open the video on [YouTube](https://www.youtube.com/watch?v=dhebl9oD5Lc), the flag is in a comment posted by Preston Pace.
Answer: `flag{10684524746ba936b43a82d84385dcf5}`

## Happy Hunting - 200
I simply searched for the quote that was provided, and the name of the village was the first result that came up.
Answer: `flag{packethackingvillage}`

## s7a73farm's Recon - 300
The first thing I did was search "s7a73farm mixes". This brings you to [this](https://www.mixcloud.com/s7a73farm/stream/) profile page. I figured that the flag could be hidden in one of the comments of a stream, so I started checking each stream for comments. I found the flag [this](https://www.mixcloud.com/s7a73farm/s7a73farm_hello-evil-corp_may_mix/) mix.
Answer: `flag{h3ll0_3viLC0rP}`

# Passwords
## Encoding != Hashing - 100
Using WireShark to open the .pcap file, we can search using **Ctrl + F** and type `flag{` and search for results. Change Display filter -> String and Packet list -> Packet detail. It directs us to an HTTP packet (no. 10464). If we look under the Hypertext Transfer Protocol > Authorization, we can see a base64 encoded string which contains our information. The decoded version is found inside as Credentials, giving us the flag.
Answer: `flag{help-me-obiwan}`   

## Zip Attack - 100

## SHA-1 - 200
For this challenge, the hints were necessary to complete it in any reasonable amount of time. The problem description and hints tell us that the first character is a digit, the fourth character is an uppercase letter, and the rest is lowercase letters and numbers. We can use [Hashcat](https://hashkiller.co.uk/hashcat-gui.aspx) to crack the SHA1 hash.
Set custom charset 1 to `?d?l` which composes of digits and lowercase letters. Since we know the password is at least four characters, we can start with a mask of `?d?1?1?u`, and add `?1` every time we do not crack the hash. I was successful in cracking the hash at length 8 with a mask of `?d?1?1?u?1?1?1?1`. In retrospect, I should have tried `?l` in place of all `?1` which would have gotten me the flag orders of magnitude faster since every character besides the first and fourth is a lowercase letter.
Answer: `1stOrder`

## The Wifi Network - 200


## The Password Manager - 300

# Reversing
## Commitment Issues - 50
We are given a binary file. If we use the command `strings <file>`, all of the strings can be extracted. Using the pipe operator, we can take the output of strings and filter out our flag using grep. Our command is now `strings commitment_issues | grep flag`, which gives us the flag.
Answer: `flag{don't_string_me_along_man!}`

## The Curse of the Hex - 100

## Where Did I Put That Flag? - 200

## Turtles All the Way Down - 300

# Interweb
## ajax_not_soap - 100
We find ourselves on a website with a simple username and password form. Whenever you type a character, it makes an AJAX request to `/webhooks/get_username.php` (which can be seen in Developer Tools > Network) for the username. If we go to that link, the username shows up as `MrClean`. Now if we check the next AJAX request for the password, the link becomes `/webhooks/get_pass.php?username=MrClean`. Following that link leads us to the flag.
Answer: `flag{hj38dsjk324nkeasd9}`

## the_red_or_blue_pill - 100
We are given a choice to take a Red Pill or a Blue Pill. Each one becomes a url query in the form `/?red` and `/?blue`. The text hints at taking both at the same time, so if we change the url query to `/?red&blue`, we are given the flag.
Answer: `flag{breaking_the_matrix...I_like_it!}`

## ajax_not_borax - 200
Once again we have a username and password input form. This time if we follow `/webhooks/get_username.php?username`, we are shown an MD5 hash value:
```
c5644ca91d1307779ed493c4dedfdcb7
```
 I put it in an online [decryption website](https://hashkiller.co.uk/md5-decrypter.aspx) and found the username to be `tideade`. Now we repeat the process at `/webhooks/get_pass.php?username=tideade`, where we are shown a base64 encoded string:
 ```
 ZmxhZ3tzZDkwSjBkbkxLSjFsczlISmVkfQ==
 ```
 Answer: `flag{sd90J0dnLKJ1ls9HJed}`

## What the LFI? - 200
The website that we need to hack is a WordPress site. We can check for vulnerabilities using [this](https://wpscans.com/scan/?id=05372f7a3b1d3b00481cefbd3a99e215) scanner. The report indicates that the "SAM Pro" plugin is susceptible to an LFI attack, which is what the problem is about. Encrypting `/var/www/blah.php` into base64 yields us our payload: `L3Zhci93d3cvYmxhaC5waHA=`
We can include that using the SAM Pro plugin in the URL as such:
```
http://54.201.224.15:14099/wp-content/plugins/sam-pro-free/sam-pro-ajax-admin.php?action=NA&wap=L3Zhci93d3cvYmxhaC5waHA=
```
Answer: `flag{dont_include_files_derived_from_user_input_kthx_bai}`

## Das blog - 200
Here we have another login page with a username and password. If we check Developer Tools, someone left behind a comment with the following credentials:
```
User: JohnsTestUser
Pass: AT3stAccountForT3sting
```
When we log in with this account, we are given user permissions. Go back once and you'll find yourself on the blog page. We can change the cookie which stores our permissions to `admin`, refresh the page, and find our flag.

## tik-tik-boom - 300

# Network
## Fuzzy Packets - 300

# Trivia
## Can you name It? - 50
Answer: `Common Vulnerabilities and Exposures`

## Can you find it? (Bonus) - 50
Answer: `EternalBlue`

## I love tools... - 50
Answer: `Developer Tools`

## Yummy... - 50
Answer: `Cookies`

## Can you find it? - 100

## Who knew? - 100
Answer: `windows nt`

## What is it? - 100
Answer: `Exploit-DB`

## Can you search it? - 100

## How far can you go? - 200
Password 1: `+++The Mentor+++`

Password 2: `tech model railroad club`

Password 3: `phreaking`

Password 4: `2600 hertz`

Password 5: `february 1984`

Password 6: `new york stock exchange`

Password 7: `the gibson`

Password 8: `global thermonuclear war`

Password 9: `the matrix`

Answer: `flag{W3lc0m3_2_th3_r4bb!7h0l3}`

## Can you use it? - 200

<a id="blastfromthepast"></a>
# Blast from the Past
## cookie_monster - 50
If we go into Developer Tools > Application > Cookies, we see there is a field that tells us to put a name. Replacing the value with `Elmo` and refreshing the page shows the flag.
Answer: `flag{C00kies_4r3_the_b3st}`

## Siths use Ubuntu (Part 1 of 3) - 125
## Siths use Ubuntu (Part 2 of 3) - 150
## Siths use Ubuntu (Part 3 of 3) - 150
