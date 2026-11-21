# rpbasiledev.com — Cutover e bonifica

**Ultimo aggiornamento verificato: 21 novembre 2026.**

## Stato

| Componente | Esito |
|---|---|
| `https://www.rpbasiledev.com` | **LIVE**, portfolio nuovo su Render, HTTPS certificato |
| `https://rpbasiledev-temporary.onrender.com` | LIVE, origin statico di fallback |
| Modulo contatti | FormSubmit attivato e testato su indirizzo Render; return URL aggiornato a WWW |
| `/robots.txt` e `/sitemap.xml` su WWW | **LIVE, verificati in browser** |
| Canonical + meta robots | WWW canonical, index/follow |
| `rpbasiledev.com` (senza www) | Ancora A vecchio `131.226.5.6`; Render attende verifica |
| Mail del dominio | Ancora MX `0 rpbasiledev.com`, legato ad A @ legacy |
| Registrazione presso THCServers | ACTIVE; hosting Geek Web Hosting sospeso |
| Old WordPress files | **NON migrati**; nessun file sospetto inserito nel nuovo sito |

## DNS pubblico verificato il 21/11/2026

- NS: `ns1.thcservers.com` ... `ns4.thcservers.com`
- A `@` -> `131.226.5.6`
- CNAME `www` -> `rpbasiledev-temporary.onrender.com` (ticket registrar #227944)
- MX `@` -> `0 rpbasiledev.com`
- CNAME `mail` -> `rpbasiledev.com`
- TXT SPF `v=spf1 +a +mx +ip4:131.226.5.3 include:cpanel2-us.thcservers.com ~all`
- TXT `_dmarc`: `v=DMARC1; p=none;`
- TXT `default._domainkey`: chiave pubblica DKIM sul vecchio provider (valore verificato, omesso da questo riepilogo)
- TXT `@`: `MS=E7EDB373F9FA9E2FC5A32E39A88AE2EA4F3C5381`
- Altri sottodomini usati dal vecchio provider: `cpanel`, `webmail`, `webdisk`, `ftp`, `autoconfig`, `autodiscover` e record SRV di autodiscover.

## Richiesta al registrar in corso

Il 21 novembre 2026 è stato aggiornato il ticket **THCServers #227944** con richiesta di cutover **in due fasi**:

1. Chiedere conferma se la posta `@rpbasiledev.com` funziona ancora malgrado la sospensione del cPanel.
2. **Solo se funziona**, trasformare `mail.rpbasiledev.com` da CNAME ad A `131.226.5.6`, e MX @ da `0 rpbasiledev.com` a `0 mail.rpbasiledev.com`; **non** cambiare ancora A @. Testare ricezione/inoltro.
3. Solo dopo il gate email, richiedere in un **intervento separato** A @ -> `216.24.57.1` (IP pubblicato da Render), attivare il redirect apex -> www e verificare TLS.
4. Se la posta è già interrotta dalla sospensione, chiedere supporto a mail routing indipendente e **non procedere** con l'A @ finché un MX indipendente è pronto.

Questo ponte DNS separerebbe l'IP di mail da quello del web, **ma lascerebbe le caselle sul vecchio server**. Alternativa più robusta: migrare la posta in ricezione su Cloudflare Email Routing gratuito, con destinazione Gmail verificata e zona DNS completa, oppure su un servizio mailbox indipendente se serve anche SMTP in uscita. Eventuali modifiche nameserver richiedono preflight record-per-record.

### Gate di sicurezza

**NON cambiare A @ né nameserver ora.** Esistono messaggi in Gmail destinati a `admin@rpbasiledev.com`, che è stato usato per servizi di sviluppo: perdere l'email del dominio potrebbe compromettere account esterni. Il record MX attuale dipende da A @. Non si presume che il servizio mail legacy sia funzionante sotto sospensione.

## Bonifica e sicurezza

- Hosting nuovo statico HTML/CSS/JS, senza database, WordPress, plugin o runtime PHP.
- Nessuna migrazione di script, binari o backup del server sospeso.
- Conservare ticket, log e prove di incidente; ruotare eventuali credenziali riutilizzate e abilitare MFA su GitHub/Render/registrar.
- Non pubblicare in repository informazioni personali e segreti non necessari.
- Form di contatto: servizio di inoltro esterno; non è una mailbox `@rpbasiledev.com`.

## Link operativi

- Sito: https://www.rpbasiledev.com/
- Render: https://dashboard.render.com/static/srv-dc0b32g8v5cc7394tptg
- Issue di cutover: https://github.com/ruggeropaolobasile/rpbasiledev-temporary/issues/1

## Rollback

WWW CNAME precedente: `rpbasiledev.com`. La strategia deve evitare il ritorno al sito sospeso; in caso di regressione preferire la pagina Render fallback. Prima di qualsiasi mutazione di @, salvare i record correnti e predisporre rollback entro il TTL.
