# OverTheWire-Leviathan
Walk-through guide with examples for the OverTheWire Leviathan game

## Level 00
**Needs:** Web browser + puTTY
User/Pass: `leviathan0`
Connect: `leviathan.labs.overthewire.org`

**Nota Bene:** No hints are given (via website) for **ANY** of the levels in this game
Data for the levels can be found in this game

## Level 01
No hints

```
cd .backup/
cat bookmarks.html | grep password
```
  * Password: rio6egei8m

## Level 02
`./check` asks for a password
**Nota Bene:** Look into the `ltrace` command

`ltrace ./check`
* 1st - saw `strcmp()` between "pas" & "sex"
* Assume first three chars of password is "sex"

Passing "sex" to `./check` causes new terminal to open
* `whoami` shows I am now `leviathan2`

Now `cat` the password
* `/etc/leviathan_pass/leviathan2`

Pass: ougahZi8Ta

## Level 03
`./printfile` - \*\*\***File Printer**\*\*\*
* Usage: `./printfile filename`

`ltrace ./printfile /etc/leviathan_pass/leviathan2
* Shows calls to functions:
  1. `accesss()`
  2. `snprintf()`
  3. `system()`
1. Checks that we have access to the file being examined
2. Uses `snprintf()` to copy the passed file name into a string
  * "`/bin/cat %s`"
3. Call the crafted "`/bin/cat %s`" command using the `system()` function

**Note:** Can exploit `snprintf()` to create a `/bin/cat` system call to multiple files (e.g. file: `file\ aswd`)

1. Create `tmp` folder
2. Create a link to the desired password file
  1. `ln -s /etc/leviathan_pass/leviathan3 ./file`
3. Create touch file
  1. `touch file\asdf` makes file "file asdf" (exploit part)
4. Call `./printfile` on the created touch file (`file\ asdf`)

The reason this works is that when the file `file\ asdf` is cllaed by `snprintf()` "`file\ asdf`" becomes "`file`" and "`asdf`"
* This allows for passing the `access()` function while still reading the *desired* password file

Pass: Ahdiemoo1j

## Level 04
`ltrace ./level3`
* see `strcmp` against `snlprintf`

`./level3` -> `snlprintf` -> `cat /etc/leviathan_pass/leviathan4`

Pass: vuH0coox6m

## Level 05
`cd ./trash`
`./bin` - returns sets of binary

Use binary to ascii converter
* Pass: Tith4cokei

## Level 06
`./leviathan5`

```
ln -s /etc/leviathan_pass/leviathan6 /tmp/file.out
```
* Pass: UgaoFee4li

## Level 07
`./leviathan6`
* Need 4 digit pin code

Brute force the pin
* Can do one-liner:
  * `for i in {0000..9999}; do ./leviathan6 $i; echo $i; done`

Get new shell
* `cat /etc/leviathan_pass/leviathan7`
  * Pass: ahy7MaeBo9
