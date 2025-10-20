 
 https://cyberdigi.medium.com/improve-your-password-cracking-skills-for-ncl-ctfs-2f4626117c64
 
 **Steps:** 
1. identify hash type
	- `hashid 932814uyhfnqvpf98ufp0` - cli tool
	- [hash identifier](https://hashes.com/en/tools/hash_identifier)
	- [hash analyzer - tunnels up](https://www.tunnelsup.com/hash-analyzer/)
	- [hashcat hash list](https://hashcat.net/wiki/doku.php?id=example_hashes)  - hashes and attack modes
2. generate wordlists and hashlists
	`cewl`
3. Munge and massage the wordlists
4.      Leet
5.       Append 1 special char to end
6. crack

**Tools**
hashid
www.l0phtcrack.com
[Crackstation](https//:crackstation.net) - a web tool... no way to add to the list
good for the easier questions - only rockYou

overview of basics, mask and ophcrack:
https://www.youtube.com/watch?v=ll0xcqWF_AM&t=3488s

wordlists and cewl:
https://www.youtube.com/watch?v=9Nn8d1VYBUM&t=1277s

# hashcat:
[hashcat hash types](https://hashcat.net/wiki/doku.php?id=example_hashes)]
https://hashcat.net/wiki/doku.php?id=example_hashes
[hashcat attack types](https://hashcat.net/wiki/#core_attack_modes)

general usage: 
`hashcat  -a 0 -m 0 [file of hashes] [wordlist]

-a = attack mode
-m hash style

-O - optimize kernel: no by default, may add speed but *limits password length to 15*
-w -  workload, GPU utilization ( 1-4 ), default = 2, *okay to use 4*

-a = attack mode (0= wordlist attack mode)
-m = the hash type ( number from hash type list). (md5 = 0)
-g Number - generates N random rules on the fly and applies to wordlist

**MASK attack** - passwords that are in a specific format
`hashcat  -a 3 -m 0 [file of hashes] 'SKY-KAIT-?d?d?d?d`
-a 3 = mask attack
?d = some numerical 0-9 character


**HYBRID attack**, wordlist + mask, 
-a 6
`hashcat -m 0 -a 6 hashes.txt svupws.txt ?d?d    `
?d appends a digit to each entry in wordlist

## using rules with hashcat

a ruleset is a way to manipulate your wordlist and make the words more like passwords, such as adding numners, special characters, substitutions and capitalization.  
You can apply them on the fly, or run a wordlist through a ruleset and dave the output to a new text file.

General use: 
- to save your new list:
`hashcat -r /usr/share/hashcat/best64.rule myWordlist.txt --stdout > myBest64Wordlist.txt`

- to apply on the fly without saving the new list: ( using  md5crypt hash , -m 5)
`hashcat -a 0 -m 5 -r /usr/share/hashcat/best64.rule hashes.txt wordlist.txt`

order matters - they will be applied in order
### making you own rules...

you can make your own rulefiles to
save with .rule extension

Rule files can be debugged using the -r flag to specify the rule, followed by the wordlist.

A complete list of functions can be found [here](https://hashcat.net/wiki/doku.php?id=rule_based_attack#implemented_compatible_functions).

here are some examples:

|**Function**|**Description**|**Input**|**Output**|
|---|---|---|---|
|l|Convert all letters to lowercase|InlaneFreight2020|inlanefreight2020|
|u|Convert all letters to uppercase|InlaneFreight2020|INLANEFREIGHT2020|
|c / C|capitalize / lowercase first letter and invert the rest|inlaneFreight2020 / Inlanefreight2020|Inlanefreight2020 / iNLANEFREIGHT2020|
|t / TN|Toggle case : whole word / at position N|InlaneFreight2020|iNLANEfREIGHT2020|
|d / q / zN / ZN|Duplicate word / all characters / first character / last character|InlaneFreight2020|InlaneFreight2020InlaneFreight2020 / IInnllaanneeFFrreeiigghhtt22002200 / IInlaneFreight2020 / InlaneFreight20200|
|{ / }|Rotate word left / right|InlaneFreight2020|nlaneFreight2020I / 0InlaneFreight202|
|^X / $X|Prepend / Append character X|InlaneFreight2020 (^! / $! )|!InlaneFreight2020 / InlaneFreight2020!|
|r|Reverse|InlaneFreight2020|0202thgierFenalnI|




## rejection rules 
can be used to prevent the processing of such words.

Words of length less than N can be rejected with `>N`, while words greater than N can be rejected with `<N`. A list of rejection rules can be found [here](https://hashcat.net/wiki/doku.php?id=rule_based_attack#rules_used_to_reject_plains).

**must use -k or -j flag** for rejection rules

# john the ripper
1. Use a pre-made wordlist. (Easy)
2. Create your own enumerated wordlist. (Medium)
3. Generating a wordlist from a database. (Hard)database or what they tell you about a perosn

`john --wordlist=myWordlist.txt hashes.txt`
--format=md5crypt - if you need to specify

rules based attack with custom wordlist
`john ./hashfile --wordlist=mylist --rules /OneRuleToRuleThemAll.rule
 
munge
`munge.py -l 9 -i ./wordlist -o ./mungedWordlist`

# pdfs and zips
zip2john file.zip > hash.txt
run john on the hash with rockyou

# ophcrack - NTLM attack (windows) 

Must download rainbow tables from sourceforge
XP Free and XP Special 

**Separate cracking (recommended approach)**:
- Crack the LM hash first (it's much weaker):    
    `hashcat -m 3000 lmhash.txt wordlist.txt`
- Then crack the NT hash:    
    `hashcat -m 1000 nthash.txt wordlist.txt`
**Combined format**: You can use the NTLM format that includes both hashes: (lthash and nthash)
``hashcat -m 1000 combined.txt wordlist.txt
Where your combined.txt file would contain each hash pair on a single line like this:
`username:RID:LMHash:NTHash:::`

# Wordlists 
in kali:
/usr/share/wordlists/rockyou.txt
## community built wordlists
movies: https://pastebin.com/eRdpBQg2

## building wordlists with cewl

cewl -d depth to spider -m minimum word length -w output wordlist url of website

crawl a page:
`cewl [url] -d (depth) -w (file to write)`

**clean** list:  look at the list and see what needs to be fixed

**change all to all lowercase:**
tr '[:upper:]' '[:lower:]'  < input.txt > output.txt

get rid of wikipedia addons ('rft$')
`cat wordlist.txt | egrep "rft$" -v > cleansvu.txt`

filter out list based on word size
`grep -E '^.{6,}$'  
filter out less than 6 char's

**remove duplicates**
awk '!seen[$0]++' input.txt > output.txt




## cupp
CUPP stands for Common User Password Profiler, and is used to create highly targeted and customized wordlists based on information gained from social engineering and OSINT. People tend to use personal information while creating passwords, such as phone numbers, pet names, birth dates, etc. CUPP takes in this information and creates passwords from them.

## KWPROCESSOR

`Kwprocessor` is a tool that creates wordlists with keyboard walks. Another common password generation technique is to follow patterns on the keyboard. These passwords are called keyboard walks, as they look like a walk along the keys. For example, the string "`qwertyasdfg`" is created by using the first five characters from the keyboard's first two rows. This seems complex to the normal eye but can be easily predicted. `Kwprocessor` uses various algorithms to guess patterns such as these.

The tool can be found [here](https://github.com/hashcat/kwprocessor) and has to be installed manually.

## Princeprocessor

`PRINCE` or `PRobability INfinite Chained Elements` is an efficient password guessing algorithm to improve password cracking rates. [Princeprocessor](https://github.com/hashcat/princeprocessor) is a tool that generates passwords using the PRINCE algorithm. The program takes in a wordlist and creates chains of words taken from this wordlist. For example, if a wordlist contains the words:

# salted passwords
$5 might specify the version, then some characters for the salt, then another $, then the password

`hashcat -m 7400 -a 0 [file] [wordlist]`

# crunch - generate wordlist / passwords form scratch or defined patter

usage:  command / min / max / -t - use a pattern / pattern / -o outputFile

`crunch min max -t SKY-KAIT-%%%% -o SKY_KAIT_NUMB.txt`

# pdf encrypted

`pdf2john  [encryptedFile.pdf} > hashOutput.txt`

remove filename from front of hashfile and save to clean.txt
`cat hash.txt | cut -d ":" -f 2- > clean.txt

# yes crypt , etc/shadow
https://www.cyberciti.biz/faq/understanding-etcshadow-file/

`hollie:$y$j9T$/WzixhAsn8sdXhCquYzh01$KZlio78LilItobsx/17ecFf1e2SbsduhP1sZEWuHrL4:18934:0:99999:7:::

`$y$j9T$`  
indicates yescrypt, which is not supported by hashcat or john, but if you run john in a system that supports yescrypt natively (Kali  linux does ), you can tell John to use the systems native yescrypt hashing function:

```bash
john --format=crypt --wordlist=/usr/share/wordlists/rockyou.txt passwords.txt
```

https://discord.com/channels/1010199032536236052/1010199033148608554/1212645513690746911

copy the entire etc/shadow file into a text file and use it as the hash file, run john without a wordlist and --format=crypt

`john shadowFile.txt --format=crypt`

WHY?  
without a given wordlist, **it makes one on the spot based on the username**, so the password is quick: hollie03, based on username hollie



