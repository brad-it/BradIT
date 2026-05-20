#John The Ripper Overview

What is John the Ripper?

John the Ripper (often called JtR) is a popular open-source password security auditing and password recovery tool available on Linux, Windows, and macOS.

It is commonly used by:

Security professionals
System administrators
Penetration testers
Digital forensics analysts
Developers testing password strength

John the Ripper can:

Crack password hashes
Test password strength
Recover lost passwords
Audit weak credentials
Work with many hash formats and encrypted files

##The Jumbo version supports:

ZIP files
SSH keys
PDF files
Office documents
WPA/WPA2 handshakes
Kerberos tickets
RAR archives
KeePass databases
And many more
Installing John the Ripper on Linux
Ubuntu / Debian
sudo apt update
sudo apt install john -y
Fedora
sudo dnf install john
Arch Linux
sudo pacman -S john
Verify Installation
john --version
Basic Terminology
Term	Meaning
Hash	A one-way encrypted representation of data
Wordlist	A text file containing possible passwords
Cracking	Attempting to recover the original password
Salt	Random data added before hashing
Dictionary Attack	Using a list of passwords
Brute Force	Trying every possible combination
Basic Usage
Cracking a Hash File

Suppose you have a file called hashes.txt.

Example:

user:$6$randomsalt$3vQ6...

Run:

john hashes.txt
Showing Cracked Passwords
john --show hashes.txt

Example output:

user:password123
Using a Wordlist

A wordlist attack tries passwords from a file.

Example using the popular rockyou.txt wordlist:

john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
Incremental (Brute Force) Mode
john --incremental hashes.txt

This mode systematically tries combinations of characters.

⚠️ Brute force attacks can take a very long time depending on password complexity.

Single Crack Mode

Uses username and related information to generate guesses.

john --single hashes.txt
Identifying Hash Types

Use:

john --list=formats

You can specify a format manually:

john --format=sha512crypt hashes.txt



# John Utility Reference Table

| Utility | Purpose | Example Input | Output Hash File |
|---|---|---|---|
| `ssh2john.py` | Extract hash from encrypted SSH private keys | `id_rsa` | `ssh.hash` |
| `zip2john` | Extract hash from ZIP archives | `secret.zip` | `zip.hash` |
| `rar2john` | Extract hash from RAR archives | `locked.rar` | `rar.hash` |
| `pdf2john.py` | Extract hash from password-protected PDFs | `document.pdf` | `pdf.hash` |
| `office2john.py` | Extract hash from Microsoft Office files | `report.docx` | `office.hash` |
| `keepass2john` | Extract hash from KeePass databases | `vault.kdbx` | `keepass.hash` |
| `hccap2john` | Convert WPA/WPA2 captures | `capture.hccap` | `wifi.hash` |
| `unshadow` | Combine Linux passwd/shadow files | `passwd shadow` | `combined.txt` |
ssh2john

    Extracts password hashes from encrypted SSH private keys.

        ssh2john.py id_rsa > ssh.hash
        john ssh.hash

        john --show ssh.hash

zip2john

    Extracts hashes from ZIP files.

        zip2john secret.zip > zip.hash
        john zip.hash


pdf2john

    Extracts hashes from password-protected PDFs.

        pdf2john.py file.pdf > pdf.hash
        john pdf.hash


hccap2john

    Used with captured WPA/WPA2 Wi-Fi handshakes.

        hccap2john capture.hccap > wifi.hash
        john wifi.hash


##Cracking Modes Table

Mode             | Description	                | Example Command                              |	Best Use Case
Single Crack     |	Uses usernames and metadata	| 'john --single hashes.txt'                   | Fast weak-password discovery
Wordlist Mode    |	Uses password lists	        | john --wordlist=rockyou.txt hashes.txt       |	Common password auditing
Incremental Mode |	Brute-force combinations    |	john --incremental hashes.txt              |	Unknown passwords
Rules Mode       |	Mutates wordlists           | john --wordlist=words.txt --rules hashes.txt |	Realistic password variations
Mask Mode        |	Uses specific patterns      | john --mask='?d?d?d?d' hashes.txt          |	PINs or known patterns
Fork Mode        |	Multi-process cracking      | john --fork=4 hashes.txt                   |	Multi-core CPUs
Session Mode     |	Save and restore sessions   | john --session=test hashes.txt             |	Long cracking jobs


##Useful Commands Reference

Command	                            | Description
john hashes.txt                     | Start cracking
john --show hashes.txt              | Show cracked passwords
john --wordlist=file.txt hashes.txt | Dictionary attack
john --incremental hashes.txt       | Brute-force mode
john --single hashes.txt            | Single crack mode
john --restore                      | Resume previous session
john --session=name hashes.txt      | Create named session
john --fork=4 hashes.txt            | Multi-process cracking
john --format=bcrypt hashes.txt	    | Specify hash format
john --list=formats	                | List supported formats
john --test                         | Benchmark performance
john --mask='?d?d?d?d' hashes.txt   | Crack using masks

Hash Format Reference Table

Hash Type	| Example Prefix        |	Common Usage
raw-md5	    |    5f4dcc...	        |   Legacy applications
raw-sha1    |	cbfdac...	        |   Older software systems
bcrypt	    |   $2a$	            |   Modern web applications
sha512crypt	|   $6$	                |   Linux password hashes
NT	        |   32-character NTLM	|   Windows systems
zip	        |   ZIP encrypted data	|   ZIP archives
rar	        |   RAR encrypted data	|   RAR archives
KeePass	    |   KeePass database    |   Password vaults
pdf	        |   PDF encryption	    |   Protected documents

Example Hashes for Practice
MD5 Example
5f4dcc3b5aa765d61d8327deb882cf99
SHA1 Example
cbfdac6008f9cab4083784cbd1874f76618d2a97

⚠️ These are public demonstration hashes.

##Tips and Best Practices

    Use strong and unique passwords
    Prefer password managers
    Use long passphrases
    Enable MFA/2FA whenever possible
    Regularly audit password strength
    Avoid using common wordlists as real passwords
