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
<script src="https://static.primacdn.cz/sas/cmp/iprima-cmp.js?v4"></script>
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


### 7. Přidání odkazu na zobrazení změny nastavení cookies
Doporučené umístění je v patičce webu.

```sh
<a href="javascript:void(0)" onclick="try{Didomi.notice.show();}catch (e) {}">Zobrazit CMP</a>
```

### Didomi API - čtení účelů
Alternativní možnost řízení spouštní skriptů bez GTM a to přímo na základě signálů z CMP.

#### API
```sh
https://developers.didomi.io/cmp/web-sdk/reference/api
```

#### Účely

| Účel | Hodnota | Systém |
| ------ | ------ | ------ |
| Ukládání a/nebo přístup k informacím v zařízení | cookies | všechny |
| Základní nastavení reklamy  | select_basic_ads | reklamní systémy |
| Výběr personalizovaného obsahu | select_personalized_ads | doporučovácí systémy - Recombee |
| Vytvoření profilu pro personalizovanou reklamu | create_ads_profile | Facebook a další sociální sítě |
| Vytvoření profilu pro personalizovaný obsah | create_content_profile | Recombee |
| Výběr personalizovaného obsahu | select_personalized_content | Recombee |
| Měření výkonu reklamy | measure_ad_performance | reklamní systémy |
| Měření výkonu obsahu | measure_content_performance | Google Analytics |
| Používání výzkumu trhu pro získání poznatků o uživatelích | market_research | offline data |
| Vývoj a zlepšování produktů | improve_products | Hotjar |

#### Příklad získání účelu
```sh
<script type="text/javascript">
window.didomiOnReady = window.didomiOnReady || [];
window.didomiOnReady.push(function (Didomi) { 
    if(Didomi.getUserConsentStatusForPurpose('cookies')){

        // call script

    }
});
</script>
```

#### Příklad navázání na změnu nastavení
```sh
<script type="text/javascript">
window.didomiEventListeners = window.didomiEventListeners || [];
window.didomiEventListeners.push({
  event: 'consent.changed',
  listener: function (context) {
    // The user consent status has changed
  }
});
</script>
```



#### Příklad volání pro první návštěvu a následně po již získaném souhlasu

###### Skript se ve funkci isConsent vykoná pouze 1x i při opakovaném volání, aby nedocházelo ke zdvojení volání při změně nastavení volby v CMP

###### Souslednost kroků je následující:
 - při první návštěvě nemáme od uživatele souhlas a proto je nastaven event na consent.changed, aby po vyjádření mohl být provedek podle souhlasů skript a tak jsme nepřišli o jeho první návštěvu.
- při další návštěvě se již podle uděleného souhlasu volá skript 


```sh
<script type="text/javascript">

let isConsent = (function (){
    let callOnlyOne = false;
    return function(){
        if(!callOnlyOne){

            callOnlyOne = true;
            console.log('%c [ mam consent ] ','background:green;color:white;');

        }
    }
})();

window.didomiOnReady = window.didomiOnReady || [];
window.didomiOnReady.push(function (Didomi) { 
    if(Didomi.getUserConsentStatusForPurpose('cookies')){
        
        isConsent();

    }else{

        window.didomiEventListeners = window.didomiEventListeners || [];
        window.didomiEventListeners.push({
        event: 'consent.changed',
        listener: function (context) {
            if(Didomi.getUserConsentStatusForPurpose('cookies')){

                isConsent();

            }
        }
        });

    }
});

</script>
```

#### Příklad Hotjar
```sh
<script type="text/javascript">

let consentHotjar = (function (){
    let callOnlyOne = false;
    return function(){
        if(!callOnlyOne){

            callOnlyOne = true;
            
                (function(h,o,t,j,a,r){
                    h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
                    h._hjSettings={hjid: [ XXXXX ] ,hjsv:6};
                    a=o.getElementsByTagName('head')[0];
                    r=o.createElement('script');r.async=1;
                    r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
                    a.appendChild(r);
                })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');

        }
    }
})();

window.didomiOnReady = window.didomiOnReady || [];
window.didomiOnReady.push(function (Didomi) { 
    if(Didomi.getUserConsentStatusForPurpose('cookies') && Didomi.getUserConsentStatusForPurpose('improve_products')){
        
        consentHotjar();

    }else{

        window.didomiEventListeners = window.didomiEventListeners || [];
        window.didomiEventListeners.push({
        event: 'consent.changed',
        listener: function (context) {
            if(Didomi.getUserConsentStatusForPurpose('cookies') && Didomi.getUserConsentStatusForPurpose('improve_products')){

                consentHotjar();

            }
        }
        });

    }
});

</script>
```

#### Příklad Twitter
```sh
<script type="text/javascript">

let consentTwitter = (function (){
    let callOnlyOne = false;
    return function(){
        if(!callOnlyOne){

            callOnlyOne = true;
            
            window.twttr = (function(d, s, id) {
                var js, fjs = d.getElementsByTagName(s)[0],
                    t = window.twttr || {};
                if (d.getElementById(id)) return t;
                js = d.createElement(s);
                js.id = id;
                js.src = "https://platform.twitter.com/widgets.js";
                fjs.parentNode.insertBefore(js, fjs);

                t._e = [];
                t.ready = function(f) {
                    t._e.push(f);
                };

                return t;
            }(document, "script", "twitter-wjs"));

        }
    }
})();

window.didomiOnReady = window.didomiOnReady || [];
window.didomiOnReady.push(function (Didomi) { 
    if(Didomi.getUserConsentStatusForPurpose('cookies') && Didomi.getUserConsentStatusForPurpose('create_ads_profile')){
        
        consentTwitter();

    }else{

        window.didomiEventListeners = window.didomiEventListeners || [];
        window.didomiEventListeners.push({
        event: 'consent.changed',
        listener: function (context) {
            if(Didomi.getUserConsentStatusForPurpose('cookies') && Didomi.getUserConsentStatusForPurpose('create_ads_profile')){

                consentTwitter();

            }
        }
        });

    }
});

</script>
```


#### Příklad Google Analytics
```sh
<script type="text/javascript">

let consentGA = (function (){
    let callOnlyOne = false;
    return function(){
        if(!callOnlyOne){

            callOnlyOne = true;
            
            var _gaq = _gaq || [];
            _gaq.push(['_setAccount', 'UA-XXXXXXXXX']);
            _gaq.push(['_trackPageview']);

            (function() {
                var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
                ga.src = ('https:' === document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
                var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
            })();

        }
    }
})();

window.didomiOnReady = window.didomiOnReady || [];
window.didomiOnReady.push(function (Didomi) { 
    if(Didomi.getUserConsentStatusForPurpose('cookies') && Didomi.getUserConsentStatusForPurpose('measure_content_performance')){
        
        consentGA();

    }else{

        window.didomiEventListeners = window.didomiEventListeners || [];
        window.didomiEventListeners.push({
        event: 'consent.changed',
        listener: function (context) {
            if(Didomi.getUserConsentStatusForPurpose('cookies') && Didomi.getUserConsentStatusForPurpose('measure_content_performance')){

                consentGA();

            }
        }
        });

    }
});

</script>
```

### 8. Nastavení volání skriptů v GTM podle slouhlasů

#### 1. Vytvoření proměnné v GTM

- Variable Type => Data Layer Variable => DidomiCookiesConsent

![](https://github.com/primasas/zek/blob/main/images/gtm-didomi-variable.png)


#### 2. Vytvoření události Didomi

- Triger Type => Costume Event => Event name => didomi-cookies-consent 
- Vytvoření souhlasu podle typu skriptu, který potřebuje

![](https://github.com/primasas/zek/blob/main/images/gtm-didomi-trigger.png)


#### 3. Přidání Triggeru do tagu

![](https://github.com/primasas/zek/blob/main/images/gtm-didomi-tag-triggering.png)

