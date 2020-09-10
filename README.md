# gojwtcrack
Fast JSON Web Token (JWT) cracker. Currently supports dictionary attacks against HS256.

## Installation
### Binaries
Compiled 64-bit executable files for Windows, Mac and Linux are available [here](https://github.com/x1sec/gojwtcrack/releases/)

### Go get
If you would prefer to build yourself (and Go is setup [correctly](https://golang.org/doc/install)):
```
go get -u github.com/x1sec/gojwtcrack
```
### Building from source
If you prefer to compile yourself:
```
$ go build -o gojwtcrack main.go
```

## Running

Place a token into a text file and specify this file with the `-t` flag.
The  file containing a list of secrets (e.g. password dictionary file) can either be specified with the `-d` flag or piped in via stdin, e.g.

```
$ cat rockyou.txt | ./gojwtcrack -t mytoken.txt
```

Help file:
```
Usage of ./gojwtcrack:
  -c int
    	set concurrent workers (default 10)
  -d string
    	Dictionary file. If ommited, will read from stdin
  -t string
    	File containing JWT token(s)
```

Example:
```
$ ./gojwtcrack -t token.txt -d ~/SecLists/Passwords/xato-net-10-million-passwords-1000000.txt
secret123	eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.y3kjst36zujMF4HssVk3Uqxf_3bzumNAvOB9N0_uRV4
```
## Benchmark

Cracking a token that uses a secret contained in the last entry of 3.7 million long dictionary file on a Intel 2.8Ghz i5.
Comparing against an another JWT cracking program ([jwtcat](https://github.com/aress31/jwtcat) - chosen arbitrarily from a Google search) shows a 48.8% speed increase when using jwtcrack.

| Program | Execution time |
--- | --- |
| gojwtcrack | 3.4 seconds | 
| jwtcat | 166 seconds |

Dictionary size:
```
$ wc -l  openwall.net-all.txt 
3721224 openwall.net-all.txt
```

Last passphrase in dictionary file:
```
$ tail -1 openwall.net-all.txt 
ex-wethouder
```

Execution time with gojwtcrack :
```
time ./gojwtcrack -t token.txt -d openwall.net-all.txt 
ex-wethouder	eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.L1UzzeBYF7-NCBw-_1AJ1pihxG3pbJwOfbbzG86Qhe0

real	0m3.470s
user	0m9.986s
sys	0m0.085s
```

Execution time with jwtcat :
```
time python3 jwtcat.py -t eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.L1UzzeBYF7-NCBw-_1AJ1pihxG3pbJwOfbbzG86Qhe0 -w openwall.net-all.txt
<.. cut ..>
[INFO] Secret key: ex-wethouder
[INFO] Secret key saved to location: jwtpot.potfile
[INFO] Finished in 166.24219918251038 sec

real	2m46.317s
user	2m46.235s
sys	0m0.056s
```
## TODO

- [ ] Support cracking multiple tokens in input text file
- [ ] Support brute force cracking (permutations in character set)
