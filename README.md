# jwtcrack
Fast JSON Web Token (JWT) brute-force cracker in Go

## Instructions

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
