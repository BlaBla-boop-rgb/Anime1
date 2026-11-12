# Anime Cosmetics 1.1.0: Riss-Auge und Wirbelhimmel

Dieses Projekt ist der Quellcode von Anime Cosmetics 1.0.0 (aus der .jar dekompiliert) mit zwei neuen Features.
Beide stammen aus der Fähigkeit „Autorität des Todes“ der Ronova-Mod 1.12.3.

## Neue Features

| Feature | Taste (Optionen › Steuerung › Anime Cosmetics) | Standard |
|---|---|---|
| **Riss-Auge** | „Riss-Auge umschalten“ | AUS |
| **Wirbelhimmel** | „Wirbelhimmel umschalten“ | AN (wirkt nur, solange der Riss offen ist) |

**Riss-Auge** ist der Riss aus Ronova: das große Auge mit Lidern, Iris und Feuer-Kern, die Feuersimulation, die vier kleinen Augen im Riss, Ranken und Lichtströme, die Aura und das Aufreißen mit Hand-Leuchten. Er öffnet sich wie in Ronova 6 Blöcke hinter dir und 2,4 Blöcke über dir. Er bleibt offen, solange du im Umkreis von 5 Blöcken bleibst. Läufst du weg oder schaltest ihn aus, schließt er sich. Sobald du kurz stillstehst, öffnet er sich von selbst wieder hinter dir.

**Wirbelhimmel** ist der Himmel vom Screenshot: purpurne Wolken, die sich über dem Riss zu einem dunklen Auge zusammenziehen (`RiftVortex`). Dazu kommt die rote, abgedunkelte Atmosphäre (`RiftAtmosphere`). Die Vanilla-Wolken werden unter dem Wirbel ausgeblendet.

Auch das hier sieht nur der Spieler selbst. Die Mod bleibt rein clientseitig.

## Geänderte und neue Dateien

Neu:
- `fx/RiftFx.java`: der Riss (Port von Ronovas `DeathAuthorityFx`, ohne Portale, Zersplittern und Kino-Kamera) und die Steuerung (öffnen, halten, schließen)
- `fx/RiftVortex.java`, `fx/RiftAtmosphere.java`: Wolkenwirbel und rote Atmosphäre (Port)
- `fx/FireSim.java`, `fx/DeathRiftEyes.java`: Feuersimulation und die Daten der kleinen Augen (1:1 aus Ronova)
- `fx/RiftTypes.java`: Render-Typen für den Riss. Sie sind getrennt von `FxTypes`, damit alle alten Features genau so aussehen wie vorher.
- `mixin/CloudRenderMixin.java`, `mixin/CloudFadeMixin.java`: blenden Wolken unter dem Wirbel aus (`require = 0`, also kein Absturz, falls eine andere Mod die Wolken ersetzt)
- `assets/animecosmetics/textures/entity/fx/rift_vortex*.png` (+ `.mcmeta`)
- `assets/animecosmetics/sounds/authority_open.ogg`, `authority_close.ogg` und `sounds.json`

Geändert:
- `Toggles.java`: neue Features `RIFT` und `RIFT_SKY`
- `AnimeCosmeticsClient.java`: zwei neue Tasten, `RiftFx.register()`, `RiftFx.tick()`, `RiftFx.clear()` beim Trennen
- `mixin/RenderPipelinesAccessor.java`: zusätzlicher Accessor `GLOBALS_SNIPPET` (für den Wirbel unter Iris-Shadern)
- `animecosmetics.mixins.json`: die beiden Wolken-Mixins
- `lang/de_de.json`, `lang/en_us.json`: Tasten, Feature-Namen und Untertitel
- `fabric.mod.json`: Version 1.1.0 und Beschreibung

## Unterschiede zu Ronova

- Ronovas eigene Partikel (`ronova:spark`, `ronova:ember`) und die Federstöße sind durch rote Vanilla-Staubpartikel ersetzt. Eigene Partikeltypen müssten registriert werden, und das geht in einer reinen Client-Mod nicht sauber, wenn man auf fremden Servern spielt.
- Der Riss liegt waagerecht hinter dir. Neigt man in Ronova beim Wirken den Kopf, kippt der Riss mit, hier nicht.

## Bauen

- `gradle.properties`: **`fabric_api_version` eintragen** (deine Fabric-API-Version für 26.3). Die übrigen Werte entsprechen dem Build der Ronova-Mod (Loom 1.18.4, Loader 0.19.5, MC 26.3).
- `.github/workflows/build.yml` baut mit Java 25 und Gradle 9.7.1 und lädt die .jar als Artifact hoch.
- Hast du das Original-Projekt noch, nimm am besten dein eigenes `build.gradle` und kopiere nur die Dateien aus der Liste oben hinein.

Hinweis: Der übrige Code ist dekompiliert. Er ist funktional gleich, aber Kommentare und ursprüngliche Variablennamen fehlen zum Teil (z. B. `var26`).
