# oklab-cottbus
Webseite des OKlab Cottbus.

Die Seite ist unter https://okcb.de erreichbar.

#### Github pages

Das Anlegen der github-Seite ist hier dokumentiert: https://pages.github.com/.

Man kann auch wie in unserem Falle eine [eigene Domain verwenden](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site). 

#### Lokales Testen der Seite mit Jekyll

[Hier](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll#updating-the-github-pages-gem) ist dokumentiert, wie man die Seite lokal mit [Jekyll](https://jekyllrb.com/) testet. Nach der Installation von Jekyll kann die Seite mit `jekyll serve .` generiert und lokal getetstet werden. Eventuell müssen noch weitere Pakete installiert werden (siehe `bundle`-Befehl weiter unten). Mit der Option `--future`  werden auch zukünftige Posts angezeigt. Mit der Option `-H 0.0.0.0`  kann die Seite auch von anderen Computern aus (oder z.B. vom Handy) getestet werden.

Jekyll generiert die (statische) Seite im Ordner `_site`. Dieser Ordner muss nicht auf github gepusht werden, da github die Seite intern selber nochmal genieriert.

#### CSS mit SASS

Die CSS-Dateien werden mit SASS generiert. Zentrales Layout-Dokument ist  `_sass/_layout.scss`. Jekyll generiert die CSS Dateien automatisch beim Servieren der Seite.

#### SVG Logo

Das Logo ist eine SVG-Datei, dessen Quellcode in die Seite eingebettet ist. Die SVG-Datei wurde in [Inkscape ](https://inkscape.org) gestaltet. Mit dem Tool [svgo](https://github.com/svg/svgo) kann die SVG-Datei optimiert und komprimiert werden. Der Befehl dazu lautet: `svgo -i logo.svg -o logo.min.svg`.

Svgo kann auf dem Mac mit `brew install svgo` installiet werden.

#### bundle

Um alle Pakete zu installieren, die Jekyll benötigt, wird `bundle` verwendet. Mit dem Befehl `bundle init`  wurde ein `Gemfile` erzeugt, in das alle benötigten Pakete (in Ruby heißen diese "Gems") eingetragen wurden. Diese können anschließend mit `bundle install` installiert werden. 

`bundle lock`  schreibt die Datei `Gemfile.lock`, die alle aktuell verwendeten Paketversionen speichert. Auf einem neuen Rechner können diese Pakete mit dem Befehl `bundle install` installiert werden.
