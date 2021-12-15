# ZEK - úprava skriptů a nasazení CMP

Cílem úprav je navázání vykonávání skriptů, které ukládají cookies do prohlížeče, na základě nastavení souhlasů uživatelem. 

## Seznam úprav
1. Nasazení CMP lišty do hlavičky
2. Nasazení GTM  / úprava inicializace
3. Přesun skriptů do GTM - GA, Facebook pixel, Hotjar apod.
4. Nastavení pravidel spouštění podle souhlasů v GTM
5. Přidání proměnné pro Gemius skript, který nastaví aby se řídil podle CMP
6. Nasazení reklamní knihovny loader.js
7. Přidání odkazu na zobrzení změny nastavení cookie do patičky

### 1. Nasazení CMP lišty do hlavičky
```sh
<head>
<script src="https://static.primacdn.cz/scripts/cmp/iprima-cmp.min.js?v2"></script>
</head>
```

### 2. Nasazení GTM
```sh
<head>
<!-- Google Tag Manager -->
<script>
    function addGTM(w, d, s, l, i) {
        w[l] = w[l] || [];
        w[l].push({'gtm.start': new Date().getTime(), event: 'gtm.js'});
        var f = d.getElementsByTagName(s)[0],
            j = d.createElement(s),
            dl = l != 'dataLayer' ? '&l=' + l : '';
        j.async = true;
        j.src = 'https://www.googletagmanager.com/gtm.js?id=' + i + dl;
        f.parentNode.insertBefore(j, f);
    }
    window.didomiOnReady = window.didomiOnReady || [];
    window.didomiOnReady.push(function () {
        addGTM(window, document, 'script', 'dataLayer', 'GTM-XXXXXXX');
    });
</script>
<!-- End Google Tag Manager -->
</head>
```
*Nahradit dle příslušného webu: GTM-XXXXXXX*

### 3. Přesun skriptů do GTM - GA, Facebook pixel, Hotjar apod.
Dodejte seznam skriptů, které na stránce používáte a bude nutné přesunout do GTM.

### 4. Nastavení pravidel spouštění podle souhlasů v GTM
Pravidla budou nastavene po vzájemné konzultaci.

### 5. Přidání proměnné pro Gemius skript, který nastaví aby se řídil podle CMP
Gemius skript má vlastní detekci a není jej nutné přesouvat do GTM. Aktivace řízení podle CMP se provede, přídáním proměnné -  pp_gemius_use_cmp = true
```sh
var pp_gemius_identifier = 'XXXXXXXXXXXXXXXXXXXXXXX';
var pp_gemius_use_cmp = true;
```

### 6. Nasazení reklamní knihovny loader.js
Inicializace reklamního systému je přes knihovnu loader.js, která je umístěna na konci kódu. Jednotlivé skripty se dále načítají jako async a defer.
```sh
<script async  src="https://static.primacdn.cz/sas/[ WEBSITE ]/prod/loader.js"></script>
```

#####  Autosalon
---
| Doména | WEBSITE |
| ------ | ------ |
| autosalon.tv | autosalon |

#####  Prima Doma
---
| Doména | WEBSITE |
| ------ | ------ |
| ceskykutil.cz | ceskykutil |
| fachmani.cz | fachmani |
| primadoma.cz | primadomacz |
| primadoma.tv | primadomatv |
| primanapady.cz | primanapady |
| primarady.cz | primarady |
| zahradkarskaporadna.cz | zahradkarskaporadna |

##### Radia
---
| Doména | WEBSITE |
| ------ | ------ |
| countryradio.cz | country |
| kiss.cz | kiss |
| radio1.cz | radio1 |
| radiobeat.cz | beat |
| radiospin.cz | spin |
| signalradio.cz | signal |

##### Zastupované
---
| Doména | WEBSITE |
| ------ | ------ |
| mzone.cz | mzone |
| nakluky.cz | nakluky |
| playzone.cz | playzone |

##### iPrima
---
| Doména | WEBSITE |
| ------ | ------ |
| cnn.iprima.cz | cnn_prima_news |
| cool.iprima.cz | cool |
| fresh.iprima.cz | fresh |
| iprima.cz | iprima |
| krimi.iprima.cz | krimi |
| lajk.iprima.cz | lajk |
| living.iprima.cz | living |
| love.iprima.cz | love |
| max.iprima.cz | max |
| pauza.iprima.cz | pauza |
| playboy.cz | playboy |
| show.iprima.cz | show |
| star.iprima.cz | star |
| style.iprima.cz | style |
| zeny.iprima.cz | zeny |
| zoom.iprima.cz | zoom |


### 7. Přidání odkazu na zobrzení změny nastavení cookies
Doporučené umístění je v patičce webu.

```sh
<a href="javascript:void(0)" onclick="try{Didomi.notice.show();}catch (e) {}">Zobrazit CMP</a>
```
