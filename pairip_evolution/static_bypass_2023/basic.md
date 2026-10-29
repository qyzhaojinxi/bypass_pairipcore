## For removing integrity
Remove this call
Search:
(invoke-static \{p1\}, Lcom/pairip/SignatureCheck;->verifyIntegrity\(Landroid/content/Context;\)V)
Replace:
#$1

Delete theses two classes
1. Lcom/pairip/SignatureCheck;
2. Lcom/pairip/SignatureCheck$SignatureTamperedException;

## For removing license checks
#Note: If V3 License Not found then skip this.
Remove this call
Search:
(invoke-static \{p0\}, Lcom/pairip/licensecheck3/LicenseClientV3;->onActivityCreate\(Landroid/app/Activity;\)V)
Replace:
#$1

Delete all Classes of
com/pairip/licensecheck3/

## For CRC bypass
Patch libpairipcore.so
Or restore old CRC using Apkanalyzer

( I suggest use apkanalyzer its easy for restoring old CRC select original apk with original signature ' using Convert apks to apk without sign so the real signature doesn't changed ' )

Note: We can remove all classes from
com/pairip/ except VMRunner.

What is CRC ? and how to Patch libpairipcore.so bro?

algorithm is used to verify the integrity of the transaction set ( Dex )

People use radare2 to patch lib but if you don't have knowledge about that you can use apkanalyzer with CRC METHOD
In this we copy Crc from Original application and paste into moded one to bypass the security of Pairip.