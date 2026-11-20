# rpbasiledev.com — Cutover e bonifica (20 novembre 2026)

## Stato verificato

- Nuovo portfolio statico: `https://rpbasiledev-temporary.onrender.com/` — Render, deployment Live.
- Repository sorgente: `ruggeropaolobasile/rpbasiledev-temporary`.
- Modulo contatti: FormSubmit, attivazione completata e test email ricevuto sulla Gmail personale il 20 novembre 2026.
- Dominio `rpbasiledev.com`: registrazione Active nel pannello THCServers; hosting Geek Web Hosting Suspended.
- Nessun file, database, script o backup del servizio THCServers è stato trasferito nel nuovo sito.
- Nessun cambio DNS è stato ancora effettuato.

## Inventario DNS pubblico alla verifica del 20/11/2026

- A `@` -> `131.226.5.6`
- CNAME `www` -> `rpbasiledev.com`
- MX `@` -> `0 rpbasiledev.com`
- TXT SPF `v=spf1 +a +mx +ip4:131.226.5.3 include:cpanel2-us.thcservers.com ~all`
- NS: `ns1.thcservers.com` ... `ns4.thcservers.com`

**Attenzione:** l'host MX coincide con il dominio principale. Cambiare A di `@` mentre MX punta a `@` rischia di rompere la ricezione della posta. Non trasferire i nameserver senza un inventario completo DNS di mail, TXT, DKIM, DMARC e altri sottodomini.

## Cutover sicuro

1. Collegare `www.rpbasiledev.com` come custom domain al servizio statico Render `srv-dc0b32g8v5cc7394tptg`. Attendere conferma ownership/verifica e predisposizione TLS.
2. Sostituire soltanto il CNAME `www` su THCServers con `rpbasiledev-temporary.onrender.com` (o il target specifico mostrato da Render).
3. Verificare dal web `https://www.rpbasiledev.com`: HTTPS valido, nessuna pagina Account Suspended, modulo, privacy e navigazione responsive.
4. Inventariare caselle email esistenti e relativi dati, preparare servizio email indipendente. Migrare MX, SPF, DKIM e DMARC con piano di rollback e test invio/ricezione.
5. Solo dopo la separazione della posta, puntare l'apex `rpbasiledev.com` a Render seguendo **esattamente** i record supportati e confermati dal servizio.
6. A cutover verificato, rimuovere `noindex` e `Disallow: /` dal sito e abilitare SEO.

## Bonifica e sicurezza

- Non trasferire binari o script dal vecchio server sospeso.
- Conservare ticket, email e prove di incidente; non cancellare o alterare le evidenze.
- Ruotare tutte le credenziali del vecchio hosting riutilizzate altrove e abilitare MFA sugli account GitHub, Render e registrar.
- Se THCServers concede export, estrarre soltanto dati legittimi, con scansione e validazione prima di un eventuale recupero.
- Nuovo sito: HTML/CSS/JS statici, senza WordPress, senza database pubblico, senza librerie esterne; il modulo usa un servizio di inoltro esterno.
- Non esporre dump CV, dati sanitari, numero telefonico privato, token di servizi o credenziali nel repository pubblico.

## Rollback

Finché si modifica solo il CNAME `www`, il rollback è la precedente destinazione `rpbasiledev.com`. Evitare di modificare i nameserver globali fino alla chiusura delle verifiche email.
