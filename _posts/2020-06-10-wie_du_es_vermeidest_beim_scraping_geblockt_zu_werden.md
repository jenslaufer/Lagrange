# Wie du es vermeidest beim Scrapen geblockt zu werden

## Welche Dienste dir mit wenigen Veränderungen im Code helfen

Die letzten Stunden hatte ich einen Scraper geschrieben, um die Produkte eines großen Internetmarktplatz zu ziehen. Ich hatte einigen Aufwand in das Projekt reingesteckt,
weil die Website JavaScript in der UI verwendet, d.h. viel Code erst im Browser ausgeführt wird. Selenium mit einem Headless Chromebrowser brachte schliesslich
den Erfolg. Nun kam die Stunde der Wahrheit, ich wollte nun zum ersten Mal Daten im großen Stile ziehen. Ich starte meinen Scraper, der die Seiten in meine MongoDB-Datenbank schaufeln sollte.
Durch die Parallelverarbeitung sollte das Ganze recht schnell sein. Während ich gespannt wartete passierte es: Die Größe der Files war auf einmal immer gleich und viel kleiner als am Anfang.
Die Http-Status-Codes waren auf einmal nicht mehr 200, sondern 429. Mir war sofort klar, was passiert war: Ich war geblockt worden. Selbstverständlich war ich mir dieser Gefahr von Anfang an bewusst,
jedoch ist es immer das Gleiche: Ich versuche es zunächst einmal ohne Maßnahmen in der Hoffung, dass es schon gut gehen wird. Meist ist der Block nur temporär und nicht weiter schlimm, aber es besteht immer Gefahr 
sich einen permanenten Block einzuhandeln.

Egal ob du Daten analysierst, Machine Learning Modelle trainieren oder ein neuronales Netz antrainieren möchtest du benötigst Daten. Sicherlich gibt
es viele Quellen für Daten auf Website wie Kaggle oder , aber häufig passen diese Daten nicht, es fehlt etwas oder du benötigst mehr Daten, um ein qualitativ  hochwertiges
Modell zu trainieren. Die einzige Möglichkeit, die dir häufig bleibt ist diese Daten von Websites zu scrapen. Wenn die Policy der entsprechenden Websites, dir das erlaubt steht
dem Ganzen nihcts mehr im Wege. Häufig gelingt es dir mit wenigen Zeilen Code die Website abzuscrapen, wenn sie statisch sind. Wird Javascriptcode im Browser 