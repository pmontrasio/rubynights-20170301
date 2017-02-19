Licensed under the Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0) https://creativecommons.org/licenses/by-sa/4.0/


# Introduzione

https://www.theguardian.com/world/2017/jan/10/xiaolu-guo-why-i-moved-from-beijing-to-london

my grammar book said: “Peter had been painting his house for weeks, but he finally gave up.”
[...]  the word order “had been” and the added flourishes like “ing” [...] were bizarre decorations that did nothing but obscure a simple, strong building. My instinct was to say something like: “Peter tries to paint his house, but sadness overwhelms him, causing him to lay down his brushes and give up his dream.

https://www.theguardian.com/books/2016/oct/13/my-writing-day-xiaolu-guo
Xiaolu Guo: ‘One language is not enough – I write in both Chinese and English’

http://syndication.theguardian.com/open-licence-terms/
100 parole
 “Courtesy of Guardian News & Media Ltd”.

L'aneddoto di Asimov ed il critico letterario.

http://wiki.c2.com/?AsimovAndTheCritic
Speaker: Sir, I am an English professor who has been teaching literature for years. What makes you think you know more about it than I?
Asimov: Because I, sir, am the story's author.
Speaker: So? Just because you wrote it, what makes you think you know anything about it?

La lingua inglese del primo esempio è un linguaggio a cui vi state avvicinando ed il cinese è uno che avete usato da anni e che conoscete alla perfezione.
L'autore del secondo esempio è il designer del linguaggio ed il critico è chi sviluppa software da anni usando quello ed altri linguaggi.

Credo che abbiamo fatto tutti l'esperienza di dire qualcosa aspettandoci che venga capita in un certo modo e invece viene capito tutt'altro.  Chi crea i linguaggi si imbatte nello stesso problema. Fa scelte finalizzate ad uno scopo, ma:

* ci sono conseguenze inattese?
* l'obiettivo è raggiunto?

Per tacere del fatto che se anche lo fosse non è detto che sia un obiettivo ragionevole.

Dopo trent'anni di sviluppo di software ormai mi sono fatto un'idea di cosa funzioni meglio e di cosa funzioni peggio.

# Età

Ruby. Design started on February 24, 1993 first release on December 21, 1995.
https://en.wikipedia.org/wiki/Ruby_(programming_language)

Python. Implementation started in December 1989. First release on February 1991.
https://en.wikipedia.org/wiki/History_of_Python


# Molte opinioni vs poche opinioni

Django è poco opinionato. Poichè mi sono abituato da 10 anni a Rails non è una sorpresa che lo trovi un difetto.

Ogni progetto avrà le sue convenzioni e le sue eccezioni. Questo porta via tempo e costa soldi al cliente che paga a giornate. Oppure riduce il profitto dello sviluppatore che fa offerte a corpo, a meno che non chieda più soldi al cliente. Ma questo lo mette a rischio di perdere le offerte contro chi propone progetti Rails.

## Il routing

Uno dei pregi di Rails basta guardare una URL per risalire rapidamente al file controller e al metodo che la implementa, e alla view che la presenta. Con Django e Web2Py bisogna per forza guardare il file delle rotte, che può essere spezzato in più file sparsi per le directory dell'applicazione. Si perde tempo. Si può installare https://github.com/django-extensions/django-extensions ma ci si chiede perché una funzionalità così importante non sia già nel core del progetto, soprattutto quando è così scomodo farlo a mano. Tutto sommato per via delle convenzioni di ```rake routes``` quasi si può fare a meno.

Django può avere file ```urls.py``` sparsi per tutto il progetto, che è molto strano a chi è abituato ad avere tutto in ```config/routes.py```.

Ad onor del vero Rails può avere ```routes.rb``` dentro alle gemme (e pure controller, modelli, migrazioni, view), il concetto se si vuole è simile. Ma l'applicazione principale ha un solo file di rotte. Pure la sintassi è più umana.

Rails già include http://www.django-rest-framework.org/api-guide/routers/ e con una sintassi più compatta grazie al poter passare blocchi di codice ai metodi e all'importare tutto.

```
from rest_framework import routers

router = routers.SimpleRouter()
router.register(r'users', UserViewSet)
urlpatterns = router.urls
```

vs

```
Rails.application.routes.draw do
  resources :users
end
```

Il passare o meno blocchi di codice come argomenti è forse la differenza principale tra i due linguaggi, che impatta maggiormente sulle possibilità espressive. È quello che rende Ruby ottimo per i DSL.

Il ruouting non restful è circa uguale. Ci sono pro e contro ma in Django occhio alle parentesi nelle regexp. Rails sposta la complessità nei constraints facilitando la lettura dell'url.

Django

```
url(r'info/detail/(?P<id>[0-9]+)$', 'console.views.detail', name='detail')
<a href="{% url 'console.views.detail' id %}" target="_blank" rel="noopener noreferrer">{{ label }}</a>
```

vs Rails

```
get 'info/detail/:id', to: "controller.detail", as: :detail, constraints: { id: /[0-9]+/ }
<%= link_to label, detail_path(id), target:"_blank", rel:"noopener noreferrer" %>
```

oppure per chi vuole fare tutto a mano

```
<a href="<%= detail_path(id) %>" target="_blank" rel="noopener noreferrer"><%= label %></a>
```

A proposito: Target=”_blank” — the most underestimated vulnerability ever
https://medium.com/@jitbit/target-blank-the-most-underestimated-vulnerability-ever-96e328301f4c

Immagine per il Phishing: Kenneth Lu
https://www.flickr.com/photos/toasty/1276202472
https://creativecommons.org/licenses/by/2.0/
Scaricata in ```/home/montra/Downloads/1276202472_ce7e194cf2_o.jpg```

Altro caso in cui la convenzione facilita lo sviluppatore:

* vedi una stringa in una pagina HTML nel browser
* la cerchi nel codice Django
* la trovi in ```templates/console/editproject_everything.html```

Dove sarà la action corrispondente?

è in ```console/views.py```, insieme a tante altre (1000+ righe nel progetto che ho ereditato), dentro a ```def edit(...)```

In Rails al file ```app/views/console/editproject_everything.html.erb``` avrebbe corrisposto ```app/controllers/console_controller.rb```, ```def editproject_everything```. Più facile.

In realtà Rails ti avrebbe invitato a progettare un controller ```console``` con azioni restful e quindi ci sarebbe stato un altro controller ```projects``` con dentro una ```def edit_everything``` o meglio ancora una semplice ```def edit```. Il controller sarebbe stato generato dallo scaffolder e riempito di codice dallo sviluppatore. Più ordinato. Non è una coincidenza che non abbia mai visto progetto Rails con tutte le action in un unico controller.

Gli amici Pythonisti mi danno l'ottima notizia che ora esistono le class based views:

* https://docs.djangoproject.com/en/1.10/topics/class-based-views/
* https://github.com/brack3t/django-braces

oltre che http://www.django-rest-framework.org/

per cui questi problemi potrebbero essere già scomparsi. Una costante degli ultimi 10 anni è la cross-contamination di tutti i framework web.

## MVC vs MVT

Django dice di essere un framework MVT mentre gli altri due framework dicono di essere MVC. Qualcuno dice che Rails non è MVC
https://www.quora.com/Is-Ruby-on-Rails-a-truly-MVC-framework

> One of the key characteristics of the MVC pattern is that the Observer pattern is used for the model to communicate state changes directly to the view which Ruby on Rails doesn't do

Mi sembra più una questione di nomenclatura che di sostanza. Alla fine il codice in ```views.py``` di Django fa esattamente quel che fa il codice dei controller di Rails e i template Django sono esattamente uguali alle viste Rails, a parte il linguaggio di templating più limitato.

A proposito, anche per Ruby c'è un linguaggio di templating con le restrizioni imposte da Jinja. Si tratta di Liquid di Shopify http://shopify.github.io/liquid/ che per evidenti ragioni non vuole che i suoi clienti possano far girare potenzialmente di tutto sui propri server.

## Admin

Django ha un admin built in, Rails no. Web2py ce l'ha almeno per la gestione degli errori.

Con Rails bisogna usare gemme tipo http://activeadmin.info/ che nella pratica si usa poco. Si possono creare in fretta applicazioni Rails usando solo ActiveAdmin, ma appen chiede anche solo una piccola cosa in più si impazzisce nel cercar di piegare i default del sistema e viene voglia di rifare tutto in Rails "standard".

Anche senza usare ActiveAdmin, Rails ha uno scaffolding command line molto pratico http://guides.rubyonrails.org/command_line.html#rails-generate
Lo si usa per creare modelli, controller e viste consistenti tra di loro e dà un CRUD con cui iniziare. Tipicamente poi si aggiungono metodi ai modelli, si modifica un po' il controller e si butta la vista (il template Python) rimpiazzandola con il codice in arrivo dai designer.

L'admin di Django non fa un mero scaffolding, ma mette a disposizione una serie di strumenti "magici", perché nell'admin di magia ce n'è molta.
L'admin ha già pronto un sistema di ricerca con filtri e con poche righe di codice si riesce a personalizzare parecchio, compresa la granularità di accesso con permessi e ruoli. Ancora, si possono suddividere i dati mostrati in serie, ad esempio anno, mese, giorno.

Grazie alla semplicità nel costruire form, model ecc, molti suggeriscono di partire senza admin e di utilizzarlo solamente lato sviluppo, per aver un controllo backend sui dati e costruire l'applicazione senza legami ad una app di terze parti.

## ORM

Negli ORM Python si possono creare le tabelle con qualsiasi naming convention. In Rails si devono modificare i default e lo si fa per lavorare su db preesistenti. La convenzione è diversa per ogni progetto e va imparata. Se lo sviluppatore originale non è stato superumano nella disciplina, vanno imparate e ricordate anche le eccezioni.

E se non piace la convenzione? Esempio: in Rails le tabelle sono nomi plurali. Il professore del Politecnico di un mio cliente insegnava che devono avere nome singolare e forse anche il mio. Gli ORM Python che ho visto usano il singolare. Rails sembra considerare le tabelle come degli array, da cui il plurale. Se il plurale fosse un peccato mortale non conviene usare Rails.

# I default

Django e Web2Py hanno dei default un po' "laschi". Invogliano ad avere un solo controller, la ```app_name/views.py``` di Django e il ```app_name/controllers/default.py``` di Web2py e questo è male. Sarà un caso ma mi sono sempre trovato di fronte a controller di 1000 o 2000 righe. Inducono il principiante in errore e quando arriva uno sviluppatore più esperto il cliente non ha soldi per il refactoring ma solo per le funzionalità per cui l'ha chiamato.

# Legacy?

Django e Web2Py hanno il concetto di applicazioni, come gli application server Java. Non so se abbia senso dato che non si fa deploy con i war ma con i sorgenti. Forse è un modo per far sentire a casa gli sviluppatori Java del 2005. Di sicuro Rails è andato bene con una sola applicazione per istanza. Se ne serve una seconda sullo stesso server, si lancia un altro interprete Ruby con un'altra applicazione. E' utile anche per il parallelismo, che come in Python è per processo.

# Il deploy

Le applicazioni Rails fanno deploy con Capistrano, o mina che mi piace di più per la velocità.
http://capistranorb.com/
http://nadarei.co/mina/

Con Django e Web2Py non sembra esserci nulla di equivalente. Ho trovato fabistrano ma non pare essere mainstream.
Ancora mi chiedo se esista un modo standard per fare deploy e rollback. ```git pull``` e ```rsync``` hanno degli svantaggi come il deploy di file che non hanno senso in produzione (es: i test). ```scp``` ha il problema di non cancellare i file. Il rollback è difficoltoso.

In realtà nulla vieta di usare Capistrano anche per Python ma è strano che non esista uno strumento nativo.

# Migrazioni DRY

Invece è davvero interessante come gli ORM Python cerchino di tenere DRY la modifica del DB. Si descrivono i modelli in Python in ```models/``` invece che in Ruby in ```db/migrations```. Significa avere la truth del modello in un solo file vs averla distribuita in più migrazioni non facilmente identificabili. Questo è positivo. È poi Django a generare le migrazioni in base alle modifiche ai file dei modelli. I file dei modelli di Django si possono mettere ovunque, basta che ci sia un file ```models.py``` (devono stare tutti nello stesso file) e una directory ```migrations/``` con dentro un ```__init.py__```. Dovrebbe farlo Django in automatico. Per fortuna li si possono estrarre in file separati da importare da dentro ```models.py```. Brutto default indurre lo sviluppatore a mettere tutti i modelli insieme. Rails invece fa autodiscovery degli attributi e la sua truth è lo schema del DB.
Qual è la scelta migliore? Probabilmente sono sbagliate entrambe, chi più, chi meno.
È corretto che la truth sia il database, perché è l'owner dei dati. È pratico ma non va bene che le migrazioni siano comandate da una delle tante applicazioni che accederanno al database, anche se quella web di solito è l'applicazione principale. Soprattutto in un modo che va verso i microservizi sarebbe meglio avere un repository a parte solo per le migrazioni. Importare la truth dal database quindi è importante (reverse engineering). Il linguaggio che si usa non è importante, anche se Ruby è migliore di Python per i DSL.

Notare che benché le migrazioni di Rails siano tutte in ```db/migrations``` e i modelli tutti in ```app/models```, anche con Rails si possono spargere file ovunque. Il metodo standard è creare gemme con le loro migrazioni, controller, modelli, viste, asset etc. È più laborioso che scrivere il codice tutto nell'app principale e quindi Rails non invoglia a farlo. Cattivo default?

Uno dei problemi con l'approccio di Django è che se per caso un modello sparisce per errore, poi ```manage.py makemigrations``` genera la ```DROP TABLE``` e si posso perdere i dati. Con Rails uno sviluppatore deve scrivere esplicitamente il comando di drop.

Per disabilitare le migrazioni in Django
https://docs.djangoproject.com/en/dev/ref/models/options/#managed

Web2py ha un approccio molto rischioso alle migrazioni.
Appena un modello modificato viene eseguito, magari perché si apre la console interattiva, fa la diff tra la definizione nel modello e la tabella e crea ed esegue il codice SQL che allinea la tabella alla definizione. Si possono disabilitare e lo consiglio tantissimo.


# Import automatico vs esplicito

In Django e Web2Py (e Java, e PHP, e ... ) si deve importare quel che serve  in ogni file. Rails importa tutto da solo. In rare occasioni Rails può dare conflitti (uno o due in 10 anni nel mio caso) mentre Django obbliga a riempire il codice di import. Idem per la shell, che così diventa un po' meno efficiente.

$ python manage.py shell
from modules.plugins.censys.models import Settings
x = Settings.objects.get(pk=1)
x.api_id

E infatti http://stackoverflow.com/questions/4863301/automatically-import-models-on-django-shell-launch

Anche Phoenix è così. Fastidioso...

Anche Ruby richiede ```require``` di tutto, ma Rails risparmia fatica allo sviluppatore.

# Fatica

In generale ho l'impressione che con Python e i suoi framework web si debba faticare di più rispetto a Ruby e Rails. Forse limitatamente ai framework web, la maggior flessibilità permette di fare più facilmente cose non standard che con Rails richiederebbero una maggior fatica, ma davvero si vuol pagare questo prezzo ogni singola volta? Mi ricorda la click fatigue di Windows e di KDE rispetto a Gnome. Il doppio dei click per lo stesso risultato.


# Templating

Il linguaggio di templating di Django purtroppo non è Python, ma un linguaggio molto limitato con lo scopo dichiarato di costringere lo sviluppatore a preparare tutti i dati nelle view (il controller) e non angustiare il designer con codici strani per lui. La scelta è curiosa perché praticamente è l'unico posto in cui Django limita lo sviluppatore rispetto a Rails. Non ci sono sviluppatori Rails sani di mente che mettono di proposito query al db nelle viste di Rails.

Il risultato è che si deve imparare un nuovo linguaggio. Una fatica inutile che rallenta lo sviluppo. Mi ricorda tanto le tag libs JSP.
Altro risultato: si finisce per dover scrivere taglib che implementano funzionalità di base, come un loop su due liste. Quel che si sarebbe fatto in pochi secondi costa dieci minuti tra cercare, pensare, scrivere. Moltiplicandolo per un po' di occorrenze il costo per il cliente sale. Esempio:

http://stackoverflow.com/questions/2415865/iterating-through-two-lists-in-django-templates/14857664#14857664

```
@register.filter(name='zip')
def zip_lists(a, b):
  return zip(a, b)

{% for a, b in first_list|zip:second_list %}
  {{a}}
  {{b}}
{% endfor %}
```

vs

```
<% @first_list.each_with_index |a, index| %>
  <%= a %>
  <%= second_list[index] %>
<% end %>
```

Notare tra l'altro il mismatch tra il nome del filtro ```zip``` nel template ed il nome della funzione nella templatetag ```zip_lists```. Il nome della funzione include il tipo degli argomenti. Non è il principio di least surprise.

Web2py invece usa Python e quindi non si può dire che si tratti di una scelta comune a tutti i framework web di Python. Qui vincono Rails e Web2Py.

Però copierei in Rails l'idea dei filtri, immagino come metodi da applicare ad oggetti, ma non l'ho studiata.


# Alto livello vs basso livello.

Python obbliga a fare un po' del lavoro del compilatore.

Il misterioso file __init.py__ deve essere presente in una directory perché i file al suo interno vengano riconosciuti come moduli. Ogni altro linguaggio noto se la cava tranquillamente senza. Forse qui si vede l'età. Il design di Python è iniziato alla fine degli anni '80 e allora poteva sembrare una buona idea. Chi è arrivato dopo l'ha eliminato e ha fatto in altro modo.

Il due punti in fondo alle righe è inutile andrebbe depracato. La sua origine è in uno studio di usabilità presso sviluppatori principianti: gli rendeva più semplice capire che dopo iniziava un blocco indentato, nonostante ci fosse già l'indentatura.

# Mixed paradigm

Python è funzionale ed object oriented, anche se a Guido Van Rossum la parte funzionale non piace molto. Non si è obbligati a dichiarare classi o a metter metodi in classi o in moduli come in Ruby. Si possono fare file di sole funzioni che poi vanno importati come moduli andando a ricreare implicitamente con il file system il nome del modulo. I file si possono spostare in altre directory e il nome del modulo cambia senza traccia né dentro al file stesso né in quelli dove è usato. Credo un male. È un linguaggio un po' disordinato...

# Funzioni predefinite

https://docs.python.org/2/library/functions.html
Python ha molte funzioni predefinite. Dati gli anni, forse è stato disegnato così sul modello della standard library C. Una certa somiglianza con un linguaggio più noto aiuta la diffusione nei primi anni di vita di un linguaggio.

Scrivo

```
>>> print netblocks.keys().sort()
None
```

Perché ```None``` e non la lista ordinata?

Perché ```.sort()``` modifica l'argomento e non lo ritorna.

Quel che volevo era ```sorted```, una delle funzioni predefinite di Python.

```
>>> sorted(netblocks.keys())
[u'1.2.3.0/24', u'1.3.0.0/16', u'2.2.4.0/24', u'3.2.3.0/24', u'4.2.3.0/24']
```

È il principio di Least surprise girato al contrario.
Non si chiamano neppure nello stesso modo, ```sorted``` vs ```sort```.

```
>>> a = [u'2.2.4.0/24', u'3.2.3.0/24', u'1.3.0.0/16', u'1.2.3.0/24', u'4.2.3.0/24']
>>> a.sort()
>>> a
[u'1.2.3.0/24', u'1.3.0.0/16', u'2.2.4.0/24', u'3.2.3.0/24', u'4.2.3.0/24']
```

Il metodo ```sort``` di array in Ruby non modifica l'array. Lo modifica ```sort!```
La convenzione è usare un ```!``` nel nome dei metodi che modificano l'oggetto su cui vengono chiamati.

```
> a = ['2.2.4.0/24', '3.2.3.0/24', '1.3.0.0/16', '1.2.3.0/24', '4.2.3.0/24']
 => ["2.2.4.0/24", "3.2.3.0/24", "1.3.0.0/16", "1.2.3.0/24", "4.2.3.0/24"]
> a.sort
 => ["1.2.3.0/24", "1.3.0.0/16", "2.2.4.0/24", "3.2.3.0/24", "4.2.3.0/24"]
> a
 => ["2.2.4.0/24", "3.2.3.0/24", "1.3.0.0/16", "1.2.3.0/24", "4.2.3.0/24"]
> a.sort!
 => ["1.2.3.0/24", "1.3.0.0/16", "2.2.4.0/24", "3.2.3.0/24", "4.2.3.0/24"]
> a
 => ["1.2.3.0/24", "1.3.0.0/16", "2.2.4.0/24", "3.2.3.0/24", "4.2.3.0/24"]
```


# Iteratori

Python:

```
for k, v in list.iteritems():
    print k,v
```

Perché si deve affaticare lo sviluppatore e non si disegna un linguaggio dove basta questo?

```
for k, v in list:
    print k,v
```

Ruby:

```
for k,v in x do
    print k,v
end
```


# Error reporting

Gli errori di Web2py sono quasi inutili. Per qualche ragione non riesce a mostrare la riga di un template in cui è avvenuto l'errore. Riporta quella del file Python in cui è tradotto il template, che serve a poco. E' carina l'autoapertura dei ticket ma in sviluppo sarebbe molto meglio vedere subito il messaggio d'errore.

# String join e split

```
"a.b.c".split(".") # passive verb, string is split by .
['a', 'b', 'c']
```

ma

```
".".join(["a", "b", "c"]) # active verb: . joins array
'a.b.c'
```

Perchè? In Ruby c'è più consistenza

```
"a.b.c".split(".") # passive: string is split by .
["a", "b", "c"]
["a", "b", "c"].join(".") # passive: array is joined by .
"a.b.c"
```

In un lingua naturale, dove prevale il modo attivo avremmo

```
. separa "a.b.c"
. unisce ["a", "b", "c"]
```

Esperti Python mi dicono che il primo è ```string.join(iterable)``` e sarebbe stato strano forzare ogni iterable ad avere un metodo che ha a che fare con le stringhe. Meglio metterlo in string. In effetti in Ruby join è un metodo di Array e di nient'altro.

```
Python > ",".join([str(i) for i in range(1,21)])
'1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20'

Ruby > (1..20).to_a.join(",").to_s
 => "1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20"
```

# Fatiche di sintassi

## I due punti

```
    if product
             ^
SyntaxError: invalid syntax
```

Non spiega cosa c'è che non va. E se sa che è syntax error per via dei due punti, perché non li aggiunge da solo e non si fa una PEP per renderli opzionali?

Essendo rumore e non segnale, presto si impara a non vedere i due punti e alle volte capita che restano attaccati in fondo alle righe quando si fa refactoring e si passa ad esempio da un ```if expr:``` a ```return expr```

```
return match_version(vm, protocol_data):
                                           ^
SyntaxError: invalid syntax
```

Sono pure meno visibili di una ```{```, anche perché si tendono a scrivere attaccati alla parentesi.

## La spaziatura

```
    def match_version(version_matcher, protocol_data):
                                                     ^
IndentationError: unindent does not match any outer indentation level
```

Qual è il problema?

```
def find_version(product_matcher, protocol_data):
     if "version" in product_matcher:
         version_matcher = product["version"]
     else:
         return None

     return match_version(version_matcher, protocol_data)

 def match_version(version_matcher, protocol_data):
```

Ah, uno spazio "invisibile" a inizio linea nel def match_version, eredità di un copia e incolla.

Altra situazione, quando si copia codice dall'editor alla console interattiva è molto più rapido copiare tutta la linea inclusi gli spazi all'inizio. Peccata che si vada in errore. O si perde tempo a fare un copia e incolla preciso oppure si installa iPython ```pip install ipython```. Ma se occorre un extra per risolvere un problema del linguaggio vuol dire che il design iniziale non era buono. Per contro ```gem install pry``` aggiunge molte cose interessanti a ```irb```, ma si vive anche senza.

## do end

Venendo da C e Java 12 anni fa ```do``` ```end``` mi facevano abbastanza schifo (si può scrivere?). Poi ci si abitua ma non tutto quello a cui ci si abitua fa bene (vedi rumore, inquinamento, etc). In realtà ```do``` si usa relativamente poco. Quel che affatica è ```end```.
Facendo i conti ed anche omettendo le parentesi tonde e i punti e virgola obbligatori in C e Java

C e Java

```
if (cond) { # 4 caratteri extra rispetto a Ruby
  something(); # 2 carattere extra rispetto a Ruby e Python
} # 1 carattere extra rispetto a Python, 2 meno di Ruby
```

Ruby

```
if cond # 4 caratteri meno di Java, 1 rispetto a Python
  something() # omettere le parentesi è cattivo stile
end # 3 caratteri extra rispetto a Python, 2 rispetto a Java
```

Python

```
if cond: # 1 carattere extra rispetto a Ruby
  something() # 1 in meno di Java
# 3 in meno di Ruby
```

In Python si scrive di meno. Si paga il prezzo di possibili bug con gli spazi sintattici. De gustibus. Personalmente non disegnerei mai un linguaggio con spazio significativo. Lo spazio è solo per l'occhio, non per il compilatore. Non dopo le schede perforate.


## {{ }} e {% %}

Il linguaggio di templating di Django usa ```{{ value }}```  per generare HTML e ```{% tag %}``` per comandi e templatetag (gli helper di Rails, organizzati meglio in Django).
Quando si passa da una all'altra ci sono due punti da modificare, a inizio e fine blocco.

Con ERB ```<%= value %>``` ```<% block %>``` si fa una sola modifica e complessivamente si risparmia tempo perché il carattere extra ERB si recupera subito con i tanti tasti freccia schiacciati in meno.


## Chiavi stringa negli hash Ruby / dict Python

Questo è meglio in Python, perché in Ruby si devono usare le frecce ```=>```

In origine si poteva usare solo la freccia ```=>``` come in ```{:a => 1}```
Poi con Ruby 2.0 è stata introdotta la scorciatoia in stile JavaScript ```{a: 1}```
Purtroppo non funziona se come chiave si usano le stringhe

```
> hash1 = {a: 1} # simbolo
 => {:a=>1}
> hash2 = {"a" => 1} # stringa
 => {"a"=>1}
> hash3 = {:"a" => 1} # simbolo, ovviamente
 => {:a=>1}
> hash4 = {"a": 1} # simbolo :-(
 => {:a=>1}
```

Mi sarei atteso che l'ultimo esempio inserisse una chiave di tipo stringa.

Nota: ```:"stringa"``` è il simbolo ```:stringa``` e non una stringa. Si usa per poter usare spazi nel nome del simbolo come in ```:"simbolo con spazi"```. Simboli e stringhe sono diversi, anche se con le stringhe frozen per default in Ruby 2.3 non c'è molta differenza nelle prestazioni.

Da questo punto meglio Python che può usare le stringhe come chiavi e ci permette di copiare il JSON direttamente nel sorgente Python senza trasformare i due punti in frecce.

```
$ python
> dict1 = {"a": 1}
```

Purtroppo non avendo i simboli in Python siamo costretti a perder tempo e inserire i doppi apici sempre.

```
> dict2 = {a: 1}
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'a' is not defined
```

In Ruby non accadrà https://bugs.ruby-lang.org/issues/4801

> Iff {'key': 'value'} means {:key => 'value'} I have no objection.
> Matz

> [...] we have already tried making Symbols a subclass of String, or making Symbols behave like Strings, and it didn't work out.
> Matz.

Questo è Python

```
vulnerabilities = {
    "HighlyPossibleSqlInjection": {"value": 1, "description": "SQL Injection"},
    "Xss": {"value": 2, "description": "Temporary / Reflected XSS"}
    }
print(vulnerabilities["Xss"]["value"]
```

e questo è Ruby

```
vulnerabilities = {
         "HighlyPossibleSqlInjection": {"value": 1, "description": "SQL Injection"},
         "Xss": {"value": 2, "description": "Temporary / Reflected XSS"}
}
 => {:HighlyPossibleSqlInjection=>{:value=>1, :description=>"SQL Injection"}, :Xss=>{:value=>2, :description=>"Temporary / Reflected XSS"}}
```

Le chiavi sono simboli e potrebbe non essere quel che vogliamo se stiamo lavorando con del JSON. Rails però ha ```HashWithIndifferentAccess```
http://api.rubyonrails.org/classes/ActiveSupport/HashWithIndifferentAccess.html
Come ho già scritto, un linguaggio ha qualcosa che non va se bisogna aggiungergli dei tool esterni.

Però in Ruby puro possiamo un po' meglio e un po' peggio rispetto a Python

```
require 'ostruct'
vulnerabilities = OpenStruct.new(
  HighlyPossibleSqlInjection: OpenStruct.new(value: 1, description: "SQL Injection"),
  Xss: OpenStruct.new(value: 2, description: "Temporary / Reflected XSS")
)

vulnerabilities.each_pair {|attribute, item| puts "#{attribute} #{item.value} #{item.description}" }
HighlyPossibleSqlInjection 1 SQL Injection
Xss 2 Temporary / Reflected XSS
```

Meglio perché è molto più bello scrivere

```
puts vulnerabilities.Xss.value
```

oppure

```
key = params[key]
puts vulnerabilities[key].value
```

Peggio perché la definizione di vulnerabilities non è altrettanto facile da leggere. Quegli ```OpenStruct.new``` sono rumore.
Peggio anche perché creare una ```OpenStruct``` invalida la cache dei metodi
https://github.com/charliesome/charlie.bz/blob/master/posts/things-that-clear-rubys-method-cache.md
Se lo dovete fare fatelo solo quando l'applicazione parte non ad esempio ad ogni chiamata di una URL.

## Switch / case

Python non ce l'ha e quindi si trovano alle volte blocchi di 100 linee come queste

```
if var == "uno":
  funzione_uno(arg)
elif var == "due":
  funzione_due(arg)
elif var == "tre":
  funzione_tre(arg)
...
elif var == "cento":
  funzione_cento(arg)
else:
  funzione_default(arg)
```

Il modo pythonico sarebbe usare un dict, che ricorda tanto una tabella di function pointer del C.

```
{
"uno":  funzione_uno,
"due":  funzione_due,
"tre":  funzione_tre,
...
"cento": funzione_cento
}.get(var, funzione_default)(arg)
```

Oltre ai dizionari si potrebbe scrivere

```
def func0(arg):
   ...
def func1(arg):
  ...

i_want_func = 'func1'
globals()[i_want_func]()
```

e c'è la tecnica object oriented di eliminare ```if``` e ```case``` con le classi.
https://sourcemaking.com/refactoring/replace-conditional-with-polymorphism

Il modo sano sarebbe stato

```
switch var:
  "uno":  funzione_uno(arg)
  "due":  funzione_due(arg)
  "tre":  funzione_tre(arg)
   ...
  "cento": funzione_cento(arg)
  default: funzione_default(arg)
```

che è più asciutto dell'equivalente Ruby con i ```when```.

Notare che in Ruby si potrebbe scrivere

```
self.send(Hash.new(:metodo_default).merge({
"uno" => :metodo_uno,
"due" => :metodo_due,
"tre" => :metodo_tre,
...
"cento" => :metodo_cento
})[var], arg)
```

se si tratta di metodi della stessa classe del codice. È assai brutto da vedere. È come se Ruby ci sconsigliasse di usare questa forma.

```
case var
  when "uno"
    funzione_uno(arg)
  when "due"
   funzione_due(arg)
  when "tre"
    funzione_tre(arg)
    ...
  when "cento"
    funzione_cento(arg)
 else
    funzione_default(arg)
end
```

Un difetto di Ruby? Avrebbe potuto usare ```switch``` e ```case``` come gli altri linguaggi.


## Pipe

```
[1, 2, 3].map { |x| x * x }
```

C'è una ```|``` di troppo. Basterebbe

```
[1, 2, 3].map { x | x * x }
```

L'ho visto in un linguaggio di recente, ma non ricordo quale.
Quindi forse si può omettere anche dalla forma estesa.

Questo sarebbe l'ideale

```
fai qualcosa do con, degli, argomenti
   con.metodo1 degli, argomenti
   con.metodo2 degli, argomenti
end
```

Se servono dei deliminatori forse questo è accettabile

```
fai qualcosa | con, degli, argomenti do
   con.metodo1 degli, argomenti
   con.metodo2 degli, argomenti
end
```

Di certo nella forma breve non ci sono problemi.


## Operatore ternario

Forse per mantenere consistenza con lo stile discorsivo degli operatori booleani ```and or not``` Python (solo dalla 2.5) ha introdotto questo operatore ternario

```
a if condition else b
```

dove praticamente ogni altro linguaggio, Ruby incluso, usa

```
condition ? a : b
```

Da un lato apprezzo la consistenza dello stile, dall'altro non approvo il render difficile la vita allo sviluppatore.
Inoltre forse è arrivato un po' tardi perché un paio di mesi fa lo sviluppatore Python a capo di uno dei progetti che seguo mi aveva detto che Python non ha un operatore ternario. Quando si impara che una cosa non esiste, poi non la si cerca più.

# Blocchi

Il passaggio di blocchi a metodi caratterizza profondamente Ruby. Python ha qualcosa di simile con ```with```

Un esempio da https://jeffknupp.com/blog/2016/03/07/python-with-context-managers/

```
with open('what_are_context_managers.txt', 'r') as infile:
    for line in infile:
        print('> {}'.format(line))
```

In Ruby si fa così, usando i blocchi e la cooperazione della classe File che ha un costruttore che accetta un blocco e un enumeratore (perché usa la classe Enumerator, sarebbe un iteratore Python) che ritorna le linee del file

```
File.open("file", "r") do |f|
  f.each do |line|
    puts line
  end
end
```

```with as``` si mappa quasi 1:1 sul ```do |f|``` di Ruby e ```for in``` (che esiste anche in Ruby) è l'equivalente di ```each |line|```.


## L'orribile do while Ruby

Non è possibile che un linguaggio non abbia un ```do while``` e costringa a scrivere codice come questo.

```
loop do
  # codice
  break if condition
end
```

Si faceva di peggio solo in BASIC sugli home computer negli anni '80.

# Stile

La raccomandazione di usare la notazione arg=valore senza spazi per i keyword argument e i parametri di default è cattiva perché confonde ed involontariamente provoca la scrittura di codice come questo

```
db(db.rigaordinipo.id==vars.idriga[i]).update(quantita=vars.quantita[i])
```

mentre lo stile consigliato sarebbe invece

```
db(db.rigaordinipo.id == vars.idriga[i]).update(quantita=vars.quantita[i])
```

Cambiare stile nella stessa riga non è banale e una qualsiasi tra

```
legacy_db(legacy_db.rigaordinipo.id==vars.idriga[i]).update(quantita=vars.quantita[i])
legacy_db(legacy_db.rigaordinipo.id == vars.idriga[i]).update(quantita = vars.quantita[i])
```

sembra più consistente. A me piace la seconda con gli spazi ovunque.
https://www.python.org/dev/peps/pep-0008/

A parte vecchio codice PHP non avevo mai più visto così tante righe come queste prima di iniziare a leggere programmi Python altrui.

```
assegno = "con lo spazio, meno male!"
assegno="tutto attaccato"
assegno= "attaccato a sinistra"
assegno ="attaccato a destra"
```

Coincidenza?

Altro problema con lo stile, lo smodato uso di underscore in Python, forse altra eredità degli anni '80. Questo si ripercuote lungo tutto l'ecosistema, con convenzioni che dipendono da differenze quasi invisibili, come due underscore invece di uno.

Esempio d'uso dell'ORM di Django

```
    def matching_lines(self):
        """It looks for a match in all the netsparker scan entries of every scan of the project"""
        doc_ids = Doc.objects.values_list("id", flat=True).filter(project=self.doc.project)
        return DocLine.objects.filter(
            type=self.type,
            url=self.url,
            parameter=self.parameter,
            parameter_type=self.parameter_type,
            parameter_value=self.parameter_value,
            doc__in=doc_ids # due underscore a sinistra dell'uguale!
        )
```

I due underscore indicano che il Doc riferito dalla DocLine deve avere un id incluso in ```doc_ids```.

Lo stesso in Rails

```
   def matching_entries
      doc_ids = Doc.where(project: doc.project).pluck(:id) # I don't like the pluck name!
      DocLine.where(
            type: type,
            url: url,
            parameter: parameter,
            parameter_type: parameter_type,
            parameter_value: parameter_value,
            doc: doc_ids # and select(:id) instead of pluck would make this do a nested select again
        )
     end
```

Rails se la cava semplicemente accorgendosi che ```doc_ids``` è un array e quindi è ovvio che va aggiunta la ```WHERE doc_id in SELECT IN (doc_ids)```. Va detto che la gemma ```squeel``` quasi va in quella direzione e c'è a chi piace.

Altri due ORM Python, SQLAlchemy e peewee, non usano underscore semantici.

## Eccezioni

Eccezioni, per una volta preferisco la sintassi di Python, a parte i due punti

```
try:
  something(that, might, break)
except SomeException as e:
  print(e)

begin # meglio un try, più esplicito
  something that, might, break
rescue SomeException => e # perché non rescue SomeException |e|  come per i blocchi?
  puts e
end
```


BTW, non fate ```rescue``` di ```Exception`` perché intercettereste ogni segnale ed errore. Provate questa:

```
loop do
  begin
    print "Try to control-C"
    sleep(1)
  rescue Exception => e
    puts e
    print "how to terminate this?"
  end
end
```


## La virgola che non si vede

```
cursor.execute("SELECT * FROM table WHERE field = ?, (value))
sqlite3.ProgrammingError: Incorrect number of bindings supplied. The current statement uses 1, and there are 2 supplied.
```

La virgola!

```
cursor.execute("SELECT * FROM table WHERE field = ?, (value,))
```

Vista adesso?

Senza virgola è solo un'espressione tra parentesi, con la virgola è una tupla di un elemento. Cattiva idea usare le parentesi tonde per tutte e due le cose.

Le tuple in Elixir hanno le graffe ```{value}``` ed il problema si risolve da sè.

Ruby ne fa a meno, non ha tuple. Le ```Struct``` gli assomigliano ma devono essere dichiarate in anticipo e sono molto verbose.
Inoltre possono avere dei metodi. Sono delle specie di classi.
http://ruby-doc.org/core-2.4.0/Struct.html


# Logging

Il setup del logging è incredibilmente complicato. Con Web2py non ho ancora capito come si fa a fare logging da un controller. Questo non fa nulla.

```
import logging
logging.basicConfig(filename='/home/www-data/web2py/APP.log', filemode='w', level=logging.DEBUG)
logger = logging.getLogger("web2py.app.APP")
logger.setLevel(logging.DEBUG)
logger.debug("Creato il logger")
```

Da console python invece funziona e anche in Django va. I framework web invogliano a creare tanti file di log diversi.

Anche Ruby ha i suoi logger, e non sono tanto più semplici. Eppure Rails va avanti da sempre con un solo file di log globale. Se proprio si vuole se ne possono creare altri, ma nessuno lo fa se non in casi particolari. Django complica un affare semplice e fa perdere tempo. Avrebbero dovuto includere un logger globale in ogni view. Forse non lo fa per via del design di base del framework. Le view avrebbero dovuto essere classi derivate da una stessa classe di base, che gestisce il logger. Django invece di base è un insieme di funzioni.


# Pretty printing

Questo è un hack eppure pare che non si possa fare di meglio.

```
import pprint
records = Model.objects.order_by("field")
pp = pprint.PrettyPrinter()
pp.pprint(records[0].__dict__)
```

Anche Ruby ha una classe di pretty printing. Non ricordo come si chiami perché basta fare così

```
records = Model.order("field")
p records[0] # o puts records[0].inspect
```

# Un problema di design

Il colpevole è Web2py, ma l'insegnamento è generale.

C'è una query che estrae un campo da una tabella, ma la query è sbagliata

Errata:

```
filenames = db(db.tabella).select(db.tabella2.filename, distinct=True)
SELECT DISTINCT tabella2.filename FROM tabella, tabella2 WHERE (tabella.id IS NOT NULL);
```

Corretta

```
filenames = db(db.tabella2).select(db.tabella2.filename, distinct=True)
SELECT DISTINCT tabella2.filename FROM tabella2 WHERE (tabella2.id IS NOT NULL);
```

La prima non ritorna record. La seconda torna quel che dovrebbe. Qual è l'errore nella prima?
Trovato?

Con ActiveRecord in Rails 5 avremmo scritto una di queste

```
filenames = Tabella2.pluck("DISTINCT filename") # pluck è un nome orrendo!
filenames = Tabella2.select("DISTINCT filename").map(&:status) # ma anche il map in fondo non è bello
SELECT DISTINCT filename FROM tabella2;

filenames = Tabella2.distinct.pluck("filename")
filenames = Tabella2.distinct.select("filename").map(&:status)
SELECT DISTINCT tabella2.filename FROM tabella2
```

con solo una occasione in cui sbagliare il nome della tabella. Personale preferenza per la prima.

Lesson learned: mai al programmatore l'occasione di sbagliare. La coglierà :-)

# Unicode

Meglio non parlarne perché con Python 3 il problema è risolto. Chi deve continuare a lavorare con Python 2 ogni tanto soffre.
Di Ruby 1.8 si sono quasi perse le tracce.


# Interpolazione di stringhe

La forma Python

```
"Ciao {} {}, come stai?".format(nome, cognome)
```

secondo me si legge peggio di

```
"Ciao #{nome} #{cognome}, come stai?"
```

perché costringe a saltare con l'occhio a destra per capire cosa verrà inserito a sinistra.

Sarebbe fantastico se si potesse fare a meno del ```#``` e si potesse scrivere solo

```
"Ciao {nome} {cognome}, come stai?"
```


# Hot reload

Django fa un restart quando si salva un file. È veloce ma spesso il reload dello sviluppatore è più veloce ancora e si ha un errore e si deve ricaricare una seconda volta. Meglio il comportamento di Rails che resta appeso fino al reload (o lo fa ogni volta?) senza dare errori. Anche Web2py non dà problemi.
Django inoltre pare consumare CPU mentre sta in attesa, forse proprio per tener d'occhio i file e ricaricare. Rails e Web2Py non hanno problemi. Abbatte la batteria e fa girare un po' la ventola.


# Simboli

Python non ha i simboli e quindi bisogna definire costanti anche se non ci interessa il loro valore.

# Keyword arguments

In Python si usano keyword arguments molto più che in Ruby, forse perché in Ruby siamo stati abituati a simularli usando hash di opzioni e pochi li stanno usando ora che ci sono (Ruby 2.0, 4 anni fa)

```
def metodo(arg1, options = {})
    arg2 = options[:arg2] || ""
    arg3 = options[:arg3] || ""
    ...
end

metodo(arg1)
metodo(arg1, arg2: 2)
metodo(arg1, arg3: 3)
metodo(arg1, arg2: 2, arg3: 3)
metodo(arg1, arg3: 3, arg2: 2)
```

Funziona perché quando Ruby vede ```arg3: 3, arg2: 2``` lo collassa in un unico argomento di tipo Hash. L'ultima riga equivale a

```
metodo(arg1, {arg3: 3, arg2: 2})
```

Si può anche scrivere così, ma dover usare ```[0]``` per arrivare agli argomenti non è bello

```
def metodo(arg1, *options)
    arg2 = options[0][:arg2] || ""
    arg3 = options[0][:arg3] || ""
    ...
end
```

Quella sintassi si usa quando gli argomenti sono in numero variabile e non sono un Hash.

I keyword argument invece si definiscono così

Ruby

```
def metodo(arg1, arg2: "", arg3: "")
   ...
end

metodo(1)
metodo(1, arg2: 2, arg3: 2)
...
```

Python

```
def funzione(arg1, arg2="", arg3=""):
   ...

funzione(1)
funzione(1, arg2=2, arg3=2)
```

# yield

Stesso nome per due funzionalità molto diverse.

Python

```
def firstn(n):
   num = 0
   while num < n:
     yield num
     num += 1

firstn(3)
#<generator object firstn at 0x7f9c084866e0>
[x for x in firstn(3)]
# [0, 1, 2]
```

La chiamata a ```firstn(3)``` ritorna un Generator. ```firstn``` inizia davvero ad eseguire quando alla comprehension serve la prima ```x```. Arriva fino a ```yield num``` e ritorna ```0``` e resta sospesa. Poi la comprehension le chiede una seconda ```x``` e ```firstn``` riparte da dove era arrivata, incrementando ```num``` e ritornando ```1```.

Lo stesso codice in Ruby fa una cosa diversa

```
def firstn(n)
   num = 0
   while num < n
     yield num
     num += 1
   end
end
```

prova ne è questo errore

```
firstn(3)
LocalJumpError: no block given (yield)
       from (irb):4:in `firstn'
       from (irb):8
       from /home/me/.rvm/rubies/ruby-2.3.0/bin/irb:11:in `<main>'
```

Infatti in Ruby ```firstn``` parte e quando arriva a ```yield``` passa ```num``` ad un blocco di codice, che deve esserle stato passato come argomento. Ad esempio.

```
firstn(3) {|x| puts x}
0
1
2
```

oppure

```
firstn(3) do |x|
   molte
   linee
   di
   codice
end
```

Per creare un generatore in Ruby si usa la classe ```Enumerator```

```
def firstn(n)
  Enumerator.new do |enum|
    num = 0
    while num < n
      enum.yield num # metodo yield di Enumerator, non keyword yield di Ruby
      num +=1
    end
  end
end
firstn(3)
#<Enumerator: #<Enumerator::Generator:0x00000000cace98>:each>
 ```

Adesso

```
firstn(3).map {|x| x}
# [0, 1, 2]
```

come la ```[x for x in firstn(3)]``` di Python.

Viceversa, l'equivalente di una ```yield``` Ruby in Python è il passaggio di una funzione come argomento ad un'altra

```
def firstn(n, fun):
   num = 0
   while num < n:
     fun(num)
     num += 1

def pr(n):
   print(n)

firstn(3, pr)
0
1
2
```

In Ruby non si può passare il nome del metodo bensì il simbolo corrispondente al suo nome.

```
def firstn(n, fun)
   num = 0
   while num < n
     send(fun, num)
     num += 1
   end
end

def pr(n)
   puts n
end

firstn(3, :pr)
0
1
2
```

È una conseguenza dell'aver reso facoltative le parentesi: ```pr``` è identico a ```pr()``` ed eseguirebbe il metodo andando in errore per la mancanza del parametro.

```
firstn(3, pr)
ArgumentError: wrong number of arguments (given 0, expected 1)
```

In Python bisogna sempre passare come parametro una funzione definita esplicitamente, a meno che la funzione non si limiti a calcolare un'espressione. In quel caso si può usare una ```lambda```. Le lambda Python non possono contenere statement.

https://docs.python.org/2/reference/expressions.html#lambda
https://docs.python.org/3/reference/expressions.html#lambda

```
def fn(n, fun):
   return fun(n)

fn(2, lambda x: x + 1)
3
```

Equivale a

```
def fn(n)
  return yield n
end

fn(2) {|x| x + 1}
```

# WAT

Ruby

https://www.destroyallsoftware.com/talks/wat


Python

https://youtu.be/sH4XF6pKKmk
https://github.com/cosmologicon/pywat

# Le basi

## Boolean

Ruby usa la sintassi C per gli operatori booleani con il comportamento che ci si aspetta ```&& || !``` e una sintassi "umana" per quelli con un comportamento sorprendente ```and or not```. Gli ultimi vengono da Perl e in realtà non sono operatori booleani (alla faccia del principio di least surprise).
Invece si usano per modificare il flusso di controllo del programma, come in

```
if error
  manage(error) or puts "Niente da fare"
  puts "Error #{error}" and return
end
```

Nella pratica si sconsiglia di usarli. Le uniche volte in cui capita di incontrarli è quando un principiante ne fraintende l'uso li impiega in espressioni booleane introducendo bug incomprensibili causati dalla mancata conoscenza della reale precedenza dei due set di  operatori. https://ruby-doc.org/core-2.4.0/doc/syntax/precedence_rdoc.html

Esempio:

```
true || false
# true
true or false
# true
false || true
# true
false or true
# true
```

sembrano equivalenti, ma usandoli in un assegnamento ```=```, che è un operatore a maggior precedenza di ```or```

```
x = true || false; puts x
# true
x = true or false; puts x
# true
x = false || true; puts x
# true
x = false or true; puts x
# false     # OPS!
x = (false or true); puts x
# true     # ma non vogliamo usare le parentesi attorno ad ogni espressione booleana
```

Quanto a ```not```

```
!true && false
# false
not true && false # equivale a not (true && false)
# true     # OPS
```

Dettagli a

* http://www.prestonlee.com/2010/08/04/ruby-on-the-perl-origins-of-and-versus-and-and-or/
* http://www.virtuouscode.com/2010/08/02/using-and-and-or-in-ruby/
* http://www.virtuouscode.com/2014/08/26/how-to-use-rubys-english-andor-operators-without-going-nuts/

Python usa ```and or not``` come operatori logici e non ci sono sorprese. Vince Python, anche per la praticità di scrittura.

## Strutture dati di base

Python ha liste, tuple, range, set, dict.

Ruby ha array, range, set, hash. Non ha tuple.

Le liste di Python equivalgono agli array di Ruby. I dict sono hash.
I dict dalla versione 3.6 di Python sono ordinati e le performance di lookup sono migliorate.
Hanno migliorato le performance di lookup degli hash anche in Ruby 2.4.
https://blog.heroku.com/ruby-2-4-features-hashes-integers-rounding#hash-changes
Gli elementi sono enumerati in ordine di inserimento.

I range sono identici, a parte la sintassi: ```range(6)``` di Python è ```0..5``` di Ruby.

I set sono identici.

Python ha anche degli array, ma sono un modulo a parte e possono contenere solo oggetti di uno stesso tipo, che va dichiarato in fase di creazione https://docs.python.org/2/library/array.html

```
import array
x = array.array("i", [1, 2, 3, 4]) # signed int
x[2]
# 3
```

Questo aiuta le performance in casi di oggetti tutti dello stesso tipo. Forse è una delle ragioni per cui c'è un NumPy e non un NumRuby. A dir la verità c'è SciRuby https://github.com/SciRuby/sciruby ma non se la passa altrettanto bene.

In Ruby array e hash sono iterabili praticamente con gli stessi metodi di Enumerator https://ruby-doc.org/core-2.4.0/Enumerator.html
Esempio sugli hash.

```
hash = {a: 1, b: 2}
hash.each {|k, v| puts "#{k} => #{v}"}
a => 1
b => 2
```

o in modo non idiomatico con ```for```

```
hash = {a: 1, b: 2}
for k, v in hash do
  puts "#{k} => #{v}"
end
a => 1
b => 2
```

Non è idiomatico perché non si può passare un blocco a ```for``` e non lo si può concatenare con altre trasformazioni sui dati ritornati.

Le tuple in Ruby non esistono. Si possono usare OpenStruct o Struct ma è veramente raro usarle. In Ruby si usano array dove ho visto usare tuple in Python. Se proprio si vuole, si può rendere l'array immutabile come una tupla usando ```freeze```, ma in pratica non lo si fa mai.

Python
```
[(x, x**2) for x in range(6)]
[(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
```

Ruby
```
(0..5).map {|x| [x, x**2].freeze}
[[0, 0], [1, 1], [2, 4], [3, 9], [4, 16], [5, 25]]
```

# Mutabilità e stringhe

Le strutture dati Ruby sono mutabili, a meno che non venga invocato il metodo ```freeze``` su un oggetto.

Le stringhe sono mutabili per default. Le si possono rendere immutabili con il metodo ```freeze```.
Con Ruby 2.3 si può usare una pragma a inizio file (o in ```irb```) per renderle immutabili per default.
Saranno immutabili per default con Ruby 3.

```
$ irb
2.3.0 :001 > # frozen_string_literal: true
2.3.0 :002 >   s = 'abc'
 => "abc"
2.3.0 :003 > s[0] = '1'
RuntimeError: can't modify frozen String
	from (irb):5:in `[]='
	from (irb):5
	from /home/montra/.rvm/rubies/ruby-2.3.0/bin/irb:11:in `<main>'
```

Una stringa immutabile è praticamente identica ai simboli. Avendo già i simboli l'immutabilità delle stringhe non è un'esigenza particolermente sentita, ma con il proliferare di hash con chiavi di tipo string (JSON) è sicuramente un beneficio per le prestazioni. Probabilmente viene utile anche in vista dell'introduzione della concorrenza tramite ```Guild``` http://olivierlacan.com/posts/concurrency-in-ruby-3-with-guilds/


# comprehension, filter, map, reduce

Dove Ruby di solito itera su array con ```each``` o ```map```, Python usa la list comprehension.

```
[x * x for x in [1, 2, 3, 4]]
# [1, 4, 9, 16]
```

È simile alla notazione matematica di "per ogni x in N tale che ..., applica f(x)"
La parte "tale che" è un filtro

```
filter(lambda x: x % 2 == 0, [1, 2, 3, 4])
# [2, 4]
```

Combinandoli

```
[x * x for x in filter(lambda x: x % 2 == 0, [1, 2, 3, 4])]
# [4, 16]
```

La differenza rispetto alla notazione matematica è che la parte "applica f(x)" viene all'inizio rendendo un po' difficile la lettura.
Python ha anche ```map``` e ```reduce```. Adottando la notazione funzionale classica la composizione si deve scrivere al contrario rispetto al flusso di esecuzione, come per l'appunto ```f(g(x))``` dove prima si calcola ```g(x)``` e poi ```f```.

```
from functools import reduce
reduce(lambda x, sum: sum + x, map(lambda x: x * x, filter(lambda x: x % 2 == 0, [1, 2, 3, 4])))
# 20
```

E' meno immediato da leggere rispetto a

```
[1, 2, 3, 4].select {|n| n % 2 == 0}.map {|n| n * n}.reduce {|sum, n| sum + n}
# 20
```

Elixir (inizio a scriverne) evita il problema con lo zucchero sintattico dell'operatore ```|>```, che per altro non è comodo da scrivere.

```
$ iex
[1, 2, 3, 4] \
|> Enum.filter(fn(n) -> rem(n, 2) == 0 end) \
|> Enum.map(fn(n) -> n * n end) \
|> Enum.reduce(0, fn(n, sum) -> sum + n end)
# 20
```

Dovendo scrivere ogni volta sia il nome del modulo ```Enum``` che ```end```, è il linguaggio più verboso dei tre.


* Elixir http://elixir-lang.org/

* Reia http://reia-lang.org/
