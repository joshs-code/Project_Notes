# String Replacement

Within the Stratos DC, the backup server holds template XML files crucial for the Nautilus application. Before utilization, these files require valid data insertion. As part of routine maintenance, system admins at `xFusionCorp Industries` employ string and file manipulation commands.

Your task is to substitute all occurrences of the string `Text` with `Sonar` within the XML file located at `/root/nautilus.xml` on the backup server.



My Solution:

```
thor@jumphost ~$ ssh clint@172.16.238.16
The authenticity of host '172.16.238.16 (172.16.238.16)' can't be established.
ED25519 key fingerprint is SHA256:Lr2DHKZe3fexdQh6wSf7XNNm4EXuI9p4bsXX0EvcTP0.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '172.16.238.16' (ED25519) to the list of known hosts.
clint@172.16.238.16's password: 
[clint@stbkp01 ~]$ sed --help
Usage: sed [OPTION]... {script-only-if-no-other-script} [input-file]...

  -n, --quiet, --silent
                 suppress automatic printing of pattern space
      --debug
                 annotate program execution
  -e script, --expression=script
                 add the script to the commands to be executed
  -f script-file, --file=script-file
                 add the contents of script-file to the commands to be executed
  --follow-symlinks
                 follow symlinks when processing in place
  -i[SUFFIX], --in-place[=SUFFIX]
                 edit files in place (makes backup if SUFFIX supplied)
  -c, --copy
                 use copy instead of rename when shuffling files in -i mode
  -b, --binary
                 does nothing; for compatibility with WIN32/CYGWIN/MSDOS/EMX 
                 (open files in binary mode; CR+LF are not processed specially)
  -l N, --line-length=N
                 specify the desired line-wrap length for the `l' command
  --posix
                 disable all GNU extensions.
  -E, -r, --regexp-extended
                 use extended regular expressions in the script
                 (for portability use POSIX -E).
  -s, --separate
                 consider files as separate rather than as a single,
                 continuous long stream.
      --sandbox
                 operate in sandbox mode (disable e/r/w commands).
  -u, --unbuffered
                 load minimal amounts of data from the input files and flush
                 the output buffers more often
  -z, --null-data
                 separate lines by NUL characters
      --help     display this help and exit
      --version  output version information and exit

If no -e, --expression, -f, or --file option is given, then the first
non-option argument is taken as the sed script to interpret.  All
remaining arguments are names of input files; if no input files are
specified, then the standard input is read.

GNU sed home page: <https://www.gnu.org/software/sed/>.
General help using GNU software: <https://www.gnu.org/gethelp/>.
E-mail bug reports to: <bug-sed@gnu.org>.

[clint@stbkp01 ~]$ sudo sed 's/Text/Sonar/g' /root/nautilus.xml > nautilus.xml
[clint@stbkp01 ~]$ sudo mv nautilus.xml /root/nautilus.xml
```
