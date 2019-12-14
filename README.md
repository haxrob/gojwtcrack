# jwtcrack
Fast JSON Web Token (JWT)cracker. Currently supports dictionary attacks against HS256.

## Usage

To build:
```
$ go build -o jwtcrack main.go
```

To run, place a token into a text file and specify this file with the -t flag.
The dictionary file to brute force with can either be specified with the -d flag or piped in via stdin, e.g.

```
$ cat rockyou.txt | ./jwtcrack -t mytoken.txt
```

Help file:
```
Usage of ./jwtcrack:
  -c int
    	set concurrent workers (default 10)
  -d string
    	Dictionary file. If ommited, will read from stdin
  -t string
    	File containing JWT token(s)
```

Example:
```
$ ./jwtcrack -t token.txt -d ~/SecLists/Passwords/xato-net-10-million-passwords-1000000.txt
secret123	eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.y3kjst36zujMF4HssVk3Uqxf_3bzumNAvOB9N0_uRV4
```
## Benchmark

Cracking a token that has the secret of the last entry in a 3.7 million long dictionary on a Intel 2.8 Ghz i5:

| Program | Execution time |
--- | --- |
| jwtcrack | 3.4 seconds | 
| jwtcat | 166 seconds |

```
$ wc -l  openwall.net-all.txt 
3721224 openwall.net-all.txt

$ tail -1 openwall.net-all.txt 
ex-wethouder
```

```
time ./jwtcrack -t token.txt -d openwall.net-all.txt 
ex-wethouder	eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.L1UzzeBYF7-NCBw-_1AJ1pihxG3pbJwOfbbzG86Qhe0

real	0m3.470s
user	0m9.986s
sys	0m0.085s
```

https://github.com/aress31/jwtcat
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
