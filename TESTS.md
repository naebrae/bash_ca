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
./ecc_mkcrt ecc1.lab.local ecc.lab.local 127.0.0.1
openssl x509 -in ecc1_lab_local.crt -text -noout
```
```
./ecc_mkcrt ecc2.lab.local
openssl x509 -in ecc2_lab_local.crt -text -noout
```
```
./ecc_mkcrt *.ecc.lab.local
openssl x509 -in star_ecc_lab_local.crt -text -noout
```
```
./ecc_mkcsr ecc3.lab.local
openssl req -in ecc3_lab_local.csr -text -noout
```
```
./ecc_signcsr ecc3_lab_local.csr ecc.lab.local 127.0.0.1
openssl x509 -in ecc3_lab_local.crt -text -noout
```

3. RSA Full Certificate Authority

```
./full_mkclncrt "Full1 Client/emailAddress=full1.client@lab.local" full.client\@lab.local
openssl x509 -in Full1_Client.crt -text -noout
```
```
./full_mkclncrt "Full2 Client"
openssl x509 -in Full2_Client.crt -text -noout
```
```
./full_mkevcrt fullev.lab.local ev.lab.local
openssl x509 -in fullev_lab_local.crt -text -noout
```
```
./full_mksrvcrt full1.lab.local
openssl x509 -in full1_lab_local.crt -text -noout
```
```
./full_mksrvcrt full2.lab.local 127.0.0.1
openssl x509 -in full2_lab_local.crt -text -noout
```
```
./full_revoke full2_lab_local.crt
```
```
./full_mksrvcrt *.full.lab.local
openssl x509 -in star_full_lab_local.crt -text -noout
```
```
./full_mkclncsr "Full3 Client/emailAddress=full3@lab.local"
openssl req -in Full3_Client.csr -text -noout
```
```
./full_signclncsr Full3_Client.csr full.client\@lab.local
openssl x509 -in Full3_Client.crt -text -noout
```
```
./full_mksrvcsr full3.lab.local
openssl req -in full3_lab_local.csr -text -noout
```
```
./full_signsrvcsr full3_lab_local.csr full.lab.local ::1
openssl x509 -in full3_lab_local.crt -text -noout
```
```
./full_mksrvcsr full4.lab.local
openssl req -in full4_lab_local.csr -text -noout
```
```
./full_signaddev full4_lab_local.csr
openssl x509 -in full4_lab_local.crt -text -noout
```

4. RSA Partial Certifcate Authority

```
./part_mkcrt part1.lab.local part.lab.local 127.0.0.1 ::1
openssl x509 -in part1_lab_local.crt -text -noout
```
```
./part_mkcrt *.part.lab.local
openssl x509 -in star_part_lab_local.crt -text -noout
```
```
./part_mkclncrt "Part1 Client"
openssl x509 -in Part1_Client.crt -text -noout
```
```
./part_mkeccrt part2.lab.local
openssl x509 -in part2_lab_local.crt -text -noout
```
```
./part_mkrpki rpki.lab.local
openssl x509 -in rpki_lab_local.crt -text -noout
```
```
./part_mksan part3.lab.local
openssl x509 -in part3_lab_local.crt -text -noout
```
```
./part_mkself part4.lab.local
openssl x509 -in part4_lab_local.crt -text -noout
```
```
./part_mkcsr part5.lab.local
openssl req -in part5_lab_local.csr -text -noout
```
```
./part_signcsr part5_lab_local.csr
openssl x509 -in part5_lab_local.crt -text -noout
```

5. RSA Windows Certificate Authority

```
./win_dccrt win1.ad.lab.local
openssl x509 -in win1_ad_lab_local.crt -text -noout
```
```
./win_dccsr win2.ad.lab.local
openssl req -in win2_ad_lab_local.csr -text -noout
```
```
./win_mkcsr win3.ad.lab.local
openssl req -in win3_ad_lab_local.csr -text -noout
```
```
./win_oldsign win3_ad_lab_local.csr
openssl x509 -in win3_ad_lab_local.crt -text -noout
```
```
./win_mkcsr win4.ad.lab.local win.ad.lab.local 127.0.0.1 ::1
openssl req -in win4_ad_lab_local.csr -text -noout
```
```
./win_signcsr win4_ad_lab_local.csr
openssl x509 -in win4_ad_lab_local.crt -text -noout
```
```
./win_mkpfx win4_ad_lab_local.crt
openssl pkcs12 -in win4_ad_lab_local.pfx -info -password pass:changeit -nodes
```

