# Example Usage

1. Create the Certificate Authority and Optional Intermediate CA's

```
./mkrootca
./mkfullca
./mkpartca
./mkwinca
./mkeccca
```

2. Elliptic Curve Cryptography (prime256v1) Certificate Authority

```
./ecc_mkcrt ecc1.lab.home ecc.lab.home 127.0.0.1
openssl x509 -in ecc1_lab_home.crt -text -noout
```
```
./ecc_mkcrt ecc2.lab.home
openssl x509 -in ecc2_lab_home.crt -text -noout
```
```
./ecc_mkcrt *.ecc.lab.home
openssl x509 -in star_ecc_lab_home.crt -text -noout
```
```
./ecc_mkcsr ecc3.lab.home
openssl req -in ecc3_lab_home.csr -text -noout
```
```
./ecc_signcsr ecc3_lab_home.csr ecc.lab.home 127.0.0.1
openssl x509 -in ecc3_lab_home.crt -text -noout
```

3. RSA Full Certificate Authority

```
./full_mkclncrt "Full1 Client/emailAddress=full1.client@lab.home" full.client\@lab.home
openssl x509 -in Full1_Client.crt -text -noout
```
```
./full_mkclncrt "Full2 Client"
openssl x509 -in Full2_Client.crt -text -noout
```
```
./full_mkevcrt fullev.lab.home ev.lab.home
openssl x509 -in fullev_lab_home.crt -text -noout
```
```
./full_mksrvcrt full1.lab.home
openssl x509 -in full1_lab_home.crt -text -noout
```
```
./full_mksrvcrt full2.lab.home 127.0.0.1
openssl x509 -in full2_lab_home.crt -text -noout
```
```
./full_revoke full2_lab_home.crt
```
```
./full_mksrvcrt *.full.lab.home
openssl x509 -in star_full_lab_home.crt -text -noout
```
```
./full_mkclncsr "Full3 Client/emailAddress=full3@lab.home"
openssl req -in Full3_Client.csr -text -noout
```
```
./full_signclncsr Full3_Client.csr full.client\@lab.home
openssl x509 -in Full3_Client.crt -text -noout
```
```
./full_mksrvcsr full3.lab.home
openssl req -in full3_lab_home.csr -text -noout
```
```
./full_signsrvcsr full3_lab_home.csr full.lab.home ::1
openssl x509 -in full3_lab_home.crt -text -noout
```
```
./full_mksrvcsr full4.lab.home
openssl req -in full4_lab_home.csr -text -noout
```
```
./full_signaddev full4_lab_home.csr
openssl x509 -in full4_lab_home.crt -text -noout
```

4. RSA Partial Certifcate Authority

```
./part_mkcrt part1.lab.home part.lab.home 127.0.0.1 ::1
openssl x509 -in part1_lab_home.crt -text -noout
```
```
./part_mkcrt *.part.lab.home
openssl x509 -in star_part_lab_home.crt -text -noout
```
```
./part_mkclncrt "Part1 Client"
openssl x509 -in Part1_Client.crt -text -noout
```
```
./part_mkeccrt part2.lab.home
openssl x509 -in part2_lab_home.crt -text -noout
```
```
./part_mkrpki rpki.lab.home
openssl x509 -in rpki_lab_home.crt -text -noout
```
```
./part_mksan part3.lab.home
openssl x509 -in part3_lab_home.crt -text -noout
```
```
./part_mkself part4.lab.home
openssl x509 -in part4_lab_home.crt -text -noout
```
```
./part_mkcsr part5.lab.home
openssl req -in part5_lab_home.csr -text -noout
```
```
./part_signcsr part5_lab_home.csr
openssl x509 -in part5_lab_home.crt -text -noout
```

5. RSA Windows Certificate Authority

```
./win_dccrt win1.ad.lab.home
openssl x509 -in win1_ad_lab_home.crt -text -noout
```
```
./win_dccsr win2.ad.lab.home
openssl req -in win2_ad_lab_home.csr -text -noout
```
```
./win_mkcsr win3.ad.lab.home
openssl req -in win3_ad_lab_home.csr -text -noout
```
```
./win_oldsign win3_ad_lab_home.csr
openssl x509 -in win3_ad_lab_home.crt -text -noout
```
```
./win_mkcsr win4.ad.lab.home win.ad.lab.home 127.0.0.1 ::1
openssl req -in win4_ad_lab_home.csr -text -noout
```
```
./win_signcsr win4_ad_lab_home.csr
openssl x509 -in win4_ad_lab_home.crt -text -noout
```
```
./win_mkpfx win4_ad_lab_home.crt
openssl pkcs12 -in win4_ad_lab_home.pfx -info -password pass:changeit -nodes
```

