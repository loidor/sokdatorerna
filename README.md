# Sunne biblioteks sökdatorer

Bibliotekets sökdatorer är nedlåsta för att enbart visa en hemsida och minska möjligheten i största mån att kunna göra något annat. Det görs genom följande förändringar:

## Registerändringar

Logga in som användaren som ska ändras. Starta ```regedit.exe``` och gå till ```HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon```. Här ska följande nycklar ändras eller läggas till om de saknas.

* ```DefaultDomainName``` (Strängvärde). Skriv in domännamnet på datorn det gäller.
* ```DefaultUserName``` (Strängvärde). Skriv in användarnamnet på användaren som automatiskt ska loggas in.
* ```DefaultPassword``` (Strängvärde). Skriv in lösenordet på användaren som automatiskt ska loggas in.
* ```AutoAdminLogon``` (Strängvärde). Sätt till ```1```.

Nästa registernyckel behöver ändras för den specifika användaren som ska loggas in. Öppna ```powershell.exe``` och importera användarens registernycklar till regedit.
```reg load HKU\AnnansRegister C:\Users\[Användarnamn]\ntuser.dat```

Nu ska användarens nycklar finnas under ```HKEY_USERS``` i regedit.

Leta reda på nyckeln ```SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System``` och ändra eller lägg till nyckeln ```DisableLockWorkstation``` (DWORD 32-bit) innehållandes ```0```.

Nu kommer sagda användare loggas in automatiskt och loggas inte ut av inaktivitet.

Avslutningsvis ska windows skrivbordsmiljö ersättas av en egenskriven, som låser alla knappkombinationer som kan öppna saker, lägger till några nya knappkombinationer för administration samt automatiskt öppnar hemsidan som ska visas.

Även den nyckeln finns under ```HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon```. Öppna nyckeln Shell och skriv in sökvägen till scriptet, exempelvis ```C:\startscript\autohotkeyshell.exe```. Om sökvägen innehåller mellanslag behöver den omges av "".

## Autohotkey-script

Scriptet finns för nedladdning under filer, i form av en exe-fil. Därmed behöver inget extra installeras.

```
#MaxHotKeysPerInterval 500
Run "C:\Program Files\Internet Explorer\iexplore.exe" -k https://www.bibliotekvarmland.se
RButton::Return
MButton::Return
^MButton::Return
!MButton::Return
+MButton::Return
^RButton::Return
!RButton::Return
+RButton::Return
^LButton::Return
!LButton::Return
+LButton::Return
^J::Return
^H::Return
^W::Return
^G::Return
^S::Return
^B::Return
^O::Return
^I::Return
+F10::Return
F1::Return
^+Delete::Return
^!Delete::Return
^NumpadAdd:Return
^NumpadSub::Return
^=::Return
^-::Return
^WheelUp::Return
^WheelDown::Return
RButton & F1::Run cmd
RButton & F2::Run "C:\kioskprogram\procexp.exe"
RButton & F3::Run cmd /C "shutdown -r -t 0"
^n::Return
!F4::Return
```

Inledningsvis startas internet explorer i kiosk-läge, så inga menyer syns. Därefter anges URLen till den sida som ska öppnas. Ändra den till den sida som ska öppnas.

Alla knappkombinationer som kan öppna saker är här inaktiverade.

Sedan är tre nya kombinationer tillagda. Höger musknapp plus F1 startar ```cmd```. Höger musknapp plus F2 startar procexp.exe (finns för nedladdning på https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer), vilket är en mer avancerad aktivitetshanterare. Höger musknapp plus F3 startar om datorn.
