# Wie du es vermeidest beim Scrapen geblockt zu werden

## Welche Dienste dir mit wenigen Veränderungen im Code helfen

Die letzten Stunden hatte ich einen Scraper geschrieben, um die Produkte eines großen Internetmarktplatz für eine Nischenanalyse zu ziehen. Der Scraper erforderte einigen Aufwand,
weil die Website JavaScript für das Rendering verwendet, d.h. viel Code erst im Browser ausgeführt wird. Das Setzen einiger spezieller Header und die Verwendung von Selenium mit einem Headless Chromebrowser brachte schliesslich
den Erfolg. Nun kam die Stunde der Wahrheit, ich wollte nun zum ersten Mal Daten im großen Stile ziehen. Ich startete meinen Scraper, der die Seiten in meine MongoDB-Datenbank schaufeln sollte.
Hat man erst einmal die Rohdaten so ist die Extraktion von Feldern die halbe Miete.
Durch die Parallelverarbeitung sollte das Ganze recht schnell sein. Während ich gespannt wartete passierte es: Die Größe der Files war auf einmal immer gleich und viel kleiner als am Anfang.
Die Http-Status-Codes waren auf einmal nicht mehr 200, sondern 429. Mir war sofort klar, was passiert war: Ich war geblockt worden. Selbstverständlich war ich mir dieser Gefahr von Anfang an bewusst,
jedoch ist es eine Lektion, die ich wohl nie in meinem Leben lernen werde: Ich versuche es zunächst einmal ohne Maßnahmen in der Hoffung, dass es schon gut gehen wird. Zwar sind Blocks häufig nur temporär, aber sie
verhindern, dass du die Menge an Daten ziehen kannst, die du möchtest. Gerade bei Parallelverarbeitung ist die Gefahr sehr groß. Du solltest dir auch bewusst sein, dass du bei wiederholten Verstössen lebenslang geblockt werden kannst.

Du fragst dich nun sicher was du tun kannst um diesem Problem aus dem Weg zu gehen. Es gibt nur eine Möglichkeit: Beim Scrapen musst von möglichst viele verschiedene IP-Adressen aus unterschiedlichen Subnetzen am Besten weltweit verteilt verwenden. Eine erste Idee ist sicher frei zugängliche Proxies aus dem Internet zu verwenden. Leider muss ich dich enttäuschen, weil das dich nicht ans Ziel führen wird. Viele dieser Proxies sind nach einiger Zeit nicht mehr verfügbar oder sie sind extrem langsam. Ausserdem bist du nicht der einzige Scraper auf der Welt. Meist bekommst du deine Request nicht mit diesen Proxies durch.

Als ich das Erstemal vor dem Problem stand hatte ich die Idee über Cloudservices z.B. bei AWS, Azure oder GCloud eine eigene Proxyfarm aufzusetzen. Beim Durchrechnen wurde mir schnell klar, dass das nur mit sehr hohen Kosten zu bewerktstelligen ist. Nehmen wir an du möchtest in 20 Threads parallel Daten von einer Websites ziehen und meine E