# Configuració de l'enviament d'email i cua amb Mautic 5

La motivació d'aquest document és tenir una guia pròpia sobre els canvis que hi ha hagut en l'enviament d'emails respecte les versions de Mautic de la 4 a la 5.

La antiga comanda bin/console mautic:emails:send ja no funciona.

Aquesta configuració està basada en la informació obtinguda al forum de mautic, en [aquest post](https://forum.mautic.org/t/changing-how-mautic-5-handles-email-from-send-immediately-to-queue/30374/20) i en la [documentació de mautic 5](https://docs.mautic.org/en/5.x/configuration/settings.html#mautic-doesn-t-use-queues-by-default) sobre la configuració de l'enviament d'email.

Mautic 5 no fa servir cues d'email per defecte, per tant, s'ha de canviar la configuració.

Segons la documentació, a la configuració de Mautic, apartat **Queue Settings**, s'han de canviar una sèrie de paràmetres. 

A la part Queue for email (SMS and push messages), s'ha de posar:

- Scheme: **doctrine**
- Host (Servidor): **default**

i fer clic a desar.

![Imatge configuració](./imatges/mautic/config_cua_01.png)

Al fer clic a test ho dona com a *èxit*, però no s'envia cap email.

Després d'això, s'ha d'emprar una comanda de symfony (segons documentació de Mautic) que és qui s'encarrega d'enviar l'email. La comanda s'anomena **consume** [documentació de symfony](https://symfony.com/doc/current/messenger.html#consuming-messages-running-the-worker)

`php bin/console messenger:consume email -vv`

La opció -vv serveix per veure detalls del que està passant

## Limitar el número d'emails a enviar

La comanda *messenger:consume* és el procés i *email* és la cua. messenger-consum espera els emails de "emails" i els va enviant. És una comanda que no atura el procés a no ser que li especifiquem quan ha d'acabar.

Les opcions que ens poden interessar són:

- --memory-limit=1280M (El procés acabarà quan hagi consumit 1280Mb de memòria.)
- --time-limit=30 (El procés acabarà d'enviar emails desprès de 30 segons)
- --limit=10 (El procès acabarà quan hagi enviat 10 emails)

Les opcions es poden combinar --time-limit=30 --limit=10 (el procès acabarà quan hagin passat 30 segons o hagi enviat 10 emails)

Per no ser considerat spammer, els servidors solen tenir uns límits diaris.

- Gmail compte gratuit: 300-500 emails al dia.
- Google Workspace: 2000 emails al dia.
- Contabo: no hi ha límit

Les recomanacions per ser respectuosos és no enviar més de 500 emails diaris. El volum per hora ha de ser entre 20 i 50 emails.

En el cas de fer servir servidors com Amazon SES per enviar gran quantitat d'emails aquests números canvien.

La nostra programació serà enviar emails entre les 6:00 del matí (hora Madrid/Europa) i les 20:00 de la tarda. En total 14 hores. Si volem enviar 500 emails són 35,71 per hora. Per fer números rodons farem 30 emails per hora, el que correspon a 1 email cada 2 minuts.

Per tant, crearem una tasca cron que executi el procés cada 2 minuts i sols envïi un email, --limit=1

`*/2 * * * * php [ruta mautic] bin/console messenger:consume email --limit=1` 

## Emails de Campanyes

`mautic:broadcasts:send`

php -d memory_limit=-1 ./bin/console mautic:broadcasts:send --batch=30 --limit=2000


sudo -u web5 php bin/console messenger:consume email --time-limit=30 --limit=1 -vv

--time-limit=360 arg or/and --limit=10


php bin/console messenger:stats