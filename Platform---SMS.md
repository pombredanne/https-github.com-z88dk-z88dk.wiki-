
![](images/platform/sms.jpg)

## Quick Stars

    zcc +sms -create-app -o program.bin program.c

    zcc +sms -create-app -lgamegear -o program.bin program.c


'program.sms' will be built


## Emulators

MEDNAFEN (https://mednafen.github.io/)

    mednafen -force_module sms program.sms
    mednafen -force_module gg  program.sms

MAME/MESS

    mame sms -cart program.sms
    mame gamegear -cart program.sms


## Links

["stevepro studios" - z88dk Programming related articles](http://steveproxna.blogspot.it/search/label/z88dk)

[King Kong game](http://hirudov.com/sega/KingKongSMS.php)

[Thread about using sprites on SMS and z88dk](http://www.mastersystem-france.com/t1686p30-programmation-master-system-en-assembleur-variante-en-c) (French)

[Haroldo-OK website, some game for the SMS is written with z88dk](http://www.haroldo-ok.com/)

[SMSBomberman preview (YouTube video)](https://www.youtube.com/watch?v=akYolXhhL1Q)

