# angular-cli

Seit Brad Green auf der ng-conf verkündete angular2 sei kein Framework mehr sonder eine Plattform, stellt sich die Frage was die anderen Teile neben dem Framework angular2 (@angular) sind.

Der interesanteste Teil (für mich jedenfalls) ist die cli (command line interface) angular-cli.

Ich werde hier erklären was die cli genau ist, was sie kann und warum man sie verwenden sollte.
Aufgelockert wird das ganze dann mit einem Beispiel zum nachcoden.  
Btw.: Wenn ich im weiteren verlauf von cli spreche, meine ich angular-cli.

- Was ist angular-cli?
- Warum sollte man es verwenden?
- Wie verwendet man die anglar-cli?

# Was ist angular-cli?
Zu allererst ist die cli ein Tool um angular2 Apps aufzusetzten. Dabei wird das komplette Projekt generiert, mit der Entwicklungsumgebung und dem eigentlichen Projekt, dem was man als code versteht.
Die Entwicklungsumgebung umfasst:
1. Einen Server mit autoreload wenn der code verändert wurde
2. Einem Modulhandler (webpack) mit dem ale gängigen Modul arten geladen werden können
3. Webpack übernimt auch das bündeln der Sourcen
4. Test umgebungen für Unit-Test und e2e-Test
5. Code Generatoren für alle Angular 2 Elemente (Component, directive, pipe, service..)
6. Dependency managment für die generierten Elemente
7. Build system um den Auslieferungscode zu generieren.
8. Alle diese funktionen werden von der cli entgegengenommen und an die eigentliche logig weitergereicht (Blackboxed) was den umgang mit der cli recht komfortabel macht.
 
# Warum sollte man es verwenden?
Dabei wird der Styleguide befolgt, wodurch man einen sehr aufgeräumten Code bekommt.
Im Idealfall findet sich so jeder recht schnell in dem Projekt zurecht.  
Im Urlaub fand ich ein Perfektes Beispiel anhand zweier Grundstücke die nebeneinenader liegen.

![Styleguide](https://pbs.twimg.com/media/Cp_9AwhXYAAqBJz.jpg:large)

Ihr werdet warscheinlich schnell erkenen, welcher von beiden den Styleguide ignoriert.

# Wie verwendet man die anglar-cli?
## installation
Zuerst muss die cli installiert werden, 
dafür benötigen wir wie so oft nodejs, da die cli per npm installiert wird.

`npm install -g angular-cli@webpack`

Das Angular-cli team kam dem Wunsch der Comunity nach 
und wechselte von broccoli/SytemJS als Modulbundler zu Webpack. 
Das war meiner Meinung nach eine sehr gute Entscheidung, 
denn dadurch wurde die Nutzung sehr einfach.
Zum Zeitpunkt als ich diesen Artikel schrieb war `1.0.0-beta.11-webpack.2` aktuell.

## Aufruf
Wenn ihr die cli installiert habt könnt ihr in die cli in der CMD über ng erreichen.

z.B.: `ng v` um die Versionen der cli und von Nodejs abzufragen. 
Bei mir gibt es dann die follgende Ausgabe:
 

`angular-cli: 1.0.0-beta.11-webpack.2`  
`node: 4.5.0`  
`os: win32 ia32`  

 

## Unser erstes Projekt
Somit sind wir bereit ein Projekt aufzusetzten.
Beim aufsetzen können diverse optionale Parameter mitgegeben werden. 
Der wichtigste ist das Setzen des css Preprocessors, 
hier können wir zur Zeit unter den vier populärsten wählen.  

`--style=scss`  
`--style=sass`  
`--style=less`  
`--style=styl`  

Dann werden die Styles bei jeder Ändrung kompiliert.


Um ein Projekt zu starten gibt es zwei Wege, 
die je nach Netzwerkverbindung und Geschwindigkeit des Rechners auchmal etwas länger dauern können (maximal 10 Minuten),
aber zur Not einfach mal einen Kaffe holen.

Der erste Weg ist 
#### ng new [projektname] [options]
Wir starten in unserem workspace

`cd workspace`

Hier rufen wir
`ng new hello-cli --style=scss` auf

Es existiert ein bekannter Bug (issue#1528): 
wenn man ein Projekt mit ng new erstellt wird im Projekt eine Falsche version von angular-cli verwendet.
Bis es eine Lösung für dieses Problem gibt noch follgendes tun

`cd hello-cli`  
`npm i --save-dev angular-cli@webpack`

Der zweite Weg ist 
#### ng init [options]
Wir starten wieder in unserem workspace  
`cd workspace`  
wo wir einen Ordner erstellen (oder einen vorhandenen nutzen)  
`mkdir hello-cli`  
hier rufen wir `ng init --style=scss` auf.  


Im Ordner hello-cli wurden nun dieverse Ordner und Dateien erstellt.
Hier sind die beiden wichtgsten:
#### public
Hier werden eigene Libs oder 3rd Party libs abgelegt die dann von cli mit kopiert werden und global verfügbar sind.
#### src
Das ist unser entwicklungs Ordner. Hier spielt die Musik.

## Lass mal was Sehen!
Um nun unsere App im Browser anzuzeigen müssen wir in der CMD `ng serve` ausführen.  
Anschließend können wir im Browser unter `http://localhost:4200/` die App sehen.  

![awsome](https://media.giphy.com/media/Qh6NZWsFx1G1O/giphy.gif)

## Code generieren
Wir wollen mal eine eigene componente generieren. Eine Navigation bietet sich immer an.  
Den Code Generator werfen wir über `ng g [type] [name]` an.  
Wenn wir also eine Componente Navbar anlegen wollen, führen wir `ng g component navbar` in der CMD aus.  
Die ausgabe sieht dann so aus:  
`installing component`    
  `create src\app\navbar\navbar.component.scss`     
  `create src\app\navbar\navbar.component.html`     
  `create src\app\navbar\navbar.component.spec.ts`     
  `create src\app\navbar\navbar.component.ts`     
  `create src\app\navbar\index.ts`     
  `create src\app\navbar\shared\index.ts`  

Die Component wird nicht nur angelegt, sie wird auch in der app.module.ts importiert und registriert.

<pre>
import { BrowserModule } from '@angular/platform-browser';
import { NgModule, ApplicationRef } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FormsModule } from '@angular/forms';
import { AppComponent } from './app.component';
import { NavbarComponent } from './navbar/navbar.component'; //<= hier der Import

@NgModule({
  declarations: [
    AppComponent,
    NavbarComponent //<= hier die Registrierung
  ],
  imports: [
    BrowserModule,
    CommonModule,
    FormsModule
  ],
  providers: [],
  entryComponents: [AppComponent],
  bootstrap: [AppComponent]
})
export class AppModule {

}

</pre>

Wenn wir nun in der app.component.html &lt;app-navbar&gt;&lt;/app-navbar&gt; schreiben, läd der Browser neu und wir sehen die Ausgabe der Component.

<pre>
<app-navbar></app-navbar>
<h1>
  {{title}}
</h1>

</pre>

## Was können wir noch generieren?
Es gibt Generatoren für 
- component
- directive
- pipe
- service

Der Name des generierten Elements kann auch einen existierenden Pfad enthalten z.B.: `ng g service shared/my-service`
Das generiert dann:   

`create src\app\shared\my-service.service.spec.ts`  
`create src\app\shared\my-service.service.ts`  

## Einbinde globaler Sourcen über den Public Ordner
Wir gehen mal davon aus, das wir vom Project designer eine CSS datei bekommen haben.  
Diese Soll ja für alle Elemente der App erreichbar sein, global eben.

Die datei (gobal.css) sieht so aus:
<pre>
h1{
  color: deeppink;
}
</pre>
#### btw: Ich möchte hier keinen Designer verärgern, Ihr leistet tolle Arbeit!

Diese Datei legt ihr im Ordner public ab. Der Ordner wird nicht überwacht von der Changedetection, ein Reload wird nur aus den src Ordner initiiert.  
In der Index.html muss diese Datei noch eingebunden werden.  
`<link rel="stylesheet" href="global.css">`  
Wenn ihr jetzt speichert, wird die App neugeladen und alle h1 tags sind in deeppink.

## Test
Wie ihr gesehen habt, werden bei jedem Element eine `*.spec.ts` mit generiert. Das ist, wie viele wissen, der Unittest.  
Dieser wird mit `ng test` ausgeführt und verwendent Karma und Jasmine.
Für den e2e Test (die Specs findet ihr im Ordner e2e) müsst ihr eine zweite CMD aufmachen.
In der ersten muss `ng serve` ausgeführt werden, im zweiten führt ihr `ng e2e` aus, dabei werden die Test mit Protractor ausgeführt.

## Build
Am ende des Tages soll die App zum Kunden oder auf euren Server deployed werden, dafür müssen wir den Build Schritt ausführen.
Wärend der Testphase macht es Sinn, die Sourcen leserlich zu haben dafür gibt es `ng build` wenn ihr dann Prodktiv deployen wollt ist `ng build --prod` euer Freund. Damit werden die Sourcen minized und mit einem hash versehen. 


## Gibt es eine Doku?
Ihr könnt euch mit `ng h` die buildin Doku ansehen und unter cli.angular.io findet ihr ebenfalls eine Doku sowie den Link zum github repo.

## Ist das Alles?
In naher Zukunft (evtl. schon jetzt wo du das hier liest) sollen 3rd Party Libs auch über npm installierbar sein, dann genügt ein `npm i --save [libname]` und die cli kümmert sich um den Rest.
Auch ein import von styles und scripts über die angular-cli.json soll in den nächsten versionen funktinieren. Dann kann man die global.scss im src Ordner ablegen, diese Datei in der angular-cli.json registrieren und sie wird dann von webpack mit eingebunden. Das selbe gilt für .js Dateien.

<pre>
...
 "apps": [
    {
      "main": "src/main.ts",
      "tsconfig": "src/tsconfig.json",
      "mobile": false,
      "styles":["global.scss"],
      "scripts": ["global.js"]
    }
  ],
...
</pre>

Zur Zeit des Artikels ist die generstion von Routen deaktiviert, das soll natürlich nach dem gesehenen Schema funktionieren `ng g route [route]`  
Auch macht man sich gerade Gedanken wie man die cli für Plugins öffnen kann. 


## Kann man das so schon einsetzen?
Ja, auch wenn es noch im Beta Stadium ist, habe ich schon Projekte damit umgesetzt.  
Für mich bekommt das Tool das Prädikat:  
## besonders wertvoll
