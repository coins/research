# Open SSL Timestamps 

Prove that a server returned a certain answer via SSL dumps and [OpenTimestamps](https://opentimestamps.org/)



Most simple example:
```bash
echo -e 'GET /source/OCB-patent-grant-OpenSSL.pdf HTTP/1.1\nHost: www.openssl.org\n\n' | openssl s_client -crlf -msg -debug -quiet -connect www.openssl.org:443 -servername www.openssl.org > openssl_org_dump.dat
```

Timestamp the dumped file `openssl_org_dump.dat` on [OpenTimestamps.org](https://opentimestamps.org/)
